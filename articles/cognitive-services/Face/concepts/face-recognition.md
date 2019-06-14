---
title: Conceitos de reconhecimento facial
titleSuffix: Azure Cognitive Services
description: Aprenda os conceitos sobre reconhecimento facial.
services: cognitive-services
author: PatrickFarley
manager: nitime
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: pafarley
ms.openlocfilehash: fa38c492530cb8938e49bc15e13fdd39ed5b6f1c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65890890"
---
# <a name="face-recognition-concepts"></a>Conceitos de reconhecimento facial

Este artigo explica os conceitos das operações de reconhecimento de face verificar, localizar Similar, grupo e identifique e estruturas de dados subjacentes. Em larga escala, reconhecimento descreve o trabalho de comparar duas faces diferentes para determinar se eles são semelhantes ou que pertencem à mesma pessoa.

## <a name="recognition-related-data-structures"></a>Estruturas de dados relacionados ao reconhecimento

As operações de reconhecimento usam principalmente as seguintes estruturas de dados. Esses objetos são armazenados na nuvem e podem ser referenciados por suas cadeias de caracteres de ID. Cadeias de caracteres de ID sempre são exclusivas dentro de uma assinatura. Campos de nome podem ser duplicado.

|NOME|DESCRIÇÃO|
|:--|:--|
|DetectedFace| Essa representação única face recuperada com o [detecção facial](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md) operação. Sua ID expira 24 horas após sua criação.|
|PersistedFace| Quando objetos DetectedFace são adicionados a um grupo, como FaceList ou pessoa, eles se tornam objetos PersistedFace. Eles podem ser [recuperados](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524c) a qualquer hora e não expiram.|
|[FaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039524b) ou [LargeFaceList](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/5a157b68d2de3616c086f2cc)| Essa estrutura de dados é uma lista variada de PersistedFace objetos. Um FaceList tem uma ID exclusiva, uma cadeia de caracteres de nome e, opcionalmente, uma cadeia de caracteres de dados do usuário.|
|[Pessoa](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523c)| Essa estrutura de dados é uma lista de objetos PersistedFace que pertencem à mesma pessoa. Ele tem uma ID exclusiva, uma cadeia de caracteres de nome e, opcionalmente, uma cadeia de caracteres de dados do usuário.|
|[PersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395244) ou [LargePersonGroup](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/599acdee6ac60f11b48b5a9d)| Essa estrutura de dados é uma lista variada de objetos Person. Ele tem uma ID exclusiva, uma cadeia de caracteres de nome e, opcionalmente, uma cadeia de caracteres de dados do usuário. Um PersonGroup deve ser [treinado](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395249) antes que ele pode ser usado em operações de reconhecimento.|

## <a name="recognition-operations"></a>Operações de reconhecimento

Esta seção fornece detalhes sobre como as quatro operações de reconhecimento usam as estruturas de dados descritas anteriormente. Para obter uma descrição de cada operação de reconhecimento ampla, consulte [visão geral](../Overview.md).

### <a name="verify"></a>Verificar

O [Verify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a) operação usa uma ID de face de DetectedFace ou PersistedFace e outro face ID ou um objeto Person e determina se eles pertencem à mesma pessoa. Se você passar um objeto Person, você pode, opcionalmente, passar um PersonGroup ao qual essa pessoa pertence para melhorar o desempenho.

### <a name="find-similar"></a>Localizar semelhante

O [localizar Similar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237) operação leva uma identificação de face de DetectedFace ou PersistedFace e um FaceList ou uma matriz de outras IDs de face. Com um FaceList, ele retornará um FaceList menor de faces que são semelhantes à determinada face. Com uma matriz de IDs de face, ela da mesma forma retorna uma matriz de menor.

### <a name="group"></a>Agrupar

O [grupo](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238) operação pega uma matriz de IDs de face variadas de DetectedFace ou PersistedFace e retorna as mesmas IDs agrupadas em várias matrizes menores. Cada matriz de "grupos" contém as IDs que pareçam semelhantes de face. Uma matriz única "messyGroup" contém IDs de face para que nenhum semelhanças foram encontradas.

### <a name="identify"></a>Identificar

O [Identify](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239) operação usa uma ou várias IDs de face da DetectedFace ou PersistedFace e um PersonGroup e retorna uma lista de objetos Person que cada face pode pertencer a. Retornado pessoa objetos são encapsulados como objetos de candidato, que tem um valor de confiança de previsão.

## <a name="input-data"></a>Dados de entrada

Use as dicas a seguir para garantir que suas imagens de entrada fornecer os resultados mais precisos do reconhecimento:

* Os formatos de imagem de entrada com suporte são JPEG, PNG, GIF (o primeiro quadro), BMP.
* Tamanho do arquivo de imagem deve ser maior do que 4 MB.
* Quando você cria objetos Person, use as fotos que possuem tipos diferentes de ângulos e iluminação.
* Algumas faces não poderão ser reconhecidas devido a desafios técnicos, como:
  * Imagens com extrema iluminação, por exemplo, grave luz de fundo.
  * Se há obstruções que bloqueiam um ou ambos os olhos.
  * Diferenças de cabelo de tipo ou facial de cabelo.
  * Alterações na aparência facial devido a idade.
  * Expressões faciais extremas.

## <a name="next-steps"></a>Próximas etapas

Agora que você está familiarizado com conceitos de reconhecimento de face, saiba como escrever um script que identifica as faces em relação a um PersonGroup treinado.

* [Identifique rostos em imagens](../Face-API-How-to-Topics/HowtoIdentifyFacesinImage.md)