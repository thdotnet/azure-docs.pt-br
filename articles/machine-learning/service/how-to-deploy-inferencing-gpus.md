---
title: Implantar um modelo para inferência de tipos com GPU
titleSuffix: Azure Machine Learning service
description: Saiba como implantar um modelo de aprendizado profundo como um serviço web que usa uma GPU para inferência de tipos. Neste artigo, um modelo do Tensorflow é implantado em um cluster do serviço de Kubernetes do Azure. O cluster usa uma VM habilitada para GPU para hospedar o serviço web e solicitações de inferência de tipos de pontuação.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: vaidyas
author: csteegz
ms.reviewer: larryfr
ms.date: 05/02/2019
ms.openlocfilehash: ec71165553a1d65ff133d605bf94255100f74e6e
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66388916"
---
# <a name="deploy-a-deep-learning-model-for-inference-with-gpu"></a>Implantar um modelo de aprendizado profundo para inferência de tipos com GPU

Saiba como usar a inferência GPU para um modelo implantado como um serviço web de aprendizado de máquina. Inferência de tipos ou modelo de pontuação, é a fase em que o modelo implantado é usado para previsão, mais comumente em dados de produção.

Este artigo ensina a usar o serviço de Azure Machine Learning para implantar um modelo para um cluster do serviço de Kubernetes do Azure (AKS) em uma máquina de virtual (VM) habilitadas para GPU de aprendizado profundo de Tensorflow do exemplo. Quando as solicitações são enviadas para o serviço, o modelo usa a GPU para executar as cargas de trabalho de inferência de tipos.

GPUs oferecem vantagens de desempenho sobre CPUs na computação altamente paralelizável. Casos de uso excelente para VMs habilitadas para GPU incluem aprendizado profundo e Inferência de tipos, especialmente para grandes lotes de solicitações de treinamento do modelo.

Este exemplo demonstra como implantar um TensorFlow Salvar modelo do Azure Machine Learning. Siga estas etapas:

* Criar um cluster AKS habilitadas para GPU
* Implantar um modelo do Tensorflow GPU

## <a name="prerequisites"></a>Pré-requisitos

* Um espaço de trabalho de serviços do Azure Machine Learning
* Uma distribuição do Python
* Um Tensorflow registrado salvo o modelo. Para saber como registrar modelos, consulte [implantar modelos](../service/how-to-deploy-and-where.md#registermodel).

Este artigo se baseia o bloco de anotações do Jupyter [implantando modelos de Tensorflow no AKS](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/production-deploy-to-aks-gpu/production-deploy-to-aks-gpu.ipynb). O bloco de anotações do Jupyter usa TensorFlow salva modelos e implanta-os em um cluster do AKS. Você também pode aplicar o bloco de anotações a qualquer estrutura que dá suporte a GPUs fazendo pequenas alterações para o arquivo de pontuação e o arquivo do ambiente de aprendizado de máquina.  

## <a name="provision-an-aks-cluster-with-gpus"></a>Provisionar um cluster do AKS com GPUs

O Azure tem várias opções diferentes de GPU. Você pode usar qualquer um deles para inferência. Ver [a lista de VMs da série N](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#n-series) para obter uma análise completa de recursos e os custos.

Para obter mais informações sobre como usar o AKS com o serviço Azure Machine Learning, consulte [como implantar e onde](../service/how-to-deploy-and-where.md#deploy-aks).

```python
# Provision AKS cluster with GPU machine
prov_config = AksCompute.provisioning_configuration(vm_size="Standard_NC6")

# Create the cluster
aks_target = ComputeTarget.create(
    workspace=ws, name=aks_name, provisioning_configuration=prov_config
)

aks_target.wait_for_deployment()
```

> [!IMPORTANT]
> Azure cobrará desde que o cluster do AKS é provisionado. Certifique-se de excluir o cluster do AKS quando tiver terminado com ele.

## <a name="write-the-entry-script"></a>Gravar o script de entrada

Salve o código a seguir em seu diretório de trabalho como `score.py`. Esse arquivo pontuações de imagens que são enviadas ao seu serviço. Carrega o modelo do TensorFlow salvo, passa a imagem de entrada para a sessão de TensorFlow em cada solicitação de POSTAGEM e, em seguida, retorna as pontuações resultantes. Outras estruturas de inferência exigem diferentes arquivos de pontuação.

```python
import tensorflow as tf
import numpy as np
import ujson
from azureml.core.model import Model
from azureml.contrib.services.aml_request import AMLRequest, rawhttp
from azureml.contrib.services.aml_response import AMLResponse

def init():
    global session
    global input_name
    global output_name
    
    session = tf.Session()

    model_path = Model.get_model_path('resnet50')
    model = tf.saved_model.loader.load(session, ['serve'], model_path)
    if len(model.signature_def['serving_default'].inputs) > 1:
        raise ValueError("This score.py only supports one input")
    input_name = [tensor.name for tensor in model.signature_def['serving_default'].inputs.values()][0]
    output_name = [tensor.name for tensor in model.signature_def['serving_default'].outputs.values()]
    

@rawhttp
def run(request):
    if request.method == 'POST':
        reqBody = request.get_data(False)
        resp = score(reqBody)
        return AMLResponse(resp, 200)
    if request.method == 'GET':
        respBody = str.encode("GET is not supported")
        return AMLResponse(respBody, 405)
    return AMLResponse("bad request", 500)

def score(data):
    result = session.run(output_name, {input_name: [data]})
    return ujson.dumps(result[1])

if __name__ == "__main__":
    init()
    with open("lynx.jpg", 'rb') as f: #load file for testing locally
        content = f.read()
        print(score(content))

```

## <a name="define-the-conda-environment"></a>Definir o ambiente do conda

Crie um arquivo de ambiente do conda chamado `myenv.yml` para especificar as dependências para o seu serviço. É importante especificar que você esteja usando `tensorflow-gpu` para alcançar desempenho acelerado.

```yaml
name: aml-accel-perf
channels:
  - defaults
dependencies:
  - tensorflow-gpu = 1.12
  - numpy
  - ujson
  - pip:
    - azureml-core
    - azureml-contrib-services
```

## <a name="define-the-gpu-inferenceconfig-class"></a>Definir a classe InferenceConfig de GPU

Criar um `InferenceConfig` objeto que permite que as GPUs e garante que o CUDA é instalado com a imagem do Docker.

```python
from azureml.core.model import Model
from azureml.core.model import InferenceConfig

aks_service_name ='gpu-rn'
gpu_aks_config = AksWebservice.deploy_configuration(autoscale_enabled = False, 
                                                    num_replicas = 3, 
                                                    cpu_cores=2, 
                                                    memory_gb=4)
model = Model(ws,"resnet50")

inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml", 
                                   gpu_enabled=True)
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

## <a name="issue-a-sample-query-to-your-deployed-model"></a>Emitir uma consulta de exemplo para seu modelo implantado

Envie uma consulta de teste para o modelo implantado. Quando você envia uma imagem jpeg para o modelo, pontuações de imagem.

```python
scoring_url = aks_service.scoring_uri
api_key = aks_service.get_key()(0)
IMAGEURL = "https://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Lynx_lynx_poing.jpg/220px-Lynx_lynx_poing.jpg"

headers = {'Authorization':('Bearer '+ api_key)}
img_data = read_image_from(IMAGEURL).read()
r = requests.post(scoring_url, data = img_data, headers=headers)
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
