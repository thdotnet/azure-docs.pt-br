---
title: Detecção de marca – Pesquisa Visual Computacional
titleSuffix: Azure Cognitive Services
description: Conceitos relacionados à detecção de marca/logotipo usando a API da Pesquisa Visual Computacional.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: pafarley
ms.openlocfilehash: c3a80cbdd166f77681665ee3675c1a71ce3a9bd6
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70967082"
---
# <a name="detect-popular-brands-in-images"></a>Detectar marcas populares em imagens

A detecção de marca é um modo especializado de [detecção de objeto](concept-object-detection.md) que usa um banco de dados de milhares de logotipos globais para identificar marcas comerciais em imagens ou vídeo. Você pode usar esse recurso, por exemplo, para descobrir quais marcas são mais populares em mídia social ou mais predominantes no posicionamento de produto de mídia.

O serviço de Pesquisa Visual Computacional detecta se há logotipos da marca em uma determinada imagem; quando há, ele retorna o nome da marca, uma pontuação de confiança e as coordenadas de uma caixa delimitadora em torno do logotipo.

O banco de dados interno do logotipo aborda marcas populares em aparelhos eletrônicos, roupas e muito mais. Se você acha que a marca que você está procurando não foi detectada pelo serviço de Pesquisa Visual Computacional, pode ser melhor criar e treinar seu próprio detector de logotipos usando o serviço de [Visão Personalizada](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/).

## <a name="brand-detection-example"></a>Exemplo de detecção de marca

As respostas JSON a seguir ilustram o que a Pesquisa Visual Computacional retorna ao detectar marcas nas imagens de exemplo.

![Uma camiseta vermelha com um rótulo e o logotipo da Microsoft nele](./Images/red-shirt-logo.jpg)

```json
"brands":[  
   {  
      "name":"Microsoft",
      "rectangle":{  
         "x":20,
         "y":97,
         "w":62,
         "h":52
      }
   }
]
```

Em alguns casos, o detector de marca selecionará a imagem do logotipo e o nome da marca estilizado como dois logotipos separados.

![Um agasalho leve cinza com um rótulo e o logotipo da Microsoft nele](./Images/gray-shirt-logo.jpg)

```json
"brands":[  
   {  
      "name":"Microsoft",
      "rectangle":{  
         "x":58,
         "y":106,
         "w":55,
         "h":46
      }
   },
   {  
      "name":"Microsoft",
      "rectangle":{  
         "x":58,
         "y":86,
         "w":202,
         "h":63
      }
   }
]
```

## <a name="use-the-api"></a>Usar a API

O recurso de detecção de marca faz parte da API [Analisar Imagem](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa). Você pode chamar essa API por meio de um SDK nativo ou por meio de chamadas REST. Inclua `Brands` no parâmetro de consulta **visualFeatures** . Em seguida, quando você obtém a resposta JSON completa, simplesmente analise a cadeia de caracteres para o `"brands"` conteúdo da seção.

* [Início Rápido: SDK do .NET Pesquisa Visual Computacional](./quickstarts-sdk/csharp-sdk.md)
* [Início Rápido: Analisar uma imagem (API REST)](./quickstarts/csharp-analyze.md)
