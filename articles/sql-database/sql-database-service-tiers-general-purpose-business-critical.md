---
title: Banco de dados SQL do Azure - uso geral e negócios críticos | Microsoft Docs
description: O artigo discute o uso geral e as camadas de serviço críticos de negócios no modelo de compra baseado em vCore.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: sashan, moslake, carlrab
manager: craigg
ms.date: 02/23/2019
ms.openlocfilehash: e5426bb7c8eba9d58dbf0472360c6ce0b19c9bc4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66431334"
---
# <a name="azure-sql-database-service-tiers"></a>Camadas de serviço do Banco de Dados SQL do Azure

Banco de dados SQL do Azure baseia-se na arquitetura de mecanismo de banco de dados do SQL Server que está ajustada para o ambiente de nuvem garantir a disponibilidade de 99,99%, mesmo se houver uma falha de infraestrutura. Três camadas de serviço são usadas no banco de dados SQL, cada um com um modelo de arquitetura diferente. Essas camadas de serviço são:

- [Uso geral](sql-database-service-tier-general-purpose.md), que é projetado para cargas de trabalho mais genéricas.
- [Comercialmente crítico](sql-database-service-tier-business-critical.md), que é projetado para cargas de trabalho de baixa latência com uma réplica legível.
- [Hiperescala](sql-database-service-tier-hyperscale.md), que é projetado para grandes bancos de dados (até 100 TB) com várias réplicas legíveis.

Este artigo aborda considerações de armazenamento e backup para o uso geral e as camadas de serviço críticos de negócios no modelo de compra baseado em vCore.

> [!NOTE]
> Para obter informações sobre a camada de serviço em hiperescala no modelo de compra baseado em vCore, consulte [camada de serviço em hiperescala](sql-database-service-tier-hyperscale.md). Para obter uma comparação do modelo de compra baseado em vCore com o modelo de compra baseado em DTU, consulte [Modelos e recursos de compra do Banco de Dados SQL do Azure](sql-database-purchase-models.md).

## <a name="data-and-log-storage"></a>Armazenamento de dados e de log

Os seguintes fatores afetam a quantidade de armazenamento usado para arquivos de log e de dados:

- O armazenamento alocado é usado por arquivos de dados (MDF) e arquivos de log (LDF).
- Cada banco de dados único computação tamanho dá suporte um tamanho máximo do banco de dados, com um tamanho padrão máximo de 32 GB.
- Quando você configura o tamanho do banco de dados único necessário (o tamanho do arquivo MDF), 30 por cento a mais de armazenamento adicional é adicionado automaticamente para dar suporte a arquivos LDF.
- O tamanho do armazenamento para uma instância gerenciada do banco de dados SQL deve ser especificado em múltiplos de 32 GB.
- Você pode selecionar qualquer tamanho de banco de dados individual entre 10 GB e o máximo com suporte.
  - Para o armazenamento nas camadas de serviço standard ou de uso geral, aumente ou diminua o tamanho em incrementos de 10 GB.
  - Para o armazenamento no premium ou camadas de serviço críticos de negócios, aumentar ou diminuir o tamanho em incrementos de 250 GB.
- Na camada de serviço de uso geral, `tempdb` usa um SSD anexado e esse custo de armazenamento está incluído no preço do vCore.
- Na camada de serviço críticos de negócios, `tempdb` compartilha o SSD anexado com os arquivos MDF e LDF e o `tempdb` custo de armazenamento está incluído no preço do vCore.

> [!IMPORTANT]
> Você é cobrado para o armazenamento total alocado para os arquivos MDF e LDF.

Para monitorar o tamanho total atual de seus arquivos MDF e LDF, use [sp_spaceused](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-spaceused-transact-sql). Para monitorar o tamanho atual dos arquivos MDF e LDF individuais, use [sys.database_files](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-database-files-transact-sql).

> [!IMPORTANT]
> Em algumas circunstâncias, talvez seja necessário reduzir um banco de dados para recuperar o espaço não utilizado. Para obter mais informações, consulte [gerenciar o espaço de arquivo no banco de dados SQL do Azure](sql-database-file-space-management.md).

## <a name="backups-and-storage"></a>Backups e armazenamento

O armazenamento para backups de banco de dados é alocado para dar suporte a restauração point-in-time (PITR) e [retenção de longo prazo (LTR)](sql-database-long-term-retention.md) recursos do banco de dados SQL. Esse armazenamento é alocado separadamente para cada banco de dados e faturado como duas cobranças separadas por banco de dados.

- **PITR**: Backups de banco de dados individuais são copiados para [armazenamento do acesso de leitura com redundância geográfica (RA-GRS)](../storage/common/storage-designing-ha-apps-with-ragrs.md) automaticamente. O tamanho de armazenamento aumenta dinamicamente conforme novos backups são criados. O armazenamento é usado por backups completos semanais, backups diferenciais diários e backups de log de transações, que são copiados a cada 5 minutos. O consumo de armazenamento depende da taxa de alteração de banco de dados e o período de retenção para backups. É possível configurar um período de retenção separado para cada banco de dados entre 7 e 35 dias. Uma quantidade de armazenamento mínimo igual a 100 por cento (1 x) do tamanho do banco de dados é fornecida sem custo adicional. Para a maioria dos bancos de dados, esse valor é suficiente para armazenar 7 dias de backups.
- **LTR**: Banco de dados SQL oferece a opção de configurar a retenção de longo prazo de backups completos por até 10 anos. Se você configurar uma política LTR, esses backups são armazenados no armazenamento RA-GRS automaticamente, mas você pode controlar a frequência com que os backups são copiados. Para atender aos requisitos de conformidade diferente, você pode selecionar diferentes períodos de retenção para backups semanais, mensais e anuais. A escolha da configuração determina a quantidade de armazenamento será usada para backups LTR. Para estimar o custo do armazenamento LTR, você pode usar a Calculadora de preços de LTR. Para obter mais informações, consulte [retenção de longo prazo do banco de dados SQL](sql-database-long-term-retention.md).

## <a name="next-steps"></a>Próximas etapas

- Para obter detalhes sobre o específico de computação tamanhos e os tamanhos de armazenamento disponíveis para um banco de dados de uso geral e camadas de serviço críticos de negócios, veja [limites de recurso de baseado em vCore do banco de dados SQL para bancos de dados únicos](sql-database-vcore-resource-limits-single-databases.md).
- Para obter detalhes sobre o específico de computação tamanhos e os tamanhos de armazenamento disponíveis para pools Elásticos do uso geral e camadas de serviço críticos de negócios, veja [limites de recurso baseado em vCore do banco de dados SQL para pools Elásticos](sql-database-vcore-resource-limits-elastic-pools.md).
