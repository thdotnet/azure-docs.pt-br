---
title: Configurações de armazenamento de máquina virtual do Azure do SAP HANA | Microsoft Docs
description: Recomendações de armazenamento para a VM com SAP HANA implantado neles.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/05/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d062b6fff9693d5bda75edd65b8fe88d834eff57
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735505"
---
# <a name="sap-hana-azure-virtual-machine-storage-configurations"></a>Configurações de armazenamento de máquina virtual do SAP HANA no Azure

O Azure fornece diferentes tipos de armazenamento que são adequados para VMs do Azure que executam o SAP HANA. Os tipos de armazenamento do Azure que podem ser considerados para a lista de implantações do SAP HANA, como: 

- Unidades de disco SSD Standard (SSD)
- Unidades de estado sólido premium (SSD)
- Ultra SSD em visualização pública e ainda não tem suporte com aplicativos SAP de produção

Para saber mais sobre esses tipos de disco, consulte o artigo [selecionar um tipo de disco](https://docs.microsoft.com/azure/virtual-machines/linux/disks-types)

O Azure oferece dois métodos de implantação para VHDs nos Armazenamentos do Azure Standard e Premium. Se o cenário geral permitir, aproveite as implantações do [Azure Managed disk](https://azure.microsoft.com/services/managed-disks/). 

Para obter uma lista de tipos de armazenamento e os respectivos SLAs em IOPS e taxa de transferência de armazenamento, revise a [documentação do Azure para discos gerenciado](https://azure.microsoft.com/pricing/details/managed-disks/).

**Recomendação: Usar o armazenamento Premium do Azure em conjunto com o Acelerador de gravação do Azure e usar o Azure Managed Disks para implantação**

No mundo local, raramente você precisou preocupar os subsistemas de e/s e seus recursos. O motivo era que o fornecedor do dispositivo precisava certificar-se de que os requisitos mínimos de armazenamento fossem atendidos para o SAP HANA. Ao criar a infraestrutura do Azure por conta própria, você deve estar ciente de alguns desses requisitos. Algumas das características de taxa de transferência mínima que serão solicitadas que resultam na necessidade:

- Ativar leitura/gravação **hana/log** de um 250 MB/s com tamanhos de e/s de 1 MB
- Habilite a atividade de leitura de pelo menos 400MB/seg para **/hana/data** para tamanhos de E / S de 16MB e 64MB
- Habilite a atividade gravação de pelo menos 250 MB/seg para **/hana/data** para tamanhos de E / S de 16MB e 64MB

Uma vez que a baixa latência de armazenamento é crítica aos sistemas DBMS, mesmo que o DBMS, como SAP HANA, mantenha os dados na memória. O caminho crítico no armazenamento é geralmente em torno das gravações de log de transações dos sistemas DBMS. Mas também operações como gravar pontos de salvamento ou carregar dados na memória após a recuperação de falhas poderão ser críticas. Portanto, é **obrigatórios** utilizar discos de Premium do Azure para **hana/data** e **hana/log** volumes. Para alcançar a taxa de transferência mínima **hana/log** e **hana/data** conforme desejado pelo SAP, você precisa para criar um RAID 0 utilizando MDADM ou LVM em vários discos de armazenamento Premium do Azure. E usar os volumes RAID como **hana/data** e **hana/log** volumes. 

**Recomendação: Tamanhos de faixa para RAID 0 a recomendação é usar:**

- 64 KB ou 128 KB para **hana/data**
- 32KB para **hana/log**

> [!NOTE]
> Não é necessário configurar nenhum nível de redundância usando volumes RAID, pois o armazenamento Premium e Standard do Azure mantém três imagens de um VHD. O uso de um volume RAID é puramente para configurar volumes que fornecem rendimento de E/S suficiente.

A acumulação de vários VHDs do Azure sob um RAID é cumulativa de um lado de taxa de transferência de IOPS e armazenamento. Portanto, se você colocar um RAID 0 em discos de Armazenamento Premium do Azure de 3 x P30, ele deverá fornecer três vezes a IOPS e três vezes a taxa de transferência de armazenamento de um único disco P30 do Armazenamento Premium do Azure.

As recomendações de cache abaixo pressupõem que as características de E/S para o SAP HANA sejam listadas como:

- Praticamente, não há nenhuma carga de trabalho de leitura nos arquivos de dados do HANA. As exceções são E / Ss de tamanho grande após o reinício da instância do HANA ou quando os dados são carregados no HANA. Outro caso de E/Ss de leitura maiores em arquivos de dados podem ser backups de banco de dados do HANA. Como resultado, em grande parte, o cache de leitura não faz sentido pois, na maioria dos casos, todos os volumes de arquivos de dados precisam ser lidos na íntegra.
- A gravação nos arquivos de dados está ocorrendo em intermitências com base nos pontos de salvamento e na recuperação de pane do HANA. Os pontos de salvamento de gravação são assíncronos e não impedem nenhuma transação de usuário. A gravação de dados durante a recuperação de pane é crítica para o desempenho para que o sistema responda com rapidez novamente. No entanto, a recuperação de pane devem ser situações excepcionais
- Praticamente, não há nenhuma leitura dos arquivos de refazer do HANA. As exceções são E/Ss grandes ao executar backups de log de transações, recuperação de pane ou na fase de reinicialização de uma instância do HANA.  
- A carga principal no arquivo de log de refazer do SAP HANA são gravações. Dependendo da natureza da carga de trabalho, você pode ter E/Ss pequenas de 4 KB ou, em outros casos, tamanhos de E/S iguais ou superiores a 1 MB. A latência de gravação no log de refazer do SAP HANA é crítica para o desempenho.
- Todas as gravações precisam ser persistidas em disco de forma confiável

**Recomendação: Como resultado desses padrões de e/s observados pelo SAP HANA, o armazenamento em cache dos diferentes volumes usando o armazenamento Premium do Azure deve ser definido como:**

- **hana/data** -sem cache
- **hana/log** - sem cache - exceção para a série M (veja mais adiante neste documento)
- **hana/shared** - ler o cache


Considere a taxa de transferência de E/S geral ao dimensionar ou escolher uma VM. A taxa de transferência de armazenamento de VM geral está documentada no artigo [Tamanhos da máquina virtual otimizada para memória](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

## <a name="linux-io-scheduler-mode"></a>Modo Agendador de E/S do Linux
O Linux possui vários modos de agendamento de E/S diferentes. Recomendação comum por meio de fornecedores do Linux e do SAP é reconfigurar o modo de Agendador de e/s para volumes de disco desde o **cfq** modo para o **noop** modo. Detalhes são referenciados na [Nota SAP Nº 1984798](https://launchpad.support.sap.com/#/notes/1984787). 


## <a name="production-storage-solution-with-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Solução de armazenamento de produção com o Acelerador de gravação do Azure para máquinas virtuais da série M do Azure
O Acelerador de Gravação do Azure é uma funcionalidade que está sendo implementada exclusivamente para VMs da Série M do Azure. Como o nome indica, o objetivo da funcionalidade é melhorar a latência de E/S das gravações no Armazenamento Premium do Azure. Para o SAP HANA, acelerador de gravação deve ser usado em relação a **hana/log** somente no volume. Portanto, o **hana/data** e **hana/log** são volumes separados com o Acelerador de gravação do Azure com suporte a **hana/log** somente no volume. 

> [!IMPORTANT]
> Certificação do SAP HANA para máquinas virtuais da série M do Azure é exclusivamente com o Acelerador de gravação do Azure para o **hana/log** volume. Como resultado, as implantações de SAP HANA de cenário de produção em máquinas virtuais da série M do Azure devem ser configurados com o Acelerador de gravação do Azure para o **hana/log** volume.  

> [!NOTE]
> Para cenários de produção, verifique se um determinado tipo de VM tem suporte pelo SAP HANA pelo SAP na [documentação do SAP para IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

**Recomendação: As configurações recomendadas para cenários de produção se parecer com:**

| SKU da VM | RAM | Máx. VM E/S<br /> Produtividade | /hana/data | /hana/log | /hana/shared | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GiB | 1000 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1000 GB | 1000 MB/s | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 2 x P40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 4 x P40 |
| M208s_v2 | 2850 GiB | 1000 MB/s | 4 x P30 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P40 |
| M208ms_v2 | 5700 GiB | 1000 MB/s | 4 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P50 |
| M416s_v2 | 5700 GiB | 2000 MB/s | 4 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 3 x P50 |
| M416ms_v2 | 11400 GiB | 2000 MB/s | 8 x P40 | 2 x P20 | 1 x P30 | 1 x P10 | 1 x P6 | 4 x P50 |

Tipos de VM M416xx_v2 são ainda não disponibilizados pela Microsoft para o público. Verifique se a taxa de transferência de armazenamento para os diferentes volumes recomendados atende a carga de trabalho que você deseja executar. Se a carga de trabalho exigir volumes maiores para **hana/data** e **hana/log**, você precisa aumentar o número de VHDs de armazenamento Premium do Azure. Dimensionar um volume com mais VHDs que listados aumenta a IOPS e taxa de transferência de e/s dentro dos limites do tipo de máquina virtual do Azure.

O Acelerador de Gravação do Azure funciona somente em conjunto com o [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/). Portanto, pelo menos os discos de armazenamento Premium do Azure que formam o **hana/log** volume precisam ser implantados como discos gerenciados.

Há limites de VHDs de Armazenamento Premium do Azure por VM que podem ter suporte pelo Acelerador de Gravação do Azure. Os limites atuais são:

- 16 VHDs para uma VM M128xx e M416xx
- 8 VHDs para uma VM M64xx e M208xx
- 4 VHDs para uma VM M32xx

Instruções mais detalhadas sobre como habilitar o Acelerador de Gravação do Azure podem ser encontradas no artigo [Acelerador de Gravação](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).

Detalhes e restrições para o Acelerador de Gravação do Azure podem ser localizados na mesma documentação.

**Recomendação: Você precisa usar o Acelerador de gravação para discos que formam o volume /Hana/log.**


## <a name="cost-conscious-azure-storage-configuration"></a>Configuração com consciência de custos do Armazenamento do Microsoft Azure
A tabela a seguir mostra uma configuração de tipos de VM que os clientes também usam para hospedar o SAP HANA em VMs do Azure. Pode haver alguns tipos de VM que poderá não atender todos os critérios de armazenamento mínimo para o SAP HANA ou não tenham suporte oficial com o SAP HANA pelo SAP. Mas até o momento essas VMs parecem funcionar adequadamente para cenários de não produção. O **hana/data** e **hana/log** são combinados em um volume. Como resultado do uso do acelerador de gravação do Azure pode se tornar uma limitação em termos de IOPS.

> [!NOTE]
> Para cenários com suporte do SAP, verifique se um determinado tipo de VM tem suporte para o SAP HANA pelo SAP na [documentação do SAP para IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

> [!NOTE]
> É uma mudança da antigas recomendações para uma solução de custo consciente, mover de [discos de HDD Standard do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-hdd) para executar melhor e mais confiável [discos SSD Standard do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#standard-ssd)


| SKU da VM | RAM | Máx. VM E/S<br /> Produtividade | hana/data e hana/log<br /> distribuídos com LVM ou MDADM | /hana/shared | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 112 GiB | 768 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E15 |
| E16v3 | 128 GiB | 384 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E15 |
| E32v3 | 256 GiB | 768 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| E64v3 | 432 GiB | 1200 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E30 |
| GS5 | 448 GiB | 2000 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E30 |
| M32ts | 192 GiB | 500 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 | 1 x E20 |
| M64ls | 512 GiB | 1000 MB/s | 3 x P20 | 1 x E20 | 1 x E6 | 1 x E6 |1 x E30 |
| M64s | 1000 GB | 1000 MB/s | 2 x P30 | 1 x E30 | 1 x E6 | 1 x E6 |2 x E30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 1 x E30 | 1 x E6 | 1 x E6 | 3 x E30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 1 x E30 | 1 x E10 | 1 x E6 | 2 x E40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 1 x E30 | 1 x E10 | 1 x E6 | 2 x E50 |
| M208s_v2 | 2850 GiB | 1000 MB/s | 4 x P30 | 1 x E30 | 1 x E10 | 1 x E6 |  3 x E40 |
| M208ms_v2 | 5700 GiB | 1000 MB/s | 4 x P40 | 1 x E30 | 1 x E10 | 1 x E6 |  4 x E40 |
| M416s_v2 | 5700 GiB | 2000 MB/s | 4 x P40 | 1 x E30 | 1 x E10 | 1 x E6 |  4 x E40 |
| M416ms_v2 | 11400 GiB | 2000 MB/s | 8 x P40 | 1 x E30 | 1 x E10 | 1 x E6 |  4 x E50 |


Tipos de VM M416xx_v2 são ainda não disponibilizados pela Microsoft para o público. Os discos recomendados para os tipos menores de VM com 3 x P20 ultrapassam os volumes de recomendação de espaço conforme o [Whitepaper de armazenamento do SAP TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). No entanto a opção conforme exibida na tabela foi feita para fornecer a taxa suficiente de transferência de disco para o SAP HANA. Se você precisar de alterações no volume **/hana/backu**p, que foi dimensionado para manter backups que representam o dobro do volume da memória, sinta-se à vontade para fazer ajustes.   
Verifique se a taxa de transferência de armazenamento para os diferentes volumes recomendados atende a carga de trabalho que você deseja executar. Se a carga de trabalho exigir volumes maiores para **hana/data** e **hana/log**, você precisa aumentar o número de VHDs de armazenamento Premium do Azure. Dimensionar um volume com mais VHDs que listados aumenta a IOPS e taxa de transferência de e/s dentro dos limites do tipo de máquina virtual do Azure. 

> [!NOTE]
> As configurações acima NÃO seriam benefício de [SLA de única VM de máquina virtual do Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/), pois ele utiliza uma combinação do Armazenamento Premium do Azure e Armazenamento Standard do Azure. No entanto, a seleção foi escolhida para otimizar os custos. É necessário escolher o Armazenamento Premium para todos os discos acima listados como o Armazenamento Standard do Azure (Sxx) para tornar a configuração da VM compatível com o SLA da VM única do Azure.


> [!NOTE]
> As recomendações de configuração de disco declaradas estão visando requisitos mínimos que o SAP expressa para seus provedores de infraestrutura. Em cenários de carga de trabalho e implantações de clientes reais, houve situações em que essas recomendações ainda não forneciam recursos suficientes. Essas poderiam ser situações em que um cliente precisava de um recarregamento mais rápido dos dados após uma reinicialização do HANA ou em que as configurações de backup demandavam mais alta largura de banda para o armazenamento. Outros casos incluíram **/hana/log**, onde 5000 IOPS não foram suficientes para a carga de trabalho específica. Portanto, considere essas recomendações como um ponto de partida e adapte-se com base nos requisitos da carga de trabalho.
>  

## <a name="azure-ultra-ssd-storage-configuration-for-sap-hana"></a>Configuração de armazenamento de SSD Ultra do Azure para SAP HANA
Microsoft está no processo de introduzir um novo tipo de armazenamento do Azure chamado [SSD de Ultra Azure](https://azure.microsoft.com/updates/ultra-ssd-new-azure-managed-disks-offering-for-your-most-latency-sensitive-workloads/). A grande diferença entre o armazenamento do Azure oferecido até o momento e Ultra SSD é que os recursos de disco não estão vinculados ao tamanho do disco mais. Como um cliente, você pode definir esses recursos para SSD Ultra:

- Tamanho de um disco desde 4 GiB até 65.536 GiB
- Intervalo IOPS de 100 IOPS a 160 mil IOPS (máximo depende também tipos de VM)
- Taxa de transferência do armazenamento de 300 MB/s para 2.000 MB/s

Para obter detalhes leia o artigo [anunciando Ultra SSD – a próxima geração de tecnologia de discos do Azure (visualização)](https://azure.microsoft.com/blog/announcing-ultra-ssd-the-next-generation-of-azure-disks-technology-preview/)

UltraSSD lhe dá a possibilidade de definir um disco único que atenda a seu tamanho, o IOPS e o intervalo de taxa de transferência do disco. Em vez de usar gerenciadores de volume lógico como LVM ou MDADM sobre o armazenamento Premium do Azure para construir os volumes que atendam aos requisitos de taxa de transferência IOPS e armazenamento. Você pode executar uma combinação de configuração entre UltraSSD e o armazenamento Premium. Como resultado, você pode limitar o uso de UltraSSD para /hana/data críticos de desempenho e /Hana/log. e abrangem os outros volumes de armazenamento Premium do Azure

| SKU da VM | RAM | Máx. VM E/S<br /> Produtividade | /hana/data volume | taxa de transferência de e/s de dados Hana | /hana/data IOPS | /hana/log volume | taxa de transferência de e/s de log Hana | /hana/log IOPS |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 GiB | 500 MB/s | 250 GB | 500 MBps | 7500 | 256 GB | 500 MBps  | 2000 |
| M32ls | 256 GiB | 500 MB/s | 300 GB | 500 MBps | 7500 | 256 GB | 500 MBps  | 2000 |
| M64ls | 512 GiB | 1000 MB/s | 600 GB | 500 MBps | 7500 | 512 GB | 500 MBps  | 2000 |
| M64s | 1000 GB | 1\.000 MB/s |  1\.200 GB | 500 MBps | 7500 | 512 GB | 500 MBps  | 2000 |
| M64ms | 1750 GiB | 1\.000 MB/s | 2100 GB | 500 MBps | 7500 | 512 GB | 500 MBps  | 2000 |
| M128s | 2000 GiB | 2\.000 MB/s |2400 GB | 1200 MBps |9000 | 512 GB | 800 MBps  | 2000 | 
| M128ms | 3800 GiB | 2\.000 MB/s | 4800 GB | 1200 MBps |9000 | 512 GB | 800 MBps  | 2000 | 
| M208s_v2 | 2850 GiB | 1\.000 MB/s | 3500 GB | 1000 MBps | 9000 | 512 GB | 500 MBps  | 2000 | 
| M208ms_v2 | 5700 GiB | 1\.000 MB/s | 7200 GB | 1000 MBps | 9000 | 512 GB | 500 MBps  | 2000 | 
| M416s_v2 | 5700 GiB | 2\.000 MB/s | 7200 GB | 1500 M Bps | 9000 | 512 GB | 800 MBps  | 2000 | 
| M416ms_v2 | 11400 GiB | 2\.000 MB/s | 14400 GB | 1500 MBps | 9000 | 512 GB | 800 MBps  | 2000 |   

Tipos de VM M416xx_v2 são ainda não disponibilizados pela Microsoft para o público. Os valores listados devem ser um ponto de partida e precisam ser avaliadas em relação às demandas reais. A vantagem com SSD Ultra do Azure é que os valores de IOPS e taxa de transferência podem ser adaptados sem a necessidade de desligar a VM ou interrompendo a carga de trabalho aplicados ao sistema.   

> [!NOTE]
> Até agora, os instantâneos de armazenamento com armazenamento UltraSSD não está disponível. Isso bloqueia o uso de instantâneos da VM com os serviços de Backup do Azure

## <a name="next-steps"></a>Próximas etapas
Para obter mais informações, consulte:

- [Guia de alta disponibilidade do SAP HANA para máquinas virtuais do Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).