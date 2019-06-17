---
title: Modelos de compra do Banco de Dados SQL do Azure| Microsoft Docs
description: Saiba mais sobre modelos de compra que estão disponíveis para o banco de dados SQL.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 04/26/2019
ms.openlocfilehash: 98c19732c372fbcda3ca8e746d002f94c2687b22
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66431375"
---
# <a name="choose-between-the-vcore-and-the-dtu-purchasing-models"></a>Escolha entre o vCore e o modelos de compra de DTU

Banco de dados SQL do Azure permite que você compre facilmente uma plataforma totalmente gerenciada como um mecanismo de banco de dados de serviço (PaaS) que atenda às suas necessidades de desempenho e custo. Dependendo do modelo de implantação que você escolheu para o banco de dados SQL, você pode selecionar o modelo de compra que funciona para você:

- [VCore (núcleo virtual) – com base em modelo de compra](sql-database-service-tiers-vcore.md) (recomendado). O modelo de compra fornece uma escolha entre uma camada de computação provisionada e uma camada de computação sem servidor (visualização). Com a camada de computação provisionada, você pode escolher a quantidade exata de recursos de computação que sempre são provisionados para sua carga de trabalho. Com a camada de computação sem servidor, você pode especificar o dimensionamento automático dos recursos de computação em um intervalo configurável de computação. Com essa camada de computação, você pode também automaticamente pausar e retomar o banco de dados com base na atividade de carga de trabalho. O preço unitário de vCore por unidade de tempo é menor na camada de computação provisionada do que na camada de computação sem servidor.
- [Unidade de transação de banco de dados (DTU)-com base em modelo de compra](sql-database-service-tiers-dtu.md). O modelo de compra fornece pacotes de computação e armazenamento incorporados com balanceamento de cargas de trabalho comuns.

Modelos de compra diferentes estão disponíveis para diferentes modelos de implantação de banco de dados SQL:

- As opções de implantação [banco de dados individual](sql-database-single-databases-manage.md) e [pool elástico](sql-database-elastic-pool.md) no [Banco de Dados SQL do Azure](sql-database-technical-overview.md) oferecem tanto o [modelo de compra baseado em DTU](sql-database-service-tiers-dtu.md) quanto o [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md).
- O [instância gerenciada](sql-database-managed-instance.md) opção de implantação no banco de dados SQL oferece apenas as [o modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md).
- O [camada de serviço em hiperescala](sql-database-service-tier-hyperscale.md) está disponível para bancos de dados individuais que estão usando o [o modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md).

A seguinte tabela e gráfico de comparam e contrastam os baseado em vCore e modelos de compra baseado em DTU:

|**Modelo de compra**|**Descrição**|**Mais adequado para**|
|---|---|---|
|Modelo baseado em DTU|Esse modelo se baseia em uma medida combinada de computação, armazenamento e recursos de e/s. Tamanhos de computação são expressos em DTUs para bancos de dados únicos e em unidades de transação de banco de dados Elástico (eDTUs) para pools elásticos. Para saber mais sobre DTUs e eDTUs, consulte [o que são DTUs e eDTUs?](sql-database-purchase-models.md#dtu-based-purchasing-model).|Melhor para clientes que desejam opções de recursos simples e pré-configuradas.|
|Modelo baseado em vCore|Esse modelo permite escolher recursos de armazenamento e computação de maneira independente. O modelo de compra baseado em vCore também permite que você use o [Benefício Híbrido do Azure para SQL Server](https://azure.microsoft.com/pricing/hybrid-benefit/) para poupar custos.|Mais adequado para clientes que valorizam flexibilidade, controle e transparência.|
||||  

![comparação do modelo de preços](./media/sql-database-service-tiers/pricing-model.png)

## <a name="compute-costs"></a>Custos de computação

### <a name="provisioned-compute-costs"></a>Custos de computação provisionada

Na camada de computação provisionada, o custo de computação reflete a capacidade de computação total provisionada para o aplicativo.

Na camada de serviço comercialmente crítico, alocamos automaticamente pelo menos três réplicas. Para refletir essa alocação adicional de recursos de computação, o preço no modelo de compra baseado em vCore é aproximadamente 2,7 x mais alto da camada de serviço crítico comercial do que na camada de serviço de uso geral. Da mesma forma, o maior preço de armazenamento por GB na camada de serviço crítico comercial reflete a e/s alta e baixa latência do armazenamento SSD.

O custo de armazenamento de backup é o mesmo para a camada de serviço críticos de negócios e a camada de serviço de uso geral porque ambas as camadas de usam o armazenamento padrão.

### <a name="serverless-compute-costs"></a>Custos de computação sem servidor

Para uma descrição de como a capacidade de computação é definida e os custos são calculados para a camada de computação sem servidor, consulte [banco de dados SQL sem servidor (visualização)](sql-database-serverless.md).

## <a name="storage-costs"></a>Custos de armazenamento

Diferentes tipos de armazenamento são cobrados de formas diferentes. Para o armazenamento de dados, você será cobrado para o armazenamento provisionado baseado no tamanho máximo do banco de dados ou pool, que você selecionar. O custo não muda, a menos que você reduza ou aumente esse máximo. O armazenamento de backup está associado a backups automatizados de sua instância é alocado dinamicamente. Aumentar o período de retenção de backup aumenta o armazenamento de backup que é consumido pela instância.

Por padrão, 7 dias de backups automatizados dos bancos de dados são copiados para uma conta de armazenamento de Blob padrão do armazenamento com redundância geográfica de acesso de leitura (RA-GRS). Esse armazenamento é usado por backups completos semanais, backups diferenciais diários e backups de log de transações, que são copiados a cada 5 minutos. O tamanho dos logs de transação depende da taxa de alteração do banco de dados. Uma quantidade de armazenamento mínimo igual a 100 por cento do tamanho do banco de dados é fornecida sem custo adicional. O consumo adicional de armazenamento de backup é cobrado em GB por mês.

Para obter mais informações sobre preços de armazenamento, confira a página de [preços](https://azure.microsoft.com/pricing/details/sql-database/single/).

## <a name="vcore-based-purchasing-model"></a>Modelo de compra baseado em vCore

Um vCore (núcleo virtual) representa uma CPU lógica e lhe oferece a opção de escolher entre gerações de hardware e as características físicas de hardware (por exemplo, o número de núcleos, memória e o tamanho do armazenamento). O modelo de compra baseado em vCore proporciona flexibilidade, controle, transparência do consumo de recursos individuais, e uma maneira simples de mover os requisitos de carga de trabalho para a nuvem local. Esse modelo permite que você escolha os recursos de computação, memória e armazenamento com base em suas necessidades de carga de trabalho.

O modelo de compra baseado em vCore, você pode escolher entre o [finalidade geral](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) e [comercialmente crítico](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) camadas para de serviço [bancos de dados únicos](sql-database-single-database-scale.md), [ pools Elásticos](sql-database-elastic-pool.md), e [instâncias gerenciadas](sql-database-managed-instance.md). Para bancos de dados individuais, você também pode escolher o [camada de serviço em hiperescala](sql-database-service-tier-hyperscale.md).

O modelo de compra baseado em vCore permite que você escolha os recursos de computação e armazenamento independentemente, combine o desempenho local e otimizar o preço. O modelo de compra baseado em vCore, você paga por:

- Os recursos de computação (camada de serviço + o número de vCores e a quantidade de memória + a geração de hardware).
- O tipo e a quantidade de armazenamento de dados e de log.
- Armazenamento de backup (RA-GRS).

> [!IMPORTANT]
> Recursos de computação, e/s e armazenamento de dados e de log são cobrados por banco de dados ou pool Elástico. Armazenamento de backup é cobrado por cada banco de dados. Para obter mais informações sobre encargos de instância gerenciada, confira [instâncias gerenciadas](sql-database-managed-instance.md).
> **Limitações de região:** Para obter a lista atual de regiões compatíveis, consulte [Produtos disponíveis por região](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Para criar uma instância gerenciada em uma região que não é suportada no momento, [enviar uma solicitação de suporte por meio do portal do Azure](sql-database-managed-instance-resource-limits.md#obtaining-a-larger-quota-for-sql-managed-instance).

Se seu banco de dados individual ou pool Elástico consumir mais de 300 DTUs, convertendo para o modelo de compra baseado em vCore poderá reduzir seus custos. Você pode converter usando a API de escolha ou por meio do portal do Azure, sem tempo de inatividade. No entanto, a conversão não é obrigatório e não é feito automaticamente. Se o modelo de compra baseado em DTU atender aos seus requisitos de desempenho e de negócios, você deverá continuar utilizando-o.

Para converter o modelo de compra baseado em DTU para o modelo de compra baseado em vCore, selecione o tamanho de computação usando as seguintes regras básicas:

- Cada 100 DTUs na camada standard requerem pelo menos 1 vCore na camada de serviço de uso geral.
- Cada 125 DTUs na camada premium requerem pelo menos 1 vCore da camada de serviço crítico comercial.

## <a name="dtu-based-purchasing-model"></a>Modelo de compra baseado em DTU

Uma unidade de transação de banco de dados (DTU) representa uma medida combinada de CPU, memória, leituras e gravações. O modelo de compra baseado em DTU oferece um conjunto de pacotes pré-configurados de recursos de computação e inclui armazenamento para impulsionar diferentes níveis de desempenho do aplicativo. Se você preferir a simplicidade de um pacote pré-configurado e pagamentos fixos por mês, o modelo baseado em DTU pode ser mais adequado às suas necessidades.

O modelo de compra baseado em DTU, você pode escolher entre o basic, standard e camadas de serviço premium para ambos [bancos de dados únicos](sql-database-single-database-scale.md) e [pools Elásticos](sql-database-elastic-pool.md). O modelo de compra baseado em DTU não está disponível para [instâncias gerenciadas](sql-database-managed-instance.md).

### <a name="database-transaction-units-dtus"></a>DTUs (Unidades de Transação de Banco de Dados)

Para um banco de dados em um determinado tamanho dentro de computação uma [camada de serviço](sql-database-single-database-scale.md), Microsoft garante um certo nível de recursos do banco de dados (independente de qualquer outro banco de dados na nuvem do Azure). Essa garantia fornece um nível previsível de desempenho. A quantidade de recursos alocados para um banco de dados é calculada como um número de DTUs e é uma medida combinada de computação, armazenamento e recursos de e/s.

A proporção entre esses recursos originalmente é determinada por uma [carga de trabalho de parâmetro de comparação de processamento de transações online (OLTP)](sql-database-benchmark-overview.md) projetado para ser típico de cargas de trabalho OLTP reais. Quando sua carga de trabalho excede a quantidade de qualquer um desses recursos, a taxa de transferência é limitada, resultando em desempenho mais lento e tempos limite.

Os recursos usados pela sua carga de trabalho não afetam os recursos disponíveis para outros bancos de dados SQL na nuvem do Azure. Da mesma forma, os recursos usados por outras cargas de trabalho não afetam os recursos disponíveis para seu banco de dados SQL.

![caixa delimitadora](./media/sql-database-what-is-a-dtu/bounding-box.png)

DTUs são úteis para entender os recursos relativos que são alocados para bancos de dados SQL do Azure em camadas de serviço e tamanhos de computação diferentes. Por exemplo:

- Dobrar as DTUs aumentando o tamanho de computação de um banco de dados equivale a dobrar o conjunto de recursos disponíveis para esse banco de dados.
- Um serviço camada P11 banco de dados premium com 1.750 DTUs fornece 350x DTU mais potência de computação que um banco de dados de camada de serviço básico com 5 DTUs.  

Para obter uma visão mais profunda o consumo de recursos (DTU) da carga de trabalho, use [insights de desempenho de consulta](sql-database-query-performance.md) para:

- Identifique as principais consultas por contagem de CPU/duração/execução que potencialmente pode ser ajustada para melhorar o desempenho. Por exemplo, uma consulta de e/S intensiva pode se beneficiar [técnicas de otimização de memória](sql-database-in-memory.md) para fazer melhor uso da memória disponível em uma determinada camada de serviço e o tamanho de computação.
- Fazer drill down nos detalhes de uma consulta para exibir seu texto e seu histórico de uso de recursos.
- Acessar as recomendações de ajuste de desempenho que mostram as ações tomadas pelo [Advisor do banco de dados SQL](sql-database-advisor.md).

### <a name="elastic-database-transaction-units-edtus"></a>Unidades de transação de banco de dados Elástico (eDTUs)

Para bancos de dados SQL que estão sempre disponíveis, em vez disso, que fornecem um conjunto dedicado de recursos (DTUs) que pode nem sempre ser necessário, você pode colocar esses bancos de dados em um [pool Elástico](sql-database-elastic-pool.md). Os bancos de dados em um pool Elástico estão em um único servidor de banco de dados SQL e compartilham um pool de recursos.

Os recursos compartilhados em um pool Elástico são medidos por unidades de transação de banco de dados Elástico (eDTUs). Pools Elásticos fornecem uma solução simples e econômica para gerenciar as metas de desempenho para vários bancos de dados que têm padrões de uso muito variáveis e imprevisíveis. Um pool Elástico garante que todos os recursos não podem ser consumidos por um banco de dados no pool, garantindo que cada banco de dados no pool sempre tem uma quantidade mínima de recursos necessários disponíveis.

Um pool é fornecido com um número definido de eDTUs por um preço definido. No pool Elástico, bancos de dados individuais podem dimensionamento automático dentro dos limites configurados. Um banco de dados sob uma carga mais pesada irá consumir mais eDTUs para atender à demanda. Bancos de dados sob cargas mais leves consumirá menos eDTUs. Bancos de dados sem carga não consumirão eDTUs. Porque os recursos são provisionados para o pool inteiro, em vez de por banco de dados, pools Elásticos simplificam suas tarefas de gerenciamento e fornecem um orçamento previsível para o pool.

Você pode adicionar eDTUs adicionais a um pool existente sem tempo de inatividade do banco de dados e sem nenhum impacto nos bancos de dados no pool. Da mesma forma, se você não precisa mais eDTUs extras, removê-los de um pool existente a qualquer momento. Você também pode adicionar bancos de dados ou subtrair bancos de dados de um pool a qualquer momento. Para reservar eDTUs para outros bancos de dados, limite o número de eDTUs que um banco de dados pode usar sob uma carga pesada. Se um banco de dados consistentemente Subutiliza os recursos, mova-a para fora do pool e configurá-lo como um único banco de dados com uma quantidade previsível de recursos necessários.

### <a name="determine-the-number-of-dtus-needed-by-a-workload"></a>Determinar o número de DTUs necessárias para uma carga de trabalho

Se você quiser migrar um local ou carga de trabalho de máquina virtual do SQL Server para o banco de dados SQL, use o [Calculadora de DTU](https://dtucalculator.azurewebsites.net/) para aproximar o número de DTUs necessários. Para uma carga de trabalho existente do banco de dados SQL, use [insights de desempenho de consulta](sql-database-query-performance.md) para compreender o consumo de recursos de banco de dados (DTUs) e aprofunde-se para otimizar sua carga de trabalho. O [resource_stats sys](https://msdn.microsoft.com/library/dn800981.aspx) exibição de gerenciamento dinâmico (DMV) permite que você exiba o consumo de recursos de última hora. O [sys. resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) exibição de catálogo mostra o consumo de recursos para os últimos 14 dias, mas com uma fidelidade menor de médias de cinco minutos.

### <a name="workloads-that-benefit-from-an-elastic-pool-of-resources"></a>Cargas de trabalho que se beneficiam de um pool elástico de recursos

Pools são bem adequados para bancos de dados com uma média de baixa utilização de recursos e picos de utilização relativamente pouco frequentes. Banco de dados SQL avalia o uso do histórico de recursos de bancos de dados em um servidor de banco de dados SQL existente e recomenda a configuração de pool apropriada no portal do Azure automaticamente. Para obter mais informações, consulte [quando você deve considerar um pool Elástico do banco de dados SQL?](sql-database-elastic-pool.md).

## <a name="frequently-asked-questions-faqs"></a>Perguntas frequentes (FAQs)

### <a name="do-i-need-to-take-my-application-offline-to-convert-from-a-dtu-based-service-tier-to-a-vcore-based-service-tier"></a>É necessário colocar meu aplicativo offline para converter de uma camada de serviço baseado em DTU para uma camada de serviço baseado em vCore?

Não. Você não precisa colocar o aplicativo offline. As novas camadas de serviço oferecem um método de conversão online simples que é semelhante ao processo existente de atualização de bancos de dados do padrão para a camada de serviço premium e vice-versa. Você pode iniciar essa conversão, usando o portal do Azure, PowerShell, a CLI do Azure, T-SQL ou a API REST. Consulte [Gerenciar bancos de dados individuais](sql-database-single-database-scale.md) e [Gerenciar pools elásticos](sql-database-elastic-pool.md).

### <a name="can-i-convert-a-database-from-a-service-tier-in-the-vcore-based-purchasing-model-to-a-service-tier-in-the-dtu-based-purchasing-model"></a>É possível converter um banco de dados de uma camada de serviço no modelo de compra baseado em vCore para uma camada de serviço no modelo de compra baseado em DTU?

Sim, você pode converter facilmente seu banco de dados em qualquer objetivo de desempenho com suporte usando o portal do Azure, PowerShell, a CLI do Azure, T-SQL ou a API REST. Consulte [Gerenciar bancos de dados individuais](sql-database-single-database-scale.md) e [Gerenciar pools elásticos](sql-database-elastic-pool.md).

## <a name="next-steps"></a>Próximas etapas

- Para obter mais informações sobre o modelo de compra baseado em vCore, consulte [modelo de compra baseado em vCore](sql-database-service-tiers-vcore.md).
- Para obter mais informações sobre o modelo de compra baseado em DTU, consulte [modelo de compra baseado em DTU](sql-database-service-tiers-dtu.md).
