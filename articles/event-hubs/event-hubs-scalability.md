---
title: Escalabilidade, os Hubs de eventos do Azure | Microsoft Docs
description: Fornece informações sobre como dimensionar os Hubs de eventos do Azure.
services: event-hubs
documentationcenter: na
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.custom: seodec18
ms.date: 06/18/2019
ms.author: shvija
ms.openlocfilehash: 3eb20013a6b3afaddce10f2e4652add0edf22a9a
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276776"
---
# <a name="scaling-with-event-hubs"></a>Dimensionamento com os Hubs de eventos

Há dois fatores que influenciam o dimensionamento com os Hubs de eventos.
*   Unidades de transferência
*   Partições

## <a name="throughput-units"></a>Unidades de transferência

A capacidade de transferência dos Hubs de Eventos é controlada pelas *unidades de transferência*. As unidades de taxa de transferência são unidades de capacidade pré-adquiridas. Uma única taxa de transferência permite que você:

* Entrada: Até 1 MB por segundo ou 1000 eventos por segundo (o que ocorrer primeiro).
* Saída: Até 2 MB por segundo ou 4096 eventos por segundo.

Além da capacidade de unidades de transferência adquiridas, a entrada está limitada e uma [ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) é retornada. A saída não gera exceções de limitação, mas ainda é limitada à capacidade das unidades de produtividade adquiridas. Se você receber exceções de taxa de publicação ou estiver esperando ver mais saída, verifique quantas unidades de transferência você comprou para o namespace. Você pode gerenciar unidades de produtividade na folha **Escala** dos namespaces no [portal do Azure](https://portal.azure.com). Você também pode gerenciar unidades de produtividade programaticamente usando as [APIs de Hubs de Eventos](event-hubs-api-overview.md).

As unidades de taxa de transferência são pré-adquiridas e cobradas por hora. Depois de adquiridas, as unidades de taxa de transferência são cobradas por um mínimo de uma hora. Até 20 unidades de produtividade podem ser adquiridas para um namespace de Hubs de Eventos e elas são compartilhadas entre todos os Hubs de Eventos no namespace.

O recurso **inflar automaticamente** dos Hubs de Eventos escala verticalmente automaticamente aumentando o número de unidades de taxa de transferência para atender às necessidades de uso. O aumento de unidades de taxa de transferência evita cenários de limitação, nos quais:

- As taxas de entrada de dados excedem as unidades de taxa de transferência definidas.
- As taxas de solicitação de saída de dados excedem as unidades de taxa de transferência definidas.

O serviço de Hubs de Eventos aumenta a taxa de transferência quando a carga aumentar ultrapassando o limite mínimo, sem quaisquer solicitações com falha com erros de ServerBusy. 

Para obter mais informações sobre o recurso inflar, consulte [dimensionar automaticamente as unidades de produtividade](event-hubs-auto-inflate.md).

## <a name="partitions"></a>Partições

As partições permitem que você escala para o processamento de downstream. Por causa do modelo de consumidor particionado Hubs de eventos oferece com partições, você pode expandir ao processar seus eventos simultaneamente. Um Hub de eventos pode ter até 32 partições.

É recomendável balancear partições e unidades de taxa de transferência de 1:1 para obter uma escala ideal. Uma única partição tem uma entrada e saída de até uma unidade de taxa de transferência garantida. Embora você possa ser capaz de alcançar maior taxa de transferência em uma partição, o desempenho não é garantido. É por isso é altamente recomendável que o número de partições em um hub de eventos seja maior que ou igual ao número de unidades de taxa de transferência.

Dada a taxa de transferência total que você planeja necessidade, você sabe o número de unidades de taxa de transferência, que você precisa e o número mínimo de partições, mas quantas partições você deve ter? Escolha o número de partições com base em paralelismo de downstream que você quer atingir, bem como suas necessidades futuras de taxa de transferência. Não há nenhum custo para o número de partições que você tem em um Hub de eventos.

Para obter informações detalhadas sobre o preço de Hubs de Eventos, consulte [Preço de Hubs de Eventos](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="next-steps"></a>Próximas etapas
Você pode saber mais sobre Hubs de Eventos visitando os links abaixo:

- [Dimensionar automaticamente as unidades de taxa de transferência](event-hubs-auto-inflate.md)
- [Visão geral do serviço dos Hubs de Eventos](event-hubs-what-is-event-hubs.md)
