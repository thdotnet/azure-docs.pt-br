---
title: Gerenciar Microsoft Azure Cosmos DB usando a CLI do Azure
description: Use a CLI do Azure para gerenciar sua conta, banco de dados e contêineres do Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/05/2019
ms.author: mjbrown
ms.openlocfilehash: f9d8bf9161343e4b36a3c16209873962b69d8af5
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69615216"
---
# <a name="manage-azure-cosmos-resources-using-azure-cli"></a>Gerenciar Microsoft Azure Cosmos usando a CLI do Azure

O guia a seguir descreve os comandos comuns para automatizar o gerenciamento de suas contas de Azure Cosmos DB, bancos de dados e contêineres usando CLI do Azure. As páginas de referência de todos os comandos do Azure Cosmos DB CLI estão disponíveis na [Referência de CLI do Azure](https://docs.microsoft.com/cli/azure/cosmosdb). Você também pode encontrar mais exemplos em [amostras de CLI do Azure para o Azure Cosmos DB](cli-samples.md), incluindo como criar e gerenciar contas, bancos de dados e contêineres do Cosmos DB para MongoDB, Gremlin, Cassandra e API de Tabela.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se você optar por instalar e usar a CLI localmente, este tópico exigirá que você esteja executando a CLI do Azure versão 2.0 ou posterior. Execute `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, consulte [Instalar a CLI do Azure](/cli/azure/install-azure-cli).

## <a name="create-an-azure-cosmos-db-account"></a>Criar uma conta do Azure Cosmos DB

Para criar uma conta de Azure Cosmos DB com a API do SQL, consistência de sessão nas regiões leste dos EUA e oeste dos EUA, execute o seguinte comando:

```azurecli-interactive
az cosmosdb create \
   --name mycosmosdbaccount # must be lowercase and < 31 characters \
   --resource-group myResourceGroup \
   --kind GlobalDocumentDB \
   --default-consistency-level Session \
   --locations regionName=EastUS failoverPriority=0 isZoneRedundant=False \
   --locations regionName=WestUS failoverPriority=1 isZoneRedundant=False \
   --enable-multiple-write-locations false
```

> [!IMPORTANT]
> O nome da conta do Azure Cosmos deve estar em minúsculas.

## <a name="create-a-database"></a>Criar um banco de dados

Para criar um banco de dados Cosmos, execute o seguinte comando:

```azurecli-interactive
az cosmosdb database create \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup
```

## <a name="create-a-container"></a>Criar um contêiner

Para criar um contêiner cosmos com RU/s de 400 e uma chave de partição, execute o seguinte comando:

```azurecli-interactive
# Create a container
az cosmosdb collection create \
   --collection-name myContainer \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup \
   --partition-key-path /myPartitionKey \
   --throughput 400
```

## <a name="change-the-throughput-of-a-container"></a>Alterar o rendimento de um contêiner

Para alterar a taxa de transferência de um contêiner Cosmos para 1000 RU/s, execute o seguinte comando:

```azurecli-interactive
# Update container throughput
az cosmosdb collection update \
   --collection-name myContainer \
   --name mycosmosdbaccount \
   --db-name myDatabase \
   --resource-group myResourceGroup \
   --throughput 1000
```

## <a name="list-account-keys"></a>Listar chaves de conta

Para obter as chaves para sua conta do cosmos, execute o seguinte comando:

```azurecli-interactive
# List account keys
az cosmosdb keys list \
   --name  mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="list-connection-strings"></a>Cadeias de caracteres de conexão de lista

Para obter as cadeias de conexão para sua conta do cosmos, execute o seguinte comando:

```azurecli-interactive
# List connection strings
az cosmosdb list-connection-strings \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup
```

## <a name="regenerate-account-key"></a>Regenerar a chave de conta

Para regenerar uma nova chave primária para sua conta do cosmos, execute o seguinte comando:

```azurecli-interactive
# Regenerate account key
az cosmosdb regenerate-key \
   --name mycosmosdbaccount \
   --resource-group myResourceGroup \
   --key-kind primary
```

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre a CLI do Azure, consulte:

- [Instalar a CLI do Azure.](/cli/azure/install-azure-cli)
- [Referência à CLI do Azure](https://docs.microsoft.com/cli/azure/cosmosdb)
- [Amostras adicionais do CLI do Azure para o Azure Cosmos DB](cli-samples.md)
