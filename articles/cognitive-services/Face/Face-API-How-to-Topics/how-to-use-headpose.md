---
title: Use o atributo HeadPose
titleSuffix: Azure Cognitive Services
description: Saiba como usar o atributo HeadPose para girar automaticamente o retângulo facial ou detectar gestos principais em uma transmissão de vídeo.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: sample
ms.date: 05/29/2019
ms.author: pafarley
ms.openlocfilehash: 168b4fce873206e39a32a83da3dc5509b431d6a1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67058585"
---
# <a name="use-the-headpose-attribute"></a>Use o atributo HeadPose

Neste guia, você verá como você pode usar o atributo HeadPose de um rosto detectado para habilitar alguns cenários importantes.

## <a name="rotate-the-face-rectangle"></a>Girar o retângulo facial

O retângulo facial, retornado com cada rosto detectado, marca o local e o tamanho da superfície na imagem. Por padrão, o retângulo é sempre alinhado com a imagem (suas laterais são horizontal e vertical); Isso pode ser ineficiente para angulares faces de delimitação de quadros. Em situações onde você deseja cortar programaticamente rostos em uma imagem, é melhor que seja capaz de girar o retângulo para cortar.

O [WPF de Face de serviços Cognitivos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) aplicativo de exemplo usa o atributo HeadPose para girar suas retângulos de rosto detectado.

### <a name="explore-the-sample-code"></a>Explorar o código de exemplo

Programaticamente, você pode girar o retângulo facial, usando o atributo HeadPose. Se você especificar esse atributo ao detectar faces (consulte [como detectar faces](HowtoDetectFacesinImage.md)), você poderá consultá-lo mais tarde. O seguinte método da [WPF de Face de serviços Cognitivos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) aplicativo utiliza uma lista de **DetectedFace** objetos e retorna uma lista de **[Face](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/Face.cs)** objetos. **Face** aqui está uma classe personalizada que armazena enfrenta dados, incluindo as coordenadas do retângulo atualizado. Novos valores são calculados para **superior**, **esquerdo**, **largura**, e **altura**e um novo campo **FaceAngle**Especifica a rotação.

```csharp
/// <summary>
/// Calculate the rendering face rectangle
/// </summary>
/// <param name="faces">Detected face from service</param>
/// <param name="maxSize">Image rendering size</param>
/// <param name="imageInfo">Image width and height</param>
/// <returns>Face structure for rendering</returns>
public static IEnumerable<Face> CalculateFaceRectangleForRendering(IList<DetectedFace> faces, int maxSize, Tuple<int, int> imageInfo)
{
    var imageWidth = imageInfo.Item1;
    var imageHeight = imageInfo.Item2;
    var ratio = (float)imageWidth / imageHeight;
    int uiWidth = 0;
    int uiHeight = 0;
    if (ratio > 1.0)
    {
        uiWidth = maxSize;
        uiHeight = (int)(maxSize / ratio);
    }
    else
    {
        uiHeight = maxSize;
        uiWidth = (int)(ratio * uiHeight);
    }

    var uiXOffset = (maxSize - uiWidth) / 2;
    var uiYOffset = (maxSize - uiHeight) / 2;
    var scale = (float)uiWidth / imageWidth;

    foreach (var face in faces)
    {
        var left = (int)(face.FaceRectangle.Left * scale + uiXOffset);
        var top = (int)(face.FaceRectangle.Top * scale + uiYOffset);

        // Angle of face rectangles, default value is 0 (not rotated).
        double faceAngle = 0;

        // If head pose attributes have been obtained, re-calculate the left & top (X & Y) positions.
        if (face.FaceAttributes?.HeadPose != null)
        {
            // Head pose's roll value acts directly as the face angle.
            faceAngle = face.FaceAttributes.HeadPose.Roll;
            var angleToPi = Math.Abs((faceAngle / 180) * Math.PI);

            // _____       | / \ |
            // |____|  =>  |/   /|
            //             | \ / |
            // Re-calculate the face rectangle's left & top (X & Y) positions.
            var newLeft = face.FaceRectangle.Left +
                face.FaceRectangle.Width / 2 -
                (face.FaceRectangle.Width * Math.Sin(angleToPi) + face.FaceRectangle.Height * Math.Cos(angleToPi)) / 2;

            var newTop = face.FaceRectangle.Top +
                face.FaceRectangle.Height / 2 -
                (face.FaceRectangle.Height * Math.Sin(angleToPi) + face.FaceRectangle.Width * Math.Cos(angleToPi)) / 2;

            left = (int)(newLeft * scale + uiXOffset);
            top = (int)(newTop * scale + uiYOffset);
        }

        yield return new Face()
        {
            FaceId = face.FaceId?.ToString(),
            Left = left,
            Top = top,
            OriginalLeft = (int)(face.FaceRectangle.Left * scale + uiXOffset),
            OriginalTop = (int)(face.FaceRectangle.Top * scale + uiYOffset),
            Height = (int)(face.FaceRectangle.Height * scale),
            Width = (int)(face.FaceRectangle.Width * scale),
            FaceAngle = faceAngle,
        };
    }
}
```

### <a name="display-the-updated-rectangle"></a>Exibir o retângulo atualizado

A partir daqui, você pode usar retornado **Face** objetos na exibição. As seguintes linhas de [FaceDetectionPage.xaml](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/app-samples/Cognitive-Services-Face-WPF/Sample-WPF/Controls/FaceDetectionPage.xaml) mostram como o novo retângulo é renderizado a partir desses dados:

```xaml
 <DataTemplate>
    <Rectangle Width="{Binding Width}" Height="{Binding Height}" Stroke="#FF26B8F4" StrokeThickness="1">
        <Rectangle.LayoutTransform>
            <RotateTransform Angle="{Binding FaceAngle}"/>
        </Rectangle.LayoutTransform>
    </Rectangle>
</DataTemplate>
```

## <a name="detect-head-gestures"></a>Detectar gestos principais

Você pode detectar principais gestos, como nodding e head agitando acompanhando as alterações de HeadPose em tempo real. Você pode usar esse recurso como um detector de atividade personalizada.

Detecção de atividade é a tarefa de determinar que uma entidade é uma pessoa real e não uma representação de imagem ou vídeo. Um detector de gesto principal pode servir como uma maneira de ajudar a verificar a execução, especialmente em vez de uma representação de imagem de uma pessoa.

> [!CAUTION]
> Para detectar gestos principais em tempo real, você precisará chamar a API de detecção facial em uma taxa alta (mais de uma vez por segundo). Se você tiver uma assinatura gratuita – camada (f0), isso não será possível. Se você tiver uma assinatura de camada paga, certifique-se de calcular os custos de API rápida de fazer chamadas para o cabeçalho de detecção de gesto.

Consulte a [HeadPose exemplo da API de detecção facial](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/FaceAPIHeadPoseSample) no GitHub para obter um exemplo de funcionamento da cabeça de detecção de gesto.

## <a name="next-steps"></a>Próximas etapas

Consulte a [WPF de Face de serviços Cognitivos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples/Cognitive-Services-Face-WPF) app no GitHub para obter um exemplo prático de retângulos faciais girado. Ou, consulte a [HeadPose exemplo da API de detecção facial](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/app-samples) aplicativo, que controla o atributo HeadPose em tempo real para detectar movimentos principais.