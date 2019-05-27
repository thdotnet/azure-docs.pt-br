---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: c20f86fe7fdcfc7ecc940923a8c98fa1fbf4cf65
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132173"
---
## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um grupo de recursos com [az group create](/cli/azure/group). Um grupo de recursos do Azure é um contêiner lógico no qual os recursos do Azure, como os aplicativos de funções, bancos de dados e contas de armazenamento, são implantados e gerenciados.

O seguinte exemplo cria um grupo de recursos chamado `myResourceGroup`.  
Se você não estiver usando o Cloud Shell, primeiro você deverá entrar usando `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```
Em geral, você cria seu grupo de recursos e os recursos em uma região próxima a você. Para ver todos os locais com suporte para planos do Serviço de Aplicativo, execute o comando [az appservice list-locations](/cli/azure/appservice#az-appservice-list-locations).
