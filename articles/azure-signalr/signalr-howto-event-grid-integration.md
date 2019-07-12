---
title: Como enviar eventos de serviço do Azure SignalR à grade de eventos
description: Um guia para mostrar a você como habilitar a grade de eventos para seu SignalR Service, em seguida, enviar conexão de cliente conectado/desconectado eventos para um aplicativo de exemplo.
services: azure-signalr
author: chenyl
ms.service: azure-signalr
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: chenyl
ms.openlocfilehash: 2d782306938136ce6d21a331185f591316f58a29
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789170"
---
# <a name="how-to-send-events-from-azure-signalr-service-to-event-grid"></a>Como enviar eventos de serviço do Azure SignalR à grade de eventos

Grade de eventos do Azure é um serviço de roteamento de eventos totalmente gerenciado que fornece o consumo de eventos uniforme usando um modelo pub-sub. Neste guia, você pode usar a CLI do Azure para criar um serviço do Azure SignalR, assinar eventos de conexão e, em seguida, implantar um aplicativo web de exemplo para receber os eventos. Por fim, você pode se conectar e desconectar-se e ver a carga do evento no aplicativo de exemplo.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita][azure-account] antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Os comandos da CLI do Azure neste artigo são formatados para o shell de **Bash**. Se você estiver usando um shell diferente, como PowerShell ou Prompt de Comando, talvez seja necessário ajustar os caracteres de continuação de linha ou as linhas de atribuição de variáveis apropriadamente. Este artigo usa variáveis para minimizar a quantidade de edição de comando necessária.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos do Azure é um contêiner lógico no qual você implanta e gerencia os recursos do Azure. O seguinte [criar grupo de az][az-group-create] comando cria um grupo de recursos denominado *myResourceGroup* no *eastus* região. Se você quiser usar um nome diferente para o grupo de recursos, configure `RESOURCE_GROUP_NAME` para um valor diferente.

```azurecli-interactive
RESOURCE_GROUP_NAME=myResourceGroup

az group create --name $RESOURCE_GROUP_NAME --location eastus
```

## <a name="create-a-signalr-service"></a>Criar um Serviço SignalR

Em seguida, implante um serviço do Azure Signalr no grupo de recursos com os seguintes comandos.
```azurecli-interactive
SIGNALR_NAME=SignalRTestSvc

az signalr create --resource-group $RESOURCE_GROUP_NAME --name $SIGNALR_NAME --sku Free_F1
```

Depois que o SignalR Service tiver sido criado, a CLI do Azure retorna uma saída semelhante à seguinte:

```json
{
  "externalIp": "13.76.156.152",
  "hostName": "clitest.servicedev.signalr.net",
  "hostNamePrefix": "clitest",
  "id": "/subscriptions/28cf13e2-c598-4aa9-b8c8-098441f0827a/resourceGroups/clitest1/providers/Microsoft.SignalRService/SignalR/clitest",
  "location": "southeastasia",
  "name": "clitest",
  "provisioningState": "Succeeded",
  "publicPort": 443,
  "resourceGroup": "clitest1",
  "serverPort": 443,
  "sku": {
    "capacity": 1,
    "family": null,
    "name": "Free_F1",
    "size": "F1",
    "tier": "Free"
  },
  "tags": null,
  "type": "Microsoft.SignalRService/SignalR",
  "version": "1.0"
}

```

## <a name="create-an-event-endpoint"></a>Criar um ponto de extremidade de evento

Nesta seção, você usa um modelo do Resource Manager localizado em um repositório GitHub para implantar um aplicativo Web de exemplo pré-criado no Serviço de Aplicativo do Azure. Posteriormente, você assina os eventos da Grade de Eventos do registro e especifica esse aplicativo como o ponto de extremidade para o qual os eventos são enviados.

Para implantar o aplicativo de exemplo, defina `SITE_NAME` como um nome exclusivo para o aplicativo Web e execute os seguintes comandos. O nome do site deve ser exclusivo no Azure porque faz parte do FQDN (nome de domínio totalmente qualificado) do aplicativo Web. Em uma seção posterior, você navega para o FQDN do aplicativo em um navegador da Web para exibir os eventos do registro.

```azurecli-interactive
SITE_NAME=<your-site-name>

az group deployment create \
    --resource-group $RESOURCE_GROUP_NAME \
    --template-uri "https://raw.githubusercontent.com/Azure-Samples/azure-event-grid-viewer/master/azuredeploy.json" \
    --parameters siteName=$SITE_NAME hostingPlanName=$SITE_NAME-plan
```

Depois que a implantação for bem-sucedida (pode levar alguns minutos), abra um navegador e navegue até seu aplicativo web para verificar se ele está em execução:

`http://<your-site-name>.azurewebsites.net`

[!INCLUDE [event-grid-register-provider-cli.md](../../includes/event-grid-register-provider-cli.md)]

## <a name="subscribe-to-registry-events"></a>Assinar eventos de registro

Na Grade de Eventos, você assina um *tópico* para informar quais eventos quer acompanhar e para onde enviá-los. O seguinte [Criar assinatura de evento az eventgrid][az-eventgrid-event-subscription-create] comando assina o serviço do Azure SignalR você criou e especifica a URL do seu aplicativo web como o ponto de extremidade ao qual ele deve enviar eventos. As variáveis de ambiente que você preencheu nas seções anteriores são reutilizadas aqui, portanto, nenhuma edição é necessária.

```azurecli-interactive
SIGNALR_SERVICE_ID=$(az signalr show --resource-group $RESOURCE_GROUP_NAME --name $SIGNALR_NAME --query id --output tsv)
APP_ENDPOINT=https://$SITE_NAME.azurewebsites.net/api/updates

az eventgrid event-subscription create \
    --name event-sub-signalr \
    --source-resource-id $SIGNALR_SERVICE_ID \
    --endpoint $APP_ENDPOINT
```

Quando a assinatura estiver concluída, você deverá ver uma saída semelhante à seguinte:

```JSON
{
  "deadLetterDestination": null,
  "destination": {
    "endpointBaseUrl": "https://$SITE_NAME.azurewebsites.net/api/updates",
    "endpointType": "WebHook",
    "endpointUrl": null
  },
  "filter": {
    "includedEventTypes": [
      "Microsoft.SignalRService.ClientConnectionConnected",
      "Microsoft.SignalRService.ClientConnectionDisconnected"
    ],
    "isSubjectCaseSensitive": null,
    "subjectBeginsWith": "",
    "subjectEndsWith": ""
  },
  "id": "/subscriptions/28cf13e2-c598-4aa9-b8c8-098441f0827a/resourceGroups/myResourceGroup/providers/Microsoft.SignalRService/SignalR/SignalRTestSvc/providers/Microsoft.EventGrid/eventSubscriptions/event-sub-signalr",
  "labels": null,
  "name": "event-sub-signalr",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "retryPolicy": {
    "eventTimeToLiveInMinutes": 1440,
    "maxDeliveryAttempts": 30
  },
  "topic": "/subscriptions/28cf13e2-c598-4aa9-b8c8-098441f0827a/resourceGroups/myResourceGroup/providers/microsoft.signalrservice/signalr/SignalRTestSvc",
  "type": "Microsoft.EventGrid/eventSubscriptions"
}
```

## <a name="trigger-registry-events"></a>Disparar eventos de registro

Alterne para o modo de serviço para `Serverless Mode` e configurar uma conexão de cliente para o SignalR Service. Você pode tirar [exemplo sem servidor](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/Serverless) como uma referência.

```bash
git clone git@github.com:aspnet/AzureSignalR-samples.git

cd samples/Management

# Start the negotiation server
# Negotiation server is responsible for generating access token for clients
cd NegotitationServer
dotnet user-secrets set Azure:SignalR:ConnectionString "<Connection String>"
dotnet run

# Use a seperate command line
# Start a client
cd SignalRClient
dotnet run
```

## <a name="view-registry-events"></a>Exibir eventos do registro

Agora você conectou um cliente para o SignalR Service. Navegue até o aplicativo web do Visualizador de grade de eventos, e você deverá ver um `ClientConnectionConnected` eventos. Se você encerrar o cliente, você também verá um `ClientConnectionDisconnected` eventos.

<!-- LINKS - External -->
[azure-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[sample-app]: https://github.com/dbarkol/azure-event-grid-viewer

<!-- LINKS - Internal -->
[az-eventgrid-event-subscription-create]: /cli/azure/eventgrid/event-subscription#az-eventgrid-event-subscription-create
[az-group-create]: /cli/azure/group#az-group-create
