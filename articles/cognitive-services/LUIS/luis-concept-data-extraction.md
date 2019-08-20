---
title: Extração de dados-LUIS
titleSuffix: Azure Cognitive Services
description: Extraia dados de texto expressão com intenções e entidades. Saiba que tipo de dados pode ser extraído de Reconhecimento vocal (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: 055cd25f534de5d3cc3ccbe44df88e7111e101a3
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560752"
---
# <a name="extract-data-from-utterance-text-with-intents-and-entities"></a>Extrair dados de texto expressão com intenções e entidades
O LUIS oferece a capacidade de obter informações de declarações de idioma natural de um usuário. As informações são extraídas de forma que possam ser usadas por um programa, aplicativo ou chat bot para executar uma ação. Nas seções a seguir, saiba quais dados são retornados de intenções e entidades com exemplos de JSON.

Os dados mais difíceis de extrair são dados de aprendizado de máquina, porque eles não são uma correspondência exata do texto. A extração de dados das [entidades](luis-concept-entity-types.md) de aprendizado de máquina precisa fazer parte do [ciclo de criação](luis-concept-app-iteration.md) até que você esteja confiante de receber os dados esperados.

## <a name="data-location-and-key-usage"></a>Local dos dados e uso da chave
O LUIS fornece os dados do [ponto de extremidade](luis-glossary.md#endpoint) publicado. A **solicitação HTTPS** (POST ou GET) contém a declaração, assim como algumas configurações opcionais, como ambientes de preparo ou de produção.

`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<subscription-key>&verbose=true&timezoneOffset=0&q=book 2 tickets to paris`

O `appID` estará disponível na página **Configurações** do aplicativo LUIS, assim como parte da URL (após `/apps/`) quando você estiver editando esse aplicativo do LUIS. A `subscription-key` é a chave do ponto de extremidade usada para consultar seu aplicativo. Embora seja possível usar a chave início/criação gratuita enquanto estiver treinando o LUIS, é importante alterar a chave de ponto de extremidade para uma chave que dê suporte ao [uso esperado do LUIS](luis-boundaries.md#key-limits). A unidade `timezoneOffset` é de minutos.

A **resposta HTTPS** contém todas as informações de intenção e de entidade que o LUIS pode determinar com base no modelo publicado atual do ponto de extremidade de preparo ou de produção. A URL de ponto de extremidade é encontrada no site [LUIS](luis-reference-regions.md), na seção **Gerenciar**, na página **Chaves e os pontos de extremidade**.

## <a name="data-from-intents"></a>Dados de intenções
Os dados primários são o **nome da intenção** da pontuação mais alta. Usando o `MyStore` [início rápido](luis-quickstart-intents-only.md), a resposta do ponto de extremidade é:

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "entities": []
}
```

|Objeto de dados|Tipo de dados|Local dos dados|Valor|
|--|--|--|--|
|Intenção|Cadeia|topScoringIntent.intent|"GetStoreInfo"|

Se o chatbot ou o aplicativo que chama o LUIS tomar uma decisão com base em mais de uma pontuação de intenção, retorne as pontuações de todas as intenções definindo o parâmetro querystring, `verbose=true`. A resposta do ponto de extremidade é:

```JSON
{
  "query": "when do you open next?",
  "topScoringIntent": {
    "intent": "GetStoreInfo",
    "score": 0.984749258
  },
  "intents": [
    {
      "intent": "GetStoreInfo",
      "score": 0.984749258
    },
    {
      "intent": "None",
      "score": 0.2040639
    }
  ],
  "entities": []
}
```

As intenções são ordenadas da pontuação mais alta para a mais baixa.

|Objeto de dados|Tipo de dados|Local dos dados|Valor|Pontuação|
|--|--|--|--|:--|
|Intenção|Cadeia|intents[0].intent|"GetStoreInfo"|0,984749258|
|Intenção|Cadeia|intents[1].intent|"None"|0,0168218873|

Se você adicionar domínios predefinidos, o nome da intenção indicará o domínio, como `Utilties` ou `Communication`, assim como a intenção:

```JSON
{
  "query": "Turn on the lights next monday at 9am",
  "topScoringIntent": {
    "intent": "Utilities.ShowNext",
    "score": 0.07842206
  },
  "intents": [
    {
      "intent": "Utilities.ShowNext",
      "score": 0.07842206
    },
    {
      "intent": "Communication.StartOver",
      "score": 0.0239675418
    },
    {
      "intent": "None",
      "score": 0.0168218873
    }],
  "entities": []
}
```

|Domínio|Objeto de dados|Tipo de dados|Local dos dados|Valor|
|--|--|--|--|--|
|Utilidades|Intenção|Cadeia|intents[0].intent|"<b>Utilities</b>.ShowNext"|
|Comunicação|Intenção|Cadeia|intents[1].intent|<b>Communication</b>.StartOver"|
||Intenção|Cadeia|intents[2].intent|"None"|


## <a name="data-from-entities"></a>Dados de entidades
A maioria dos chatbots e aplicativos precisam de mais do que o nome da intenção. Esses dados adicionais e opcionais são provenientes de entidades descobertas na declaração. Cada tipo de entidade retorna diferentes informações sobre a correspondência.

Uma única palavra ou frase em uma declaração pode corresponder a mais de uma entidade. Nesse caso, cada entidade de correspondência é retornada com sua pontuação.

Todas as entidades são retornadas na matriz de **entidades** da resposta do ponto de extremidade:

```JSON
"entities": [
  {
    "entity": "bob jones",
    "type": "Name",
    "startIndex": 0,
    "endIndex": 8,
    "score": 0.473899543
  },
  {
    "entity": "3",
    "type": "builtin.number",
    "startIndex": 16,
    "endIndex": 16,
    "resolution": {
      "value": "3"
    }
  }
]
```

## <a name="tokenized-entity-returned"></a>Entidade indexada retornada
Várias [culturas](luis-language-support.md#tokenization) retornam o objeto de entidade com o `entity` valor [indexado](luis-glossary.md#token). Os startIndex e endIndex retornados pelo LUIS no objeto de entidade não mapeiam o valor novo e indexado, mas a consulta original para você extrair a entidade bruta programaticamente. 

Por exemplo, em alemão, a palavra `das Bauernbrot` é indexado em `das bauern brot`. O valor indexado, `das bauern brot`, é retornado e o valor original pode ser determinado programaticamente do startIndex e do endIndex da consulta original, dando a você `das Bauernbrot`.

## <a name="simple-entity-data"></a>Dados de entidade simples

Uma [entidade simples](reference-entity-simple.md) é um valor de aprendizado de máquina. Ele pode ser uma palavra ou frase.

## <a name="composite-entity-data"></a>Dados da entidade composta

Uma [entidade composta](reference-entity-composite.md) é composta por outras entidades, como entidades predefinidas, expressão simples, regular e entidades de lista. As entidades separadas formam uma entidade inteira. 

## <a name="list-entity-data"></a>Dados da entidade Lista

As [entidades de lista](reference-entity-list.md) representam um conjunto fixo e fechado de palavras relacionadas junto com seus sinônimos. O LUIS não descobre valores adicionais para entidades de lista. Use o recurso **Recomendado** para consultar sugestões de novas palavras com base na lista atual. Se houver mais de uma entidade de lista com o mesmo valor, cada entidade será retornada na consulta de ponto de extremidade. 

## <a name="prebuilt-entity-data"></a>Dados de entidade predefinida
Entidades [predefinidas](luis-concept-entity-types.md) são descobertas com base em uma correspondência de expressão regular usando o projeto [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text) de software livre. Entidades predefinidas são retornadas na matriz de entidades e usam o nome do tipo que começa com `builtin::`. O texto a seguir é uma declaração de exemplo com as entidades predefinidas retornadas:

`Dec 5th send to +1 360-555-1212`

```JSON
"entities": [
    {
      "entity": "dec 5th",
      "type": "builtin.datetimeV2.date",
      "startIndex": 0,
      "endIndex": 6,
      "resolution": {
        "values": [
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2017-12-05"
          },
          {
            "timex": "XXXX-12-05",
            "type": "date",
            "value": "2018-12-05"
          }
        ]
      }
    },
    {
      "entity": "1",
      "type": "builtin.number",
      "startIndex": 18,
      "endIndex": 18,
      "resolution": {
        "value": "1"
      }
    },
    {
      "entity": "360",
      "type": "builtin.number",
      "startIndex": 20,
      "endIndex": 22,
      "resolution": {
        "value": "360"
      }
    },
    {
      "entity": "555",
      "type": "builtin.number",
      "startIndex": 26,
      "endIndex": 28,
      "resolution": {
        "value": "555"
      }
    },
    {
      "entity": "1212",
      "type": "builtin.number",
      "startIndex": 32,
      "endIndex": 35,
      "resolution": {
        "value": "1212"
      }
    },
    {
      "entity": "5th",
      "type": "builtin.ordinal",
      "startIndex": 4,
      "endIndex": 6,
      "resolution": {
        "value": "5"
      }
    },
    {
      "entity": "1 360 - 555 - 1212",
      "type": "builtin.phonenumber",
      "startIndex": 18,
      "endIndex": 35,
      "resolution": {
        "value": "1 360 - 555 - 1212"
      }
    }
  ]
```

## <a name="regular-expression-entity-data"></a>Dados de entidade de expressão regular

Uma [entidade de expressão regular](reference-entity-regular-expression.md) extrai uma entidade com base em um padrão de expressão regular fornecido por você.

## <a name="extracting-names"></a>Extraindo nomes
Obter nomes de uma declaração é difícil, porque um nome pode ser quase qualquer combinação de letras e palavras. Dependendo de qual tipo de nome você está extraindo, haverá várias opções. As sugestões a seguir não são regras, mas mais diretrizes.

### <a name="add-prebuilt-personname-and-geographyv2-entities"></a>Adicionar entidades PersonName e GeographyV2 predefinidas

As entidades [PersonName](luis-reference-prebuilt-person.md) e [GeographyV2](luis-reference-prebuilt-geographyV2.md) estão disponíveis em algumas [culturas de linguagem](luis-reference-prebuilt-entities.md). 

### <a name="names-of-people"></a>Nomes de pessoas

Os nomes de pessoas podem ter um formato pequeno dependendo do idioma e da cultura. Use uma entidade **[PersonName](luis-reference-prebuilt-person.md)** predefinida ou uma **[entidade simples](luis-concept-entity-types.md#simple-entity)** com [ funções ](luis-concept-roles.md) de First e Last Name. 

Se você usar a entidade simples, certifique-se de fornecer exemplos que usam o nome e o sobrenome em diferentes partes do expressão, em declarações de comprimentos diferentes e declarações em todas as intenções, incluindo a intenção de nenhum. [Examine](luis-how-to-review-endoint-utt.md) declarações de ponto de extremidade regularmente para rotular nomes que não foram previstos corretamente.

### <a name="names-of-places"></a>Nomes de locais

Os nomes de local são definidos e conhecidos como cidades, municípios, Estados, províncias e países/regiões. Use a entidade predefinida **[geographyV2](luis-reference-prebuilt-geographyv2.md)** para extrair informações de localização.

### <a name="new-and-emerging-names"></a>Nomes novos e emergentes

Alguns aplicativos precisam poder encontrar nomes novos e emergentes, como produtos ou empresas. Esses tipos de nomes são o tipo mais difícil de extração de dados. Comece com uma **[entidade simples](luis-concept-entity-types.md#simple-entity)** e adicione uma [lista de frases](luis-concept-feature.md). [Examine](luis-how-to-review-endoint-utt.md) declarações de ponto de extremidade regularmente para rotular nomes que não foram previstos corretamente.

## <a name="pattern-roles-data"></a>Dados de funções de padrão
Funções são diferenças contextuais de entidades.

```JSON
{
  "query": "move bob jones from seattle to redmond",
  "topScoringIntent": {
    "intent": "MoveAssetsOrPeople",
    "score": 0.9999998
  },
  "intents": [
    {
      "intent": "MoveAssetsOrPeople",
      "score": 0.9999998
    },
    {
      "intent": "None",
      "score": 1.02040713E-06
    },
    {
      "intent": "GetEmployeeBenefits",
      "score": 6.12244548E-07
    },
    {
      "intent": "GetEmployeeOrgChart",
      "score": 6.12244548E-07
    },
    {
      "intent": "FindForm",
      "score": 1.1E-09
    }
  ],
  "entities": [
    {
      "entity": "bob jones",
      "type": "Employee",
      "startIndex": 5,
      "endIndex": 13,
      "score": 0.922820568,
      "role": ""
    },
    {
      "entity": "seattle",
      "type": "Location",
      "startIndex": 20,
      "endIndex": 26,
      "score": 0.948008537,
      "role": "Origin"
    },
    {
      "entity": "redmond",
      "type": "Location",
      "startIndex": 31,
      "endIndex": 37,
      "score": 0.7047979,
      "role": "Destination"
    }
  ]
}
```

## <a name="patternany-entity-data"></a>Dados de entidade pattern.any

[Padrão. any](reference-entity-pattern-any.md) é um espaço reservado de comprimento variável usado somente no modelo de um padrão expressão para marcar onde a entidade começa e termina.  

## <a name="sentiment-analysis"></a>Análise de Sentimento
Se a análise de sentimento estiver configurada, a resposta JSON do LUIS incluirá a análise de sentimento. Saiba mais sobre a análise de sentimento na documentação [Análise de Texto](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).

### <a name="sentiment-data"></a>Dados de sentimento
Dados de sentimento são uma pontuação entre 1 e 0 indicando o sentimento positivo (mais próximo de 1) ou negativo (mais próximo de 0) dos dados.

Quando a cultura for `en-us`, a resposta será:

```JSON
"sentimentAnalysis": {
  "label": "positive",
  "score": 0.9163064
}
```

Para todas as outras culturas, a resposta é:

```JSON
"sentimentAnalysis": {
  "score": 0.9163064
}
```


### <a name="key-phrase-extraction-entity-data"></a>Dados de entidade de extração de frases-chave
A entidade de extração de frases-chave retorna frases-chave na declaração, fornecida pela [Análise de Texto](https://docs.microsoft.com/azure/cognitive-services/text-analytics/).

```JSON
{
  "query": "Is there a map of places with beautiful views on a favorite trail?",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.764368951
  },
  "intents": [
    ...
  ],
  "entities": [
    {
      "entity": "beautiful views",
      "type": "builtin.keyPhrase",
      "startIndex": 30,
      "endIndex": 44
    },
    {
      "entity": "map of places",
      "type": "builtin.keyPhrase",
      "startIndex": 11,
      "endIndex": 23
    },
    {
      "entity": "favorite trail",
      "type": "builtin.keyPhrase",
      "startIndex": 51,
      "endIndex": 64
    }
  ]
}
```

## <a name="data-matching-multiple-entities"></a>Dados que correspondem a várias entidades

O LUIS retorna todas as entidades descobertas na declaração. Como resultado, seu chatbot pode precisar tomar uma decisão com base nos resultados. Uma declaração pode ter muitas entidades em uma declaração:

`book me 2 adult business tickets to paris tomorrow on air france`

O ponto de extremidade LUIS pode descobrir os mesmos dados em diferentes entidades:

```JSON
{
  "query": "book me 2 adult business tickets to paris tomorrow on air france",
  "topScoringIntent": {
    "intent": "BookFlight",
    "score": 1.0
  },
  "intents": [
    {
      "intent": "BookFlight",
      "score": 1.0
    },
    {
      "intent": "Concierge",
      "score": 0.04216196
    },
    {
      "intent": "None",
      "score": 0.03610297
    }
  ],
  "entities": [
    {
      "entity": "air france",
      "type": "Airline",
      "startIndex": 54,
      "endIndex": 63,
      "score": 0.8291798
    },
    {
      "entity": "adult",
      "type": "Category",
      "startIndex": 10,
      "endIndex": 14,
      "resolution": {
        "values": [
          "adult"
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Cities",
      "startIndex": 36,
      "endIndex": 40,
      "resolution": {
        "values": [
          "Paris"
        ]
      }
    },
    {
      "entity": "tomorrow",
      "type": "builtin.datetimeV2.date",
      "startIndex": 42,
      "endIndex": 49,
      "resolution": {
        "values": [
          {
            "timex": "2018-02-21",
            "type": "date",
            "value": "2018-02-21"
          }
        ]
      }
    },
    {
      "entity": "paris",
      "type": "Location::ToLocation",
      "startIndex": 36,
      "endIndex": 40,
      "score": 0.9730773
    },
    {
      "entity": "2",
      "type": "builtin.number",
      "startIndex": 8,
      "endIndex": 8,
      "resolution": {
        "value": "2"
      }
    },
    {
      "entity": "business",
      "type": "Seat",
      "startIndex": 16,
      "endIndex": 23,
      "resolution": {
        "values": [
          "business"
        ]
      }
    },
    {
      "entity": "2 adult business",
      "type": "TicketSeatOrder",
      "startIndex": 8,
      "endIndex": 23,
      "score": 0.8784727
    }
  ],
  "compositeEntities": [
    {
      "parentType": "TicketSeatOrder",
      "value": "2 adult business",
      "children": [
        {
          "type": "Category",
          "value": "adult"
        },
        {
          "type": "builtin.number",
          "value": "2"
        },
        {
          "type": "Seat",
          "value": "business"
        }
      ]
    }
  ]
}
```

## <a name="data-matching-multiple-list-entities"></a>Dados que correspondem a várias entidades de lista

Se uma palavra ou frase for correspondente a mais de uma entidade de lista, a consulta de ponto de extremidade retornará cada entidade de lista.

Para a consulta `when is the best time to go to red rock?`, e o aplicativo tiver a palavra `red` em mais de uma lista, o LUIS reconhecerá todas as entidades e retornará uma matriz de entidades como parte da resposta do ponto de extremidade JSON: 

```JSON
{
  "query": "when is the best time to go to red rock?",
  "topScoringIntent": {
    "intent": "Calendar.Find",
    "score": 0.06701678
  },
  "entities": [
    {
      "entity": "red",
      "type": "Colors",
      "startIndex": 31,
      "endIndex": 33,
      "resolution": {
        "values": [
          "Red"
        ]
      }
    },
    {
      "entity": "red rock",
      "type": "Cities",
      "startIndex": 31,
      "endIndex": 38,
      "resolution": {
        "values": [
          "Destinations"
        ]
      }
    }
  ]
}
```

## <a name="next-steps"></a>Próximas etapas

Confira [Adicionar entidades](luis-how-to-add-entities.md) para saber como adicionar entidades ao seu aplicativo LUIS.
