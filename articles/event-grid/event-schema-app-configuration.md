---
title: Esquema de eventos de configuração de aplicativo do Azure de grade de eventos do Azure
description: Descreve as propriedades que são fornecidas para eventos de configuração de aplicativo do Azure com a grade de eventos do Azure
services: event-grid
author: jimmyca
ms.service: event-grid
ms.topic: reference
ms.date: 05/30/2019
ms.author: jimmyca
ms.openlocfilehash: fe0274f723692eea3cfd25cc0e9e146b35dce2ae
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735775"
---
# <a name="azure-event-grid-event-schema-for-azure-app-configuration"></a>Esquema de evento de grade de eventos do Azure para a configuração de aplicativo do Azure

Este artigo fornece as propriedades e o esquema para eventos de configuração de aplicativo do Azure. Para obter uma introdução a esquemas de evento, consulte [esquema de grade de eventos do Azure](event-schema.md).

Para obter uma lista de scripts de exemplo e tutoriais, consulte [origem do evento de configuração de aplicativo do Azure](event-sources.md#app-configuration).

## <a name="available-event-types"></a>Tipos de evento disponíveis

Configuração de aplicativo do Azure emite os seguintes tipos de evento:

| Tipo de evento | DESCRIÇÃO |
| ---------- | ----------- |
| Microsoft.AppConfiguration.KeyValueModified | Gerado quando um valor de chave é criado ou substituído. |
| Microsoft.AppConfiguration.KeyValueDeleted | Gerado quando um valor de chave é excluído. |

## <a name="example-event"></a>Exemplo de evento

O exemplo a seguir mostra o esquema de um evento de modificação de chave-valor: 

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

O esquema para um evento de chave-valor excluído é semelhante: 

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
  "eventType": "Microsoft.AppConfiguration.KeyValueDeleted",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```
 
## <a name="event-properties"></a>Propriedades do evento

Um evento tem os seguintes dados de nível superior:

| Propriedade | Type | DESCRIÇÃO |
| -------- | ---- | ----------- |
| topic | cadeia de caracteres | Caminho de recurso completo para a origem do evento. Esse campo não é gravável. Grade de Eventos fornece esse valor. |
| subject | cadeia de caracteres | Caminho definido pelo fornecedor para o assunto do evento. |
| eventType | cadeia de caracteres | Um dos tipos de evento registrados para a origem do evento. |
| eventTime | cadeia de caracteres | A hora em que o evento é gerado com base na hora UTC do provedor. |
| id | cadeia de caracteres | Identificador exclusivo do evento. |
| data | objeto | Dados de eventos de configuração de aplicativo. |
| dataVersion | cadeia de caracteres | A versão do esquema do objeto de dados. O fornecedor define a versão do esquema. |
| metadataVersion | cadeia de caracteres | A versão do esquema do metadados de evento. Grade de Eventos define o esquema de propriedades de nível superior. Grade de Eventos fornece esse valor. |

O objeto de dados tem as seguintes propriedades:

| Propriedade | Type | DESCRIÇÃO |
| -------- | ---- | ----------- |
| chave | cadeia de caracteres | A chave da chave-valor que foi modificada ou excluída. |
| label | cadeia de caracteres | O rótulo, se houver, de chave-valor que foi modificado ou excluído. |
| etag | cadeia de caracteres | Para `KeyValueModified` a etag do novo valor de chave. Para `KeyValueDeleted` etag de chave-valor que foi excluído. |
 
## <a name="next-steps"></a>Próximas etapas

* Para ver uma introdução à Grade de Eventos do Azure, confira [O que é uma Grade de eventos?](overview.md)
* Para obter mais informações sobre como criar uma assinatura da Grade de Eventos do Azure, confira [Event Grid subscription schema](subscription-creation-schema.md) (Esquema de assinatura da Grade de Eventos).
* Para obter uma introdução ao trabalhar com eventos de configuração de aplicativo do Azure, consulte [eventos de configuração de aplicativo do Azure de rota - CLI do Azure](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json). 