---
title: Conectar um Raspberry Pi ao aplicativo Azure IoT Central (Python) | Microsoft Docs
description: Como um desenvolvedor de dispositivos, como conectar um Raspberry Pi ao seu aplicativo IoT Central do Azure usando o Python.
author: dominicbetts
ms.author: dobett
ms.date: 04/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: c8fd5309f50cfc024083cb8a05d679d04bf112dc
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69972256"
---
# <a name="connect-a-raspberry-pi-to-your-azure-iot-central-application-python"></a>Conectar um Raspberry Pi ao aplicativo Azure IoT Central (Python)

[!INCLUDE [howto-raspberrypi-selector](../../includes/iot-central-howto-raspberrypi-selector.md)]

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

Este artigo descreve como um desenvolvedor de dispositivos conecta um Raspberry Pi ao aplicativo Microsoft Azure IoT Central usando a linguagem de programação Python.

## <a name="before-you-begin"></a>Antes de começar

Para concluir as etapas deste artigo, você precisa dos seguintes componentes:

* Um aplicativo Azure IoT Central criado a partir do modelo de aplicativo de **Devkits de Exemplo**. Para obter mais informações, consulte o [Criar um início rápido de aplicativo](quick-deploy-iot-central.md).
* Um dispositivo Raspberry Pi executando o sistema operacional Raspbian. O Raspberry Pi deve ser capaz de se conectar à Internet. Para obter mais informações, consulte Configurando [o Raspberry Pi](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/3).

> [!TIP]
> Para saber mais sobre como configurar e conectar-se a um dispositivo Raspberry Pi, visite introdução [ao Raspberry Pi](https://projects.raspberrypi.org/en/pathways/getting-started-with-raspberry-pi)

## <a name="sample-devkits-application"></a>Aplicativo **Devkits de exemplo**

Um aplicativo criado a partir do modelo de aplicativo de **Devkits de Exemplo** inclui um modelo de dispositivo **Raspberry Pi** com as características a seguir:

- Telemetria, que inclui as seguintes medidas que o dispositivo coletará:
  - Umidade
  - Temperatura
  - Pressão
  - Magnetômetro (X, Y, Z)
  - Acelerômetro (X, Y, Z)
  - Giroscópio (X, Y, Z)
- Configurações
  - Voltagem
  - Atual
  - Velocidade da ventoinha
  - Alternar IR.
- Propriedades
  - Propriedade do dispositivo número de dado
  - Propriedade de localização da nuvem

Para obter os detalhes completos da configuração do modelo de dispositivo, consulte os [detalhes do modelo de dispositivo do Raspberry Pi](howto-connect-raspberry-pi-python.md#raspberry-pi-device-template-details).

## <a name="add-a-real-device"></a>Adicionar um dispositivo real

No aplicativo IoT Central do Azure, adicione um dispositivo real do modelo de dispositivo **Raspberry Pi** . Anote os detalhes de conexão do dispositivo (**ID do escopo**, **ID do dispositivo**e **chave primária**). Para obter mais informações, consulte [Adicionar um dispositivo real ao aplicativo Azure IoT Central](tutorial-add-device.md).

### <a name="configure-the-raspberry-pi"></a>Configurar o Raspberry Pi

As etapas a seguir descrevem como baixar e configurar o aplicativo Python de exemplo do GitHub. Este aplicativo de exemplo:

* Envia dados de telemetria e valores de propriedade para o Azure IoT Central.
* Responde a alterações de configuração feitas no Azure IoT Central.

1. Conecte-se a um ambiente de Shell em seu Raspberry Pi, seja da área de trabalho do Raspberry Pi ou remotamente usando SSH.

1. Execute o seguinte comando para instalar o IoT Central cliente Python:

    ```sh
    pip install iotc
    ```

1. Baixe o código Python de exemplo:

    ```sh
    curl -O https://raw.githubusercontent.com/Azure/iot-central-firmware/master/RaspberryPi/app.py
    ```

1. Edite `app.py` o arquivo baixado e substitua os `DEVICE_ID`espaços `SCOPE_ID`reservados, `PRIMARY/SECONDARY device KEY` e pelos valores de conexão anotados anteriormente. Salve as alterações.

    > [!TIP]
    > No Shell no Raspberry Pi, você pode usar os editores de texto **nano** ou **vi** .

1. Use o seguinte comando para executar o exemplo:

    ```sh
    python app.py
    ```

    O Raspberry Pi começa a enviar medidas de telemetria para o Azure IoT Central.

1. No aplicativo Azure IoT Central, é possível ver como o código em execução no Raspberry Pi interage com o aplicativo:

    * Na página **Medidas** do dispositivo real, é possível ver a telemetria enviada do Raspberry Pi.
    * Na página **Propriedades** , você pode ver a propriedade de dispositivo **número de matriz** .
    * Na página **configurações** , você pode alterar as configurações no Raspberry Pi, como tensão e velocidade do ventilador. Quando o Raspberry Pi reconhece a alteração, a configuração é mostradacomo sincronizada.

## <a name="raspberry-pi-device-template-details"></a>Detalhes do modelo de dispositivo Raspberry Pi

Um aplicativo criado a partir do modelo de aplicativo de **Devkits de Exemplo** inclui um modelo de dispositivo **Raspberry Pi** com as características a seguir:

### <a name="telemetry-measurements"></a>Medidas de telemetria

| Nome do campo     | Unidades  | Mínimo | Máximo | Casas decimais |
| -------------- | ------ | ------- | ------- | -------------- |
| umidade       | %      | 0       | 100     | 0              |
| temp           | °C     | -40     | 120     | 0              |
| pressão       | hPa    | 260     | 1260    | 0              |
| magnetometerX  | mgauss | -1000   | 1000    | 0              |
| magnetometerY  | mgauss | -1000   | 1000    | 0              |
| magnetometerZ  | mgauss | -1000   | 1000    | 0              |
| accelerometerX | mg     | -2000   | 2000    | 0              |
| accelerometerY | mg     | -2000   | 2000    | 0              |
| accelerometerZ | mg     | -2000   | 2000    | 0              |
| gyroscopeX     | mdps   | -2000   | 2000    | 0              |
| gyroscopeY     | mdps   | -2000   | 2000    | 0              |
| gyroscopeZ     | mdps   | -2000   | 2000    | 0              |

### <a name="settings"></a>Configurações

Configurações numéricas

| Display name | Nome do campo | Unidades | Casas decimais | Mínimo | Máximo | Initial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Voltagem      | setVoltage | Volts | 0              | 0       | 240     | 0       |
| Atual      | setCurrent | Amps  | 0              | 0       | 100     | 0       |
| Velocidade da ventoinha    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

Configurações de alternância

| Display name | Nome do campo | Texto ativado | Texto desativado | Initial |
| ------------ | ---------- | ------- | -------- | ------- |
| IR           | activateIR | LIGAR      | DESLIGAR      | Desativar     |

### <a name="properties"></a>Propriedades

| Tipo            | Display name | Nome do campo | Tipo de dados |
| --------------- | ------------ | ---------- | --------- |
| Propriedade de dispositivo | Número impresso   | dieNumber  | número    |
| Texto            | Location     | localização   | N/D       |

## <a name="next-steps"></a>Próximas etapas

Agora que você aprendeu como conectar um Raspberry Pi ao seu aplicativo IoT Central do Azure, a próxima etapa sugerida é aprender como [configurar um modelo de dispositivo personalizado](howto-set-up-template.md) para seu próprio dispositivo IOT.
