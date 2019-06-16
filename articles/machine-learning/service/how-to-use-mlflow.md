---
title: Como usar MLflow com o serviço Azure Machine Learning
titleSuffix: Azure Machine Learning service
description: Saiba como registrar em log as métricas e artefatos usando a biblioteca MLflow ao serviço de Azure Machine Learning
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: nibaccam
ms.topic: conceptual
ms.date: 06/10/2019
ms.custom: seodec18
ms.openlocfilehash: 934e9b705ab5f399d29f24c915b4c60a3b06138b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082567"
---
# <a name="how-to-use-mlflow-with-azure-machine-learning-service-preview"></a>Como usar MLflow com o serviço de Azure Machine Learning (versão prévia)

Este artigo demonstra como usar MLflow URI de rastreamento e registro em log API, coletivamente também conhecido como MLflow de acompanhamento, com o serviço de Azure Machine Learning para rastrear e registrar suas métricas de teste e os artefatos em sua [Azure Machine Learning espaço de trabalho do serviço](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture#workspace). Se você já usa o acompanhamento de MLflow para seus testes, o espaço de trabalho fornece um local centralizado, seguro e escalonável para armazenar seus modelos e métricas de treinamento.

[MLflow](https://www.mlflow.org) é uma biblioteca de código-fonte aberto para o gerenciamento de ciclo de vida de seus experimentos de aprendizado de máquina. [Acompanhamento de MLFlow](https://mlflow.org/docs/latest/quickstart.html#using-the-tracking-api) é um componente do MLflow que registra em log e rastreia sua métricas de execução de treinamento e artefatos de modelo, se seus testes são executados localmente, em uma máquina virtual ou em um controle remoto do cluster de computação.
![mlflow com o diagrama de aprendizado de máquina do azure](media/how-to-use-mlflow/mlflow-diagram.png)

## <a name="compare-mlflow-and-azure-machine-learning-service-clients"></a>Compare os clientes de serviço MLflow e Azure Machine Learning

 A tabela abaixo resume os clientes diferentes que podem usar o serviço Azure Machine Learning e seus recursos de função respectiva.

 Controle MLflow oferece registro de métricas e artefato as funcionalidades de armazenamento que só estão disponíveis por meio de [SDK do Python do Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

| | Acompanhamento de MLflow | Azure Machine Learning <br> SDK do Python |  Azure Machine Learning <br> CLI | Portal do Azure|
|-|-|-|-|-|
| Gerenciar workspace |   | ✓ |  ✓ | ✓  |
| Usar armazenamentos de dados  |   | ✓ |  ✓ |    |
| Métricas de log      | ✓ | ✓ |    |    |
| Carregar artefatos | ✓ | ✓ |    |    |
| Métricas de exibição     | ✓ | ✓ | ✓  | ✓ |
| Gerenciar a computação   |   | ✓ | ✓  | ✓ |
| Implantar modelos    |   | ✓ |   ✓ | ✓ |

## <a name="prerequisites"></a>Pré-requisitos

* [Instale MLflow.](https://mlflow.org/docs/latest/quickstart.html)
* [Instalar o SDK do Python do Azure Machine Learning no computador local e criar um espaço de trabalho do Azure Machine Learning](setup-create-workspace.md#sdk). O SDK fornece a conectividade para MLflow acessar seu espaço de trabalho.

## <a name="track-local-runs"></a>Acompanhar execuções locais

Instalar o `azureml-contrib-run` pacote para usar o controle de MLflow com o Azure Machine Learning em seus testes executados localmente em um bloco de anotações do Jupyter ou editor de código.

```shell
pip install azureml-contrib-run
```

>[!NOTE]
>O namespace azureml.contrib muda com frequência, à medida que trabalhamos para aprimorar o serviço. Assim, tudo nesse namespace deve ser considerado como uma versão prévia e sem o suporte total da Microsoft.

Importar o `mlflow` e [ `Workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) classes para acessar MLflow rastreamento do URI e configurar seu espaço de trabalho.

No código a seguir, o `get_mlflow_tracking_uri()` método atribui um endereço de URI de rastreamento exclusivo para o espaço de trabalho `ws`, e `set_tracking_uri()` aponta o MLflow URI de rastreamento para esse endereço.

```Python
import mlflow
from azureml.core import Workspace

ws = Workspace.from_config()

mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())
```

>[!NOTE]
>O URI de rastreamento é válida até uma hora ou menos. Se você reiniciar o seu script após algum tempo ocioso, use a API get_mlflow_tracking_uri para obter um novo URI.

Defina o nome do teste MLflow com `set_experiment()` e inicie seu treinamento executar com `start_run()`. Em seguida, use `log_metric()` para ativar a API de log MLflow e iniciar seu treinamento executar as métricas de registro em log.

```Python
experiment_name = "experiment_with_mlflow"
mlflow.set_experiment(experiment_name)

with mlflow.start_run():
    mlflow.log_metric('alpha', 0.03)
```

## <a name="track-remote-runs"></a>Acompanhar execuções remotas

Execuções remotas permitem que você Treine seus modelos em serviços de computação mais poderosos, como máquinas virtuais habilitadas para GPU ou clusters de computação do Machine Learning. Ver [configurar destinos de computação para treinamento de modelo](how-to-set-up-training-targets.md) para saber mais sobre as diferentes opções de computação.

Configure sua computação e o ambiente de execução de treinamento com o [ `Environment` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) classe. Incluir `mlflow` e `azure-contrib-run` pacotes no ambiente do pip [ `CondaDependencies` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) seção. Então, construir [ `ScriptRunConfig` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) com sua computação remota como o destino de computação.

```Python

from azureml.core import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core import ScriptRunConfig

exp = Experiment(workspace = "my_workspace",
                 name = "my_experiment")

mlflow_env = Environment(name="mlflow-env")

cd = CondaDependencies.create(pip_packages = ["mlflow", "azureml-contrib-run"])

mlflow_env.python.conda_dependencies=cd

src = ScriptRunConfig(source_directory="./my_script_location", script="my_training_script.py")

src.run_config.target = "my-remote-compute-compute"
src.run_config.environment = mlflow_env
```

No seu script de treinamento, importar `mlflow` para usar a APIs de registro de MLflow e começar a registrar suas métricas de execução.

```Python
import mlflow

with mlflow.start_run():
    mlflow.log_metric("example", 1.23)
```

Com essa computação e a configuração de execução de treinamento, use o `Experiment.submit("train.py")` método para enviar uma execução. Isso define o URI de acompanhamento de MLflow e direciona o registro em log do MLflow ao espaço de trabalho automaticamente.

```Python
run = exp.submit(src)
```

## <a name="view-metrics-and-artifacts-in-your-workspace"></a>Exibir métricas e os artefatos no espaço de trabalho

As métricas e os artefatos do MLflow log são mantidos em seu espaço de trabalho o [portal do Azure](https://portal.azure.com). Para exibi-los a qualquer momento, navegue até seu espaço de trabalho e localizar o teste por nome.

## <a name="clean-up-resources"></a>Limpar recursos

Se você não planeja usar as métricas registradas em log e os artefatos em seu espaço de trabalho, a capacidade de excluí-los individualmente está disponível no momento. Em vez disso, exclua o grupo de recursos que contém a conta de armazenamento e o espaço de trabalho, para não incorrer em encargos:

1. No portal do Azure, selecione **Grupos de recursos** no canto esquerdo.

   ![Exclusão no portal do Azure](media/how-to-use-mlflow/delete-resources.png)

1. Selecione o grupo de recursos criado na lista.

1. Selecione **Excluir grupo de recursos**.

1. Insira o nome do grupo de recursos. Em seguida, selecione **Excluir**.

## <a name="example-notebooks"></a>Blocos de anotações de exemplo

O [MLflow com blocos de anotações do AM do Azure](https://github.com/Azure/MachineLearningNotebooks/blob/master/contrib/mlflow) demonstra conceitos neste artigo.

## <a name="next-steps"></a>Próximas etapas

* [Como implantar um modelo](how-to-deploy-and-where.md).