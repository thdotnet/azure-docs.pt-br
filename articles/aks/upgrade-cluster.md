---
title: Atualizar um cluster do Serviço de Kubernetes do Azure (AKS)
description: Saiba como atualizar um cluster do AKS (Serviço de Kubernetes do Azure)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: mlearned
ms.openlocfilehash: dd88b5a044fe495da374178be8774f45bdd30f61
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614056"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Atualizar um cluster do Serviço de Kubernetes do Azure (AKS)

Como parte do ciclo de vida de um cluster do AKS, muitas vezes você precisará atualizar para a versão mais recente do Kubernetes. É importante aplicar as versões mais recentes de segurança do Kubernetes ou atualizar para obter os recursos mais recentes. Este artigo mostra como atualizar os componentes mestres ou um único pool de nós de padrão em um cluster AKS.

Para o AKS clusters que usam vários pools de nós ou nós do Windows Server (tanto atualmente em visualização no AKS), consulte [Upgrade de um pool de nós no AKS][nodepool-upgrade].

## <a name="before-you-begin"></a>Antes de começar

Este artigo exigirá que você está executando a CLI do Azure versão 2.0.65 ou posterior. Execute `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, consulte [Instalar a CLI do Azure][azure-cli-install].

## <a name="check-for-available-aks-cluster-upgrades"></a>Verificação de atualizações disponíveis do cluster do AKS

Para verificar quais versões do Kubernetes estão disponíveis para seu cluster, use o [az aks get-upgrades][az-aks-get-upgrades] comando. O exemplo abaixo verifica atualizações disponíveis para o cluster nomeado *myAKSCluster* em um grupo de recursos nomeado *myResourceGroup*:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> Ao atualizar um cluster do AKS, as versões secundárias do Kubernetes não poderão ser ignoradas. Por exemplo, upgrades entre *1.11.x* -> *1.12.x* ou *1.12.x* -> *1.13.x* são permitidos, porém *1.11.x* -> *1.13.x* não é.
>
> A atualização, do *1.11.x* -> *1.13.x*, primeiro atualizar do *1.11.x* -> *1.12.x*, em seguida, faça upgrade partir *1.12.x* -> *1.13.x*.

A saída de exemplo a seguir mostra que o cluster pode ser atualizado para a versão *1.12.7* ou *1.12.8*:

```console
Name     ResourceGroup    MasterVersion  NodePoolVersion  Upgrades
-------  ---------------  -------------  ---------------  --------------
default  myResourceGroup  1.11.9         1.11.9           1.12.7, 1.12.8
```

## <a name="upgrade-an-aks-cluster"></a>Atualizar um cluster AKS

Com uma lista de versões disponíveis para o cluster do AKS, use o [az aks upgrade][az-aks-upgrade] command to upgrade. During the upgrade process, AKS adds a new node to the cluster that runs the specified Kubernetes version, then carefully [cordon and drains][kubernetes-drain] um de nós do antigos para minimizar as interrupções nos aplicativos em execução. Quando o novo nó foi confirmado como pods de aplicativo em execução, o nó antigo será excluído. Esse processo se repete até que todos os nós no cluster tenham sido atualizados.

O exemplo a seguir atualiza um cluster para versão *1.12.8*:

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.12.8
```

O cluster demora alguns minutos para ser atualizado, dependendo de quantos nós que você tem.

Para confirmar que a atualização foi bem-sucedida, use o [show do az aks][az-aks-show] comando:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

A saída de exemplo a seguir mostra que o cluster agora é executada *1.12.8*:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ---------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.12.8               Succeeded            myaksclust-myresourcegroup-19da35-90efab95.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Próximas etapas

Este artigo mostrou como atualizar um cluster do AKS existente. Para saber mais sobre como implantar e gerenciar clusters do AKS, confira os tutoriais.

> [!div class="nextstepaction"]
> [Tutoriais do AKS][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
