---
title: Gerar uma cadeia de conexão do dispositivo para o Azure IoT Central | Microsoft Docs
description: Como desenvolvedor de dispositivos, como faço para gerar uma cadeia de conexão para o dispositivo que precisa se conectar a um aplicativo IoT Central?
author: dominicbetts
ms.author: dobett
ms.date: 08/23/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 97c0332656b75c3c8d0cddecb41c7a15ac2f218c
ms.sourcegitcommit: 80dff35a6ded18fa15bba633bf5b768aa2284fa8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70019790"
---
# <a name="generate-a-device-connection-string-to-connect-to-an-azure-iot-central-application"></a>Gerar uma cadeia de conexão de dispositivo para se conectar a um aplicativo de IoT Central do Azure

Este artigo descreve como, como desenvolvedor de dispositivos, gerar uma cadeia de conexão para o dispositivo que precisa se conectar a um aplicativo IoT Central. O procedimento descrito neste artigo mostra como conectar rapidamente um único dispositivo usando uma SAS (assinatura de acesso compartilhado). Essa abordagem é útil quando você está experimentando IoT Central ou dispositivos de teste. Para obter abordagens alternativas para usar em um ambiente de produção, consulte [conectividade do dispositivo no Azure IOT central](concepts-connectivity.md).

## <a name="prerequisites"></a>Pré-requisitos

Para concluir as etapas neste artigo, você precisa do seguinte:

- Um aplicativo Azure IoT Central. Para obter mais informações, consulte o [Criar um início rápido de aplicativo](quick-deploy-iot-central.md).
- Um computador de desenvolvimento com o [node. js](https://nodejs.org/) versão 8.0.0 ou posterior instalado. É possível executar `node --version` na linha de comando para verificar a versão. O Node.js está disponível para uma ampla variedade de sistemas operacionais.

## <a name="get-connection-information"></a>Obter informações de conexão

As etapas a seguir descrevem como obter as informações necessárias para gerar uma cadeia de conexão SAS para um dispositivo:

1. No **Device Explorer**, encontre o dispositivo real que você deseja conectar ao seu aplicativo:

    ![Selecionar um dispositivo real](media/howto-generate-connection-string/real-devices.png)

1. Na página **dispositivo** , selecione **conectar**:

    ![Selecione conectar](media/howto-generate-connection-string/connect.png)

1. Anote os detalhes da conexão, a **ID do escopo**, a **ID do dispositivo**e a **chave primária do dispositivo**para usar nas seguintes etapas:

    ![Detalhes da conexão](media/howto-generate-connection-string/device-connect.png)

    Você pode copiar os valores desta página para salvar.

## <a name="generate-the-connection-string"></a>Gerar a cadeia de conexão

[!INCLUDE [iot-central-howto-connection-string](../../includes/iot-central-howto-connection-string.md)]

## <a name="next-steps"></a>Próximas etapas

Agora que você gerou uma cadeia de conexão para um dispositivo real se conectar ao aplicativo IoT Central do Azure, aqui estão as próximas etapas sugeridas:

* [Preparar e conectar um dispositivo DevKit (C)](howto-connect-devkit.md)
* [Preparar e conectar um Raspberry Pi (Python)](howto-connect-raspberry-pi-python.md)
* [Preparar e conectar um Raspberry Pi (C#)](howto-connect-raspberry-pi-csharp.md)
* [Preparar e conectar um dispositivo Windows 10 IoT Core (C#)](howto-connect-windowsiotcore.md)
* [Conectar um cliente Node.js genérico ao aplicativo Azure IoT Central](howto-connect-nodejs.md)