---
title: Reagir a eventos de serviço do Azure SignalR
description: Use a grade de eventos do Azure para assinar eventos de serviço do Azure SignalR.
services: azure-signalr,event-grid
author: chenyl
ms.author: chenyl
ms.reviewer: zhshang
ms.date: 06/12/2019
ms.topic: conceptual
ms.service: azure-signalr
ms.openlocfilehash: 02f88c5953d499b30f2ea3408318f70a72f42b0f
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789183"
---
# <a name="reacting-to-azure-signalr-service-events"></a>Reagir a eventos de serviço do Azure SignalR

Eventos do Azure SignalR Service permitem que aplicativos reajam a conexões de cliente conectado ou desconectado usando modernas arquiteturas sem servidor. Isso é feito sem a necessidade de código complicado ou serviços de sondagem caros e ineficientes.  Em vez disso, os eventos são enviados por push pela [Grade de Eventos do Azure](https://azure.microsoft.com/services/event-grid/) aos assinantes como [Azure Functions](https://azure.microsoft.com/services/functions/), [Aplicativos Lógicos do Azure](https://azure.microsoft.com/services/logic-apps/), ou até mesmo seu próprio ouvinte http personalizado, e você só pague pelo que usa.

Eventos do Azure SignalR Service confiável são enviados para o serviço de grade de eventos que fornece serviços de entrega confiável para seus aplicativos por meio de rich novamente as políticas e entrega de inatividade. Para obter mais informações, consulte [entrega de mensagens da grade de eventos e tente novamente](https://docs.microsoft.com/azure/event-grid/delivery-and-retry).

![Modelo da Grade de Eventos](https://docs.microsoft.com/azure/event-grid/media/overview/functional-model.png)

## <a name="serverless-state"></a>Estado sem servidor
Eventos do Azure SignalR Service estão ativos somente quando as conexões de cliente estão em estado sem servidor. Em termos gerais, se um cliente não encaminha a um servidor de hub, ele entra no estado sem servidor. Trabalho de modo clássico somente quando o hub, o que as conexões de cliente se conectar, não tem um servidor de hub. No entanto, o modo sem servidor é recomendado para evitar que houve algum problema. Para saber mais detalhes sobre o modo de serviço, consulte [como escolher o modo de serviço](https://github.com/Azure/azure-signalr/blob/dev/docs/faq.md#what-is-the-meaning-of-service-mode-defaultserverlessclassic-how-can-i-choose).

## <a name="available-azure-signalr-service-events"></a>Eventos de serviço do Azure SignalR disponíveis
A Grade de eventos usa [assinaturas de evento](../event-grid/concepts.md#event-subscriptions) para rotear mensagens de evento para os assinantes. Assinaturas de evento do Azure SignalR Service dão suporte a dois tipos de eventos:  

|Nome do evento|DESCRIÇÃO|
|----------|-----------|
|`Microsoft.SignalRService.ClientConnectionConnected`|Gerado quando uma conexão de cliente está conectado.|
|`Microsoft.SignalRService.ClientConnectionDisconnected`|Gerado quando uma conexão de cliente é desconectado.|

## <a name="event-schema"></a>Esquema do evento
Eventos do Azure SignalR Service contêm todas as informações que você precisa para responder às alterações em seus dados. Você pode identificar um serviço do Azure SignalR evento com a propriedade eventType começa com "Microsoft.SignalRService". Informações adicionais sobre o uso das propriedades de evento da grade de eventos estão documentadas em [esquema de evento da grade de eventos](../event-grid/event-schema.md).  

Aqui está um exemplo de um evento conectados de conexão de cliente:
```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/signalr-rg/providers/Microsoft.SignalRService/SignalR/signalr-resource",
  "subject": "/hub/chat",
  "eventType": "Microsoft.SignalRService.ClientConnectionConnected",
  "eventTime": "2019-06-10T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "timestamp": "2019-06-10T18:41:00.9584103Z",
    "hubName": "chat",
    "connectionId": "crH0uxVSvP61p5wkFY1x1A",
    "userId": "user-eymwyo23"
  },
  "dataVersion": "1.0",
  "metadataVersion": "1"
}]
```

Para obter mais informações, consulte [esquema de eventos do SignalR Service](../event-grid/event-schema-azure-signalr.md).

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre a grade de eventos e dê uma chance de eventos de serviço do Azure SignalR:

> [!div class="nextstepaction"]
> [Tente uma integração de grade de eventos de exemplo com o serviço do Azure SignalR](./signalr-howto-event-grid-integration.md)
> [sobre a grade de eventos](../event-grid/overview.md)
