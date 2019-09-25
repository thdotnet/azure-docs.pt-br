---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 4dd43c5bcc5a0e9a734db4ca9a4b3d7137f85250
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71203120"
---
## <a name="create-an-azure-storage-account"></a>Criar uma conta de Armazenamento do Azure

O Functions usa uma conta de propósito geral do Armazenamento do Azure para manter o estado e outras informações sobre suas funções. Crie uma conta de propósito geral de armazenamento no grupo de recursos que você criou ao utilizar o comando [az storage account create](/cli/azure/storage/account#az-storage-account-create).

No comando a seguir, substitua um nome da conta de armazenamento globalmente exclusivo quando você vir o espaço reservado `<storage_name>`. Os nomes da conta de armazenamento devem ter entre 3 e 24 caracteres e podem conter apenas números e letras minúsculas.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```
