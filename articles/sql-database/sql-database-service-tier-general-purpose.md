---
title: Camada de serviço de uso geral – banco de dados SQL do Azure | Microsoft Docs
description: Saiba mais sobre a camada de uso geral do banco de dados SQL
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop-msft
ms.reviewer: sstein
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: b972ea985a09457d8b6a17a292e18754761f5a6e
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479184"
---
# <a name="general-purpose-service-tier---azure-sql-database"></a>Camada de serviço de uso geral – Banco de Dados SQL do Azure

> [!NOTE]
> A camada de serviço de uso geral no modelo de compra baseado em vCore é chamada da camada de serviço padrão no modelo de compra baseado em DTU. Para obter uma comparação do modelo de compra baseado em vCore com o modelo de compra baseado em DTU, consulte [Modelos e recursos de compra do Banco de Dados SQL do Azure](sql-database-purchase-models.md).

O Banco de Dados SQL do Azure é baseado na arquitetura de mecanismo de banco de dados do SQL Server, que é adaptada ao ambiente de nuvem para garantir disponibilidade de 99,99%, até mesmo no caso de falhas de infraestrutura. Três camadas de serviço são usadas no Banco de Dados SQL do Azure, cada uma com diferentes modelos de arquitetura. Essas camadas de serviço são:

- Propósito geral
- Comercialmente crítico
- Hiperescala

O modelo de arquitetura para a camada de serviço de uso geral baseia-se em uma separação de computação e armazenamento. Esse modelo de arquitetura baseia-se na alta disponibilidade e na confiabilidade do Armazenamento de Blobs do Azure, que replica os arquivos de banco de dados de forma transparente e garante que não haja perda de dados se ocorrer uma falha na infraestrutura subjacente.

A figura a seguir mostra quatro nós no modelo de arquitetura padrão com as camadas de computação e armazenamento separadas.

![Separação de computação e armazenamento](media/sql-database-managed-instance/general-purpose-service-tier.png)

No modelo de arquitetura para a camada de serviço de uso geral, há duas camadas:

- Uma camada de computação sem estado que está executando o processo `sqlserver.exe` e contém apenas dados temporários e armazenados em cache (por exemplo - cache de plano, conjunto de buffers, conjunto de armazenamentos de colunas). Este nó do SQL Server sem estado é operado pelo Microsoft Azure Service Fabric, que inicializa o processo, controla a integridade do nó e executa o failover para outro local, se necessário.
- Uma camada de dados com estado, com arquivos de banco de dados (.mdf/.ldf) que são armazenados no Armazenamento de Blobs do Azure. O Armazenamento de Blobs do Azure garante que não haja perda de dados de nenhum registro colocado em qualquer arquivo de banco de dados. O Armazenamento do Azure tem disponibilidade/redundância de dados interna, que garante que cada registro no arquivo de log ou página no arquivo de dados será preservado mesmo se o processo do SQL Server falhar.

Sempre que o mecanismo de banco de dados ou sistema operacional é atualizado, alguma parte da infraestrutura subjacente falhar ou se for detectado algum problema crítico no processo do SQL Server, o Azure Service Fabric moverá o processo do SQL Server sem estado para outro nó de computação sem estado. Há um conjunto de nós sobressalentes que fica esperando a execução do novo serviço de computação se ocorrer um failover do nó primário a fim de minimizar o tempo de failover. Os dados na camada de armazenamento do Azure não são afetados e arquivos de dados/log são anexados ao processo do SQL Server recém-inicializado. Esse processo garante a disponibilidade de 99,99%, mas pode ter alguns impactos de desempenho na carga de trabalho pesada que está sendo executada devido ao tempo de transição e ao fato de o novo nó do SQL Server ser iniciado com cache frio.

## <a name="when-to-choose-this-service-tier"></a>Quando escolher essa camada de serviço

A camada de Uso Geral é uma camada de serviço padrão no Banco de Dados SQL do Azure que foi projetada para a maioria das cargas de trabalho genéricas. Se você precisar de um mecanismo de banco de dados totalmente gerenciado com SLA de 99,99% com latência de armazenamento entre 5 e 10 ms que correspondem à IaaS do SQL Azure na maioria dos casos, a camada de uso geral é a opção para você.

## <a name="next-steps"></a>Próximas etapas

- Encontrar recursos características (número de memória de núcleos, e/s,) da camada de finalidade/Standard geral nos [instância gerenciada](sql-database-managed-instance-resource-limits.md#service-tier-characteristics)único banco de dados no [modelo vCore](sql-database-vcore-resource-limits-single-databases.md#general-purpose-service-tier-storage-sizes-and-compute-sizes) ou [modelo DTU](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes), ou Pool Elástico no [modelo vCore](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose-service-tier-storage-sizes-and-compute-sizes) e [modelo DTU](sql-database-dtu-resource-limits-elastic-pools.md#standard-elastic-pool-limits).
- Saiba mais sobre camadas [Comercialmente Críticas](sql-database-service-tier-business-critical.md) e de [Hiperescala](sql-database-service-tier-hyperscale.md).
- Saiba mais sobre o [Service Fabric](../service-fabric/service-fabric-overview.md).
- Para obter mais opções de alta disponibilidade e recuperação de desastres, consulte [Continuidade de Negócios](sql-database-business-continuity.md).
