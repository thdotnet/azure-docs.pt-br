---
title: Criar um aplicativo Azure IoT Central | Microsoft Docs
description: Crie um novo aplicativo Azure IoT Central. Crie um aplicativo de Avaliação ou de Pagamento Conforme o Uso usando um modelo de aplicativo.
author: viv-liu
ms.author: viviali
ms.date: 06/07/2019
ms.topic: quickstart
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 05b761dbf9f58f6afca6459e28d2fecca9aa7b06
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69881665"
---
# <a name="create-an-azure-iot-central-application-preview-features"></a>Criar um aplicativo do Azure IoT Central (versão prévia dos recursos)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Este início rápido mostra como criar um aplicativo do Azure IoT Central que usa a versão prévia dos recursos, como o IoT Plug and Play.

> [!WARNING]
> As funcionalidades do IoT Plug and Play no Azure IoT Central estão atualmente em versão prévia pública. Não use um aplicativo do IoT Central habilitado para o IoT Plug and Play em cargas de trabalho de produção. Para ambientes de produção, use um aplicativo do IoT Central criado com base em um modelo de aplicativo atual, em disponibilidade geral.

## <a name="create-an-application"></a>Criar um aplicativo

Navegue até a página do [Gerenciador de aplicativos](https://aka.ms/iotcentral) do Azure IoT Central. Em seguida, entre com uma conta pessoal ou uma conta corporativa ou de estudante da Microsoft.

Para começar a criar um novo aplicativo Azure IoT Central, selecione **Novo Aplicativo**. Esse link leva você para a página **Criar um aplicativo**.

![Página Criar um aplicativo do Azure IoT Central](media/quick-deploy-iot-central-pnp/iotcentralcreate.png)

Para criar um aplicativo do Azure IoT Central que inclui a versão prévia dos recursos, como o IoT Plug and Play:

1. Escolha um plano de pagamento:
   - Os aplicativos de **avaliação** são gratuitos por sete dias antes de expirarem. Eles podem ser convertidos em Pagamento Conforme o Uso em qualquer momento antes de expirarem. Se você criar um aplicativo de **Avaliação**, precisará inserir suas informações de contato e escolher se deseja receber informações e dicas da Microsoft.
   - Os aplicativos de **Pagamento Conforme o Uso** são cobrados por dispositivo, com os cinco primeiros dispositivos gratuitos. Se você criar um aplicativo de **Pagamento Conforme o Uso**, precisará escolher o *Directory*, a *Assinatura do Azure* e a *Região*:
      - *Directory* é o AD (Azure Active Directory) para criar o aplicativo. Ele contém identidades de usuário, credenciais e outras informações organizacionais. Se você não tiver um Azure AD, ele será gerado quando você criar uma assinatura do Azure.
      - Uma *Assinatura do Azure* permite que você crie instâncias de serviços do Azure. O IoT Central provisionará recursos em sua assinatura. Se você não tiver uma assinatura do Azure, poderá criar uma na [página de entrada do Azure](https://aka.ms/createazuresubscription). Após criar a assinatura do Azure, navegue de volta para a página **Criar Aplicativo**. A nova assinatura aparece na lista suspensa **Assinatura do Azure**.
      - A *Região* é o local físico onde você deseja criar seu aplicativo. Normalmente, você escolhe a região fisicamente mais próxima de seus dispositivos para obter um desempenho ideal. É possível visualizar as regiões nas quais o Azure IoT Central está disponível, na página [Produtos disponíveis por região](https://azure.microsoft.com/regions/services/). Após escolher uma região, não será mais possível mover posteriormente o aplicativo para uma região diferente.

      Saiba mais sobre preços na [Página de preços da microsoft IoT Central](https://azure.microsoft.com/pricing/details/iot-central/).

1. Escolha um nome amigável do aplicativo, como **IoT Contoso**. O Azure IoT Central gera um prefixo de URL exclusivo para você. Você pode alterar esse prefixo de URL para algo mais fácil de lembrar.

1. Escolha o modelo de **aplicativo de Versão Prévia**. Um modelo de aplicativo pode conter itens predefinidos, como modelos de dispositivo e dashboards, para ajudá-lo a começar.

1. Selecione **Criar** na parte inferior da página.

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você criou um aplicativo do IoT Central que usa a versão prévia dos recursos. A próxima etapa sugerida é:

> [!div class="nextstepaction"]
> [Faça um tour pelo IoT Central](overview-iot-central-tour-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)
