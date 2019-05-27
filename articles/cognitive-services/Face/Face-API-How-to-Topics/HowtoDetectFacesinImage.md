---
title: Detecte rostos em uma imagem - API de detecção facial
titleSuffix: Azure Cognitive Services
description: Saiba como usar vários dados retornados pelo recurso de detecção de face.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: sbowles
ms.openlocfilehash: 46bd1bdd55725878bc7b1bd55d5e24b78d82aada
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66124543"
---
# <a name="get-face-detection-data"></a>Obter dados de detecção facial

Este guia demonstra como usar a detecção de face para extrair atributos como sexo, idade ou pose de uma determinada imagem. Os trechos de código neste guia são escritos em C# usando a biblioteca de cliente da API de detecção facial de serviços Cognitivos do Azure. A mesma funcionalidade está disponível por meio de [API REST](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

Este guia mostra como fazer para:

- Para obter os locais e as dimensões de rostos em uma imagem.
- Obter os locais de vários pontos de referência, como alunos, nariz e boca, em uma imagem.
- Adivinhe o gênero, idade, emoções e outros atributos de um rosto detectado.

## <a name="setup"></a>Configuração

Este guia pressupõe que você já construiu um [FaceClient](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceclient?view=azure-dotnet) objeto, chamado `faceClient`, com uma URL de ponto de extremidade e a chave de assinatura Face. A partir daqui, você pode usar o recurso de detecção de face chamando [DetectWithUrlAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithurlasync?view=azure-dotnet), que é usado neste guia, ou [DetectWithStreamAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.faceoperationsextensions.detectwithstreamasync?view=azure-dotnet). Para obter instruções sobre como configurar esse recurso, consulte o [detectar faces de início rápido para C# ](../quickstarts/csharp-detect-sdk.md).

Este guia enfoca as especificidades da chamada de detecção, como quais argumentos, você pode passar e que você pode fazer com os dados retornados. É recomendável que você consultar apenas os recursos que necessários. Cada operação leva mais tempo para ser concluída.

## <a name="get-basic-face-data"></a>Obter dados de face básico

Para encontrar rostos e suas localizações em uma imagem, chame o método com o _returnFaceId_ parâmetro definido como **verdadeiro**. Esta é a configuração padrão.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, false, null);
```

Você pode consultar retornado [DetectedFace](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.detectedface?view=azure-dotnet) objetos para suas exclusivo IDs e um retângulo que fornece as coordenadas de pixel da face.

```csharp
foreach (var face in faces)
{
    string id = face.FaceId.ToString();
    FaceRectangle rect = face.FaceRectangle;
}
```

Para obter informações sobre como analisar a localização e as dimensões da face, consulte [FaceRectangle](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.facerectangle?view=azure-dotnet). Normalmente, esse retângulo contém os olhos, sobrancelhas, nariz e boca. Parte superior do cabeçalho, ouvidos e chin não são necessariamente incluídos. Para usar o retângulo facial para cortar uma cabeça completa ou obter um intermediário captura retrato, talvez para uma imagem do tipo de ID de foto, você pode expandir o retângulo em cada direção.

## <a name="get-face-landmarks"></a>Obter pontos de referência

[Enfrentam marcos](../concepts/face-detection.md#face-landmarks) são um conjunto de pontos de fácil de encontrar no rosto, como os alunos ou a dica do nariz. Para obter dados de ponto de referência do rosto, defina as _returnFaceLandmarks_ parâmetro **verdadeiro**.

```csharp
IList<DetectedFace> faces = await faceClient.Face.DetectWithUrlAsync(imageUrl, true, true, null);
```

O código a seguir demonstra como você pode recuperar os locais dos alunos e nariz:

```csharp
foreach (var face in faces)
{
    var landmarks = face.FaceLandmarks;

    double noseX = landmarks.NoseTip.X;
    double noseY = landmarks.NoseTip.Y;

    double leftPupilX = landmarks.PupilLeft.X;
    double leftPupilY = landmarks.PupilLeft.Y;

    double rightPupilX = landmarks.PupilRight.X;
    double rightPupilY = landmarks.PupilRight.Y;
}
```

Você também pode usar dados de pontos de referência de face para calcular com precisão a direção da face. Por exemplo, você pode definir a rotação da face como um vetor do centro da boca para o centro dos olhos. O código a seguir calcula esse vetor:

```csharp
var upperLipBottom = landmarks.UpperLipBottom;
var underLipTop = landmarks.UnderLipTop;

var centerOfMouth = new Point(
    (upperLipBottom.X + underLipTop.X) / 2,
    (upperLipBottom.Y + underLipTop.Y) / 2);

var eyeLeftInner = landmarks.EyeLeftInner;
var eyeRightInner = landmarks.EyeRightInner;

var centerOfTwoEyes = new Point(
    (eyeLeftInner.X + eyeRightInner.X) / 2,
    (eyeLeftInner.Y + eyeRightInner.Y) / 2);

Vector faceDirection = new Vector(
    centerOfTwoEyes.X - centerOfMouth.X,
    centerOfTwoEyes.Y - centerOfMouth.Y);
```

Quando você souber a direção da face, você pode girar o quadro de face retangular para alinhá-lo mais corretamente. Para cortar rostos em uma imagem, você pode programaticamente girar a imagem para que os rostos aparecem sempre na vertical.

## <a name="get-face-attributes"></a>Obter atributos faciais

Além de retângulos faciais e pontos de referência, a API de detecção facial pode analisar vários atributos conceituais de uma face. Para obter uma lista completa, consulte o [atributos faciais](../concepts/face-detection.md#attributes) seção conceitual.

Para analisar atributos faciais, defina as _returnFaceAttributes_ parâmetro a uma lista de [enumeração FaceAttributeType](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet) valores.

```csharp
var requiredFaceAttributes = new FaceAttributeType[] {
    FaceAttributeType.Age,
    FaceAttributeType.Gender,
    FaceAttributeType.Smile,
    FaceAttributeType.FacialHair,
    FaceAttributeType.HeadPose,
    FaceAttributeType.Glasses,
    FaceAttributeType.Emotion
};
var faces = await faceClient.DetectWithUrlAsync(imageUrl, true, false, requiredFaceAttributes);
```

Em seguida, obter referências para os dados retornados e fazer mais operações de acordo com suas necessidades.

```csharp
foreach (var face in faces)
{
    var attributes = face.FaceAttributes;
    var age = attributes.Age;
    var gender = attributes.Gender;
    var smile = attributes.Smile;
    var facialHair = attributes.FacialHair;
    var headPose = attributes.HeadPose;
    var glasses = attributes.Glasses;
    var emotion = attributes.Emotion;
}
```

Para saber mais sobre cada um dos atributos, consulte o [detecção facial e atributos](../concepts/face-detection.md) guia conceitual.

## <a name="next-steps"></a>Próximas etapas

Neste guia, você aprendeu a usar as várias funcionalidades de detecção facial. Em seguida, integre esses recursos em seu aplicativo seguindo um tutorial detalhado.

- [Tutorial: Criar um aplicativo do WPF para exibir dados de face em uma imagem](../Tutorials/FaceAPIinCSharpTutorial.md)
- [Tutorial: Criar um aplicativo do Android para detectar e quadro rostos em uma imagem](../Tutorials/FaceAPIinJavaForAndroidTutorial.md)

## <a name="related-topics"></a>Tópicos relacionados

- [Documentação de referência (REST)](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
- [Documentação de referência (SDK do .NET)](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/face?view=azure-dotnet)