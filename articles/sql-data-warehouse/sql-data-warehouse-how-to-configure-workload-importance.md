---
title: Configure a importância de carga de trabalho no Azure SQL Data Warehouse | Microsoft Docs
description: Saiba como definir a prioridade de nível de solicitação.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.subservice: workload management
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 10a1fe2bff43b6799f47fc0245802ce578ef8f8f
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165524"
---
# <a name="configure-workload-importance-in-azure-sql-data-warehouse"></a>Configure a importância de carga de trabalho no Azure SQL Data Warehouse

Defina a importância no SQL Data Warehouse permite que você influenciam o agendamento de consultas. Consultas com importância mais alta serão agendadas para ser executado antes de consultas com prioridade mais baixa. Para atribuir importância às consultas, você precisará criar um classificador de carga de trabalho.

## <a name="create-a-workload-classifier-with-importance"></a>Criar um classificador de carga de trabalho com importância

Muitas vezes em um cenário de depósito de dados você tiver usuários que precisam de suas consultas sejam executadas rapidamente.  O usuário poderia ser executivos da empresa que precisam para executar relatórios ou o usuário pode ser um analista executando uma consulta ad hoc. Você cria um classificador de carga de trabalho para atribuir importância para uma consulta.  Os exemplos abaixo usam o novo [criar um classificador de carga de trabalho](/sql/t-sql/statements/create-workload-classifier-transact-sql?view=azure-sqldw-latest) sintaxe para criar dois classificadores.  Nome do membro pode ser um único usuário ou um grupo. Classificações de usuário individuais têm precedência sobre as classificações de função. Para localizar os usuários existentes do depósito de dados, execute:

```sql
Select name from sys.sysusers
```

Para criar um classificador de carga de trabalho, para um usuário com importância mais alta execute:

```sql
CREATE WORKLOAD CLASSIFIER ExecReportsClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
                   ,MEMBERNAME        = 'name'  
                   ,IMPORTANCE        =  above_normal);  

```

Para criar um classificador de carga de trabalho para um usuário executar consultas ad hoc com menos importância executar:  

```sql
CREATE WORKLOAD CLASSIFIER AdhocClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
                   ,MEMBERNAME        = 'name'  
                   ,IMPORTANCE        =  below_normal);  
```

## <a name="next-steps"></a>Próximas etapas
- Para obter mais informações sobre o gerenciamento de carga de trabalho, consulte [classificação da carga de trabalho](sql-data-warehouse-workload-classification.md)
- Para obter mais informações sobre a importância, consulte [importância da carga de trabalho](sql-data-warehouse-workload-importance.md)

> [!div class="nextstepaction"]
> [Vá para gerenciar e monitorar a importância da carga de trabalho](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md)
