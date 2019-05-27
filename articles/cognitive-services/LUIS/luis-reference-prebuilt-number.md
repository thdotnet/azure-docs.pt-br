---
title: Entidade numérica predefinida
titleSuffix: Azure
description: Este artigo contém informações sobre a entidade predefinida de número de LUIS (Serviço Inteligente de Reconhecimento Vocal).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: d4f707d4bf9bac5e2208eadb94983af368b9f521
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "65072254"
---
# <a name="number-prebuilt-entity-for-a-luis-app"></a>Entidade numérica predefinida para um aplicativo LUIS
Há muitas maneiras de usar os valores numéricos para quantificar, expressar e descrever informações. Este artigo aborda apenas alguns dos exemplos possíveis. O LUIS interpreta as variações em enunciados do usuário e retorna valores numéricos consistentes. Uma vez que essa entidade já está treinada, não é necessário adicionar enunciados de exemplo contendo número às intenções do aplicativo. 

## <a name="types-of-number"></a>Tipos de número
O número é gerenciado por meio do repositório do GitHub [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml)

## <a name="examples-of-number-resolution"></a>Exemplos de resolução de número

| Enunciado        | Entidade   | Resolução |
| ------------- |:----------------:| --------------:|
| ```one thousand times```  | ```"one thousand"``` |   ```"1000"```      | 
| ```1,000 people```        | ```"1,000"```    |   ```"1000"```      |
| ```1/2 cup```         | ```"1 / 2"```    |    ```"0.5"```      |
|  ```one half the amount```     | ```"one half"```     |    ```"0.5"```      |
| ```one hundred fifty orders``` | ```"one hundred fifty"``` | ```"150"``` |
| ```one hundred and fifty books``` | ```"one hundred and fifty"``` | ```"150"```|
| ```a grade of one point five```| ```"one point five"``` |  ```"1.5"``` |
| ```buy two dozen eggs```    | ```"two dozen"``` | ```"24"``` |


O LUIS inclui o valor reconhecido de uma entidade **`builtin.number`** no campo `resolution` da resposta JSON que ele retorna.

## <a name="resolution-for-prebuilt-number"></a>Resolução para número predefinido


### <a name="api-version-2x"></a>Versão da API 2. x

O exemplo a seguir mostra uma resposta JSON de LUIS, que inclui a resolução do valor de 24, para o enunciado "duas dúzias".

```json
{
  "query": "order two dozen eggs",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.105443209
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.105443209
    },
    {
      "intent": "OrderFood",
      "score": 0.9468431361
    },
    {
      "intent": "Help",
      "score": 0.000399122015
    },
  ],
  "entities": [
    {
      "entity": "two dozen",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 14,
      "resolution": {
        "subtype": "integer",
        "value": "24"
      }
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Versão de API de visualização 3. x

O JSON a seguir é com o `verbose` parâmetro definido como `false`:

```json
{
    "query": "order two dozen eggs",
    "prediction": {
        "normalizedQuery": "order two dozen eggs",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.7124502
            }
        },
        "entities": {
            "number": [
                24
            ]
        }
    }
}
```

O JSON a seguir é com o `verbose` parâmetro definido como `true`:

```json
{
    "query": "order two dozen eggs",
    "prediction": {
        "normalizedQuery": "order two dozen eggs",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.7124502
            }
        },
        "entities": {
            "number": [
                24
            ],
            "$instance": {
                "number": [
                    {
                        "type": "builtin.number",
                        "text": "two dozen",
                        "startIndex": 6,
                        "length": 9,
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

Saiba mais sobre [moeda](luis-reference-prebuilt-currency.md), [ordinal](luis-reference-prebuilt-ordinal.md) e [percentual](luis-reference-prebuilt-percentage.md). 
