---
author: robinsh
manager: philmea
ms.author: robinsh
ms.topic: include
ms.date: 05/20/2019
ms.openlocfilehash: c164433efc6a34a3a06676a3145feb18d3de80b9
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66248839"
---
## <a name="add-a-consumer-group-to-your-iot-hub"></a>Adicionar um grupo de consumidores ao hub IoT

[Grupos de consumidores](https://docs.microsoft.com/azure/event-hubs/event-hubs-features#event-consumers) fornecem exibições independentes no fluxo de eventos que permitem que aplicativos e serviços do Azure para consumir independentemente de dados do mesmo ponto de extremidade do Hub de eventos. Nesta seção, você pode adicionar um grupo de consumidores para o ponto de extremidade interno do seu hub IoT que é usado posteriormente no tutorial para extrair dados do ponto de extremidade.

Para adicionar um grupo de consumidores ao hub IoT, siga estas etapas:

1. No [portal do Azure](https://portal.azure.com/), abra o hub IoT.

2. No painel esquerdo, selecione **pontos de extremidade internos**, selecione **eventos** no painel direito e insira um nome de **grupos de consumidores**. Clique em **Salvar**.

   ![Criar um grupo de consumidores no hub IoT](./media/iot-hub-get-started-create-consumer-group/iot-hub-create-consumer-group-azure.png)
