---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 9fefba9c79e9c58956243de8db67ca4cf316251c
ms.sourcegitcommit: 78ebf29ee6be84b415c558f43d34cbe1bcc0b38a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68949962"
---
## <a name="deploy-the-function-app-project-to-azure"></a>Implantar o projeto de aplicativo de funções no Azure

Depois que o aplicativo de funções for criado no Azure, você poderá usar o comando do Core Tools [`func azure functionapp publish`](../articles/azure-functions/functions-run-local.md#project-file-deployment) para implantar o código do projeto no Azure. Nestes exemplos, substitua `<APP_NAME>` pelo nome do seu aplicativo da etapa anterior.

### <a name="c--javascript"></a>C\# / JavaScript

```command
func azure functionapp publish <APP_NAME>
```

### <a name="typescript"></a>TypeScript

```command
npm run build:production 
func azure functionapp publish <APP_NAME>
```

Você verá a saída semelhante à seguinte, que foi truncada para facilitar a leitura:

```output
Getting site publishing info...
...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
Functions in myfunctionapp:
    HttpTrigger - [httpTrigger]
        Invoke url: https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....
```

Copie o valor `Invoke url` para seu `HttpTrigger`, que você agora poderá usar para testar sua função no Azure. A URL contém um valor da cadeia de caracteres de consulta `code` que é sua chave de função. Essa chave dificulta para outras pessoas chamarem seu ponto de extremidade de gatilho HTTP no Azure.