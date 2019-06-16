---
ms.topic: include
ms.service: time-series-insights
author: kingdomofends
ms.author: adgera
ms.date: 04/29/2019
ms.openlocfilehash: 8a3c630b54ff95a9b1200e2421c787a514a0aa52
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66431047"
---
## <a name="business-disaster-recovery"></a>Recuperação de desastre de negócios

Esta seção descreve os recursos do Azure Time Series Insights que mantêm os aplicativos e serviços em execução, mesmo se ocorrer um desastre (conhecido como *recuperação de desastres de negócios*).

### <a name="high-availability"></a>Alta disponibilidade

Como um serviço do Azure, o Time Series Insights fornece certas *alta disponibilidade* recursos usando redundâncias no nível da região do Azure. Por exemplo, o Azure dá suporte a recursos de recuperação de desastres por meio do Azure *disponibilidade de região cruzada* recurso.

Recursos adicionais de alta disponibilidade fornecidos por meio do Azure (e também está disponível em qualquer instância do Time Series Insights) incluem:

- **Failover**: O Azure fornece [balanceamento de carga e replicação geográfica](https://docs.microsoft.com/azure/architecture/resiliency/recovery-loss-azure-region).
- **Restauração de dados** e **recuperação do armazenamento**: O Azure fornece [várias opções para preservar e recuperar dados](https://docs.microsoft.com/azure/architecture/resiliency/recovery-data-corruption).
- **Recuperação de site**: O Azure fornece recursos de recuperação de site por meio [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/).

Verifique se que você habilitar os recursos relevantes do Azure fornecer alta disponibilidade global e entre regiões para seus dispositivos e usuários.

> [!NOTE]
> Se o Azure está configurado para habilitar a disponibilidade de região cruzada, nenhuma configuração de disponibilidade de região cruzada adicional é necessária no Azure Time Series Insights.

### <a name="iot-and-event-hubs"></a>IoT e hubs de eventos

Alguns serviços de IoT do Azure também incluem recursos de recuperação de desastres de negócios integrados:

- [Recuperação de desastres de alta disponibilidade do IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-ha-dr), incluindo redundância entre regiões
- [Políticas de Hubs de eventos](https://docs.microsoft.com/azure/event-hubs/event-hubs-geo-dr)
- [Redundância do Armazenamento do Azure](https://docs.microsoft.com/azure/storage/common/storage-redundancy)

A integração com outros serviços do Time Series Insights fornece desastres adicionais oportunidades de recuperação. Por exemplo, a telemetria enviada ao hub de eventos pode ser persistente para um backup de banco de dados de armazenamento de BLOBs do Azure.

### <a name="time-series-insights"></a>Time Series Insights

Há várias maneiras de manter seus dados de séries temporais, aplicativos e serviços em execução, mesmo se elas são interrompidas. 

No entanto, você pode determinar que uma cópia de backup completa do seu ambiente Azure Time Series também é necessária, para as seguintes finalidades:

- Como uma *instância failover* especificamente para o Time Series Insights redirecionar dados e o tráfego para
- Para preservar os dados e informações de auditoria

Em geral, a melhor maneira para duplicar um ambiente do Time Series Insights é criar um segundo ambiente de Time Series Insights em um backup de região do Azure. Eventos também são enviados para esse ambiente secundário da fonte de evento principal. Certifique-se de que você use um grupo de consumidores dedicado, segundo. Siga as diretrizes de recuperação de desastres de negócios que da origem, conforme descrito anteriormente.

Para criar um ambiente duplicado:

1. Crie um ambiente na segunda região. Para obter mais informações, consulte [criar um novo ambiente do Time Series Insights no portal do Azure](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started).
1. Crie um segundo grupo de consumidores dedicado para a origem do evento.
1. Conecte essa origem do evento ao novo ambiente. Certifique-se de que você designe o grupo de consumidores dedicado, segundo.
1. Examine o Time Series Insights [IoT Hub](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-how-to-add-an-event-source-iothub) e [Hubs de eventos](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-data-access) documentação.

Se ocorrer um evento:

1. Se a região primária for afetada durante um incidente de desastres, redirecione as operações para o backup do ambiente do Time Series Insights.
1. Use sua segunda região para fazer backup e recuperar todos os dados de telemetria e consulta do Time Series Insights.

> [!IMPORTANT]
> Se ocorrer um failover:
> 
> * Também pode ocorrer um atraso.
> * Pode ocorrer um aumento momentâneo no processamento de mensagens, como operações são redirecionadas.
> 
> Para saber mais, confira [Reduzir a latência no Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-environment-mitigate-latency).

