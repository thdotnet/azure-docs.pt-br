---
title: Enviar e receber eventos usando o Node. js – os Hubs de eventos do Azure | Microsoft Docs
description: Este artigo apresenta instruções passo a passo para a criação de um aplicativo Node.js que envia eventos de Hubs de Eventos do Azure.
services: event-hubs
author: spelluru
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 04/15/2019
ms.author: spelluru
ms.openlocfilehash: e67be59e0ed78b2080986acb73a33fc87599c9d3
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65539338"
---
# <a name="send-events-to-or-receive-events-from-azure-event-hubs-using-nodejs"></a>Enviar eventos para ou receber eventos de Hubs de eventos do Azure usando Node. js

O Hubs de Eventos do Azure é uma plataforma de streaming de Big Data e um serviço de ingestão de eventos que pode receber e processar milhões de eventos por segundo. Os Hubs de Eventos podem processar e armazenar eventos, dados ou telemetria produzidos pelos dispositivos e software distribuídos. Os dados enviados para um Hub de Eventos podem ser transformados e armazenados usando qualquer provedor de análise em tempo real ou adaptadores de envio em lote/armazenamento. Para obter uma visão detalhada dos Hubs de Eventos, confira [Visão geral de Hubs de Eventos](event-hubs-about.md) e [Recursos de Hubs de Eventos](event-hubs-features.md).

Este tutorial descreve como criar aplicativos Node. js para enviar eventos para ou receber eventos de um hub de eventos.

> [!NOTE]
> Você pode baixar do [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client) este início rápido como um exemplo, substituir as cadeias de caracteres `EventHubConnectionString` e `EventHubName` pelos valores do hub de eventos e executá-lo. Como alternativa, é possível seguir as etapas deste tutorial para criar sua própria solução.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, você precisará dos seguintes pré-requisitos:

- Uma conta ativa do Azure. Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.
- Versão do Node.js 8.x e superiores. Baixe a versão mais recente do LTS em [https://nodejs.org](https://nodejs.org).
- Visual Studio Code (recomendado) ou qualquer outro IDE
- **Criar um namespace de Hubs de eventos e um hub de eventos**. A primeira etapa é usar o [portal do Azure](https://portal.azure.com) para criar um namespace do tipo Hubs de eventos e obter as credenciais de gerenciamento das quais que seu aplicativo precisa para se comunicar com o hub de eventos. Para criar um namespace e um hub de eventos, siga o procedimento [neste artigo](event-hubs-create.md) e então continue com as etapas a seguir neste tutorial. Em seguida, obtenha a cadeia de conexão para o namespace de hub de eventos seguindo as instruções do artigo: [Obter a cadeia de conexão](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Você usa a cadeia de conexão posteriormente no tutorial.


### <a name="install-npm-package"></a>Instalar o pacote npm
Para instalar o [pacote npm dos Hubs de eventos](https://www.npmjs.com/package/@azure/event-hubs), abra um prompt de comando que tenha `npm` em seu caminho, altere o diretório para a pasta onde você deseja ter seus exemplos e, em seguida, execute este comando

```shell
npm install @azure/event-hubs
```

Para instalar o [pacote npm para Host do processador de eventos](https://www.npmjs.com/package/@azure/event-processor-host), execute o comando abaixo em vez disso

```shell
npm install @azure/event-processor-host
```

## <a name="send-events"></a>Enviar eventos

Esta seção mostra como criar um aplicativo Node. js que envia eventos para um hub de eventos. 

1. Abra seu editor favorito, como [Visual Studio Code](https://code.visualstudio.com). 
2. Crie um arquivo chamado `send.js` e cole o código para ele abaixo.
    ```javascript
    const { EventHubClient } = require("@azure/event-hubs");

    // Define connection string and the name of the Event Hub
    const connectionString = "";
    const eventHubsName = "";

    async function main() {
      const client = EventHubClient.createFromConnectionString(connectionString, eventHubsName);

      for (let i = 0; i < 100; i++) {
        const eventData = {body: `Event ${i}`};
        console.log(`Sending message: ${eventData.body}`);
        await client.send(eventData);
      }

      await client.close();
    }

    main().catch(err => {
      console.log("Error occurred: ", err);
    });
    ```
3. Insira a cadeia de caracteres de conexão e o nome do Hub de eventos no código acima
4. Em seguida, execute o comando `node send.js` em um prompt de comando para executar esse arquivo. Isso enviará os 100 eventos ao Hub de eventos

Parabéns! Agora você enviou eventos para um hub de eventos.


## <a name="receive-events"></a>Receber eventos

Esta seção mostra como criar um aplicativo Node. js que recebe eventos de uma única partição do grupo de consumidores padrão em um hub de eventos. 

1. Abra seu editor favorito, como [Visual Studio Code](https://code.visualstudio.com). 
2. Crie um arquivo chamado `receive.js` e cole o código para ele abaixo.
    ```javascript
    const { EventHubClient, delay } = require("@azure/event-hubs");

    // Define connection string and related Event Hubs entity name here
    const connectionString = "";
    const eventHubsName = "";

    async function main() {
      const client = EventHubClient.createFromConnectionString(connectionString, eventHubsName);
      const allPartitionIds = await client.getPartitionIds();
      const firstPartitionId = allPartitionIds[0];

      const receiveHandler = client.receive(firstPartitionId, eventData => {
        console.log(`Received message: ${eventData.body} from partition ${firstPartitionId}`);
      }, error => {
        console.log('Error when receiving message: ', error)
      });

      // Sleep for a while before stopping the receive operation.
      await delay(15000);
      await receiveHandler.stop();

      await client.close();
    }

    main().catch(err => {
      console.log("Error occurred: ", err);
    });
    ```
3. Insira a cadeia de caracteres de conexão e o nome do Hub de eventos no código acima.
4. Em seguida, execute o comando `node receive.js` em um prompt de comando para executar esse arquivo. Isso irá receber eventos de uma das partições do grupo de consumidores padrão em seu Hub de eventos

Parabéns! Agora você recebeu a eventos do hub de eventos.

## <a name="receive-events-using-event-processor-host"></a>Receber eventos usando o Host do processador de eventos

Esta seção mostra como receber eventos de um hub de eventos usando o Azure [EventProcessorHost](event-hubs-event-processor-host.md) em um aplicativo Node. js. O EPH (EventProcessorHost) ajuda você a receber eventos de um hub de eventos com eficiência criando receptores em todas as partições no grupo de consumidores de um hub de eventos. Ele usa um ponto de verificação nos metadados das mensagens recebidas em intervalos regulares em um Azure Storage Blob. Com essa abordagem, fica fácil continuar a receber mensagens mais tarde exatamente no ponto em que você parou.

1. Abra seu editor favorito, como [Visual Studio Code](https://code.visualstudio.com). 
2. Crie um arquivo chamado `receiveAll.js` e cole o código para ele abaixo.
    ```javascript
    const { EventProcessorHost, delay } = require("@azure/event-processor-host");

    // Define connection string and related Event Hubs entity name here
    const eventHubConnectionString = "";
    const eventHubName = "";
    const storageConnectionString = "";

    async function main() {
      const eph = EventProcessorHost.createFromConnectionString(
        "my-eph",
        storageConnectionString,
        "my-storage-container-name",
        eventHubConnectionString,
        {
          eventHubPath: eventHubName,
          onEphError: (error) => {
            console.log("[%s] Error: %O", error);
          }
        }
      );


      eph.start((context, eventData) => {
        console.log(`Received message: ${eventData.body} from partition ${context.partitionId}`);
      }, error => {
        console.log('Error when receiving message: ', error)
      });

      // Sleep for a while before stopping the receive operation.
      await delay(15000);
      await eph.stop();
    }

    main().catch(err => {
      console.log("Error occurred: ", err);
    });

    ```
3. Insira a cadeia de caracteres de conexão e o nome do Hub de eventos no código acima, juntamente com a cadeia de caracteres de conexão para um armazenamento de BLOBs do Azure
4. Em seguida, execute o comando `node receiveAll.js` em um prompt de comando para executar esse arquivo.

Parabéns! Agora você recebeu a eventos de hub de eventos usando o Host do processador de eventos. Isso irá receber eventos de todas as partições do grupo de consumidores padrão em seu Hub de eventos

## <a name="next-steps"></a>Próximas etapas
Leia os seguintes artigos:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Recursos e a terminologia nos Hubs de eventos do Azure](event-hubs-features.md)
- [Perguntas frequentes sobre os Hubs de Eventos](event-hubs-faq.md)
- Confira outros exemplos de Node. js para [dos Hubs de eventos](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples) e [Event Processor Host](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-processor-host/samples) no GitHub
