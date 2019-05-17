---
title: Perguntas frequentes sobre os arquivos do Azure NetApp | Microsoft Docs
description: Respostas a perguntas frequentes sobre os arquivos do Azure NetApp.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: b-juche
ms.openlocfilehash: 6f1ca3398678b59a81e5c22b51b36a1f5505d4c2
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65806380"
---
# <a name="faqs-about-azure-netapp-files"></a>Perguntas frequentes sobre os arquivos do Azure NetApp

Este artigo responde perguntas frequentes (FAQs) sobre os arquivos do Azure NetApp. 

## <a name="networking-faqs"></a>Perguntas frequentes sobre o sistema de rede

### <a name="does-the-nfs-data-path-go-over-the-internet"></a>O caminho de dados NFS passa pela Internet?  

Não. O caminho de dados NFS não passam pela Internet. Os arquivos do Azure do NetApp é um serviço nativo do Azure que é implantado na rede Virtual do Azure (VNet) no qual o serviço está disponível. Os arquivos do Azure do NetApp usa uma sub-rede de delegado e provisiona uma interface de rede diretamente na rede virtual. 

Ver [planejamento de rede de diretrizes de arquivos do Azure NetApp](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-network-topologies) para obter detalhes.  

### <a name="can-i-connect-a-vnet-that-i-already-created-to-the-azure-netapp-files-service"></a>Posso conectar uma rede virtual que já criei para o serviço de arquivos do Azure NetApp?

Sim, você pode conectar redes virtuais que você criou para o serviço. 

Ver [planejamento de rede de diretrizes de arquivos do Azure NetApp](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-network-topologies) para obter detalhes.  

### <a name="can-i-mount-an-nfs-volume-of-azure-netapp-files-using-dns-fqdn-name"></a>Posso montar um volume do NFS de arquivos do NetApp do Azure usando o nome FQDN do DNS?

Sim, você pode, se você criar as entradas DNS necessárias. Os arquivos do Azure do NetApp fornece o IP do serviço para o volume provisionado. 

> [!NOTE] 
> Os arquivos do Azure do NetApp pode implantar IPs adicionais para o serviço conforme necessário.  As entradas DNS talvez precise ser atualizadas periodicamente.

## <a name="security-faqs"></a>Perguntas frequentes sobre segurança

### <a name="can-the-network-traffic-between-the-azure-vm-and-the-storage-be-encrypted"></a>O tráfego de rede entre a VM do Azure e o armazenamento pode ser criptografado?

O tráfego de dados (tráfego do cliente NFS V3 ou SMBv3 para volumes de arquivos do Azure NetApp) não é criptografado. No entanto, o tráfego de uma VM do Azure (executando um cliente NFS ou SMB) para arquivos do Azure NetApp é tão seguro quanto qualquer outro tráfego de VM do Azure de VM. Esse tráfego é local para a rede de data center do Azure. 

### <a name="can-the-storage-be-encrypted-at-rest"></a>O armazenamento pode ser criptografado em repouso?

Todos os volumes de arquivos do Azure NetApp são criptografados usando o FIPS 140-2 padrão. Todas as chaves são gerenciadas pelo serviço arquivos do Azure NetApp. 

### <a name="how-are-encryption-keys-managed"></a>Como as chaves de criptografia são gerenciadas? 

Gerenciamento de chaves para os arquivos do Azure NetApp é tratado pelo serviço.  Atualmente, não há suporte para chaves gerenciadas pelo usuário (traga seu próprio chaves).

### <a name="can-i-configure-the-nfs-export-policy-rules-to-control-access-to-the-azure-netapp-files-service-mount-target"></a>Posso configurar as regras de política de exportação NFS para controlar o acesso para o destino de montagem do serviço de arquivos do Azure NetApp?


Sim, você pode configurar até cinco regras em uma única política de exportação NFS.

### <a name="does-azure-netapp-files-support-network-security-groups"></a>Arquivos do Azure NetApp dá suporte a grupos de segurança de rede?

Não, atualmente, é possível aplicar grupos de segurança de rede à sub-rede de delegados de arquivos do Azure NetApp ou as interfaces de rede criadas pelo serviço.

### <a name="can-i-use-azure-iam-with-azure-netapp-files"></a>Pode usar o Azure IAM com arquivos do Azure NetApp?

Sim, os arquivos NetApp Azure dá suporte a recursos RBAC com o Azure IAM.

## <a name="performance-faqs"></a>Perguntas frequentes sobre o desempenho

### <a name="what-should-i-do-to-optimize-or-tune-azure-netapp-files-performance"></a>O que devo fazer para otimizar ou ajustar o desempenho do arquivos do Azure NetApp?

Você pode executar as seguintes ações de acordo com os requisitos de desempenho: 
- Certifique-se de que a máquina Virtual está dimensionado adequadamente.
- Habilite a rede acelerada para a VM.
- Selecione o nível de serviço desejado e o tamanho para o pool de capacidade.
- Crie um volume com o tamanho da cota desejada para a capacidade e desempenho.

### <a name="how-do-i-convert-throughput-based-service-levels-of-azure-netapp-files-to-iops"></a>Como converter os níveis de serviço com base em taxa de transferência de arquivos do Azure NetApp para IOPS?

Você pode converter MB/s para IOPS, usando a fórmula a seguir:  

`IOPS = (MBps Throughput / KB per IO) * 1024`

### <a name="how-do-i-change-the-service-level-of-a-volume"></a>Como posso alterar o nível de serviço de um volume?

Atualmente, não há suporte para a alteração do nível de serviço de um volume.

### <a name="how-do-i-monitor-azure-netapp-files-performance"></a>Como monitorar o desempenho dos arquivos do Azure NetApp?

Os arquivos do Azure do NetApp fornece métricas de desempenho do volume. Você também pode usar o Azure Monitor para monitorar as métricas de uso para arquivos do Azure NetApp.  Ver [métricas para os arquivos do Azure NetApp](azure-netapp-files-metrics.md) para a lista de métricas de desempenho para arquivos do Azure NetApp.

## <a name="nfs-faqs"></a>Perguntas frequentes do NFS

### <a name="i-want-to-have-a-volume-mounted-automatically-when-an-azure-vm-is-started-or-rebooted--how-do-i-configure-my-host-for-persistent-nfs-volumes"></a>Desejo ter um volume montado automaticamente quando uma VM do Azure é iniciada ou reinicializada.  Como configurar o meu host para volumes do NFS persistentes?

Para um volume do NFS montado automaticamente no início VM ou a reinicialização, adicione uma entrada para o `/etc/fstab` arquivo no host. 

Por exemplo: `$ANFIP:/$FILEPATH      /$MOUNTPOINT    nfs bg,rw,hard,noatime,nolock,rsize=65536,wsize=65536,vers=3,tcp,_netdev 0 0`

- $ANFIP  
    O endereço IP do volume de arquivos do Azure NetApp encontrado na folha de propriedades de volume
- $FILEPATH  
    O caminho de exportação do volume de arquivos do Azure NetApp
- $MOUNTPOINT  
    O diretório criado no host Linux usado para montar a exportação NFS

### <a name="why-does-the-df-command-on-nfs-client-not-show-the-provisioned-volume-size"></a>Por que o comando DF no cliente NFS não mostra o tamanho do volume provisionado?

O tamanho do volume relatado no DF é o tamanho máximo que o volume de arquivos do Azure NetApp pode atingir. O tamanho do volume de arquivos do Azure NetApp no comando DF não é reflete a cota ou o tamanho do volume.  Você pode obter o tamanho do volume de arquivos do Azure NetApp ou cota por meio do portal do Azure ou a API.

### <a name="what-nfs-version-does-azure-netapp-files-support"></a>Qual versão do NFS NetApp serviço arquivos do Azure dá suporte?

Os arquivos do Azure do NetApp atualmente dá suporte ao NFS V3.

### <a name="how-do-i-enable-root-squashing"></a>Como habilitar eliminando raiz?

Finalizando de raiz não é suportado atualmente.

## <a name="smb-faqs"></a>Perguntas frequentes sobre o SMB

### <a name="does-azure-netapp-files-support-azure-active-directory"></a>Arquivos do Azure NetApp dá suporte a Azure Active Directory?

Não, atualmente não há suporte.  Suporte a NetApp os arquivos do Azure Active Directory Domain Services (traga seu próprio AD), que pode usar controladores de domínio do Active Directory existente com os arquivos do Azure NetApp. Controladores de domínio podem residir no Azure como máquinas virtuais ou no local por meio do ExpressRoute.

### <a name="is-an-active-directory-connection-required-for-smb-access"></a>É uma conexão do Active Directory necessário para acesso ao SMB? 

Sim, você deve criar uma conexão do Active Directory antes de implantar um volume SMB. Os controladores de domínio especificado deve ser acessíveis pela sub-rede delegada de arquivos do Azure NetApp para uma conexão bem-sucedida.  Ver [criar um volume SMB](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-create-volumes#create-an-smb-volume) para obter detalhes. 

### <a name="how-many-active-directory-connections-are-supported"></a>Número de conexões do Active Directory têm suporte?

Atualmente o NetApp os arquivos do Azure dá suporte a uma conexão do Active Directory por assinatura. Além disso, a conexão do Active Directory é específico para uma única conta do NetApp; ele não é compartilhado entre contas. 

### <a name="what-versions-of-windows-ad-are-supported"></a>Há suporte para quais versões do Windows do AD?

Os arquivos NetApp do Azure dá suporte à versão do Windows Server 2016 2008r2SP1 dos serviços de domínio do Active Directory.

## <a name="capacity-management-faqs"></a>Perguntas frequentes sobre o gerenciamento de capacidade

### <a name="how-do-i-monitor-usage-for-capacity-pool-and-volume-of-azure-netapp-files"></a>Como monitorar o uso de pool de capacidade e o volume de arquivos do Azure NetApp? 

Os arquivos do Azure do NetApp fornece métricas de uso de pool e o volume de capacidade. Você também pode usar o Azure Monitor para monitorar o uso de arquivos do Azure NetApp. Ver [métricas para os arquivos do Azure NetApp](azure-netapp-files-metrics.md) para obter detalhes. 

### <a name="can-i-manage-azure-netapp-files-through-azure-storage-explorer"></a>Pode gerenciar o Azure NetApp arquivos por meio do Gerenciador de armazenamento do Azure?

Não. Não há suporte para os arquivos do Azure do NetApp pelo Gerenciador de armazenamento do Azure.

## <a name="data-migration-and-protection-faqs"></a>Perguntas frequentes sobre migração e proteção de dados

### <a name="how-do-i-migrate-data-to-azure-netapp-files"></a>Como faço para migrar dados para arquivos do Azure NetApp?
Os arquivos do Azure do NetApp fornece volumes SMB e NFS.  Você pode usar qualquer ferramenta de cópia baseada em arquivo para migrar dados para o serviço. 

NetApp oferece uma solução baseada em SaaS, [sincronização de nuvem do NetApp](https://cloud.netapp.com/cloud-sync-service).  A solução permite que você replique NFS ou compartilha dados SMB para exportações Azure NetApp arquivos NFS ou SMB. 

Você também pode usar uma ampla gama de ferramentas gratuitas para copiar dados. NFS, você pode usar ferramentas de cargas de trabalho como [rsync](https://rsync.samba.org/examples.html) para copiar e sincronizar dados de origem em um volume de arquivos do Azure NetApp. Para o SMB, você pode usar as cargas de trabalho [robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) da mesma maneira.  Essas ferramentas também podem replicar as permissões de arquivo ou pasta. 

Os requisitos para a migração de dados do local para arquivos do Azure NetApp são da seguinte maneira: 

- Certifique-se de que os arquivos do NetApp do Azure está disponível na região do Azure do destino.
- Valide a conectividade de rede entre a origem e o endereço IP de volume de destino de arquivos do Azure NetApp. Transferência de dados entre locais e o serviço de arquivos do Azure NetApp dá suporte por meio do ExpressRoute.
- Crie o volume de arquivos do Azure NetApp de destino.
- Transferir os dados de origem para o volume de destino usando sua ferramenta de cópia de arquivo preferido.

### <a name="how-do-i-create-a-copy-of-an-azure-netapp-files-volume-in-another-azure-region"></a>Como para criar uma cópia de um volume de arquivos do Azure NetApp em outra região do Azure?
    
Os arquivos do Azure do NetApp fornece volumes SMB e NFS.  Qualquer ferramenta de cópia com base em arquivo pode ser usada para replicar dados entre regiões do Azure. 

NetApp oferece uma solução de SaaS, com base [sincronização de nuvem do NetApp](https://cloud.netapp.com/cloud-sync-service).  A solução permite que você replique NFS ou compartilha dados SMB para exportações Azure NetApp arquivos NFS ou SMB. 

Você também pode usar uma ampla gama de ferramentas gratuitas para copiar dados. NFS, você pode usar ferramentas de cargas de trabalho como [rsync](https://rsync.samba.org/examples.html) para copiar e sincronizar dados de origem em um volume de arquivos do Azure NetApp. Para o SMB, você pode usar as cargas de trabalho [robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy) da mesma maneira.  Essas ferramentas também podem replicar as permissões de arquivo ou pasta. 

Os requisitos para replicação de um volume de arquivos do Azure NetApp para outra região do Azure são da seguinte maneira: 
- Certifique-se de que os arquivos do NetApp do Azure está disponível na região do Azure do destino.
- Valide a conectividade de rede entre redes virtuais em cada região. Atualmente, não há suporte para emparelhamento global entre redes virtuais.  Você pode estabelecer conectividade entre redes virtuais vinculando um circuito de ExpressRoute ou usar uma conexão VPN S2S. 
- Crie o volume de arquivos do Azure NetApp de destino.
- Transferir os dados de origem para o volume de destino usando sua ferramenta de cópia de arquivo preferido.

### <a name="is-migration-with-azure-data-box-supported"></a>É a migração com suporte do Azure Data Box?

Não. O Azure Data Box não oferece suporte atualmente NetApp serviço arquivos do Azure. 

### <a name="is-migration-with-azure-importexport-service-supported"></a>É a migração com suporte do serviço de importação/exportação do Azure?

Não. Serviço de importação/exportação do Azure não oferece suporte no momento, os arquivos do Azure NetApp.

## <a name="next-steps"></a>Próximas etapas  

- [Perguntas frequentes sobre o Microsoft Azure ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-faqs)
- [Perguntas frequentes sobre de rede Virtual do Microsoft Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq)
- [Como criar uma solicitação de suporte do Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request)
- [Azure Data Box](https://docs.microsoft.com/azure/databox-family/)