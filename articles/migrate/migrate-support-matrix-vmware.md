---
title: Matriz de suporte de migrações do Azure para VMware avaliação e migração
description: Resume as configurações de suporte e limitações para avaliação e migração de VMs VMware no Azure usando o serviço migrações para Azure.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: raynew
ms.openlocfilehash: 567a6582e193208a7ff4aa37bafefe1dec4f4e8f
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811577"
---
# <a name="support-matrix-for-vmware-assessment-and-migration"></a>Matriz de suporte para migração e de avaliação do VMware

Você pode usar o [serviço migrações para Azure](migrate-overview.md) para avaliar e migrar as máquinas para a nuvem do Microsoft Azure. Este artigo resume as configurações de suporte e limitações para avaliar e migrar VMs do VMware local.


## <a name="vmware-scenarios"></a>Cenários de VMware

A tabela resume os cenários com suporte para VMs do VMware.

**Implantação** | **Detalhes** 
--- | --- 
**Avaliar as VMs VMware locais** | [Configurar](tutorial-prepare-vmware.md) sua primeira avaliação.<br/><br/> [Executar](scale-vmware-assessment.md) uma avaliação em larga escala.
**Migrar VMs VMware** | Você pode migrar usando a migração sem agente, com algumas limitações, ou usar uma migração baseada em agente. [Saiba mais](server-migrate-overview.md)


    


## <a name="azure-migrate-projects"></a>Projetos de migração do Azure

**Suporte** | **Detalhes**
--- | ---
Permissões do Azure | Você precisa de permissões de Colaborador ou proprietário na assinatura para criar um projeto de migrações para Azure.
Limitações da VMware  | Avalie até 35.000 VMs do VMware em um único projeto.

Um projeto pode incluir VMs VMware e VMs do Hyper-V, até os limites de avaliação.

## <a name="assessment-vmware-server-requirements"></a>Requisitos do servidor VMware de avaliação

Esta tabela resume o suporte de avaliação e limitações para servidores de virtualização do VMware.

**Suporte** | **Detalhes**
--- | ---
**vCenter Server** | VMs VMware que você deseja avaliar deve ser gerenciadas por um ou mais servidores vCenter 5.5, 6.0, 6.5 ou 6.7 de execução.

## <a name="assessment-vcenter-server-permissions"></a>Permissões de servidor do vCenter de avaliação

Para avaliação, você precisa de uma conta de somente leitura para o vCenter Server.

## <a name="assessment-appliance-requirements"></a>Requisitos da ferramenta de avaliação

**Suporte** | **Detalhes**
--- | ---
**ESXi** | O dispositivo de VM deve ser implantado em um host ESXi executando a versão 5.5 ou posterior.
**Projeto de migrações do Azure** | Um dispositivo pode ser associado um único projeto.
**vCenter Server** | Um dispositivo pode descobrir até 10.000 VMs do VMware em um servidor vCenter.<br/> Um dispositivo pode se conectar a um vCenter Server.


## <a name="assessment-url-access-requirements"></a>Requisitos de acesso de URL de avaliação

O dispositivo de migrações para Azure precisa de conectividade de internet à internet.

- Quando você implanta o dispositivo, o migrações para Azure faz uma verificação de conectividade às URLs resumidos na tabela a seguir.
- Se você estiver usando um firewall.proxy baseado em URL, permita o acesso a essas URLs, certificando-se de que o proxy resolve quaisquer registros CNAME recebido ao pesquisar as URLs.

**URL** | **Detalhes**  
--- | --- |
*. portal.azure.com  | Navegue até as migrações para Azure no portal do Azure.
*.windows.net | Faça logon em sua assinatura do Azure.
*.microsoftonline.com | Crie aplicativos do Active Directory para o dispositivo para se comunicar com o serviço migrações para Azure.
management.azure.com | Crie aplicativos do Active Directory para o dispositivo para se comunicar com o serviço migrações para Azure.
dc.services.visualstudio.com | Carregar logs de aplicativo usados para o monitoramento interno.
*.vault.azure.net | Gerencie segredos no cofre de chaves do Azure.
*.servicebus.windows.net | Comunicação entre o dispositivo e o serviço migrações para Azure.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Conecte-se a URLs de serviço migrações para Azure.
*.blob.core.windows.net | Carregar dados para as contas de armazenamento.


## <a name="assessment-port-requirements"></a>Requisitos de porta de avaliação

**Dispositivo** | **Conexão**
--- | --- 
Dispositivo | Conexões de entrada na porta TCP 3389 para permitir conexões da área de trabalho remota para o dispositivo.<br/> Conexões na porta 44368 para acessar remotamente o aplicativo de gerenciamento de dispositivo usando a URL de entrada: https://<appliance-ip-or-name>:44368 <br/>Conexões de saída na porta 443 para enviar os metadados de descoberta e o desempenho ao migrar do Azure.
Servidor vCenter | Conexões na porta TCP 443 para permitir que o dispositivo de coletar metadados de configuração e desempenho para as avaliações de entrada. <br/> O dispositivo se conecta ao vCenter na porta 443 por padrão. Se o servidor do vCenter escutar em uma porta diferente, você pode modificar a porta quando configurar a descoberta.


## <a name="agentless-migration-vmware-server-requirements"></a>Requisitos do servidor VMware de migração sem agente

Esta tabela resume o suporte de avaliação e limitações para servidores de virtualização do VMware.

**Suporte** | **Detalhes**
--- | ---
**vCenter Server** | VMs VMware que você migrar usando uma migração sem agente deve ser gerenciadas por um ou mais servidores vCenter 5.5, 6.0, 6.5 ou 6.7 de execução.

## <a name="agentless-migration-vcenter-server-permissions"></a>Permissões de servidor do vCenter de migração sem agente

**Permissões** | **Detalhes**
--- | --- 
Datastore.Browse | Permita navegação na VM arquivos de log para solucionar problemas de instantâneo de criação e exclusão.
Datastore.LowLevelFileOperations | Permitir operações de leitura/gravação/delete/rename no navegador de repositório de dados, para solucionar problemas de instantâneo de criação e exclusão.
VirtualMachine.Configuration.DiskChangeTracking | Permitir habilitar ou desabilitar o controle de alteração de discos VM, para efetuar pull dos dados entre os instantâneos, os blocos alterados
VirtualMachine.Configuration.DiskLease | Permitir operações de concessão de disco para uma VM, ler o disco usando o Kit de desenvolvimento de disco Virtual (VDDK) do VMware vSphere.
VirtualMachine.Provisioning.AllowReadOnlyDiskAccess | Permitir a abertura de um disco em uma VM, para ler o disco usando o VDDK.
VirtualMachine.Provisioning.AllowVirtualMachineDownload  | Permite operações de leitura em arquivos associados a uma VM, para baixar os logs e solucionar problemas se ocorrer falha. 
VirtualMachine.SnapshotManagement.* | Permitir a criação e gerenciamento de instantâneos da VM para replicação. 
Virtual Machine.Interaction.Power desativado | Permitir que a VM seja desligado durante a migração para o Azure.


## <a name="agentless-migration-vmware-vm-requirements"></a>Requisitos de VM do VMware de migração sem agente

**Suporte** | **Detalhes**
--- | ---
**Sistemas operacionais com suporte** | [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) e [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) sistemas operacionais com suporte pelo Azure podem ser migrados usando a migração sem agente.
**Alterações necessárias para o Azure** | Algumas VMs podem exigir alterações para que eles podem ser executados no Azure. As migrações para Azure faz essas alterações automaticamente para os seguintes sistemas operacionais:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> - SUSE Linux Enterprise Server 12 SP1+<br/> - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS<br/> - Debian 7,8<br/><br/> Para outros sistemas operacionais, você precisa fazer ajustes manualmente antes da migração. Os artigos relevantes contém instruções sobre como fazer isso.
**Inicialização do Linux** | Se /boot estiver em uma partição dedicada, ele deve residir no disco do SO e não sejam distribuído em vários discos.<br/> Se /boot faz parte da partição raiz (/), em seguida, a partição '/' deve estar no disco do sistema operacional e não abrangem outros discos.
**Inicialização de UEFI** | Máquinas virtuais com inicialização UEFI não têm suporte para a migração.
**Discos/volumes criptografados** | VMs com discos/volumes criptografados não têm suporte para a migração.
**Discos de passagem/RDM** | Se as VMs tiverem discos RDM ou passagem, esses discos não serão duplicados para o Azure.
**NFS** | Volumes NFS montados como volumes nas máquinas virtuais não serão duplicados.
**Disco de destino** | VMs só podem ser migradas para discos gerenciados (HDD standard, premium SSD) no Azure.


## <a name="agentless-migration-appliance-requirements"></a>Requisitos da ferramenta de migração sem agente


**Suporte** | **Detalhes**
--- | ---
**ESXi** | O dispositivo de VM deve ser implantado em um host ESXi executando a versão 5.5 ou posterior.
**Projeto de migrações do Azure** | Um dispositivo pode ser associado um único projeto.
**vCenter Server** | Um dispositivo pode descobrir até 10.000 VMs do VMware em um servidor vCenter.<br/> Um dispositivo pode se conectar a um vCenter Server.
**VDDK** | Se você estiver executando uma migração sem agente com a migração de servidor de migrar do Azure, o VMware vSphere Kit de desenvolvimento de disco Virtual (VDDK) deve ser instalado no dispositivo de VM.

## <a name="agentless-migration-url-access-requirements"></a>Requisitos de acesso de URL de migração sem agente

O dispositivo de migrações para Azure precisa de conectividade de internet à internet.

- Quando você implanta o dispositivo, o migrações para Azure faz uma verificação de conectividade às URLs resumidos na tabela a seguir.
- Se você estiver usando um firewall.proxy baseado em URL, permita o acesso a essas URLs, certificando-se de que o proxy resolve quaisquer registros CNAME recebido ao pesquisar as URLs.

**URL** | **Detalhes**  
--- | --- 
*. portal.azure.com | Navegue até as migrações para Azure no portal do Azure.
*.windows.net | Faça logon em sua assinatura do Azure.
*.microsoftonline.com | Crie aplicativos do Active Directory para o dispositivo para se comunicar com o serviço migrações para Azure.
management.azure.com | Crie aplicativos do Active Directory para o dispositivo para se comunicar com o serviço migrações para Azure.
dc.services.visualstudio.com | Carregar logs de aplicativo usados para o monitoramento interno.
*.vault.azure.net | Gerencie segredos no cofre de chaves do Azure.
*.servicebus.windows.net | Comunicação entre o dispositivo e o serviço migrações para Azure.
*.discoverysrv.windowsazure.com<br/> *.migration.windowsazure.com<br/> *.hypervrecoverymanager.windowsazure.com | Conecte-se a URLs de serviço migrações para Azure.
*.blob.core.windows.net | Carregar dados para as contas de armazenamento.


## <a name="agentless-migration-port-requirements"></a>Requisitos de porta de migração sem agente

**Dispositivo** | **Conexão**
--- | --- 
Dispositivo | Porta de saída TCP 3389 para carregar os dados replicados no Azure e para se comunicar com as migrações para Azure para replicação e migração.
Servidor vCenter | Conexões de entrada na porta TCP 443 para permitir que o dispositivo para orquestrar a replicação - criar instantâneos, copiar dados de instantâneos de versão
host vSphere/ESXI | Entrada na porta TCP 902 para o dispositivo replicar dados de instantâneos. 


## <a name="agent-based-migration-vmware-server-requirements"></a>Requisitos do servidor VMware de migração baseada em agente

Esta tabela resume o suporte de avaliação e limitações para servidores de virtualização do VMware.

**Suporte** | **Detalhes**
--- | ---
**servidor do vCenter/ESXI** | Migrar VMs do VMware deve ser gerenciadas por um ou mais servidores do vCenter executando 5.5, 6.0, 6.5 ou 6.7 ou em um host ESXI vSphere versão 5.5, 6.0, 6.5 ou 6.7.

### <a name="agent-based-migration-vcenter-server-permissions"></a>Permissões de servidor do vCenter de migração baseada em agente

**Permissões** | **Detalhes**
--- | --- 
Datastore.AllocateSpace | Permitir a alocação de espaço em um repositório de dados para uma VM, disco virtual, clone ou instantâneo. 
Datastore.Browse | Permita navegação na VM arquivos de log para solucionar problemas de instantâneo de criação e exclusão.
Datastore.LowLevelFileOperations | Permitir leitura, gravação, excluir e renomear as operações no navegador de repositório de dados para solucionar problemas de criação/exclusão de instantâneo.
Datastore.UpdateVirtualMachineFiles | Permitir a atualização caminhos de arquivos da VM em um repositório de dados depois que o repositório de dados tiver sido recebe nova assinatura.
Network.AssignNetwork | Permitem a atribuição de uma rede para um recurso de máquina virtual.
AssignVirtualMachineToResourcePool | Permitir a atribuição de uma VM para um pool de recursos.
Resource.MigratePoweredOffVirtualMachine | Permitir a migração de uma VM desligada para um pool de recursos diferente ou o host.
Resource.MigratePoweredOnVirtualMachine | Permitir a migração usando o vMotion, de uma VM ligado a um pool de recursos diferente ou o host.
Tasks.CreateTask | Permitir que uma extensão criar uma tarefa definida pelo usuário.
Tasks.UpdateTask | Permitir que uma extensão atualizar uma tarefa definida pelo usuário.
VirtualMachine.Configuration. | Permite configurar opções de VM e os dispositivos.
Machine.Interaction.AnswerQuestion virtual | Permita a resolução de problemas com erros de tempo de execução ou transições de estado da VM.
Machine.Interaction.DeviceConnection virtual | Permite alterar o estado conectado de dispositivos virtuais de disconnectable da VM.
Machine.Interaction.ConfigureCDMedia virtual | Permitir a configuração de um dispositivo de DVD ou CD-ROM virtual.
Machine.Interaction.ConfigureFloppyMedia virtual | Permitir a configuração de um dispositivo de disquete virtual.
Machine.Interaction.PowerOff virtual | Permite que a VM seja desligado durante a migração para o Azure.
Virtual Machine.Interaction.PowerOn | Permite ligar uma VM desligada e retomar uma VM suspensa.
Machine.Interaction.VMwareToolsInstall virtual | Permitir montar e desmontar o instalador do CD de ferramentas do VMware como um CD-ROM para o sistema operacional convidado.
VirtualMachine.Inventory.CreateNew | Permitir a criação de uma VM e a alocação de recursos necessários.
VirtualMachine.Inventory.Register | Permitir a adição de uma VM existente para um servidor vCenter ou o inventário de host.
VirtualMachine.Inventory.Unregister | Permitir o cancelamento do registro um inventário de host ou VM de um servidor vCenter.
VirtualMachine.Provisioning.AllowVirtualMachineFilesUpload | Permitir operações de gravação em arquivos associados a uma VM, incluindo o vmx, discos, logs e nvram.
VirtualMachine.Provisioning.AllowVirtualMachineDownload | Permitir operações de leitura em arquivos associados a uma VM, para baixar os logs para solução de problemas.
VirtualMachine.SnapshotManagement.RemoveSnapshot | Permitir a remoção de um instantâneo do histórico de instantâneo.

## <a name="agent-based-migration-replication-appliance-requirements"></a>Requisitos de replicação de migração baseada em agente de dispositivo

Os requisitos para o [appliance replicação](migrate-replication-appliance.md) usado para a migração baseada em agente de VMs VMware e físicos servidores com migração de servidor de migrar do Azure são resumidas na tabela.

> [!NOTE]
> Quando você configura o dispositivo de replicação usando o modelo de OVA fornecido no hub de migrações para Azure, o dispositivo executa o Windows Server 2016 e está em conformidade com os requisitos de suporte. Se você configurar o dispositivo de replicação manualmente em um servidor físico, certifique-se de que ele esteja em conformidade com os requisitos.



**Componente** | **Requisito** 
--- | ---
 | **Configurações de VMware** (dispositivo de VM do VMware)
**PowerCLI** | [O PowerCLI versão 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) deve ser instalado se o dispositivo de replicação está em execução em uma VM VMware.
**Tipo de NIC** | VMXNET3 (se o dispositivo é uma VM VMware)
 | **Configurações de hardware** 
Núcleos de CPU | 8 
RAM | 16 GB
Número de discos | Três: O disco do sistema operacional, a disco de cache do servidor de processo e unidade de retenção.
Espaço livre em disco (cache) | 600 GB
Espaço livre em disco (disco de retenção) | 600 GB
**Configurações de software** | 
Sistema operacional | Windows Server 2016 ou Windows Server 2012 R2
Localidade do sistema operacional | Inglês (en-us)
TLS | TLS 1.2 deve ser habilitado.
.NET Framework | .NET framework 4.6 ou posterior deve ser instalado no computador (com criptografia forte habilitada.
MySQL | MySQL deve ser instalado no dispositivo.<br/> MySQL deve ser instalado. Você pode instalar manualmente ou recuperação de Site pode instalá-lo durante a implantação do dispositivo. 
Outros aplicativos | Você não deve executar outros aplicativos no dispositivo de replicação.
Funções do Windows Server | Não habilite essas funções: <br> - Active Directory Domain Services <br>- Serviços de Informações da Internet <br> - Hyper-V 
Políticas de grupo | Não habilite estas políticas de grupo: <br> - Impedir o acesso ao prompt de comando. <br> - Impedir o acesso às ferramentas de edição do registro. <br> - Lógica de confiança para anexos de arquivo. <br> - Ativar a execução do script. <br> [Saiba mais](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | – Nenhum site da Web padrão já existente <br> - Nenhum aplicativo/site da Web pré-existente escutando na porta 443 <br>- Habilitar [autenticação anônima](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> - Habilitar configuração [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) 
**Configurações de rede** | 
Tipo de endereço IP | Estático 
Portas | 443 (orquestração do canal de controle)<br>9443 (transporte de dados) 
Tipo de NIC | VMXNET3 

### <a name="replication-appliance-url-access"></a>Acesso à URL de solução de replicação

O dispositivo de replicação precisa de acesso a essas URLs.

**URL** | **Detalhes**
--- | ---
\*.backup.windowsazure.com | Usado para transferência de dados replicados e coordenação
\*.store.core.windows.net | Usado para transferência de dados replicados e coordenação
\*.blob.core.windows.net | Usado para acessar a conta de armazenamento que armazena os dados replicados
\*.hypervrecoverymanager.windowsazure.com | Usado para operações de gerenciamento de replicação e coordenação
https:\//management.azure.com | Usado para operações de gerenciamento de replicação e coordenação 
*.services.visualstudio.com | Usado para fins de telemetria (é opcional)
time.nist.gov | Usados para verificar a sincronização de horário entre a hora do sistema e a hora global.
time.windows.com | Usados para verificar a sincronização de horário entre a hora do sistema e a hora global.
https:\//login.microsoftonline.com <br/> https:\//secure.aadcdn.microsoftonline-p.com <br/> https:\//login.live.com <br/> https:\//graph.windows.net <br/> https:\//login.windows.net <br/> https:\//www.live.com <br/> https:\//www.microsoft.com  | Instalação OVF precisa de acesso a essas URLs. Elas são usadas pelo Azure Active Directory para o gerenciamento de identidade e controle de acesso
https:\//dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi | Para concluir o download do MySQL


#### <a name="mysql-installation-options"></a>Opções de instalação do MySQL

MySQL pode ser instalado no dispositivo de replicação usando um dos seguintes métodos.

**Instalar** | **Detalhes**
--- | ---
Baixe e instale manualmente | Baixe o aplicativo MySQL e colocá-lo na pasta C:\Temp\ASRSetup e instalar manualmente.<br/> Quando você configura o dispositivo de MySQL mostrará como já instalado. 
Não baixe online | Coloque o aplicativo de instalação do MySQL na pasta C:\Temp\ASRSetup. Quando você instala o dispositivo e clique para baixar e instalar o MySQL, a instalação usará o instalador que você adicionou. 
Migrar do download do Azure | Quando você instala o dispositivo e será solicitado para MySQL, selecione **Baixe e instale o**.



## <a name="agent-based-migration-vmware-vm-requirements"></a>Requisitos de VM do VMware de migração baseada em agente

**Suporte** | **Detalhes**
--- | ---
**Carga de trabalho de máquina** | As migrações para Azure dá suporte à migração de qualquer carga de trabalho (digamos Active Directory, SQL server, etc.) em execução em um computador com suporte.
**Sistemas operacionais** | Para obter as informações mais recentes, examine os [suporte do sistema operacional](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines) para o Site Recovery. As migrações para Azure fornece suporte de sistema operacional VM idêntico.
**Armazenamento de convidado/sistema de arquivos do Linux** | Para obter as informações mais recentes, examine os [suporte do sistema de arquivos Linux](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage) para o Site Recovery. As migrações para Azure tem suporte do sistema de arquivos Linux idêntico.
**Armazenamento/rede** | Para obter as informações mais recentes, examine os [rede](../site-recovery/vmware-physical-azure-support-matrix.md#network) e [armazenamento](../site-recovery/vmware-physical-azure-support-matrix.md#storage) pré-requisitos para o Site Recovery. As migrações para Azure fornece os requisitos de armazenamento/rede idêntica.
**Requisitos do Azure** | Para obter as informações mais recentes, examine os [rede do Azure](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover), [armazenamento](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage), e [computação](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute) requisitos para o Site Recovery. As migrações para Azure tem requisitos idênticos para migração de VMware.
**Serviço de mobilidade** | O agente de serviço de mobilidade deve ser instalado em cada VM que você deseja migrar.
**Disco de destino** | VMs só podem ser migradas para discos gerenciados (HDD standard, premium SSD) no Azure.

   

## <a name="agent-based-migration-url-access-requirements"></a>Requisitos de acesso de URL de migração baseada em agente

O serviço de mobilidade em execução em VMs do VMware precisa de conectividade de internet à internet.

- Quando você implanta o serviço de mobilidade, ele faz uma verificação de conectividade às URLs resumidos na tabela a seguir.
- Se você estiver usando um firewall.proxy baseado em URL, permita o acesso a essas URLs, certificando-se de que o proxy resolve quaisquer registros CNAME recebido ao pesquisar as URLs.

**URL** | **Detalhes**  
--- | --- 
*. portal.azure.com | Navegue até as migrações para Azure no portal do Azure.
*.windows.net | Faça logon em sua assinatura do Azure.
*.microsoftonline.com | Crie aplicativos do Active Directory para o dispositivo para se comunicar com o serviço migrações para Azure. 
management.azure.com | Crie aplicativos do Active Directory para o dispositivo para se comunicar com o serviço migrações para Azure.
dc.services.visualstudio.com | Carregar logs de aplicativo usados para o monitoramento interno.
*.vault.azure.net | Gerencie segredos no cofre de chaves do Azure.
*.servicebus.windows.net | Comunicação entre o dispositivo e o serviço migrações para Azure.
*.discoverysrv.windowsazure.com <br/> *.migration.windowsazure.com <br/> *.hypervrecoverymanager.windowsazure.com | Conecte-se a URLs de serviço migrações para Azure.
*.blob.core.windows.net | Carregar dados para as contas de armazenamento.

## <a name="agent-based-migration-port-requirements"></a>Requisitos de porta de migração baseada em agente

**Dispositivo** | **Conexão**
--- | --- 
VMs | O serviço de mobilidade em execução nas VMs se comunica com o servidor de configuração local na porta HTTPS 443 de entrada para o gerenciamento de replicação.<br/><br/> As VMs que estão sendo replicadas enviam dados de replicação para o servidor de processo (em execução no computador do servidor de configuração) na porta 9443 HTTPS de entrada. Essa porta pode ser modificada.
Solução de replicação | O dispositivo de replicação coordena a replicação com o Azure pela porta HTTPS 443 de saída.
Servidor de processo | O servidor de processo recebe dados de replicação, otimiza-os e criptografa-os e os envia para o armazenamento do Microsoft Azure pela porta 443 de saída.<br/> Por padrão o servidor de processo é executado no dispositivo de replicação.

## <a name="azure-vm-requirements"></a>Requisitos de VM do Azure

Todos os locais VMs replicadas no Azure devem atender os requisitos de VM do Azure resumidos nesta tabela. Quando o Site Recovery é executado uma verificação de pré-requisitos para replicação, a verificação falhará se alguns dos requisitos não forem atendidos.

**Componente** | **Requisitos** | **Detalhes**
--- | --- | ---
Sistema operacional convidado | Verifique se os sistemas operacionais com suporte para [VMs VMware usando a replicação sem agente](#agentless-migration-vmware-vm-requirements)e para [VMs VMware usando replicação baseada em agente](#agent-based-migration-vmware-vm-requirements).<br/> Você pode migrar qualquer carga de trabalho em execução em um sistema operacional com suporte. | A verificação falha se não tiver suporte.
Arquitetura do sistema operacional convidado | 64 bits. | A verificação falha se não tiver suporte.
Tamanho do disco do sistema operacional | Até 2.048 GB. | A verificação falha se não tiver suporte.
Contagem do disco do sistema operacional | 1 | A verificação falha se não tiver suporte.
Contagem de disco de dados | 64 ou menos. | A verificação falha se não tiver suporte.
Tamanho do disco de dados | Até 4.095 GB | A verificação falha se não tiver suporte.
Adaptadores de rede | Há suporte para vários adaptadores. | 
VHD compartilhado | Sem suporte. | A verificação falha se não tiver suporte.
Disco FC | Sem suporte. | A verificação falha se não tiver suporte.
BitLocker | Sem suporte. | O BitLocker precisa ser desabilitado antes de habilitar a replicação em um computador. 
Nome da VM | De 1 a 63 caracteres.<br/> Restrito a letras, números e hifens.<br/><br/> O nome do computador precisa começar e terminar com uma letra ou um número. |  Atualize o valor nas propriedades do computador no Site Recovery.
Conectar-se após a migração-Windows | Para conectar a VMs do Azure executando o Windows após a migração:<br/> -Habilite o RDP na VM local antes da migração. Certifique-se de que as regras TCP e UDP são adicionadas ao perfil **Público** e que o RDP é permitido no **Firewall do Windows** > **Aplicativos Permitidos** para todos os perfis.<br/> Para acesso VPN site a site, habilite o RDP e permitir o RDP na **Firewall do Windows** -> **aplicativos e recursos permitidos** para **domínio e particular** redes. Além disso, verifique se a opção política de SAN do sistema operacional é definida como **OnlineAll**. [Saiba mais](https://support.microsoft.com/kb/3031135). | 
Conectar-se após a migração-Linux | Para se conectar às VMs do Azure após a migração usando o SSH:<br/> Antes da migração, na máquina local, verifique o serviço Secure Shell está definido para iniciar, e se as regras de firewall permitem uma conexão SSH.<br/> Após o failover, na VM do Azure, permitem que as conexões de entrada para a porta SSH para as regras de grupo de segurança de rede na VM com failover e para a sub-rede do Azure ao qual ele está conectado. Além disso, adicione um endereço IP público da VM. |  


## <a name="next-steps"></a>Próximas etapas

[Preparar para o VMware](tutorial-prepare-vmware.md) avaliação e migração.








