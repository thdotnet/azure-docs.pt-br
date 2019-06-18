---
title: Recuperação de desastres geográfica na grade de eventos do Azure | Microsoft Docs
description: Descreve como a grade de eventos do Azure dá suporte à recuperação de desastres geográfica (GeoDR) automaticamente.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/24/2019
ms.author: spelluru
ms.openlocfilehash: 5b5c973a8daa8776efb0909092c569ea46902265
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66307311"
---
# <a name="server-side-geo-disaster-recovery-in-azure-event-grid"></a>Recuperação de desastres geográfica do lado do servidor na grade de eventos do Azure
Agora, a grade de eventos tem uma recuperação de desastres geográfica automática (GeoDR) de metadados não apenas para novo, mas todos os domínios existentes, tópicos e assinaturas de evento. Se toda a região do Azure ficar inativo, a grade de eventos já terá todos os seus metadados relacionados a eventos infraestrutura sincronizados com uma região emparelhada. Seus novos eventos começará a fluir novamente sem nenhuma intervenção por você. 

Recuperação de desastre é medida com duas métricas:

- [Objetivo de ponto de recuperação (RPO)](https://en.wikipedia.org/wiki/Disaster_recovery#Recovery_Point_Objective): minutos ou horas de dados que podem ser perdidas.
- [Objetivo de tempo de recuperação (RTO)](https://en.wikipedia.org/wiki/Disaster_recovery#Recovery_time_objective): os minutos de horas que o serviço pode ser suspenso.

O failover automático da grade de eventos tem diferentes RPOs e RTOs para seus metadados (assinaturas de evento etc.) e os dados (eventos). Se você precisar especificação diferente de páginas a seguir, você ainda pode implementar seu próprio [cliente fazer failover usando o tópico de apis de integridade](custom-disaster-recovery.md).

## <a name="recovery-point-objective-rpo"></a>RPO (Objetivo de Ponto de Recuperação)
- **RPO Metadata**: zero minutos. Sempre que um recurso é criado na grade de eventos, ele instantaneamente é replicado entre regiões. Quando ocorre um failover, nenhum metadado é perdido.
- **Dados RPO**: se o sistema estiver íntegro e atualizado quanto ao tráfego existente no momento do failover regional, o RPO para os eventos será de cerca de cinco minutos.

## <a name="recovery-time-objective-rto"></a>RTO (Objetivo de Tempo de Recuperação)
- **Metadados RTO**: Embora geralmente muito mais rapidamente, acontece dentro de 60 minutos, a grade de eventos será iniciado aceitar chamadas de criar/atualizar/excluir para tópicos e assinaturas.
- **Dados RTO**: Como metadados, isso geralmente acontece muito mais rapidamente, porém dentro de 60 minutos, a grade de eventos começará a aceitar tráfego novo após um failover regional.

> [!NOTE]
> O custo para metadados GeoDR na grade de eventos é: US $0.


## <a name="next-steps"></a>Próximas etapas
Se você quiser implementar lógica de failover do lado do cliente próprios a você, consulte [# criar seu próprios recuperação de desastre para tópicos personalizados na grade de eventos](custom-disaster-recovery.md)
