---
title: Como criar um cluster Kubernetes habilitado para espaços de desenvolvimento do Azure usando o Azure Cloud Shell
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 10/04/2018
ms.topic: conceptual
description: Aprenda a criar rapidamente um cluster do Kubernetes habilitado para Azure Dev Spaces diretamente do navegador sem instalar nada.
keywords: Docker, Kubernetes, Azure, AKS, Serviço de Kubernetes do Azure, contêineres, Helm, malha de serviço, roteamento de malha de serviço, kubectl, k8s
ms.openlocfilehash: c9dabc13e85295b88483f43b26ccf0b15406ad9b
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65861620"
---
# <a name="create-a-kubernetes-cluster-using-azure-cloud-shell"></a>Criar um cluster do Kubernetes usando o Azure Cloud Shell

É possível usar o [Azure Cloud Shell](/azure/cloud-shell) para criar um cluster do Azure Dev Spaces usando o botão **Experimentar** desta página. Se não estiver conectado, siga os prompts para entrar com uma conta do Azure e, em seguida, digite os comandos no prompt do Azure Cloud Shell quando for exibido.

## <a name="create-the-cluster"></a>Criar o cluster

Primeiro, crie o grupo de recursos em um [região que dá suporte a espaços de desenvolvimento do Azure](https://docs.microsoft.com/azure/dev-spaces/#a-rapid,-iterative-kubernetes-development-experience-for-teams).

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Crie um cluster Kubernetes com o seguinte comando:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --disable-rbac --generate-ssh-keys
```

São necessários alguns minutos para criar o cluster.  Quando concluída, a saída será mostrada no formato JSON. Procure `provisioningState` e verifique se é `Succeeded`.

## <a name="next-steps"></a>Próximas etapas

Consulte [Azure Dev Spaces](/azure/dev-spaces/) para obter links e ter acesso a tutoriais completos.
