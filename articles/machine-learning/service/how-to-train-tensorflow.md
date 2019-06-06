---
title: Treinar e registrar os modelos de TensorFlow
titleSuffix: Azure Machine Learning service
description: Este artigo mostra como treinar e registrar um modelo do TensorFlow usando o serviço de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.date: 05/28/2019
ms.custom: seodec18
ms.openlocfilehash: 4a6f9734a7b2b59035efcbb0f4e2d75f47e053be
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66515606"
---
# <a name="train-and-register-tensorflow-models-at-scale-with-azure-machine-learning-service"></a>Treinar e registrar os modelos de TensorFlow em escala com serviço de Azure Machine Learning

Este artigo mostra como treinar e registrar um modelo do TensorFlow usando o serviço de Azure Machine Learning. Estaremos usando o popular [conjunto de dados MNIST](http://yann.lecun.com/exdb/mnist/) para classificar os dígitos manuscritos usando uma rede neural profunda criada usando o [biblioteca Python de TensorFlow](https://www.tensorflow.org/overview).

Com o serviço de Azure Machine Learning, você poderá expandir rapidamente seus trabalhos de treinamento do código-fonte aberto usando recursos de computação de nuvem elásticos. Você também poderá acompanhar suas execuções de treinamento, modelos de versão, implante modelos e muito mais.

Se você estiver desenvolvendo um modelo do TensorFlow desde o início ou está trazendo um modelo existente para a nuvem, você pode criar modelos prontos para produção com o serviço Azure Machine Learning.

## <a name="prerequisites"></a>Pré-requisitos

- Instalar o [do Azure Machine Learning do SDK para Python](setup-create-workspace.md#sdk). Opcional: criar um `config.json` arquivo de configuração.
- Baixe o [arquivos de script de exemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow) `mnist-tf.py` e `utils.py`

Você também pode encontrar um concluídos [versão do bloco de anotações do Jupyter](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow/train-hyperparameter-tune-deploy-with-tensorflow.ipynb) deste guia na nossa página de amostras do Github. O bloco de anotações inclui expandidas seções que abrangem o ajuste de hiperparâmetro inteligente e implantação de modelo.

## <a name="set-up-the-experiment"></a>Configure o experimento

Esta seção define o teste de treinamento por carregar os pacotes python necessários, inicializando um espaço de trabalho, criação de um experimento e carregar os dados de treinamento e scripts de treinamento usando o SDK do Python.

### <a name="import-packages"></a>Importar pacotes

Primeiro, é necessário importar as bibliotecas necessárias do Python.

```Python
import os
import urllib
import shutil
import azureml

from azureml.core import Experiment
from azureml.core import Workspace, Run

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Inicializar um espaço de trabalho

O [espaço de trabalho de serviço de Azure Machine Learning](concept-workspace.md) é o recurso de nível superior para o serviço. Ele fornece um local centralizado para trabalhar com todos os artefatos que você cria. No SDK do Python, você pode acessar os artefatos de espaço de trabalho com a criação de um [ `workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) objeto.

Se você concluiu a etapa opcional de [seção pré-requisitos](#prerequisites), você pode usar `Workspace.from_config()` para criar rapidamente um objeto de espaço de trabalho de detalhes armazenados no arquivo de configuração.

```Python
ws = Workspace.from_config()
```

Você também pode criar explicitamente um espaço de trabalho:

```Python
ws = Workspace.create(name='<workspace-name>',
                      subscription_id='<azure-subscription-id>',
                      resource_group='<choose-a-resource-group>',
                      create_resource_group=True,
                      location='<select-location>' # For example: 'eastus2'
                      )
```

### <a name="create-an-experiment"></a>Criar uma experiência

Crie um experimento e uma pasta para armazenar seus scripts de treinamento. Neste exemplo, crie um teste chamado "tf-mnist".

```Python
script_folder = './tf-mnist'
os.makedirs(script_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='tf-mnist')
```

### <a name="upload-dataset-and-scripts"></a>Carregar o conjunto de dados e scripts

O [datastore](how-to-access-data.md) é um lugar onde os dados podem ser armazenados e acessados pela montagem ou copiar os dados para o destino de computação. Cada espaço de trabalho fornece um repositório de dados padrão. Nós carregaremos nossos dados e scripts de treinamento para que eles possam ser facilmente acessados durante o treinamento.

1. Baixe o conjunto de dados MNIST localmente.

    ```Python
    os.makedirs('./data/mnist', exist_ok=True)

    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename = './data/mnist/train-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename = './data/mnist/train-labels.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename = './data/mnist/test-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename = './data/mnist/test-labels.gz')
    ```

1. Carregue o conjunto de dados MNIST no repositório de dados padrão.

    ```Python
    ds = ws.get_default_datastore()
    ds.upload(src_dir='./data/mnist', target_path='mnist', overwrite=True, show_progress=True)
    ```

1. Carregar o script de treinamento de TensorFlow `tf_mnist.py`e o arquivo auxiliar, `utils.py`.

    ```Python
    shutil.copy('./tf_mnist.py', script_folder)
    shutil.copy('./utils.py', script_folder)
    ```

## <a name="create-a-compute-target"></a>Criar um destino de computação

Crie um destino de computação para seu trabalho de TensorFlow ser executado no. Neste exemplo, podemos criar um cluster de computação do Azure Machine Learning habilitada para GPU. Para obter uma lista de treinamento disponíveis destinos de computação, consulte [neste artigo](how-to-set-up-training-targets.md#compute-targets-for-training)

```Python
cluster_name = "gpucluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

## <a name="create-a-tensorflow-estimator"></a>Criar um estimador de TensorFlow

O [TensorFlow estimador](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) fornece uma maneira simples de iniciar um trabalho de treinamento de TensorFlow em um destino de computação. Ele criará uma imagem do docker com o TensorFlow instalado.

O avaliador de TensorFlow é implementado por meio de genéricos [ `estimator` ](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) classe, que pode ser usado para dar suporte a qualquer estrutura. Para obter mais informações sobre o treinamento de modelos usando o avaliador de genérico, consulte [treinar modelos com o Azure Machine Learning usando o Vistoriador](how-to-train-ml-models.md)

Se precisar de seu script de treinamento adicional pip ou conda pacotes para serem executados, você pode ter os pacotes instalados na imagem resultante do docker, passando nomes através de `pip_packages` e `conda_packages` argumentos.

```Python
script_params = {
    '--data-folder': ws.get_default_datastore().as_mount(),
    '--batch-size': 50,
    '--first-layer-neurons': 300,
    '--second-layer-neurons': 100,
    '--learning-rate': 0.01
}

est = TensorFlow(source_directory=script_folder,
                 entry_script='tf_mnist.py',
                 script_params=script_params,
                 compute_target=compute_target,
                 use_gpu=True)
```

## <a name="submit-a-run"></a>Enviar uma execução

O [executar objeto](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) fornece a interface para o histórico de execução, enquanto o trabalho está em execução e após sua conclusão.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Como a execução é executada, ele passará por estágios a seguir:

- **Preparando**: Uma imagem do docker é criada de acordo com o avaliador de TensorFlow. A imagem é carregada no registro de contêiner do espaço de trabalho e armazenados em cache para as execuções posteriores. Os logs também são transmitidos para o histórico de execução e podem ser exibidos para monitorar o progresso.

- **Dimensionamento**: O cluster tentará escalar verticalmente se o cluster de IA do lote requer mais nós para a execução que estão disponíveis no momento.

- **Executando**: Todos os scripts na pasta de script são carregados para o destino de computação, armazenamentos de dados sejam montados ou copiados e o entry_script é executado. Saídas de stdout e o. / pasta logs serão transmitidos para o histórico de execução e pode ser usado para monitorar a execução.

- **Pós-processamento**: A. / saídas de pasta da execução é copiada para o histórico de execução.

## <a name="register-or-download-a-model"></a>Registrar ou fazer o download de um modelo

Depois que você treinou o modelo, você pode registrá-lo ao seu espaço de trabalho. Registro de modelo permite que você store e a versão de seus modelos em seu espaço de trabalho para simplificar [gerenciamento e implantação de modelo](concept-model-management-and-deployment.md).

```Python
model = run.register_model(model_name='tf-dnn-mnist', model_path='outputs/model')
```

Você também pode baixar uma cópia local do modelo usando o objeto de execução. No script de treinamento `mnist-tf.py`, um objeto de proteção de TensorFlow persiste o modelo em uma pasta local (local para o destino de computação). Podemos usar o objeto de execução para baixar uma cópia.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="distributed-training"></a>Treinamento distribuído

O [ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) estimador também dá suporte a treinamento distribuído entre os clusters de CPU e GPU. Você pode facilmente executar trabalhos do TensorFlow distribuídos e serviço Azure Machine Learning gerenciará a orquestração para você.

O serviço do Azure Machine Learning dá suporte a dois métodos de treinamento distribuído TensorFlow:

- [Com base em MPI](https://www.open-mpi.org/) distribuídos treinamento usando o [Horovod](https://github.com/uber/horovod) framework
- Native [distribuídos TensorFlow](https://www.tensorflow.org/deploy/distributed) usando o método de servidor de parâmetro

### <a name="horovod"></a>Horovod

[Horovod](https://github.com/uber/horovod) é uma estrutura de código-fonte aberto para o treinamento distribuído desenvolvido pela Uber. Ele oferece um caminho fácil para os trabalhos de GPU TensorFlow distribuídos.

Para usar Horovod, especifique `mpi` para o `distributed_training` parâmetro no construtor de estimador de TensorFlow. Horovod será instalado para que você possa usar em seu script de treinamento.

```Python
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
estimator= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=MpiConfiguration(),
                      framework_version='1.13',
                      use_gpu=True)
```

### <a name="parameter-server"></a>Servidor de parâmetros

Também é possível executar o [TensorFlow distribuído nativo](https://www.tensorflow.org/deploy/distributed), que usa o modelo de servidor de parâmetros. Nesse método, é preciso treinar em um cluster de servidores de parâmetro e trabalhos. Os trabalhos calculam os gradientes durante o treinamento, enquanto os servidores de parâmetros agregam os gradientes.

Para usar o método de servidor de parâmetro, especifique `ps` para o `distributed_training` parâmetro no construtor de estimador de TensorFlow.

```Python
from azureml.train.dnn import TensorFlow

distributed_training = TensorflowConfiguration()
distributed_training.worker_count = 2

# Tensorflow constructor
estimator= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_backend=distributed_training,
                      use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

#### <a name="define-cluster-specifications-in-tfconfig"></a>Definir as especificações de cluster no 'TF_CONFIG'

Você também precisará os endereços de rede e portas do cluster para o [ `tf.train.ClusterSpec` ](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec), portanto, o Azure Machine Learning define o `TF_CONFIG` variável de ambiente para você.

A variável de ambiente `TF_CONFIG` é uma cadeia de caracteres JSON. Confira um exemplo da variável para um servidor de parâmetros:

```JSON
TF_CONFIG='{
    "cluster": {
        "ps": ["host0:2222", "host1:2222"],
        "worker": ["host2:2222", "host3:2222", "host4:2222"],
    },
    "task": {"type": "ps", "index": 0},
    "environment": "cloud"
}'
```

Para o nível de alta do TensorFlow [ `tf.estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator) API, o TensorFlow irá analisar isso `TF_CONFIG` especificação da variável e o cluster de compilação para você.

Para nível inferior APIs centrais do TensorFlow para treinamento, analisar os `TF_CONFIG` variável e compilação os `tf.train.ClusterSpec` em seu código de treinamento.

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

## <a name="next-steps"></a>Próximas etapas

Neste artigo, você treinado e registrou um modelo do TensorFlow no serviço do Azure Machine Learning. Para saber como implantar um modelo, continue com o nosso artigo de implantação de modelo.

> [!div class="nextstepaction"]
> [Como e onde implantar modelos](how-to-deploy-and-where.md)
