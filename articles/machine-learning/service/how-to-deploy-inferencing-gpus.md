---
title: Implantar um modelo para inferência de tipos com GPU
titleSuffix: Azure Machine Learning service
description: Este artigo ensina a usar o serviço de Azure Machine Learning para implantar um modelo como um web service.service e solicitações de inferência de tipos de pontuação de aprendizado profundo de Tensorflow habilitadas para GPU.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: csteegz
ms.reviewer: larryfr
ms.date: 06/01/2019
ms.openlocfilehash: 8086d059913cc61bff0bca31681368bea6d76777
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543795"
---
# <a name="deploy-a-deep-learning-model-for-inference-with-gpu"></a>Implantar um modelo de aprendizado profundo para inferência de tipos com GPU

Este artigo ensina a usar o serviço de Azure Machine Learning para implantar um modelo como um serviço web de aprendizado profundo de Tensorflow habilitadas para GPU.

Implante o modelo para um cluster do serviço de Kubernetes do Azure (AKS) fazer inferência habilitadas para GPU. Inferência ou modelo de pontuação, é a fase em que o modelo implantado é usado para previsão. Usando GPUs em vez de CPUs oferecem vantagens de desempenho em computação altamente paralelizável.

Embora este exemplo usa um modelo do TensorFlow, você pode aplicar as etapas a seguir para qualquer estrutura que dá suporte a GPUs fazendo pequenas alterações para o arquivo de pontuação e o arquivo do ambiente de aprendizado de máquina. 

Neste artigo, você executa as seguintes etapas:

* Criar um cluster AKS habilitadas para GPU
* Implantar um modelo do Tensorflow GPU
* Emitir uma consulta de exemplo para seu modelo implantado

## <a name="prerequisites"></a>Pré-requisitos

* Um espaço de trabalho de serviços do Azure Machine Learning.
* Uma distribuição do Python.
* Um Tensorflow registrado salvo o modelo.
    * Para saber como registrar modelos, consulte [implantar modelos](../service/how-to-deploy-and-where.md#registermodel).

Você pode concluir a primeira parte desta série de instruções [como treinar um modelo do TensorFlow](how-to-train-tensorflow.md), para cumprir os pré-requisitos necessários.

## <a name="provision-an-aks-cluster-with-gpus"></a>Provisionar um cluster do AKS com GPUs

O Azure tem várias opções diferentes de GPU. Você pode usar qualquer um deles para inferência. Ver [a lista de VMs da série N](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#n-series) para obter uma análise completa de recursos e os custos.

Para obter mais informações sobre como usar o AKS com o serviço Azure Machine Learning, consulte [como implantar e onde](../service/how-to-deploy-and-where.md#deploy-aks).

```Python
# Choose a name for your cluster
aks_name = "aks-gpu"

# Check to see if the cluster already exists
try:
    compute_target = ComputeTarget(workspace=ws, name=aks_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    # Provision AKS cluster with GPU machine
    prov_config = AksCompute.provisioning_configuration(vm_size="Standard_NC6")

    # Create the cluster
    aks_target = ComputeTarget.create(
        workspace=ws, name=aks_name, provisioning_configuration=prov_config
    )

    aks_target.wait_for_completion(show_output=True)
```

> [!IMPORTANT]
> Azure cobrará desde que o cluster do AKS é provisionado. Certifique-se de excluir o cluster do AKS quando tiver terminado com ele.

## <a name="write-the-entry-script"></a>Gravar o script de entrada

Salve o código a seguir em seu diretório de trabalho como `score.py`. Esse arquivo pontuações de imagens que são enviadas ao seu serviço. Carrega o modelo do TensorFlow salvo, passa a imagem de entrada para a sessão de TensorFlow em cada solicitação de POSTAGEM e, em seguida, retorna as pontuações resultantes. Outras estruturas de inferência exigem diferentes arquivos de pontuação.

```python
import json
import numpy as np
import os
import tensorflow as tf

from azureml.core.model import Model

def init():
    global X, output, sess
    tf.reset_default_graph()
    model_root = Model.get_model_path('tf-dnn-mnist')
    saver = tf.train.import_meta_graph(os.path.join(model_root, 'mnist-tf.model.meta'))
    X = tf.get_default_graph().get_tensor_by_name("network/X:0")
    output = tf.get_default_graph().get_tensor_by_name("network/output/MatMul:0")
    
    sess = tf.Session()
    saver.restore(sess, os.path.join(model_root, 'mnist-tf.model'))

def run(raw_data):
    data = np.array(json.loads(raw_data)['data'])
    # make prediction
    out = output.eval(session=sess, feed_dict={X: data})
    y_hat = np.argmax(out, axis=1)
    return y_hat.tolist()

```
## <a name="define-the-conda-environment"></a>Definir o ambiente do conda

Crie um arquivo de ambiente do conda chamado `myenv.yml` para especificar as dependências para o seu serviço. É importante especificar que você esteja usando `tensorflow-gpu` para alcançar desempenho acelerado.

```yaml
name: project_environment
dependencies:
  # The python interpreter version.
  # Currently Azure ML only supports 3.5.2 and later.
- python=3.6.2

- pip:
  - azureml-defaults==1.0.43.*
- numpy
- tensorflow-gpu=1.12
channels:
- conda-forge
```

## <a name="define-the-gpu-inferenceconfig-class"></a>Definir a classe InferenceConfig de GPU

Criar um `InferenceConfig` objeto que permite que as GPUs e garante que o CUDA é instalado com a imagem do Docker.

```python
from azureml.core.model import Model
from azureml.core.model import InferenceConfig

aks_service_name ='aks-dnn-mnist'
gpu_aks_config = AksWebservice.deploy_configuration(autoscale_enabled = False, 
                                                    num_replicas = 3, 
                                                    cpu_cores=2, 
                                                    memory_gb=4)
model = Model(ws,"tf-dnn-mnist")

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml", 
                                   enable_gpu=True)
```

Para obter mais informações, consulte:

- [Classe InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py)
- [Classe AksServiceDeploymentConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aks.aksservicedeploymentconfiguration?view=azure-ml-py)

## <a name="deploy-the-model"></a>Implantar o modelo

Implantar o modelo em seu cluster do AKS e aguarde a criação do seu serviço.

```python
aks_service = Model.deploy(ws,
                           models=[model],
                           inference_config=inference_config, 
                           deployment_config=gpu_aks_config,
                           deployment_target=aks_target,
                           name=aks_service_name)

aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
```

> [!NOTE]
> O serviço do Azure Machine Learning não implantar um modelo com um `InferenceConfig` objeto que espera GPU esteja habilitado para um cluster que não tem uma GPU.

Para obter mais informações, consulte [classe de modelo](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py).

## <a name="issue-a-sample-query-to-your-model"></a>Emitir uma consulta de exemplo para seu modelo

Envie uma consulta de teste para o modelo implantado. Quando você envia uma imagem jpeg para o modelo, pontuações de imagem. O exemplo de código a seguir usa uma função de utilitário externas para carregar imagens. Você pode encontrar o código relevante no pir [exemplo de TensorFlow no GitHub](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow/utils.py). 

```python
# Used to test your webservice
from utils import load_data 

# Load test data from model training
X_test = load_data('./data/mnist/test-images.gz', False) / 255.0
y_test = load_data('./data/mnist/test-labels.gz', True).reshape(-1)

# send a random row from the test set to score
random_index = np.random.randint(0, len(X_test)-1)
input_data = "{\"data\": [" + str(list(X_test[random_index])) + "]}"

api_key = aks_service.get_keys()[0]
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}
resp = requests.post(aks_service.scoring_uri, input_data, headers=headers)

print("POST to url", aks_service.scoring_uri)
#print("input data:", input_data)
print("label:", y_test[random_index])
print("prediction:", resp.text)
```

> [!IMPORTANT]
> Para minimizar a latência e otimizar a taxa de transferência, verifique se que o cliente está na mesma região do Azure como o ponto de extremidade. Neste exemplo, as APIs são criadas na região Leste dos EUA do Azure.

## <a name="clean-up-the-resources"></a>Limpar todos os recursos

Exclua os recursos depois que você terminar de usar este exemplo.

> [!IMPORTANT]
> Faturas do Azure com base em quanto tempo o cluster do AKS é implantado. Certifique-se para limpá-los depois que terminar com ele.

```python
aks_service.delete()
aks_target.delete()
```

## <a name="next-steps"></a>Próximas etapas

* [Implantar o modelo de FPGA](../service/how-to-deploy-fpga-web-service.md)
* [Implantar o modelo com ONNX](../service/concept-onnx.md#deploy-onnx-models-in-azure)
* [Treinar modelos de DNN Tensorflow](../service/how-to-train-tensorflow.md)
