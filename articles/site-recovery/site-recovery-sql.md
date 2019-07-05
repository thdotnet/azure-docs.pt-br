---
title: Configurar a recuperação de desastres para o SQL Server com o SQL Server e o Azure Site Recovery | Microsoft Docs
description: Este artigo descreve como configurar a recuperação de desastre para o SQL Server usando o SQL Server e o Azure Site Recovery.
services: site-recovery
author: sujayt
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/30/2019
ms.author: sutalasi
ms.openlocfilehash: 1c44b10b54a5f58dff1aecf36c3633cc8ffbd8f0
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491786"
---
# <a name="set-up-disaster-recovery-for-sql-server"></a>Configurar a recuperação de desastres para o SQL Server

Este artigo descreve como proteger o back-end do SQL Server de um aplicativo usando uma combinação de tecnologias BCDR (continuidade dos negócios e recuperação de desastre) do SQL Server e o [Azure Site Recovery](site-recovery-overview.md).

Antes de começar, certifique-se de que entender a recursos de recuperação de desastres do SQL Server, incluindo o clustering de failover, grupos de disponibilidade Always On, o banco de dados de espelhamento, envio de logs, replicação geográfica ativa e grupos de failover automático.

## <a name="dr-recommendation-for-integration-of-sql-server-bcdr-technologies-with-site-recovery"></a>Recomendação de recuperação de desastres para a integração de tecnologias de BCDR do SQL Server com o Site Recovery

Escolha uma tecnologia de BCDR para servidores de SQL de recuperação deve se basear em suas necessidades RTO e RPO conforme a tabela a seguir. Depois que essa escolha é feita, o Site Recovery pode ser integrado com a operação de failover do que a tecnologia para coordenar a recuperação de todo o seu aplicativo.

**Tipo de implantação** | **Tecnologia BCDR** | **RTO esperado para SQL** | **RPO esperado para SQL** |
--- | --- | --- | ---
SQL Server na VM IaaS do Azure ou no local| **[Grupo de disponibilidade Always On](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server?view=sql-server-2017)** | Equivalente ao tempo levado para tornar a réplica secundária como primário | A replicação é assíncrona para a réplica secundária, portanto, há alguma perda de dados.
SQL Server na VM IaaS do Azure ou no local| **[Clustering de failover (sempre em FCI)](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server?view=sql-server-2017)** | Equivalente ao tempo necessário para failover entre os nós | Ele usa o armazenamento compartilhado, portanto, a mesma exibição da instância de armazenamento está disponível no failover.
SQL Server na VM IaaS do Azure ou no local| **[(Modo de alto desempenho) de espelhamento de banco de dados](https://docs.microsoft.com/sql/database-engine/database-mirroring/database-mirroring-sql-server?view=sql-server-2017)** | Equivalente ao tempo levado para forçar o serviço, que usa o servidor espelho como um servidor em espera passiva. | A replicação é assíncrona. O banco de dados espelho pode ficar um pouco atrás do banco de dados principal. O intervalo é normalmente pequeno, howvever, poderá se tornar significativo se o sistema do servidor principal ou espelho está sob uma carga pesada.<br></br>Envio de logs pode ser um suplemento ao espelhamento de banco de dados e é uma alternativa favorável ao espelhamento de banco de dados assíncrono
SQL como PaaS no Azure<br></br>(Pools Elásticos, servidores de banco de dados SQL) | **Replicação geográfica ativa** | 30 segundos depois que ele for disparado<br></br>Quando o failover é ativado para um dos bancos de dados secundários, todos os outros secundários são vinculados automaticamente ao novo primário. | RPO de cinco segundos<br></br>Replicação geográfica ativa aproveita a tecnologia Always On do SQL Server para replicar de forma assíncrona transações confirmadas no banco de dados primário para um banco de dados secundário usando o isolamento de instantâneo. <br></br>Os dados secundários é garantido que nunca terem transações parciais.
SQL como PaaS configurado com replicação geográfica ativa no Azure<br></br>(Banco de dados de instância gerenciada do SQL, pools Elásticos, servidores de banco de dados SQL) | **Grupos de failover automático** | RTO de 1 hora | RPO de cinco segundos<br></br>Os grupos de failover automático fornecem a semântica de grupo além da replicação geográfica ativa, mas o mesmo mecanismo de replicação assíncrona é usado.
SQL Server na VM IaaS do Azure ou no local| **Replicação com o Azure Site Recovery** | Normalmente menos de 15 minutos. [Leia mais](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/) para saber mais sobre o SLA de RTO fornecida pelo Azure Site Recovery. | 1 hora para consistência com aplicativos e 5 minutos para consistência de falhas. 

> [!NOTE]
> Algumas considerações importantes ao proteger as cargas de trabalho do SQL com o Azure Site Recovery:
> * O Azure Site Recovery é independente do aplicativo e, portanto, qualquer versão do SQL server que é implantado em um sistema operacional com suporte pode ser protegido pelo Azure Site Recovery. [Saiba mais](vmware-physical-azure-support-matrix.md#replicated-machines).
> * Você pode optar por usar o Site Recovery para qualquer implantação no Azure, Hyper-V, VMware ou infraestrutura física. Siga as [orientação](site-recovery-sql.md#how-to-protect-a-sql-server-cluster-standard-editionsql-server-2008-r2) no final do documento sobre como proteger o Cluster do SQL Server com o Azure Site Recovery.
> * Certifique-se de que a taxa diária de alteração (bytes de gravação por segundo) observada na máquina está dentro do [limites do Site Recovery](vmware-physical-azure-support-matrix.md#churn-limits). Para máquinas windows, você pode exibir isso na guia de desempenho no Gerenciador de tarefas. Gravação de observar a velocidade para cada disco.
> * O Azure Site Recovery dá suporte à replicação de instâncias de Cluster de Failover em espaços de armazenamento diretos. [Saiba mais](azure-to-azure-how-to-enable-replication-s2d-vms.md).
 

## <a name="disaster-recovery-of-application"></a>Recuperação de desastres do aplicativo

**O Azure Site Recovery orquestra o failover de teste e o failover de todo o seu aplicativo com a Ajuda de planos de recuperação.** 

Há alguns pré-requisitos para garantir que o plano de recuperação é totalmente personalizado, de acordo com suas necessidades. Qualquer implantação do SQL Server normalmente precisa de um Active Directory. Ele também precisa de conectividade para sua camada de aplicativo.

### <a name="step-1-set-up-active-directory"></a>Etapa 1: Configurar o Active Directory

Configure o Active Directory no site de recuperação secundário para que o SQL Server seja executado corretamente.

* **Pequena empresa**— Com uma quantidade pequena de aplicativos e um único controlador de domínio para o site local, se você quiser realizar o failover de todo o site, recomendamos o uso da replicação do Site Recovery para replicar o controlador de domínio para o armazenamento de dados secundário ou para o Azure.
* **Empresa de médio e grande porte**— Se você tiver uma grande quantidade de aplicativos, uma floresta do Active Directory e quiser realizar o failover por aplicativo ou carga de trabalho, recomendamos a configuração de um controlador de domínio adicional no armazenamento de dados secundário ou no Azure. Se você estiver usando os grupos de disponibilidade AlwaysOn para recuperar em um site remoto, recomendamos a configuração de outro controlador de domínio adicional no site secundário ou no Azure, a fim de usá-lo para a instância do SQL Server recuperada.

As instruções neste artigo supõem que exista um controlador de domínio disponível no local secundário. [Ler mais](site-recovery-active-directory.md) sobre como proteger o Active Directory com o Site Recovery.

### <a name="step-2-ensure-connectivity-with-other-application-tiers-and-web-tier"></a>Etapa 2: Verifique a conectividade com outros aplicativos camada (s) e a camada da web

Certifique-se de que, depois que a camada de banco de dados está em execução na região do Azure de destino, você tem conectividade com o aplicativo e a camada da web. Etapas necessárias cuidadas com antecedência para validar a conectividade com o failover de teste.

Entenda como você pode criar aplicativos para considerações de conectividade com alguns exemplos:
* [Criar um aplicativo para recuperação de desastre de nuvem](../sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery.md)
* [Estratégias de recuperação de desastre do pool Elástico](../sql-database/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md)

### <a name="step-3-integrate-with-always-on-active-geo-replication-or-auto-failover-groups-for-application-failover"></a>Etapa 3: Integrar com o Always On, replicação geográfica ativa ou grupos de failover automático para o failover de aplicativo

Tecnologias BCDR Always On, os grupos de failover automático e replicação geográfica ativa têm réplicas secundárias do SQL server em execução na região do Azure de destino. Portanto, a primeira etapa para o failover de seu aplicativo é tornar essa réplica como primária (supondo que você já tiver um controlador de domínio no secundário). Essa etapa não pode ser necessária se você optar por fazer um failover automático. Somente após o failover de banco de dados, você deve failover suas camadas de aplicativo ou web.

> [!NOTE] 
> Se você protegeu máquinas do SQL com o Azure Site Recovery, basta criar um grupo de recuperação desses computadores e adicionar o failover no plano de recuperação.

[Criar um plano de recuperação](site-recovery-create-recovery-plans.md) com aplicativos e máquinas virtuais da camada da web. Siga as etapas a seguir para adicionar o failover da camada de banco de dados:

1. Importar scripts para sua conta da Automação do Azure. Contém os scripts de failover do grupo de disponibilidade do SQL em uma [máquina virtual do Resource Manager](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAG.ps1) e uma [máquina virtual clássica](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/asr-automation-recovery/scripts/ASR-SQL-FailoverAGClassic.ps1).

    [![Implantar no Azure](https://azurecomcdn.azureedge.net/mediahandler/acomblog/media/Default/blog/c4803408-340e-49e3-9a1f-0ed3f689813d.png)](https://aka.ms/asr-automationrunbooks-deploy)


1. Adicione ASR-SQL-FailoverAG como uma ação prévia do primeiro grupo do plano de recuperação.

1. Siga as instruções disponíveis no script para criar uma variável de automação para fornecer o nome dos grupos de disponibilidade.

### <a name="step-4-conduct-a-test-failover"></a>Etapa 4: Realizar um failover de teste

Algumas tecnologias BCDR como Always On do SQL não suportam nativamente o failover de teste. Portanto, recomendamos a seguinte abordagem **somente quando a integração com tecnologias**:

1. Configure o [Backup do Azure](../backup/backup-azure-arm-vms.md) na máquina virtual que hospeda a réplica do grupo de disponibilidade no Azure.

1. Antes de disparar o failover de teste do plano de recuperação, recupere a máquina virtual no backup feito na etapa anterior.

    ![Restaurar do Backup do Azure](./media/site-recovery-sql/restore-from-backup.png)

1. [Imponha um quorum](https://docs.microsoft.com/sql/sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum#PowerShellProcedure) na máquina virtual restaurada do backup.

1. Atualize o IP do ouvinte para um IP disponível na rede de failover de teste.

    ![Atualizar o Ouvinte de IP](./media/site-recovery-sql/update-listener-ip.png)

1. Coloque o ouvinte online.

    ![Colocar o Ouvinte Online](./media/site-recovery-sql/bring-listener-online.png)

1. Crie um balanceador de carga com um IP criado no pool IP de front-end correspondente a cada ouvinte de grupo de disponibilidade e com a máquina virtual do SQL adicionada no pool de back-end.

     ![Criar balanceador de carga – pool IP de front-end](./media/site-recovery-sql/create-load-balancer1.png)

    ![Criar balanceador de carga – pool de back-end](./media/site-recovery-sql/create-load-balancer2.png)

1. Adicione um failover de sua camada de aplicativo, seguida de camada da web neste plano de recuperação em grupos de recuperação subsequentes. 
1. Faça um failover de teste do plano de recuperação para failover de ponta a ponta do aplicativo de teste.

## <a name="steps-to-do-a-failover"></a>Etapas para fazer um failover

Depois de ter adicionado o script no plano de recuperação na etapa 3 e validado seguindo um failover de teste com uma abordagem especializado na etapa 4, você pode fazer failover do plano de recuperação criado na etapa 3.

Observe que as etapas de failover para o aplicativo e as camadas da web devem ser a mesma no failover de teste e planos de recuperação de failover.

## <a name="how-to-protect-a-sql-server-cluster-standard-editionsql-server-2008-r2"></a>Como proteger um cluster do SQL Server (standard edition/SQL, Server 2008 R2)

Para um cluster executando o SQL Server Standard ou o SQL Server 2008 R2, recomendamos o uso da replicação do Site Recovery para proteger o SQL Server.

### <a name="azure-to-azure-and-on-premises-to-azure"></a>Azure para o Azure e local para o Azure

Recuperação de site não fornece convidado suporte a clusters ao replicar para uma região do Azure. O SQL Server também não fornece uma solução de recuperação de desastres de baixo custo para a edição Standard. Nesse cenário, é recomendável que você proteger um cluster do SQL Server para um SQL Server no local primário autônomo e recuperá-lo no secundário.

1. Configure uma instância do SQL Server autônomo adicional na região primária do Azure ou no site local.
1. Configure a instância para servir como um espelho para os bancos de dados que você quer proteger. Configure o espelhamento no modo de alta segurança.
1. Configurar a recuperação de Site no site primário ([Azure](azure-to-azure-tutorial-enable-replication.md), [Hyper-V](site-recovery-hyper-v-site-to-azure.md) ou [VMs VMware/servidores físicos)](site-recovery-vmware-to-azure-classic.md).
1. Use a replicação do Site Recovery para replicar a nova instância do SQL Server para o site secundário. Como é uma cópia espelhada de alta segurança, ele será sincronizado com o cluster primário, mas ela será replicada usando a replicação de recuperação de Site.


![Cluster padrão](./media/site-recovery-sql/standalone-cluster-local.png)

### <a name="failback-considerations"></a>Considerações sobre failback

Para clusters do SQL Server Standard, a realização do failback após um failover não planejado exige um backup e restauração do SQL Server, a partir da instância de espelho para o cluster original com o restabelecimento do espelhamento.

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="how-does-sql-get-licensed-when-protected-with-azure-site-recovery"></a>Como o SQL obter licenciado quando protegido com o Azure Site Recovery?
A replicação do Azure Site Recovery para o SQL Server é coberta pelo benefício do Software Assurance – Recuperação de Desastre, para todos os cenários do Azure Site Recovery (recuperação de desastre local para o Azure ou recuperação de desastres IaaS do Azure de região cruzada). [Leia mais](https://azure.microsoft.com/pricing/details/site-recovery/)

### <a name="will-azure-site-recovery-support-my-sql-version"></a>Recuperação de Site do Azure oferecerá suporte a minha versão do SQL?
O Azure Site Recovery é independente do aplicativo. Portanto, qualquer versão do SQL server que é implantado em um sistema operacional com suporte pode ser protegido pelo Azure Site Recovery. [Saiba mais](vmware-physical-azure-support-matrix.md#replicated-machines)

## <a name="next-steps"></a>Próximas etapas
* [Saiba mais](site-recovery-components.md) sobre a arquitetura do Site Recovery.
* Para servidores SQL no Azure, saiba mais sobre [soluções de alta disponibilidade](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md#azure-only-high-availability-solutions) para recuperação na região secundária do Azure.
* Banco de dados SQL no Azure, saiba mais sobre o [continuidade de negócios](../sql-database/sql-database-business-continuity.md) e [alta disponibilidade](../sql-database/sql-database-high-availability.md) opções de recuperação na região secundária do Azure.
* Para máquinas do SQL server no local, [Saiba mais](../virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr.md#hybrid-it-disaster-recovery-solutions) sobre as opções de alta disponibilidade para a recuperação em máquinas virtuais do Azure.
