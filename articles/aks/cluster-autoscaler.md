---
title: Usar o dimensionador automático de cluster no AKS (Serviço de Kubernetes do Azure)
description: Saiba como usar o dimensionador automático de cluster para dimensionar automaticamente seu cluster e atender às demandas de aplicativo em um cluster do AKS (Serviço de Kubernetes do Azure).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: iainfou
ms.openlocfilehash: c4fe05c96b1006a7d110caa019619ce8be396fe8
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491565"
---
# <a name="preview---automatically-scale-a-cluster-to-meet-application-demands-on-azure-kubernetes-service-aks"></a>Preview - dimensionar automaticamente um cluster para atender às demandas do aplicativo no serviço de Kubernetes do Azure (AKS)

Para se manter atualizado com as demandas de aplicativo no AKS (Serviço de Kubernetes do Azure), talvez você precise ajustar o número de nós que executam as cargas de trabalho. O componente de dimensionador automático de cluster pode inspecionar os pods no cluster que não podem ser agendados devido a restrições de recursos. Quando problemas são detectados, o número de nós é aumentado para atender à demanda de aplicativo. Os nós também são regularmente verificados quanto à falta de pods em execução, com o número de nós diminuído posteriormente conforme necessário. Essa capacidade de escalar ou reduzir verticalmente o número de nós no cluster do AKS permite a execução de um cluster eficiente e econômico.

Este artigo mostra como habilitar e gerenciar o dimensionador automático de cluster em um cluster do AKS. Dimensionador automático de cluster deve ser testado somente no modo de visualização em clusters AKS com um pool de nó único.

> [!IMPORTANT]
> Recursos de visualização do AKS são Self-service, inscreva-se no. Eles são fornecidos para reunir opiniões e bugs de nossa comunidade. Na visualização, esses recursos não são destinados ao uso em produção. Recursos em visualização pública se encaixam em suporte "melhor esforço". Assistência de AKS equipes de suporte técnico está disponível durante o horário comercial do Pacífico (PST) apenas timezone. Para obter mais informações, consulte as seguintes artigos de suporte:
>
> * [Políticas de suporte do AKS][aks-support-policies]
> * [Perguntas frequentes sobre o suporte do Azure.][aks-faq]

## <a name="before-you-begin"></a>Antes de começar

Este artigo exigirá que você está executando a CLI do Azure versão 2.0.65 ou posterior. Execute `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, consulte [Instalar a CLI do Azure][azure-cli-install].

### <a name="install-aks-preview-cli-extension"></a>Instalar a extensão da CLI aks-preview

Para usar o dimensionador automático de cluster, você precisa de *versão prévia do aks* CLI versão da extensão 0.4.1 ou superior. Instalar o *versão prévia do aks* extensão de CLI do Azure usando o [Adicionar extensão az][az-extension-add] command, then check for any available updates using the [az extension update][az-extension-update] comando::

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-scale-set-feature-provider"></a>Registrar o provedor de recursos do conjunto de dimensionamento

Para criar um AKS que usa os conjuntos de dimensionamentos, você também deve habilitar um sinalizador de recursos em sua assinatura. Para registrar o *VMSSPreview* sinalizador de recursos, use o [registro de recurso az][az-feature-register] comando conforme mostrado no exemplo a seguir:

> [!CAUTION]
> Quando você registra um recurso em uma assinatura, você não pode atualmente cancelar o registro desse recurso. Depois de habilitar alguns recursos de visualização, os padrões podem ser usados para todos os clusters AKS, em seguida, é criados na assinatura. Não habilite os recursos de visualização em assinaturas de produção. Use uma assinatura separada para testar recursos de visualização e Reúna comentários.

```azurecli-interactive
az feature register --name VMSSPreview --namespace Microsoft.ContainerService
```

Demora alguns minutos para o status exibir *Registrado*. Você pode verificar o status de registro usando o [lista de recursos az][az-feature-list] comando:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
```

Quando estiver pronto, atualize o registro do *containerservice* provedor de recursos usando o [registro de provedor az][az-provider-register] comando:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations"></a>Limitações

As seguintes limitações se aplicam quando você criar e gerenciar clusters AKS que usam o dimensionador automático de cluster:

* O complemento de roteamento de aplicativo HTTP não pode ser usado.

## <a name="about-the-cluster-autoscaler"></a>Sobre o dimensionador automático de cluster

Para se ajustar às mudanças nas demandas de aplicativo, como entre a jornada de trabalho e a noite ou em um fim de semana, os clusters geralmente precisam de uma maneira de serem dimensionados automaticamente. Os clusters do AKS podem ser dimensionados de uma das duas maneiras:

* O **dimensionador automático de cluster** inspeciona os pods que não podem ser agendados em nós devido a restrições de recursos. Em seguida, o cluster aumenta automaticamente o número de nós.
* O **dimensionador automático de pod horizontal** usa o Servidor de Métricas em um cluster do Kubernetes para monitorar a demanda de recursos dos pods. Se um serviço precisa de mais recursos, o número de pods é automaticamente aumentado para atender à demanda.

![O dimensionador automático de cluster e o dimensionador automático de pod horizontal geralmente trabalham juntos para dar suporte às demandas necessárias de aplicativo](media/autoscaler/cluster-autoscaler.png)

O dimensionador automático de pod horizontal e o dimensionador automático de cluster também podem, em seguida, diminuir o número de pods e nós conforme necessário. O dimensionador automático de cluster diminui o número de nós quando a capacidade não é utilizada por um período. Os pods em um nó a serem removidos pelo dimensionador automático de cluster são agendados com segurança em outro lugar no cluster. O dimensionador automático de cluster poderá não conseguir fazer a redução vertical se os pods não puderem ser movidos, como nas seguintes situações:

* Um pod criado diretamente e que não tem o suporte de um objeto de controlador, como uma implantação ou um conjunto de réplicas.
* Um PDB (orçamento de interrupção de pod) é muito restritivo e não permite que o número de pods fique abaixo de determinado limite.
* Um pod usa seletores de nó ou uma antiafinidade que não pode ser cumprida se ele está agendado em outro nó.

Para obter mais informações sobre como o dimensionador automático de cluster não será possível reduzir verticalmente, consulte [quais tipos de pods podem impedir que o dimensionador automático cluster removendo um nó?][autoscaler-scaledown]

O dimensionador automático de cluster usa parâmetros de inicialização para itens como intervalos de tempo entre eventos de escala e limites de recursos. Esses parâmetros são definidos pela plataforma Azure e atualmente não são expostos para seu ajuste. Para obter mais informações sobre quais parâmetros o dimensionador automático de cluster usa, consulte [quais são os parâmetros do cluster dimensionador automático?][autoscaler-parameters].

Os dois dimensionadores automáticos podem trabalhar juntos e geralmente são implantados em um cluster. Quando combinados, o dimensionador automático de pod horizontal se concentra na execução do número de pods necessários para atender à demanda de aplicativo. O dimensionador automático de cluster se concentra na execução do número de nós necessários para dar suporte aos pods agendados.

> [!NOTE]
> O dimensionamento manual é desabilitado quando o dimensionador automático de cluster é usado. Deixe o dimensionador automático de cluster determinar o número de nós necessários. Caso deseje dimensionar o cluster manualmente, [desabilite o dimensionador automático de cluster](#disable-the-cluster-autoscaler).

## <a name="create-an-aks-cluster-and-enable-the-cluster-autoscaler"></a>Criar um cluster do AKS e habilitar o dimensionador automático de cluster

Se você precisar criar um cluster do AKS, use o [criar az aks][az-aks-create] comando. Especifique uma *--kubernetes-version* que atenda ao número de versão mínima necessário ou que exceda esse número, conforme descrito na seção anterior [Antes de começar](#before-you-begin). Para habilitar e configurar o dimensionador automático de cluster, use o parâmetro *--enable-cluster-autoscaler* e especifique um nó *--min-count* e *--max-count*.

> [!IMPORTANT]
> O dimensionamento automático do cluster é um componente de Kubernetes. Embora o cluster do AKS use um conjunto de dimensionamento de máquinas virtuais para os nós, não habilite ou edite manualmente as configurações de dimensionamento automático do conjunto de dimensionamento no portal do Azure ou usando a CLI do Azure. Permita que o dimensionador automático do cluster de Kubernetes gerencie as configurações de dimensionamento necessárias. Para obter mais informações, confira [Posso modificar os recursos do AKS no grupo de recursos MC_?](faq.md#can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-mc_-resource-group)

O seguinte exemplo cria um cluster do AKS com o conjunto de dimensionamento de máquinas virtuais habilitado e usa um mínimo de *1* e um máximo de *3* nós:

```azurecli-interactive
# First create a resource group
az group create --name myResourceGroup --location canadaeast

# Now create the AKS cluster and enable the cluster autoscaler
az aks create \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --node-count 1 \
  --enable-vmss \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

São necessários alguns minutos para criar o cluster e definir as configurações do dimensionador automático de cluster.

### <a name="enable-the-cluster-autoscaler-on-an-existing-aks-cluster"></a>Habilitar o dimensionador automático de cluster em um cluster do AKS existente

Habilite o dimensionador automático de cluster em um cluster do AKS existente que atenda aos requisitos, conforme descrito na seção anterior [Antes de começar](#before-you-begin). Use o [atualizar az aks][az-aks-update] de comando e optar por *– enable-cluster-autoscaler*, em seguida, especificar um nó *min – contagem* e *– contagem máxima* . O seguinte exemplo habilita o dimensionador automático de cluster em um cluster existente que usa um mínimo de *1* e um máximo de *3* nós:

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --enable-cluster-autoscaler \
  --min-count 1 \
  --max-count 3
```

Se a contagem mínima de nós for maior que o número de nós existentes no cluster, serão necessários alguns minutos para criar os nós adicionais.

## <a name="change-the-cluster-autoscaler-settings"></a>Alterar as configurações do dimensionador automático de cluster

Na etapa anterior para criar ou atualizar um cluster do AKS existente, a contagem mínima de nós do dimensionador automático de cluster foi definida como *1* e a contagem máxima de nós foi definida como *3*. Conforme as demandas de aplicativo mudam, você pode precisar ajustar a contagem de nós do dimensionador automático de cluster.

Para alterar a contagem de nós, use o [az aks atualizar][az-aks-update] de comando e especifique um valor mínimo e máximo. O seguinte exemplo define a *--min-count* como *1* e a *--max-count* como *5*:

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --update-cluster-autoscaler \
  --min-count 1 \
  --max-count 5
```

> [!NOTE]
> Durante a versão prévia, não é possível definir uma contagem mínima de nós maior que a atualmente definida para o cluster. Por exemplo, se atualmente você tiver uma contagem mínima definida como *1*, não poderá atualizar a contagem mínima para *3*.

Monitore o desempenho de seus aplicativos e serviços e ajuste as contagens de nós do dimensionador automático de cluster para que correspondam ao desempenho necessário.

## <a name="disable-the-cluster-autoscaler"></a>Desabilitar o dimensionador automático de cluster

Se você não deseja mais usar o dimensionador automático de cluster, você pode desabilitá-lo usando o [az aks atualizar][az-aks-update] comando. Os nós não são removidos quando o dimensionador automático de cluster é desabilitado.

Para remover o dimensionador automático de cluster, especifique o parâmetro *--disable-cluster-autoscaler*, conforme mostrado no seguinte exemplo:

```azurecli-interactive
az aks update \
  --resource-group myResourceGroup \
  --name myAKSCluster \
  --disable-cluster-autoscaler
```

Você pode Dimensionar manualmente seu cluster usando o [escala do az aks][az-aks-scale] comando. Se você usar o dimensionador automático de pod horizontal, esse recurso continuará sendo executado com o dimensionador automático de cluster desabilitado, mas os pods poderão acabar não conseguindo ser agendados se os recursos do nó estiverem todos em uso.

## <a name="next-steps"></a>Próximas etapas

Este artigo mostrou como dimensionar automaticamente o número de nós do AKS. Você também pode usar o dimensionador automático de pod horizontal para ajustar automaticamente o número de pods que executam o aplicativo. Para obter etapas sobre como usar o dimensionador automático horizontal de pods, consulte [dimensionar aplicativos no AKS][aks-scale-apps].

<!-- LINKS - internal -->
[aks-upgrade]: upgrade-cluster.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-extension-add]: /cli/azure/extension#az-extension-add
[aks-scale-apps]: tutorial-kubernetes-scale.md
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update

<!-- LINKS - external -->
[az-aks-update]: https://github.com/Azure/azure-cli-extensions/tree/master/src/aks-preview
[autoscaler-scaledown]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-types-of-pods-can-prevent-ca-from-removing-a-node
[autoscaler-parameters]: https://github.com/kubernetes/autoscaler/blob/master/cluster-autoscaler/FAQ.md#what-are-the-parameters-to-ca
