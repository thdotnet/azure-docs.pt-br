---
title: Adicionar um dispositivo real a um aplicativo Azure IoT Central | Microsoft Docs
description: Como um operador, adicione um dispositivo real ao aplicativo Azure IoT Central.
author: sarahhubbard
ms.author: sahubbar
ms.date: 08/06/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: peterpr
ms.openlocfilehash: 7191c98cfc522068dbea576a8f81776ae4301da8
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69878857"
---
# <a name="tutorial-add-a-simulated-device-to-your-azure-iot-central-application-preview-features"></a>Tutorial: Adicionar um dispositivo simulado no aplicativo do Azure IoT Central (versão prévia dos recursos)

[!INCLUDE [iot-central-pnp-original](../../includes/iot-central-pnp-original-note.md)]

Este tutorial mostra como adicionar e configurar um dispositivo simulado para o seu aplicativo do Microsoft Azure IoT Central.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Adicionar um novo dispositivo simulado
> * Usar um dispositivo simulado na experiência de construção

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, o construtor deve concluir o primeiro tutorial para construtores para criar o aplicativo Azure IoT Central:

* [Definir um novo tipo de dispositivo](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json) (Obrigatório)

## <a name="add-a-simulated-device"></a>Adicionar um dispositivo simulado

Para adicionar um dispositivo real no seu aplicativo, você deve usar o modelo de dispositivo **Sensor Ambiental** que você criou no tutorial [Definir um novo tipo de dispositivo](tutorial-define-device-type-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json).

1. Para adicionar um novo dispositivo como um operador, escolha **Dispositivos** no menu de navegação à esquerda. A guia **Dispositivos** mostra **Todos os dispositivos** e o modelo de dispositivo **Sensor Ambiental**.

1. Para adicionar um dispositivo de sensor de ambiente simulado, selecione **+ Novo**. Use a **ID do Dispositivo** sugerida ou insira sua própria **ID do Dispositivo** em minúsculas. Você também pode inserir um nome para o novo dispositivo. Alterne a opção **simulado** para **Ativado** e, em seguida, selecione **Criar**.

    ![Dispositivo simulado](./media/tutorial-add-device-pnp/simulated-device.png)

Agora você pode interagir com as exibições que foram criadas pelo construtor para o modelo de dispositivo usando dados simulados.

## <a name="use-a-simulated-device-to-improve-views"></a>Usar um dispositivo simulado para melhorar as exibições

Depois de criar um novo dispositivo simulado, o construtor pode usar esse dispositivo para continuar a melhorar e reforçar as exibições para o modelo de dispositivo.

1. Enquanto estiver na exibição do dispositivo, copie a **ID do Dispositivo** simulado que você criou.

1. Escolha a guia **Modelos de Dispositivo** no menu de navegação à esquerda e selecione o modelo **Sensor Ambiental**.

1. Selecione qualquer uma das exibições que você gostaria de editar ou crie uma nova exibição. Clique em **Configurar visualização do dispositivo**. Aqui você pode escolher entre não ter nenhuma visualização do dispositivo, usando um dispositivo real que pode ser configurado para teste ou selecionado de um dispositivo existente que você adicionou ao IoT Central.

1. Escolha **Selecionar de um dispositivo em execução** e insira a **ID do Dispositivo** simulado que você copiou.

1. Escolha **Aplicar**. Agora você pode ver o mesmo dispositivo simulado em sua experiência de construção de exibições de modelo de dispositivo. Essa exibição é particularmente útil para gráficos e outras visualizações.

    ![Configurar visualização do dispositivo](./media/tutorial-add-device-pnp/configure-preview.png)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

* Adicionar um novo dispositivo simulado
* Usar um dispositivo simulado na experiência de construção

Agora que você conectou um dispositivo simulado ao aplicativo Azure IoT Central, apresentamos algumas das próximas etapas sugeridas.

Como um operador, você pode aprender como:

> [!div class="nextstepaction"]
> [Configurar regras](tutorial-configure-rules.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)

Como um desenvolvedor do dispositivo, você pode aprender como:

> [!div class="nextstepaction"]
> [Conectar um dispositivo MXChip IoT DevKit ao seu aplicativo Azure IoT Central](howto-connect-devkit-pnp.md?toc=/azure/iot-central-pnp/toc.json&bc=/azure/iot-central-pnp/breadcrumb/toc.json)



