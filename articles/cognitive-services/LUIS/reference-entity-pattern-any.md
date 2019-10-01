---
title: Padrão. qualquer tipo de entidade-LUIS
titleSuffix: Azure Cognitive Services
description: Pattern.any é um espaço reservado de tamanho variável usado apenas em um enunciado de modelo para marcar onde a entidade começa e termina.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: 2239387ffff4c30e1183721a528e666199316bed
ms.sourcegitcommit: 8bae7afb0011a98e82cbd76c50bc9f08be9ebe06
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71695088"
---
# <a name="patternany-entity"></a>Entidade pattern.any 

Pattern.any é um espaço reservado de tamanho variável usado apenas em um enunciado de modelo para marcar onde a entidade começa e termina.  

As entidades Pattern.any devem ser marcadas nos exemplos de modelo [Padrão](luis-how-to-model-intent-pattern.md), não exemplos de usuários de intenção.

**A entidade é uma boa opção quando:**

* O final da entidade pode ser confundido com o texto restante do enunciado. 

## <a name="usage"></a>Uso

Em um determinado aplicativo cliente que pesquisa livros com base no título, o pattern.any extrai o título completo. Um enunciado de modelo usando pattern.any para essa busca de livro é `Was {BookTitle} written by an American this year[?]`. 

Na tabela a seguir, cada linha tem duas versões do enunciado. A principal expressão é como a LUIS inicialmente vê o expressão. Não fica claro onde o título do livro começa e termina. O expressão inferior usa um padrão. qualquer entidade para marcar o início e o fim da entidade. 

|Expressão com a entidade em negrito|
|--|
|`Was The Man Who Mistook His Wife for a Hat and Other Clinical Tales written by an American this year?`<br><br>**The Man Who Mistook His Wife for a Hat and Other Clinical Tales** foi escrito por um americano este ano?|
|`Was Half Asleep in Frog Pajamas written by an American this year?`<br><br>**Half Asleep in Frog Pajamas** foi escrito por uma americano este ano?|
|`Was The Particular Sadness of Lemon Cake: A Novel written by an American this year?`<br><br>**The Particular Sadness of Lemon Cake: foi um romance escrito** por um americano este ano?|
|`Was There's A Wocket In My Pocket! written by an American this year?`<br><br>**There's A Wocket In My Pocket!** foi escrito por um americano este ano?|
||



## <a name="example-json"></a>JSON de exemplo

Considere a seguinte consulta:

`where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

Pelo nome do formulário inserido a ser extraído como padrão. any:

`Understand your responsibilities as a member of the community`

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[Resposta de ponto de extremidade de previsão v2](#tab/V2)

```JSON
"entities": [
  {
    "entity": "understand your responsibilities as a member of the community",
    "type": "FormName",
    "startIndex": 18,
    "endIndex": 78,
    "role": ""
  }
```


#### <a name="v3-prediction-endpoint-responsetabv3"></a>[Resposta de ponto de extremidade de previsão v3](#tab/V3)

Esse é o JSON se `verbose=false` for definido na cadeia de caracteres de consulta:

```json
"entities": {
    "FormName": [
        "Understand your responsibilities as a member of the community"
    ]
}
```

Esse é o JSON se `verbose=true` for definido na cadeia de caracteres de consulta:

```json
"entities": {
    "FormName": [
        "Understand your responsibilities as a member of the community"
    ],
    "$instance": {
        "FormName": [
            {
                "type": "FormName",
                "text": "Understand your responsibilities as a member of the community",
                "startIndex": 18,
                "length": 61,
                "modelTypeId": 7,
                "modelType": "Pattern.Any Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * * 

## <a name="next-steps"></a>Próximas etapas

Neste [tutorial](luis-tutorial-pattern-any.md), use o **padrão. qualquer** entidade para extrair dados de declarações em que o declarações está bem formatado e onde o final dos dados pode ser facilmente confundido com as palavras restantes do expressão.