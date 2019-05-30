---
title: Configurar a recuperação de desastres para o Azure para um grande número de VMs VMware ou servidores físicos com o Azure Site Recovery | Microsoft Docs
description: Saiba como configurar a recuperação de desastres para o Azure para um grande número de VMs do VMware locais ou servidores físicos com o Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: raynew
ms.openlocfilehash: e96aafe61c0d8547ffca9e97bfd9e90c9529155f
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237273"
---
# <a name="set-up-disaster-recovery-at-scale-for-vmware-vmsphysical-servers"></a>Configurar a recuperação de desastre em grande escala para VMs VMware/servidores físicos

Este artigo descreve como configurar a recuperação de desastres para o Azure para números grandes (> 1000) de VMs do VMware locais ou servidores físicos em seu ambiente de produção, usando o [Azure Site Recovery](site-recovery-overview.md) service.


## <a name="define-your-bcdr-strategy"></a>Definir sua estratégia de BCDR

Como parte da sua estratégia BCDR (recuperação) de desastres e continuidade dos negócios, você pode definir os objetivos de ponto de recuperação (RPOs) e objetivos de tempo de recuperação (RTOs) para seus aplicativos de negócios e cargas de trabalho. RTO mede a duração do tempo e o serviço de nível dentro da qual um aplicativo de negócios ou processo deve ser restaurado e disponível, para evitar problemas de continuidade.
- O site Recovery fornece replicação contínua para VMs VMware e servidores físicos e um [SLA](https://azure.microsoft.com/support/legal/sla/site-recovery/v1_2/) para RTO.
- Ao planejar para a recuperação de desastre em grande escala para VMs VMware e descobrir os recursos do Azure, que você precisa, você pode especificar um valor RTO que será usado para cálculos de capacidade.


## <a name="best-practices"></a>Práticas recomendadas

Algumas práticas recomendadas gerais para a recuperação de desastre em grande escala. Essas práticas recomendadas são discutidas mais detalhadamente nas próximas seções do documento.

- **Identificar requisitos de destino**: Estimar out necessidades de capacidade e recursos no Azure antes de configurar a recuperação de desastres.
- **Planejar para componentes do Site Recovery**: Descobrir quais componentes do Site Recovery (servidor de configuração, servidores de processo) que você precisa para atender sua capacidade estimada.
- **Configurar um ou mais servidores de processo de escalonamento horizontal**: Não use o servidor de processo é executado por padrão no servidor de configuração. 
- **Execute as atualizações mais recentes**: A equipe de Site Recovery libera as novas versões dos componentes do Site Recovery com regularidade e verifique se que você estiver executando as versões mais recentes. Para ajudar com isso, rastrear [quais são as novidades](site-recovery-whats-new.md) para obter atualizações, e [habilitar e instalar atualizações](service-updates-how-to.md) liberam.
- **Monitorar proativamente**: À medida que você se a recuperação de desastres em funcionamento, você deve monitorar proativamente o status e a integridade dos computadores replicados e recursos de infraestrutura.
- **Recuperação de desastre**: Você deve executar simulações de recuperação de desastres regularmente. Elas não causem impacto em seu ambiente de produção, mas ajudam a garantir que o failover para o Azure funcionará conforme o esperado quando necessário.



## <a name="gather-capacity-planning-information"></a>Coletar informações de planejamento de capacidade

Colete informações sobre seu ambiente local, para ajudar a avaliar e estimar que necessidades de sua capacidade de destino (Azure).
- Para o VMware, execute o Planejador de implantação de VMs VMware fazer isso.
- Para servidores físicos, reúna as informações manualmente.

### <a name="run-the-deployment-planner-for-vmware-vms"></a>Execute o Planejador de implantação de VMs VMware

O Planejador de implantação ajuda a coletar informações sobre seu ambiente do VMware no local.

- Execute o Planejador de implantação durante um período que representa a variação típica para suas VMs. Isso irá gerar mais precisas as previsões e recomendações.
- É recomendável que você execute o Planejador de implantação no computador do servidor de configuração, uma vez que o planejador calcula a taxa de transferência do servidor no qual ele está em execução. [Saiba mais](site-recovery-vmware-deployment-planner-run.md#get-throughput) sobre como medir a taxa de transferência.
- Se você ainda não tiver um configuração de servidor de configuração:
    - [Obtenha uma visão geral](vmware-physical-azure-config-process-server-overview.md) de componentes do Site Recovery.
    - [Configurar um servidor de configuração](vmware-azure-deploy-configuration-server.md), para executar o Planejador de implantação nele.

Em seguida, execute o planejador da seguinte maneira:

1. [Saiba mais sobre](site-recovery-deployment-planner.md) o Planejador de implantação. Você pode baixar a versão mais recente do portal, ou [baixá-lo diretamente](https://aka.ms/asr-deployment-planner).
2. Examine os [pré-requisitos](site-recovery-deployment-planner.md#prerequisites) e [atualizações mais recentes](site-recovery-deployment-planner-history.md) para o Planejador de implantação, e [Baixe e extraia](site-recovery-deployment-planner.md#download-and-extract-the-deployment-planner-tool) a ferramenta.
3. [Execute o Planejador de implantação](site-recovery-vmware-deployment-planner-run.md) no servidor de configuração.
4. [Gerar um relatório](site-recovery-vmware-deployment-planner-run.md#generate-report) para resumir as estimativas e recomendações.
5. Analisar a [relatório de recomendações](site-recovery-vmware-deployment-planner-analyze-report.md) e [estimativas de custo](site-recovery-vmware-deployment-planner-cost-estimation.md).

>[!NOTE]
> Por padrão, a ferramenta está configurada para o perfil e gera relatórios para até 1.000 VMs. Você pode alterar esse limite, aumentando o valor da chave MaxVMsSupported no arquivo Asrdeploymentplanner.

## <a name="plan-target-azure-requirements-and-capacity"></a>Planejar a capacidade e requisitos de destino (Azure)

Usando as estimativas reunidas e recomendações, você pode planejar capacidade e os recursos de destino. Se você executou o Planejador de implantação de VMs VMware, você pode usar um número das [recomendações de relatório](site-recovery-vmware-deployment-planner-analyze-report.md#recommendations) para ajudá-lo.

- **VMs compatíveis**: Use esse número para identificar o número de VMs que estão prontos para recuperação de desastres no Azure. Recomendações sobre a largura de banda de rede e os núcleos do Azure baseiam-se sobre esse número.
- **Largura de banda necessária**: Observe que a largura de banda que necessária para a replicação delta de VMs compatíveis. 
    - Quando você executar o planejador é especificar o RPO desejado em minutos. As recomendações de mostram a largura de banda necessária para atender esse RPO 100% e 90% do tempo. 
    - As recomendações de largura de banda de rede levam em conta a largura de banda necessária para o número total de servidores de configuração e servidores de processo recomendados no Planner.
- **Necessário de núcleos do Azure**: Observe o número de núcleos que você precisa na região do Azure, do destino com base no número de VMs compatíveis. Se você não tiver núcleos suficientes, no failover de recuperação de Site não conseguirá criar VMs do Azure necessários.
- **Tamanho de lote VM recomendado**: O tamanho do lote recomendado se baseia a capacidade para concluir a replicação inicial para o lote dentro de 72 horas por padrão, enquanto atende a um RPO de 100%. O valor de hora pode ser modificado.

Você pode usar essas recomendações para planejar para recursos do Azure, largura de banda de rede e o envio em lote de VM.

## <a name="plan-azure-subscriptions-and-quotas"></a>Planejar as cotas e as assinaturas do Azure

Queremos garantir que as cotas disponíveis na assinatura de destino sejam suficientes para lidar com failover.

**Tarefa** | **Detalhes** | **Ação**
--- | --- | ---
**Núcleos de seleção** | Se núcleos na cota disponível não igual ou exceder a contagem total de destino no momento do failover, failovers falhará. | Para VMs do VMware, verifique a que ter núcleos suficientes na assinatura de destino para atender a recomendação de núcleo do Planejador de implantação.<br/><br/> Para servidores físicos, verifique que os núcleos do Azure atendem às suas estimativas manuais.<br/><br/> Para verificar as cotas, no portal do Azure > **assinatura**, clique em **uso + cotas**.<br/><br/> [Saiba mais](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) sobre como aumentar as cotas.
**Verifique os limites de failover** | O número de failovers não deve exceder os limites de failover do Site Recovery. |  Se failovers exceder os limites, você pode adicionar assinaturas e fazer failover para várias assinaturas ou aumentar a cota para uma assinatura. 


### <a name="failover-limits"></a>Limites de failover

Os limites de indicam o número de failover com suporte pelo Site Recovery dentro de uma hora, supondo que três discos por máquina.

O que está em conformidade média? Para iniciar uma VM do Azure, o Azure requer alguns drivers para estar no estado inicial de inicialização e de serviços como o DHCP a ser definido para iniciar automaticamente.
- As máquinas que estão em conformidade já terá essas configurações em vigor.
- Para computadores executando o Windows, proativamente você pode verificar a conformidade e torná-los em conformidade se necessário. [Saiba mais](site-recovery-failover-to-azure-troubleshoot.md#failover-failed-with-error-id-170010).
- Máquinas Linux só são trazidas para conformidade no momento do failover.

**Computador está em conformidade com o Azure?** | **Limites VM do Azure (failover de disco gerenciado)**
--- | --- 
Sim | 2000
Não  | 1000

- Limites de supõem que mínimo que outros trabalhos estão em andamento na região de destino para a assinatura.
- Algumas regiões do Azure são menores e podem ter limites de um pouco menores.

## <a name="plan-infrastructure-and-vm-connectivity"></a>Planejar a infraestrutura e conectividade de VM

Após o failover no Azure, você precisa para operar como ocorria no local e permitir que os usuários acessem as cargas de trabalho em execução nas VMs do Azure suas cargas de trabalho.

- [Saiba mais](site-recovery-active-directory.md#test-failover-considerations) sobre failover em sua infraestrutura DNS ou Active Directory local para o Azure.
- [Saiba mais](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) sobre como preparar para se conectar às VMs do Azure após o failover.



## <a name="plan-for-source-capacity-and-requirements"></a>Planejar a capacidade de origem e requisitos

É importante que você tem suficiente servidores de configuração e servidores de processo de expansão para atender aos requisitos de capacidade. Quando você começar a implantação em larga escala, começar com um servidor de configuração única e um servidor de processo de expansão. Conforme você alcançar os limites prescritos, adicione mais servidores.

>[!NOTE]
> Para VMs do VMware, o Planejador de implantação faz algumas recomendações sobre os servidores de configuração e de processo que você precisa. É recomendável que você use as tabelas incluídas nos procedimentos a seguir, em vez de seguir a recomendação do Planejador de implantação. 


## <a name="set-up-a-configuration-server"></a>Configurar um servidor de configuração
 
Capacidade do servidor de configuração é afetado pelo número de máquinas que replicam e taxa de variação não pelos dados. Para descobrir se você precisar de servidores de configuração adicionais, use esses limites VM definidos.

**CPU** | **Memória** | **Cache de disco** | **Replicado limite do computador**
 --- | --- | --- | ---
8 vCPUs<br> 2 soquetes * 4 núcleos @ 2,5 Ghz | 16 GB | 600 TB | Até 550 máquinas<br> Presume-se de que cada máquina tem três discos de 100 GB cada.

- Esses limites são baseados em um servidor de configuração usando um modelo de OVF.
- Os limites de supõem que você não estiver usando o servidor de processo é executado por padrão no servidor de configuração.

Se você precisar adicionar um novo servidor de configuração, siga estas instruções:

- [Configurar um servidor de configuração](vmware-azure-deploy-configuration-server.md) para recuperação de desastres de VM do VMware, usando um modelo de OVF.
- [Configurar um servidor de configuração](physical-azure-set-up-source.md) manualmente para servidores físicos, ou para implantações do VMware que não é possível usar um modelo de OVF.

Como configurar um servidor de configuração, observe o seguinte:

- Quando você configura um servidor de configuração, é importante considerar a assinatura e o cofre no qual ela reside, já que eles não devem ser alterados após a instalação. Se você precisar alterar o cofre, você precisa desassociar o servidor de configuração do cofre e registrá-la novamente. Isso interrompe a replicação de VMs no cofre.
- Se você quiser configurar um servidor de configuração com vários adaptadores de rede, você deve fazer isso durante o conjunto de. Você não pode fazer isso depois de registrar o servidor de configuração no cofre.

## <a name="set-up-a-process-server"></a>Configurar um servidor de processo

Capacidade do servidor de processo é afetada pelas taxas de variação de dados e não pelo número de máquinas habilitadas para replicação.

- Para grandes implantações, você deve sempre ter pelo menos um servidor de processo de escalonamento horizontal.
- Para descobrir se você precisa de servidores adicionais, use a tabela a seguir.
- É recomendável que você adicionar um servidor com a especificação mais alta. 


**CPU** | **Memória** | **Cache de disco** | **Taxa de variação**
 --- | --- | --- | --- 
12 vCPUs<br> 2 soquetes * 6 núcleos @ 2,5 Ghz | 24 GB | 1 GB | Até 2 TB por dia

Configure o servidor de processo da seguinte maneira:

1. Analise os [pré-requisitos](vmware-azure-set-up-process-server-scale.md#prerequisites).
2. Instale o servidor na [portal](vmware-azure-set-up-process-server-scale.md#install-from-the-ui), ou o [linha de comando](vmware-azure-set-up-process-server-scale.md#install-from-the-command-line).
3. Configure as máquinas replicadas para usar o novo servidor. Se você já tiver máquinas replicando:
    - Você pode [mover](vmware-azure-manage-process-server.md#switch-an-entire-workload-to-another-process-server) uma carga de trabalho do servidor de todo o processo para o novo servidor de processo.
    - Como alternativa, você pode [mover](vmware-azure-manage-process-server.md#move-vms-to-balance-the-process-server-load) VMs específicas para o novo servidor de processo.



## <a name="enable-large-scale-replication"></a>Habilitar a replicação em larga escala

Depois de planejamento de capacidade e implantar os componentes necessários e a infraestrutura, habilite a replicação para um grande número de VMs.

1. Máquinas de classificação em lotes. Habilite a replicação para VMs dentro de um lote e, em seguida, passar para o próximo lote.

    - Para VMs do VMware, você pode usar o [tamanho de lote VM recomendado](site-recovery-vmware-deployment-planner-analyze-report.md#recommended-vm-batch-size-for-initial-replication) no relatório do Planejador de implantação.
    - Para máquinas físicas, é recomendável que você identificar lotes com base em computadores que têm um tamanho semelhante e a quantidade de dados e na taxa de transferência de rede disponível. O objetivo é lote de máquinas que têm probabilidade de concluir sua replicação inicial em aproximadamente o mesmo tempo.
    
2. Se a variação de disco para uma máquina for alta ou excede os limites em thePlanner de implantação, você pode mover os arquivos não críticos, que você não precisa replicar a máquina (como despejos de log ou arquivos temporários). Para VMs do VMware, você pode mover esses arquivos em um disco separado e, em seguida [excluir esse disco](vmware-azure-exclude-disk.md) da replicação.
3. Antes de habilitar a replicação, verifique que máquinas atendam [requisitos de replicação](vmware-physical-azure-support-matrix.md#replicated-machines).
4. Configurar uma política de replicação para [VMs VMware](vmware-azure-set-up-replication.md#create-a-policy) ou [servidores físicos](physical-azure-disaster-recovery.md#create-a-replication-policy).
5. Habilitar a replicação [VMs VMware](vmware-azure-enable-replication.md) ou [servidores físicos](physical-azure-disaster-recovery.md#enable-replication). Isso inicia a replicação inicial para os computadores selecionados.

## <a name="monitor-your-deployment"></a>Monitorar a implantação

Depois de você iniciar a replicação para o primeiro lote de VMs, inicie o monitoramento de sua implantação da seguinte maneira:  

1. Atribui um administrador de recuperação de desastres para monitorar o status de integridade de computadores replicados.
2. [Monitorar eventos](site-recovery-monitor-and-troubleshoot.md) para itens replicados e a infraestrutura.
3. [Monitorar a integridade](vmware-physical-azure-monitor-process-server.md) dos seus servidores de processo de escalonamento horizontal.
4. Inscreva-se para obter [notificações por email](https://docs.microsoft.com/azure/site-recovery/site-recovery-monitor-and-troubleshoot#subscribe-to-email-notifications) para eventos, para facilitar o monitoramento.
5. Conduzir regular [recuperação de desastre](site-recovery-test-failover-to-azure.md), para garantir que tudo está funcionando conforme o esperado.


## <a name="plan-for-large-scale-failovers"></a>Plano para failovers em larga escala

No caso de desastre, você talvez precise fazer failover de um grande número de máquinas/cargas de trabalho para o Azure. Prepare para esse tipo de evento da seguinte maneira.

Você pode preparar antecipadamente para failover da seguinte maneira:

- [Prepare sua infraestrutura e as VMs](#plan-infrastructure-and-vm-connectivity) para que suas cargas de trabalho estará disponíveis após o failover e para que os usuários possam acessar as VMs do Azure.
- Observe a [limites de failover](#failover-limits) anteriormente neste documento. Verifique se que seus failovers cairá dentro desses limites.
- Executar regular [recuperação de desastre](site-recovery-test-failover-to-azure.md). Ajuda de análises para:
    - Localize lacunas na sua implantação antes do failover.
    - Estimar o RTO de ponta a ponta para seus aplicativos.
    - Estimar o RPO de ponta a ponta para suas cargas de trabalho.
    - Identifica conflitos de intervalo de endereços IP.
    - Como executar os exercícios, recomendamos que você não usar as redes de produção para análises, evite usar os mesmos nomes de sub-rede em redes de teste e produção e limpe failovers de teste após cada drill.

Para executar um failover em larga escala, recomendamos o seguinte:

1. Crie planos de recuperação para failover de carga de trabalho.
    - Cada plano de recuperação pode disparar o failover de até 50 máquinas.
    - [Saiba mais](recovery-plan-overview.md) sobre planos de recuperação.
2. Adicione scripts de runbook de automação do Azure aos planos de recuperação para automatizar as tarefas manuais no Azure. As tarefas típicas incluem configurar balanceadores de carga, atualização de DNS etc. [Saiba mais](site-recovery-runbook-automation.md)
2. Antes do failover, prepare as máquinas do Windows para que eles estejam em conformidade com o ambiente do Azure. [Limites de failover](#plan-azure-subscriptions-and-quotas) são mais altos para computadores que estão em conformidade. [Saiba mais](site-recovery-failover-to-azure-troubleshoot.md#failover-failed-with-error-id-170010) sobre runbooks.
4.  Disparar o failover com o [AzRecoveryServicesAsrPlannedFailoverJob início](https://docs.microsoft.com/powershell/module/az.recoveryservices/start-azrecoveryservicesasrplannedfailoverjob?view=azps-2.0.0&viewFallbackFrom=azps-1.1.0) cmdlet do PowerShell, junto com um plano de recuperação.



## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Recuperação de Site do monitor](site-recovery-monitor-and-troubleshoot.md)
