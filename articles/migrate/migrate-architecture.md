---
title: Arquitetura de migração do Azure | Microsoft Docs
description: Fornece uma visão geral do serviço Migrações para Azure.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: raynew
ms.openlocfilehash: 16fbf8d3523ac2ab36447aa51a93a0bb5a9fad54
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811421"
---
# <a name="azure-migrate-architecture-and-processes"></a>Processos e a arquitetura de migrações para azure

[As migrações para Azure](migrate-overview.md) fornece um hub de ferramentas que ajudam você a descobrir, avaliar e migrar cargas de trabalho, infraestrutura e aplicativos para o Microsoft Azure. O hub inclui ferramentas de migrações para Azure e as ofertas do ISV (fornecedor) independentes de software de terceiros. 

 Este artigo resume a avaliação e arquitetura de migração e processos para avaliação de servidor de migrar do Azure e migração do servidor de migrar do Azure.

## <a name="assessment-with-azure-migrate-server-assessment"></a>Avaliação de servidor de migrar de avaliação com o Azure

Avaliação de servidor de migrar do Azure pode avaliar as VMs VMware e VMs do Hyper-V para a migração para o Azure. Avaliação funciona da seguinte maneira:

1. **Preparar para a avaliação**: No local, você configura uma leve as migrações para Azure dispositivo local como uma VM VMware ou VM do Hyper-V e registre-se o dispositivo com as migrações para Azure.
2. **Descobrir VMs**: O dispositivo de migrações para Azure é executado para descobrir VMs locais. 
    - Nada precisa ser instalado em VMs descobertas.
    - VM metadados incluem informações sobre os núcleos, memória, discos, tamanhos de disco e adaptadores de rede.
    - Dados de desempenho incluem informações sobre CPU e memória, uso, IOPS, taxa de transferência do disco (MBps) de disco e rede de saída (MBps)
- **Coletar e avaliar computadores**: Após a conclusão da descoberta, no Azure portal que você colete VMs descobertas em grupos que normalmente consistem em VMs que você deseja migrar em conjunto. Executar uma avaliação em um grupo.


## <a name="vmware-assessment"></a>Avaliação do VMware 

O diagrama resume como avaliação de VMs VMware funciona usando a avaliação de servidor de migrar do Azure.

![Arquitetura de avaliação da VMware](./media/migrate-architecture/sas-architecture-vmware.png)

O processo é o seguinte:

1. **Implantar o dispositivo de migrações para Azure**:

    a. Baixe o modelo (OVA) do portal do Azure.

    b. Importá-lo à máquina do servidor vCenter para criar a VM.

    c. Conectar-se a essa VM, definir as configurações básicas para ele e registrá-lo com as migrações para Azure.

2. **Iniciar descoberta**:

    a. Conecte-se para o aplicativo de coletor em execução no dispositivo para iniciar a descoberta.

    b. O dispositivo envia continuamente os metadados e dados de desempenho das VMs no Azure.

    - O dispositivo sempre é conectado ao serviço migrações para Azure.
    - As alterações de ambiente durante a descoberta contínua são capturadas. Por exemplo, adição de VMs no escopo de descoberta ou adicionando discos de VM ou NICs.

3. **Avaliar máquinas**:

    a. Após a conclusão da descoberta, no Azure portal que você colete VMs descobertas em grupos.  Normalmente, um grupo consiste em VMs que você deseja migrar em conjunto.

    b. Execute uma avaliação para cada grupo.

4. **Examinar a avaliação**: 

    a. Após a conclusão da avaliação, exibi-lo no portal.

    b. Para análise posterior, baixe a avaliação no formato do Excel.



### <a name="vmware-ports"></a>Portas do VMware
As migrações para Azure usa as seguintes portas de conexão para o VMware:

**Source** | **Destino** | **Porta** | **Detalhes**
--- | --- | --- | ---
Dispositivo de migrações para Azure | Serviço Migrações para Azure | Destino TCP 443 | Envie dados de desempenho e metadados para migrações para Azure.
Dispositivo de migrações para Azure | vCenter Server | Destino TCP 443 | Conecte-se ao servidor do vCenter para metadados e dados de desempenho. 443 é o padrão, mas pode ser modificado com vCenter escuta em uma porta diferente. 
Cliente RDP | Dispositivo de migrações para Azure | Target-TCP3389 | Conexão de RDP para descoberta de gatilho do dispositivo.

### <a name="collected-vmware-metadata"></a>Metadados coletados do VMware

O dispositivo envia metadados e dados relacionados ao desempenho das VMs no Azure.

**Ação** | **Detalhes**
--- | ---
**Metadados coletados** | nome VM do vCenter<br/> caminho de VM do vCenter (pasta de host/cluster)<br/> Endereços IP e MAC<br/> Sistema operacional<br/> Número de discos/núcleos/NICs<br/> Tamanho de memória e disco.
**Dados de desempenho coletados** | Uso de CPU/memória<br/> Dados de disco (taxa de transferência de leitura/gravação de disco; leituras/gravações de disco por segundo) por<br/> Dados NIC (rede de saída da rede).<br/><br/> Dados de desempenho são coletados continuamente depois que o dispositivo se conecta ao vCenter Server. Os dados históricos não são coletados.

## <a name="hyper-v-assessment"></a>Avaliação do Hyper-V

O diagrama resume como funciona de avaliação do Hyper-V sing avaliação das migrações Server do Azure.

![Arquitetura de avaliação do Hyper-V](./media/migrate-architecture/sas-architecture-hyper-v.png)

O processo é o seguinte:

1. **Criar o dispositivo de migrações para Azure**:

    a. Baixe a VM em formato compactado do portal do Azure.

    b. Importá-lo para um host Hyper-V.

    c. Conecte-se para o dispositivo de VM. Definir as configurações básicas para ele e registrá-lo com as migrações para Azure.

2. **Iniciar descoberta**:

    a. Conecte-se para o dispositivo de migrações para Azure para iniciar a descoberta. Nada precisa ser instalado em VMs descobertas.

    b. O dispositivo envia continuamente os dados de metadados e o desempenho da VM para migrações para Azure.

    - O dispositivo sempre é conectado ao serviço de migrações para Azure (usando sessões CIM).
    - As alterações de ambiente durante a descoberta contínua são capturadas. Por exemplo, adição de VMs no escopo de descoberta ou adicionando discos de VM ou NICs.


3. **Avaliar máquinas**:

    a. Após a conclusão da descoberta, no Azure portal que você colete VMs descobertas em grupos.  Normalmente, um grupo consiste em VMs que você deseja migrar em conjunto.

    b. Execute uma avaliação para cada grupo.

4. **Examinar a avaliação**:

    a. Após a conclusão da avaliação, exibi-lo no portal.

    b. Para análise posterior, baixe a avaliação no formato do Excel.

### <a name="hyper-v-ports"></a>Portas do Hyper-V

Serviço migrações para Azure usa as seguintes portas de conexão para o Hyper-v:

**Source** | **Destino** | **Porta** | **Detalhes**
--- | --- | --- | ---
Dispositivo de migrações para Azure | Serviço Migrações para Azure | Destino TCP 443 | Envie dados de desempenho e metadados para migrações para Azure.
Dispositivo de migrações para Azure | Host/cluster do Hyper-V | Portas de destino padrão WinRM-HTTP:5985; HTTPS:5986 | Conecte-se ao host para metadados e dados de desempenho. Sessão CIM usado para conexão
Cliente RDP | Dispositivo de migrações para Azure | Target-TCP3389 | Conexão de RDP para descoberta de gatilho do dispositivo.

## <a name="collected-hyper-v-vm-metadata"></a>Metadados coletados de VM do Hyper-V

O dispositivo envia metadados e dados relacionados ao desempenho das VMs no Azure.


**Ação** | **Detalhes**
--- | ---
**Metadados coletados** | Nome da VM<br/> Caminho VM (cluster de host/pasta)<br/> Endereços IP e MAC<br/> Sistema operacional<br/> Número de discos/núcleos/NICs<br/> Tamanho de disco e memória<br/> Disco IOPS, taxa de transferência do disco (MBps), a saída de rede (MBps)
**Dados de desempenho coletados** |  Uso de CPU/memória<br/> Dados de disco (taxa de transferência de leitura/gravação de disco; leituras/gravações de disco por segundo) por<br/> Dados NIC (rede de saída da rede).<br/><br/> Dados de desempenho são coletados continuamente depois que o dispositivo se conecta ao host do Hyper-V. Os dados históricos não são coletados.




## <a name="migration-with-azure-migrate-server-migration"></a>A migração com o Azure migrar a migração do servidor

Usando a migração de servidor de migrar do Azure, você pode migrar o seguinte para o Azure:

- VMs VMware locais
- VMs do Hyper-V local
- Servidores físicos locais
- Instâncias de VM do Windows AWS
- Instâncias de VM do Windows GCP

O processo de migração é ligeiramente diferente, dependendo do que você está migrando.


## <a name="migrate-vmware-vms"></a>Migrar VMs VMware

Migração de servidor de migrar do Azure oferece dois métodos para migrar VMs do VMware no local:

- **A replicação sem agente**: Migre VMs sem precisar instalar nada neles.
- **Replicação baseada em agente**. Instale um agente na VM para replicação.

Embora a replicação sem agente é mais fácil de uma perspectiva de implantação, ele atualmente tem várias limitações. [Saiba mais](server-migrate-overview.md) sobre essas limitações.
 

### <a name="agent-based-migration"></a>Migração com base em agente

Com base em agente de migração de máquinas virtuais VMware requer um número de componentes a serem implantados, como resumido na tabela.

**Componente** | **Requisito** | **Detalhes**
--- | --- | ---
**Máquina do servidor de configuração** | Uma única VM VMware que é implantado a partir de um modelo OVF baixado no local.<br/><br/> A máquina executa todos os componentes do Site Recovery no local, que incluem o servidor de configuração e o servidor de processo. | **Servidor de configuração**: Coordena as comunicações entre o local e o Azure e gerencia a replicação de dados.<br/><br/> **Servidor de processo**: Instalado por padrão no servidor de configuração. Ele recebe dados de replicação. Otimiza-os com caching, compactação e criptografia; e o envia para o Azure. O servidor de processo também instala o serviço de mobilidade do Azure Site Recovery nas VMs e executa a descoberta automática de máquinas locais.
**Serviço de mobilidade** | O serviço de mobilidade deve ser instalado em cada VM VMware que você replicar. | É recomendável permitir a instalação automática do servidor em processo. Como alternativa, você pode instalar o serviço manualmente ou usar um método de implantação automatizado como o System Center Configuration Manager.





### <a name="agent-based-replication-process"></a>Processo de replicação com base em agente

![Processo de replicação](./media/migrate-architecture/v2a-architecture-henry.png)

1. Quando você habilitar a replicação para uma VM, inicia a replicação inicial para o Azure. 
    - A replicação está em nível de bloco, quase contínua, usando o agente de serviço de mobilidade em execução na VM.
    - Configurações de política de replicação são aplicadas:
        - **Limite de RPO**. Essa configuração não afeta a replicação. Isso ajuda no monitoramento. Um evento é gerado e, opcionalmente, um email enviado, se o RPO atual exceder o limite que você especificar.
        - **Retenção do ponto de recuperação**. Essa configuração especifica o tempo retroativo da recuperação quando ocorrer uma interrupção. A retenção máxima no armazenamento Premium é de 24 horas. No armazenamento padrão, são 72 horas. 
        - **Instantâneos consistentes com o aplicativo**. Um instantâneo consistente com o aplicativo pode de demorar de 1 a 12 horas, dependendo das necessidades do aplicativo. Instantâneos são instantâneos de blob padrão do Azure. O agente de Mobilidade executando em uma VM solicita um instantâneo de VSS de acordo com essa configuração e indica esse ponto no tempo como um ponto consistente do aplicativo no fluxo de replicação.

2. O tráfego é replicado para pontos de extremidade públicos do Armazenamento do Microsoft Azure pela Internet.

    - Como alternativa, você pode usar o Azure ExpressRoute com [emparelhamento da Microsoft](../expressroute/expressroute-circuit-peerings.md#microsoftpeering).
    - Não há suporte para replicação de tráfego através de uma VPN (rede virtual privada) site a site de um site local para o Azure.
3. Após a conclusão da replicação inicial, a replicação de alterações delta para o Azure é iniciada. As alterações controladas de uma máquina são enviadas para o servidor de processo.
2. A comunicação ocorre da seguinte maneira:

    - As VMs se comunicam com o servidor de configuração local na porta HTTPS 443 de entrada para o gerenciamento de replicação.
    - O servidor de configuração coordena a replicação com o Azure pela porta HTTPS 443 de saída.
    - As VMs que estão sendo replicadas enviam dados de replicação para o servidor de processo (em execução no computador do servidor de configuração) na porta 9443 HTTPS de entrada. Essa porta pode ser modificada.
    - O servidor de processo recebe dados de replicação, otimiza-os e criptografa-os e os envia para o armazenamento do Microsoft Azure pela porta 443 de saída.
5. Os dados de replicação registra land primeiro em uma conta de armazenamento de cache no Azure. Esses logs são processados e os dados são armazenados no Azure (disco de semente do Azure Site Recovery) de disco gerenciado. Os pontos de recuperação são criados no disco.







## <a name="next-steps"></a>Próximas etapas

- [Reveja as perguntas frequentes](resources-faq.md) sobre Migrações para Azure.
- Para obter ajuda da comunidade, visite o [Fórum do MSDN do Azure migrar](https://social.msdn.microsoft.com/Forums/home?forum=AzureMigrate&filter=alltypes&sort=lastpostdesc) ou [Stack Overflow](https://stackoverflow.com/search?q=azure+migrate).

