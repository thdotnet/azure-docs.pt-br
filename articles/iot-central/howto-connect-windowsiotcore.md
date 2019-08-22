---
title: Conectar um dispositivo Windows IoT Core ao aplicativo Azure IoT Central | Microsoft Docs
description: Como um desenvolvedor de dispositivos, saiba como conectar um dispositivo MXChip IoT DevKit ao aplicativo Azure IoT Central.
author: miriambrus
ms.author: miriamb
ms.date: 04/05/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: dca0146cc16ea63e0621eff8f508f15e0046b63b
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69877397"
---
# <a name="connect-a-windows-iot-core-device-to-your-azure-iot-central-application"></a>Conectar um dispositivo Windows IoT Core ao aplicativo Azure IoT Central

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

Este artigo descreve como um desenvolvedor de dispositivos conecta um dispositivo Windows IoT Core ao aplicativo Microsoft Azure IoT Central.

## <a name="before-you-begin"></a>Antes de começar

Para concluir as etapas neste artigo, você precisa do seguinte:

- Um aplicativo Azure IoT Central criado a partir do modelo de aplicativo de **Devkits de Exemplo**. Para obter mais informações, consulte o [Criar um início rápido de aplicativo](quick-deploy-iot-central.md).

- Um dispositivo executando o sistema operacional Windows 10 IoT Core. Para obter mais informações, consulte Configurando [seu dispositivo Windows 10 IOT Core](https://docs.microsoft.com/windows/iot-core/tutorials/quickstarter/devicesetup).

- Um computador de desenvolvimento com o [node. js](https://nodejs.org/) versão 8.0.0 ou posterior instalado. É possível executar `node --version` na linha de comando para verificar a versão. O Node.js está disponível para uma ampla variedade de sistemas operacionais.

## <a name="the-sample-devkits-application"></a>O aplicativo devkits de exemplo

Um aplicativo criado a partir do modelo de aplicativo de **Devkits de Exemplo** inclui um modelo de dispositivo **Windows IoT Core** com as características a seguir:

- Medições de telemetria para o dispositivo: **Umidade**, **temperatura**e **pressão**.
- Configuração para controlar a **velocidade do ventilador**.
- Um número de **matriz** de propriedade de dispositivo e um **local**de propriedade de nuvem.

Para obter detalhes completos sobre a configuração do modelo de dispositivo, consulte [detalhes do modelo de dispositivo do Windows IOT Core](#device-template-details).

## <a name="add-a-real-device"></a>Adicionar um dispositivo real

No aplicativo IoT Central do Azure, use a página **Device Explorer** para adicionar um dispositivo real do modelo de dispositivo do **Windows 10 IOT Core** . Anote os detalhes de conexão do dispositivo (**ID do escopo**, **ID do dispositivo**e **chave primária**). Para obter mais informações, consulte [obter informações de conexão](howto-generate-connection-string.md#get-connection-information).

## <a name="prepare-the-device"></a>Preparar o dispositivo

Para que o dispositivo se conecte ao IoT Central, ele precisa de uma cadeia de conexão.

[!INCLUDE [iot-central-howto-connection-string](../../includes/iot-central-howto-connection-string.md)]

Para que o código do dispositivo acesse a cadeia de conexão, salve-o em um arquivo chamado **Connection. String. iothub** na pasta `C:\Data\Users\DefaultAccount\Documents\` em seu dispositivo Windows 10 IOT Core.

Para copiar o arquivo **Connection. String. iothub** de seu computador desktop para a `C:\Data\Users\DefaultAccount\Documents\` pasta em seu dispositivo, você pode usar o [portal de dispositivo do Windows](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal):

1. Use seu navegador da Web para navegar até o portal do dispositivo Windows em seu dispositivo.
1. Para procurar os arquivos em seu dispositivo, escolha **aplicativos > explorador de arquivos**.
1. Navegue para o **usuário Folders\Documents**. Em seguida, carregue o arquivo **Connection. String. iothub** :

    ![Carregar cadeia de conexão](media/howto-connect-windowsiotcore/device-portal.png)

## <a name="deploy-and-run"></a>Implantar e executar

Para implantar e executar o aplicativo de exemplo em seu dispositivo, você pode usar o [portal de dispositivo do Windows](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal):

1. Use seu navegador da Web para navegar até o portal do dispositivo Windows em seu dispositivo.
1. Para implantar e executar o aplicativo **cliente do Hub IOT do Azure** , escolha **aplicativos > amostras de execução rápida**. Em seguida, escolha **cliente do Hub IOT do Azure**.
1. Em seguida, escolha **implantar e executar**.

    ![Implantar e executar](media/howto-connect-windowsiotcore/quick-run.png)

Após alguns minutos, você pode exibir a telemetria do seu dispositivo no seu aplicativo IoT Central.

O [portal de dispositivos do Windows](https://docs.microsoft.com/windows/iot-core/manage-your-device/deviceportal) inclui ferramentas que você pode usar para solucionar problemas de seu dispositivo:

- A página **Gerenciador de aplicativos** permite controlar os aplicativos em execução no seu dispositivo.
- Se você não tiver um monitor conectado ao seu dispositivo, poderá usar a página **configurações do dispositivo** para capturar capturas de tela do seu dispositivo. Por exemplo:

    ![Captura de tela do aplicativo](media/howto-connect-windowsiotcore/iot-hub-foreground-client.png)

## <a name="download-the-source-code"></a>Fazer o download do código-fonte

Se você quiser explorar e modificar o código-fonte do aplicativo cliente, poderá baixá-lo no [repositório GitHub Windows-iotcore-Samples](https://github.com/Microsoft/Windows-iotcore-samples/blob/master/Samples/Azure/IoTHubClients).

## <a name="device-template-details"></a>Detalhes do modelo de dispositivo

Um aplicativo criado a partir do modelo de aplicativo de **Devkits de Exemplo** inclui um modelo de dispositivo **Windows IoT Core** com as características a seguir:

### <a name="telemetry-measurements"></a>Medidas de telemetria

| Nome do campo     | Unidades  | Mínimo | Máximo | Casas decimais |
| -------------- | ------ | ------- | ------- | -------------- |
| umidade       | %      | 0       | 100     | 0              |
| temp           | °C     | -40     | 120     | 0              |
| pressão       | hPa    | 260     | 1260    | 0              |

### <a name="settings"></a>Configurações

Configurações numéricas

| Display name | Nome do campo | Unidades | Casas decimais | Mínimo | Máximo | Initial |
| ------------ | ---------- | ----- | -------------- | ------- | ------- | ------- |
| Velocidade da ventoinha    | fanSpeed   | RPM   | 0              | 0       | 1000    | 0       |

### <a name="properties"></a>Propriedades

| Tipo            | Display name | Nome do campo | Tipo de dados |
| --------------- | ------------ | ---------- | --------- |
| Propriedade de dispositivo | Número impresso   | dieNumber  | número    |
| Texto            | Location     | localização   | N/D       |

## <a name="next-steps"></a>Próximas etapas

Agora que você aprendeu como conectar um dispositivo Windows IoT Core ao seu aplicativo IoT Central do Azure, a próxima etapa sugerida é aprender a [configurar um modelo de dispositivo personalizado](howto-set-up-template.md) para seu próprio dispositivo IOT.
