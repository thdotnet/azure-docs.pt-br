---
title: Migrações para Azure - Perguntas frequentes (FAQ) | Microsoft Docs
description: Apresenta respostas para perguntas frequentes sobre Migrações para Azure
author: snehaamicrosoft
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: snehaa
ms.openlocfilehash: 08a0312f12b3daab8b7f5e88da118b5bcbeb2f4c
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807319"
---
# <a name="azure-migrate---frequently-asked-questions-faq"></a>Migrações para Azure - Perguntas frequentes (FAQ)

Este artigo contém perguntas frequentes sobre o Migrações para Azure. Se você tiver mais perguntas após a leitura deste artigo, publique-as no [fórum do Migrações para Azure](https://aka.ms/AzureMigrateForum).

## <a name="general"></a>Geral

### <a name="which-azure-geographies-are-supported-by-azure-migrate"></a>Em quais regiões geográficas do Azure há suporte das Migrações para Azure?
As migrações para Azure atualmente dá suporte a um número de regiões geográficas em que um projeto de migrações para Azure pode ser criado. Mesmo que você só pode criar projetos nessas regiões, você ainda pode avaliar as máquinas para outros locais de destino. A região geográfica do projeto é usada apenas para armazenar os metadados descobertos.

**Geografia** | **local de armazenamento de metadados** Azure governamental | US gov – Virgínia Ásia | Sudeste da Ásia ou na Ásia Oriental Europa | Sul da Europa ou Europa Ocidental Reino Unido | Sul do Reino Unido ou oeste dos Estados Unidos Reino Unido | Centro dos EUA ou oeste dos EUA 2

### <a name="how-is-azure-migrate-different-from-azure-site-recovery"></a>Como o Migrações para Azure é diferente do Azure Site Recovery?

As migrações para Azure fornecem ferramentas que ajudam você a descobrir, avaliar e migrar as máquinas e cargas de trabalho para o Azure. [O Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/migrate-tutorial-on-premises-azure) é uma solução de recuperação de desastres. Os dois serviços compartilham alguns componentes.

## <a name="azure-migrate-appliance-vmwarephysical-servers"></a>Dispositivo de migrações para Azure (VMware/servidores físicos)

### <a name="how-does-the-azure-migrate-appliance-connect-to-azure"></a>Como o dispositivo de migrações para Azure se conectar ao Azure?

A conexão pode ser pela internet, ou você pode usar o ExpressRoute com emparelhamento público.

### <a name="what-network-connectivity-requirements-are-needed-for-azure-migrate-server-assessment-and-migration"></a>Quais requisitos de conectividade de rede são necessárias para a migração e avaliação de servidor de migrar do Azure

Para as URLs e portas necessárias para migrar do Azure para se comunicar com o Azure, examine os [VMWare](migrate-support-matrix-vmware.md) e [Hyper-V](migrate-support-matrix-hyper-v.md) dar suporte a matrizes.

### <a name="can-i-harden-the-appliance-vmware-vm-i-set-up-with-the-ova-template"></a>Posso proteger o dispositivo de VM do VMware configurei com o modelo OVA?

Componentes adicionais (por exemplo, um antivírus) podem ser adicionados no modelo OVA, desde que as regras de comunicação e firewall necessárias para o dispositivo de migrações para Azure re esquerda como está.   

### <a name="to-harden-the-azure-migrate-appliance-what-are-the-recommended-antivirus-av-exclusions"></a>Para reforçar o dispositivo de Migrações para Azure, quais são as exclusões de AV (antivírus) recomendadas?

Você precisará excluir as seguintes pastas da varredura no dispositivo:

- Pasta que contém os binários para o serviço de migrações para Azure. Exclua todas as subpastas.
- %ProgramFiles%\ProfilerService  
- Aplicativo Web de Migrações para Azure. Exclua todas as subpastas.
- %SystemDrive%\inetpub\wwwroot
- O cache local para arquivos de log e banco de dados. O serviço migrações para Azure precisa de leitura/gravação acesso a essa pasta.
  - %SystemDrive%\Profiler

### <a name="what-data-is-collected-by-azure-migrate"></a>Quais dados são coletados pelo Migrações para Azure?

O dispositivo de migrações para Azure coleta metadados para as VMs locais, incluindo:

**Dados de configuração da VM**
- Nome de exibição da VM (no vCenter)
- Caminho de inventário da VM (host/cluster/pasta no vCenter)
- Endereço IP
- Endereço MAC
- Sistema operacional
- Número de núcleos, discos, NICs
- Tamanho da memória, tamanhos de disco

**Dados de desempenho da VM**
- Uso da CPU
- Uso de memória
- Para cada disco anexado à VM:
  - Taxa de transferência de leitura do disco
  - Taxa de transferência de gravações de disco
  - Operações de leitura do disco por segundo
  - Operações de gravação do disco por segundo
- Para cada adaptador de rede conectado à VM:
  - Rede no
  - Limite de rede

Adição de TN, se você implantar o mapeamento de dependência, os agentes de mapeamento de dependência coletam informações que incluem o FQDN do computador, o sistema operacional, o endereço IP, o endereço MAC, processos em execução dentro da VM, entrada/saída de conexões de TCP para a VM. Essa descoberta é usada somente se você habilitar o mapeamento de dependência para a descoberta opcional.

### <a name="is-there-any-performance-impact-on-the-analyzed-esxi-host-environment"></a>Há algum impacto no desempenho no ambiente do host de ESXi analisado?

Com a criação de perfil contínua de dados de desempenho, os perfis de dispositivo de migrações para Azure máquinas locais para medir os dados de desempenho de VM. Isso tem quase zero impacto no desempenho, os hosts ESXi, bem como o vCenter Server.

### <a name="where-is-the-collected-data-stored-and-for-how-long"></a>Onde e por quanto tempo os dados coletados são armazenados?

Os dados coletados pelo dispositivo de migrações para Azure são armazenados no local do Azure que você especificar ao criar o projeto de migração. Os dados são armazenados com segurança em uma assinatura da Microsoft e são excluídos quando você excluir o projeto de migrações para Azure.

Para visualização de dependência, se você instalar agentes nas VMs, os dados coletados pelos agentes de dependência são armazenados nos EUA, no espaço de trabalho do Log Analytics criado na assinatura do Azure. Esses dados são excluídos quando você exclui o espaço de trabalho do Log Analytics na sua assinatura. [Saiba mais](concepts-dependency-visualization.md).

### <a name="what-is-the-volume-of-data-uploaded-by-azure-migrate-during-continuous-profiling"></a>O que é o volume de dados carregados pelas migrações para Azure durante a criação de perfil contínua?

O volume de dados enviados para migrações para Azure varia de acordo com vários parâmetros. Para atribuir um número de uma indicação, um projeto do migrações para Azure com 10 máquinas (cada um com um disco e uma NIC), envia cerca de 50 MB por dia. Este é um valor aproximado, que as alterações com base no número de pontos de dados para as NICs e discos (os dados enviados serão não-linear se aumentar o número de discos, NICs ou computadores).

### <a name="is-the-data-encrypted-at-rest-and-in-transit"></a>É os dados criptografados em repouso e em trânsito?

Sim para ambos. Os metadados com segurança é enviado para o serviço migrações para Azure pela internet, por meio de https. Os metadados são armazenados em uma [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/database-encryption-at-rest)e, na [armazenamento de BLOBs do Azure](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) em uma assinatura da Microsoft, e é criptografado em repouso.

Os dados coletados pelos agentes de dependência são também criptografado em trânsito (HTTPS seguro) e são armazenados em um espaço de trabalho do Log Analytics na assinatura do usuário. Também é criptografado em repouso.

### <a name="how-does-the-azure-migrate-appliance-communicate-with-the-vcenter-server-and-the-azure-migrate-service"></a>Como o dispositivo de migrações para Azure se comunicam com o vCenter Server e o serviço de migrações para Azure?

O dispositivo se conecta ao vCenter Server (porta 443) usando as credenciais fornecidas quando você configura o dispositivo. Ele consulta o vCenter Server usando o VMware PowerCLI para reunir metadados sobre as VMs gerenciadas pelo vCenter Server. Ele coleta ambos os dados de configuração sobre máquinas virtuais (núcleos, memória, discos, etc. NIC), bem como o histórico de desempenho de cada VM para o último mês. Os metadados coletados é enviado para o Azure Server avaliação das migrações (pela internet por meio de HTTPS) para avaliação. 

### <a name="can-i-connect-the-same-appliance-to-multiple-vcenter-servers"></a>Posso conectar o mesmo dispositivo em vários servidores do vCenter?

Sim, um único dispositivo de migrações para Azure pode ser usado para descobrir o vCenter vários servidores, mas não ao mesmo tempo. Você precisa executar descobertas uma após a outra.

### <a name="is-the-ova-template-used-by-site-recovery-integrated-with-the-ova-used-by-azure-migrate"></a>O modelo OVA usado pelo Site Recovery é integrado ao OVA usado pelas Migrações para Azure?

Atualmente, não há integração. O modelo .OVA no Site Recovery é usado para configurar um servidor de configuração do Site Recovery para replicação de VM VMware/servidor físico. A. OVA usado pelas migrações para Azure é usado para descobrir máquinas virtuais VMware gerenciadas por um vCenter server, para fins de avaliação e migração.

### <a name="i-changed-my-machine-size-can-i-rerun-an-assessment"></a>Alterei o tamanho da máquina. Executo novamente uma avaliação?
O dispositivo de migrações para Azure continuamente coleta informações sobre o ambiente local. No entanto, uma avaliação é um instantâneo point-in-time de VMs locais. Se você alterar as configurações em uma VM que você deseja avaliar, use a opção 'Recalculate' para a avaliação de atualização com as últimas alterações.

### <a name="how-can-i-discover-a-multi-tenant-environment-in-azure-migrate"></a>Como é possível descobrir um ambiente multilocatário no Migrações para Azure?

Para o VMware, se você tiver um ambiente que é compartilhado entre locatários, e você não quiser descobrir as VMs de um locatário em outra assinatura de locatário, crie vCenter credenciais do servidor com acesso somente para as VMs que você deseja descobrir. Em seguida, use as credenciais quando iniciar descoberta no dispositivo de migrações para Azure.

Para Hyper-V, a descoberta usa as credenciais de host do Hyper-V, não se as VMs compartilharem o mesmo host do Hyper-V, há atualmente nenhuma maneira de separar a descoberta.  

### <a name="how-many-vms-can-be-discovered-using-a-single-migration-appliance"></a>Quantas VMs podem ser descobertas usando um dispositivo de migração único?

Você pode descobrir até 10.000 VMs do VMware e até 5.000 VMs Hyper-V usando um dispositivo de migração única.  Se você tiver mais máquinas no seu ambiente local, saiba como dimensionar [Hyper-V](scale-hyper-v-assessment.md) e [VMware](scale-vmware-assessment.md) avaliação.


## <a name="azure-migrate-server-assessment"></a>Migrações para Azure: Avaliação de servidor

### <a name="does-azure-migrate-server-assessment-support-assessment-of-physical-servers"></a>Migrações para Azure: Avaliação de servidor dá suporte à avaliação de servidores físicos?

Não, migrações para Azure atualmente não dá suporte à avaliação de servidores físicos. 

### <a name="does-azure-migrate-need-vcenter-server-to-discover-a-vmware-environment"></a>O Migrações para Azure precisa do vCenter Server para descobrir um ambiente VMware?

Sim, o migrações para Azure precisa vCenter Server para descobrir um ambiente VMware. Ele não dá suporte a descoberta de hosts ESXi que não são gerenciadas pelo vCenter Server.

### <a name="whats-the-difference-between-using-azure-migrate-server-assessment-and-the-map-toolkit"></a>Qual é a diferença entre o uso de migrações para Azure: Avaliação de servidor e o Map Toolkit?

Migrações para Azure: Avaliação de servidor fornece avaliação de migração para ajudar na preparação para migração e avaliação de cargas de trabalho para a migração para o Azure. [Microsoft Assessment and Planning (MAP) Toolkit](https://www.microsoft.com/download/details.aspx?id=7826) tem outras funcionalidades, como planejamento de migração para versões mais recentes dos sistemas de operacionais de cliente e servidor Windows e acompanhamento de uso do software. Para esses cenários, continue a usar o Kit de ferramentas MAP.

### <a name="how-is-azure-migrate-server-assessment-different-from-azure-site-recovery-deployment-planner"></a>Como é o migrações para Azure: Avaliação de servidor diferente do Planejador de implantação do Azure Site Recovery?

Migrações para Azure: Avaliação de servidor é uma ferramenta de planejamento de migração. Planejador de implantação do Azure Site Recovery é uma ferramenta de planejamento de recuperação de desastre.

- **Migração de VMware/Hyper-V para Azure**: Se você pretende migrar seus servidores locais para o Azure, use as migrações para Azure: Ferramenta de avaliação do servidor para planejamento de migração. A ferramenta avalia cargas de trabalho local e fornece diretrizes, insights e mecanismos para ajudar você a migrar para o Azure. Quando estiver pronto com seu plano de migração, você pode usar ferramentas como o migrações para Azure: Migração do servidor, para migrar as máquinas para o Azure.
- **Recuperação de desastre do VMware/Hyper-V para o Azure**: Recuperação de desastres para o Azure usando o Site Recovery, use o Planejador de implantação do Site Recovery para o planejamento de recuperação de desastres. Planejador de implantação da recuperação de site faz uma profunda, avaliação de específico de recuperação de Site do seu ambiente local. Ele fornece recomendações necessárias pelo Site Recovery para as operações de desastres com êxito, como replicação e failover de máquinas virtuais. 

### <a name="does-azure-migrate-support-enterprise-agreement-ea-based-cost-estimation"></a>Migrações para Azure dá suporte a estimativa de custo com base em EA Enterprise Agreement?

As migrações para Azure atualmente não dá suporte a estimativa de custo para [oferta Enterprise Agreement](https://azure.microsoft.com/offers/enterprise-agreement-support/). A solução alternativa é especificar o pagamento conforme o uso como a oferta e especificar manualmente a porcentagem de desconto (aplicável para a assinatura) no campo 'Desconto' de propriedades de avaliação.

  ![Desconto](./media/resources-faq/discount.png)

### <a name="whats-the-difference-between-as-on-premises-sizing-and-performance-based-sizing"></a>Qual é a diferença entre o dimensionamento como local e o dimensionamento com base no desempenho?

- Em como dimensionamento, no local migrações para Azure não considera os dados de desempenho de VM. Ele dimensiona as VMs com base na configuração local. -Em dimensionamento baseado em desempenho, dimensionamento é baseado nos dados de utilização.
- Por exemplo, se uma VM local tem 4 núcleos e 8 GB de memória com 50% da utilização da CPU e 50% de utilização de memória, como local dimensionamento recomenda um SKU de VM do Azure com 4 núcleos e 8GB de memória. Com base no desempenho de dimensionamento, no entanto, recomenda um SKU de VM de 2 núcleos e 4 GB, como a porcentagem de utilização é considerada.
- Da mesma forma, o dimensionamento de disco depende de duas propriedades de avaliação - tipo de armazenamento e de critério de dimensionamento.
= Se o critério de dimensionamento for baseado em desempenho e o tipo de armazenamento é automático, os valores IOPS e taxa de transferência do disco são considerados ao identificar o tipo de disco de destino (Standard ou Premium).
- Se o critério de dimensionamento for baseado em desempenho e é o tipo de armazenamento premium, é recomendável um disco premium. O disco premium que SKU é selecionado com base no tamanho do disco local. A mesma lógica é usada para dimensionamento do disco quando o critério de dimensionamento é como dimensionamento local, e o tipo de armazenamento é standard ou premium.

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>Que impacto que a utilização do percentil e o histórico de desempenho têm sobre as recomendações de tamanho?

Essas propriedades só são aplicáveis ao dimensionamento com base no desempenho.

- As migrações para Azure coleta o histórico de desempenho das máquinas locais e as usa para recomendar o tipo de disco e tamanho VM no Azure.
- O dispositivo de perfis continuamente o ambiente local para coletar dados de utilização em tempo real a cada 20 segundos. O dispositivo acumula as amostras de 20 segundos e cria um único ponto de dados a cada 15 minutos. Para criar o ponto de dados único, o dispositivo seleciona o valor de pico de todas as amostras de 20 segundos e envia-o para o Azure.
- Quando você cria uma avaliação no Azure (com base na duração de desempenho e valor de percentil de histórico de desempenho), migrações para Azure calcula o valor de utilização eficiente e usa-o para o dimensionamento.
- Por exemplo, se você definir a duração de desempenho para ser um dia e o valor do percentil 95 Percentil, migrações para Azure usa os pontos de amostra minuto 15 enviados pelo coletor para o último dia, classifica em ordem crescente e seleciona o valor do 95 º percentil como o utilização eficiente.
- O valor do 95 º percentil garante que você está ignorando quaisquer exceções que pode ocorrer se você usar o 99 º percentil. Se você deseja escolher o pico de uso para o período e não quiser perder exceções, selecione o 99 º percentil.

### <a name="what-is-dependency-visualization"></a>O que é a visualização de dependência?

Visualização de dependência permite que você avaliar grupos de VMs para a migração com maior confiança. Ele verificações dependências da máquina antes de executar uma avaliação. Visualização de dependência ajuda a garantir que nada seja deixado para trás e evitar interrupções inesperadas quando você migra para o Azure. As migrações para Azure aproveita a solução Mapa do serviço nos logs do Azure Monitor, para habilitar a visualização de dependência.

> [!NOTE]
> A funcionalidade de visualização de dependências não está disponível no Azure Governamental.

### <a name="do-i-need-to-pay-to-use-the-dependency-visualization-feature"></a>É necessário pagar para usar o recurso de visualização de dependência?

Nº [Saiba mais](https://azure.microsoft.com/pricing/details/azure-migrate/) sobre os preços das Migrações para Azure.

### <a name="do-i-need-to-install-anything-for-dependency-visualization"></a>É necessário instalar alguma coisa para a visualização de dependência?

Para usar a visualização de dependência, você precisa fazer o download e instalar agentes em cada computador local que você deseja avaliar.

- O [Microsoft Monitoring agent (MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) precisa ser instalado em cada computador.
- O [Agente de Dependência](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure) precisa ser instalado em cada máquina.
- Além disso, se você tiver máquinas sem conectividade com a Internet, será necessário fazer o download e instalar o gateway do Log Analytics nelas.

Esses agentes não é necessário, a menos que você está usando a visualização de dependência.

### <a name="can-i-use-an-existing-workspace-for-dependency-visualization"></a>Posso usar um workspace existente para visualização de dependência?

Sim, você pode anexar a um espaço de trabalho para o projeto de migração e aproveitá-lo para visualização de dependência. [Saiba mais](concepts-dependency-visualization.md#how-does-it-work).

### <a name="can-i-export-the-dependency-visualization-report"></a>Posso exportar o relatório de visualização de dependência?

Não, a visualização de dependência não pode ser exportada. No entanto, como as Migrações para Azure usam o Mapa do Serviço para visualização de dependência, você pode usar as [APIs REST do Mapa do Serviço](https://docs.microsoft.com/rest/api/servicemap/machines/listconnections) para obter as dependências em um formato json.

### <a name="how-can-i-automate-the-installation-of-microsoft-monitoring-agent-mma-and-dependency-agent"></a>Como posso automatizar a instalação do Microsoft Monitoring Agent (MMA) e do agente de dependência?

[Use este script](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#installation-script-examples) para instalação dos agentes. [Siga estas instruções](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#install-and-configure-agent) para instalar o MMA usando a linha de comando ou a automação. Para MMA, aproveitar [esse script](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab).

Além dos scripts, você pode usar ferramentas de implantação, como o System Center Configuration Manager, [Intigua](https://www.intigua.com/getting-started-intigua-for-azure-migration) etc. para implantar os agentes.

### <a name="what-operating-systems-are-supported-by-mma"></a>Quais sistemas operacionais têm suporte pelo MMA?

- [Revisão](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems) a lista de sistemas de operacionais do Windows compatíveis com o MMA.
- [Examinar] https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) a lista de sistemas operacionais de Linux com suporte pelo MMA.

### <a name="what-are-the-operating-systems-supported-by-the-dependency-agent"></a>Quais são os sistemas operacionais suportados pelo agente de dependência?

[Revisão](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems) os sistemas operacionais do Windows compatíveis com o agente de dependência.
[Revisão](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems) a lista de sistemas operacionais de Linux com suporte pelo agente de dependência.

### <a name="can-i-visualize-dependencies-in-azure-migrate-for-more-than-an-hour"></a>Pode visualizar as dependências nas migrações para Azure por mais de uma hora?
Não, você pode visualizar as dependências para até uma hora. Você pode voltar para uma data específica no histórico, até o último mês, mas a duração máxima para a visualização é uma hora. Por exemplo, você pode usar o tempo de duração no mapa de dependência para exibir as dependências para ontem, mas só pode exibi-lo para uma janela de uma hora. No entanto, você pode usar os logs do Azure Monitor [consultar dados de dependência](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) em um período mais longo.

### <a name="is-dependency-visualization-supported-for-groups-with-more-than-ten-vms"></a>Visualização de dependência tem suporte para grupos com mais de dez VMs?
Você pode [visualize as dependências para grupos](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) com até dez VMs. Se você tiver um grupo com mais de dez VMs, recomendamos que você dividir o grupo em grupos menores e, em seguida, visualizar as dependências.

## <a name="azure-migrate-server-migration"></a>Migrações para Azure: Migração do servidor

### <a name="how-is-azure-migrate-server-migration-different-from-azure-site-recovery"></a>Como é o migrações para Azure: Migração do servidor diferente do Azure Site Recovery?

Migrações para Azure: Migração do servidor aproveita o mecanismo de replicação do Site Recovery para a migração baseada em agentes de servidores no Azure.
## <a name="next-steps"></a>Próximas etapas
Leia a [visão geral do Migrações para Azure](migrate-services-overview.md)
