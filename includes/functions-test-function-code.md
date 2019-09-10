---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.openlocfilehash: 9381f436aaeedb31732f853a6c4765ac43c6a752
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70170856"
---
## <a name="test"></a>Testar a função no Azure

Use o cURL para testar a função implantada. Usando a URL, incluindo a chave de função, que você copiou da etapa anterior, acrescente a cadeia de consulta `&name=<yourname>` à URL.

![usando o cURL para chamar a função no Azure.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png) 

Você também pode colar a URL copiada, incluindo a chave de função, para o endereço do navegador da Web. Acrescente novamente o valor de cadeia de consulta `&name=<yourname>` à URL e execute a solicitação.

![Usando um navegador da Web para chamar a função.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
