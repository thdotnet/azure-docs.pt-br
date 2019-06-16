---
title: Entidade predefinida de idade
titleSuffix: Azure
description: Este artigo contém informações sobre a entidade predefinida e idade em LUIS (Serviço Inteligente de Reconhecimento Vocal).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: b8239688000f0ce32ca2c2be054b1443bbb698b5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65069130"
---
# <a name="age-prebuilt-entity-for-a-luis-app"></a>Entidade de idade predefinida para um aplicativo LUIS
A entidade de idade predefinida captura o valor de idade tanto numericamente quanto em termos de dias, semanas, meses e anos. Uma vez que essa entidade já está treinada, não é necessário adicionar enunciados contendo email às intenções do aplicativo. A entidade de idade é compatível com [muitas culturas](luis-reference-prebuilt-entities.md). 

## <a name="types-of-age"></a>Tipos de idade
A idade é gerenciada por meio do repositório do GitHub [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3)

## <a name="resolution-for-prebuilt-age-entity"></a>Resolução para entidade de idade predefinida

### <a name="api-version-2x"></a>Versão da API 2. x

O exemplo a seguir mostra a resolução da entidade **builtin.age**.

```json
{
  "query": "A 90 day old utilities bill is quite late.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8236133
  },
  "entities": [
    {
      "entity": "90 day old",
      "type": "builtin.age",
      "startIndex": 2,
      "endIndex": 11,
      "resolution": {
        "unit": "Day",
        "value": "90"
      }
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Versão de API de visualização 3. x

O JSON a seguir é com o `verbose` parâmetro definido como `false`:

```json
{
    "query": "A 90 day old utilities bill is quite late.",
    "prediction": {
        "normalizedQuery": "a 90 day old utilities bill is quite late.",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.558252
            }
        },
        "entities": {
            "age": [
                {
                    "number": 90,
                    "unit": "Day"
                }
            ]
        }
    }
}
```

O JSON a seguir é com o `verbose` parâmetro definido como `true`:

```json
{
    "query": "A 90 day old utilities bill is quite late.",
    "prediction": {
        "normalizedQuery": "a 90 day old utilities bill is quite late.",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.558252
            }
        },
        "entities": {
            "age": [
                {
                    "number": 90,
                    "unit": "Day"
                }
            ],
            "$instance": {
                "age": [
                    {
                        "type": "builtin.age",
                        "text": "90 day old",
                        "startIndex": 2,
                        "length": 10,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor"
                    }
                ]
            }
        }
    }
}
```

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre as entidades [moeda](luis-reference-prebuilt-currency.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md) e [dimensão](luis-reference-prebuilt-dimension.md). 
