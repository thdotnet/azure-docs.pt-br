---
title: O que é a integridade do serviço do Azure?
description: Informações personalizadas sobre como seus aplicativos do Azure são afetados pela manutenção e pelos problemas de serviço atuais e futuros do Azure.
author: stephbaron
ms.author: stbaron
services: service-health
ms.service: service-health
ms.topic: article
ms.date: 05/10/2019
ms.openlocfilehash: 058a171766680f09eaf4de14ddd25235020b1ba4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079568"
---
# <a name="what-is-azure-service-health"></a>O que é a integridade do serviço do Azure?

O Azure oferece um conjunto de experiências para mantê-lo informado sobre a integridade de seus recursos de nuvem. Essas informações incluem os problemas atuais e futuros, como o impacto de eventos, manutenção planejada e outras alterações que podem afetar a disponibilidade de serviço.

A integridade do serviço do Azure é uma combinação de três serviços menores separados.

[Status do Azure](azure-status-overview.md) informa a você de interrupções de serviço no Azure na  **[página de Status do Azure](https://status.azure.com)** . A página é uma exibição global da integridade de todos os serviços do Azure em todas as regiões do Azure. A página de status é uma boa referência para incidentes com impacto generalizado, mas é altamente recomendável que os usuários do Azure atuais aproveitam a integridade do serviço do Azure para ficar informado sobre incidentes do Azure e manutenção.

[Integridade do serviço do Azure](service-health-overview.md) fornece uma exibição personalizada da integridade dos serviços do Azure e regiões que você está usando. Esse é o melhor lugar para procurar por serviço que afete as comunicações sobre interrupções, atividades de manutenção planejada e outros avisos de integridade porque a experiência autenticada da Integridade do Serviço do Azure sabe quais serviços e recursos você usa atualmente. A melhor maneira de usar a Integridade do Serviço é configurar alertas de Integridade do Serviço para notificá-lo por meio de seus canais de comunicação preferenciais quando problemas de serviço, manutenção planejada ou outras alterações podem afetar os serviços do Azure e as regiões que você usa.

[O Azure resource health](resource-health-overview.md) fornece informações sobre a integridade de seus recursos de nuvem individuais, como uma instância de máquina virtual específica. Usando o Azure Monitor, você também pode configurar alertas para notificá-lo de alterações de disponibilidade para seus recursos de nuvem. O Azure Resource Health juntamente com as notificações do Azure Monitor ajudará você a ficar mais bem informado sobre a disponibilidade de seus recursos de minuto a minuto e avaliar rapidamente se um problema ocorreu devido a uma complicação no seu lado ou está relacionado a um evento da plataforma do Azure.

Juntas, essas experiências fornecem uma visão abrangente da integridade do Azure, na granularidade mais relevante para você.

**Assista a uma visão geral da página Status do Azure, a integridade do serviço do Azure e Azure Resource Health**

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2OgX6]
