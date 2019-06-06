---
title: Reagir a eventos de chave-valor de configuração de aplicativo do Azure | Microsoft Docs
description: Use a grade de eventos do Azure para assinar eventos de configuração do aplicativo.
services: azure-app-configuration,event-grid
author: jimmyca
ms.author: jimmyca
ms.date: 05/30/2019
ms.topic: article
ms.service: azure-app-configuration
ms.openlocfilehash: 601124aef37d2b285db71130f5c63b3620c7768f
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735640"
---
# <a name="reacting-to-azure-app-configuration-events"></a>Reagir a eventos de configuração de aplicativo do Azure

Eventos de configuração de aplicativo do Azure permitem que aplicativos reagir a alterações nos valores de chave. Isso é feito sem a necessidade de código complicado ou serviços de sondagem ineficientes e caros. Em vez disso, os eventos são enviados por push pela [Grade de Eventos do Azure](https://azure.microsoft.com/services/event-grid/) aos assinantes como [Azure Functions](https://azure.microsoft.com/services/functions/), [Aplicativos Lógicos do Azure](https://azure.microsoft.com/services/logic-apps/), ou até mesmo seu próprio ouvinte http personalizado, e você só pague pelo que usa.

Eventos de configuração de aplicativo do Azure são enviados para a grade de eventos do Azure que fornece serviços de entrega confiável para seus aplicativos por meio de rich novamente as políticas e entrega de inatividade. Para obter mais informações, consulte [entrega de mensagens da grade de eventos e tente novamente](https://docs.microsoft.com/azure/event-grid/delivery-and-retry).

Cenários comuns de eventos de configuração de aplicativo incluem a atualização de configuração de aplicativo, disparar implantações ou qualquer fluxo de trabalho orientados a configuração. Quando as alterações não forem frequentes, mas seu cenário exigir uma capacidade de resposta imediata, a arquitetura baseada em eventos pode ser especialmente eficaz.

Dê uma olhada [eventos de configuração de aplicativo do Azure de rota para um personalizado da web de ponto de extremidade - CLI](./howto-app-configuration-event.md) para obter um exemplo rápido. 

![Modelo da Grade de Eventos](./media/event-grid-functional-model.png)

## <a name="available-azure-app-configuration-events"></a>Eventos de configuração de aplicativo do Azure disponíveis
A Grade de eventos usa [assinaturas de evento](../event-grid/concepts.md#event-subscriptions) para rotear mensagens de evento para os assinantes. Assinaturas de evento de configuração de aplicativo do Azure podem incluir dois tipos de eventos:  

> |Nome do evento|DESCRIÇÃO|
> |----------|-----------|
> |`Microsoft.AppConfiguration.KeyValueModified`|Acionado quando um valor de chave é criado ou substituído|
> |`Microsoft.AppConfiguration.KeyValueDeleted`|Acionado quando um valor de chave é excluído|

## <a name="event-schema"></a>Esquema do evento
Eventos de configuração de aplicativo do Azure contêm todas as informações que você precisa para responder às alterações em seus dados. Você pode identificar um evento de configuração de aplicativo porque a propriedade eventType começa com "Microsoft.AppConfiguration". Encontre informações adicionais sobre o uso de propriedades de evento da Grade de Eventos em [Esquema de eventos da Grade de Eventos](../event-grid/event-schema.md).  

> |Propriedade|Type|DESCRIÇÃO|
> |-------------------|------------------------|-----------------------------------------------------------------------|
> |topic|cadeia de caracteres|Id completa do Azure Resource Manager da configuração do aplicativo que emite o evento.|
> |subject|cadeia de caracteres|O URI de chave-valor que é o assunto do evento.|
> |eventTime|string|A data/hora que o evento foi gerado, no formato ISO 8601.|
> |eventType|cadeia de caracteres|"Microsoft.AppConfiguration.KeyValueModified" ou "Microsoft.AppConfiguration.KeyValueDeleted".|
> |ID|string|Um identificador exclusivo deste evento.|
> |dataVersion|cadeia de caracteres|A versão do esquema do objeto de dados.|
> |metadataVersion|string|A versão do esquema de propriedades de nível superior.|
> |data|objeto|Coleta de dados de evento específicas de configuração de aplicativo do Azure|
> |data.key|cadeia de caracteres|A chave da chave-valor que foi modificada ou excluída.|
> |data.label|string|O rótulo, se houver, de chave-valor que foi modificado ou excluído.|
> |data.etag|cadeia de caracteres|Para `KeyValueModified` a etag do novo valor de chave. Para `KeyValueDeleted` etag de chave-valor que foi excluído.|

Aqui está um exemplo de um evento KeyValueModified:
```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueModified",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]

```

Para obter mais informações, consulte [esquema de eventos de configuração de aplicativo do Azure](../event-grid/event-schema-app-configuration.md).

## <a name="practices-for-consuming-events"></a>Práticas para consumo de eventos
Aplicativos que tratam os eventos de configuração de aplicativo devem seguir algumas práticas recomendadas:
> [!div class="checklist"]
> * Como várias assinaturas podem ser configuradas para eventos de rota para o mesmo manipulador de eventos, é importante não supor que os eventos são de uma fonte específica, mas para verificar o tópico da mensagem para garantir que ela venha da configuração do aplicativo que você está esperando.
> * Da mesma forma, verifique se o eventType é do tipo que você está preparado para processar, e não suponha que todos os eventos recebidos serão os tipos esperados.
> * Como as mensagens podem chegar fora de ordem e após algum atraso, use os campos de etag para entender se suas informações sobre objetos ainda estão atualizadas.  Além disso, use os campos do sequenciador para entender a ordem de eventos em qualquer objeto específico.
> * Use o campo de assunto para acessar o valor de chave que foi modificado.


## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre a grade de eventos e dê uma chance de eventos de configuração de aplicativo do Azure:

- [Sobre a Grade de Eventos](../event-grid/overview.md)
- [Encaminhar eventos de configuração de aplicativo do Azure para um ponto de extremidade da web personalizado](./howto-app-configuration-event.md)