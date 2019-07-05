---
title: Alterar as chaves de acesso da conta de armazenamento
titleSuffix: Azure Machine Learning service
description: Saiba como alterar as chaves de acesso para a conta de armazenamento do Azure usada pelo seu espaço de trabalho. Serviço de Machine Learning do Azure usa uma conta de armazenamento do Azure para armazenar dados e modelos. Quando você regenerar a chave de acesso da conta de armazenamento, você deve atualizar o serviço de Azure Machine Learning para usar as novas chaves.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 05/17/2019
ms.openlocfilehash: 488a032e177897caf2897ba6335f4e7f64dc0e4d
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543837"
---
# <a name="regenerate-storage-account-access-keys"></a>Regenerar chaves de acesso da conta de armazenamento

Saiba como alterar as chaves de acesso para contas de armazenamento do Azure usadas pelo serviço do Azure Machine Learning. O Azure Machine Learning pode usar contas de armazenamento para armazenar dados ou modelos treinados.

Para fins de segurança, talvez você precise alterar as chaves de acesso para uma conta de armazenamento do Azure. Quando você regenerar a chave de acesso, o Azure Machine Learning deve ser atualizado para usar a nova chave. O Azure Machine Learning pode estar usando a conta de armazenamento para ambos os armazenamento de modelo e como um repositório de dados.

## <a name="prerequisites"></a>Pré-requisitos

* Um workspace de serviço do Azure Machine Learning. Para obter mais informações, consulte o [criar um espaço de trabalho](setup-create-workspace.md) artigo.

* O [do Azure Machine Learning SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

* O [extensão de CLI do Azure Machine Learning](reference-azure-machine-learning-cli.md).

<a id="whattoupdate"></a> 

## <a name="what-needs-to-be-updated"></a>O que precisa ser atualizado

Contas de armazenamento podem ser usadas pelo espaço de trabalho do serviço de Azure Machine Learning (armazenamento de logs, modelos, instantâneos, etc.) e como um repositório de dados. O processo para atualizar o espaço de trabalho é um único comando da CLI do Azure e pode ser executado depois de atualizar a chave de armazenamento. O processo de atualização de armazenamentos de dados é mais complicado e exige o descobrimento quais repositórios de dados estiver usando a conta de armazenamento e, em seguida, registrá-los novamente.

> [!IMPORTANT]
> Atualize o espaço de trabalho usando a CLI do Azure e os armazenamentos de dados usando o Python, ao mesmo tempo. Atualizar apenas um ou outro não é suficiente e pode causar erros até que ambos sejam atualizadas.

Para descobrir as contas de armazenamento que são usadas por seus armazenamentos de dados, use o seguinte código:

```python
import azureml.core
from azureml.core import Workspace, Datastore

ws = Workspace.from_config()

default_ds = ws.get_default_datastore()
print("Default datstore: " + default_ds.name + ", storage account name: " + default_ds.account_name + ", container name: " + ds.container_name)

datastores = ws.datastores
for name, ds in datastores.items():
    if ds.datastore_type == "AzureBlob" or ds.datastore_type == "AzureFile":
        print("datastore name: " + name + ", storage account name: " + ds.account_name + ", container name: " + ds.container_name)
```

Esse código procura por quaisquer armazenamentos de dados registrados que usam o armazenamento do Azure e lista as seguintes informações:

* Nome do repositório de dados: O nome do repositório de dados que a conta de armazenamento está registrada.
* Nome da conta de armazenamento: O nome da conta de armazenamento do Azure.
* Contêiner: O contêiner na conta de armazenamento que é usado por esse registro.

Se houver uma entrada para a conta de armazenamento que você pretende regenerar chaves de acesso para o, salve o nome do repositório de dados, o nome da conta de armazenamento e o nome do contêiner.

## <a name="update-the-access-key"></a>Atualizar a chave de acesso

Para atualizar o serviço de Azure Machine Learning para usar a nova chave, use as seguintes etapas:

> [!IMPORTANT]
> Execute todas as etapas, atualizando a ambos o espaço de trabalho usando a CLI e armazenamentos de dados usando o Python. Atualizar apenas um ou outro pode causar erros até que ambos sejam atualizadas.

1. Regenere a chave. Para obter informações sobre como regenerar uma chave de acesso, consulte o [gerenciar uma conta de armazenamento](/azure/storage/common/storage-account-manage#access-keys) artigo. Salve a nova chave.

1. Para atualizar o espaço de trabalho para usar a nova chave, use as seguintes etapas:

    1. Para entrar assinatura do Azure que contém o seu espaço de trabalho usando o seguinte comando da CLI do Azure:

        ```azurecli-interactive
        az login
        ```

    1. Para instalar a extensão do Azure Machine Learning, use o seguinte comando:

        ```azurecli-interactive
        az extension add -n azure-cli-ml 
        ```

    1. Para atualizar o espaço de trabalho para usar a nova chave, use o comando a seguir. Substitua `myworkspace` com seu nome de espaço de trabalho do Azure Machine Learning e substitua `myresourcegroup` com o nome do grupo de recursos do Azure que contém o espaço de trabalho.

        ```azurecli-interactive
        az ml workspace sync-keys -w myworkspace -g myresourcegroup
        ```

        Este comando sincroniza automaticamente as novas chaves para a conta de armazenamento do Azure usada pelo espaço de trabalho.

1. Para registrar novamente os armazenamentos de dados que usam a conta de armazenamento, use os valores da [o que precisa ser atualizado](#whattoupdate) seção e a chave da etapa 1 com o código a seguir:

    ```python
    ds = Datastore.register_azure_blob_container(workspace=ws, 
                                              datastore_name='your datastore name', 
                                              container_name='your container name',
                                              account_name='your storage account name', 
                                              account_key='new storage account key',
                                              overwrite=True)
    ```

    Uma vez que `overwrite=True` for especificado, esse código substitui o registro existente e atualiza-lo para usar a nova chave.

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre como registrar os armazenamentos de dados, consulte o [ `Datastore` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py) referência da classe.
