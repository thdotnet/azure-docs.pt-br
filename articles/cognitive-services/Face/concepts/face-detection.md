---
title: Conceitos de atributos e de detecção facial
titleSuffix: Azure Cognitive Services
description: Aprenda os conceitos sobre a detecção de face e atributos faciais.
services: cognitive-services
author: PatrickFarley
manager: nitime
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: pafarley
ms.openlocfilehash: e61048eeab9d7061c18f3237db22fc87ca52f526
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65891159"
---
# <a name="face-detection-and-attributes"></a>Detecção facial e atributos

Este artigo explica os conceitos de detecção facial e de dados de atributo de face. Detecção facial é a ação de localização rostos humanos em uma imagem e, opcionalmente, retornar diferentes tipos de dados relacionados a face.

Você usa o [enfrentam - detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) operação detectar faces em uma imagem. No mínimo, cada face detectada corresponde a um campo de faceRectangle na resposta. Esse conjunto de coordenadas de pixel para a esquerda, superior, largura e altura marcam a face localizada. Usando essas coordenadas, você pode obter a localização de face e seu tamanho. A resposta da API, faces são listadas na ordem de tamanho do maior ao menor.

## <a name="face-id"></a>Face ID

A identificação de face é uma cadeia de caracteres de identificador exclusivo para cada face detectada em uma imagem. Você pode solicitar uma face ID em sua [enfrentam - detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) chamada à API.

## <a name="face-landmarks"></a>Pontos de referência facial

Pontos de referência são um conjunto de pontos de fácil de encontrar no rosto, como os alunos ou a dica do nariz. Por padrão, há 27 pontos de referência facial predefinidos. A figura a seguir mostra todos os pontos de 27:

![Um diagrama de face com todos os pontos de 27 referência rotulado](../Images/landmarks.1.jpg)

As coordenadas dos pontos são retornadas em unidades de pixels.

## <a name="attributes"></a>Atributos

Os atributos são um conjunto de recursos que opcionalmente podem ser detectados pelo [enfrentam - detectar](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API. Os seguintes atributos podem ser detectados:

* **Idade**. A idade estimada em anos de uma determinada face.
* **Desfoque**. O pouco de desfoque da face na imagem. Esse atributo retorna um valor entre zero e um e uma classificação informal de baixa, média ou alta.
* **Emoções**. Uma lista de emoções com confiança suas detecção para determinada face. Pontuações de confiança são normalizadas e as pontuações em todas as emoções adicionar até um. As emoções retornadas são felicidade, tristeza, neutral, raiva, desdém, aversão, surpresa e medo.
* **Exposição**. A exposição da face na imagem. Esse atributo retorna um valor entre zero e um e uma classificação informal de subexposição, goodExposure ou Superexposição.
* **Pelos faciais**. A presença de cabelo faciais estimado e o comprimento para determinada face.
* **Gênero**. O sexo estimado de determinada face. Os valores possíveis são genderless masculino e feminino.
* **Óculos**. Se a determinada face tem óculos. Os valores possíveis são NoGlasses, ReadingGlasses, óculos de sol e Swimming óculos.
* **Hair**. O tipo de cabelo da face. Esse atributo mostra se os cabelos está visível, se baldness é detectado e quais cores de cabelo são detectadas.
* **Head apresentar**. Orientação da superfície no espaço 3D. Esse atributo é descrito pelo pitch, roll e yaw ângulos em graus. Os intervalos de valores são os graus de-90 a 90 graus, - 180 graus a 180 graus e graus de-90 a 90 graus, respectivamente. Consulte o diagrama a seguir para mapeamentos de ângulo:

    ![Um cabeçalho com o tom, roll e yaw eixos rotulados](../Images/headpose.1.jpg)
* **Makeup**. Se a face tem composição. Esse atributo retorna um valor booleano para eyeMakeup e lipMakeup.
* **Ruído**. Poluição visual detectada na imagem de face. Esse atributo retorna um valor entre zero e um e uma classificação informal de baixa, média ou alta.
* **Oclusão**. Se houver objetos bloqueio partes da face. Esse atributo retorna um valor booleano para eyeOccluded, foreheadOccluded e mouthOccluded.
* **Smiley**. A expressão de Smiley da face determinado. Esse valor está entre zero para nenhum Smiley e um para um sorriso claro.

> [!IMPORTANT]
> Atributos faciais foram previstos com o uso de algoritmos estatísticos. Eles podem não ser precisos. Tenha cuidado ao tomar decisões com base nos dados de atributo.

## <a name="input-data"></a>Dados de entrada

Use as dicas a seguir para certificar-se de que suas imagens de entrada fornecer os resultados mais precisos de detecção:

* Os formatos de imagem de entrada com suporte são JPEG, PNG, GIF, para o primeiro quadro e BMP.
* O tamanho do arquivo de imagem deve ser maior do que 4 MB.
* O intervalo de tamanhos de face detectáveis é de 36 x 36 a 4096 x 4096 pixels. Faces fora desse intervalo não serão detectados.
* Algumas faces não podem ser detectados devido a desafios técnicos. Extremos ângulos de face (principal) ou oclusão face (objetos como óculos de sol ou mãos que bloqueiam a parte da face) pode afetar a detecção. Faces frontais e quase frontais fornecem os melhores resultados.

Se você estiver detectando rostos de uma transmissão de vídeo, você poderá melhorar o desempenho ajustando determinadas configurações em sua câmera de vídeo:

* **A suavização**: Muitas câmeras de vídeo aplicar um efeito de atenuação. Você deve desativar isso se for possível, pois ele cria um desfoque entre os quadros e reduz a clareza.
* **Obturador velocidade**: Uma rápida do obturador reduz a quantidade de movimento entre os quadros e faz com que cada quadro mais claro. É recomendável do obturador velocidade de 1/60 segundos ou mais rápido.
* **Ângulo de obturador**: Algumas câmeras especificam ângulo do obturador, em vez de velocidade do obturador. Você deve usar um ângulo do obturador inferior se possível. Isso resultará em quadros de vídeo mais claros.

    >[!NOTE]
    > Uma câmera com um ângulo do obturador inferior receberá menos luz em cada quadro, portanto, a imagem será mais escura. Você precisará determinar o nível certo para usar.

## <a name="next-steps"></a>Próximas etapas

Agora que você está familiarizado com conceitos de detecção de face, saiba como escrever um script que detecta rostos em uma determinada imagem.

* [Detecte rostos em uma imagem](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)