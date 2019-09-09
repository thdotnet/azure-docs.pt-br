---
title: Sobre a migração de máquinas locais e VMs do Azure Azure Site Recovery
description: Este artigo descreve como migrar VMs locais e Azure IaaS para o Azure usando o serviço Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 09/09/2019
ms.author: raynew
ms.openlocfilehash: c043950de9565f96d52c848f96efac80385f2321
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70814477"
---
# <a name="about-migration"></a>Sobre a migração

Leia este artigo para obter uma visão geral de como o serviço [Azure Site Recovery](site-recovery-overview.md) ajuda você a migrar as máquinas. 

Veja o que você pode migrar com a Site Recovery:

- **Migrar do local para Azure**: Migre VMs do VMware, VMs do Hyper-V local e servidores físicos para o Azure. Após a migração, as cargas de trabalho em execução nas máquinas locais serão executadas em VMs do Azure. 
- **Migrar dentro do Azure**: Migrar VMs do Azure entre regiões do Azure. 
- **Migrar AWS**: Migre instâncias AWS Windows para VMs da IaaS do Azure. 

> [!NOTE]
> Agora você pode migrar do local para o Azure usando o serviço migrações para Azure. [Saiba mais](../migrate/migrate-overview.md).

## <a name="what-do-we-mean-by-migration"></a>O que queremos dizer por migração?

Além de usar o Site Recovery para recuperação de desastre de VMs locais e do Azure, você poderá usar o serviço Site Recovery para migrá-las. Qual é a diferença?

- Para a recuperação de desastre, você pode replicar máquinas regularmente no Azure. Quando ocorre uma paralisação, há o failover de máquinas do site primário para o site secundário do Azure e você as acessa a partir daí. Quando o site primário estiver disponível novamente, você executar o failback a partir do Azure.
- Para a migração, você replica as máquinas locais para o Azure ou as VMs do Azure para uma região secundária. Em seguida, faça o failover da VM do site primário para o secundário e conclua o processo de migração. Não há nenhum failback envolvido.  


## <a name="migration-scenarios"></a>Cenários de migração

**Cenário** | **Detalhes**
--- | ---
**Migrar do local para o Azure** | Você pode migrar máquinas virtuais do VMware locais, VMs do Hyper-V e servidores físicos para o Azure. Para fazer isso, você conclui quase as mesmas etapas que usaria para a recuperação de desastre completo. Você simplesmente não faz failover de máquinas do Azure para o site local.
**Migrar entre regiões do Azure** | Você pode migrar máquinas virtuais do Azure de uma região do Azure para outra. Após a conclusão da migração, você poderá configurar a recuperação de desastre para VMs do Azure agora na região secundária para a qual migrou.
**Migrar AWS para o Azure** | Você pode migrar instâncias do AWS para VMs do Azure. O Site Recovery trata instâncias do AWS como servidores físicos para fins de migração. 

## <a name="next-steps"></a>Próximas etapas

- [Migrar máquinas locais para o Azure](migrate-tutorial-on-premises-azure.md)
- [Migrar máquinas virtuais de uma região do Azure para outra](azure-to-azure-tutorial-migrate.md)
- [Migrar AWS para o Azure](migrate-tutorial-aws-azure.md)
