---
title: 'Início Rápido: Sintetizar fala, C++ (Windows) – Serviço de Fala'
titleSuffix: Azure Cognitive Services
description: Saiba como sintetizar fala em C++ no Windows Desktop usando o SDK de Fala
services: cognitive-services
author: yinhew
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 08/24/2019
ms.author: yinhew
ms.openlocfilehash: 686b21d3e02266af77687778c32f0d1ca6d55154
ms.sourcegitcommit: 49c4b9c797c09c92632d7cedfec0ac1cf783631b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2019
ms.locfileid: "70383096"
---
# <a name="quickstart-synthesize-speech-in-c-on-windows-by-using-the-speech-sdk"></a>Início Rápido: Sintetizar fala em C++ no Windows usando o SDK de Fala

Também há inícios rápidos disponíveis para [reconhecimento de fala](quickstart-cpp-windows.md) e [tradução de fala](quickstart-translate-speech-cpp-windows.md).

Neste artigo, você criará um aplicativo de console C++ para o Windows. Você usa os serviços Cognitivos do [SDK de Fala](speech-sdk.md) para sintetizar fala do texto em tempo real e reproduzir a fala no alto-falante do seu PC. O aplicativo é criado com o [Pacote NuGet do SDK de Fala](https://aka.ms/csspeech/nuget) e o Microsoft Visual Studio 2019 (qualquer edição).

Para obter uma lista completa dos idiomas/vozes disponíveis para síntese de fala, confira [suporte ao idioma](language-support.md#text-to-speech).

## <a name="prerequisites"></a>Pré-requisitos

Você precisa de uma chave de assinatura dos Serviços de Fala para concluir este Início Rápido. Obtenha uma gratuitamente. Confira [Experimentar os Serviços de Fala gratuitamente](get-started.md) para obter mais detalhes.

## <a name="create-a-visual-studio-project"></a>Criar um projeto do Visual Studio

[!INCLUDE [Quickstart C++ project](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Adicionar código de exemplo

1. Abra o arquivo de origem **helloworld.cpp**.

1. Substitua todo o código pelo seguinte snippet de código:

   [!code-cpp[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/text-to-speech/cpp-windows/helloworld/helloworld.cpp#code)]

1. No mesmo arquivo, substitua a cadeia de caracteres `YourSubscriptionKey` por sua chave de assinatura.

1. Substitua a cadeia de caracteres `YourServiceRegion` pela [região](regions.md) associada à assinatura (por exemplo, `westus` para a assinatura de avaliação gratuita).

1. Na barra de menus, escolha **Arquivo** > **Salvar tudo**.

## <a name="build-and-run-the-application"></a>Compile e execute o aplicativo

1. Na barra de menus, selecione **Compilar** > **Compilar solução** para compilar o aplicativo. Agora, o código deve compilar sem erros.

1. Escolha **Depurar** > **Iniciar depuração** (ou pressione **F5**) para iniciar o aplicativo **Olá Mundo**.

1. Digite uma frase ou expressão em inglês. O aplicativo transmite o texto para os Serviços de Fala, que enviam a fala sintetizada para o aplicativo para reprodução em seu alto-falante.

   ![Saída do console após uma síntese de fala bem-sucedida](media/sdk/qs-tts-cpp-windows-console-output.png)

## <a name="next-steps"></a>Próximas etapas

Amostras adicionais, como salvar a fala de um arquivo de áudio, estão disponíveis no GitHub.

> [!div class="nextstepaction"]
> [Explorar amostras de C++ no GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Consulte também

- [Criar uma Voz Personalizada](how-to-custom-voice-create-voice.md)
- [Registrar amostras de voz personalizada](record-custom-voice-samples.md)
