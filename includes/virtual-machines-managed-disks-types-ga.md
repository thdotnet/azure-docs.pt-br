---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 05/14/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 30df480eb314594cbc4d949302aff11e6d764b6f
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65815502"
---
## <a name="premium-ssd"></a>SSD Premium

Os SSDs Premium do Azure oferecem compatibilidade de disco de alto desempenho e baixa latência para VMs (máquinas virtuais) com cargas de trabalho com uso intensivo de E/S (entrada/saída). Para aproveitar a velocidade e o desempenho dos discos de armazenamento Premium, você pode migrar os discos de VM existentes para os SSDs Premium. Os SSDs Premium são adequados para aplicativos de produção críticos. Premium SSDs só pode ser usado com séries de VMs que são compatíveis com o armazenamento do premium.

Para saber mais sobre os tipos VM individuais e tamanhos no Azure para Windows, incluindo quais tamanhos são premium compatíveis com o armazenamento, consulte [tamanhos de VM do Windows](../articles/virtual-machines/windows/sizes.md). Para saber mais sobre os tamanhos no Azure e os tipos de VM individuais para o Linux, incluindo quais tamanhos são premium compatíveis com o armazenamento, consulte [tamanhos de VM Linux](../articles/virtual-machines/linux/sizes.md).

### <a name="disk-size"></a>Tamanho do disco
[!INCLUDE [disk-storage-premium-ssd-sizes](disk-storage-premium-ssd-sizes.md)]

Quando você provisiona um disco de armazenamento premium, ao contrário do armazenamento padrão, a capacidade, IOPS e taxa de transferência de disco são garantidos. Por exemplo, se você criar um disco P50, o Azure provisionará uma capacidade de armazenamento de 4.095 GB, 7.500 IOPS e uma taxa de transferência de 250 MB/s para o disco. O aplicativo pode usar a capacidade e o desempenho no todo ou em parte. Os discos SSD Premium são projetados para oferecer desempenho 99,9% do tempo de destino.

### <a name="transactions"></a>Transações

Para o SSDs premium, cada operação e/s menor ou igual a 256 KiB de taxa de transferência é considerada uma única operação de e/s. Operações de e/s de taxa de transferência maiores que 256 KiB são consideradas várias e/s de tamanho 256 KiB.

## <a name="standard-ssd"></a>SSD Standard

Os SSDs Standard do Azure são uma opção econômica de armazenamento otimizado para cargas de trabalho que necessitam de desempenho consistente em níveis de IOPS mais baixos. SSD padrão oferece uma experiência de bom nível de entrada para aqueles que desejam migrar para a nuvem, especialmente se passar por problemas com a variação das cargas de trabalho em execução nas soluções de HD no local. Em comparação com HDDs padrão, o SSDs padrão fornecem o melhor disponibilidade, consistência, confiabilidade e latência. Os SSDs Standard são adequado para servidores Web, servidores de aplicativos de IOPS baixo, aplicativos empresariais pouco usados e cargas de trabalho de Desenvolvimento/Teste. Como padrão HDDs, SSDs padrão estão disponível em todas as VMs do Azure.

### <a name="disk-size"></a>Tamanho do disco
[!INCLUDE [disk-storage-standard-ssd-sizes](disk-storage-standard-ssd-sizes.md)]

SSDs padrão é projetado para fornecer latências de milissegundo de dígito único para a maioria das operações de e/s e entregar o IOPS e taxa de transferência até os limites descritos a tabela 99% do tempo anterior. IOPS e taxa de transferência reais podem variar, às vezes, dependendo dos padrões de tráfego. SSDs Padrão fornecerão um desempenho mais consistente do que os discos HD com a menor latência.

### <a name="transactions"></a>Transações

Para o SSDs padrão, cada operação e/s menor ou igual a 256 KiB de taxa de transferência é considerada uma única operação de e/s. Operações de e/s de taxa de transferência maiores que 256 KiB são consideradas várias e/s de tamanho 256 KiB. Essas transações têm um impacto de cobrança.

## <a name="standard-hdd"></a>HDD Standard

Os HDs Standard do Azure oferecem compatibilidade de disco confiável e de baixo custo para VMs que executam cargas de trabalho insensíveis a latência. Ele também oferece suporte a tabelas, blobs, filas e arquivos. Com o Armazenamento Standard, os dados são armazenados em HDs (unidades de disco rígido). Ao trabalhar com VMs, você pode usar discos SSD e HD Standard para cenários de Dev/Test e cargas de trabalho menos críticas. HDDs padrão estão disponíveis em todas as regiões do Azure e podem ser usados com todas as VMs do Azure.

### <a name="disk-size"></a>Tamanho do disco
[!INCLUDE [disk-storage-standard-hdd-sizes](disk-storage-standard-hdd-sizes.md)]

### <a name="transactions"></a>Transações

Para os HDDs padrão, cada operação de e/s é considerada como uma única transação, independentemente do tamanho de e/s. Essas transações têm um impacto de cobrança.

## <a name="billing"></a>Cobrança

Ao usar discos gerenciados, as seguintes considerações de cobrança se aplicam:

- Tipo de disco
- Tamanho do disco gerenciado
- Instantâneos
- Transferências de dados de saída
- Número de transações

**Tamanho do disco gerenciado**: discos gerenciados são cobrados no tamanho provisionado. O Azure mapeia o tamanho provisionado (arredondado) para o tamanho de disco oferecido mais próximo. Para obter detalhes sobre os tamanhos de disco oferecidos, confira as tabelas anteriores. Cada disco é mapeado para um tamanho de disco provisionado compatível e é cobrado de acordo. Por exemplo, se você provisionar uma SSD Standard de 200 GiB, ela será mapeada para a oferta de tamanho de disco E15 (256 GiB). A cobrança por qualquer disco provisionado é rateada por hora usando o preço mensal para a oferta de Armazenamento Premium. Por exemplo, se você provisionou um disco E10 e o excluiu após 20 horas, será cobrado pela a oferta E10 rateada em 20 horas. Isso é independente da quantidade de dados reais gravados no disco.

**Instantâneos**: Instantâneos são cobrados com base no tamanho usado. Por exemplo, se você criar um instantâneo de um disco gerenciado com capacidade provisionada de 64 GiB e tamanho real de dados usados de 10 GiB, o instantâneo será cobrado apenas pelo tamanho de dados usados de 10 GiB.