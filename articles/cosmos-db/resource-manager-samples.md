---
title: Modelos do Azure Resource Manager para o Azure Cosmos DB
description: Use modelos do Azure Resource Manager para criar e configurar o Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: 4a32b0497d2457a740e9c082f990bb9112208bfd
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969171"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Modelos do Azure Resource Manager para o Azure Cosmos DB

As tabelas a seguir incluem links para modelos do Azure Resource Manager para o Azure Cosmos DB:

## <a name="sql-core-api"></a>API de SQL (Core)

|**Modelo**|**Descrição**|
|---| ---|
|[Criar uma conta do Cosmos do Azure, banco de dados, contêiner](manage-sql-with-resource-manager.md#create-resource) | Este modelo cria uma conta de API do SQL (núcleos) em duas regiões com vários mestres habilitado. A conta de Cosmos do Azure terá dois contêineres que compartilham a taxa de transferência de nível de banco de dados. |
|[Atualizar a taxa de transferência (RU/s) para um banco de dados](manage-sql-with-resource-manager.md#database-ru-update) | Este modelo atualiza a taxa de transferência para um banco de dados em uma conta de API do SQL (núcleos). |
|[Atualizar a taxa de transferência (RU/s) para um contêiner](manage-sql-with-resource-manager.md#container-ru-update) | Este modelo atualiza a taxa de transferência para um contêiner em uma conta de API do SQL (núcleos). |

## <a name="mongodb-api"></a>API do MongoDB

|**Modelo**|**Descrição**|
|---| ---|
|[Criar uma conta do Cosmos do Azure, o banco de dados, a coleção](manage-mongodb-with-resource-manager.md#create-resource) | Este modelo cria uma conta usando a API do Azure Cosmos DB para MongoDB em duas regiões com vários mestres habilitado. A conta de Cosmos do Azure terá dois contêineres que compartilham a taxa de transferência de nível de banco de dados. |
|[Atualizar a taxa de transferência (RU/s) para um banco de dados](manage-mongodb-with-resource-manager.md#database-ru-update) | Este modelo atualiza a taxa de transferência para um banco de dados em uma conta de API do MongoDB. |
|[Taxa de transferência (RU/s) para uma coleção de atualização](manage-mongodb-with-resource-manager.md#collection-ru-update) | Este modelo atualiza a taxa de transferência para um contêiner em uma conta de API do MongoDB. |

## <a name="cassandra-api"></a>API Cassandra

|**Modelo**|**Descrição**|
|---| ---|
|[Criar uma conta do Azure Cosmos, keyspace, tabela](manage-cassandra-with-resource-manager.md#create-resource) | Este modelo cria uma conta de API do Cassandra em duas regiões com vários mestres habilitado. A conta de Cosmos do Azure terá duas tabelas que compartilham a taxa de transferência de nível de keyspace. |
|[Atualizar a taxa de transferência (RU/s) para um keyspace](manage-cassandra-with-resource-manager.md#keyspace-ru-update) | Este modelo atualiza a taxa de transferência para um keyspace em uma conta de API do Cassandra. |
|[Taxa de transferência (RU/s) para uma tabela de atualização](manage-cassandra-with-resource-manager.md#table-ru-update) | Este modelo atualiza a taxa de transferência para uma tabela em uma conta de API do Cassandra. |

## <a name="gremlin-api"></a>API do Gremlin

|**Modelo**|**Descrição**|
|---| ---|
|[Criar uma conta do Cosmos do Azure, banco de dados, gráfico](manage-gremlin-with-resource-manager.md#create-resource) | Este modelo cria uma conta da API do Gremlin em duas regiões com vários mestres habilitado. A conta de Cosmos do Azure terá dois gráficos que compartilham a taxa de transferência de nível de banco de dados. |
|[Atualizar a taxa de transferência (RU/s) para um banco de dados](manage-gremlin-with-resource-manager.md#database-ru-update) | Este modelo atualiza a taxa de transferência para um banco de dados em uma conta de API do Gremlin. |
|[Atualizar a taxa de transferência (RU/s) para um gráfico](manage-gremlin-with-resource-manager.md#graph-ru-update) | Este modelo atualiza a taxa de transferência para um gráfico em uma conta de API do Gremlin. |

## <a name="table-api"></a>API de Tabela

|**Modelo**|**Descrição**|
|---| ---|
|[Criar uma conta do Cosmos do Azure, tabela](manage-table-with-resource-manager.md#create-resource) | Este modelo cria uma conta da API de tabela em duas regiões com vários mestres habilitado. A conta de Cosmos do Azure terá uma única tabela. |
|[Taxa de transferência (RU/s) para uma tabela de atualização](manage-table-with-resource-manager.md#table-ru-update) | Este modelo atualiza a taxa de transferência para uma tabela em uma conta de API de tabela. |

> [!TIP]
> Para habilitar a taxa de transferência compartilhada ao usar a API de tabela, habilite a taxa de transferência no nível da conta no Portal do Azure.

Ver [referência ARM do Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions) página para obter a documentação de referência.