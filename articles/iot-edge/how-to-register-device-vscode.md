---
title: Registrar um novo dispositivo do Visual Studio Code – Azure IoT Edge | Microsoft Docs
description: Use o Visual Studio Code para criar um dispositivo IoT Edge no seu hub IoT do Azure e recuperar a cadeia de conexão
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/03/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: c8fce104d48acc3a562599c65eb15cb0a66657b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66495310"
---
# <a name="register-a-new-azure-iot-edge-device-from-visual-studio-code"></a>Registre um novo dispositivo Azure IoT Edge do Visual Studio Code

Antes de poder usar os dispositivos do IoT com Azure IoT Edge, será necessário registrá-los no hub IoT. Quando você registra um dispositivo, você receberá uma cadeia de caracteres de conexão que pode ser usada para configurar seu dispositivo para cargas de trabalho de IoT Edge.

Este artigo descreve como registrar um novo dispositivo do IoT Edge usando Visual Studio Code (VS Code). Há múltiplas maneiras de fazer mais operações no VS Code. Este artigo usa o Explorer, mas você também pode usar a paleta de comandos para executar as etapas.

## <a name="prerequisites"></a>Pré-requisitos

* Um [Hub IoT](../iot-hub/iot-hub-create-through-portal.md) na assinatura do Azure
* [Visual Studio Code](https://code.visualstudio.com/)
* [Ferramentas de IoT do Microsoft Azure](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) para Visual Studio Code

## <a name="sign-in-to-access-your-iot-hub"></a>Entre para acessar o Hub IoT

É possível usar as extensões do Azure IoT para Visual Studio Code para executar operações com o Hub IoT. Para essas operações trabalhar, você precisará entrar em sua conta do Azure e selecione seu hub IoT.

1. No Visual Studio Code, abra a exibição do **Explorer**.

1. Na parte inferior do Explorer, expanda o **IoT Hub do Azure** seção.

   ![Expanda a seção de Dispositivos do Azure Hub IoT](./media/how-to-register-device-vscode/azure-iot-hub-devices.png)

1. Clique no **...**  no **IoT Hub do Azure** cabeçalho de seção. Se você não visualizar as reticências, clique ou passe o mouse sobre o cabeçalho.

1. Escolha **Selecionar Hub IoT**.

1. Se você não tiver entrado sua conta do Azure, siga os prompts para fazer isso.

1. Selecione sua assinatura do Azure.

1. Selecione seu Hub IoT.

## <a name="create-a-device"></a>Criar um dispositivo

1. No explorador do VS Code, expanda a seção **Dispositivos do Hub IoT do Azure**.

1. Clique em **...** no cabeçalho da seção **Dispositivos do Hub IoT**. Se você não visualizar as reticências, clique ou passe o mouse sobre o cabeçalho.

1. Selecione **Criar dispositivo IoT Edge**.

1. Na caixa de texto que abrir, dê a seu dispositivo um ID.

Na tela de saída, você verá o resultado do comando. A informação do dispositivo é impressa, o que inclui o **deviceId** que você forneceu e o **connectionString** que você consegue usar para conectar seu dispositivo físico ao Hub IoT.

## <a name="view-all-devices"></a>Exibir todos os dispositivos

Todos os dispositivos que se conectam ao hub IoT são listados na **IoT Hub do Azure** seção do Gerenciador de código do Visual Studio. Dispositivos do IoT Edge são distinguíveis de dispositivos de borda não com um ícone diferente e o fato de que o **$edgeAgent** e **$edgeHub** módulos são implantados em cada dispositivo IoT Edge.

   ![Exibir todos os dispositivos do IoT Edge no hub IoT](./media/how-to-register-device-vscode/view-devices.png)

## <a name="retrieve-the-connection-string"></a>Recuperar a cadeia de conexão

Quando estiver pronto para configurar o dispositivo, você precisará da cadeia de conexão que vincula o dispositivo físico à identidade no hub IoT.

1. Clique com botão direito na ID do seu dispositivo na **IoT Hub do Azure** seção.

1. Selecione **Copiar cadeia de conexão de dispositivo**.

   A cadeia de conexão é copiada à sua área de transferência.

Você também pode selecionar **Pegar Informação do Dispositivo** do menu de botão direito para ver todas as informações do dispositivo, incluindo a cadeia de conexão, na janela de saída.

## <a name="next-steps"></a>Próximas etapas

Saiba como [Implantar módulos em um dispositivo com o Visual Studio Code](how-to-deploy-modules-vscode.md).
