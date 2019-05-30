---
title: Restringir o tráfego de saída no serviço de Kubernetes do Azure (AKS)
description: Saiba quais portas e endereços são necessários para controlar o tráfego de saída no serviço de Kubernetes do Azure (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/14/2019
ms.author: iainfou
ms.openlocfilehash: 4c2dad687d31597954b023dde9d1b9d69788fe04
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241400"
---
# <a name="preview---limit-egress-traffic-for-cluster-nodes-and-control-access-to-required-ports-and-services-in-azure-kubernetes-service-aks"></a>Visualização – tráfego de saída de limite para nós de cluster e controlar o acesso a portas e os serviços no serviço de Kubernetes do Azure (AKS)

Por padrão, os clusters AKS têm irrestrito acesso à internet de saída. Esse nível de acesso à rede permite que nós e serviços que você executa para acessar recursos externos, conforme necessário. Se você quiser restringir o tráfego de saída, um número limitado de portas e endereços deve ser acessível para manter as tarefas de manutenção de cluster íntegro. O cluster, em seguida, está configurado para usar somente as imagens de contêiner base do sistema de registro de contêiner da Microsoft (MCR) ou Azure contêiner ACR (registro), não externos repositórios públicos.

Este artigo fornece detalhes sobre quais portas de rede e os nomes de domínio totalmente qualificados (FQDNs) são obrigatórios e opcionais se você restringir o tráfego de saída em um cluster AKS.  Esse recurso está atualmente na visualização.

> [!IMPORTANT]
> Recursos de visualização do AKS são Self-service, inscreva-se no. Eles são fornecidos para reunir opiniões e bugs de nossa comunidade. Na visualização, esses recursos não são destinados ao uso em produção. Recursos em visualização pública se encaixam em suporte "melhor esforço". Assistência de AKS equipes de suporte técnico está disponível durante o horário comercial do Pacífico (PST) apenas timezone. Para obter mais informações, consulte as seguintes artigos de suporte:
>
> * [Políticas de suporte do AKS][aks-support-policies]
> * [Perguntas frequentes sobre o suporte do Azure][aks-faq]

## <a name="before-you-begin"></a>Antes de começar

Você precisa da CLI do Azure versão 2.0.61 ou posterior instalado e configurado. Execute `az --version` para encontrar a versão. Se precisar instalar ou atualizar, consulte [Instalar a CLI do Azure][install-azure-cli].

Para criar um cluster do AKS que pode limitar o tráfego de saída, primeiro habilite um sinalizador de recursos em sua assinatura. Esse registro de recurso configura qualquer cluster do AKS criar para usar imagens de contêiner do sistema de base da MCR ou ACR. Para registrar o *AKSLockingDownEgressPreview* sinalizador de recursos, use o [registro de recurso az] [ az-feature-register] comando conforme mostrado no exemplo a seguir:

```azurecli-interactive
az feature register --name AKSLockingDownEgressPreview --namespace Microsoft.ContainerService
```

Demora alguns minutos para o status exibir *Registrado*. Você pode verificar o status de registro usando o [lista de recursos az] [ az-feature-list] comando:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSLockingDownEgressPreview')].{Name:name,State:properties.state}"
```

Quando estiver pronto, atualize o registro do *containerservice* provedor de recursos usando o [registro de provedor az] [ az-provider-register] comando:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="egress-traffic-overview"></a>Visão geral do tráfego de saída

Para fins operacionais e de gerenciamento, nós em um cluster AKS é necessário acessar determinadas portas e os nomes de domínio totalmente qualificados (FQDNs). Essas ações podem ser para se comunicar com o servidor de API, ou baixar e instalar os componentes principais do cluster Kubernetes e atualizações de segurança de nó. Por padrão, o tráfego de internet (saída) de saída não é restrito para nós em um cluster AKS. O cluster pode extrair sistema base imagens de contêiner de repositórios externos.

Para aumentar a segurança de cluster do AKS, talvez você queira restringir o tráfego de saída. O cluster está configurado para efetuar pull de imagens de contêiner do ACR ou MCR base do sistema. Se você bloquear o tráfego de saída dessa maneira, você deve definir as portas específicas e FQDNs para permitir que os nós do AKS para se comunicar corretamente com serviços externos necessários. Sem essas portas autorizadas e FQDNs, seus nós AKS não podem se comunicar com o servidor de API ou instalar os componentes principais.

Você pode usar [Firewall do Azure] [ azure-firewall] ou um dispositivo de firewall de terceiros 3º para proteger o tráfego de saída e defini-los necessárias portas e endereços.

No AKS, há dois conjuntos de portas e endereços:

* O [necessárias portas e endereços para clusters AKS](#required-ports-and-addresses-for-aks-clusters) fornece detalhes sobre os requisitos mínimos para o tráfego de saída autorizados.
* O [opcional recomendado endereços e portas para clusters AKS](#optional-recommended-addresses-and-ports-for-aks-clusters) não são necessárias para todos os cenários, mas a integração com outros serviços, como o Azure Monitor não funcionará corretamente. Examine esta lista de portas opcionais e FQDNs e autorizar qualquer um dos serviços e componentes usados em seu cluster do AKS.

> [!NOTE]
> Limitar o tráfego de saída só funciona em novos clusters AKS criados depois de habilitar o registro do sinalizador de recurso. Para clusters existentes, [executar uma operação de atualização de cluster] [ aks-upgrade] usando o `az aks upgrade` antes de você limitar o tráfego de saída de comando.

## <a name="required-ports-and-addresses-for-aks-clusters"></a>Portas e os endereços para clusters AKS

As seguintes portas de saída / regras de rede são necessárias para um cluster do AKS:

* A porta TCP *443*
* A porta TCP *9000* para o pod frontal de túnel para se comunicar com o fim do túnel no servidor de API.

O seguinte FQDN / regras de aplicativo são necessárias:

| FQDN                      | Port      | Uso      |
|---------------------------|-----------|----------|
| *.azmk8s.io               | HTTPS:443 | Esse endereço é o ponto de extremidade do servidor de API. |
| aksrepos.azurecr.io       | HTTPS:443 | Esse endereço é necessário para acesso de imagens no registro de contêiner do Azure (ACR). |
| *.blob.core.windows.net   | HTTPS:443 | Esse endereço é o armazenamento de back-end para imagens armazenadas no ACR. |
| mcr.microsoft.com         | HTTPS:443 | Esse endereço é necessário para acesso de imagens no registro de contêiner da Microsoft (MCR). |
| management.azure.com      | HTTPS:443 | Esse endereço é necessário para operações do Kubernetes GET/PUT. |
| login.microsoftonline.com | HTTPS:443 | Esse endereço é necessário para autenticação do Active Directory do Azure. |

## <a name="optional-recommended-addresses-and-ports-for-aks-clusters"></a>Opcional recomendado endereços e portas para clusters AKS

As seguintes portas de saída / regras de rede não são necessárias para clusters AKS funcionar corretamente, mas são recomendadas:

* A porta UDP *123* para sincronização de horário NTP
* A porta UDP *53* para DNS

O seguinte FQDN / regras de aplicativo são recomendadas para clusters AKS funcionar corretamente:

| FQDN                                    | Port      | Uso      |
|-----------------------------------------|-----------|----------|
| *.ubuntu.com                            | HTTP:80   | Esse endereço permite que os nós de cluster do Linux baixem os patches de segurança necessários e atualizações. |
| packages.microsoft.com                  | HTTPS:443 | Esse endereço é o repositório de pacotes da Microsoft usado para armazenada em cache *apt-get* operações. |
| dc.services.visualstudio.com            | HTTPS:443 | Recomendado para métricas corretas e o monitoramento usando o Azure Monitor. |
| *.opinsights.azure.com                  | HTTPS:443 | Recomendado para métricas corretas e o monitoramento usando o Azure Monitor. |
| *.monitoring.azure.com                  | HTTPS:443 | Recomendado para métricas corretas e o monitoramento usando o Azure Monitor. |
| gov-prod-policy-data.trafficmanager.net | HTTPS:443 | Esse endereço é usado para operação correta da política do Azure (atualmente em visualização no AKS). |
| apt.dockerproject.org                   | HTTPS:443 | Esse endereço é usado para instalação de driver correto e a operação em nós com base em GPU. |
| nvidia.github.io                        | HTTPS:443 | Esse endereço é usado para instalação de driver correto e a operação em nós com base em GPU. |

## <a name="next-steps"></a>Próximas etapas

Neste artigo, você aprendeu quais portas e endereços para permitir que se você restringir o tráfego de saída para o cluster. Você também pode definir como os pods em si podem se comunicar e que restrições eles têm dentro do cluster. Para obter mais informações, consulte [proteger o tráfego entre os pods usando políticas de rede no AKS][network-policy].

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[network-policy]: use-network-policies.md
[azure-firewall]: ../firewall/overview.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[aks-upgrade]: upgrade-cluster.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
