---
title: Tutorial – como usar os aprimoramentos de mensagem do IoT Hub do Azure
description: Tutorial que mostra como usar os aprimoramentos de mensagem para mensagens do IoT Hub do Azure
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: robinsh
ms.openlocfilehash: e4906bf9f2aead69c315ddb7b2e3b10489378d87
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66259068"
---
# <a name="tutorial-using-azure-iot-hub-message-enrichments-preview"></a>Tutorial: Usando os aprimoramentos de mensagem do IoT Hub do Azure (visualização)

*Aprimoramentos da mensagem* é a capacidade do IoT Hub *carimbo de data /* mensagens com informações adicionais antes que as mensagens são enviadas ao ponto de extremidade designado. Um motivo para usar os aprimoramentos de mensagem é para incluir dados que podem ser usados para simplificar o processamento de downstream. Por exemplo, enriquecer mensagens de telemetria do dispositivo com uma marca de gêmeo de dispositivo pode reduzir a carga sobre os clientes façam chamadas de API para obter essas informações do dispositivo gêmeo. Para obter mais informações, consulte o [visão geral dos aprimoramentos de mensagem](iot-hub-message-enrichments-overview.md).

Neste tutorial, você deve usar a CLI do Azure para configurar os recursos, incluindo dois pontos de extremidade que apontam para dois contêineres de armazenamento diferentes – **enriquecida** e **original**. Em seguida, você usa o [portal do Azure](https://portal.azure.com) para configurar os aprimoramentos de mensagem a ser aplicado somente às mensagens enviadas para o ponto de extremidade com o **enriquecida** contêiner de armazenamento. Você enviar mensagens ao IoT Hub, que são roteadas para ambos os contêineres de armazenamento. Somente as mensagens enviadas para o ponto de extremidade para o **enriquecida** contêiner de armazenamento será enriquecido.

Aqui estão as tarefas que serão executadas para concluir este tutorial:

**Usando os aprimoramentos de mensagem do IoT Hub**
> [!div class="checklist"]
> * Usando a CLI do Azure, crie os recursos – um hub IoT, uma conta de armazenamento com dois pontos de extremidade e a configuração de roteamento.
> * Use o portal do Azure para configurar os aprimoramentos de mensagem.
> * Execute um aplicativo que simula um dispositivo de IoT, enviar mensagens ao hub.
> * Exibir os resultados e verifique se que os aprimoramentos de mensagem estão funcionando conforme o esperado.

## <a name="prerequisites"></a>Pré-requisitos

* Você precisa ter uma assinatura do Azure. Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

* Instalar o [Visual Studio](https://www.visualstudio.com/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="retrieve-the-sample-code"></a>Recuperar o código de exemplo

Baixe [simulação de dispositivo IoT](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) e descompacte-o. Esse repositório possui vários aplicativos, incluindo o que você usará para enviar mensagens ao hub IoT.

Esse download também contém o script para criar os recursos usados para testar os aprimoramentos de mensagem. O script está em /azure-iot-samples-csharp/iot-hub/Tutorials/Routing/SimulatedDevice/resources/iothub_msgenrichment_cli.azcli. Por enquanto, você pode observar o script e usá-lo. Você também pode copiar o script diretamente do artigo.

Quando você estiver pronto para começar a testar, você usará o aplicativo de simulação de dispositivo deste download para enviar mensagem ao hub IoT.

## <a name="set-up-and-configure-resources"></a>Instalar e configurar recursos

Além de criar os recursos necessários, o script de CLI do Azure também configura as duas rotas para os pontos de extremidade que são contêineres de armazenamento separada. Para obter mais informações sobre como configurar o roteamento, consulte o [roteamento tutorial](tutorial-routing.md). Depois de configurar os recursos, você usar o [portal do Azure](https://portal.azure.com) para configurar os aprimoramentos de mensagem para cada ponto de extremidade e, em seguida, vá para a etapa de teste.

> [!NOTE]
> Todas as mensagens são roteadas para os pontos de extremidade, mas apenas as mensagens que passam para o ponto de extremidade com aprimoramentos de mensagem configurado serão enriquecidas.
>

Você pode usar o script a seguir, ou abra o script na pasta /recursos do repositório baixado. Aqui estão as etapas que executará o script:

* Crie um Hub IoT.
* Criar uma conta de armazenamento.
* Crie dois contêineres na conta de armazenamento – uma para as mensagens enriquecidas e outra para mensagens que não são aprimoradas.
* Configure o roteamento para as duas contas de armazenamento diferente.
    * Crie um ponto de extremidade para cada contêiner da conta de armazenamento.
    * Crie uma rota para cada um dos pontos de extremidade do contêiner conta de armazenamento.

Há vários nomes de recurso que devem ser globalmente exclusivos, como o nome do Hub IoT e o nome da conta de armazenamento. Para tornar mais fácil a execução do script, esses nomes de recurso são acrescentados com um valor alfanumérico aleatório chamado *randomValue*. O randomValue é gerado uma vez na parte superior do script e acrescentado aos nomes de recursos conforme necessário em todo o script. Se você não quiser que ele seja aleatório, defina uma cadeia de caracteres vazia ou como um valor específico.

Se você ainda não fez isso, abra uma [janela do Cloud Shell do Bash.](https://shell.azure.com). Abra o script no repositório descompactado, use Ctrl-A para selecionar tudo e, em seguida, Ctrl-C para copiá-lo. Como alternativa, você pode copiar o script CLI a seguir ou abri-lo diretamente no cloud shell. Cole o script na janela do shell de nuvem do Azure clicando duas vezes na linha de comando e selecionando **colar**. O script é executado uma instrução por vez. Depois que o script terminar a execução, selecione **Enter** para garantir que ele é executado o último comando. O bloco de código a seguir mostra o script que é usado, com comentários explicando o que está fazendo.

Aqui estão os recursos criados pelo script. **Enriquecida** significa que o recurso é para mensagens com aprimoramentos. **Original** significa que o recurso é para mensagens que não são aprimoradas.

| NOME | Value |
|-----|-----|
| resourceGroup | ContosoResourcesMsgEn |
| Nome do contêiner | original  |
| Nome do contêiner | aprimorados  |
| Nome do dispositivo IoT | Dispositivo de teste de Contoso |
| Nome do Hub IoT | ContosoTestHubMsgEn |
| Nome da conta de armazenamento | contosostorage |
| ponto de extremidade 1 nome | ContosoStorageEndpointOriginal |
| ponto de extremidade 2 nome | ContosoStorageEndpointEnriched|
| 1 de nome de rota | ContosoStorageRouteOriginal |
| 2 de nome de rota | ContosoStorageRouteEnriched |

```azurecli-interactive
# This command retrieves the subscription id of the current Azure account.
# This field is used when setting up the routing rules.
subscriptionID=$(az account show --query id -o tsv)

# Concatenate this number onto the resources that have to be globally unique.
# You can set this to "" or to a specific value if you don't want it to be random.
# This retrieves a random value.
randomValue=$RANDOM

# This command installs the IOT Extension for Azure CLI.
# You only need to install this the first time.
# You need it to create the device identity.
az extension add --name azure-cli-iot-ext

# Set the values for the resource names that
#   don't have to be globally unique.
location=westus2
resourceGroup=ContosoResourcesMsgEn
containerName1=original
containerName2=enriched
iotDeviceName=Contoso-Test-Device

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique,
#   so add a random value to the end.
iotHubName=ContosoTestHubMsgEn$randomValue
echo "IoT hub name = " $iotHubName

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# You need a storage account that will have two containers
#   -- one for the original messages and
#   one for the enriched messages.
# The storage account name must be globally unique,
#   so add a random value to the end.
storageAccountName=contosostorage$randomValue
echo "Storage account name = " $storageAccountName

# Create the storage account to be used as a routing destination.
az storage account create --name $storageAccountName \
    --resource-group $resourceGroup \
    --location $location \
    --sku Standard_LRS

# Get the primary storage account key.
#    You need this to create the containers.
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroup \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"')

# See the value of the storage account key.
echo "storage account key = " $storageAccountKey

# Create the containers in the storage account.
az storage container create --name $containerName1 \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

az storage container create --name $containerName2 \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
# If you are using Cloud Shell, you can scroll the window back up to retrieve this value.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName

##### ROUTING FOR STORAGE #####

# You're going to have two routes and two endpoints.
# One points to container1 in the storage account
#   and includes all messages.
# The other points to container2 in the same storage account
#   and only includes enriched messages.

endpointType="azurestoragecontainer"
endpointName1="ContosoStorageEndpointOriginal"
endpointName2="ContosoStorageEndpointEnriched"
routeName1="ContosoStorageRouteOriginal"
routeName2="ContosoStorageRouteEnriched"

# for both endpoints, retrieve the messages going to storage
condition='level="storage"'

# Get the connection string for the storage account.
# Adding the "-o tsv" makes it be returned without the default double quotes around it.
storageConnectionString=$(az storage account show-connection-string \
  --name $storageAccountName --query connectionString -o tsv)

# Create the routing endpoints and routes.
# Set the encoding format to either avro or json.

# This is the endpoint for container 1, for endpoint messages that are not enriched.
az iot hub routing-endpoint create \
  --connection-string $storageConnectionString \
  --endpoint-name $endpointName1 \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --container $containerName1 \
  --resource-group $resourceGroup \
  --encoding json

# This is the endpoint for container 2, for endpoint messages that are enriched.
az iot hub routing-endpoint create \
  --connection-string $storageConnectionString \
  --endpoint-name $endpointName2 \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --container $containerName2 \
  --resource-group $resourceGroup \
  --encoding json

# This is the route for messages that are not enriched.
# Create the route for the first storage endpoint.
az iot hub route create \
  --name $routeName1 \
  --hub-name $iotHubName \
  --source devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName1 \
  --enabled \
  --condition $condition

# This is the route for messages that are not enriched.
az iot hub route create \
  --name $routeName2 \
  --hub-name $iotHubName \
  --source devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName2 \
  --enabled \
  --condition $condition
```

Neste ponto, os recursos são definidos para cima e o roteamento é configurado. Você pode exibir a configuração de roteamento de mensagem no portal e configurar os aprimoramentos de mensagem para mensagens enviadas para o **enriquecida** contêiner de armazenamento.

### <a name="view-routing-and-configure-the-message-enrichments"></a>Exibir o roteamento e configurar os aprimoramentos de mensagem

1. Vá ao IoT Hub selecionando **grupos de recursos**, em seguida, selecione o grupo de recursos definido para este tutorial (**ContosoResources_MsgEn**). Localize o IoT Hub na lista e selecioná-lo. Selecione *roteamento de mensagens** para o Hub Iot.

   ![Selecione o roteamento de mensagens](./media/tutorial-message-enrichments/select-iot-hub.png)

   A mensagem de roteamento painel tem três guias – **rotas**, **pontos de extremidade personalizados**, e **enriquecer mensagens**. Você pode procurar as duas primeiras guias para ver a configuração configurar pelo script. Use a terceira guia para adicionar aprimoramentos de mensagem. Vamos enriquecer mensagens enviadas para o ponto de extremidade para o contêiner de armazenamento chamado **enriquecida**. Preencha o nome e valor e, em seguida, selecione o ponto de extremidade **ContosoStorageEndpointEnriched** na lista suspensa. Aqui está um exemplo de como configurar um aprimoramento que adiciona o nome do IoT Hub à mensagem:

   ![Adicionar o enriquecimento da primeira](./media/tutorial-message-enrichments/add-message-enrichments.png)

2. Adicione esses valores para a lista para o ponto de extremidade ContosoStorageEndpointEnriched.

   | NOME | Value | Ponto de extremidade (lista suspensa) |
   | ---- | ----- | -------------------------|
   | myIotHub | $iothubname | AzureStorageContainers > ContosoStorageEndpointEnriched |
   | Localização do dispositivo | $twin.tags.location | AzureStorageContainers > ContosoStorageEndpointEnriched |
   |customerID | 6ce345b8-1e4a-411e-9398-d34587459a3a | AzureStorageContainers > ContosoStorageEndpointEnriched |

   > [!NOTE]
   > Se seu dispositivo não tiver um gêmeo, o valor que você coloca aqui será marcado como uma cadeia de caracteres para o valor em aprimoramentos de mensagem. Para ver o dispositivo gêmeos informações, vá para seu hub no portal, em seguida, selecione **dispositivos IoT**, selecione seu dispositivo e, em seguida, selecione **dispositivo gêmeo** na parte superior da página.
   >
   > Você pode editar as informações de gêmeo para adicionar marcas (como local) e defini-lo como um valor específico, se você quiser. Para saber mais, veja [Noções básicas e uso de dispositivos gêmeos no Hub IoT](iot-hub-devguide-device-twins.md)

3. Quando tiver terminado, o painel deve ser semelhante a esta imagem:

   ![Tabela com todos os aprimoramentos adicionados](./media/tutorial-message-enrichments/all-message-enrichments.png)

4. Selecione **aplicar** para salvar as alterações.

## <a name="send-messages-to-the-iot-hub"></a>Enviar mensagens ao IoT Hub

Agora que os aprimoramentos de mensagem são configurados para o ponto de extremidade, execute o aplicativo de dispositivo simulado para enviar mensagens ao IoT Hub. O hub foi configurado com regras que realizar o seguinte:

* As mensagens roteadas para o ponto de extremidade do armazenamento ContosoStorageEndpointOriginal não será ser aprimoradas e serão armazenadas no contêiner de armazenamento `original`.

* As mensagens roteadas para o ponto de extremidade do armazenamento ContosoStorageEndpointEnriched serão enriquecidas e armazenadas no contêiner de armazenamento `enriched`.

O aplicativo de dispositivo simulado é um dos aplicativos no download descompactado. O aplicativo envia mensagens para cada um dos métodos de roteamento de mensagem diferente na [roteamento Tutorial](tutorial-routing.md); isso inclui o armazenamento do Azure.

Clique duas vezes no arquivo de solução (IoT_SimulatedDevice.sln) para abrir o código no Visual Studio, em seguida, abra o Program.cs. SUBSTITUTE `{your hub name}` com o nome do hub IoT. É o formato de nome de host do hub IoT **seu nome de hub}. Azure-Devices.NET**. Para este tutorial, é o nome de host do hub **ContosoTestHubMsgEn.azure Azure-Devices.NET**. Em seguida, substitua `{device key}` com a chave do dispositivo que você salvou anteriormente ao executar o script para criar os recursos.

Se você não tiver a chave do dispositivo, você pode recuperá-la a partir do portal. Depois de entrar, vá para **grupos de recursos**, selecione o grupo de recursos e selecione seu IoT Hub. Procure **dispositivos IoT** para seu dispositivo de teste e selecione seu dispositivo. Selecione o ícone de cópia lado **Primary key** copiá-lo para a área de transferência.

   ```csharp
        static string myDeviceId = "contoso-test-device";
        static string iotHubUri = "ContosoTestHubMsgEn.azure-devices.net";
        // This is the primary key for the device. This is in the portal.
        // Find your IoT hub in the portal > IoT devices > select your device > copy the key.
        static string deviceKey = "{your device key here}";
   ```

## <a name="run-and-test"></a>Executar e testar

Execute o aplicativo de console. Aguarde alguns minutos. As mensagens que estão sendo enviadas são exibidas na tela do aplicativo de console.

Esse aplicativo envia uma nova mensagem de dispositivo para a nuvem ao Hub IoT a cada segundo. A mensagem contém um objeto serializado para JSON com a ID do dispositivo, temperatura, umidade e nível de mensagem, cujo padrão é `normal`. Atribui um nível de aleatoriamente `critical` ou `storage`, fazendo com que a mensagem a ser roteado para a conta de armazenamento ou para o ponto de extremidade padrão. As mensagens enviadas para o **enriquecida** contêiner na conta de armazenamento será enriquecida.

Após terem sido enviadas várias mensagens de armazenamento, exiba os dados.

1. Selecione **grupos de recursos**, em seguida, localize seu grupo de recursos (ContosoResourcesMsgEn) e selecioná-lo.

2. Selecione sua conta de armazenamento (contosostorage). Em seguida, selecione **Gerenciador de armazenamento (visualização)** do painel de seleção à esquerda.

   ![Selecione o Gerenciador de armazenamento](./media/tutorial-message-enrichments/select-storage-explorer.png)

   Selecione **CONTÊINERES de BLOB** para ver os dois contêineres que podem ser usados.

   ![Ver os contêineres na conta de armazenamento](./media/tutorial-message-enrichments/show-blob-containers.png)

As mensagens no contêiner chamado **enriquecida** têm os aprimoramentos de mensagem incluídos nas mensagens. As mensagens no contêiner chamado **original** terão as mensagens brutas com aprimoramentos de nenhum. Fazer drill down em um dos contêineres de até você chega à parte inferior, abra o arquivo de mensagem mais recente e faça o mesmo para outro contêiner verificar que não há nenhum aprimoramentos adicionados às mensagens nesse contêiner.

Quando você examinar as mensagens que têm sido aprimoradas, você deverá ver "My IoT Hub" com o nome do hub, bem como o local e a ID do cliente, como este:

```json
{"EnqueuedTimeUtc":"2019-05-10T06:06:32.7220000Z","Properties":{"level":"storage","my IoT Hub":"contosotesthubmsgen3276","device location":"$twin.tags.location","customerID":"6ce345b8-1e4a-411e-9398-d34587459a3a"},"SystemProperties":{"connectionDeviceId":"Contoso-Test-Device","connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}","connectionDeviceGenerationId":"636930642531278483","enqueuedTime":"2019-05-10T06:06:32.7220000Z"},"Body":"eyJkZXZpY2VJZCI6IkNvbnRvc28tVGVzdC1EZXZpY2UiLCJ0ZW1wZXJhdHVyZSI6MjkuMjMyMDE2ODQ4MDQyNjE1LCJodW1pZGl0eSI6NjQuMzA1MzQ5NjkyODQ0NDg3LCJwb2ludEluZm8iOiJUaGlzIGlzIGEgc3RvcmFnZSBtZXNzYWdlLiJ9"}
```

E aqui está uma mensagem unenriched. "" Meu IoT Hub","Local do dispositivo"e"customerID"não aparecem aqui, porque esse ponto de extremidade não tem aprimoramentos de nenhum.

```json
{"EnqueuedTimeUtc":"2019-05-10T06:06:32.7220000Z","Properties":{"level":"storage"},"SystemProperties":{"connectionDeviceId":"Contoso-Test-Device","connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}","connectionDeviceGenerationId":"636930642531278483","enqueuedTime":"2019-05-10T06:06:32.7220000Z"},"Body":"eyJkZXZpY2VJZCI6IkNvbnRvc28tVGVzdC1EZXZpY2UiLCJ0ZW1wZXJhdHVyZSI6MjkuMjMyMDE2ODQ4MDQyNjE1LCJodW1pZGl0eSI6NjQuMzA1MzQ5NjkyODQ0NDg3LCJwb2ludEluZm8iOiJUaGlzIGlzIGEgc3RvcmFnZSBtZXNzYWdlLiJ9"}
```

## <a name="clean-up-resources"></a>Limpar recursos

Se você quiser remover todos os recursos que você criou neste tutorial, exclua o grupo de recursos. Essa ação também exclui todos os recursos contidos no grupo. Nesse caso, ela remove o hub IoT, a conta de armazenamento e o próprio grupo de recursos.

### <a name="use-the-azure-cli-to-clean-up-resources"></a>Usar a CLI do Azure para limpar os recursos

Para remover o grupo de recursos, use o comando [excluir grupo az](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete). `$resourceGroup` foi definido como **ContosoResources** novamente no início deste tutorial.

```azurecli-interactive
az group delete --name $resourceGroup
```

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, configurado e testado adicionando aprimoramentos de mensagem para mensagens do IoT Hub usando as seguintes etapas:

**Usando os aprimoramentos de mensagem do IoT Hub**
> [!div class="checklist"]
> * Usando a CLI do Azure, crie os recursos – um hub IoT, uma conta de armazenamento com duas enendpoints e a configuração de roteamento.
> * Use o portal do Azure para configurar os aprimoramentos de mensagem.
> * Execute um aplicativo que simula uma mensagem de envio do dispositivo IoT para o hub.
> * Exibir os resultados e verifique se que os aprimoramentos de mensagem estão funcionando conforme o esperado.

Para obter mais informações sobre aprimoramentos de mensagem, consulte a [visão geral dos aprimoramentos de mensagem](iot-hub-message-enrichments-overview.md).

Para obter mais informações sobre o roteamento de mensagens, consulte estes artigos:

* [Usar o roteamento de mensagens do IoT Hub para enviar mensagens do dispositivo para nuvem para pontos de extremidade diferentes](iot-hub-devguide-messages-d2c.md)

* [Tutorial: Roteamento do IoT Hub](tutorial-routing.md)