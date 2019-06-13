---
title: O que é o SQL Data Warehouse do Azure? | Microsoft Docs
description: Banco de dados distribuído no nível corporativo com capacidade de processar volumes de petabytes de dados relacionais e não relacionais. É o primeiro data warehouse de nuvem do setor que pode ser expandido, reduzido e pausado questão de segundos.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: overview
ms.subservice: design
ms.date: 05/30/2019
ms.author: martinle
ms.reviewer: igorstan
mscustom: sqlfreshmay19
ms.openlocfilehash: a9126e9023091dd8c3df71f2aa2558a01227a8be
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428023"
---
# <a name="what-is-azure-sql-data-warehouse"></a>O que é o SQL Data Warehouse do Azure?

O SQL Data Warehouse é um EDW (Enterprise Data Warehouse) que usa o MPP (Massively Parallel Processing) para executar rapidamente consultas complexas em petabytes de dados. Use o SQL Data Warehouse como um componente fundamental de uma solução de big data. Importe big data para o SQL Data Warehouse com consultas T-SQL simples do [PolyBase](/sql/relational-databases/polybase/polybase-guide?view=sql-server-2017&viewFallbackFrom=azure-sqldw-latest) e, em seguida, use o poder do MPP para executar a análise de alto desempenho. À medida que você integrar e analisar, o data warehouse se tornará a única versão da verdade com que sua empresa poderá contar para obter informações.  

## <a name="key-component-of-big-data-solution"></a>Componente fundamental da solução de big data

SQL Data Warehouse é um componente fundamental de uma solução de ponta a ponta de big data na Nuvem.

![Solução de data warehouse](media/sql-data-warehouse-overview-what-is/data-warehouse-solution.png) 

Em uma solução de dados de nuvem, os dados são incluídos nos repositórios de big data de uma variedade de fontes. Uma vez em um repositório de big data, os algoritmos do Hadoop, do Spark e de machine learning preparam e treinam os dados. Quando os dados estiverem prontos para análises complexas, o SQL Data Warehouse usará o PolyBase para consultar os armazenamentos de big data. O PolyBase usa consultas T-SQL padrão para trazer os dados para o SQL Data Warehouse.
 
O SQL Data Warehouse armazena dados em tabelas relacionais com armazenamento colunar. Esse formato reduz consideravelmente os custos de armazenamento de dados e melhora o desempenho da consulta. Depois que os dados forem armazenados no SQL Data Warehouse, você poderá executar análises em grande escala. Em comparação aos tradicionais sistemas de banco de dados, as consultas de análise são concluídas em segundos em vez de minutos, ou em horas em vez de dias. 

Os resultados da análise podem ir para aplicativos ou bancos de dados em todo o mundo. Os analistas de negócios podem então obter informações para tomar decisões de negócios bem informadas.

## <a name="next-steps"></a>Próximas etapas

- Explore [Arquitetura do SQL Data Warehouse do Azure](/azure/sql-data-warehouse/massively-parallel-processing-mpp-architecture)
- [Criar rapidamente um SQL Data Warehouse][create a SQL Data Warehouse]
- [Carregar dados de amostra][load sample data].
- Explore os [Vídeos](/azure/sql-data-warehouse/sql-data-warehouse-videos)

Ou, dê uma olhada em alguns desses outros Recursos do SQL Data Warehouse.  
* Pesquisar em [Blogs]
* Enviar [Solicitações de recursos]
* Pesquisar em [Blogs da Equipe Consultoria para Clientes]
* [Criar um tíquete de suporte]
* Pesquisar no [Fórum do MSDN]
* Pesquisar no [Fórum Stack Overflow]


<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Criar um tíquete de suporte]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Blogs da Equipe Consultoria para Clientes]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Solicitações de recursos]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Fórum do MSDN]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Fórum Stack Overflow]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videos]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[Service Level Agreements]: https://azure.microsoft.com/support/legal/sla/
