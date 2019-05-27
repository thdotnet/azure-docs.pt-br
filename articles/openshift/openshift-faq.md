---
title: Perguntas frequentes para o Azure Red Hat OpenShift | Microsoft Docs
description: Aqui estão as respostas para perguntas comuns sobre o Microsoft Azure Red Hat OpenShift
services: container-service
author: tylermsft
ms.author: twhitney
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/08/2019
ms.openlocfilehash: 4f11eac106fe984aa06e41083c2d4f66c7d0e62c
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65872907"
---
# <a name="azure-red-hat-openshift-faq"></a>Azure Red Hat OpenShift perguntas Frequentes

Este artigo aborda perguntas frequentes (FAQs) sobre o Microsoft Azure Red Hat OpenShift.

## <a name="how-do-i-get-started"></a>Como começar?

Antes de usar Azure Red Hat OpenShift, você precisará comprar um mínimo de 4 nós de aplicativo do Azure Red Hat OpenShift reservado.

Se você for um cliente do Azure,[comprar instâncias reservadas do Azure Red Hat OpenShift](https://aka.ms/openshift/buy) por meio do portal do Azure. Após a compra, sua assinatura será ativada dentro de 24 horas, após o qual você poderá provisionar clusters.

Se você não for um cliente do Azure, [entre em contato com vendas](https://aka.ms/openshift/contact-sales) e preencha o formulário na parte inferior da página para iniciar o processo de venda.

Consulte a [página de preços do Azure Red Hat OpenShift](https://aka.ms/openshift/pricing) para obter mais informações.

## <a name="which-azure-regions-are-supported"></a>Há suporte para quais regiões do Azure?

Ver [recursos com suporte](supported-resources.md#azure-regions) para obter uma lista de regiões globais, onde há suporte para Azure Red Hat OpenShift.

## <a name="can-i-deploy-a-cluster-into-an-existing-virtual-network"></a>Posso implantar um cluster em uma rede virtual existente?

Não. Mas você pode se conectar a um cluster do Azure Red Hat OpenShift para uma rede virtual existente por meio de emparelhamento. Ver [conectar-se a rede virtual de um cluster para uma rede virtual existente ](tutorial-create-cluster.md#optional-connect-the-clusters-virtual-network-to-an-existing-virtual-network) para obter detalhes.

## <a name="what-cluster-operations-are-available"></a>Quais operações de cluster estão disponíveis?

Você só pode escalar ou reduzir verticalmente o número de nós de computação. Não há outras modificações têm permissão para o `Microsoft.ContainerService/openShiftManagedClusters` recursos após a criação. O número máximo de nós de computação é limitado a 20.

## <a name="what-virtual-machine-sizes-can-i-use"></a>Quais tamanhos de máquina virtual pode usar?

Ver [tamanhos de máquina virtual do Azure Red Hat OpenShift](supported-resources.md#virtual-machine-sizes) para obter uma lista de tamanhos de máquina virtual, você pode usar com um cluster do Azure Red Hat OpenShift.

## <a name="is-data-on-my-cluster-encrypted"></a>São dados em meu cluster criptografado?

Por padrão, há a criptografia em repouso. A plataforma de armazenamento do Azure criptografa automaticamente seus dados antes de persisti-lo e descriptografa os dados antes da recuperação. Ver [criptografia do serviço de armazenamento do Azure para dados em repouso](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) para obter detalhes.

## <a name="can-i-use-prometheusgrafana-to-monitor-containers-and-manage-capacity"></a>Pode usar o Prometheus/Grafana para monitorar os contêineres e gerenciar a capacidade?

Não, não no momento atual.

## <a name="is-the-docker-registry-available-externally-so-i-can-use-tools-such-as-jenkins"></a>O registro do Docker está disponível externamente para que eu possa usar ferramentas como Jenkins?

O registro do Docker está disponível no `https://docker-registry.apps.<clustername>.<region>.azmosa.io/` no entanto, uma garantia de durabilidade do armazenamento de alta segurança não for fornecida. Você também pode usar [registro de contêiner do Azure](https://azure.microsoft.com/services/container-registry/).

## <a name="is-cross-namespace-networking-supported"></a>Há suporte para namespace entre a rede?

Cliente e administradores de projeto individual podem personalizar o namespace entre a rede (incluindo recusá-lo) em uma base por projeto usando `NetworkPolicy` objetos.

## <a name="can-an-admin-manage-users-and-quotas"></a>Um administrador pode gerenciar usuários e cotas?

Sim. Um administrador do Azure Red Hat OpenShift pode gerenciar usuários e cotas, além de acessar todos os usuário criou projetos.

## <a name="can-i-restrict-a-cluster-to-only-certain-azure-ad-users"></a>Pode restringir um cluster para que somente determinados usuários do AD do Azure?

Sim. Você pode restringir quais Azure usuários do AD podem entrar em um cluster por meio da configuração de aplicativo do AD do Azure. Para obter detalhes, consulte [como: Restringir seu aplicativo a um conjunto de usuários](https://docs.microsoft.com/azure/active-directory/develop/howto-restrict-your-app-to-a-set-of-users)

## <a name="can-a-cluster-have-compute-nodes-across-multiple-azure-regions"></a>Pode um cluster tiver nós de computação em várias regiões do Azure?

Não. Todos os nós em um cluster do Azure Red Hat OpenShift devem ser obtidos da mesma região do Azure.

## <a name="are-master-and-infrastructure-nodes-abstracted-away-as-they-are-with-azure-kubernetes-service-aks"></a>São nós mestre e de infraestrutura abstraídos como estão com o serviço de Kubernetes do Azure (AKS)?

Não. Todos os recursos, incluindo o mestre do cluster, execute em sua assinatura do cliente. Esses tipos de recursos são colocados em um grupo de recursos somente leitura.

## <a name="is-open-service-broker-for-azure-osba-supported"></a>É o Service Broker aberto para Azure (OSBA) tem suportada?

Sim. Você pode usar OSBA com Azure Red Hat OpenShift. Ver [Service Broker aberto para Azure](https://github.com/Azure/open-service-broker-azure#openshift-project-template) para obter mais informações.
