---
title: Cálculos de avaliação em Migrações para Azure | Microsoft Docs
description: Fornece uma visão geral dos cálculos de avaliação no serviço Migrações para Azure.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: raynew
ms.openlocfilehash: a328549307772cbdf470160cc1ad713fe1ee5e05
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67805982"
---
# <a name="assessment-calculations"></a>Cálculos de avaliação

Avaliação de servidor de migrar do Azure avalia cargas de trabalho local para a migração para o Azure. Este artigo fornece informações sobre como as avaliações são calculadas.


[As migrações para Azure](migrate-services-overview.md) fornece um hub central para acompanhar a descoberta, avaliação e migração de seus aplicativos locais e cargas de trabalho e instâncias de nuvem privada/pública, para o Azure. O hub fornece ferramentas de migrações para Azure para avaliação e migração, bem como ofertas do ISV (fornecedor) independentes de software de terceiros.

## <a name="overview"></a>Visão geral

Uma avaliação de avaliação de servidor de migrar do Azure tem três etapas. A avaliação começa com uma análise de adequação, seguida de dimensionamento, e por fim, uma estimativa de custo mensal. Uma máquina só passa para um estágio posterior se passar pelo anterior. Por exemplo, se um computador falhar na verificação de adequação do Azure, será marcado como não adequado para o Azure, e o dimensionamento e o custo não serão calculados.


## <a name="whats-in-an-assessment"></a>O que é uma avaliação?

**Property** | **Detalhes**
--- | ---
**Local de destino** | O local do Azure para o qual você deseja migrar.<br/><br/> As migrações para Azure atualmente dá suporte a essas regiões de destino: Leste da Austrália, Sudeste da Austrália, Sul do Brasil, Canadá Central, Leste do Canadá, Índia Central, centro dos EUA, Leste da China, Norte da China, Ásia Oriental, Leste dos EUA, Leste dos EUA 2, Alemanha Central, Alemanha – nordeste da Alemanha, Japão Leste, oeste do Japão, Coreia Central, Coreia Sul, Norte Centro dos EUA, Europa Setentrional, Centro-Sul dos EUA, Sudeste Asiático, Sul da Índia, Sul do Reino Unido, oeste do Reino Unido, gov – Arizona, US gov – Texas, US Gov. EUA Virgínia, EUA Central, Europa Ocidental, Índia Ocidental, oeste dos EUA e Oeste dos EUA 2.<br/> Por padrão, a região de destino é definida como Oeste dos EUA 2.
**Tipo de armazenamento** | Discos SSD de discos/Standard de HDD padrão/Premium.<br/><br/> Quando você especifica o tipo de armazenamento como automáticos em uma avaliação, a recomendação de disco é baseada nos dados de desempenho dos discos (IOPS e taxa de transferência).<br/><br/> Se você especificar o tipo de armazenamento como Premium/Standard, recomenda a avaliação um disco SKU dentro do tipo de armazenamento selecionada.<br/><br/> Se você quiser obter uma única instância VM SLA de 99,9%, você pode definir o tipo de armazenamento como discos gerenciados Premium. Em seguida, todos os discos na avaliação serão recomendados como discos gerenciados Premium. <br/> As Migrações para Azure são compatíveis com discos gerenciados apenas para avaliação de migração.<br/> 
**Instâncias reservadas (RI)** | Especifica essa propriedade, se você tiver reservado instâncias no Azure. As estimativas de custo na avaliação levarão descontos de RI em conta. As instâncias reservadas são atualmente só tem suporte para pago pelo uso oferece nas migrações para Azure.
**Critério de dimensionamento** | Usado para redimensionar VMs. Dimensionamento pode ser baseada em desempenho, ou como-está no local, sem considerar o histórico de desempenho.
**Histórico de desempenho** | A duração a considerar para avaliar o desempenho da VM. Essa propriedade só é aplicável quando o dimensionamento é baseado em desempenho.
**Utilização de percentual** | O valor percentual da amostra de desempenho que é usado para VMs de redimensionamento. Essa propriedade só é aplicável quando o dimensionamento é baseado em desempenho.
**Série de VM** | A série de VM usada para estimativas de tamanho. Por exemplo, se você tiver um ambiente de produção que não pretende migrar para VMs da série A no Azure, poderá excluir a série A da lista ou da série. O dimensionamento baseia-se apenas na série selecionada.
**Fator de conforto** | Avaliação de servidor de migrar do Azure consideram um buffer (fator de conforto) durante a avaliação. Esse buffer é aplicado sobre os dados de utilização da máquina para VMs (CPU, memória, disco e rede). O fator de conforto considera problemas como uso sazonal, histórico curto de desempenho e aumento provável do uso futuro.<br/><br/> Por exemplo, uma VM com 10 núcleos e 20% de utilização normalmente resulta em uma VM de dois núcleos. No entanto, com um fator de conforto de 2.0x, o resultado é uma VM de quatro núcleos.
**Oferta** | A [oferta do Azure](https://azure.microsoft.com/support/legal/offer-details/) na qual você se inscreveu. As Migrações para Azure calculam o custo de acordo com isso.
**Moeda** | Moeda de cobrança. 
**Desconto (%)** | Qualquer desconto específico da assinatura  recebido por você sobre a oferta do Azure.<br/> A configuração padrão é 0%.
**Tempo de atividade da VM** | Se as VMs não estiverem em execução 24x7 no Azure, será possível especificar a duração da execução (número de dias por mês e número de horas por dia) e as estimativas de custo serão feitas adequadamente.<br/> O valor padrão é 31 dias por mês e 24 horas por dia.
**Benefício Híbrido do Azure** | Especifica se você tiver o software assurance e é elegíveis para [benefício híbrido do Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Se definido como Sim, os preços do Windows Azure são considerados para VMs do Windows. 



## <a name="azure-suitability-analysis"></a>Análise de adequação do Azure

Nem todos os computadores são adequados para execução no Azure. Avaliação de servidor de migrar do Azure avalia cada computador local para a migração e categoriza máquinas em uma das seguintes categorias de adequação:
- **Pronto para o Azure** - O computador pode ser migrado como está para o Azure sem alterações. Ele será inicializado no Azure com o suporte completo do Azure.
- **Condicionalmente pronto para o Azure** - O computador pode ser inicializado no Azure, mas pode não ter suporte completo do Azure. Por exemplo, um computador com uma versão mais antiga do sistema operacional Windows Server não tem suporte no Azure. Você precisa ter cuidado antes de migrar esses computadores para o Azure e seguir as orientações de correção sugeridas na avaliação para corrigir os problemas de preparação antes de migrar.
- **Não está pronto para o Azure** - O computador não será inicializado no Azure. Por exemplo, se um computador local tem um disco com tamanho de mais de 4 TB anexado a ele, não poderá ser hospedado no Azure. Você precisa seguir as diretrizes de correção sugeridas na avaliação para corrigir o problema de preparação antes de migrar para o Azure. O dimensionamento correto e a estimativa de custo não são feitos para computadores marcados como não prontos para o Azure.
- **Preparação desconhecida** - As Migrações para Azure não puderam encontrar a preparação do computador devido a dados insuficientes disponíveis no vCenter Server.



### <a name="machine-properties"></a>Propriedades do computador

As migrações para Azure analisa as seguintes propriedades da VM local para identificar se ele pode ser executado no Azure.

**Property** | **Detalhes** | **Status de preparação do Azure**
--- | --- | ---
**Tempo de inicialização** | O Azure oferece suporte a VMs com tipo de inicialização BIOS, e não UEFI. | Condicionalmente pronto se o tipo de inicialização for UEFI.
**Núcleos** | O número de núcleos nos computadores deve ser igual ou menor que o número máximo de núcleos (128 núcleos) com suporte para uma VM do Azure.<br/><br/> Se o histórico de desempenho estiver disponível, as Migrações para Azure considerarão os núcleos utilizados para comparação. Se um fator de conforto for especificado nas configurações de avaliação, o número de núcleos utilizados será multiplicado pelo fator de conforto.<br/><br/> Se não houver histórico de desempenho, as Migrações para Azure usarão os núcleos alocados, sem aplicar o fator de conforto. | Pronto, se for menor que ou igual aos limites.
**Memória** | O tamanho de memória do computador deve ser igual ou menor do que a memória máxima (3892 GB na série M do Azure Standard_M128m&nbsp;<sup>2</sup>) permitida para uma VM do Azure. [Saiba mais](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).<br/><br/> Se o histórico de desempenho estiver disponível, as Migrações para Azure considerarão a memória utilizada para comparação. Se um fator de conforto for especificado, a memória utilizada será multiplicada pelo fator de conforto.<br/><br/> Se não houver histórico, a memória alocada será usada, sem aplicar o fator de conforto.<br/><br/> | Se estiver pronto dentro dos limites.
**Disco de armazenamento** | O tamanho alocado de um disco deve ser de 4 TB (4096 GB) ou menos.<br/><br/> O número de discos anexados à máquina deve ser de 65 ou menos, incluindo o disco do sistema operacional. | Se estiver pronto dentro dos limites.
**Rede** | Um computador deve ter 32 NICs ou menos conectados a ele. | Se estiver pronto dentro dos limites.

### <a name="guest-operating-system"></a>Sistema operacional convidado
Juntamente com as propriedades da VM, avaliação de servidor de migrar do Azure examina o sistema operacional das máquinas, para identificar se ele pode ser executado no Azure.

> [!NOTE]
> Avaliação de servidor de migrar do Azure usa o sistema operacional especificado para a VM no vCenter Server para análise. Avaliação de servidor de migrar do Azure é baseado em dispositivo para a descoberta VM, e não é possível verificar se o sistema operacional executado na VM é o mesmo que aquele especificado no vCenter Server.

A lógica a seguir é usada pela avaliação de servidor migrar do Azure, para identificar a preparação para o Azure com base no sistema operacional.

**Sistema operacional** | **Detalhes** | **Status de preparação do Azure**
--- | --- | ---
Windows Server 2016 e todos os SPs | O Azure fornece suporte total. | Pronto para o Azure
Windows Server 2012 R2 e todos os SPs | O Azure fornece suporte total. | Pronto para o Azure
Windows Server 2012 e todos os SPs | O Azure fornece suporte total. | Pronto para o Azure
Windows Server 2008 R2 com todos os SPs | O Azure fornece suporte total.| Pronto para o Azure
Windows Server 2008 (32 bits e 64 bits) | O Azure fornece suporte total. | Pronto para o Azure
Windows Server 2003, 2003 R2 | Esses sistemas operacionais passaram da data final de suporte e precisam de um [Contrato de Suporte Personalizado (CSA)](https://aka.ms/WSosstatement) para obter suporte no Azure. | Condicionalmente pronto para o Azure, considere atualizar o sistema operacional antes de migrar para o Azure.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Esses sistemas operacionais passaram da data final de suporte, o computador pode ser inicializado no Azure, mas não há suporte do sistema operacional fornecido pelo Azure. | Condicionalmente pronto para o Azure, considere atualizar o sistema operacional antes de migrar para o Azure.
Cliente do Windows 7, 8 e 10 | O Azure oferece suporte apenas [com a Assinatura do Visual Studio.](https://docs.microsoft.com/azure/virtual-machines/windows/client-images) | Condicionalmente pronta para o Azure
Windows 10 Pro Desktop | O Azure oferece suporte com [Direitos de Hospedagem multilocatário.](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment) | Condicionalmente pronta para o Azure
Windows Vista, XP Professional | Esses sistemas operacionais passaram da data final de suporte, o computador pode ser inicializado no Azure, mas não há suporte do sistema operacional fornecido pelo Azure. | Condicionalmente pronto para o Azure, considere atualizar o sistema operacional antes de migrar para o Azure.
Linux | O Azure endossa esses [sistemas operacionais Linux](../virtual-machines/linux/endorsed-distros.md). Outros sistemas operacionais podem ser inicializados no Azure, mas é recomendado atualizar o sistema operacional para uma versão endossada antes de migrar para o Azure. | Pronto para o Azure se a versão é aprovada.<br/><br/>Condicionalmente pronto se a versão não é aprovada.
Outros sistemas operacionais<br/><br/> Por exemplo, Oracle Solaris, sistema operacional Apple Mac etc., FreeBSD, etc. | O Azure não endossa esses sistemas operacionais. O computador pode ser inicializado no Azure, mas não há suporte do sistema operacional fornecido pelo Azure. | Condicionalmente pronto para o Azure, é recomendável atualizar o sistema operacional antes de migrar para o Azure.  
Sistema operacional especificado como **Outros** no vCenter Server | As Migrações para Azure não podem identificar o sistema operacional neste caso. | Preparação desconhecida. Certifique-se de que o sistema operacional em execução dentro da VM tem suporte no Azure.
Sistemas operacionais de 32 bits | O computador pode ser inicializado no Azure, mas o Azure pode não fornecer suporte completo. | Condicionalmente pronto para o Azure, considere atualizar o sistema operacional do computador de 32 bits para 64 bits antes de migrar para o Azure.

## <a name="sizing"></a>Dimensionamento

Depois que uma máquina é marcada como pronta para o Azure, as Migrações para Azure dimensionam a VM e seus discos para o Azure.

- Se a avaliação usa o dimensionamento com base no desempenho, o migrações para Azure consideram o histórico de desempenho da máquina para identificar o tipo de disco e tamanho VM no Azure. Esse método é especialmente útil se você alocou em excesso a VM local, mas a utilização é baixa, e você deseja redimensionar a VM no Azure para economizar custos.
- Se você estiver usar um como locais avaliação, avaliação de servidor de migrar do Azure será dimensionar as VMs com base nas configurações locais, sem considerar os dados de utilização. Nesse caso, dimensionamento de disco, se baseia no tipo de armazenamento que você especificar nas propriedades de avaliação (disco padrão ou disco Premium).

### <a name="performance-based-sizing"></a>Dimensionamento com base no desempenho

Para o dimensionamento baseado em desempenho, migrações para Azure começa com os discos anexados à VM, seguido por adaptadores de rede, e, em seguida, mapeia uma VM do Azure com base nos requisitos de computação da VM local.

- **Armazenamento**: As Migrações para Azure tentam mapear todos os discos anexados à máquina para um disco no Azure.
    - Para obter a E/S de disco por segundo (IOPS) e a taxa de transferência (MBps) efetivas, as Migrações para Azure multiplicam o IOPS de disco e a taxa de transferência com o fator de conforto. Com base nos valores eficazes da taxa de transferência e IOPS, as Migrações para Azure identificam se o disco deve ser mapeado para um disco padrão ou premium no Azure.
    - Se as Migrações para Azure não conseguirem encontrar um disco com a IOPS e a taxa de transferência necessárias, ele marcará a máquina como inadequada para o Azure. [Saiba mais](../azure-subscription-service-limits.md#storage-limits) sobre os limites do Azure por disco e VM.
    - Se ele encontrar um conjunto de discos adequados, as Migrações para Azure selecionarão aqueles que dão suporte ao método de redundância de armazenamento e ao local especificado nas configurações de avaliação.
    - Se houver vários discos qualificados, elas selecionarão os que têm o menor custo.
    - Se os dados de desempenho para discos não estiverem disponíveis, todos os discos serão mapeados para discos padrão no Azure.

- **Rede**: As Migrações para Azure tentam localizar uma VM do Azure que possa dar suporte ao número de adaptadores de rede anexados à máquina local e o desempenho exigido por esses adaptadores de rede.
    - Para obter o desempenho de rede efetivo da VM local, as Migrações para Azure agregam os dados transmitidos por segundo (MBps) do computador (saída da rede), em todos os adaptadores de rede, e aplicam o fator de conforto. Esse número é usado para localizar uma VM do Azure que pode dar suporte ao desempenho de rede necessário.
    - Juntamente com o desempenho da rede, ele também considera se a VM do Azure pode dar suporte ao número necessário de adaptadores de rede.
    - Se nenhum dado de desempenho de rede estiver disponível, somente a contagem de adaptadores de rede será considerada para o dimensionamento da VM.

- **Computação**: Depois que os requisitos de armazenamento e rede são calculados, as Migrações para Azure consideram os requisitos de CPU e memória para localizar um tamanho adequado de VM no Azure.
    - As Migrações para Azure analisam os núcleos e a memória utilizados e aplica o fator de conforto para obter os núcleos e memória efetivos. Com base nesse número, tentará encontrar um tamanho adequado de VM no Azure.
    - Se nenhum tamanho adequado for encontrado, a máquina será marcada como inadequada para o Azure.
    - Se um tamanho adequado for encontrado, as Migrações para Azure aplicarão os cálculos de armazenamento e rede. Depois, aplicam as configurações de localização e preços, para a recomendação final de tamanho da VM.
    - Se houver vários discos qualificados, será recomendado aquele com o menor custo.

### <a name="as-on-premises-sizing"></a>Como dimensionamento local

Se você usar como dimensionamento local, a avaliação de servidor aloca um SKU de VM no Azure com base no tamanho no local. De forma semelhante ao dimensionamento de disco, elas examinam o tipo de armazenamento especificado nas propriedades de avaliação (Standard/Premium) e recomendam o tipo de disco de acordo com ele. O tipo de armazenamento padrão é discos Premium.

## <a name="confidence-ratings"></a>Classificações de confiança
Cada avaliação de desempenho nas migrações para Azure está associada uma classificação de confiança que varia de um (mais baixa) a cinco começa (mais alta).
- A classificação de confiança é atribuída a uma avaliação com base na disponibilidade de pontos de dados necessários para calcular a avaliação.
- A classificação de confiança de uma avaliação ajuda a estimar a confiabilidade das recomendações de tamanho fornecidas pelas Migrações para Azure.
- Classificação de confiança não é aplicável para como avaliações no local.
- Para o dimensionamento baseado em desempenho, avaliação de servidor de migrar do Azure precisa de:
    - Os dados de utilização de CPU e a memória da VM.
    - Além disso, para cada disco anexado à VM, é necessário ter os dados da taxa de transferência e a IOPS do disco.
    - Da mesma forma para cada adaptador de rede anexado a uma VM, a classificação de confiança precisa a e/s de rede para fazer o dimensionamento com base no desempenho.
    - Se qualquer um dos números de utilização acima não estão disponíveis no vCenter Server, a recomendação de tamanho pode não ser confiável. 

Dependendo da porcentagem de pontos de dados disponível, o nível de confiança para as avaliações é fornecido conforme abaixo:

   **Disponibilidade dos pontos de dados** | **Classificação de confiança**
   --- | ---
   0%-20% | 1 estrela
   21%-40% | 2 estrelas
   41%-60% | 3 estrelas
   61%-80% | 4 estrelas
   81%-100% | 5 estrelas

### <a name="low-confidence-ratings"></a>Classificações de confiança baixa

Algumas das razões por que uma avaliação foi possível obter uma classificação de confiança baixa:

- Você não cria o perfil do seu ambiente para a duração para a qual você está criando a avaliação. Por exemplo, se você criar a avaliação com duração de desempenho definida como 1 dia, você precisa aguardar pelo menos um dia após iniciar a descoberta para todos os pontos de dados é coletado.
- Algumas VMs foram desligadas durante o período para o qual a avaliação é calculada. Se todas as VMs foram desligadas por algum tempo, avaliação de servidor de migrar do Azure não é possível coletar os dados de desempenho para esse período.
- Algumas VMs foram criadas durante o período para o qual a avaliação é calculada. Por exemplo, se você cria uma avaliação para o histórico de desempenho do último mês, mas algumas VMs foram criadas no ambiente de apenas uma semana atrás, o histórico de desempenho das novas VMs não será lá durante todo o período.

  > [!NOTE]
  > Se a classificação de confiança de qualquer avaliação estiver abaixo de cinco estrelas, é recomendável que você aguarde pelo menos um dia para o dispositivo para o ambiente de perfil e, em seguida, recalcular a avaliação. Se você não fizer isso, dimensionamento com base no desempenho pode não ser confiável e podemos e recomendável que você passe a avaliação para usar como dimensionamento local.
  
## <a name="monthly-cost-estimation"></a>Estimativa de custo mensal

Depois de concluir as recomendações de dimensionamento, migrações para Azure calcularão os custos de computação e armazenamento para após a migração.

- **Custo de Computação**: Usando o tamanho de VM do Azure recomendado, as Migrações para Azure usam a API de Cobrança para calcular o custo mensal para a VM.
    - O cálculo leva em conta sistema operacional, Software Assurance, instâncias reservadas, tempo de atividade da VM, localização e configurações de moeda.
    - Ele agrega o custo em todas as máquinas, para calcular o custo total de computação mensal.
- **Custo de armazenamento**: O custo mensal do armazenamento para uma máquina é calculado agregando o custo mensal de todos os discos anexados à máquina
    - Avaliação de servidor de migrar do Azure calcula o total de custos de armazenamento mensal agregando os custos de armazenamento de todas as máquinas.
    - Atualmente, o cálculo não leva em consideração as ofertas especificadas nas configurações de avaliação.

Os custos são exibidos na moeda especificada nas configurações de avaliação.


## <a name="next-steps"></a>Próximas etapas

Criar uma avaliação para [VMs VMware](tutorial-assess-vmware.md) ou [VMs Hyper-V](tutorial-assess-hyper-v.md).
