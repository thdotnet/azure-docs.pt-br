---
title: Interagir com um dispositivo IoT Plug and Play Preview conectado à solução de IoT do Azure | Microsoft Docs
description: Use o Node.js para se conectar a um dispositivo IoT Plug and Play Preview que está conectado à sua solução de IoT do Azure e interagir com ele.
author: miagdp
ms.author: miag
ms.date: 08/02/2019
ms.topic: quickstart
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 246d3eac8f9d8aff6d603ea8686e430ce0f772ea
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69881585"
---
# <a name="quickstart-interact-with-an-iot-plug-and-play-preview-device-thats-connected-to-your-solution"></a>Início Rápido: Interagir com um dispositivo IoT Plug and Play Preview que está conectado à sua solução

O IoT Plug and Play Preview simplifica a IoT, permitindo que você interaja com as funcionalidades de um dispositivo sem conhecer a implementação do dispositivo subjacente. Este início rápido mostra como usar o Node.js para se conectar a um dispositivo IoT Plug and Play que está conectado à sua solução e controlá-lo.

## <a name="prerequisites"></a>Pré-requisitos

Baixe e instale o Node.js em [nodejs.org](https://nodejs.org).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prepare-an-iot-hub"></a>Preparar um hub IoT

Você também precisará de um hub IoT do Azure em sua assinatura do Azure para concluir este início rápido. Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

Adicione a Extensão IoT do Microsoft Azure para a CLI do Azure:

```azurecli-interactive
az extension add --name azure-cli-iot-ext
```

Use o comando a seguir para criar uma identidade do dispositivo no hub IoT. Substitua **YourIoTHubName** e **YourDevice** pelos nomes reais. Caso não tenha um Hub IoT, siga [estas instruções](../iot-hub/iot-hub-create-using-cli.md) para criar um:

```azurecli-interactive
az iot hub device-identity create --hub-name [YourIoTHubName] --device-id [YourDevice]
```

Execute os seguintes comandos para obter a _cadeia de conexão de dispositivo_ para a dispositivo recém-registrado:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name [YourIoTHubName] --device-id [YourDevice] --output table
```

Execute os seguintes comandos para obter a _cadeia de conexão do hub IoT_ para o hub:

```azurecli-interactive
az iot hub show-connection-string --hub-name [YourIoTHubName] --output table
```

## <a name="connect-your-device"></a>Conectar o dispositivo

Neste início rápido, você usará um sensor ambiental de exemplo que é escrito em Node.js como o dispositivo IoT Plug and Play. As seguintes instruções mostram como instalar e executar o dispositivo:

1. Clone o repositório do GitHub:

    ```cmd/sh
    git clone https://github.com/azure-samples/azure-iot-samples-node
    ```

1. Em um terminal, acesse a pasta raiz do repositório clonado, navegue até a pasta **/azure-iot-samples-node/digital-twins/Quickstarts/Device** e, em seguida, instale todas as dependências executando o seguinte comando:

    ```cmd/sh
    npm install
    ```

1. Configure a _cadeia de conexão do dispositivo_:

    ```cmd/sh
    set DEVICE_CONNECTION_STRING=<your device connection string>
    ```

1. Execute a amostra com o seguinte comando:

    ```cmd/sh
    node sample_device.js
    ```

1. Você verá mensagens indicando que o dispositivo enviou a telemetria e suas propriedades. Agora, o dispositivo está pronto para receber comandos e atualizações de propriedade. Não feche este terminal; você precisará dele mais tarde para confirmar se as amostras de serviço também funcionaram.

## <a name="build-the-solution"></a>Compilar a solução

Neste início rápido, você usará uma solução de IoT de exemplo em Node.js para interagir com o dispositivo de exemplo.

1. Abra outro terminal. Acesse a pasta do repositório clonado e navegue até a pasta do **/azure-iot-samples-node/digital-twins/Quickstarts/Service**. Instale todas as dependências executando o seguinte comando:

    ```cmd/sh
    npm install
    ```

1. Configure a _cadeia de conexão do hub_:

    ```cmd/sh
    set IOTHUB_CONNECTION_STRING=<your hub connection string>
    ```

### <a name="read-a-property"></a>Ler uma propriedade

1. Ao conectar o dispositivo no terminal, você verá a seguinte mensagem:

    ```cmd/sh
    reported state property as online
    ```

1. Abra o arquivo **get_digital_twin.js**. Substitua a `deviceID` pela identificação do dispositivo e salve o arquivo.

1. Acesse o terminal que você abriu para executar a amostra de serviço e execute o seguinte comando:

    ```cmd/sh
    node get_digital_twin.js
    ```

1. Na saída, no componente _environmentalSensor_, você verá que o mesmo estado foi relatado:

    ```JSON
    reported state property as online
    ```

### <a name="update-a-writable-property"></a>Atualizar uma propriedade gravável

1. Abra o arquivo **update_digital_twin_property.js**.

1. No início do arquivo, há um conjunto de constantes definidas com espaços reservados em maiúsculas. Substitua a **deviceID** pela identificação do dispositivo real, atualize as constantes com os seguintes valores e salve o arquivo:

    ```javascript
    const componentName = 'environmentalSensor';
    const propertyName = 'brightness';
    const propertyValue = 60;
    ```

1. Acesse o terminal que você abriu para executar a amostra de serviço e use o seguinte comando para executar a amostra:

    ```cmd/sh
    node update_digital_twin_property.js
    ```

1. No terminal, você verá as informações do gêmeo digital associadas ao dispositivo. Localize o componente _environmentalSensor_ e você verá o novo valor de brilho 60.

    ```json
        "environmentalSensor": {
        "name": "environmentalSensor",
        "properties": {
          "brightness": {
            "reported": {
              "value": 60,
              "desiredState": {
                "code": 200,
                "version": 14,
                "description": "helpful descriptive text"
              }
            },
            "desired": {
              "value": 60
            }
          },
          "state": {
            "reported": {
              "value": "online"
            }
          }
        }
      }
    ```

1. Acesse o terminal do _dispositivo_ e você verá que o dispositivo recebeu a atualização:

    ```cmd/sh
    Received an update for brightness: 60
    updated the property
    ```
2. Volte para o terminal de _serviço_ e execute o comando abaixo novamente para confirmar que a propriedade foi atualizada.
    
    ```cmd/sh
    node get_digital_twin.js
    ```
3. Na saída, no componente environmentalSensor, você verá que o valor de brilho atualizado foi relatado. Observação: pode levar algum tempo para que o dispositivo conclua a atualização. Repita essa etapa até o dispositivo realmente processar a atualização da propriedade.
    
    ```json
      "brightness": {
        "reported": {
          "value": 60,
          }
       }
    ```

### <a name="invoke-a-command"></a>Invocar um comando

1. Abra o arquivo **invoke_command.js**.

1. No início do arquivo, substitua `deviceID` pela identificação do dispositivo real. Atualize as constantes com os seguintes valores e, em seguida, salve o arquivo:

    ```javascript
    const interfaceInstanceName = 'environmentalSensor';
    const commandName = 'blink';
    ```

1. Acesse o terminal que você abriu para executar a amostra de serviço. Use o seguinte comando para executar a amostra:

    ```cmd/sh
    node invoke_command.js
    ```

1. No terminal, o êxito é semelhante à seguinte saída:

    ```cmd/sh
    invoking command blink on component environmentalSensor for device test...
    {
      "result": "helpful response text",
      "statusCode": 200,
      "requestId": "33e536d3-14f7-4105-88f3-629b9933851c",
      "_response": "helpful response text"
    }
    ```

1. Acesse o terminal do _dispositivo_ e você verá que o comando foi confirmado:

    ```cmd/sh
    received command: blink for component: environmentalSensor
    acknowledgement succeeded.
    ```

## <a name="clean-up-resources"></a>Limpar recursos

Se você planeja continuar com os artigos posteriores, mantenha os recursos usados neste início rápido. Caso contrário, exclua os recursos criados para este início rápido a fim de evitar encargos adicionais.

Para excluir o hub e o dispositivo registrado, conclua as seguintes etapas usando a CLI do Azure:

```azurecli-interactive
az group delete --name <Your group name>
```

Para excluir apenas o dispositivo registrado no Hub IoT, conclua as seguintes etapas usando a CLI do Azure:

```azurecli-interactive
az iot hub device-identity delete --hub-name [YourIoTHubName] --device-id [YourDevice]
```

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você aprendeu a conectar um dispositivo IoT Plug and Play a uma solução de IoT. Para saber mais sobre como criar uma solução que interage com os dispositivos IoT Plug and Play, confira:

> [!div class="nextstepaction"]
> [Instruções: Conectar-se a um dispositivo e interagir com ele](howto-develop-solution.md)
