---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 0fba49ec5a610d03ece2dce7fa5bb0d9d094ab4c
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71203185"
---
## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com o comando [az group create](/cli/azure/group#az-group-create). Um grupo de recursos do Azure é um contêiner lógico no qual os recursos do Azure, como os aplicativos de funções, bancos de dados e contas de armazenamento, são implantados e gerenciados.

O seguinte exemplo cria um grupo de recursos chamado `myResourceGroup`.  
Caso você não esteja usando o Cloud Shell, entre primeiro usando `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Em geral, você cria seu grupo de recursos e os recursos em uma [região](https://azure.microsoft.com/global-infrastructure/regions/) próxima a você. 
