---
title: Matriz de suporte de migração do Azure para avaliação do Hyper-V e migração
description: Resume as configurações e limitações de avaliação do Hyper-V e migração usando o serviço migrações para Azure.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/02/2019
ms.author: raynew
ms.openlocfilehash: f6edbe19429b38d68aea1f1ecfe426c9b2d194d0
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811343"
---
# <a name="support-matrix-for-hyper-v-assessment-and-migration"></a>Matriz de suporte para a avaliação do Hyper-V e migração

Você pode usar o [serviço migrações para Azure](migrate-overview.md) para avaliar e migrar as máquinas para a nuvem do Microsoft Azure. Este artigo resume as configurações de suporte e limitações para avaliar e migrar VMs do Hyper-V local.



## <a name="hyper-v-scenarios"></a>Cenários do Hyper-V

A tabela resume os cenários com suporte para VMs do Hyper-V.

**Implantação** | **Detalhes*** 
--- | --- 
**Avaliar as VMs do Hyper-V no local** | [Configurar](tutorial-prepare-hyper-v.md) sua primeira avaliação.<br/><br/> [Executar](scale-hyper-v-assessment.md) uma avaliação em larga escala.
**Migrar VMs Hyper-V para o Azure** | [Experimente](tutorial-migrate-hyper-v.md) migração para o Azure.

    

## <a name="azure-migrate-projects"></a>Projetos de migração do Azure

**Suporte** | **Detalhes**
--- | ---
Permissões do Azure | Você precisa de permissões de Colaborador ou proprietário na assinatura para criar um projeto de migrações para Azure.
VMs Hyper-V | Avalie até 10.000 VMs Hyper-V em um único projeto.

Um projeto pode incluir VMs VMware e VMs do Hyper-V, até os limites de avaliação.


## <a name="assessment-hyper-v-host-requirements"></a>Requisitos do host de avaliação de Hyper-V

| **Suporte**                | **Detalhes**               
| :-------------------       | :------------------- |
| **Implantação do host**       | O host Hyper-V pode ser autônomo ou implantado em um cluster. |
| **Permissões**           | Você precisa de permissões de administrador no host Hyper-V. |
| **Sistema operacional do host** | Windows Server 2016 ou Windows Server 2012 R2.<br/> Não é possível avaliar as VMs localizadas nos hosts do Hyper-V executando o Windows Server 2019. |
| **Comunicação remota do PowerShell**   | Deve ser habilitado em cada host. |
| **Réplica do Hyper-V**       | Se você usar a réplica do Hyper-V (ou você tiver várias VMs com os mesmos identificadores VM) e descobrir as duas VMs originais e replicadas usando migrações para Azure, a avaliação gerada pelas migrações para Azure pode não ser precisa. |


## <a name="assessment-hyper-v-vm-requirements"></a>Requisitos de VM de avaliação de Hyper-V

| **Suporte**                  | **Detalhes**               
| :----------------------------- | :------------------- |
| **Sistema operacional** | Todos os [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) e [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) sistemas operacionais com suporte pelo Azure. |
| **Permissões**           | Você precisa de permissões de administrador em cada VM do Hyper-V que você deseja avaliar. |
| **Integration Services**       | [Serviços de integração do Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) deve estar em execução em máquinas virtuais que você avaliar, para capturar as informações do sistema operacional. |
| **Alterações necessárias para o Azure** | Algumas VMs podem exigir alterações para que eles podem ser executados no Azure. As migrações para Azure faz essas alterações automaticamente para os seguintes sistemas operacionais:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> - SUSE Linux Enterprise Server 12 SP1+<br/> - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS<br/> - Debian 7,8<br/><br/> Para outros sistemas operacionais, você precisa fazer ajustes manualmente antes da migração. Os artigos relevantes contém instruções sobre como fazer isso. |
| **Inicialização do Linux**                 | Se /boot estiver em uma partição dedicada, ele deve residir no disco do SO e não sejam distribuído em vários discos.<br/> Se /boot faz parte da partição raiz (/), em seguida, a partição '/' deve estar no disco do sistema operacional e não abrangem outros discos. |
| **Inicialização de UEFI**                  | Máquinas virtuais com inicialização UEFI não têm suporte para a migração. |
| **Discos/volumes criptografados**    | VMs com discos/volumes criptografados não têm suporte para a migração. |
| **Discos de passagem/RDM**      | Se as VMs tiverem discos RDM ou passagem, esses discos não serão duplicados para o Azure. |
| **NFS**                        | Volumes NFS montados como volumes nas máquinas virtuais não serão duplicados. |
| **Disco de destino**                | Avaliações das migrações para Azure recomendam migração para VMs do Azure com discos gerenciados apenas. |


## <a name="assessment-appliance-requirements"></a>Requisitos da ferramenta de avaliação

Para avaliação de migrações para Azure é executado em um dispositivo leve para descobrir VMs do Hyper-V e enviar dados de metadados e o desempenho da VM para migrações para Azure. O dispositivo é executado em uma VM do Hyper-V, e você definiu usando um VHD do Hyper-V compactado que você pode baixar do portal do Azure. A tabela a seguir resume os requisitos de dispositivo.

| **Suporte**                | **Detalhes**               
| :-------------------       | :------------------- |
| **Projeto de migrações do Azure**  |  Um dispositivo pode ser associado um único projeto.<br/> Você pode descobrir até 5.000 VMs do Hyper-V com um único dispositivo.
| **Limitações do Hyper-V**    |  Você pode implantar o dispositivo como uma VM do Hyper-V.<br/> O dispositivo de VM fornecida é a VM do Hyper-V versão 5.0.<br/> O host VM deve estar executando o Windows Server 2012 R2 ou posterior.<br/> Ele precisa de espaço suficiente para alocar a 16 GB de RAM, 4 processadores virtuais, e 1 externo alternar para o dispositivo de VM.<br/> Dispositivo requer um endereço IP estático ou dinâmico e acesso à internet.
| **Dispositivo de Hyper-V**      |  O dispositivo está configurado como uma VM do Hyper-V.<br/> O VHD fornecido para download é VM do Hyper-V versão 5.0.
| **Host**                   | O host VM em execução o dispositivo de VM deve estar executando o Windows Server 2012 R2 ou posterior.<br/> Ele precisa de espaço suficiente para alocar memória RAM, 4 processadores virtuais e um comutador externo 16 GB para o dispositivo de VM.<br/> Dispositivo requer um endereço IP estático ou dinâmico e acesso à internet. |
| **Suporte à migração**      | Para iniciar a replicação de máquinas, o serviço de Gateway de migração a no dispositivo deve ser 1.18.7141.12919 ou posterior. Entrar no aplicativo de web do dispositivo para verificar a versão. |

## <a name="assessment-appliance-url-access"></a>Acesso à URL de avaliação de dispositivo

Para avaliar as VMs, o dispositivo de migrações para Azure precisa de conectividade de internet.

- Quando você implanta o dispositivo, o migrações para Azure faz uma verificação de conectividade às URLs resumidos na tabela a seguir.
- Se você estiver usando um firewall.proxy baseado em URL, permita o acesso às URLs na tabela, certificando-se de que o proxy resolve quaisquer registros CNAME recebido ao pesquisar as URLs.
- Se você tiver um proxy de interceptação, você precisa importar o certificado do servidor do servidor proxy para o dispositivo. 

    
**URL** | **Detalhes**  
--- | --- 
*. portal.azure.com | Navegação no portal do Azure
*.windows.net | Entre em sua assinatura do Azure
*.microsoftonline.com | Aplicativos de criação do Azure Active Directory para o dispositivo para comunicações de serviço.
management.azure.com | Aplicativos de criação do Azure Active Directory para o dispositivo para comunicações de serviço.
dc.services.visualstudio.com | Log e monitoramento 
*.vault.azure.net | Gerencie segredos no cofre de chaves do Azure ao se comunicar entre o dispositivo e serviço.


## <a name="assessment-port-requirements"></a>Requisitos de porta de avaliação

A tabela a seguir resume os requisitos de porta para avaliação.

**Dispositivo** | **Conexão**
--- | --- 
**Dispositivo** | Conexões de entrada na porta TCP 3389 para permitir conexões da área de trabalho remota para o dispositivo.<br/> Conexões na porta 44368 para acessar remotamente o aplicativo de gerenciamento de dispositivo usando a URL de entrada: https://<appliance-ip-or-name>:44368<br/> Conexões de saída na porta 443 para enviar os metadados de descoberta e o desempenho ao migrar do Azure.
**Host/cluster do Hyper-V** | Conexões nas portas de WinRM 5985 (HTTP) e 5986 (HTTPS) para efetuar pull de metadados de configuração e desempenho das VMs do Hyper-V usando uma sessão CIM Common Information Model () de entrada.

## <a name="migration-hyper-v-host-requirements"></a>Requisitos do host de migração-Hyper-V.

| **Suporte**                | **Detalhes**               
| :-------------------       | :------------------- |
| **Implantação do host**       | O host Hyper-V pode ser autônomo ou implantado em um cluster. |
| **Permissões**           | Você precisa de permissões de administrador no host Hyper-V. |
| **Sistema operacional do host** | Windows Server 2012 R2, Windows Server 2016 ou Windows Server de 2019. |

## <a name="migration-hyper-v-vm-requirements"></a>Requisitos de migração-VM do Hyper-V

| **Suporte**                  | **Detalhes**               
| :----------------------------- | :------------------- |
| **Sistema operacional** | Todos os [Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines) e [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) sistemas operacionais com suporte pelo Azure. |
| **Permissões**           | Você precisa de permissões de administrador em cada VM do Hyper-V que você deseja avaliar. |
| **Integration Services**       | [Serviços de integração do Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services) deve estar em execução em máquinas virtuais que você avaliar, para capturar as informações do sistema operacional. |
| **Alterações necessárias para o Azure** | Algumas VMs podem exigir alterações para que eles podem ser executados no Azure. As migrações para Azure faz essas alterações automaticamente para os seguintes sistemas operacionais:<br/> - Red Hat Enterprise Linux 6.5+, 7.0+<br/> - CentOS 6.5+, 7.0+</br> - SUSE Linux Enterprise Server 12 SP1+<br/> - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS<br/> - Debian 7,8<br/><br/> Para outros sistemas operacionais, você precisa fazer ajustes manualmente antes da migração. Os artigos relevantes contém instruções sobre como fazer isso. |
| **Inicialização do Linux**                 | Se /boot estiver em uma partição dedicada, ele deve residir no disco do SO e não sejam distribuído em vários discos.<br/> Se /boot faz parte da partição raiz (/), em seguida, a partição '/' deve estar no disco do sistema operacional e não abrangem outros discos. |
| **Inicialização de UEFI**                  | Máquinas virtuais com inicialização UEFI não têm suporte para a migração. |
| **Discos/volumes criptografados**    | VMs com discos/volumes criptografados não têm suporte para a migração. |
| **Discos de passagem/RDM**      | Se as VMs tiverem discos RDM ou passagem, esses discos não serão duplicados para o Azure. |
| **NFS**                        | Volumes NFS montados como volumes nas máquinas virtuais não serão duplicados. |
| **Disco de destino**                | Você pode migrar VMs do Azure com discos gerenciados apenas. |




## <a name="migration-hyper-v-host-url-access"></a>Acesso à URL de host de migração-Hyper-V.

A tabela a seguir resume os requisitos de acesso de URL para hosts Hyper-V.

**URL** | **Detalhes**  
--- | ---
login.microsoftonline.com | Identidade e controle de gerenciamento de acesso usando o Active Directory.
*.backup.windowsazure.com | Transferência de dados de replicação e coordenação.
*.hypervrecoverymanager.windowsazure.com | Conecte-se a URLs de serviço migrações para Azure.
*.blob.core.windows.net | Carregar dados para as contas de armazenamento.
dc.services.visualstudio.com | Carregar logs de aplicativo usados para o monitoramento interno.
time.windows.com | Verifica o tempo de sincronização entre o sistema e a hora global.

## <a name="migration-port-access"></a>Acesso à porta de migração

A tabela a seguir resume os requisitos de porta nos hosts Hyper-V e máquinas virtuais para migração de VM.

**Dispositivo** | **Conexão**
--- | --- 
Hosts/VMs Hyper-V | Conexões de saída em HTTPS porta 443 para enviar dados de replicação de VM para migrações para Azure.

  
## <a name="migration-vm-disk-support"></a>Suporte de disco de VM de migração 

**Suporte** | **Detalhes**
--- | ---
Discos migrados | VMs só podem ser migradas para discos gerenciados (HDD standard, premium SSD) no Azure.
   

## <a name="next-steps"></a>Próximas etapas

[Preparar para a avaliação da VM do Hyper-V](tutorial-prepare-hyper-v.md) para a migração.




 
