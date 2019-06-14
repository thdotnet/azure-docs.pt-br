---
title: Como usar as filas do barramento de serviço do Azure no Node. js – azure/do barramento de serviço | Microsoft Docs
description: Aprenda a usar as filas do Barramento de Serviço no Azure a partir de um aplicativo Node.js.
services: service-bus-messaging
documentationcenter: nodejs
author: axisc
manager: timlt
editor: spelluru
ms.assetid: a87a00f9-9aba-4c49-a0df-f900a8b67b3f
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 04/10/2019
ms.author: aschhab
ms.openlocfilehash: 7aacefde9c037fcce64d9256e35082eb04e0a2f3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65988361"
---
# <a name="how-to-use-service-bus-queues-with-nodejs-and-the-azureservice-bus-package"></a>Como usar filas do barramento de serviço com Node. js e o pacote de barramento de serviço/do azure
> [!div class="op_multi_selector" title1="Linguagem de programação" title2="Pacote de Node. js"]
> - [(Node.js | azure-sb)](service-bus-nodejs-how-to-use-queues.md)
> - [(Node.js | @azure/service-bus)](service-bus-nodejs-how-to-use-queues-new-package.md)

Neste tutorial, você aprenderá como escrever um programa Nodejs para enviar e receber mensagens de uma fila do barramento de serviço usando o novo [ @azure/service-bus ](https://www.npmjs.com/package/@azure/service-bus) pacote. Esse pacote usa o mais rápido [protocolo AMQP 1.0](service-bus-amqp-overview.md) enquanto o mais antigo [azure-sb](https://www.npmjs.com/package/azure-sb) pacote usado [APIs de tempo de execução do REST do barramento de serviço](/rest/api/servicebus/service-bus-runtime-rest). Os exemplos são escritos em JavaScript.

## <a name="prerequisites"></a>Pré-requisitos
- Uma assinatura do Azure. Para concluir este tutorial, você precisa de uma conta do Azure. Você pode ativar sua [benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) ou se inscrever para uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
- Se você não tiver uma fila para trabalhar com, siga as etapas na [portal do Azure de uso para criar uma fila do barramento de serviço](service-bus-quickstart-portal.md) artigo para criar uma fila. Anote a cadeia de caracteres de conexão para sua instância do barramento de serviço e o nome da fila que você criou. Usaremos esses valores nos exemplos.

> [!NOTE]
> - Este tutorial funciona com exemplos que você pode copiar e executados usando [Nodejs](https://nodejs.org/). Para obter instruções sobre como criar um aplicativo Node. js, consulte [criar e implantar um aplicativo Node. js para um site do Azure](../app-service/app-service-web-get-started-nodejs.md), ou [serviço de nuvem do Node. js usando o Windows PowerShell](../cloud-services/cloud-services-nodejs-develop-deploy-app.md).
> - O novo [ @azure/service-bus ](https://www.npmjs.com/package/@azure/service-bus) pacote não dá suporte à criação de filas ainda. Use o [ @azure/arm-servicebus ](https://www.npmjs.com/package/@azure/arm-servicebus) de pacote para criá-los de forma programática.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Usar o NPM (Gerenciador de Pacotes de Nós) para obter o pacote
Para instalar o pacote npm do barramento de serviço, abra um prompt de comando que tenha `npm` em seu caminho, altere o diretório para a pasta onde você deseja ter seus exemplos e, em seguida, execute este comando.

```bash
npm install @azure/service-bus
```

## <a name="send-messages-to-a-queue"></a>Enviar mensagens a uma fila
Interagindo com um barramento de serviço de fila inicia com instanciar o [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) classe e usá-lo para criar uma instância de [QueueClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/queueclient) classe. Depois que o cliente de fila, você pode criar um remetente e use um [envie](https://docs.microsoft.com/javascript/api/%40azure/service-bus/sender#send-sendablemessageinfo-) ou [sendBatch](https://docs.microsoft.com/javascript/api/@azure/service-bus/sender#sendbatch-sendablemessageinfo---) método nele para enviar mensagens.

1. Abra seu editor favorito, como [Visual Studio Code](https://code.visualstudio.com/)
2. Crie um arquivo chamado `send.js` e cole o código para ele abaixo. Esse código enviará 10 mensagens na fila.

    ```javascript
    const { ServiceBusClient } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const queueName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const queueClient = sbClient.createQueueClient(queueName);
      const sender = queueClient.createSender();
      
      try {
        for (let i = 0; i < 10; i++) {
          const message= {
            body: `Hello world! ${i}`,
            label: `test`,
            userProperties: {
                myCustomPropertyName: `my custom property value ${i}`
           }
          };
          console.log(`Sending message: ${message.body}`);
          await sender.send(message);
        }
        
        await queueClient.close();
      } finally {
        await sbClient.close();
      }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Insira a cadeia de conexão e o nome da fila no código acima.
4. Em seguida, execute o comando `node send.js` em um prompt de comando para executar esse arquivo.

Parabéns! Você acabou de enviar mensagens para uma fila do barramento de serviço.

As mensagens têm algumas propriedades padrão, como `label` e `messageId` que podem ser definidas durante o envio. Se você quiser definir todas as propriedades personalizadas, use o `userProperties`, que é um objeto json que pode conter pares chave-valor de seus dados personalizados.

As filas do Barramento de Serviço dão suporte ao tamanho máximo de mensagem de 256 KB na [camada Standard](service-bus-premium-messaging.md) e 1 MB na [camada Premium](service-bus-premium-messaging.md). Não há nenhum limite no número de mensagens mantidas em uma fila, mas há um limite no tamanho total das mensagens mantidas por uma fila. O tamanho da fila é definido no momento da criação, com um limite superior de 5 GB. Para saber mais sobre cotas, consulte [Cotas do Barramento de Serviço](service-bus-quotas.md).

## <a name="receive-messages-from-a-queue"></a>Receber mensagens de uma fila
Interagindo com um barramento de serviço de fila inicia com instanciar o [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) classe e usá-lo para criar uma instância de [QueueClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/queueclient) classe. Depois que o cliente de fila, você pode criar um receptor e use um [receiveMessages](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#receivemessages-number--undefined---number-) ou [registerMessageHandler](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#registermessagehandler-onmessage--onerror--messagehandleroptions-) método nele para receber mensagens.

1. Abra seu editor favorito, como [Visual Studio Code](https://code.visualstudio.com/)
2. Crie um arquivo chamado `recieve.js` e cole o código para ele abaixo. Esse código tentará receber 10 mensagens da fila. A contagem real, que você recebe depende do número de mensagens na latência de rede e de fila.

    ```javascript
    const { ServiceBusClient, ReceiveMode } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const queueName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const queueClient = sbClient.createQueueClient(queueName);
      const receiver = queueClient.createReceiver(ReceiveMode.ReceiveAndDelete);
      try {
        const messages = await receiver.receiveMessages(10)
        console.log("Received messages:");
        console.log(messages.map(message => message.body));
         
        await queueClient.close();
      } finally {
        await sbClient.close();
      }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Insira a cadeia de conexão e o nome da fila no código acima.
4. Em seguida, execute o comando `node receiveMessages.js` em um prompt de comando para executar esse arquivo.

Parabéns! Você acabou de receber mensagens de uma fila do barramento de serviço.

O [createReceiver](https://docs.microsoft.com/javascript/api/%40azure/service-bus/queueclient#createreceiver-receivemode-) método usa um `ReceiveMode` que é uma enumeração com valores [ReceiveAndDelete](message-transfers-locks-settlement.md#settling-receive-operations) e [PeekLock](message-transfers-locks-settlement.md#settling-receive-operations). Lembre-se [liquidar as mensagens](message-transfers-locks-settlement.md#settling-receive-operations) se você usar o `PeekLock` modo usando qualquer um dos `complete()`, `abandon()`, `defer()`, ou `deadletter()` métodos na mensagem.

> [!NOTE]
> É possível gerenciar os recursos do Barramento de Serviço com o [Gerenciador de Barramento de Serviço](https://github.com/paolosalvatori/ServiceBusExplorer/). O Gerenciador de Barramento de Serviço permite que usuários se conectem a um namespace de serviço do Barramento de Serviço e administrem entidades de mensagens de uma maneira fácil. A ferramenta fornece recursos avançados, como a funcionalidade de importação/exportação ou a capacidade de testar tópicos, filas, assinaturas, serviços de retransmissão, hubs de notificação e hubs de eventos. 

## <a name="next-steps"></a>Próximas etapas
Para obter mais informações, consulte os seguintes recursos.
- [Filas, tópicos e assinaturas](service-bus-queues-topics-subscriptions.md)
- Confira outros [Nodejs exemplos do barramento de serviço no GitHub](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/servicebus/service-bus/samples/javascript)
- [Centro de desenvolvedores do Node. js](https://azure.microsoft.com/develop/nodejs/)

