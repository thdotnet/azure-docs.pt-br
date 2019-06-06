---
title: Adicionar e chamar o Azure functions de aplicativos lógicos do Azure
description: Adicionar e executar o Azure functions de aplicativos lógicos
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.topic: article
ms.date: 06/04/2019
ms.reviewer: klam, LADocs
ms.openlocfilehash: 524b927ec0966199c51cdee93e920d7b847139ae
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66495063"
---
# <a name="call-azure-functions-from-azure-logic-apps"></a>Chamar o Azure functions de aplicativos lógicos do Azure

Quando você deseja executar o código que executa um trabalho específico em seus aplicativos lógicos, você pode criar sua própria função usando [Azure Functions](../azure-functions/functions-overview.md). Este serviço ajuda você a criar o Node. js, C#, e F# para que você não precisa criar um aplicativo completo ou a infraestrutura para executar o código de funções. Você também pode [chamar aplicativos lógicos de dentro de funções do Azure](#call-logic-app). O Azure Functions fornece computação sem servidor na nuvem e é útil para executar tarefas, como estes exemplos:

* Estenda o comportamento do seu aplicativo lógico com funções em Node. js ou C#.
* Execute cálculos em seu fluxo de trabalho do aplicativo lógico.
* Aplicar a formatação avançada ou computação de campos em seus aplicativos lógicos.

Para executar trechos de código sem a criação de funções do Azure, saiba como [adicionar e executar o código embutido](../logic-apps/logic-apps-add-run-inline-code.md).

> [!NOTE]
> Integração entre aplicativos lógicos e o Azure Functions no momento, não funciona com Slots habilitados.

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure. Se você não tiver uma assinatura do Azure, [inscreva-se em uma conta gratuita do Azure](https://azure.microsoft.com/free/).

* Um aplicativo de funções do Azure, que é um contêiner para as funções do Azure, juntamente com a função do Azure. Se você não tiver um aplicativo de funções, [crie primeiro o aplicativo de função](../azure-functions/functions-create-first-azure-function.md). Você pode criar sua função seja fora de seu aplicativo lógico no portal do Azure, ou [de dentro de seu aplicativo lógico](#create-function-designer) no Designer do aplicativo lógico.

* Ao trabalhar com aplicativos lógicos, os mesmos requisitos se aplicam a funções e aplicativos de funções, independentemente de estarem novo ou existente:

  * Seu aplicativo de funções e o aplicativo lógico devem usar a mesma assinatura do Azure.

  * Novos aplicativos de função devem usar o .NET ou JavaScript como a pilha de tempo de execução. Quando você adiciona uma nova função para os aplicativos de função existentes, você pode selecionar C# ou JavaScript.

  * A função usa o **gatilho HTTP** modelo.

    Esse modelo de gatilho HTTP pode aceitar o conteúdo que tenha o tipo `application/json` do seu aplicativo lógico. Quando você adiciona uma função do Azure ao seu aplicativo lógico, o Designer de aplicativos lógicos mostra funções personalizadas que são criadas com base neste modelo dentro de sua assinatura do Azure.

  * Sua função não usa rotas personalizadas, a menos que você definiu uma [definição de OpenAPI](../azure-functions/functions-openapi-definition.md) (anteriormente conhecida como uma [arquivo do Swagger](https://swagger.io/)).

  * Se você tiver uma definição de OpenAPI para sua função, o Designer de aplicativos lógicos lhe dá mais rica experiência ao seu trabalho com parâmetros de função. Antes de seu aplicativo lógico pode localizar e acessar funções que têm definições de OpenAPI, [configurar seu aplicativo de função seguindo estas etapas](#function-swagger).

* O aplicativo lógico no qual você deseja adicionar a função, incluindo uma [gatilho](../logic-apps/logic-apps-overview.md#logic-app-concepts) como a primeira etapa no seu aplicativo lógico

  Antes de adicionar ações que executam funções, seu aplicativo lógico deve começar com um gatilho. Se ainda não estiver familiarizado com aplicativo lógicos, consulte [O que são os Aplicativos Lógicos do Azure](../logic-apps/logic-apps-overview.md) e [Início Rápido: criar seu primeiro aplicativo lógico](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="function-swagger"></a>

## <a name="find-functions-that-have-openapi-descriptions"></a>Localize funções que têm descrições de OpenAPI

Para obter uma experiência mais rica ao trabalhar com parâmetros de função no Designer de aplicativos lógicos [gerar uma definição de OpenAPI](../azure-functions/functions-openapi-definition.md), anteriormente conhecido como um [arquivo do Swagger](https://swagger.io/), para sua função. Para configurar seu aplicativo de função, para que seu aplicativo lógico possa encontrar e usar funções com descrições de Swagger, siga estas etapas:

1. Certifique-se de que seu aplicativo de funções ativamente em execução.

1. No aplicativo de função, configure [Cross-Origin Resource Sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) para que todas as origens são permitidas, seguindo estas etapas:

   1. Dos **aplicativos de funções** , selecione seu aplicativo de funções. No painel direito, selecione **recursos da plataforma** > **CORS**.

      ![Selecione seu aplicativo de função> "Recursos da plataforma"> "CORS"](./media/logic-apps-azure-functions/function-platform-features-cors.png)

   1. Sob **CORS**, adicione o asterisco ( **`*`** ) curinga de caractere, mas remover todas as outras origens na lista e escolha **salvar**.

      ![Defina "CORS* como o caractere curinga "*"](./media/logic-apps-azure-functions/function-platform-features-cors-origins.png)

## <a name="access-property-values-inside-http-requests"></a>Valores de propriedade de acesso dentro de solicitações HTTP

As funções do Webhook podem aceitar solicitações HTTP como entradas e passar essas solicitações para outras funções. Por exemplo, embora o Logic Apps possua [funções que convertem valores de DateTime](../logic-apps/workflow-definition-language-functions-reference.md), essa função JavaScript de amostra básica mostra como você pode acessar uma propriedade dentro de um objeto de solicitação transmitido à função e executar operações nesse valor de propriedade. Para acessar propriedades dentro de objetos, este exemplo usa o [operador ponto (.)](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/Property_accessors):

```javascript
function convertToDateString(request, response){
   var data = request.body;
   response = {
      body: data.date.ToDateString();
   }
}
```

Aqui está o que acontece dentro dessa função:

1. A função cria uma variável `data` e atribui o objeto `body` dentro do objeto `request` a essa variável. A função usa o operador ponto (.) para fazer referência a `body` dentro do objeto a `request` objeto:

   ```javascript
   var data = request.body;
   ```

1. A função agora pode acessar a propriedade `date` por meio da variável `data` e converter esse valor de propriedade do tipo DateTime para o tipo DateString chamando a função `ToDateString()`. A função também retorna o resultado através da propriedade `body` na resposta da função:

   ```javascript
   body: data.date.ToDateString();
   ```

Agora que você criou sua função do Azure, siga as etapas de como [ adicionar funções a aplicativos lógicos ](#add-function-logic-app).

<a name="create-function-designer"></a>

## <a name="create-functions-inside-logic-apps"></a>Criar funções dentro de aplicativos lógicos

Antes de criar uma função do Azure a partir de dentro de seu aplicativo lógico usando o Designer do aplicativo lógico, você deve primeiramente ter um aplicativo de funções do Azure, que é um contêiner para suas funções. Se você não tiver um aplicativo de funções, crie primeiro o aplicativo de função. Veja [Crie sua primeira função no portal do Azure](../azure-functions/functions-create-first-azure-function.md).

1. No [portal do Azure](https://portal.azure.com), abra o aplicativo lógico no Designer do aplicativo lógico.

1. Para criar e adicionar sua função, siga a etapa que se aplica ao seu cenário:

   * Na última etapa no fluxo de trabalho do aplicativo lógico, escolha **Nova etapa**.

   * Entre as etapas existentes no fluxo de trabalho do aplicativo lógico, mova o mouse sobre a seta, escolha o sinal de adição (+) e, em seguida, selecione **Adicionar uma ação**.

1. Na caixa de pesquisa, insira "funções azure" como seu filtro. Na lista de ações, selecione esta ação: **Escolher uma função do Azure – Azure Functions**

   ![Localizar "Funções do Azure"](./media/logic-apps-azure-functions/find-azure-functions-action.png)

1. Na lista de aplicativos de função, selecione seu aplicativo de função. Depois de abre a lista de ações, selecione esta ação: **Funções do Azure – criar nova função**

   ![Selecione seu aplicativo de funções](./media/logic-apps-azure-functions/select-function-app-create-function.png)

1. No editor de definição de função, defina sua função:

   1. Na caixa **Nome da função**, forneça um nome para sua função.

   1. No **código** caixa, adicione seu código para o modelo de função, incluindo a resposta e o conteúdo que você deseja retornado ao seu aplicativo lógico após a conclusão de sua função em execução.

      ![Definir sua função](./media/logic-apps-azure-functions/function-definition.png)

      No código do modelo, o *`context` objeto* refere-se à mensagem que seu aplicativo lógico envia por meio do campo **Corpo da solicitação** em uma etapa posterior. Para acessar as propriedades do objeto `context` de dentro de sua função, use a seguinte sintaxe:

      `context.body.<property-name>`

      Por exemplo, para fazer referência à propriedade `content` dentro do objeto `context`, use a seguinte sintaxe: 

      `context.body.content`

      O código de modelo também inclui uma variável `input`, que armazena o valor do parâmetro `data` para que sua função possa executar operações nesse valor. Dentro de funções de JavaScript, a variável `data` também é um atalho para `context.body`.

      > [!NOTE]
      > A propriedade `body` aqui se aplica ao objeto `context` e não é igual ao token **Corpo** a partir de uma saída da ação, que você também pode passar para sua função.

   1. Quando terminar, escolha **Criar**.

1. Na caixa **Corpo da Solicitação**, forneça sua entrada de função, que deve ser formatada como um objeto JavaScript Object Notation (JSON).

   Essa entrada é o *objeto de contexto* ou a mensagem que seu aplicativo lógico envia para sua função. Quando você clica no campo **Corpo da Solicitação**, a lista de conteúdo dinâmico é exibida para que você possa selecionar tokens de saídas de etapas anteriores. Este exemplo especifica que o conteúdo do contexto contém uma propriedade chamada `content` que tem o valor do token **De** do gatilho de email:

   ![Exemplo de "Corpo da solicitação" – o conteúdo do objeto de contexto](./media/logic-apps-azure-functions/function-request-body-example.png)

   Aqui, o objeto de contexto não é convertido como uma cadeia de caracteres, portanto, o conteúdo do objeto é adicionado diretamente ao conteúdo do JSON. No entanto, quando o objeto de contexto não é um token JSON que passa uma cadeia de caracteres, um objeto JSON ou uma matriz JSON, é exibido um erro. Portanto, se este exemplo tiver usado o token **Hora de Recebimento**, você poderá converter o objeto de contexto como uma cadeia de caracteres adicionando marcas de aspas duplas:  

   ![Converter um objeto como cadeia de caracteres](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

1. Para especificar outros detalhes como o método de uso, os cabeçalhos de solicitação ou parâmetros de consulta, abra o **adicionar novo parâmetro** lista e selecione as opções que você deseja.

<a name="add-function-logic-app"></a>

## <a name="add-existing-functions-to-logic-apps"></a>Adicionar funções existentes para aplicativos lógicos

Para chamar funções existentes do Azure de seus aplicativos lógicos, você pode adicionar funções do Azure como qualquer outra ação no Logic App Designer.

1. No [portal do Azure](https://portal.azure.com), abra o aplicativo lógico no Designer do aplicativo lógico.

1. Na etapa onde você deseja adicionar a função, escolha **nova etapa**e selecione **adicionar uma ação**.

1. Na caixa de pesquisa, insira "funções azure" como seu filtro. Na lista de ações, selecione esta ação: **Escolher uma função do Azure – Azure Functions**

   ![Localizar "Funções do Azure"](./media/logic-apps-azure-functions/find-azure-functions-action.png)

1. Na lista de aplicativos de função, selecione seu aplicativo de função. Depois que a lista de funções aparecer, selecione sua função.

   ![Selecione seu aplicativo de função e a função do Azure](./media/logic-apps-azure-functions/select-function-app-existing-function.png)

   Para funções que têm definições de API (descrições de Swagger) e que são [configuradas para que seu aplicativo lógico possa encontrar e acessar essas funções](#function-swagger), é possível selecionar **Ações de Swagger**:

   ![Selecione seu aplicativo de função, "Ações de Swagger" "e sua função do Azure](./media/logic-apps-azure-functions/select-function-app-existing-function-swagger.png)

1. Na caixa **Corpo da Solicitação**, forneça sua entrada de função, que deve ser formatada como um objeto JavaScript Object Notation (JSON).

   Essa entrada é o *objeto de contexto* ou a mensagem que seu aplicativo lógico envia para sua função. Quando você clica na **corpo da solicitação** campo, a lista de conteúdo dinâmico é exibido para que você possa selecionar tokens de saídas de etapas anteriores. Este exemplo especifica que o conteúdo do contexto contém uma propriedade chamada `content` que tem o valor do token **De** do gatilho de email:

   ![Exemplo de "Corpo da solicitação" – o conteúdo do objeto de contexto](./media/logic-apps-azure-functions/function-request-body-example.png)

   Aqui, o objeto de contexto não é convertido como uma cadeia de caracteres, portanto, o conteúdo do objeto é adicionado diretamente ao conteúdo do JSON. No entanto, quando o objeto de contexto não é um token JSON que passa uma cadeia de caracteres, um objeto JSON ou uma matriz JSON, é exibido um erro. Portanto, se este exemplo tiver usado o token **Hora de Recebimento**, você poderá converter o objeto de contexto como uma cadeia de caracteres adicionando marcas de aspas duplas:

   ![Converter um objeto como cadeia de caracteres](./media/logic-apps-azure-functions/function-request-body-string-cast-example.png)

1. Para especificar outros detalhes como o método de uso, os cabeçalhos de solicitação ou parâmetros de consulta, abra o **adicionar novo parâmetro** lista e selecione as opções que você deseja.

<a name="call-logic-app"></a>

## <a name="call-logic-apps-from-azure-functions"></a>Chamar aplicativos lógicos do Azure functions

Quando você quiser acionar um aplicativo lógico de dentro de uma função do Azure, esse aplicativo lógico deverá ser iniciado com um gatilho que fornece um ponto de extremidade que pode ser chamado. Por exemplo, você pode iniciar o aplicativo lógico com o acionador **HTTP**, **Solicitação**, **Filas do Azure** ou **Grade de Eventos**. Dentro de sua função, envie uma solicitação HTTP POST para a URL do gatilho e inclua o conteúdo que você deseja que esse aplicativo lógico processe. Para obter mais informações, consulte [Chamar, acionar ou aninhar aplicativos lógicos](../logic-apps/logic-apps-http-endpoint.md).

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre [Conectores de Aplicativos Lógicos](../connectors/apis-list.md)
