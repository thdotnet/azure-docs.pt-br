---
title: Modelos do Azure Resource Manager para a API para MongoDB do Azure Cosmos DB
description: Use modelos do Azure Resource Manager para criar e configurar a API do Azure Cosmos DB para MongoDB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mjbrown
ms.openlocfilehash: 99f1e41107c277c8b3f1b21f81952d5d5cadaa29
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65968877"
---
# <a name="manage-azure-cosmos-db-mongodb-api-resources-using-azure-resource-manager-templates"></a>Gerenciar recursos de API do MongoDB do Azure Cosmos DB usando modelos do Azure Resource Manager

## Criar API do Azure Cosmos DB para a conta do MongoDB, banco de dados e coleção <a id="create-resource"></a>

Crie recursos do Azure Cosmos DB usando um modelo do Azure Resource Manager. Este modelo criará uma conta de Cosmos do Azure para API do MongoDB com duas coleções que compartilham uma taxa de transferência de 400 RU/s no nível do banco de dados. Copie o modelo e implantar, conforme mostrado abaixo ou visite [Galeria de início rápido do Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb/) e implantar do portal do Azure. Você também pode baixar o modelo em seu computador local ou criar um novo modelo e especifique o caminho local com o `--template-file` parâmetro.

[!code-json[create-cosmos-mongo](~/quickstart-templates/101-cosmosdb-mongodb/azuredeploy.json)]

### <a name="deploy-via-azure-cli"></a>Implantar por meio da CLI do Azure

Para implantar o modelo do Resource Manager usando a CLI do Azure, **cópia** script e selecione **Experimente** para abrir o Azure Cloud shell. Para colar o script, o shell e, em seguida, selecione **colar**:

```azurecli-interactive

read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the location (i.e. westus2): ' location
read -p 'Enter the account name: ' accountName
read -p 'Enter the primary region (i.e. westus2): ' primaryRegion
read -p 'Enter the secondary region (i.e. eastus2): ' secondaryRegion
read -p 'Enter the database name: ' databaseName
read -p 'Enter the first collection name: ' collection1Name
read -p 'Enter the second collection name: ' collection2Name

az group create --name $resourceGroupName --location $location
az group deployment create --resource-group $resourceGroupName \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb/azuredeploy.json \
  --parameters accountName=$accountName primaryRegion=$primaryRegion secondaryRegion=$secondaryRegion \
  databaseName=$databaseName collection1Name=$collection1Name collection2Name=$collection2Name

az cosmosdb show --resource-group $resourceGroupName --name accountName --output tsv
```

O `az cosmosdb show` comando mostra a conta recém-criada do Azure Cosmos depois que ele foi provisionado. Se você optar por usar uma versão do CLI do Azure instalada localmente em vez de usar CloudShell, consulte [Interface de linha de comando do Azure (CLI)](/cli/azure/) artigo.

## Atualizar a taxa de transferência (RU/s) em um banco de dados <a id="database-ru-update"></a>

O modelo a seguir atualizará a taxa de transferência de um banco de dados. Copie o modelo e implantar, conforme mostrado abaixo ou visite [Galeria de início rápido do Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb-database-ru-update/) e implantar do portal do Azure. Você também pode baixar o modelo em seu computador local ou criar um novo modelo e especifique o caminho local com o `--template-file` parâmetro.

[!code-json[cosmosdb-mongodb-database-ru-update](~/quickstart-templates/101-cosmosdb-mongodb-database-ru-update/azuredeploy.json)]

### <a name="deploy-database-template-via-azure-cli"></a>Implantar o modelo de banco de dados por meio da CLI do Azure

Para implantar o modelo do Resource Manager usando a CLI do Azure, selecione **Experimente** para abrir o Azure Cloud shell. Para colar o script, o shell e, em seguida, selecione **colar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb-database-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName throughput=$throughput
```

## Taxa de transferência (RU/s) em uma coleção de atualização <a id="collection-ru-update"></a>

O modelo a seguir atualizará a taxa de transferência de uma coleção. Copie o modelo e implantar, conforme mostrado abaixo ou visite [Galeria de início rápido do Azure](https://azure.microsoft.com/resources/templates/101-cosmosdb-mongodb-collection-ru-update/) e implantar do portal do Azure. Você também pode baixar o modelo em seu computador local ou criar um novo modelo e especifique o caminho local com o `--template-file` parâmetro.

[!code-json[cosmosdb-mongodb-collection-ru-update](~/quickstart-templates/101-cosmosdb-mongodb-collection-ru-update/azuredeploy.json)]

### <a name="deploy-collection-template-via-azure-cli"></a>Implantar o modelo de coleção por meio da CLI do Azure

Para implantar o modelo do Resource Manager usando a CLI do Azure, selecione **Experimente** para abrir o Azure Cloud shell. Para colar o script, o shell e, em seguida, selecione **colar**:

```azurecli-interactive
read -p 'Enter the Resource Group name: ' resourceGroupName
read -p 'Enter the account name: ' accountName
read -p 'Enter the database name: ' databaseName
read -p 'Enter the collection name: ' collectionName
read -p 'Enter the new throughput: ' throughput

az group deployment create --resource-group $resourceGroupName \
   --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-cosmosdb-mongodb-collection-ru-update/azuredeploy.json \
   --parameters accountName=$accountName databaseName=$databaseName collectionName=$collectionName throughput=$throughput
```

## <a name="next-steps"></a>Próximas etapas

Estes são alguns recursos adicionais:

- [Documentação do Azure Resource Manager](/azure/azure-resource-manager/)
- [Esquema do provedor de recursos do Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
- [Modelos de início rápido do BD Cosmos do Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.DocumentDB&pageNumber=1&sort=Popular)
- [Solucionar problemas de erros comuns de implantação do Azure Resource Manager](../azure-resource-manager/resource-manager-common-deployment-errors.md)