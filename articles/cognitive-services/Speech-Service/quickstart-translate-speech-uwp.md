---
title: 'Início Rápido: Traduzir fala, C# (UWP) – Serviço de Fala'
titleSuffix: Azure Cognitive Services
description: Neste início rápido, você criará um aplicativo da UWP (Plataforma Universal do Windows) para capturar a fala do usuário, traduzi-la para outro idioma e produzir o texto para a linha de comando. Este guia foi projetado para usuários do Windows.
services: cognitive-services
author: lisaweixu
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.date: 08/19/2019
ms.author: erhopf
ms.topic: quickstart
ms.openlocfilehash: e513cbbc615965ef196a830351aab8ac241c3f20
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70382643"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-c-uwp"></a>Início Rápido: Tradução de fala com o Speech SDK para C# (UWP)

Guias de início rápido também estão disponíveis para o [reconhecimento de fala](quickstart-csharp-uwp.md), a [síntese de fala](quickstart-text-to-speech-csharp-uwp.md) e [o assistente virtual que tem como prioridade o uso de voz](quickstart-virtual-assistant-csharp-uwp.md).

Neste início rápido, você criará um aplicativo da UPW (Plataforma Universal do Windows) que captura a fala do usuário do microfone do seu computador, converte a fala e transcreve o texto traduzido para a linha de comando em tempo real. Este aplicativo foi projetado para ser executado no Windows de 64 bits e é criado com o [pacote NuGet do SDK de Fala](https://aka.ms/csspeech/nuget) e o Microsoft Visual Studio 2019.

Para obter uma lista completa dos idiomas disponíveis para tradução de fala, confira [suporte ao idioma](language-support.md).

> [!NOTE]
> A UWP permite desenvolver aplicativos que executam em qualquer dispositivo com suporte para Windows 10, incluindo PCs, Xbox, Surface Hub e outros dispositivos.

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

   [!code-xml[UI elements](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml#StackPanel)]

1. No **Gerenciador de Soluções**, abra o arquivo de origem code-behind `MainPage.xaml.cs`. (Ele está agrupado em `MainPage.xaml`.)

1. Substitua todo o código nele pelo snippet a seguir:

   [!code-csharp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/speech-translation/csharp-uwp/helloworld/MainPage.xaml.cs#code)]

1. No manipulador `SpeechTranslationFromMicrophone_ButtonClicked` deste arquivo, localize a cadeia de caracteres `YourSubscriptionKey` e substitua-a por sua chave de assinatura.

1. No manipulador `SpeechTranslationFromMicrophone_ButtonClicked`, localize a cadeia de caracteres `YourServiceRegion` e substitua-a pela [região](regions.md) associada à assinatura. (Por exemplo, use `westus` para a assinatura de avaliação gratuita.)

1. Na barra de menus, escolha **Arquivo** > **Salvar Tudo** para salvar suas alterações.

## <a name="build-and-run-the-application"></a>Compile e execute o aplicativo

Agora, você está pronto para criar e testar seu aplicativo.

1. Na barra de menus, selecione **Compilar** > **Compilar solução** para compilar o aplicativo. Agora, o código deve compilar sem erros.

1. Escolha **Depurar** > **Iniciar Depuração** (ou pressione **F5**) para iniciar o aplicativo. A janela **helloworld** é exibida.

   ![Exemplo de aplicativo de tradução de fala da UWP em C# – início rápido](media/sdk/qs-translate-speech-uwp-helloworld-window.png)

1. Selecione **Habilitar Microfone** e, quando a solicitação de permissão de acesso for exibida, selecione **Sim**.

   ![Solicitação de permissão de acesso ao microfone](media/sdk/qs-csharp-uwp-10-access-prompt.png)

1. Selecione **Traduzir fala da entrada do microfone** e fale uma frase ou uma oração em inglês no microfone do dispositivo. O aplicativo transmite sua fala para os serviço de Fala, que a traduz fala em texto para outro idioma (neste caso, alemão). O serviço de Fala envia o texto traduzido de volta para o aplicativo, que exibe a tradução na janela.

   ![Interface do usuário de tradução de fala](media/sdk/qs-translate-csharp-uwp-ui-result.png)

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Explorar amostras de C# no GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Consulte também

- [Treinar um modelo para Fala Personalizada](how-to-custom-speech-train-model.md)
