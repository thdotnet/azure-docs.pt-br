---
title: Personalizar as avaliações para avaliação de servidor de migrar do Azure | Microsoft Docs
description: Descreve como personalizar as avaliações criadas com a avaliação de servidor de migrar do Azure
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 07/09/2019
ms.author: raynew
ms.openlocfilehash: 8b200ce3d6e73a575b1b89d82a9323d58f435a48
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807920"
---
# <a name="customize-an-assessment"></a>Personalizar uma avaliação

Este artigo descreve como personalizar as avaliações criadas pela avaliação de servidor de migrar do Azure.

[As migrações para Azure](migrate-services-overview.md) fornece um hub central para acompanhar a descoberta, avaliação e migração de seus aplicativos locais e cargas de trabalho e VMs, para o Azure na nuvem privada/pública. O hub fornece ferramentas de migrações para Azure para avaliação e migração, bem como ofertas do ISV (fornecedor) independentes de software de terceiros.

Você pode usar a ferramenta de avaliação de servidor de migrar do Azure para criar avaliações para VMs do VMware locais e VMs do Hyper-V, em preparação para migração para o Azure. 

## <a name="about-assessments"></a>Sobre avaliações

Há dois tipos de avaliações, que você pode executar usando a avaliação de servidor de migrar do Azure.

**Avaliação** | **Detalhes** | **Dados**
--- | --- | ---
**Com base no desempenho** | Avaliações com base nos dados de desempenho coletados | **Tamanho VM recomendado**: Com base nos dados de utilização de CPU e memória.<br/><br/> **O tipo de disco (disco padrão ou premium gerenciado) recomendado**: Com base na IOPS e taxa de transferência dos discos no local.
**Como no local** | Avaliações com base no local de dimensionamento. | **Tamanho VM recomendado**: Com base no tamanho da VM local<br/><br> **Tipo de disco recomendado**: Com base na configuração do tipo de armazenamento selecionada para a avaliação.


## <a name="how-is-an-assessment-done"></a>Como uma avaliação é feita?

Uma avaliação das Migrações para Azure tem três etapas. A avaliação começa com uma análise de adequação, seguida de dimensionamento, e por fim, uma estimativa de custo mensal. Uma máquina só passa para um estágio posterior se passar pelo anterior. Por exemplo, se um computador falhar na verificação de adequação do Azure, será marcado como não adequado para o Azure, e o dimensionamento e o custo não serão calculados.

## <a name="whats-in-an-assessment"></a>O que é uma avaliação?

**Property** | **Detalhes**
--- | ---
**Local de destino** | O local do Azure para o qual você deseja migrar.<br/> As migrações para Azure atualmente dá suporte a essas regiões de destino: Leste da Austrália, Sudeste da Austrália, Sul do Brasil, Canadá Central, Leste do Canadá, Índia Central, centro dos EUA, Leste da China, Norte da China, Ásia Oriental, Leste dos EUA, Leste dos EUA 2, Alemanha Central, Alemanha – nordeste da Alemanha, Japão Leste, oeste do Japão, Coreia Central, Coreia Sul, Norte Centro dos EUA, Europa Setentrional, Centro-Sul dos EUA, Sudeste Asiático, Sul da Índia, Sul do Reino Unido, oeste do Reino Unido, gov – Arizona, US gov – Texas, US Gov. EUA Virgínia, EUA Central, Europa Ocidental, Índia Ocidental, oeste dos EUA e Oeste dos EUA 2.<br/> Por padrão, a região de destino é definida como Oeste dos EUA 2.
**Tipo de armazenamento** | Discos SSD de discos/Standard de HDD padrão/Premium.<br/> Quando você especifica o tipo de armazenamento como automáticos em uma avaliação, a recomendação de disco é baseada nos dados de desempenho dos discos (IOPS e taxa de transferência).<br/> Se você especificar o tipo de armazenamento como Premium/Standard, recomenda a avaliação um disco SKU dentro do tipo de armazenamento selecionada.<br/> Se você quiser obter uma única instância VM SLA de 99,9%, você pode definir o tipo de armazenamento como discos gerenciados Premium. Em seguida, todos os discos na avaliação serão recomendados como discos gerenciados Premium. <br/> As Migrações para Azure são compatíveis com discos gerenciados apenas para avaliação de migração.<br/> 
**Instâncias reservadas (RI)** | Especifica essa propriedade, se você tiver reservado instâncias no Azure. As estimativas de custo na avaliação levarão descontos de RI em conta. As instâncias reservadas são atualmente só tem suporte para pago pelo uso oferece nas migrações para Azure.
**Critério de dimensionamento** | Usado para redimensionar VMs. Dimensionamento pode ser baseada em desempenho, ou **como local**, sem considerar o histórico de desempenho.
**Histórico de desempenho** | A duração a considerar para avaliar o desempenho da VM. Essa propriedade só é aplicável quando o dimensionamento é baseado em desempenho.
**Utilização de percentual** | O valor percentual da amostra de desempenho que é usado para VMs de redimensionamento. Essa propriedade só é aplicável quando o dimensionamento é baseado em desempenho.
**Série de VM** | A série de VM usada para estimativas de tamanho. Por exemplo, se você tiver um ambiente de produção que não pretende migrar para VMs da série A no Azure, poderá excluir a série A da lista ou da série. O dimensionamento baseia-se apenas na série selecionada.
**Fator de conforto** | Avaliação de servidor de migrar do Azure consideram um buffer (fator de conforto) durante a avaliação. Esse buffer é aplicado sobre os dados de utilização da máquina para VMs (CPU, memória, disco e rede). O fator de conforto considera problemas como uso sazonal, histórico curto de desempenho e aumento provável do uso futuro.<br/><br/> Por exemplo, uma VM com 10 núcleos e 20% de utilização normalmente resulta em uma VM de dois núcleos. No entanto, com um fator de conforto de 2.0x, o resultado é uma VM de quatro núcleos.
**Oferta** | A [oferta do Azure](https://azure.microsoft.com/support/legal/offer-details/) na qual você se inscreveu. As Migrações para Azure calculam o custo de acordo com isso.
**Moeda** | Moeda de cobrança. 
**Desconto (%)** | Qualquer desconto específico da assinatura  recebido por você sobre a oferta do Azure.<br/> A configuração padrão é 0%.
**Tempo de atividade da VM** | Se as VMs não estiverem em execução 24x7 no Azure, será possível especificar a duração da execução (número de dias por mês e número de horas por dia) e as estimativas de custo serão feitas adequadamente.<br/> O valor padrão é 31 dias por mês e 24 horas por dia.
**Benefício Híbrido do Azure** | Especifica se você tiver o software assurance e é elegíveis para [benefício híbrido do Azure](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Se definido como Sim, os preços do Windows Azure são considerados para VMs do Windows. | O padrão é Sim.


## <a name="edit-assessment-properties"></a>Editar propriedades de avaliação

Para editar propriedades de avaliação após a criação de uma avaliação, faça o seguinte:

1. No projeto de migrações para Azure, clique em **servidores**.
2. No **migrações para Azure: Avaliação de servidor**, clique na contagem de avaliações.
3. Na **Assessment**, clique em avaliação relevante > **editar propriedades**.
5. Personalize as propriedades de avaliação de acordo com a tabela acima.
6. Clique em **Salvar** para atualizar a avaliação.


Você também pode editar as propriedades de avaliação quando você estiver criando uma avaliação.


## <a name="next-steps"></a>Próximas etapas

[Saiba mais](concepts-assessment-calculation.md) sobre como as avaliações são calculadas.
