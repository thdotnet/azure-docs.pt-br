---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/05/2019
ms.author: glenga
ms.openlocfilehash: 0493de7374cffcc40f0434d84facf50b6a708c7b
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68592878"
---
1. Para executar a função, pressione **F5**. É necessário habilitar a exceção de firewall para que as ferramentas possam lidar com solicitações HTTP. Os níveis de autorização nunca são impostos quando estão em execução localmente.

2. Copie a URL da sua função da saída de tempo de execução do Azure Functions.

    ![Tempo de execução local do Azure](./media/functions-run-function-test-local-vs/functions-debug-local-vs.png)

3. Cole a URL para a solicitação HTTP na barra de endereços do navegador. Acrescente o valor de cadeia de consulta `?name=<YOUR_NAME>` a essa URL e execute a solicitação. O exemplo a seguir mostra a resposta no navegador à solicitação GET local retornada pela função: 

    ![Resposta da função localhost no navegador](./media/functions-run-function-test-local-vs/functions-run-browser-local-vs.png)

4. Para interromper a depuração, pressione **Shift + F5**.