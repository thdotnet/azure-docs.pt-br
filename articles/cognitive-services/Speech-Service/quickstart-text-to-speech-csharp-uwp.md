---
title: 'Início Rápido: Sintetizar fala, C# (UWP) – Serviço de Fala'
titleSuffix: Azure Cognitive Services
description: Neste artigo, você criará um aplicativo UWP (Plataforma Universal do Windows) em C# usando o SDK de Fala dos Serviços Cognitivos. Você pode sintetizar em tempo real uma fala a partir de texto no alto-falante do seu dispositivo. O aplicativo é criado com o Pacote NuGet do SDK de Fala e o Microsoft Visual Studio 2019.
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/19/2019
ms.author: yinhew
ms.openlocfilehash: 65b65c9af377b6a9951f9f328e0732850d3b9c1d
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382197"
---
# <a name="quickstart-synthesize-speech-in-a-uwp-app-by-using-the-speech-sdk"></a>Início Rápido: Sintetizar a fala em um aplicativo UWP usando o SDK de Fala

Guias de início rápido também estão disponíveis para o [reconhecimento de fala](quickstart-csharp-uwp.md), a [tradução de fala](quickstart-translate-speech-uwp.md) e [o assistente virtual que tem como prioridade o uso de voz](quickstart-virtual-assistant-csharp-uwp.md).

Neste artigo, você desenvolverá um aplicativo UWP (Plataforma Universal do Windows) em C# usando o [SDK de Fala](speech-sdk.md) dos Serviços Cognitivos. O programa sintetiza em tempo real uma fala com base no texto no alto-falante do seu dispositivo. Você cria o aplicativo com o [Pacote NuGet do SDK de Fala](https://aka.ms/csspeech/nuget) e o Microsoft Visual Studio 2019 (qualquer edição).

> [!NOTE]
> A Plataforma Universal do Windows permite desenvolver aplicativos que executam em qualquer dispositivo com suporte para Windows 10, incluindo PCs, Xbox, Surface Hub e outros dispositivos.

## <a name="prerequisites"></a>Pré-requisitos

Este início rápido requer:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).
* Uma chave de assinatura do Azure para o Serviço de Fala. [Obtenha uma gratuitamente](get-started.md).

## <a name="create-a-visual-studio-project"></a>Criar um projeto do Visual Studio

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-uwp-create-proj.md)]

## <a name="add-sample-code"></a>Adicionar código de exemplo

Agora, adicione o código XAML que define a interface do usuário do aplicativo e adicione a implementação code-behind em C#.

1. No **Gerenciador de Soluções**, abra `MainPage.xaml`.

1. Na exibição XAML do designer, insira o snippet de código XAML a seguir na tag **Grade** (entre `<Grid>` e `</Grid>`):

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. No **Gerenciador de Soluções**, abra o arquivo de origem code-behind `MainPage.xaml.cs`. (Ele está agrupado em `MainPage.xaml`.)

1. Substitua todo o código nele pelo snippet a seguir:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. No manipulador `Speak_ButtonClicked` do arquivo de origem, localize a cadeia de caracteres `YourSubscriptionKey` e substitua-a por sua chave de assinatura.

1. No manipulador `Speak_ButtonClicked`, localize a cadeia de caracteres `YourServiceRegion` e substitua-a pela [região](regions.md) associada à assinatura. (Por exemplo, use `westus` para a assinatura de avaliação gratuita.)

1. Na barra de menus, escolha **Arquivo** > **Salvar Tudo** para salvar suas alterações.

## <a name="build-and-run-the-application"></a>Compile e execute o aplicativo

Agora, você está pronto para criar e testar seu aplicativo.

1. Na barra de menus, escolha **Compilar** > **Compilar Solução** para compilar o aplicativo. Agora, o código deve compilar sem erros.

1. Escolha **Depurar** > **Iniciar Depuração** (ou pressione **F5**) para iniciar o aplicativo. A janela **helloworld** é exibida.

   ![Exemplo de aplicativo de síntese de fala da UWP em C# – início rápido](media/sdk/qs-text-to-speech-uwp-helloworld-window.png)

1. Digite algum texto na caixa de texto e clique em **Fala**. Seu texto é transmitido para os Serviços de Fala e sintetizado em fala. Depois, é reproduzido no alto-falante.

    ![Interface do usuário de síntese de fala](media/sdk/qs-tts-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Explorar amostras de C# no GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Consulte também

- [Criar e usar modelos de Voz Personalizada](how-to-custom-voice-create-voice.md)
- [Amostras de memorando de voz](record-custom-voice-samples.md)
