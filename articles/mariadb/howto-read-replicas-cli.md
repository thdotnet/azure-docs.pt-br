---
title: Criar e gerenciar réplicas de leitura no banco de dados do Azure para MariaDB
description: Este artigo descreve como configurar e gerenciar réplicas de leitura no banco de dados do Azure para MariaDB usando o CLI do Azure.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 07/26/2019
ms.openlocfilehash: ba0f3bd002b2675c33ea5106ce64c7957c9989d0
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70309117"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mariadb-using-the-azure-cli"></a>Como criar e gerenciar réplicas de leitura no banco de dados do Azure para MariaDB usando o CLI do Azure

Neste artigo, você aprenderá a criar e gerenciar réplicas de leitura na mesma região do Azure que o mestre no banco de dados do Azure para o serviço MariaDB usando o CLI do Azure.

## <a name="prerequisites"></a>Pré-requisitos

- [Instalar a CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- Um [banco de dados do Azure para o servidor MariaDB](quickstart-create-mariadb-server-database-using-azure-portal.md) que será usado como o servidor mestre. 

> [!IMPORTANT]
> O recurso ler réplica só está disponível para o banco de dados do Azure para servidores MariaDB nos tipos de preço Uso Geral ou com otimização de memória. Assegure-se de que o servidor principal esteja em um desses níveis de preços.

## <a name="create-a-read-replica"></a>Criar uma réplica de leitura

Um servidor de réplica de leitura pode ser criado usando o seguinte comando:

```azurecli-interactive
az mariadb server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup
```

O comando `az mariadb server replica create` exige os seguintes parâmetros:

| Configuração | Valor de exemplo | Descrição  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  O grupo de recursos para o qual o servidor de réplica será criado.  |
| name | mydemoreplicaserver | O nome do novo servidor de réplica criado. |
| source-server | mydemoserver | O nome ou a ID do servidor mestre existente para replicar. |

Para criar uma réplica de leitura entre regiões, use `--location` o parâmetro. O exemplo de CLI abaixo cria a réplica no oeste dos EUA.

```azurecli-interactive
az mariadb server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup --location westus
```

> [!NOTE]
> Para saber mais sobre em quais regiões você pode criar uma réplica, visite o [artigo conceitos de leitura de réplica](concepts-read-replicas.md). 

> [!NOTE]
> Réplicas de leitura são criadas com a mesma configuração de servidor que o mestre. A configuração do servidor de réplica pode ser alterada depois de criada. Recomenda-se que a configuração do servidor de réplica seja mantida em valores iguais ou maiores que o mestre para garantir que a réplica seja capaz de acompanhar o mestre.

## <a name="stop-replication-to-a-replica-server"></a>Parar a replicação para um servidor de réplica

> [!IMPORTANT]
> Parar a replicação para um servidor é irreversível. Depois que a replicação tiver parado entre um mestre e uma réplica, ela não poderá ser desfeita. O servidor de réplica então se torna um servidor autônomo e agora suporta tanto leitura quanto gravação. Este servidor não pode ser transformado em uma réplica novamente.

A replicação para um servidor de réplica de leitura pode ser interrompida usando o seguinte comando:

```azurecli-interactive
az mariadb server replica stop --name mydemoreplicaserver --resource-group myresourcegroup
```

O comando `az mariadb server replica stop` exige os seguintes parâmetros:

| Configuração | Valor de exemplo | Descrição  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  O grupo de recursos em que há o servidor de réplica.  |
| name | mydemoreplicaserver | O nome do servidor de réplica para interromper a replicação. |

## <a name="delete-a-replica-server"></a>Excluir um servidor de réplica

A exclusão de um servidor de réplica de leitura pode ser feita executando o comando **[AZ MariaDB Server Delete](/cli/azure/mariadb/server)** .

```azurecli-interactive
az mariadb server delete --resource-group myresourcegroup --name mydemoreplicaserver
```

## <a name="delete-a-master-server"></a>Excluir um servidor mestre

> [!IMPORTANT]
> A exclusão de um servidor mestre interrompe a replicação para todos os servidores de réplica e exclui o próprio servidor mestre. Os servidores de réplica tornam-se servidores independentes que agora suportam leitura e gravação.

Para excluir um servidor mestre, você pode executar o comando **[AZ MariaDB Server Delete](/cli/azure/mariadb/server)** .

```azurecli-interactive
az mariadb server delete --resource-group myresourcegroup --name mydemoserver
```

## <a name="list-replicas-for-a-master-server"></a>Listar réplicas para um servidor mestre

Para exibir todas as réplicas de um determinado servidor mestre, execute o seguinte comando: 

```azurecli-interactive
az mariadb server replica list --server-name mydemoserver --resource-group myresourcegroup
```

O comando `az mariadb server replica list` exige os seguintes parâmetros:

| Configuração | Valor de exemplo | Descrição  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  O grupo de recursos para o qual o servidor de réplica será criado.  |
| server-name | mydemoserver | O nome ou a ID do servidor mestre. |

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre [ler réplicas](concepts-read-replicas.md)
