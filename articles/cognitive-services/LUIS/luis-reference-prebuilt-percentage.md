---
title: Entidade de percentual predefinida
titleSuffix: Azure
description: Este artigo contém informações sobre a entidade predefinida de percentual de LUIS (Serviço Inteligente de Reconhecimento Vocal).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 75538519b7d43aa702e15ce3c22ea4acc73ade87
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "65072209"
---
# <a name="percentage-prebuilt-entity-for-a-luis-app"></a>Entidade de percentual predefinida para um aplicativo LUIS
Números de percentual podem aparecer como frações, `3 1/2` ou como percentual, `2%`. Uma vez que essa entidade já está treinada, não é necessário adicionar enunciados de exemplo contendo percentual às intenções do aplicativo. A entidade de percentual é compatível com [muitas culturas](luis-reference-prebuilt-entities.md). 

## <a name="types-of-percentage"></a>Tipos de percentual
O percentual é gerenciado usando o repositório do GitHub [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L114)

## <a name="resolution-for-prebuilt-percentage-entity"></a>Resolução para entidade de percentual predefinida

### <a name="api-version-2x"></a>Versão da API 2. x

O exemplo a seguir mostra a resolução da entidade **builtin.percentage**.

```json
{
  "query": "set a trigger when my stock goes up 2%",
  "topScoringIntent": {
    "intent": "SetTrigger",
    "score": 0.971157849
  },
  "intents": [
    {
      "intent": "SetTrigger",
      "score": 0.971157849
    }
  ],
  "entities": [
    {
      "entity": "2%",
      "type": "builtin.percentage",
      "startIndex": 36,
      "endIndex": 37,
      "resolution": {
        "value": "2%"
      }
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Versão de API de visualização 3. x

O JSON a seguir é com o `verbose` parâmetro definido como `false`:

```json
{
    "query": "set a trigger when my stock goes up 2%",
    "prediction": {
        "normalizedQuery": "set a trigger when my stock goes up 2%",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.541765451
            }
        },
        "entities": {
            "percentage": [
                2
            ]
        }
    }
}
```

O JSON a seguir é com o `verbose` parâmetro definido como `true`:

```json
{
    "query": "set a trigger when my stock goes up 2%",
    "prediction": {
        "normalizedQuery": "set a trigger when my stock goes up 2%",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.541765451
            }
        },
        "entities": {
            "percentage": [
                2
            ],
            "$instance": {
                "percentage": [
                    {
                        "type": "builtin.percentage",
                        "text": "2%",
                        "startIndex": 36,
                        "length": 2,
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

Saiba mais sobre as entidades [ordinal](luis-reference-prebuilt-ordinal.md), [número](luis-reference-prebuilt-number.md) e [temperatura](luis-reference-prebuilt-temperature.md). 
