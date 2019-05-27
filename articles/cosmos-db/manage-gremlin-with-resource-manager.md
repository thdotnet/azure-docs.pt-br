---
title: Modelos do Azure Resource Manager para a API do Gremlin do Azure Cosmos DB
description: Use modelos do Azure Resource Manager para criar e configurar a API do Gremlin do Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: 9f62399e3a1ef2a4ceaa8bdf64196bdb634fb4b6
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968893"
---
# <a name="manage-azure-cosmos-db-gremlin-api-resources-using-azure-resource-manager-templates"></a>Gerenciar recursos de API do Gremlin do Azure Cosmos DB usando modelos do Azure Resource Manager

## Criar API do Azure Cosmos DB para a conta do MongoDB, banco de dados e coleção <a id="create-resource"></a>

Crie recursos do Azure Cosmos DB usando um modelo do Azure Resource Manager. Este modelo criará uma conta do Azure Cosmos para a API do Gremlin com dois gráficos que compartilham uma taxa de transferência de 400 RU/s no nível do banco de dados. Copie o modelo e implantar, conforme mostrado abaixo ou visite [Galeria de início rápido do Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-gremlin/) e implantar do portal do Azure. Você também pode baixar o modelo em seu computador local ou criar um novo modelo e especifique o caminho local com o `--template-file` parâmetro.

[!code-json[create-cosmos-gremlin](~/quickstart-templates/101-cosmosdb-gremlin/azuredeploy.json)]

## <a name="deploy-with-azure-cli"></a>Implantar com a CLI do Azure

Para implantar o modelo do Resource Manager usando a CLI do Azure, **cópia** script e selecione **Experimente** para abrir o Azure Cloud shell. Para colar o script, o shell e, em seguida, selecione **colar**:

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first graph name: ' graph1Name
read -p 'Enter the second graph name: ' graph2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-gremlin/azuredeploy.json \
   --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion databaseName=$databaseName \
   graph1Name=$graph1Name graph2Name=$graph2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

O `az cosmosdb show` comando mostra a conta recém-criada do Azure Cosmos depois que ele foi provisionado. Se você optar por usar uma versão do CLI do Azure instalada localmente em vez de usar CloudShell, consulte [Interface de linha de comando do Azure (CLI)](/cli/azure/) artigo.

## Atualizar a taxa de transferência (RU/s) em um banco de dados <a id="database-ru-update"></a>

O modelo a seguir atualizará a taxa de transferência de um banco de dados. Copie o modelo e implantar, conforme mostrado abaixo ou visite [Galeria de início rápido do Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-gremlin-database-ru-update/) e implantar do portal do Azure. Você também pode baixar o modelo em seu computador local ou criar um novo modelo e especifique o caminho local com o `--template-file` parâmetro.

[!code-json[cosmosdb-gremlin-database-ru-update](~/quickstart-templates/101-cosmosdb-gremlin-database-ru-update/azuredeploy.json)]

### <a name="deploy-database-template-via-azure-cli"></a>Implantar o modelo de banco de dados por meio da CLI do Azure

Para implantar o modelo do Resource Manager usando a CLI do Azure, selecione **Experimente** para abrir o Azure Cloud shell. Para colar o script, o shell e, em seguida, selecione **colar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-gremlin-database-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName throughput=$throughput
```

## Atualizar a taxa de transferência (RU/s) em um gráfico <a id="graph-ru-update"></a>

O modelo a seguir atualizará a taxa de transferência de um gráfico. Copie o modelo e implantar, conforme mostrado abaixo ou visite [Galeria de início rápido do Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-gremlin-graph-ru-update/) e implantar do portal do Azure. Você também pode baixar o modelo em seu computador local ou criar um novo modelo e especifique o caminho local com o `--template-file` parâmetro.

[!code-json[cosmosdb-gremlin-graph-ru-update](~/quickstart-templates/101-cosmosdb-gremlin-graph-ru-update/azuredeploy.json)]

### <a name="deploy-graph-template-via-azure-cli"></a>Implantar o modelo de gráfico por meio da CLI do Azure

Para implantar o modelo do Resource Manager usando a CLI do Azure, selecione **Experimente** para abrir o Azure Cloud shell. Para colar o script, o shell e, em seguida, selecione **colar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the graph name: ' graphName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-gremlin-graph-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName graphName=$graphName throughput=$throughput
```

## <a name="next-steps"></a>Próximas etapas

Estes são alguns recursos adicionais:

- [Documentação do Azure Resource Manager](/azure/azure-resource-manager/)
- [Esquema do provedor de recursos do Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
- [Modelos de início rápido do BD Cosmos do Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Solucionar problemas de erros comuns de implantação do Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)