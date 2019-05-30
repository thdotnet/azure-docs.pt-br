---
title: Arquivo de inclusão
description: Arquivo de inclusão
ms.topic: include
ms.custom: include file
services: time-series-insights
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 04/29/2019
ms.openlocfilehash: e3e7044148e262e6977ae3be96f7cb61218114a3
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66388738"
---
## <a name="business-disaster-recovery"></a>Recuperação de desastre de negócios

Esta seção descreve os recursos do Azure Time Series Insights que mantêm os aplicativos e serviços em execução, mesmo se ocorrer um desastre. Esse conceito é conhecido como recuperação de desastres de negócios.

### <a name="high-availability"></a>Alta disponibilidade

Como um serviço do Azure, o Time Series Insights fornece determinados recursos de alta disponibilidade usando redundâncias no nível da região do Azure. Por exemplo, o Microsoft Azure dá suporte à recuperação de desastre por meio do recurso de disponibilidade entre regiões do Azure.

Recursos adicionais de alta disponibilidade, fornecidos por meio do Azure e que também estão disponíveis em qualquer instância do Time Series Insights, incluem:

* **Failover**: O Azure fornece [balanceamento de carga e replicação geográfica](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region).
* **Restauração de dados** e **recuperação do armazenamento**: O Azure fornece [várias opções para preservar e recuperar dados](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption).
* **Recuperação de site**: Recursos que estão disponíveis por meio [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/).

Certifique-se de habilitar esses recursos do Azure fornecer alta disponibilidade global e entre regiões para seus dispositivos e usuários.

> [!NOTE]
> Se o Azure está configurado para habilitar a disponibilidade de região cruzada, nenhuma configuração de disponibilidade de região cruzada adicional é necessária dentro do Azure Time Series Insights.

### <a name="iot-and-event-hubs"></a>IoT e hubs de eventos

Alguns serviços de IoT do Azure também incluem recursos de recuperação de desastres de negócios integrados:

* [Recuperação de desastres de alta disponibilidade do IoT Hub do Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr), que inclui a redundância entre regiões.
* [Políticas de Hubs de eventos do Azure](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr).
* [Redundância de armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-redundancy).

A integração com esses outros serviços do Time Series Insights fornece desastres adicionais oportunidades de recuperação. Por exemplo, a telemetria enviada ao hub de eventos pode ser persistente para um backup de banco de dados de armazenamento de BLOBs do Azure.

### <a name="time-series-insights"></a>Time Series Insights

Há várias maneiras de manter seus dados de séries temporais, aplicativos e serviços em execução, mesmo se elas são interrompidas. Você também pode determinar que uma cópia de backup completa do seu ambiente Azure Time Series é necessária:

* Como uma instância de failover do Time Series Insights específicos para redirecionar dados e o tráfego para.
* Para fins de preservação de dados e de auditoria.

Em geral, a melhor maneira para duplicar um ambiente do Time Series Insights é criar um segundo ambiente de Time Series Insights em um backup de região do Azure. Eventos também são enviados para esse ambiente secundário da fonte de evento principal. Certifique-se de que para usar um segundo grupo de consumidores exclusivo e seguir diretrizes para recuperação de desastres do negócios da origem que, fornecidos anteriormente.

Especificamente, para criar um ambiente duplicado:

1. Crie um ambiente na segunda região. Para obter instruções, consulte [criar um novo ambiente do Time Series Insights no portal do Azure](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started).
1. Crie um segundo grupo de consumidores dedicado para a origem do evento.
1. Conecte essa origem do evento ao novo ambiente. Certifique-se designar o segundo grupo de consumidores dedicado.
1. Examine o Time Series Insights [hub IoT](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) e [hub de eventos](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access) documentação.

Por fim:

1. Se a região primária for afetada durante um incidente de desastres, redirecione as operações para o backup do ambiente do Time Series Insights.
1. Use sua segunda região para fazer backup e recuperar todos os dados de telemetria e consulta do Time Series Insights.

> [!IMPORTANT]
> * Pode haver atraso no evento no caso de failover.
> * Failover também pode causar um aumento momentâneo no processamento de mensagem, como operações são redirecionadas.
> * Para saber mais, confira [Reduzir a latência no Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency).
