---
title: Mensagens da nuvem para o dispositivo com o Hub IoT do Azure (Nó)| Microsoft Docs
description: Como enviar mensagens da nuvem para o dispositivo para um dispositivo de um Hub IoT do Azure usando os SDKs do IoT do Azure para Node.js. Modifique um aplicativo de dispositivo simulado para receber mensagens da nuvem para o dispositivo e modificar um aplicativo de back-end para enviá-las.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: javascript
ms.topic: conceptual
ms.date: 06/16/2017
ms.openlocfilehash: 35acc1448b662a9b0c08e9d1f91886903444bcb8
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620067"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-node"></a>Enviar mensagens da nuvem para o dispositivo com o Hub IoT (Nó)

[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Introdução

O Hub IoT do Azure é um serviço totalmente gerenciado que ajuda a permitir comunicações bidirecionais confiáveis e seguras entre milhões de dispositivos e um back-end de solução. O [enviar telemetria de um dispositivo para um hub IoT](quickstart-send-telemetry-node.md) início rápido mostra como criar um hub IoT, provisionar uma identidade do dispositivo e codificar um aplicativo de dispositivo simulado que envia mensagens do dispositivo para a nuvem.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Este tutorial se baseia no [enviar telemetria de um dispositivo para um hub IoT](quickstart-send-telemetry-node.md). Ele mostra como:

* Da sua solução de back-end, envie mensagens da nuvem para o dispositivo em um único dispositivo por meio do Hub IoT.
* Receber mensagens da nuvem para o dispositivo em um dispositivo.
* Da sua solução back-end, solicitar uma confirmação de entrega (*comentários*) para mensagens enviadas para um dispositivo do IoT Hub.

Você pode encontrar mais informações sobre as mensagens de nuvem para o dispositivo na [guia do desenvolvedor do IoT Hub](iot-hub-devguide-messaging.md).

Ao fim deste tutorial, você executará dois aplicativos de console do Node.js:

* **SimulatedDevice**, uma versão modificada do aplicativo criado no [enviar telemetria de um dispositivo para um hub IoT](quickstart-send-telemetry-node.md), que se conecta ao hub IoT e recebe mensagens da nuvem para dispositivo.

* **SendCloudToDeviceMessage**, que envia uma mensagem de nuvem para dispositivo para o aplicativo de dispositivo simulado por meio do IoT Hub e recebe sua confirmação de entrega.

> [!NOTE]
> O Hub IoT tem suporte a SDK para várias plataformas de dispositivo e linguagens (incluindo C, Java e Javascript) nos SDKs do dispositivo IoT do Azure. Para obter instruções passo a passo sobre como conectar seu dispositivo ao código deste tutorial e, em geral, ao Hub IoT do Azure, veja o [Centro de Desenvolvedores do IoT do Azure](https://azure.microsoft.com/develop/iot).
>

Para concluir este tutorial, você precisará do seguinte:

* Versão do Node. js 10.0.x ou posterior.
* Uma conta ativa do Azure. (Se você não tiver uma conta, poderá criar uma [conta gratuita](https://azure.microsoft.com/pricing/free-trial) em apenas alguns minutos.)

## <a name="receive-messages-in-the-simulated-device-app"></a>Receber mensagens no aplicativo do dispositivo simulado

Nesta seção, você modificará o aplicativo de dispositivo simulado criado na [enviar telemetria de um dispositivo para um hub IoT](quickstart-send-telemetry-node.md) para receber mensagens da nuvem para dispositivo do hub IoT.

1. Usando um editor de texto, abra o arquivo SimulatedDevice.js.

2. Modifique a função **connectCallback** para lidar com mensagens enviadas do Hub IoT. Neste exemplo, o dispositivo sempre chama a função **complete** para notificar o Hub IoT de que ele processou a mensagem. A nova versão da função **connectCallback** tem a aparência do snippet a seguir:

    ```javascript
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var temperature = 20 + (Math.random() * 15);
            var humidity = 60 + (Math.random() * 20);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', temperature: temperature, humidity: humidity });
            var message = new Message(data);
            message.properties.add('temperatureAlert', (temperature > 30) ? 'true' : 'false');
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
  
   > [!NOTE]
   > Se você usar HTTPS em vez de MQTT ou AMQP como transporte, a instância **DeviceClient** verificará se há mensagens do Hub IoT com pouca frequência (menos de cada 25 minutos). Para obter mais informações sobre as diferenças entre o suporte do MQTT, AMQP e HTTPS e a limitação do Hub IoT, consulte o [guia do desenvolvedor do IoT Hub](iot-hub-devguide-messaging.md).
   >

## <a name="send-a-cloud-to-device-message"></a>Envie uma mensagem da nuvem para o dispositivo

Nesta seção, você criará um aplicativo do console do Node.js que envia mensagens da nuvem ao dispositivo para o aplicativo do dispositivo simulado. Você precisa da ID de dispositivo do dispositivo que você adicionou na [enviar telemetria de um dispositivo para um hub IoT](quickstart-send-telemetry-node.md) guia de início rápido. A cadeia de conexão do Hub IoT também é necessária e você pode encontrá-la no [Portal do Azure](https://portal.azure.com).

1. Crie uma pasta vazia denominada **sendcloudtodevicemessage**. Na pasta **sendcloudtodevicemessage**, crie um arquivo package.json usando o comando a seguir no prompt de comando. Aceite todos os padrões:

    ```shell
    npm init
    ```

2. No prompt de comando, na pasta **sendcloudtodevicemessage**, execute o seguinte comando para instalar o pacote **azure-iothub**:

    ```shell
    npm install azure-iothub --save
    ```

3. Usando um editor de texto, crie um arquivo **SendCloudToDeviceMessage.js** na pasta **sendcloudtodevicemessage**.

4. Adicione as seguintes instruções `require` no início do arquivo **SendCloudToDeviceMessage.js** :

    ```javascript
    'use strict';

    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```

5. Adicione o seguinte código ao arquivo **SendCloudToDeviceMessage.js** . Substitua o valor de espaço reservado "{iot hub conexão string}" com a cadeia de caracteres de conexão do IoT Hub para o hub que você criou na [enviar telemetria de um dispositivo para um hub IoT](quickstart-send-telemetry-node.md) guia de início rápido. Substitua o espaço reservado "{device id}" com a ID de dispositivo do dispositivo que você adicionou na [enviar telemetria de um dispositivo para um hub IoT](quickstart-send-telemetry-node.md) guia de início rápido:

    ```javascript
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';

    var serviceClient = Client.fromConnectionString(connectionString);
    ```

6. Adicione a seguinte função para imprimir os resultados da operação no console:

    ```javascript
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Adicione a seguinte função para imprimir mensagens de comentários de entrega para o console:

    ```javascript
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```

8. Adicione o seguinte código para enviar uma mensagem ao dispositivo e lidar com a mensagem de comentário quando o dispositivo reconhecer a mensagem da nuvem para o dispositivo:

    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```

9. Salve e feche o arquivo **SendCloudToDeviceMessage.js** .

## <a name="run-the-applications"></a>Executar os aplicativos

Agora você está pronto para executar os aplicativos.

1. No prompt de comando na pasta **simulateddevice**, execute o seguinte comando para enviar telemetria ao Hub IoT e escutar mensagens da nuvem para o dispositivo:

    ```shell
    node SimulatedDevice.js
    ```

    ![Executar um aplicativo de dispositivo simulado](./media/iot-hub-node-node-c2d/receivec2d.png)

2. No prompt de comando na pasta **sendcloudtodevicemessage**, execute o comando a seguir para enviar uma mensagem da nuvem para o dispositivo e esperar os comentários de confirmação:

    ```shell
    node SendCloudToDeviceMessage.js
    ```

    ![Execute o aplicativo para enviar o comando da nuvem para o dispositivo](./media/iot-hub-node-node-c2d/sendc2d.png)

   > [!NOTE]
   > Para simplificar, este tutorial não implementa nenhuma política de repetição. No código de produção, implemente políticas de repetição (como uma retirada exponencial), conforme sugerido no artigo [Tratamento de falhas transitórias](/azure/architecture/best-practices/transient-faults).
   >

## <a name="next-steps"></a>Próximas etapas

Neste tutorial você aprendeu a enviar e receber mensagens da nuvem para o dispositivo.

Para ver exemplos de soluções completas que usam o Hub IoT, consulte [Acelerador de solução de Monitoramento Remoto do Azure IoT](https://azure.microsoft.com/documentation/suites/iot-suite/).

Para saber mais sobre como desenvolver soluções com o Hub IoT, consulte o [Guia do desenvolvedor do Hub IoT](iot-hub-devguide.md).
