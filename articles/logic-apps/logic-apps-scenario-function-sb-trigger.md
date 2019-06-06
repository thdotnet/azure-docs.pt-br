---
title: Chamar ou disparar aplicativos lógicos com Azure Functions e barramento de serviço do Azure
description: Criar funções do Azure que chamar ou disparam aplicativos lógicos por meio do barramento de serviço do Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: jehollan, klam, LADocs
ms.topic: article
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.date: 06/04/2019
ms.openlocfilehash: 3d4f642ae25a179ea2c3241240996da774cd8c23
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66494929"
---
# <a name="call-or-trigger-logic-apps-by-using-azure-functions-and-azure-service-bus"></a>Chamar ou disparar aplicativos lógicos usando o Azure Functions e barramento de serviço do Azure

Você pode usar [Azure Functions](../azure-functions/functions-overview.md) para disparar um aplicativo lógico quando você precisa implantar um ouvinte de longa execução ou tarefa. Por exemplo, você pode criar uma função do Azure que escuta em uma [do barramento de serviço do Azure](../service-bus-messaging/service-bus-messaging-overview.md) na fila e dispara imediatamente um aplicativo lógico como um gatilho de push.

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure. Se você não tiver uma assinatura do Azure, [inscreva-se em uma conta gratuita do Azure](https://azure.microsoft.com/free/).

* Um namespace do barramento de serviço do Azure. Se você não tiver um namespace [primeiro criar o seu namespace](../service-bus-messaging/service-bus-create-namespace-portal.md).

* Um aplicativo de funções do Azure, que é um contêiner para o Azure functions. Se você não tiver um aplicativo de funções [criar seu aplicativo de funções primeiro](../azure-functions/functions-create-first-azure-function.md)e certifique-se de que você selecione o .NET como a pilha de tempo de execução.

* Conhecimento básico sobre [como criar aplicativos lógicos](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="create-logic-app"></a>Criar aplicativo lógico

Para este cenário, você tem uma função de cada aplicativo lógico que você deseja disparar. Primeiro, crie um aplicativo lógico que começa com um gatilho de solicitação HTTP. A função chama esse ponto de extremidade sempre que uma mensagem da fila é recebida.  

1. Entre no [portal do Azure](https://portal.azure.com) e crie um aplicativo lógico em branco.

   Se você estiver familiarizado com aplicativos lógicos, examine [guia de início rápido: criar seu primeiro aplicativo lógico](../logic-apps/quickstart-create-first-logic-app-workflow.md).

1. Na caixa de pesquisa, digite “solicitação http”. Na lista de gatilhos, selecione este gatilho: **Quando uma solicitação HTTP é recebida**

   ![Selecionar gatilho](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger.png)

   Com gatilho de solicitação, você pode, opcionalmente, inserir um esquema JSON para usar com a mensagem da fila. Esquemas JSON ajudam o Designer do aplicativo lógico entender a estrutura para os dados de entrada e facilitar as saídas para você usar no fluxo de trabalho.

1. Para especificar um esquema, insira o esquema na caixa **Esquema JSON do Corpo da Solicitação**, por exemplo:

   ![Especificar o esquema JSON](./media/logic-apps-scenario-function-sb-trigger/when-http-request-received-trigger-schema.png)

   Se você não tem um esquema, mas tem um conteúdo de exemplo no formato JSON, você pode gerar um esquema com base nesse conteúdo.

   1. No gatilho de Solicitação, escolha **Usar o conteúdo de exemplo para gerar o esquema**.

   1. Em **Inserir ou colar um conteúdo JSON de exemplo**, forneça o conteúdo de exemplo e, em seguida, escolha **Concluído**.

      ![Inserir o conteúdo de exemplo](./media/logic-apps-scenario-function-sb-trigger/enter-sample-payload.png)

   Este conteúdo de exemplo gera o seguinte esquema que aparece no gatilho:

   ```json
   {
      "type": "object",
      "properties": {
         "address": {
            "type": "object",
            "properties": {
               "number": {
                  "type": "integer"
               },
               "street": {
                  "type": "string"
               },
               "city": {
                  "type": "string"
               },
               "postalCode": {
                  "type": "integer"
               },
               "country": {
                  "type": "string"
               }
            }
         }
      }
   }
   ```

1. Adicione quaisquer outras ações que você deseja executar depois de receber a mensagem da fila.

   Por exemplo, você pode enviar um email com o conector do Office 365 Outlook.

1. Salve seu aplicativo lógico, o qual gera a URL de retorno de chamada para o gatilho neste aplicativo lógico. Posteriormente, você usa essa URL de retorno de chamada no código para o gatilho de fila de barramento de serviço do Azure.

   O retorno de chamada URL aparece na **URL de HTTP POST** propriedade.

   ![URL de retorno de chamada gerada para o gatilho](./media/logic-apps-scenario-function-sb-trigger/callback-URL-for-trigger.png)

## <a name="create-azure-function"></a>Criar função do Azure

Em seguida, crie a função que atuará como o gatilho e escutará a fila.

1. No portal do Azure, abra e expanda seu aplicativo de funções, se ainda não estiver aberto. 

1. No nome do aplicativo de funções, expanda **Funções**. No painel **Funções**, escolha **Nova função**.

   ![Expanda "Funções" e escolha "Nova função"](./media/logic-apps-scenario-function-sb-trigger/create-new-function.png)

1. Selecione esse modelo com base em se você criou um novo aplicativo de funções em que você selecionou .NET como a pilha de tempo de execução, ou você estiver usando um aplicativo de função existente.

   * Para novos aplicativos de função, selecione esse modelo: **Gatilho de fila do barramento de serviço**

     ![Selecione o modelo para o novo aplicativo de funções](./media/logic-apps-scenario-function-sb-trigger/current-add-queue-trigger-template.png)

   * Para um aplicativo de função existente, selecione esse modelo: **Gatilho de fila do barramento de serviço-C#**

     ![Selecione o modelo para o aplicativo de funções existente](./media/logic-apps-scenario-function-sb-trigger/legacy-add-queue-trigger-template.png)

1. Sobre o **gatilho de fila de barramento de serviço do Azure** painel, forneça um nome para o gatilho e configurar o **conexão do barramento de serviço** para a fila, que usa o SDK do barramento de serviço do Azure `OnMessageReceive()` ouvinte e escolha **Criar**.

1. Grave uma função básica para chamar o ponto de extremidade do aplicativo lógico criado anteriormente usando a mensagem da fila como um gatilho. Este exemplo usa o tipo de conteúdo de mensagem `application/json`, mas você pode alterar isso conforme o necessário. Se possível, reutilize a instância de clientes HTTP. Para obter mais informações, consulte [gerenciar conexões no Azure Functions](../azure-functions/manage-connections.md).

   ```CSharp
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;

   // Callback URL for previously created Request trigger
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/workflows/<remaining-callback-URL>";

   // Reuse the instance of HTTP clients if possible
   private static HttpClient httpClient = new HttpClient();

   public static void Run(string myQueueItem, ILogger log)
   {
       log.LogInformation($"C# ServiceBus queue trigger function processed message: {myQueueItem}");

       var response = httpClient.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
   }
   ```

1. Para testar a função, adicione uma mensagem da fila usando uma ferramenta como o [Gerenciador do Barramento de Serviço](https://github.com/paolosalvatori/ServiceBusExplorer).

   O aplicativo lógico dispara imediatamente após a função receber a mensagem.

## <a name="next-steps"></a>Próximas etapas

[Chamar, disparar ou aninhar fluxos de trabalho por meio de pontos de extremidade HTTP](../logic-apps/logic-apps-http-endpoint.md)