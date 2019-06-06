---
title: Perguntas frequentes sobre o Serviço de Kubernetes do Azure (AKS)
description: Encontre respostas para algumas perguntas comuns sobre o serviço de Kubernetes do Azure (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/03/2019
ms.author: iainfou
ms.openlocfilehash: 1cc03cbcffc5253e8b357b6702cd21c45740ff81
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66514498"
---
# <a name="frequently-asked-questions-about-azure-kubernetes-service-aks"></a>Perguntas frequentes sobre o Serviço de Kubernetes do Azure (AKS)

Este artigo aborda as perguntas frequentes sobre o AKS (Serviço de Kubernetes do Azure).

## <a name="which-azure-regions-currently-provide-aks"></a>Quais regiões do Azure atualmente fornecem AKS?

Para obter uma lista completa das regiões disponíveis, consulte [AKS regiões e disponibilidade][aks-regions].

## <a name="does-aks-support-node-autoscaling"></a>O AKS é compatível com o dimensionamento automático de nó?

Sim, o dimensionamento automático está disponível por meio de [Kubernetes autoscaler] [ auto-scaler] a partir do Kubernetes 1.10. Para obter informações sobre como configurar e usar o dimensionador automático de cluster manualmente, consulte [dimensionamento automático de Cluster no AKS][aks-cluster-autoscale].

Você também pode usar o dimensionador automático interna de cluster (atualmente em visualização no AKS) para gerenciar o dimensionamento de nós. Para obter mais informações, consulte [dimensionar automaticamente um cluster para atender às demandas do aplicativo no AKS][aks-cluster-autoscaler].

## <a name="does-aks-support-kubernetes-rbac"></a>AKS oferece suporte a RBAC Kubernetes?

Sim, Kubernetes controle de acesso baseado em função (RBAC) é habilitada por padrão, quando os clusters são criados com a CLI do Azure. Você pode habilitar o RBAC para clusters que foram criados usando o portal do Azure ou modelos.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Posso implantar AKS em minha rede virtual existente?

Sim, você pode implantar um cluster do AKS em uma rede virtual existente usando o [recurso de rede avançado][aks-advanced-networking].

## <a name="can-i-make-the-kubernetes-api-server-accessible-only-within-my-virtual-network"></a>Posso tornar o servidor de API do Kubernetes acessível somente dentro de minha rede virtual?

Não no momento. O servidor de API do Kubernetes é exposto como um FQDN (nome de domínio totalmente qualificado) público. Você pode controlar o acesso ao seu cluster usando [RBAC Kubernetes e Azure Active Directory (AD do Azure)][aks-rbac-aad].

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>As atualizações de segurança são aplicadas aos nós do agente do AKS?

Azure aplica automaticamente os patches de segurança para os nós do Linux no seu cluster em uma agenda noturna. No entanto, você é responsável por garantir que esses nós são reiniciados como de Linux necessários. Você tem várias opções para nós de reinicialização:

- Manualmente, por meio do portal do Azure ou da CLI do Azure.
- Atualizando o cluster AKS. As atualizações de cluster [nós cordon e drain] [ cordon-drain] automaticamente e, em seguida, colocar um novo nó online com a imagem mais recente do Ubuntu e uma nova versão de patch ou uma versão secundária do Kubernetes. Para obter mais informações, consulte [atualizar um cluster AKS][aks-upgrade].
- Usando [Kured](https://github.com/weaveworks/kured), um daemon de reinicialização do código-fonte aberto para o Kubernetes. Kured é executado como um [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) e monitora cada nó para a presença de um arquivo que indica que uma reinicialização é necessária. Em todo o cluster, as reinicializações do sistema operacional são gerenciadas pelo mesmo [cordon e drain processo] [ cordon-drain] como uma atualização de cluster.

Para obter mais informações sobre como usar o kured, consulte [Aplicar atualizações de segurança e o kernel para nós do AKS][node-updates-kured].

### <a name="windows-server-nodes"></a>Nós do Windows Server

Para nós do Windows Server (atualmente em visualização no AKS), atualização do Windows executado automaticamente e aplicar as atualizações mais recentes. Em um agendamento regular em todo o ciclo de lançamento do Windows Update e seu próprio processo de validação, você deve realizar uma atualização sobre os pools de nó do Windows Server no cluster do AKS. Este processo de atualização cria nós que executam a imagem mais recente do Windows Server e os patches, em seguida, remove os nós mais antigos. Para obter mais informações sobre esse processo, consulte [Upgrade de um pool de nós no AKS][nodepool-upgrade].

## <a name="why-are-two-resource-groups-created-with-aks"></a>Por que são criados dois grupos de recursos com o AKS?

Cada implementação do AKS abrange dois grupos de recursos:

1. Criar o primeiro grupo de recursos. Esse grupo contém apenas o recurso de serviço do Kubernetes. O provedor de recursos AKS cria automaticamente o segundo grupo de recursos durante a implantação. É um exemplo do segundo grupo de recursos *MC_myResourceGroup_myAKSCluster_eastus*. Para obter informações sobre como especificar o nome do segundo grupo de recursos, consulte a próxima seção.
1. O segundo grupo de recursos, tais como *MC_myResourceGroup_myAKSCluster_eastus*, contém todos os recursos de infraestrutura associados ao cluster. Esses recursos incluem as máquinas virtuais do nó do Kubernetes, rede virtual e armazenamento. A finalidade desse grupo de recursos é simplificar a limpeza de recursos.

Se você criar recursos para usar com o cluster do AKS, como contas de armazenamento ou endereços IP públicos reservados, colocá-los no grupo de recursos gerada automaticamente.

## <a name="can-i-provide-my-own-name-for-the-aks-infrastructure-resource-group"></a>Posso fornecer meu próprio nome para o grupo de recursos de infraestrutura do AKS?

Sim. Por padrão, o provedor de recursos AKS cria automaticamente um grupo de recursos secundários (como *MC_myResourceGroup_myAKSCluster_eastus*) durante a implantação. Para estar em conformidade com as políticas corporativas, você pode fornecer seu próprio nome para esse cluster gerenciado (*MC_* ) grupo de recursos.

Para especificar seu próprio nome do grupo de recursos, instale o [versão prévia do aks] [ aks-preview-cli] versão da extensão da CLI do Azure *0.3.2* ou posterior. Quando você cria um cluster AKS usando o [criar az aks] [ az-aks-create] de comando, use o *– grupo de recursos de nó* parâmetro e especifique um nome para o grupo de recursos. Se você [usar um modelo do Azure Resource Manager] [ aks-rm-template] para implantar um cluster do AKS, você pode definir o nome do grupo de recursos usando o *nodeResourceGroup* propriedade.

* O grupo de recursos secundário é criado automaticamente pelo provedor de recursos do Azure em sua própria assinatura.
* Você pode especificar um nome de grupo de recursos personalizados somente quando você estiver criando o cluster.

Conforme você trabalha com o *MC_* grupo de recursos, tenha em mente que você não pode:

* Especifique um grupo de recursos existente para o *MC_* grupo.
* Especifique uma assinatura diferente para o *MC_* grupo de recursos.
* Alterar o *MC_* nome do grupo de recursos depois que o cluster foi criado.
* Especificar nomes para os recursos gerenciados dentro do *MC_* grupo de recursos.
* Modificar ou excluir marcas de recursos gerenciados dentro do *MC_* grupo de recursos. (Consulte informações adicionais na próxima seção).

## <a name="can-i-modify-tags-and-other-properties-of-the-aks-resources-in-the-mc-resource-group"></a>Posso modificar marcas e outras propriedades dos recursos no grupo de recursos MC_ AKS?

Se você modificar ou excluir marcas criada pelo Azure e outras propriedades de recurso nas *MC_* grupo de recursos, você poderá ter resultados inesperados, como o dimensionamento e erros de atualização. AKS permite que você criar e modificar marcas personalizadas. Você pode desejar para criar ou modificar marcas personalizadas, por exemplo, para atribuir um centro de custo ou de unidade de negócios. Modificando os recursos sob o *MC_* no cluster do AKS, é dividir o objetivo de nível de serviço (SLO). Para obter mais informações, consulte [AKS faz oferecem um contrato de nível de serviço?](#does-aks-offer-a-service-level-agreement)

## <a name="what-kubernetes-admission-controllers-does-aks-support-can-admission-controllers-be-added-or-removed"></a>Quais os controles de admissão de Kubernetes que o AKS suporta? Controladores de admissão podem ser adicionados ou removidos?

AKS suporta os seguintes [controles de admissão][admission-controllers]:

- *NamespaceLifecycle*
- *LimitRanger*
- *ServiceAccount*
- *DefaultStorageClass*
- *DefaultTolerationSeconds*
- *MutatingAdmissionWebhook*
- *ValidatingAdmissionWebhook*
- *ResourceQuota*
- *DenyEscalatingExec*
- *AlwaysPullImages*

No momento, você não pode modificar a lista de controladores de admissão no AKS.

## <a name="is-azure-key-vault-integrated-with-aks"></a>O Azure Key Vault é integrado com o AKS?

AKS atualmente nativamente não está integrado com o Azure Key Vault. No entanto, o [FlexVolume de Cofre de chave do Azure para o projeto Kubernetes] [ keyvault-flexvolume] permite direcionar a integração de pods Kubernetes para os segredos do Cofre de chaves.

## <a name="can-i-run-windows-server-containers-on-aks"></a>Posso executar contêineres do Windows Server no AKS?

Sim, os contêineres do Windows Server estão disponíveis na visualização. Para executar contêineres do Windows Server no AKS, você deve criar um pool de nós que executa o Windows Server como o sistema operacional convidado. Contêineres do Windows Server podem usar apenas o Windows Server 2019. Para começar, consulte [criar um cluster do AKS com um pool de nós do Windows Server][aks-windows-cli].

Suporte do servidor de janela para o pool de nós inclui algumas limitações que fazem parte do Windows Server no projeto Kubernetes upstream. Para obter mais informações sobre essas limitações, consulte [contêineres do Windows Server nas limitações de AKS][aks-windows-limitations].

## <a name="does-aks-offer-a-service-level-agreement"></a>O AKS oferece um contrato de nível de serviço?

Em um contrato de nível de serviço (SLA), o provedor concorda em reembolse o cliente para o custo do serviço se o nível de serviço publicado não for atendido. Como AKS é gratuito, sem nenhum custo está disponível para reembolsar, portanto, o AKS não tem nenhum SLA formal. No entanto, o AKS buscas manter a disponibilidade de pelo menos de 99,5% para o servidor de API do Kubernetes.

## <a name="why-cant-i-set-maxpods-below-30"></a>Por que não é possível definir maxPods abaixo de 30?

No AKS, você pode definir o `maxPods` valor quando você cria o cluster usando os modelos da CLI do Azure e o Azure Resource Manager. No entanto, Kubenet e CNI do Azure exigem um *valor mínimo* (validado no momento da criação):

| Rede | Mínimo | Máximo |
| -- | :--: | :--: |
| Azure CNI | 30 | 250 |
| Kubenet | 30 | 110 |

Como AKS é um serviço gerenciado, podemos implantar e gerenciar complementos e pods como parte do cluster. No passado, os usuários poderiam definir um `maxPods` valor menor do que o valor que os pods gerenciados necessários para executar (por exemplo, 30). AKS agora calcula o número mínimo de pods usando esta fórmula: ((maxPods ou (maxPods * vm_count)) > mínimo de pods de complemento gerenciado.

Os usuários não podem substituir o mínimo `maxPods` validação.

## <a name="can-i-apply-azure-reservation-discounts-to-my-aks-agent-nodes"></a>Pode aplicar descontos de reserva do Azure para Meus nós de agente do AKS?

Nós de agente do AKS são cobrados como máquinas virtuais do Azure padrão, portanto, se você tiver comprado [Azure reservas] [ reservation-discounts] para o tamanho VM que você está usando no AKS, esses descontos são aplicados automaticamente.

<!-- LINKS - internal -->

[aks-regions]: ./quotas-skus-regions.md#region-availability
[aks-upgrade]: ./upgrade-cluster.md
[aks-cluster-autoscale]: ./autoscaler.md
[virtual-kubelet]: virtual-kubelet.md
[aks-advanced-networking]: ./configure-azure-cni.md
[aks-rbac-aad]: ./azure-ad-integration.md
[node-updates-kured]: node-updates-kured.md
[aks-preview-cli]: /cli/azure/ext/aks-preview/aks
[az-aks-create]: /cli/azure/aks#az-aks-create
[aks-rm-template]: /rest/api/aks/managedclusters/createorupdate#managedcluster
[aks-cluster-autoscaler]: cluster-autoscaler.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[aks-windows-cli]: windows-container-cli.md
[aks-windows-limitations]: windows-node-limitations.md
[reservation-discounts]: ../billing/billing-save-compute-costs-reservations.md

<!-- LINKS - external -->

[auto-scaler]: https://github.com/kubernetes/autoscaler
[cordon-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent
[admission-controllers]: https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/
[keyvault-flexvolume]: https://github.com/Azure/kubernetes-keyvault-flexvol
