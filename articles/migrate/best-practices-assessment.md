---
title: Práticas recomendadas para a criação de avaliação com a avaliação do servidor de migrar do Azure | Microsoft Docs
description: Fornece dicas para criar avaliações com avaliação de servidor de migrar do Azure.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: raynew
ms.openlocfilehash: d417efd4abf14247af171ea77b479f590e14fe76
ms.sourcegitcommit: af31deded9b5836057e29b688b994b6c2890aa79
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812971"
---
# <a name="best-practices-for-creating-assessments"></a>Práticas recomendadas para criar avaliações

[As migrações para Azure](migrate-overview.md) fornece um hub de ferramentas que ajudam você a descobrir, avaliar e migrar cargas de trabalho, infraestrutura e aplicativos para o Microsoft Azure. O hub inclui ferramentas de migrações para Azure e as ofertas do ISV (fornecedor) independentes de software de terceiros. 

Este artigo resume as práticas recomendadas ao criar avaliações usando a ferramenta de avaliação de servidor de migrar do Azure. 

## <a name="about-assessments"></a>Sobre avaliações

Avaliações de que criar com a avaliação do servidor de migrar do Azure são um instantâneo point-in-time de dados. Há dois tipos de avaliações de migrações para Azure.

**Tipo de avaliação** | **Detalhes** | **Dados**
--- | --- | ---
**Com base no desempenho** | Avaliações que fazem recomendações com base nos dados de desempenho coletados | Recomendação de tamanho VM baseia-se nos dados de utilização de CPU e memória.<br/><br/> Recomendação de tipo de disco (standard ou premium para discos gerenciados) baseia-se na IOPS e taxa de transferência dos discos no local.
**Como-está no local** | Avaliações que não usam dados de desempenho para fazer recomendações. | Recomendação de tamanho VM baseia-se no tamanho da VM local<br/><br> O tipo de disco recomendado se baseia em que você selecionar a configuração para a avaliação do tipo de armazenamento.

### <a name="example"></a>Exemplo
Por exemplo, se você tiver uma VM no local com quatro núcleos em 20% de utilização e a memória de 8 GB com 10% de utilização, as avaliações será da seguinte maneira:

- **Avaliação de desempenho**:
    - Recomenda núcleos e memória com base nos núcleos (núcleos 0,8) e utilização de memória (0,8 GB).
    - A avaliação se aplica a um fator de conforto padrão de 30%.
    - Recomendação de VM: ~1.4 núcleos (0,8 x1.3) e ~1.4 GB de memória.
- **Como-é a avaliação de (local)** :
    -  Recomenda uma VM com quatro núcleos; 8 GB de memória.

## <a name="best-practices-for-creating-assessments"></a>Práticas recomendadas para criar avaliações

O dispositivo de migrações para Azure continuamente identifica seu ambiente local e envia os metadados e dados de desempenho para o Azure. Siga estas práticas recomendadas para a criação de avaliações:

- **Criar como-está avaliações**: Você pode criar como-está avaliações imediatamente após a descoberta.
- **Criar avaliação com base em desempenho**: Depois de configurar a descoberta, é recomendável que você espere pelo menos um dia antes de executar uma avaliação de desempenho:
    - Coletando dados de desempenho leva tempo. Aguardar pelo menos um dia garante que há suficiente pontos de dados de desempenho antes de executar a avaliação.
    - Dados de desempenho, o dispositivo coleta pontos de dados em tempo real a cada 20 segundos para cada métrica de desempenho e acumula-los até um ponto de dados único de cinco minutos. O dispositivo envia os dados de cinco minutos apontam para o Azure a cada hora para o cálculo de avaliação.  
- **Obter os dados mais recentes**: As avaliações não são atualizadas automaticamente com os dados mais recentes. Para atualizar uma avaliação com os dados mais recentes, você precisará executá-lo novamente. 
- **Certifique-se de correspondem durações**: Quando você estiver executando avaliações com base em desempenho, verifique se seu perfil de seu ambiente para a duração de avaliação. Por exemplo, se você criar uma avaliação com duração de desempenho definida como uma semana, você precisa esperar pelo menos uma semana depois de iniciar a descoberta, para todos os pontos de dados a serem coletados. Se você não fizer isso, a avaliação não terá uma classificação de cinco estrelas. 
- **Evitar a perda de pontos de dados**: Os problemas a seguir podem resultar na ausência de pontos de dados em uma avaliação de desempenho:
    - As VMs estão desligadas durante a avaliação e dados de desempenho não são coletados. 
    - Se você criar VMs durante o mês no qual basear o histórico de desempenho. os dados para essas VMs serão menos de um mês. 
    - A avaliação é criada imediatamente após a descoberta, ou o tempo de avaliação não corresponde o tempo de coleta de dados de desempenho.

## <a name="best-practices-for-confidence-ratings"></a>Práticas recomendadas para classificações de confiança

Quando você executar avaliações com base em desempenho, uma classificação de estrelas de 1 (mais baixa) a 5 estrelas (mais alta) de confiança recebeu o prêmio para a avaliação. Para usar as classificações de confiança efetivamente:
- Avaliação de servidor de migrar do Azure precisa os dados de utilização de CPU/memória da VM e o disco de dados IOPS/taxa de transferência.
- Para cada adaptador de rede anexado a uma VM, migrações para Azure precisam de dados de entrada/saída de rede.
- Se os dados de utilização não estão disponíveis no vCenter Server, a recomendação de tamanho feita pelas migrações para Azure pode não ser confiável. 

Dependendo do percentual dos pontos de dados disponíveis, as classificações de confiança para uma avaliação são resumidas na tabela a seguir.

   **Disponibilidade do ponto de dados** | **Classificação de confiança**
   --- | ---
   0%-20% | 1 estrela
   21%-40% | 2 estrelas
   41%-60% | 3 estrelas
   61%-80% | 4 estrelas
   81%-100% | 5 estrelas

- Se você receber uma classificação de confiança para uma avaliação que estiver abaixo de cinco estrelas, aguarde pelo menos um dia e, em seguida, recalcular a avaliação.
- Uma classificação baixa significa que a recomendações de dimensionamento podem não ser confiável. Nesse caso, é recomendável que você modifique as propriedades de avaliação para usar como-é a avaliação de locais.

## <a name="common-assessment-issues"></a>Problemas comuns de avaliação

Aqui está como corrigir alguns problemas comuns de ambiente que afetam as avaliações.

###  <a name="out-of-sync-assessments"></a>Avaliações de fora de sincronia

Se você adicionar ou remove computadores de um grupo depois de criar uma avaliação, a avaliação que você criou será marcada **fora de sincronia**. Execute novamente a avaliação (**recalcular**) para refletir as alterações de grupo.

### <a name="outdated-assessments"></a>Avaliações desatualizadas

Se houver alterações locais às VMs que estão em um grupo foi avaliado, a avaliação é marcada **desatualizados**. Para refletir as alterações, execute novamente a avaliação.

### <a name="missing-data-points"></a>Pontos de dados ausentes

Uma avaliação pode não ter todos os pontos de dados por uma série de motivos:

- VMs podem ser desligadas durante a avaliação e dados de desempenho não são coletados. 
- As VMs podem ser criadas durante o mês no qual desempenho histórico se baseia, portanto, seus dados de desempenho são menos de um mês. 
- A avaliação foi criada imediatamente após a descoberta. Para coletar dados de desempenho para um período de tempo especificado, você precisa esperar o período de tempo antes de executar uma avaliação especificado. Por exemplo, se você quiser avaliar os dados de desempenho por uma semana, você precisa aguardar uma semana após a descoberta. Se você não a avaliação não terá uma classificação de cinco estrelas. 


## <a name="next-steps"></a>Próximas etapas

- [Saiba mais](concepts-assessment-calculation.md) como as avaliações são calculadas.
- [Saiba mais](how-to-modify-assessment.md) como personalizar uma avaliação.
