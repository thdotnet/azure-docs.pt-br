---
title: Enviar e receber eventos usando node. js-hubs de eventos do Azure | Microsoft Docs
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
ms.openlocfilehash: 3bb222d3197ef37d56767300d71cc350d25a37bd
ms.sourcegitcommit: 909ca340773b7b6db87d3fb60d1978136d2a96b0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70984479"
---
# <a name="send-events-to-or-receive-events-from-azure-event-hubs-using-nodejs"></a>Enviar eventos para ou receber eventos de hubs de eventos do Azure usando o Node. js

O Hubs de Eventos do Azure é uma plataforma de streaming de Big Data e um serviço de ingestão de eventos que pode receber e processar milhões de eventos por segundo. Os Hubs de Eventos podem processar e armazenar eventos, dados ou telemetria produzidos pelos dispositivos e software distribuídos. Os dados enviados para um Hub de Eventos podem ser transformados e armazenados usando qualquer provedor de análise em tempo real ou adaptadores de envio em lote/armazenamento. Para obter uma visão detalhada dos Hubs de Eventos, confira [Visão geral de Hubs de Eventos](event-hubs-about.md) e [Recursos de Hubs de Eventos](event-hubs-features.md).

Este tutorial descreve como criar aplicativos node. js para enviar eventos ou receber eventos de um hub de eventos.

> [!NOTE]
> Você pode baixar do [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client) este início rápido como um exemplo, substituir as cadeias de caracteres `EventHubConnectionString` e `EventHubName` pelos valores do hub de eventos e executá-lo. Como alternativa, é possível seguir as etapas deste tutorial para criar sua própria solução.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este tutorial, você precisará dos seguintes pré-requisitos:

- Uma conta ativa do Azure. Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.
- Versão do Node.js 8.x e superiores. Baixe a versão mais recente do LTS em [https://nodejs.org](https://nodejs.org).
- Visual Studio Code (recomendado) ou qualquer outro IDE
- **Crie um namespace de hubs de eventos e um hub de eventos**. A primeira etapa é usar o [portal do Azure](https://portal.azure.com) para criar um namespace do tipo Hubs de eventos e obter as credenciais de gerenciamento das quais que seu aplicativo precisa para se comunicar com o hub de eventos. Para criar um namespace e um hub de eventos, siga o procedimento [neste artigo](event-hubs-create.md) e então continue com as etapas a seguir neste tutorial. Em seguida, obtenha a cadeia de conexão para o namespace do hub de eventos seguindo as instruções do artigo: [Obter a cadeia de conexão](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Você usa a cadeia de conexão posteriormente no tutorial.


### <a name="install-npm-package"></a>Instalar pacote NPM
Para instalar o [pacote NPM para os hubs de eventos](https://www.npmjs.com/package/@azure/event-hubs), abra um prompt de `npm` comando que tenha em seu caminho, altere o diretório para a pasta onde você deseja ter seus exemplos e, em seguida, execute este comando

```shell
npm install @azure/event-hubs
```

Para instalar o [pacote NPM para o host do processador de eventos](https://www.npmjs.com/package/@azure/event-processor-host), execute o comando abaixo em vez disso

```shell
npm install @azure/event-processor-host
```

## <a name="send-events"></a>Enviar eventos

Esta seção mostra como criar um aplicativo node. js que envia eventos para um hub de eventos. 

1. Abra seu editor favorito, como [Visual Studio Code](https://code.visualstudio.com). 
2. Crie um arquivo chamado `send.js` e cole o código abaixo nele. Obtenha a cadeia de conexão para o namespace do hub de eventos seguindo as instruções do artigo: [Obter a cadeia de conexão](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). 

    ```javascript
    const { EventHubClient } = require("@azure/event-hubs");

    // Connection string - primary key of the Event Hubs namespace. 
    // For example: Endpoint=sb://myeventhubns.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    const connectionString = "Endpoint=sb://<EVENT HUBS NAMESPACE NAME>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<SHARED ACCESS KEY>";

    // Name of the event hub. For example: myeventhub
    const eventHubsName = "<EVENT HUB NAME>";

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
3. Insira a cadeia de conexão e o nome do seu hub de eventos no código acima
4. Em seguida, execute `node send.js` o comando em um prompt de comando para executar esse arquivo. Isso enviará eventos 100 para o Hub de eventos

Parabéns! Agora você enviou eventos para um hub de eventos.


## <a name="receive-events"></a>Receber eventos

Esta seção mostra como criar um aplicativo node. js que recebe eventos de uma única partição do grupo de consumidores padrão em um hub de eventos. 

1. Abra seu editor favorito, como [Visual Studio Code](https://code.visualstudio.com). 
2. Crie um arquivo chamado `receive.js` e cole o código abaixo nele.
    ```javascript
    const { EventHubClient, delay } = require("@azure/event-hubs");

    // Connection string - primary key of the Event Hubs namespace. 
    // For example: Endpoint=sb://myeventhubns.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    const connectionString = "Endpoint=sb://<EVENT HUBS NAMESPACE NAME>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<SHARED ACCESS KEY>";

    // Name of the event hub. For example: myeventhub
    const eventHubsName = "<EVENT HUB NAME>";

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
3. Insira a cadeia de conexão e o nome do seu hub de eventos no código acima.
4. Em seguida, execute `node receive.js` o comando em um prompt de comando para executar esse arquivo. Isso receberá eventos de uma das partições do grupo de consumidores padrão em seu hub de eventos

Parabéns! Agora você recebeu eventos do hub de eventos.

## <a name="receive-events-using-event-processor-host"></a>Receber eventos usando o Host de Processador de Eventos

Esta seção mostra como receber eventos de um hub de eventos usando o Azure [EventProcessorHost](event-hubs-event-processor-host.md) em um aplicativo node. js. O EPH (EventProcessorHost) ajuda você a receber eventos de um hub de eventos com eficiência criando receptores em todas as partições no grupo de consumidores de um hub de eventos. Ele usa um ponto de verificação nos metadados das mensagens recebidas em intervalos regulares em um Azure Storage Blob. Com essa abordagem, fica fácil continuar a receber mensagens mais tarde exatamente no ponto em que você parou.

1. Abra seu editor favorito, como [Visual Studio Code](https://code.visualstudio.com). 
2. Crie um arquivo chamado `receiveAll.js` e cole o código abaixo nele.
    ```javascript
    const { EventProcessorHost, delay } = require("@azure/event-processor-host");

    // Connection string - primary key of the Event Hubs namespace. 
    // For example: Endpoint=sb://myeventhubns.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
    const eventHubConnectionString = "Endpoint=sb://<EVENT HUBS NAMESPACE NAME>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<SHARED ACCESS KEY>";

    // Name of the event hub. For example: myeventhub
    const eventHubName = "<EVENT HUB NAME>";

    // Azure Storage connection string
    const storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=<STORAGE ACCOUNT NAME>;AccountKey=<STORAGE ACCOUNT KEY>;EndpointSuffix=core.windows.net";

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
3. Insira a cadeia de conexão e o nome do seu hub de eventos no código acima junto com a cadeia de conexão para um armazenamento de BLOBs do Azure
4. Em seguida, execute `node receiveAll.js` o comando em um prompt de comando para executar esse arquivo.

Parabéns! Agora você recebeu eventos do hub de eventos usando o host do processador de eventos. Isso receberá eventos de todas as partições do grupo de consumidores padrão em seu hub de eventos

## <a name="next-steps"></a>Próximas etapas
Leia os seguintes artigos:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Recursos e terminologia nos Hubs de Eventos do Azure](event-hubs-features.md)
- [Perguntas frequentes sobre os Hubs de Eventos](event-hubs-faq.md)
- Confira outros exemplos de Node. js para [hubs de eventos](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples) e host de processador de [eventos](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-processor-host/samples) no github
