---
title: Metadados com a API de GenerateAnswer – QnA Maker
titleSuffix: Azure Cognitive Services
description: O QnA Maker permite adicionar metadados na forma de pares chave-valor aos conjuntos de perguntas/respostas. Você pode filtrar os resultados para consultas de usuário e armazenar informações adicionais que podem ser usadas nas conversas de acompanhamento.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/17/2019
ms.author: tulasim
ms.openlocfilehash: d1e7a29e4ca94405e2d6b2000309ef6e2c3a777c
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164602"
---
# <a name="get-an-answer-with-the-generateanswer-api-and-metadata"></a>Obter uma resposta com a API GenerateAnswer e metadados

Para obter a resposta prevista à pergunta de um usuário, use a API GenerateAnswer. Quando você publica uma base de Conhecimento, você pode ver informações sobre como usar essa API na **publicar** página. Você também pode configurar a API para filtrar as respostas com base em marcas de metadados e testar a base de conhecimento do ponto de extremidade com o parâmetro de cadeia de caracteres de consulta de teste.

O QnA Maker permite adicionar metadados, na forma de pares chave-valor, em seus conjuntos de perguntas e respostas. Em seguida, você pode usar essas informações para filtrar resultados para consultas de usuário e para armazenar informações adicionais que podem ser usadas nas conversas de acompanhamento. Para mais informações, consulte a [Base de dados de conhecimento](../Concepts/knowledge-base.md).

<a name="qna-entity"></a>

## <a name="store-questions-and-answers-with-a-qna-entity"></a>Perguntas e respostas com uma entidade de QnA de Store

É importante entender como o QnA Maker armazena os dados de pergunta e resposta. A ilustração a seguir mostra uma entidade QnA:

![Ilustração de uma entidade do QnA](../media/qnamaker-how-to-metadata-usage/qna-entity.png)

Cada entidade QnA tem uma ID exclusiva e persistente. Você pode usar a ID para fazer atualizações em uma determinada entidade QnA.

<a name="generateanswer-api"></a>

## <a name="get-answer-predictions-with-the-generateanswer-api"></a>Obter previsões de resposta com a API GenerateAnswer

Você usa a API GenerateAnswer em seu aplicativo ou bot para consultar sua base de dados de conhecimento com uma pergunta do usuário obter a melhor correspondência da pergunta e resposta define.

<a name="generateanswer-endpoint"></a>

## <a name="publish-to-get-generateanswer-endpoint"></a>Publicar para obter o ponto de extremidade GenerateAnswer 

Depois de publicar sua base de dados de Conhecimento, a partir de [portal QnA Maker](https://www.qnamaker.ai), ou usando o [API](https://go.microsoft.com/fwlink/?linkid=2092179), você pode obter os detalhes do ponto de extremidade GenerateAnswer.

Para obter os detalhes do ponto de extremidade:
1. Entre em [https://www.qnamaker.ai](https://www.qnamaker.ai).
1. Na **meu bases de Conhecimento**, selecione **Exibir código** para sua base de Conhecimento.
    ![Captura de tela de Meus bases de Conhecimento](../media/qnamaker-how-to-metadata-usage/my-knowledge-bases.png)
1. Obtenha os detalhes do ponto de extremidade do GenerateAnswer.

    ![Captura de tela de detalhes do ponto de extremidade](../media/qnamaker-how-to-metadata-usage/view-code.png)

Também é possível obter os detalhes de ponto de extremidade na guia **Configurações** da base de dados de conhecimento.

<a name="generateanswer-request"></a>

## <a name="generateanswer-request-configuration"></a>Configuração de solicitação GenerateAnswer

Você chama o GenerateAnswer com uma solicitação HTTP POST. Para código de exemplo que mostra como chamar o GenerateAnswer, consulte os [inícios rápidos](../quickstarts/csharp.md).

O **URL de solicitação** tem o seguinte formato: 

```
https://{QnA-Maker-endpoint}/knowledgebases/{knowledge-base-ID}/generateAnswer
```

|Propriedade de solicitação HTTP|NOME|Type|Finalidade|
|--|--|--|--|
|Parâmetro de rota de URL|ID da base de dados de Conhecimento|cadeia de caracteres|o GUID da base de dados de conhecimento.|
|Parâmetro de rota de URL|Host do ponto de extremidade QnAMaker|cadeia de caracteres|o nome do host do ponto de extremidade implantado na sua assinatura do Azure. Isso está disponível na **configurações** página depois de publicar a base de Conhecimento. |
|Cabeçalho|Tipo de conteúdo|cadeia de caracteres|o tipo de mídia do corpo enviado para a API. Valor padrão é: '|
|Cabeçalho|Autorização|cadeia de caracteres|sua chave de ponto de extremidade (EndpointKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx).|
|Corpo do POST|Objeto JSON|JSON|A pergunta com as configurações.|


O corpo JSON tem várias configurações:

|Propriedade de corpo JSON|Obrigatório|Type|Finalidade|
|--|--|--|--|
|`question`|obrigatório|cadeia de caracteres|Uma pergunta do usuário a serem enviados para sua base de dados de Conhecimento.|
|`top`|opcional|inteiro|o número de resultados classificados para incluir na saída. O valor padrão é 1.|
|`userId`|opcional|cadeia de caracteres|ID exclusiva para identificar o usuário. Essa ID será registrada nos logs de chat.|
|`scoreThreshold`|opcional|inteiro|Somente respostas com pontuação de confiança acima desse limite serão retornadas. O valor padrão é 0.|
|`isTest`|opcional|Boolean|Se definido como true, retorna os resultados dos `testkb` índice de pesquisa em vez de índice publicado.|
|`strictFilters`|opcional|cadeia de caracteres|se especificado, informa ao QnA Maker para retornar apenas as respostas com os metadados especificados. Use `none` para indicar a resposta não deve ter nenhum filtro de metadados. |
|`RankerType`|opcional|cadeia de caracteres|Se for especificado como `QuestionOnly`, informa ao QnA Maker para pesquisar somente perguntas. Se não for especificado, o QnA Maker pesquisa perguntas e respostas.

Um exemplo de corpo JSON é semelhante a:

```json
{
    "question": "qna maker and luis",
    "top": 6,
    "isTest": true,
    "scoreThreshold": 20,
    "strictFilters": [
    {
        "name": "category",
        "value": "api"
    }],
    "userId": "sd53lsY="
}
```

<a name="generateanswer-response"></a>

## <a name="generateanswer-response-properties"></a>Propriedades da resposta GenerateAnswer

Uma resposta bem-sucedida retorna um status de 200 e uma resposta JSON. 

|Propriedade de respostas (classificada pela pontuação)|Finalidade|
|--|--|
|para seu app&#39;s|uma pontuação de classificação entre 0 e 100.|
|ID|a ID exclusiva atribuída à resposta.|
|Perguntas|as perguntas fornecidas pelo usuário.|
|resposta|a resposta à pergunta.|
|source|o nome da fonte da qual a resposta foi extraída ou salva na base de dados de conhecimento.|
|metadata|os metadados associados à resposta.|
|metadata.name|nome dos metadados. (cadeia de caracteres, Comprimento máx.: 100, obrigatório)|
|metadata.value|valor dos metadados. (cadeia de caracteres, Comprimento máx.: 100, obrigatório)|


```json
{
    "answers": [
        {
            "score": 28.54820341616869,
            "Id": 20,
            "answer": "There is no direct integration of LUIS with QnA Maker. But, in your bot code, you can use LUIS and QnA Maker together. [View a sample bot](https://github.com/Microsoft/BotBuilder-CognitiveServices/tree/master/Node/samples/QnAMaker/QnAWithLUIS)",
            "source": "Custom Editorial",
            "questions": [
                "How can I integrate LUIS with QnA Maker?"
            ],
            "metadata": [
                {
                    "name": "category",
                    "value": "api"
                }
            ]
        }
    ]
}
```

## <a name="use-qna-maker-with-a-bot-in-c"></a>Use o QnA Maker com um bot emC#

A estrutura de bot fornece acesso às propriedades do QnA Maker:

```csharp
using Microsoft.Bot.Builder.AI.QnA;
var metadata = new Microsoft.Bot.Builder.AI.QnA.Metadata();
var qnaOptions = new QnAMakerOptions();

qnaOptions.Top = Constants.DefaultTop;
qnaOptions.ScoreThreshold = 0.3F;
var response = await _services.QnAServices[QnAMakerKey].GetAnswersAsync(turnContext, qnaOptions);
```

O bot de suporte tenha [um exemplo](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-support/csharp_dotnetcore/Service/SupportBotService.cs#L418) com esse código.

## <a name="use-qna-maker-with-a-bot-in-nodejs"></a>Use o QnA Maker com um bot no Node. js

A estrutura de bot fornece acesso às propriedades do QnA Maker:

```javascript
const { QnAMaker } = require('botbuilder-ai');
this.qnaMaker = new QnAMaker(endpoint);

// Default QnAMakerOptions
var qnaMakerOptions = {
    ScoreThreshold: 0.03,
    Top: 3
};
var qnaResults = await this.qnaMaker.getAnswers(stepContext.context, qnaMakerOptions);
```

O bot de suporte tenha [um exemplo](https://github.com/microsoft/BotBuilder-Samples/blob/master/experimental/qnamaker-activelearning/javascript_nodejs/Helpers/dialogHelper.js#L36) com esse código.

<a name="metadata-example"></a>

## <a name="use-metadata-to-filter-answers-by-custom-metadata-tags"></a>Usar metadados para respostas de filtro por marcas de metadados personalizados

Adicionar metadados permite que você filtre as respostas por essas marcas de metadados. Adicionar a coluna de metadados do **opções de exibição** menu. Adicionar metadados a sua base de dados de Conhecimento, selecionando os metadados **+** ícone para adicionar um par de metadados. Esse par consiste em uma chave e um valor.

![Captura de tela da adição de metadados](../media/qnamaker-how-to-metadata-usage/add-metadata.png)

<a name="filter-results-with-strictfilters-for-metadata-tags"></a>

## <a name="filter-results-with-strictfilters-for-metadata-tags"></a>Filtrar resultados com strictFilters para marcas de metadados

Considere a pergunta do usuário "Quando faz fechar este hotel?", em que a intenção é implícita para o restaurante "Paraíso".

Como os resultados são necessários somente para o restaurante "Paraíso", você pode definir um filtro na chamada GenerateAnswer nos metadados de "Nome do restaurante". O exemplo a seguir mostra isso:

```json
{
    "question": "When does this hotel close?",
    "top": 1,
    "strictFilters": [
      {
        "name": "restaurant",
        "value": "paradise"
      }]
}
```

<a name="keep-context"></a>

## <a name="use-question-and-answer-results-to-keep-conversation-context"></a>Usar resultados de pergunta e resposta para manter o contexto de conversa

A resposta para o GenerateAnswer contém as informações de metadados correspondente do conjunto de perguntas e respostas correspondente. Você pode usar essas informações em seu aplicativo cliente para armazenar o contexto da conversa anterior para uso em conversas posteriores. 

```json
{
    "answers": [
        {
            "questions": [
                "What is the closing time?"
            ],
            "answer": "10.30 PM",
            "score": 100,
            "id": 1,
            "source": "Editorial",
            "metadata": [
                {
                    "name": "restaurant",
                    "value": "paradise"
                },
                {
                    "name": "location",
                    "value": "secunderabad"
                }
            ]
        }
    ]
}
```

## <a name="match-questions-only-by-text"></a>Corresponder a apenas, perguntas por texto

Por padrão, o QnA Maker pesquisa por meio de perguntas e respostas. Se você quiser pesquisar apenas perguntas, para gerar uma resposta, use o `RankerType=QuestionOnly` no corpo POST da solicitação GenerateAnswer.

Você pode pesquisar por meio de Conhecimento publicados, usando `isTest=false`, ou no teste kb usando `isTest=true`.

```json
{
  "question": "Hi",
  "top": 30,
  "isTest": true,
  "RankerType":"QuestionOnly"
}
```

## <a name="next-steps"></a>Próximas etapas

O **Publish** página também fornece informações para gerar uma resposta com [Postman](../Quickstarts/get-answer-from-kb-using-postman.md) e [cURL](../Quickstarts/get-answer-from-kb-using-curl.md). 

> [!div class="nextstepaction"]
> [Criar uma base de dados de conhecimento](./create-knowledge-base.md)
