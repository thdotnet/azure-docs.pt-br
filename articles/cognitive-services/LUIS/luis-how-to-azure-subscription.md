---
title: Chaves de assinatura
titleSuffix: Language Understanding - Azure Cognitive Services
description: Você não precisa criar chaves de assinatura para usar suas primeiras 1.000 consultas de ponto de extremidade gratuitas. Se receber um erro _fora da cota_ na forma de um erro HTTP 403 ou 429, você precisará criar uma chave e atribuí-la a seu aplicativo.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 06/18/2019
ms.author: diberry
ms.openlocfilehash: 7f82bf5a40df0554d4f98b2d835fcbd69279be43
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204151"
---
# <a name="using-subscription-keys-with-your-luis-app"></a>Usando chaves de assinatura com seu aplicativo LUIS

Quando você usa primeiro o Luis (reconhecimento vocal a linguagem), você não precisará criar chaves de assinatura. Para começar, você tem consultas de ponto de extremidade de 1000. 

Para testar e protótipo apenas, use a camada gratuita (F0). Para sistemas de produção, use uma camada [paga](https://aka.ms/luis-price-tier). Não use a [chave de criação](luis-concept-keys.md#authoring-key) para consultas de ponto de extremidade em produção.


<a name="create-luis-service"></a>
<a name="create-language-understanding-endpoint-key-in-the-azure-portal"/>

## <a name="create-prediction-endpoint-runtime-resource-in-the-azure-portal"></a>Criar o recurso de tempo de execução do ponto de extremidade de previsão no portal do Azure

Você cria o [recurso de ponto de extremidade de previsão](get-started-portal-deploy-app.md#create-the-endpoint-resource) no portal do Azure. Esse recurso deve ser usado somente para consultas de previsão de ponto de extremidade. Não use esse recurso para criação de alterações para o aplicativo.

<a name="programmatic-key" ></a>
<a name="authoring-key" ></a>
<a name="endpoint-key" ></a>
<a name="use-endpoint-key-in-query" ></a>
<a name="api-usage-of-ocp-apim-subscription-key" ></a>
<a name="key-limits" ></a>
<a name="key-limit-errors" ></a>
<a name="key-concepts"></a>
<a name="authoring-key"></a>
<a name="create-and-use-an-endpoint-key"></a>
<a name="assign-endpoint-key"></a>
<a name="assign-resource"></a>


## <a name="assign-resource-key-to-luis-app-in-luis-portal"></a>Atribuir chave de recurso ao aplicativo do LUIS no Portal do LUIS

Sempre que você cria um novo recurso para LUIS, você precisará [atribuir o recurso para o aplicativo LUIS](get-started-portal-deploy-app.md#assign-the-resource-key-to-the-luis-app-in-the-luis-portal). Após a atribuição, você não precisará realizar esta etapa novamente, a menos que crie um novo recurso. Você pode criar um novo recurso para expandir as regiões do seu aplicativo ou para dar suporte a um número maior de consultas de previsão.

<!-- content moved to luis-reference-regions.md, need replacement links-->
<a name="regions-and-keys"></a>
<a name="publishing-to-europe"></a>
<a name="publishing-to-australia"></a>

### <a name="unassign-resource"></a>Cancelar a atribuição do recurso
Quando você cancela a atribuição da chave do ponto de extremidade, ela não é excluída do Azure. Ela é apenas desvinculada do LUIS. 

Quando uma chave de ponto de extremidade tem a atribuição cancelada ou não é atribuída ao aplicativo, qualquer solicitação para a URL do ponto de extremidade retorna um erro: `401 This application cannot be accessed with the current subscription`. 

### <a name="include-all-predicted-intent-scores"></a>Incluir todas as pontuações de intenção previstas
A caixa de seleção **Incluir todas as pontuações de intenção previstas** permite que a resposta da consulta do ponto de extremidade inclua a pontuação de previsão para cada intenção. 

Essa configuração permite que seu aplicativo que chama o LUIS ou chatbot tome uma decisão programática com base nas pontuações das intenções retornadas. Geralmente, as duas principais intenções são as mais interessantes. Se a pontuação superior for a intenção None, seu chatbot poderá optar por fazer uma pergunta de acompanhamento que realize uma escolha definitiva entre a intenção None e a outra intenção de pontuação alta. 

As intenções e suas classificações também estão incluídas os logs de ponto de extremidade. Você pode [exportar](luis-how-to-start-new-app.md#export-app) esses logs e analisar as pontuações. 

```JSON
{
  "query": "book a flight to Cairo",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.5223427
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.5223427
    },
    {
      "intent": "BookFlight",
      "score": 0.372391433
    }
  ],
  "entities": []
}
```

### <a name="enable-bing-spell-checker"></a>Habilitar o verificador ortográfico do Bing 
Nas **Configurações da URL de ponto de extremidade**, o botão de alternância **Verificador ortográfico do Bing** permite ao LUIS corrigir a ortografia de palavras antes da previsão. Crie uma **[chave de Verificação Ortográfica do Bing](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api)** . 

Adicione o parâmetro de cadeia de caracteres de consulta **spellCheck=true** e **bing-spell-check-subscription-key={YOUR_BING_KEY_HERE}** . Substitua o `{YOUR_BING_KEY_HERE}` pela sua chave do verificador ortográfico do Bing.

```JSON
{
  "query": "Book a flite to London?",
  "alteredQuery": "Book a flight to London?",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 0.780123
  },
  "entities": []
}
```

### <a name="publishing-regions"></a>Regiões de publicação

Saiba mais sobre [regiões](luis-reference-regions.md) de publicação, incluindo a publicação na [Europa](luis-reference-regions.md#publishing-to-europe) e na [Austrália](luis-reference-regions.md#publishing-to-australia). Regiões de publicação são diferentes de regiões de criação. Crie um aplicativo na região de criação correspondente à região de publicação desejada para o ponto de extremidade.

## <a name="assign-resource-without-luis-portal"></a>Atribuir recursos sem o portal do LUIS

Para fins de automação, como um pipeline de CI/CD, você talvez queira automatizar a atribuição de um recurso do LUIS para um aplicativo LUIS. Para fazer isso, é necessário executar as seguintes etapas:

1. Obter um token do Azure Resource Manager deste [site](https://resources.azure.com/api/token?plaintext=true). Esse token expira, então use-o imediatamente. A solicitação retorna um token do Azure Resource Manager.

    ![Solicite e receba o token do Azure Resource Manager](./media/luis-manage-keys/get-arm-token.png)

1. Use o token para solicitar os recursos do LUIS entre assinaturas, da [API Obter contas do Azure do LUIS](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be313cec181ae720aa2b26c), que sua conta de usuário tem acesso. 

    Essa API POST requer as seguintes configurações:

    |Cabeçalho|Value|
    |--|--|
    |`Authorization`|O valor de `Authorization` é `Bearer {token}`. Observe que o valor do token deve ser precedido pela palavra `Bearer` e um espaço.| 
    |`Ocp-Apim-Subscription-Key`|Sua [chave de criação](luis-how-to-account-settings.md).|

    Essa API retorna uma matriz de objetos JSON das suas assinaturas do LUIS, incluindo a ID da assinatura, o grupo de recursos e o nome do recurso, retornado como o nome da conta. Localize um item na matriz que é o recurso do LUIS a atribuir ao aplicativo LUIS. 

1. Atribua o token para o recurso do LUIS com a API [Atribuir contas do Azure do LUIS a um aplicativo](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5be32228e8473de116325515). 

    Essa API POST requer as seguintes configurações:

    |Type|Configuração|Value|
    |--|--|--|
    |Cabeçalho|`Authorization`|O valor de `Authorization` é `Bearer {token}`. Observe que o valor do token deve ser precedido pela palavra `Bearer` e um espaço.|
    |Cabeçalho|`Ocp-Apim-Subscription-Key`|Sua [chave de criação](luis-how-to-account-settings.md).|
    |Cabeçalho|`Content-type`|`application/json`|
    |Querystring|`appid`|A ID do aplicativo de LUIS. 
    |Corpo||{"AzureSubscriptionId":"ddda2925-af7f-4b05-9ba1-2155c5fe8a8e",<br>"ResourceGroup": "resourcegroup-2",<br>"AccountName": "luis-uswest-S0-2"}|

    Quando essa API for bem-sucedida, ela retornará um status 201 – criado. 

## <a name="change-pricing-tier"></a>Alterar tipo de preço

1.  No [Azure](https://portal.azure.com), localize sua assinatura do LUIS. Selecione a assinatura do LUIS.
    ![Localize sua assinatura do LUIS](./media/luis-usage-tiers/find.png)
1.  Selecione **Tipo de preço** para ver os tipos de preços disponíveis. 
    ![Exibir os tipos de preço](./media/luis-usage-tiers/subscription.png)
1.  Selecione o tipo de preço e clique em **Selecionar** para salvar a alteração. 
    ![Altere sua camada de pagamento do LUIS](./media/luis-usage-tiers/plans.png)
1.  Quando a alteração de preços é concluída, uma janela pop-up verifica a nova camada de preços. 
    ![Verifique sua camada de pagamento do LUIS](./media/luis-usage-tiers/updated.png)
1. Lembre-se de [atribuir essa chave do ponto de extremidade](#assign-endpoint-key) na página **Publicar** e usá-la em todas as consultas de ponto de extremidade. 

## <a name="fix-http-status-code-403-and-429"></a>Corrigir o código de status HTTP 403 e 429

Você receberá o erro 403 e 429 códigos de status quando exceder as transações por segundo ou transações por mês para o tipo de preço.

### <a name="when-you-receive-an-http-403-error-status-code"></a>Quando você receber um código de status de erro HTTP 403

Quando você usa todas essas livre 1000 ponto de extremidade consultas ou você exceder a cota mensal de transações do tipo de preço, você receberá um código de status de erro HTTP 403. 

Para corrigir esse erro, será necessário [alterar o tipo de preço](luis-how-to-azure-subscription.md#change-pricing-tier) para uma camada superior ou [criar um novo recurso](get-started-portal-deploy-app.md#create-the-endpoint-resource) e [atribuí-lo ao seu aplicativo](get-started-portal-deploy-app.md#assign-the-resource-key-to-the-luis-app-in-the-luis-portal).

As soluções para esse erro incluem:

* No [portal do Azure](https://portal.azure.com), em seu idioma Noções básicas sobre o recurso, no **gerenciamento de recursos -> tipo de preço**, alterar seu tipo de preço para uma camada TPS mais alta. Você não precisa fazer nada no portal do reconhecimento vocal se seu recurso já está atribuído ao seu aplicativo de reconhecimento vocal.
*  Se o seu uso excede o tipo de preço mais alto, adicione mais recursos de reconhecimento vocal com um balanceador de carga na frente delas. O [contêiner de reconhecimento vocal](luis-container-howto.md) com Kubernetes ou o Docker Compose pode ajudar com isso.

### <a name="when-you-receive-an-http-429-error-status-code"></a>Quando você receber um código de status de erro HTTP 429

Esse código de status é retornado quando as transações por segundo excede seu tipo de preço.  

As soluções incluem:

* Você pode [aumentar o tipo de preço](#change-pricing-tier), se você não estiver no nível mais elevado.
* Se o seu uso excede o tipo de preço mais alto, adicione mais recursos de reconhecimento vocal com um balanceador de carga na frente delas. O [contêiner de reconhecimento vocal](luis-container-howto.md) com Kubernetes ou o Docker Compose pode ajudar com isso.
* Você pode portão solicitações do aplicativo cliente com um [política de repetição](https://docs.microsoft.com/azure/architecture/best-practices/transient-faults#general-guidelines) implementar por conta própria quando você receber esse código de status. 

## <a name="viewing-summary-usage"></a>Exibindo uso de resumo
Você pode exibir informações de uso do LUIS no Azure. A página **Visão geral** mostra as informações de resumidas recentes, incluindo chamadas e erros. Se você fizer uma solicitação de ponto de extremidade do LUIS, acompanhe na **página Visão geral** e aguarde até cinco minutos para o uso ser exibido.

![Exibindo uso de resumo](./media/luis-usage-tiers/overview.png)

## <a name="customizing-usage-charts"></a>Como personalizar gráficos de uso
As métricas fornecem uma exibição mais detalhada dos dados.

![Métricas padrão](./media/luis-usage-tiers/metrics-default.png)

Você pode configurar os gráficos de métricas para o período de tempo e o tipo de métrica. 

![Métricas personalizadas](./media/luis-usage-tiers/metrics-custom.png)

## <a name="total-transactions-threshold-alert"></a>Alerta de limite total de transações
Se você quiser saber quando atingiu um determinado limite transação, por exemplo, 10.000 transações, crie um alerta. 

![Alertas padrão](./media/luis-usage-tiers/alert-default.png)

Adicionar um alerta de métrica para a métrica **total de chamadas** para um determinado período de tempo. Adicione endereços de email de todas as pessoas que devem receber o alerta. Adicione webhooks para todos os sistemas que devem receber o alerta. Você também poderá executar um aplicativo lógico quando o alerta for disparado. 

## <a name="next-steps"></a>Próximas etapas

Saiba como usar [versões](luis-how-to-manage-versions.md) para gerenciar as alterações ao seu aplicativo do LUIS.
