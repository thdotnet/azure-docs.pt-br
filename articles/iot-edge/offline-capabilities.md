---
title: Operar dispositivos offline – Azure IoT Edge | Microsoft Docs
description: Saiba como módulos e dispositivos IoT Edge podem operar sem conexão à Internet por longos períodos e como o IoT Edge pode permitir que dispositivos IoT regulares operem offline também.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/04/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 4f3e5c1566271573b43e24a1749b42daa7530555
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67051958"
---
# <a name="understand-extended-offline-capabilities-for-iot-edge-devices-modules-and-child-devices"></a>Entender os recursos estendidos de offline para dispositivos do IoT Edge, módulos e dispositivos filho

O Azure IoT Edge oferece suporte a operações offline estendidas em seus dispositivos IoT Edge e habilita operações offline em não - dispositivos do IoT Edge filho muito. Contanto que um dispositivo do IoT Edge tenha tido uma oportunidade de se conectar ao Hub IoT, ele e todos os dispositivos filho podem continuar a funcionar com uma conexão com a Internet intermitente ou sem nenhuma conexão. 


## <a name="how-it-works"></a>Como ele funciona

Quando um dispositivo do IoT Edge entra em modo offline, o hub do IoT Edge assume três funções. Primeiro, ele armazena todas as mensagens que estão no sentido upstream e as salva até que o dispositivo se reconecte. Em segundo lugar, ele atua em nome do Hub IoT para autenticar dispositivos filho e módulos para que eles possam continuar a operar. Em terceiro lugar, ele permite a comunicação entre os dispositivos filho que normalmente passariam pelo Hub IoT. 

O exemplo a seguir mostra como um cenário de IoT Edge opera no modo offline:

1. **Configurar dispositivos**

   Dispositivos do IoT Edge automaticamente têm recursos offline habilitados. Para estender essa funcionalidade para outros dispositivos de IoT, você precisa declarar uma relação de pai-filho entre os dispositivos no Hub IoT. Em seguida, você pode configurar os dispositivos de filho para confiar em seu dispositivo pai atribuído e as comunicações de dispositivo para a nuvem por meio do pai como um gateway de rota. 

2. **Sincronização com o Hub IoT**

   Pelo menos uma vez após a instalação do tempo de execução do IoT Edge, o dispositivo do IoT Edge precisa estar online para sincronizar com o Hub IoT. Nessa sincronização, o dispositivo do IoT Edge obtém detalhes sobre todos os dispositivos filho atribuídos a ele. O dispositivo do IoT Edge também atualiza seu cache local com segurança para permitir operações offline e recupera as configurações para o armazenamento local de mensagens de telemetria. 

3. **Entrar no modo offline**

   Enquanto estiver desconectado do Hub IoT, o dispositivo do IoT Edge, seus módulos implantados e quaisquer dispositivos de IoT filho poderão operar indefinidamente. Os módulos e dispositivos filho podem ser iniciados e reiniciados ao autenticarem com o hub do IoT Edge enquanto estiverem offline. A telemetria upstream associada para o Hub IoT é armazenada localmente. A comunicação entre os módulos ou entre dispositivos de IoT filho é mantida por meio de métodos diretos ou mensagens. 

4. **Reconecte e ressincronização com o IoT Hub**

   Quando a conexão com o Hub IoT for restaurada, o dispositivo do IoT Edge será sincronizado novamente. As mensagens armazenadas localmente são entregues na mesma ordem em que foram armazenadas. Todas as diferenças entre as propriedades desejadas e relatadas dos módulos e dispositivos são reconciliadas. O dispositivo do IoT Edge atualiza qualquer alteração de seu conjunto de dispositivos de IoT filho atribuídos.

## <a name="restrictions-and-limits"></a>Restrições e limites

Os recursos off-line estendidos descritos neste artigo estão disponíveis no [IoT Edge versão 1.0.4 ou superior](https://github.com/Azure/azure-iotedge/releases). As versões anteriores têm um subconjunto de recursos offline. Os dispositivos do IoT Edge existentes que não têm recursos offline estendidos não podem ser atualizados alterando a versão de tempo de execução, mas devem ser reconfigurados com uma nova identidade do dispositivo do IoT Edge para obter esses recursos. 

O suporte offline estendido está disponível em todas as regiões onde o Hub IoT está disponível, **exceto** leste dos EUA.

Somente os dispositivos não - IoT Edge podem ser adicionados como dispositivos de filho. 

Dispositivos do IoT Edge e seus dispositivos filho atribuídos podem funcionar indefinidamente offline após a sincronização inicial única. No entanto, o armazenamento de mensagens depende da configuração de TTL (vida útil) e do espaço em disco disponível para armazenar as mensagens. 

## <a name="set-up-parent-and-child-devices"></a>Configurar dispositivos pai e filho

Para um dispositivo IoT Edge estender suas funcionalidades offline estendidas para dispositivos de IoT filho, você precisa concluir duas etapas. Primeiro, declare o pai-filho relações no portal do Azure. Em segundo lugar, criar uma relação de confiança entre o dispositivo pai e todos os dispositivos filho e, em seguida, configurar as comunicações de dispositivo para nuvem para percorrer o pai como um gateway. 

### <a name="assign-child-devices"></a>Atribuir dispositivos filho

Dispositivos de filho podem ser qualquer não - dispositivo do IoT Edge registrado para o mesmo IoT Hub. Dispositivos pai podem ter vários dispositivos de filho, mas um dispositivo filho tem apenas um pai. Há três opções para definir os dispositivos de filho para um dispositivo de borda: por meio do portal do Azure, usando a CLI do Azure ou usando o SDK do serviço do IoT Hub. 

As seções a seguir fornecem exemplos de como você pode declarar a relação pai/filho no IoT Hub para dispositivos de IoT existentes. Se você estiver criando novas identidades de dispositivo para o seu filho dispositivos, consulte [autenticar um dispositivo downstream ao IoT Hub do Azure](how-to-authenticate-downstream-device.md) para obter mais informações.

#### <a name="option-1-iot-hub-portal"></a>Opção 1: Portal do Hub IoT

Ao criar um novo dispositivo, você pode declarar a relação pai-filho. Ou, para os dispositivos existentes, você pode declarar a relação da página de detalhes de dispositivo do pai dispositivo IoT Edge ou o dispositivo de IoT filho. 

   ![Gerenciar dispositivos filho da página de detalhes do dispositivo do IoT Edge](./media/offline-capabilities/manage-child-devices.png)


#### <a name="option-2-use-the-az-command-line-tool"></a>Opção 2: Use o `az` ferramenta de linha de comando

Usando o [interface de linha de comando do Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) com [extensão de IoT](https://github.com/azure/azure-iot-cli-extension) (v0.7.0 ou mais recente), você pode gerenciar relações pai-filho com o [identidade do dispositivo](https://docs.microsoft.com/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest) subcomandos. O exemplo a seguir usa uma consulta para atribuir todos os não - dispositivos do IoT Edge no hub para ser dispositivos de filho de um dispositivo IoT Edge. 

```shell
# Set IoT Edge parent device
egde_device="edge-device1"

# Get All IoT Devices
device_list=$(az iot hub query \
        --hub-name replace-with-hub-name \
        --subscription replace-with-sub-name \
        --resource-group replace-with-rg-name \
        -q "SELECT * FROM devices WHERE capabilities.iotEdge = false" \
        --query 'join(`, `, [].deviceId)' -o tsv)

# Add all IoT devices to IoT Edge (as child)
az iot hub device-identity add-children \
  --device-id $egde_device \
  --child-list $device_list \
  --hub-name replace-with-hub-name \
  --resource-group replace-with-rg-name \
  --subscription replace-with-sub-name 
```

Você pode modificar os [consulta](../iot-hub/iot-hub-devguide-query-language.md) para selecionar um subconjunto diferente de dispositivos. O comando pode levar vários segundos se você especificar um grande conjunto de dispositivos.

#### <a name="option-3-use-iot-hub-service-sdk"></a>Opção 3: Usar o SDK do serviço de Hub IoT 

Por fim, você pode gerenciar relações pai-filho por meio de programação usando o C#, Java ou do SDK do serviço de Hub IoT do Node. js. Aqui está uma [exemplo de atribuição de um dispositivo filho](https://aka.ms/set-child-iot-device-c-sharp) usando o C# SDK.

### <a name="set-up-the-parent-device-as-a-gateway"></a>Configurar o dispositivo pai como um gateway

Você pode pensar em uma relação pai/filho como um gateway transparente, em que o dispositivo filho tem sua própria identidade no IoT Hub, mas se comunica por meio da nuvem por meio de seu pai. Para uma comunicação segura, o dispositivo filho precisa ser capaz de verificar se o dispositivo pai vem de uma fonte confiável. Caso contrário, os terceiros poderia configurar dispositivos mal-intencionado para representar os pais e interceptar as comunicações. 

Uma maneira de criar essa relação de confiança é descrita em detalhes nos artigos a seguir: 
* [Configure um dispositivo IoT Edge para atuar como um gateway transparente](how-to-create-transparent-gateway.md)
* [Conectar um dispositivo downstream (filho) para um gateway do Azure IoT Edge](how-to-connect-downstream-device.md)

## <a name="specify-dns-servers"></a>Especificar servidores de DNS 

Para aumentar a robustez, é altamente recomendável que você especificar os endereços de servidor DNS usados em seu ambiente. Consulte as duas opções para [configurar o servidor DNS no artigo de solução de problemas](troubleshoot.md#resolution-7).

## <a name="optional-offline-settings"></a>Configurações offline opcionais

Se os dispositivos ficarem offline, o dispositivo do IoT Edge pai armazena todas as mensagens de dispositivo para a nuvem até que a conexão seja restabelecida. O módulo de hub do IoT Edge gerencia o armazenamento e encaminhamento de mensagens off-line. Para dispositivos que podem ficar offline por longos períodos de tempo, otimize o desempenho por meio da configuração de duas configurações de hub do IoT Edge. 

Primeiro, aumente o tempo de configuração em tempo real para que o hub do IoT Edge mantém mensagens tempo suficiente para seu dispositivo para se reconectar. Em seguida, adicione mais espaço em disco para o armazenamento de mensagens. 

### <a name="time-to-live"></a>Vida útil

A configuração de vida útil é a quantidade de tempo (em segundos) que uma mensagem pode esperar para ser entregue antes de expirar. O padrão é 7200 segundos (duas horas). O valor máximo é limitado somente pelo valor máximo de uma variável de inteiro, que é aproximadamente 2 bilhões. 

Essa configuração é uma propriedade desejada do hub do IoT Edge, que é armazenada no gêmeo do módulo. Você pode configurá-lo no portal do Azure ou diretamente no manifesto de implantação. 

```json
"$edgeHub": {
    "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {},
        "storeAndForwardConfiguration": {
            "timeToLiveSecs": 7200
        }
    }
}
```

### <a name="additional-offline-storage"></a>Armazenamento offline adicional

As mensagens são armazenadas por padrão no sistema de arquivos de contêiner do hub do IoT Edge. Se essa quantidade de armazenamento não for suficiente para suas necessidades offline, você poderá dedicar o armazenamento local no dispositivo do IoT Edge. Crie uma variável de ambiente para o hub do IoT Edge que aponte para uma pasta de armazenamento no contêiner. Em seguida, use as opções de criação para associar essa pasta de armazenamento a uma pasta no computador host. 

Você pode configurar variáveis de ambiente e as opções de criação para o módulo do hub do IoT Edge no portal do Azure na seção **Definir configurações avançadas do tempo de execução do Edge**. Ou, você pode configurá-las diretamente no manifesto de implantação. 

```json
"edgeHub": {
    "type": "docker",
    "settings": {
        "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
        "createOptions": {
            "HostConfig": {
                "Binds": ["<HostStoragePath>:<ModuleStoragePath>"],
                "PortBindings": {
                    "8883/tcp": [{"HostPort":"8883"}],
                    "443/tcp": [{"HostPort":"443"}],
                    "5671/tcp": [{"HostPort":"5671"}]
                }
            }
        }
    },
    "env": {
        "storageFolder": {
            "value": "<ModuleStoragePath>"
        }
    },
    "status": "running",
    "restartPolicy": "always"
}
```

Substitua `<HostStoragePath>` e `<ModuleStoragePath>` pelo seu caminho de armazenamento de host e módulo; o caminho de armazenamento do host e do módulo deve ser um caminho absoluto. Nas opções de criação, associe os caminhos de armazenamento do host e do módulo. Em seguida, crie uma variável de ambiente que aponta para o caminho de armazenamento do módulo.  

Por exemplo, `"Binds":["/etc/iotedge/storage/:/iotedge/storage/"]` significa que o diretório **/etc/iotedge/storage** no host do sistema é mapeado para o diretório **/iotedge/storage/** no contêiner. Ou outro exemplo de sistemas Windows: `"Binds":["C:\\temp:C:\\contemp"]` significa que o diretório **C:\\temp** no sistema do host é mapeado para o diretório **C:\\contemp** no contêiner. 

Você também pode encontrar mais detalhes sobre opções de criação nos [documentos do Docker](https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate).

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre como configurar um gateway transparente para suas conexões de dispositivo pai/filho: 

* [Configure um dispositivo IoT Edge para atuar como um gateway transparente](how-to-create-transparent-gateway.md)
* [Autenticar um dispositivo downstream ao IoT Hub do Azure](how-to-authenticate-downstream-device.md)
* [Conectar um dispositivo downstream a um gateway do Azure IoT Edge](how-to-connect-downstream-device.md)
