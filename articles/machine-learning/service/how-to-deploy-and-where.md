---
title: Como e onde implantar modelos
titleSuffix: Azure Machine Learning service
description: 'Saiba como e onde implantar os modelos do serviço do Azure Machine Learning incluindo: Instâncias de Contêiner do Azure, Serviço de Kubernetes do Azure, Azure IoT Edge e matriz de porta programável no campo.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jordane
author: jpe316
ms.reviewer: larryfr
ms.date: 05/31/2019
ms.custom: seoapril2019
ms.openlocfilehash: dcb90eb8ee25b8b0c780006f3555a5a9b815ffdd
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514255"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Implantar modelos com o serviço do Azure Machine Learning

Saiba como implantar seu modelo de machine learning como um serviço web na nuvem do Azure ou em dispositivos IoT Edge. 

O fluxo de trabalho é semelhante independentemente do [onde você implanta](#target) seu modelo:

1. Registre o modelo.
1. Preparar para implantar (especificar ativos, uso, o destino de computação)
1. Implante o modelo para o destino de computação.
1. Teste o modelo implantado, também chamado de serviço web.

Para obter mais informações sobre os conceitos envolvidos no fluxo de trabalho de implantação, confira [Gerenciar, implantar e monitorar modelos com o Serviço do Azure Machine Learning](concept-model-management-and-deployment.md).

## <a name="prerequisites"></a>Pré-requisitos

- Um modelo. Se você não tiver um modelo treinado, você pode usar o modelo e arquivos de dependência fornecida no [este tutorial](https://aka.ms/azml-deploy-cloud).

- O [extensão de CLI do Azure para o serviço de Machine Learning](reference-azure-machine-learning-cli.md), [SDK do Python do Azure Machine Learning](https://aka.ms/aml-sdk), ou o [extensão do Azure Machine Learning Visual Studio Code](how-to-vscode-tools.md).

## <a id="registermodel"></a> Registrar seu modelo

Um contêiner lógico modelo registrado para um ou mais arquivos que compõem seu modelo. Por exemplo, se você tiver um modelo que é armazenado em vários arquivos, você pode registrá-los como um único modelo no espaço de trabalho. Após o registro, você pode, em seguida, baixar ou implantar o modelo registrado e receba todos os arquivos que foram registrados.

Modelos de aprendizado de máquina são registrados no seu espaço de trabalho do Azure Machine Learning. O modelo pode vir de Azure Machine Learning ou pode vir de algum outro lugar. Os exemplos a seguir demonstram como registrar um modelo de arquivo:

### <a name="register-a-model-from-an-experiment-run"></a>Registrar um modelo de uma execução de teste

+ **Exemplo de Scikit-Learn usando o SDK**
  ```python
  model = run.register_model(model_name='sklearn_mnist', model_path='outputs/sklearn_mnist_model.pkl')
  print(model.name, model.id, model.version, sep='\t')
  ```

  > [!TIP]
  > Para incluir vários arquivos no registro de modelo, defina `model_path` para o diretório que contém os arquivos.

+ **Usando a CLI**

  ```azurecli-interactive
  az ml model register -n sklearn_mnist  --asset-path outputs/sklearn_mnist_model.pkl  --experiment-name myexperiment
  ```

  > [!TIP]
  > Para incluir vários arquivos no registro de modelo, defina `--asset-path` para o diretório que contém os arquivos.

+ **Usando o VS Code**

  Registrar modelos usando os arquivos de modelo ou pastas com o [VS Code](how-to-vscode-tools.md#deploy-and-manage-models) extensão.

### <a name="register-an-externally-created-model"></a>Registrar um modelo criado externamente

[!INCLUDE [trusted models](../../../includes/machine-learning-service-trusted-model.md)]

Você pode registrar um modelo criado externamente, fornecendo uma **caminho local** ao modelo. Você pode fornecer uma pasta ou um único arquivo.

+ **Exemplo ONNX com o SDK do Python:**
  ```python
  onnx_model_url = "https://www.cntk.ai/OnnxModels/mnist/opset_7/mnist.tar.gz"
  urllib.request.urlretrieve(onnx_model_url, filename="mnist.tar.gz")
  !tar xvzf mnist.tar.gz
  
  model = Model.register(workspace = ws,
                         model_path ="mnist/model.onnx",
                         model_name = "onnx_mnist",
                         tags = {"onnx": "demo"},
                         description = "MNIST image classification CNN from ONNX Model Zoo",)
  ```

  > [!TIP]
  > Para incluir vários arquivos no registro de modelo, defina `model_path` para o diretório que contém os arquivos.

+ **Usando a CLI**
  ```azurecli-interactive
  az ml model register -n onnx_mnist -p mnist/model.onnx
  ```

  > [!TIP]
  > Para incluir vários arquivos no registro de modelo, defina `-p` para o diretório que contém os arquivos.

**Tempo estimado**: Cerca de 10 segundos.

Para obter mais informações, consulte a documentação de referência da [classe do modelo](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

Para obter mais informações sobre como trabalhar com modelos treinados serviço fora do Azure Machine Learning, consulte [como implantar um modelo existente](how-to-deploy-existing-model.md).

<a name="target"></a>

## <a name="choose-a-compute-target"></a>Escolha um destino de computação

Os seguintes destinos de computação ou recursos de computação, podem ser usados para hospedar a implantação do serviço web. 

[!INCLUDE [aml-compute-target-deploy](../../../includes/aml-compute-target-deploy.md)]

## <a name="prepare-to-deploy"></a>Preparar-se para implantar

Para implantar como um serviço web, você deve criar uma configuração de inferência de tipos (`InferenceConfig`) e uma configuração de implantação. Inferência de tipos ou modelo de pontuação, é a fase em que o modelo implantado é usado para previsão, mais comumente em dados de produção. A configuração de inferência de tipos, você pode especificar os scripts e as dependências necessárias para atender seu modelo. A configuração de implantação você deve especificar os detalhes de como fornecer o modelo de destino de computação.


### <a id="script"></a> 1. Definir o seu script de entrada & dependências

O script de entrada recebe os dados enviados para um serviço web implantado e passa-o para o modelo. Ele então envia de volta ao cliente a resposta retornada pelo modelo. **O script é específico para seu modelo**; ele deve compreender os dados que o modelo de espera e retorna.

O script contém duas funções que carregam e executam o modelo:

* `init()`: Normalmente, essa função carrega o modelo em um objeto global. Essa função é executada apenas uma vez quando o contêiner do Docker para o serviço web é iniciado.

* `run(input_data)`: Essa função usa o modelo para prever um valor com base nos dados de entrada. Entradas e saídas para a execução normalmente usam JSON para serialização e desserialização. Você também pode trabalhar com os dados binários brutos. Você pode transformar os dados antes de enviá-los para o modelo ou antes de retorná-los ao cliente.

#### <a name="what-is-getmodelpath"></a>O que é get_model_path?

Quando você registra um modelo, você fornecer um nome de modelo usado para gerenciar o modelo no registro. Use esse nome com o [Model.get_model_path()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) para recuperar o caminho dos arquivos de modelo no sistema de arquivos local. Se você registrar uma pasta ou uma coleção de arquivos, essa API retorna o caminho para o diretório que contém os arquivos.

Se você registra um modelo, você atribuir um nome, que corresponde ao qual o modelo é colocado, localmente ou durante a implantação de serviço.

O exemplo abaixo irá retornar um caminho para um único arquivo chamado `sklearn_mnist_model.pkl` (que foi registrado com o nome `sklearn_mnist`):

```python
model_path = Model.get_model_path('sklearn_mnist')
``` 

#### <a name="optional-automatic-swagger-schema-generation"></a>(Opcional) Geração automática do esquema de Swagger

Para automaticamente gerar um esquema para o serviço web, fornecer um exemplo de entrada e/ou de saída no construtor para um dos objetos do tipo definido e o tipo e o exemplo são usados para criar automaticamente o esquema. Serviço de Machine Learning do Azure, em seguida, cria uma [OpenAPI](https://swagger.io/docs/specification/about/) (Swagger) a especificação para o serviço web durante a implantação.

Atualmente, há suporte para os seguintes tipos:

* `pandas`
* `numpy`
* `pyspark`
* objeto padrão do Python

Para usar a geração de esquema, inclua o `inference-schema` pacote em seu arquivo de ambiente do conda. O exemplo a seguir usa `[numpy-support]` , pois o script de entrada usa um tipo de parâmetro numpy: 

#### <a name="example-dependencies-file"></a>Exemplo de arquivo de dependências
O YAML a seguir está um exemplo de um arquivo de dependências de Conda para inferência de tipos.

```YAML
name: project_environment
dependencies:
  - python=3.6.2
  - pip:
    - azureml-defaults
    - scikit-learn==0.20.0
    - inference-schema[numpy-support]
```

Se você quiser usar a geração de esquema automático, o script de entrada **devem** importar o `inference-schema` pacotes. 

Definir a entrada e formatos de exemplo na saída de `input_sample` e `output_sample` variáveis que representam os formatos de solicitação e resposta para o serviço web. Usar esses exemplos na entrada e saída decoradores de função no `run()` função. Scikit-Saiba o exemplo a seguir usa a geração de esquema.

> [!TIP]
> Depois de implantar o serviço, use o `swagger_uri` propriedade para recuperar o documento de esquema JSON.

#### <a name="example-entry-script"></a>Script de exemplo de entrada

O exemplo a seguir demonstra como aceitar e retornar dados JSON:

```python
#example: scikit-learn and Swagger
import json
import numpy as np
from sklearn.externals import joblib
from sklearn.linear_model import Ridge
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType

def init():
    global model
    # note here "sklearn_regression_model.pkl" is the name of the model registered under
    # this is a different behavior than before when the code is run locally, even though the code is the same.
    model_path = Model.get_model_path('sklearn_regression_model.pkl')
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)

input_sample = np.array([[10,9,8,7,6,5,4,3,2,1]])
output_sample = np.array([3726.995])

@input_schema('data', NumpyParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # you can return any datatype as long as it is JSON-serializable
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```

#### <a name="example-script-with-dictionary-input-support-consumption-from-power-bi"></a>Exemplo de script com a entrada do dicionário (consumo de suporte do Power BI)

O exemplo a seguir demonstra como definir dados de entrada como < chave: valor > dictionary, usando o Dataframe. Esse método tem suporte para o consumo de serviço web implantado do Power BI ([Saiba mais sobre como consumir o serviço web do Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-integration)):

```python
import json
import pickle
import numpy as np
import pandas as pd
import azureml.train.automl
from sklearn.externals import joblib
from azureml.core.model import Model

from inference_schema.schema_decorators import input_schema, output_schema
from inference_schema.parameter_types.numpy_parameter_type import NumpyParameterType
from inference_schema.parameter_types.pandas_parameter_type import PandasParameterType

def init():
    global model
    model_path = Model.get_model_path('model_name')   # replace model_name with your actual model name, if needed
    # deserialize the model file back into a sklearn model
    model = joblib.load(model_path)

input_sample = pd.DataFrame(data=[{
              "input_name_1": 5.1,         # This is a decimal type sample. Use the data type that reflects this column in your data
              "input_name_2": "value2",    # This is a string type sample. Use the data type that reflects this column in your data
              "input_name_3": 3            # This is a integer type sample. Use the data type that reflects this column in your data
            }])

output_sample = np.array([0])              # This is a integer type sample. Use the data type that reflects the expected result

@input_schema('data', PandasParameterType(input_sample))
@output_schema(NumpyParameterType(output_sample))
def run(data):
    try:
        result = model.predict(data)
        # you can return any datatype as long as it is JSON-serializable
        return result.tolist()
    except Exception as e:
        error = str(e)
        return error
```
Para obter mais exemplos de script, consulte os exemplos a seguir:

* Pytorch: [https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-pytorch)
* TensorFlow: [https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow)
* Keras: [https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras)
* ONNX: [https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx/)
* Pontuação em relação aos dados binários: [Como consumir um serviço web](how-to-consume-web-service.md)

### <a name="2-define-your-inferenceconfig"></a>2. Definir seu InferenceConfig

A configuração de inferência de tipos descreve como configurar o modelo para fazer previsões. O exemplo a seguir demonstra como criar uma configuração de inferência de tipos. Essa configuração especifica o tempo de execução, o script de entrada e (opcionalmente) o arquivo de ambiente do conda:

```python
inference_config = InferenceConfig(runtime= "python",
                                   entry_script="x/y/score.py",
                                   conda_file="env/myenv.yml")
```

Para obter mais informações, consulte o [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) referência da classe.

Para obter informações sobre como usar uma imagem personalizada do Docker com a configuração de inferência de tipos, consulte [como implantar um modelo usando uma imagem personalizada do Docker](how-to-deploy-custom-docker-image.md).

### <a name="cli-example-of-inferenceconfig"></a>Exemplo CLI de InferenceConfig

O seguinte documento JSON é um exemplo de configuração de inferência de tipos para uso com a CLI de aprendizado de máquina:

```JSON
{
   "entryScript": "x/y/score.py",
   "runtime": "python",
   "condaFile": "env/myenv.yml",
   "sourceDirectory":"C:/abc",
}
```

As entidades a seguir são válidas neste arquivo:

* __entryScript__: Caminho do arquivo local que contém o código seja executado para a imagem.
* __runtime__: Qual tempo de execução a ser usado para a imagem. Tempos de execução atuais com suporte são 'spark-py' e 'python'.
* __condaFile__ (optional): Caminho do arquivo local que contém uma definição de ambiente do conda a ser usado para a imagem.
* __extraDockerFileSteps__ (optional): Caminho do arquivo local que contém etapas adicionais de Docker para ser executado durante a configuração de imagem.
* __sourceDirectory__ (opcional): Caminho para pastas que contém todos os arquivos para criar a imagem.
* __enableGpu__ (optional): Se deseja ou não habilitar a GPU dão suporte a imagem. A imagem GPU deve ser usada nos serviços do Microsoft Azure como instâncias de contêiner do Azure, computação do Azure Machine Learning, máquinas virtuais do Azure e serviço Kubernetes do Azure. Padrão é False.
* __baseImage__ (opcional): Uma imagem personalizada a ser usado como imagem de base. Se nenhuma imagem base for fornecida, em seguida, a imagem base será usada baseado fora de determinado parâmetro de tempo de execução.
* __baseImageRegistry__ (optional): Registro da imagem que contém a imagem base.
* __cudaVersion__ (opcional): Versão do CUDA para instalar para imagens que precisam de suporte GPU. A imagem GPU deve ser usada nos serviços do Microsoft Azure como instâncias de contêiner do Azure, computação do Azure Machine Learning, máquinas virtuais do Azure e serviço Kubernetes do Azure. Versões com suporte são 9.0, 10.0 e o 9.1. Se 'enable_gpu' estiver definida, padrão será '9.1'.

Essas entidades são mapeadas para os parâmetros para o [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) classe.

O comando a seguir demonstra como implantar um modelo usando a CLI:

```azurecli-interactive
az ml model deploy -n myservice -m mymodel:1 --ic inferenceconfig.json
```

Neste exemplo, a configuração contém os seguintes itens:

* Um diretório que contém os ativos necessários para inferência de tipos
* Esse modelo requer o Python
* O [script de entrada](#script), que é usado para manipular solicitações da web enviadas para o serviço implantado
* O arquivo conda que descreve os pacotes Python necessários para inferência de tipos

Para obter informações sobre como usar uma imagem personalizada do Docker com a configuração de inferência de tipos, consulte [como implantar um modelo usando uma imagem personalizada do Docker](how-to-deploy-custom-docker-image.md).

### <a name="3-define-your-deployment-configuration"></a>3. Definir sua configuração de implantação

Antes de implantar, você deve definir a configuração de implantação. A configuração de implantação é específica para o destino de computação que irá hospedar o serviço web. Por exemplo, ao implantar localmente deve especificar a porta em que o serviço aceita solicitações.

Você também precisará criar o recurso de computação. Por exemplo, se você fizer isso ainda não tiver um serviço de Kubernetes do Azure associado com seu espaço de trabalho.

A tabela a seguir fornece um exemplo de criação de uma configuração de implantação para cada destino de computação:

| Destino de computação | Exemplo de configuração de implantação |
| ----- | ----- |
| Local | `deployment_config = LocalWebservice.deploy_configuration(port=8890)` |
| Azure Container Instance | `deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |
| Serviço de Kubernetes do Azure | `deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)` |

As seções a seguir demonstram como criar a configuração de implantação e, em seguida, usá-lo para implantar o serviço web.

### <a name="optional-profile-your-model"></a>Opcional: Seu modelo de perfil
Antes de implantar seu modelo como um serviço, você talvez queira seu perfil para determinar os requisitos de memória e CPU ideal. Você pode fazer o perfil de seu modelo usando o SDK ou a CLI.

Para obter mais informações, confira nossa documentação do SDK aqui: https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#profile-workspace--profile-name--models--inference-config--input-data-

Resultados de criação de perfil de modelo são emitidos como um objeto Run.
Obter informações específicas sobre o esquema de modelo de perfil podem ser encontradas aqui: https://docs.microsoft.com/python/api/azureml-core/azureml.core.profile.modelprofile?view=azure-ml-py

## <a name="deploy-to-target"></a>Implantar no destino

### <a id="local"></a> Implantação local

Para implantar localmente, você precisa ter **Docker instalado** em seu computador local.

+ **Usando o SDK**

  ```python
  deployment_config = LocalWebservice.deploy_configuration(port=8890)
  service = Model.deploy(ws, "myservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **Usando a CLI**

  ```azurecli-interactive
  az ml model deploy -m sklearn_mnist:1 -ic inferenceconfig.json -dc deploymentconfig.json
  ```

### <a id="aci"></a> Instâncias de contêiner do Azure (desenvolvimento/teste)

Use as Instâncias de Contêiner do Azure para implantar os modelos como um serviço Web, se uma ou mais das seguintes condições forem verdadeiras:
- Você precisa implantar rapidamente e validar o modelo.
- Você está testando um modelo ainda em desenvolvimento. 

Para ver a disponibilidade de região e de cota do ACI, consulte o [cotas e disponibilidade de região para instâncias de contêiner do Azure](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) artigo.

+ **Usando o SDK**

  ```python
  deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aciservice", [model], inference_config, deployment_config)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  ```

+ **Usando a CLI**

  ```azurecli-interactive
  az ml model deploy -m sklearn_mnist:1 -n aciservice -ic inferenceconfig.json -dc deploymentconfig.json
  ```


+ **Usando o VS Code**

  Para [implantar seus modelos com o VS Code](how-to-vscode-tools.md#deploy-and-manage-models) você não precisa criar um contêiner ACI para testar com antecedência, porque os contêineres ACI são criados em tempo real.

Para obter mais informações, consulte a documentação de referência para as classes [AciWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice?view=azure-ml-py) e [Webservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.webservice?view=azure-ml-py).

### <a id="aks"></a>Serviço Kubernetes do Azure (desenvolvimento/teste e produção)

Você pode usar um cluster AKS existente ou criar outro usando o SDK do Azure Machine Learning, a CLI ou o portal do Azure.

<a id="deploy-aks"></a>

Se você já tiver um cluster do AKS anexado, você poderá implantar nele. Se você ainda não tiver criado ou anexado a um cluster do AKS, siga o processo para <a href="#create-attach-aks">criar um novo cluster do AKS</a>.

+ **Usando o SDK**

  ```python
  aks_target = AksCompute(ws,"myaks")
  # If deploying to a cluster configured for dev/test, ensure that it was created with enough
  # cores and memory to handle this deployment configuration. Note that memory is also used by
  # things such as dependencies and AML components.
  deployment_config = AksWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)
  service = Model.deploy(ws, "aksservice", [model], inference_config, deployment_config, aks_target)
  service.wait_for_deployment(show_output = True)
  print(service.state)
  print(service.get_logs())
  ```

+ **Usando a CLI**

  ```azurecli-interactive
  az ml model deploy -ct myaks -m mymodel:1 -n aksservice -ic inferenceconfig.json -dc deploymentconfig.json
  ```

+ **Usando o VS Code**

  Você também pode [implantar no AKS através da extensão do VS Code](how-to-vscode-tools.md#deploy-and-manage-models), mas você também precisará configurar clusters AKS com antecedência.

Saiba mais sobre implantação do AKS e dimensionamento automático na [AksWebservice.deploy_configuration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice) referência.

#### Criar um novo cluster do AKS<a id="create-attach-aks"></a>
**Tempo estimado**: Aproximadamente 20 minutos.

Criando ou anexando a um cluster do AKS é um momento de um processo para seu espaço de trabalho. Você pode reutilizar esse cluster para várias implantações. Se você excluir o cluster ou o grupo de recursos que o contém, você deve criar um novo cluster na próxima vez que você precisa implantar. Você pode ter vários clusters AKS anexados ao seu espaço de trabalho.

Se você quiser criar um cluster do AKS para desenvolvimento, validação e teste, você definir `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` ao usar [ `provisioning_configuration()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py). Um cluster criado com essa configuração só terá um nó.

> [!IMPORTANT]
> Definindo `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST` cria um cluster do AKS que não é adequado para tratamento do tráfego de produção. Tempos de inferência de tipos podem ser maiores do que em um cluster criado para a produção. Tolerância a falhas também não é garantida para clusters de desenvolvimento/teste.
>
> É recomendável que os clusters criados para desenvolvimento/teste usam pelo menos duas CPUs virtuais.

O exemplo a seguir demonstra como criar um novo cluster do serviço Kubernetes do Azure:

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Use the default configuration (you can also provide parameters to customize this).
# For example, to create a dev/test cluster, use:
# prov_config = AksCompute.provisioning_configuration(cluster_purpose = AksComputee.ClusterPurpose.DEV_TEST)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'myaks'
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws,
                                    name = aks_name,
                                    provisioning_configuration = prov_config)

# Wait for the create process to complete
aks_target.wait_for_completion(show_output = True)
```

Para obter mais informações sobre como criar um cluster do AKS fora do SDK do Azure Machine Learning, consulte os seguintes artigos:
* [Criar um cluster AKS](https://docs.microsoft.com/cli/azure/aks?toc=%2Fazure%2Faks%2FTOC.json&bc=%2Fazure%2Fbread%2Ftoc.json&view=azure-cli-latest#az-aks-create)
* [Criar um cluster do AKS (portal)](https://docs.microsoft.com/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest)

Para obter mais informações sobre o `cluster_purpose` parâmetro, consulte a [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py) referência.

> [!IMPORTANT]
> Para [`provisioning_configuration()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py), se você escolher valores personalizados para agent_count e vm_size, será necessário certificar-se de que agent_count multiplicado por vm_size será maior ou igual a 12 CPUs virtuais. Por exemplo, se você usar um vm_size de "Standard_D3_v2", que tenha 4 CPUs virtuais, será necessário escolher um agent_count de 3 ou maior.
>
> O SDK do Azure Machine Learning não fornece suporte ao dimensionamento de um cluster AKS. Para dimensionar os nós no cluster, use a interface do usuário para seu cluster AKS no portal do Azure. Você só pode alterar a contagem de nós, não o tamanho da VM do cluster.

#### <a name="attach-an-existing-aks-cluster"></a>Anexar a um cluster existente do AKS
**Tempo estimado:** Aproximadamente 5 minutos.

Se você já tiver o cluster do AKS em sua assinatura do Azure, e é a versão 1.12. # #, você pode usá-lo para implantar sua imagem.

> [!WARNING]
> Ao anexar a um cluster do AKS para um espaço de trabalho, você pode definir como você usará o cluster, definindo o `cluster_purpose` parâmetro.
>
> Se você não definir a `cluster_purpose` parâmetro, ou conjunto `cluster_purpose = AksCompute.ClusterPurpose.FAST_PROD`, em seguida, o cluster deve ter pelo menos 12 CPUs virtuais disponíveis.
>
> Se você definir `cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST`, em seguida, o cluster não precisa ter 12 CPUs virtuais. No entanto, um cluster que está configurado para desenvolvimento/teste não será adequado para o nível de tráfego de produção e pode aumentar o tempo de inferência de tipos.

O código a seguir demonstra como anexar um 1.12 existente do AKS. # # cluster ao espaço de trabalho:

```python
from azureml.core.compute import AksCompute, ComputeTarget
# Set the resource group that contains the AKS cluster and the cluster name
resource_group = 'myresourcegroup'
cluster_name = 'mycluster'

# Attach the cluster to your workgroup. If the cluster has less than 12 virtual CPUs, use the following instead:
# attach_config = AksCompute.attach_configuration(resource_group = resource_group,
#                                         cluster_name = cluster_name,
#                                         cluster_purpose = AksCompute.ClusterPurpose.DEV_TEST)
attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
aks_target = ComputeTarget.attach(ws, 'mycompute', attach_config)
```

Para obter mais informações sobre `attack_configuration()`, consulte o [AksCompute.attach_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-) referência.

Para obter mais informações sobre o `cluster_purpose` parâmetro, consulte a [AksCompute.ClusterPurpose](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.akscompute.clusterpurpose?view=azure-ml-py) referência.

## <a name="consume-web-services"></a>Consumir serviços Web

Cada serviço web implantado fornece uma API REST, portanto, você pode criar aplicativos de cliente em uma variedade de linguagens de programação. Se você tiver habilitado a autenticação para seu serviço, você precisa fornecer uma chave de serviço como um token em seu cabeçalho de solicitação.

### <a name="request-response-consumption"></a>Consumo de solicitação-resposta

Aqui está um exemplo de como chamar o serviço no Python:
```python
import requests
import json

headers = {'Content-Type':'application/json'}

if service.auth_enabled:
    headers['Authorization'] = 'Bearer '+service.get_keys()[0]

print(headers)
    
test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})

response = requests.post(service.scoring_uri, data=test_sample, headers=headers)
print(response.status_code)
print(response.elapsed)
print(response.json())
```

Para obter mais informações, confira [Criar aplicativos cliente para consumir serviços Web](how-to-consume-web-service.md).


### <a id="azuremlcompute"></a> Inferência de tipos de lote
Destinos de computação do Machine Learning do Azure são criados e gerenciados pelo serviço de Azure Machine Learning. Eles podem ser usados para previsão de lote de Pipelines de Machine Learning do Azure.

Para obter uma explicação de inferência de lote com a computação do Azure Machine Learning, leia as [como executar previsões de lote](how-to-run-batch-predictions.md) artigo.

### <a id="iotedge"></a> Inferência de tipos do IoT Edge
Suporte à implantação para a borda está em visualização. Para obter mais informações, consulte o [implantar o Azure Machine Learning como um módulo IoT Edge](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning) artigo.


## <a id="update"></a> Atualize o web services

Quando você cria um novo modelo, você deve atualizar manualmente cada serviço que você deseja usar o novo modelo. Para atualizar o serviço Web, use o método `update`. O código a seguir demonstra como atualizar o serviço web para usar um novo modelo:

```python
from azureml.core.webservice import Webservice
from azureml.core.model import Model

# register new model
new_model = Model.register(model_path = "outputs/sklearn_mnist_model.pkl",
                       model_name = "sklearn_mnist",
                       tags = {"key": "0.1"},
                       description = "test",
                       workspace = ws)

service_name = 'myservice'
# Retrieve existing service
service = Webservice(name = service_name, workspace = ws)

# Update to new model(s)
service.update(models = [new_model])
print(service.state)
print(service.get_logs())
```

## <a name="continuous-model-deployment"></a>Implantação de modelo contínuo 

Você pode implantar modelos usando a extensão de aprendizado de máquina para continuamente [DevOps do Azure](https://azure.microsoft.com/services/devops/). Ao usar a extensão de aprendizado de máquina para DevOps do Azure, você pode disparar um pipeline de implantação quando um novo modelo de aprendizado de máquina é registrado no espaço de trabalho de serviço de Azure Machine Learning. 

1. Inscreva-se [Pipelines do Azure](https://docs.microsoft.com/azure/devops/pipelines/get-started/pipelines-sign-up?view=azure-devops), que possibilita a integração contínua e entrega do seu aplicativo/qualquer para qualquer plataforma de nuvem. Pipelines do Azure [difere de pipelines do ML](concept-ml-pipelines.md#compare). 

1. [Crie um projeto de DevOps do Azure.](https://docs.microsoft.com/azure/devops/organizations/projects/create-project?view=azure-devops)

1. Instalar o [extensão de aprendizado de máquina para os Pipelines do Azure](https://marketplace.visualstudio.com/items?itemName=ms-air-aiagility.vss-services-azureml&targetId=6756afbe-7032-4a36-9cb6-2771710cadc2&utm_source=vstsproduct&utm_medium=ExtHubManageList) 

1. Use __conexões de serviço__ para configurar uma conexão de entidade de serviço para seu espaço de trabalho do serviço de Azure Machine Learning para acessar todos os artefatos. Vá para configurações do projeto, clique em conexões de serviço e selecione Gerenciador de recursos do Azure.

    ![view-service-connection](media/how-to-deploy-and-where/view-service-connection.png) 

1. Definir AzureMLWorkspace como o __nível de escopo__ e preencha os parâmetros subsequentes.

    ![view-azure-resource-manager](media/how-to-deploy-and-where/resource-manager-connection.png)

1. Em seguida, para implantar continuamente seu modelo de aprendizado de máquina usando os Pipelines do Azure, em pipelines marque __release__. Adicionar um novo artefato, selecione o artefato de modelo do AzureML e a conexão de serviço que foi criado na etapa anterior. Selecione o modelo e versão para disparar uma implantação. 

    ![select-AzureMLmodel-artifact](media/how-to-deploy-and-where/enable-modeltrigger-artifact.png)

1. Habilite o gatilho do modelo em seu artefato de modelo. Ativando o gatilho, sempre que a versão especificada (ou seja a versão mais recente) desse modelo é o registro em seu espaço de trabalho, um pipeline de lançamento de DevOps do Azure é disparado. 

    ![enable-model-trigger](media/how-to-deploy-and-where/set-modeltrigger.png)

Para projetos de exemplo e exemplos, fazer check-out [repositório MLOps](https://github.com/Microsoft/MLOps)

## <a name="clean-up-resources"></a>Limpar recursos
Para excluir um serviço Web implantado, use `service.delete()`.
Para excluir um modelo registrado, use `model.delete()`.

Para obter mais informações, consulte a documentação de referência [WebService.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#delete--), e [Model.delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#delete--).

## <a name="next-steps"></a>Próximas etapas
* [Como implantar um modelo usando uma imagem personalizada do Docker](how-to-deploy-custom-docker-image.md)
* [Solução de problemas de implantação](how-to-troubleshoot-deployment.md)
* [Proteger serviços Web do Azure Machine Learning com SSL](how-to-secure-web-service.md)
* [Consumir um modelo de ML implantado como um serviço Web](how-to-consume-web-service.md)
* [Monitorar seus modelos do Azure Machine Learning com o Application Insights](how-to-enable-app-insights.md)
* [Coletar dados para modelos em produção](how-to-enable-data-collection.md)

