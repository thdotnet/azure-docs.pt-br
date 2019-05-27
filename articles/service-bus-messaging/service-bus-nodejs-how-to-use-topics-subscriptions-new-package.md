---
title: Como usar tópicos e assinaturas do Barramento de Serviço do Azure com o Node.js | Microsoft Docs
description: Aprenda a usar assinaturas e tópicos do Barramento de Serviço no Azure por meio de um aplicativo Node.js.
services: service-bus-messaging
documentationcenter: nodejs
author: axisc
manager: timlt
editor: spelluru
ms.assetid: b9f5db85-7b6c-4cc7-bd2c-bd3087c99875
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 04/15/2019
ms.author: aschhab
ms.openlocfilehash: 7686014adb989494e6df277de4137b76c3125696
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65992127"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-nodejs-and-the-azureservice-bus-package"></a>Como usar tópicos do barramento de serviço e assinaturas com Node. js e o pacote de barramento de serviço/do azure
> [!div class="op_multi_selector" title1="Programming language" title2="Node.js pacakge"]
> - [(Node.js | azure-sb)](service-bus-nodejs-how-to-use-topics-subscriptions.md)
> - [(Node.js | @azure/service-bus)](service-bus-nodejs-how-to-use-topics-subscriptions-new-package.md)

Neste tutorial, você aprenderá como escrever um programa do Node. js para enviar mensagens para um tópico do barramento de serviço e receber mensagens de uma assinatura do barramento de serviço usando o novo [ @azure/service-bus ](https://www.npmjs.com/package/@azure/service-bus) pacote. Esse pacote usa o mais rápido [protocolo AMQP 1.0](service-bus-amqp-overview.md) enquanto o mais antigo [azure-sb](https://www.npmjs.com/package/azure-sb) pacote usado [APIs de tempo de execução do REST do barramento de serviço](/rest/api/servicebus/service-bus-runtime-rest). Os exemplos são escritos em JavaScript.

## <a name="prerequisites"></a>Pré-requisitos
- Uma assinatura do Azure. Para concluir este tutorial, você precisa de uma conta do Azure. Você pode ativar sua [benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) ou se inscrever para uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
- Se você não tiver uma assinatura para trabalhar com e um tópico, siga as etapas na [portal do Azure de uso para criar um barramento de serviço tópicos e assinaturas](service-bus-quickstart-topics-subscriptions-portal.md) artigo criá-los. Anote a cadeia de caracteres de conexão para sua instância do barramento de serviço e os nomes de tópico e assinatura que você criou. Usaremos esses valores nos exemplos.

> [!NOTE]
> - Este tutorial funciona com exemplos que você pode copiar e executados usando [Nodejs](https://nodejs.org/). Para obter instruções sobre como criar um aplicativo Node. js, consulte [criar e implantar um aplicativo Node. js para um site do Azure](../app-service/app-service-web-get-started-nodejs.md), ou [serviço de nuvem do Node. js usando o Windows PowerShell](../cloud-services/cloud-services-nodejs-develop-deploy-app.md).
> - O novo [ @azure/service-bus ](https://www.npmjs.com/package/@azure/service-bus) pacote não dá suporte à criação de assinaturas e topcis ainda. Use o [ @azure/arm-servicebus ](https://www.npmjs.com/package/@azure/arm-servicebus) de pacote para criá-los de forma programática.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Usar o NPM (Gerenciador de Pacotes de Nós) para obter o pacote
Para instalar o pacote npm do barramento de serviço, abra um prompt de comando que tenha `npm` em seu caminho, altere o diretório para a pasta onde você deseja ter seus exemplos e, em seguida, execute este comando.

```bash
npm install @azure/service-bus
```

## <a name="send-messages-to-a-topic"></a>Enviar mensagens para um tópico
Interagindo com um barramento de serviço tópico começa com a instanciar o [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) classe e usá-lo para criar uma instância de [TopicClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/topicclient) classe. Depois que o cliente de tópico, você pode criar um remetente e use um [envie](https://docs.microsoft.com/javascript/api/%40azure/service-bus/sender#send-sendablemessageinfo-) ou [sendBatch](https://docs.microsoft.com/javascript/api/@azure/service-bus/sender#sendbatch-sendablemessageinfo---) método nele para enviar mensagens.

1. Abra seu editor favorito, como [Visual Studio Code](https://code.visualstudio.com/)
2. Crie um arquivo chamado `send.js` e cole o código para ele abaixo. Esse código enviará 10 mensagens para o tópico.

    ```javascript
    const { ServiceBusClient } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const topicName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const topicClient = sbClient.createTopicClient(topicName);
      const sender = topicClient.createSender();
      
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

            await topicClient.close();
          } finally {
            await sbClient.close();
          }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Insira a cadeia de conexão e o nome do tópico no código acima.
4. Em seguida, execute o comando `node send.js` em um prompt de comando para executar esse arquivo. 

Parabéns! Você acabou de enviar mensagens para uma fila do barramento de serviço.

As mensagens têm algumas propriedades padrão, como `label` e `messageId` que podem ser definidas durante o envio. Se você quiser definir todas as propriedades personalizadas, use o `userProperties`, que é um objeto json que pode conter pares chave-valor de seus dados personalizados.

Os tópicos do Barramento de Serviço dão suporte ao tamanho máximo de mensagem de 256 KB na [camada Standard](service-bus-premium-messaging.md) e 1 MB na [camada Premium](service-bus-premium-messaging.md). Não há nenhum limite no número de mensagens mantidas em um tópico, mas há um limite no tamanho total das mensagens mantidas por um tópico. O tamanho do tópico é definido no momento da criação, com um limite máximo de 5 GB. Para saber mais sobre cotas, confira [Cotas do Barramento de Serviço](service-bus-quotas.md).

## <a name="receive-messages-from-a-subscription"></a>Receber mensagens de uma assinatura
Interagindo com um barramento de serviço de assinatura inicia com instanciar o [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) classe e usá-lo para criar uma instância de [SubscriptionClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/subscriptionclient) classe. Depois que o cliente de assinatura, você pode criar um receptor e use um [receiveMessages](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#receivemessages-number--undefined---number-) ou [registerMessageHandler](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#registermessagehandler-onmessage--onerror--messagehandleroptions-) método nele para receber mensagens.

1. Abra seu editor favorito, como [Visual Studio Code](https://code.visualstudio.com/)
2. Crie um arquivo chamado `recieve.js` e cole o código para ele abaixo. Esse código tentará receber 10 mensagens de sua assinatura. A contagem real, que você recebe depende do número de mensagens na assinatura e latência de rede.

    ```javascript
    const { ServiceBusClient, ReceiveMode } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const topicName = ""; 
    const subscriptionName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const subscriptionClient = sbClient.createSubscriptionClient(topicName, subscriptionName);
      const receiver = subscriptionClient.createReceiver(ReceiveMode.ReceiveAndDelete);
      
      try {
        const messages = await receiver.receiveMessages(10);
        console.log("Received messages:");
        console.log(messages.map(message => message.body));
        
        await subscriptionClient.close();
      } finally {
        await sbClient.close();
      }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. No código acima, insira a cadeia de caracteres de conexão e nomes de tópico e assinatura.
4. Em seguida, execute o comando `node receiveMessages.js` em um prompt de comando para executar esse arquivo.

Parabéns! Você acabou de receber mensagens de uma assinatura do barramento de serviço.

O [createReceiver](https://docs.microsoft.com/javascript/api/%40azure/service-bus/subscriptionclient#createreceiver-receivemode-) método usa um `ReceiveMode` que é uma enumeração com valores [ReceiveAndDelete](message-transfers-locks-settlement.md#settling-receive-operations) e [PeekLock](message-transfers-locks-settlement.md#settling-receive-operations). Lembre-se [liquidar as mensagens](message-transfers-locks-settlement.md#settling-receive-operations) se você usar o `PeekLock` modo usando qualquer um dos `complete()`, `abandon()`, `defer()`, ou `deadletter()` métodos na mensagem.

## <a name="subscription-filters-and-actions"></a>Ações e filtros de assinatura
O barramento de serviço dá suporte a [filtros e ações em assinaturas](topic-filters.md), que lhe permite filtrar as mensagens de entrada para uma assinatura e editar suas propriedades.

Quando você tiver uma instância de um `SubscriptionClient` você pode usar o abaixo de métodos para obter, adicionar e remover as regras na assinatura para controlar os filtros e ações.

- getRules
- addRule
- removeRule

Cada assinatura tem uma regra padrão que usa o filtro verdadeiro para permitir todas as mensagens de entrada. Quando você adiciona uma nova regra, lembre-se de remover o filtro padrão para o filtro na sua nova regra que funcione. Se uma assinatura não tem regras, ele receberá nenhuma mensagem.

> [!NOTE]
> Você pode gerenciar recursos do barramento de serviço com [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer/). O Service Bus Explorer permite aos usuários para se conectar a um namespace do barramento de serviço e administrar entidades de mensagens de uma maneira fácil. A ferramenta fornece recursos avançados, como a funcionalidade de importação/exportação ou a capacidade de testar tópico, filas, assinaturas, serviços de retransmissão, os hubs de notificação e os hubs de eventos. 

## <a name="next-steps"></a>Próximas etapas
Para obter mais informações, consulte os seguintes recursos.

- [Filas, tópicos e assinaturas](service-bus-queues-topics-subscriptions.md)
- Confira outros [Nodejs exemplos do barramento de serviço no GitHub](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/servicebus/service-bus/samples/javascript)
- [Centro de desenvolvedores do Node. js](https://azure.microsoft.com/develop/nodejs/)


