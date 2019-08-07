---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 09/07/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 1e919f22d5dd7975f055f262ec9ba69230aebd17
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780840"
---
Nesta seção, você usa o CLI do Azure para criar uma identidade de dispositivo para este artigo. As IDs de Dispositivo diferenciam maiúsculas de minúsculas.

1. Abra o [Azure Cloud Shell](https://shell.azure.com/).

1. No Azure Cloud Shell, execute o seguinte comando para instalar a extensão de IoT Microsoft Azure para CLI do Azure:

    ```azurecli-interactive
    az extension add --name azure-cli-iot-ext
    ```

2. Crie uma nova identidade de dispositivo `myDeviceId` chamada e recupere a cadeia de conexão do dispositivo com estes comandos:

    ```azurecli-interactive
    az iot hub device-identity create --device-id myDeviceId --hub-name {Your IoT Hub name}
    az iot hub device-identity show-connection-string --device-id myDeviceId --hub-name {Your IoT Hub name} -o table
    ```

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

Anote a cadeia de conexão do dispositivo no resultado. Essa cadeia de conexão é usada pelo aplicativo do dispositivo para se conectar ao seu Hub IoT como um dispositivo.

<!-- images and links -->
