---
title: Criar conjuntos de dados para acessar dados com conjuntos de dados de azureml
titleSuffix: Azure Machine Learning service
description: Saiba como criar conjuntos de dados de várias fontes e registrar os conjuntos de dados com o seu espaço de trabalho
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sihhu
author: MayMSFT
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/21/2019
ms.openlocfilehash: b4c22caae86e20b8379db2b7feffb1ca82001239
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66753162"
---
# <a name="create-and-access-datasets-preview-in-azure-machine-learning"></a>Criar e acessar os conjuntos de dados (versão prévia) no Azure Machine Learning

Neste artigo, você aprenderá como criar conjuntos de dados do Azure Machine Learning (versão prévia) e como acessar os dados de testes locais e remotos.

Com conjuntos de dados gerenciados, você pode: 
* **Acessar facilmente os dados durante o treinamento de modelo** sem reconectar-se ao armazenamentos subjacentes

* **Garantir a consistência dos dados e a capacidade de reprodução** usando o mesmo ponteiro em experimentos: blocos de anotações, ml automatizado, pipelines, interface visual

* **Compartilhar dados e colaborar** com outros usuários

* **Explorar dados** & Gerenciar o ciclo de vida de versões e instantâneos de dados

* **Comparar dados** no treinamento para produção


## <a name="prerequisites"></a>Pré-requisitos

Para criar e trabalhar com conjuntos de dados, você precisa:

* Uma assinatura do Azure. Se você não tiver uma assinatura do Azure, crie uma conta gratuita antes de começar. Experimente a [versão gratuita ou paga do Serviço do Azure Machine Learning](https://aka.ms/AMLFree) hoje mesmo.

* Um [espaço de trabalho de serviço de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace)

* O [SDK de aprendizado de máquina do Azure para Python instalado](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py), que inclui o pacote azureml conjuntos de dados.

> [!Note]
> Algumas classes de conjunto de dados (visualização) têm dependências no [azureml dataprep](https://docs.microsoft.com/python/api/azureml-dataprep/?view=azure-ml-py) (GA) do pacote. Para os usuários do Linux, essas classes são suportadas apenas nas seguintes distribuições:  Red Hat Enterprise Linux, Ubuntu, Fedora, and CentOS.

## <a name="data-formats"></a>Formatos de dados

Você pode criar um conjunto de dados do Azure Machine Learning dos dados a seguir:
+ [delimited](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset#from-delimited-files-path--separator------header--promoteheadersbehavior-all-files-have-same-headers--3---encoding--fileencoding-utf8--0---quoting-false--infer-column-types-true--skip-rows-0--skip-mode--skiplinesbehavior-no-rows--0---comment-none--include-path-false--archive-options-none-)
+ [binary](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-binary-files-path-)
+ [json](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-json-files-path--encoding--fileencoding-utf8--0---flatten-nested-arrays-false--include-path-false-)
+ [Excel](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-excel-files-path--sheet-name-none--use-column-headers-false--skip-rows-0--include-path-false--infer-column-types-true-)
+ [Parquet](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-parquet-files-path--include-path-false-)
+ [Banco de Dados SQL do Azure](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-sql-query-data-source--query-)
+ [Geração do Azure Data Lake. 1](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#from-sql-query-data-source--query-)

## <a name="create-datasets"></a>Criar conjuntos de dados 

Você pode interagir com seus conjuntos de dados com o pacote azureml conjuntos de dados na [SDK do Python do Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) e, especificamente [o `Dataset` classe](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py).

### <a name="create-from-local-files"></a>Criar a partir de arquivos locais

Carregar arquivos em seu computador local, especificando o caminho de arquivo ou pasta com o [ `auto_read_files()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset(class)?view=azure-ml-py#auto-read-files-path--include-path-false-) método a partir de `Dataset` classe.  Esse método executa as etapas a seguir sem exigir que você especifique o tipo de arquivo ou argumentos de análise:

* Inferindo e definindo o delimitador.
* Ignorando os registros vazios na parte superior do arquivo.
* Inferindo e definir a linha de cabeçalho.
* Inferindo e convertendo tipos de dados de coluna.

```Python
from azureml.core.dataset import Dataset

dataset = Dataset.auto_read_files('./data/crime.csv')
```

Alternativamente, utilize as funções específicas do arquivo para controlar explicitamente a análise do seu arquivo. 


### <a name="create-from-azure-datastores"></a>Criar a partir de armazenamentos de dados do Azure

Para criar conjuntos de dados de um armazenamento de dados do Azure:

* Verifique se você tiver `contributor` ou `owner` acesso ao repositório de dados do Azure registrado.

* Importar o [ `Workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) e [ `Datastore` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.datastore(class)?view=azure-ml-py#definition) e `Dataset` pacotes do SDK.

```Python
from azureml.core.workspace import Workspace
from azureml.core.datastore import Datastore
from azureml.core.dataset import Dataset

datastore_name = 'your datastore name'

# get existing workspace
workspace = Workspace.from_config()
```

 O `get()` método recupera um armazenamento de dados existente no espaço de trabalho.

```
dstore = Datastore.get(workspace, datastore_name)
```

Use o `from_delimited_files()` método para ler arquivos delimitados e criar um conjunto de dados não registrado.

```Python
# create an in-memory Dataset on your local machine
datapath = dstore.path('data/src/crime.csv')
dataset = Dataset.from_delimited_files(datapath)

# returns the first 5 rows of the Dataset as a pandas Dataframe.
dataset.head(5)
```

## <a name="register-datasets"></a>Conjuntos de dados de registro

Para concluir o processo de criação, registre seus conjuntos de dados com o espaço de trabalho:

Use o [ `register()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#register-workspace--name--description-none--tags-none--visible-true--exist-ok-false--update-if-exist-false-) método para registrar os conjuntos de dados para seu espaço de trabalho para que possam ser compartilhados com outras pessoas e reutilizados em vários experimentos.

```Python
dataset = dataset.register(workspace = workspace,
                           name = 'dataset_crime',
                           description = 'Training data',
                           exist_ok = False
                           )
```

>[!NOTE]
> Se `exist_ok = False` (padrão), e você tentar registrar um conjunto de dados com o mesmo nome que outro, ocorrerá um erro. Definido como `True` para substituir o existente.

## <a name="access-data-in-datasets"></a>Acessar dados em conjuntos de dados

Conjuntos de dados registrados estão acessíveis e consumíveis localmente, remotamente e em clusters de computação como a computação do Azure Machine Learning. Para reutilizar seu conjunto de dados registrado em experimentos e ambientes de computação, use o código a seguir para obter o conjunto de dados registrado e espaço de trabalho por nome.

```Python
workspace = Workspace.from_config()

# See list of datasets registered in workspace.
Dataset.list(workspace)

# Get dataset by name
dataset = workspace.datasets['dataset_crime']
```

## <a name="next-steps"></a>Próximas etapas

* [Explorar e preparar os conjuntos de dados](how-to-explore-prepare-data.md).
* [Gerenciar o ciclo de vida das definições de conjunto de dados](how-to-manage-dataset-definitions.md).
* Para obter um exemplo de como usar conjuntos de dados, consulte o [notebooks de exemplo](https://aka.ms/dataset-tutorial).
