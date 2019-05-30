---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.openlocfilehash: a3f75b7273164abc5318f16e9ab8d9883ff0c0aa
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132082"
---
## <a name="test"></a>Testar a função no Azure

Use o cURL para testar a função implantada. Usando a URL que você copiou da etapa anterior, acrescente a cadeia de consulta `&name=<yourname>` à URL, como no seguinte exemplo:

```bash
curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
```

![usando o cURL para chamar a função no Azure.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png) 

Você também pode colar a URL copiada para o endereço do navegador da Web. Acrescente novamente o valor de cadeia de consulta `&name=<yourname>` à URL e execute a solicitação.

![Usando um navegador da Web para chamar a função.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
