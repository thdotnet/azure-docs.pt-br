---
title: Guia de solução de problemas de implantação
titleSuffix: Azure Machine Learning service
description: Saiba como contornar, resolver e solucionar erros comuns de implantação do Docker com o serviço Kubernetes do Azure e instâncias de contêiner do Azure usando o serviço de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: chris-lauren
ms.author: clauren
ms.reviewer: jmartens
ms.date: 07/09/2018
ms.custom: seodec18
ms.openlocfilehash: e0f4b024d717c08df3514df057abf89d55be1dc9
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707031"
---
# <a name="troubleshooting-azure-machine-learning-service-azure-kubernetes-service-and-azure-container-instances-deployment"></a>Solucionando problemas de implantação de serviço Kubernetes do Azure e instâncias de contêiner do Azure do serviço de Azure Machine Learning

Aprenda a contornar ou resolver erros comuns de implantação do Docker com instâncias de contêiner do Azure (ACI) e o Azure o AKS (serviço) usando o serviço de Azure Machine Learning.

Ao implantar um modelo de serviço do Azure Machine Learning, o sistema executa uma série de tarefas. As tarefas de implantação são:

1. Registre o modelo no Registro de modelo do workspace.

2. Crie uma imagem do Docker, incluindo:
    1. Baixe o modelo registrado do Registro. 
    2. Crie um dockerfile, com um ambiente Python baseado nas dependências especificadas no arquivo yaml do ambiente.
    3. Adicione seus arquivos de modelo e o script de pontuação que você fornece no dockerfile.
    4. Crie uma nova imagem do Docker usando o dockerfile.
    5. Registre a imagem do Docker com o Registro de Contêiner do Azure associado com o workspace.

    > [!IMPORTANT]
    > Dependendo do seu código, criação de imagem ocorrem automaticamente sem a sua entrada.

3. Implante a imagem do Docker para o serviço ACI (Instância de Contêiner do Azure) ou o AKS (Serviço de Kubernetes do Azure).

4. Inicialize um novo contêiner (ou contêineres) em ACI ou AKS. 

Saiba mais sobre esse processo na introdução a [Gerenciamento de Modelos](concept-model-management-and-deployment.md).

## <a name="before-you-begin"></a>Antes de começar

Se você tiver qualquer problema, a primeira medida a adotar é dividir a tarefa de implantação (descrita anteriormente) em etapas individuais para isolar o problema.

Dividir a implantação em tarefas é útil se você estiver usando o [Webservice.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-) API, ou [Webservice.deploy_from_model()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-) API, como ambas as funções, execute as etapas mencionadas anteriormente como um única ação. Normalmente, essas APIs são convenientes, mas é útil para dividir as etapas na solução de problemas, substituindo-os com o abaixo de chamadas à API.

1. Registre o modelo. Aqui está um exemplo de código:

    ```python
    # register a model out of a run record
    model = best_run.register_model(model_name='my_best_model', model_path='outputs/my_model.pkl')

    # or, you can register a file or a folder of files as a model
    model = Model.register(model_path='my_model.pkl', model_name='my_best_model', workspace=ws)
    ```

2. Crie a imagem. Aqui está um exemplo de código:

    ```python
    # configure the image
    image_config = ContainerImage.image_configuration(runtime="python",
                                                      entry_script="score.py",
                                                      conda_file="myenv.yml")

    # create the image
    image = Image.create(name='myimg', models=[model], image_config=image_config, workspace=ws)

    # wait for image creation to finish
    image.wait_for_creation(show_output=True)
    ```

3. Implante a imagem como serviço. Aqui está um exemplo de código:

    ```python
    # configure an ACI-based deployment
    aci_config = AciWebservice.deploy_configuration(cpu_cores=1, memory_gb=1)

    aci_service = Webservice.deploy_from_image(deployment_config=aci_config, 
                                               image=image, 
                                               name='mysvc', 
                                               workspace=ws)
    aci_service.wait_for_deployment(show_output=True)    
    ```

Depois de termos dividido o processo de implantação em tarefas individuais, podemos ver alguns dos erros mais comuns.

## <a name="image-building-fails"></a>Falhas na criação da imagem

Se a imagem do Docker não pode ser criada, o [image.wait_for_creation()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image(class)?view=azure-ml-py#wait-for-creation-show-output-false-) ou [service.wait_for_deployment()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#wait-for-deployment-show-output-false-) chamada falhar com algumas mensagens de erro que podem oferecer algumas dicas. Você também pode encontrar mais detalhes sobre os erros no log de build da imagem. Abaixo está um código de exemplo mostrando como descobrir o URI do log de build da imagem.

```python
# if you already have the image object handy
print(image.image_build_log_uri)

# if you only know the name of the image (note there might be multiple images with the same name but different version number)
print(ws.images['myimg'].image_build_log_uri)

# list logs for all images in the workspace
for name, img in ws.images.items():
    print (img.name, img.version, img.image_build_log_uri)
```

O URI do log de imagem é uma URL SAS que aponta para um arquivo de log armazenado no Armazenamento de Blobs do Azure. Basta copiar e colar o URI em uma janela do navegador para poder baixar e exibir o arquivo de log.

### <a name="azure-key-vault-access-policy-and-azure-resource-manager-templates"></a>Política de acesso do Cofre de chaves do Azure e modelos do Azure Resource Manager

A compilação de imagem também pode falhar devido a um problema com a política de acesso no cofre de chaves do Azure. Essa situação pode ocorrer quando você usa um modelo do Azure Resource Manager para criar o espaço de trabalho e recursos associados (incluindo o Azure Key Vault), várias vezes. Por exemplo, usando o modelo várias vezes com os mesmos parâmetros como parte de um pipeline de implantação e integração contínua.

A maioria das operações de criação de recursos por meio de modelos são idempotentes, mas o Cofre de chaves limpa as políticas de acesso de cada vez que o modelo é usado. Limpando o acesso de quebras políticas de acesso para o Cofre de chaves para qualquer espaço de trabalho existente que está sendo usado. Essa condição resulta em erros ao tentar criar novas imagens. A seguir estão exemplos dos erros que você pode receber:

__Portal__:
```text
Create image "myimage": An internal server error occurred. Please try again. If the problem persists, contact support.
```

__SDK__:
```python
image = ContainerImage.create(name = "myimage", models = [model], image_config = image_config, workspace = ws)
Creating image
Traceback (most recent call last):
  File "C:\Python37\lib\site-packages\azureml\core\image\image.py", line 341, in create
    resp.raise_for_status()
  File "C:\Python37\lib\site-packages\requests\models.py", line 940, in raise_for_status
    raise HTTPError(http_error_msg, response=self)
requests.exceptions.HTTPError: 500 Server Error: Internal Server Error for url: https://eastus.modelmanagement.azureml.net/api/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.MachineLearningServices/workspaces/<workspace-name>/images?api-version=2018-11-19

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Python37\lib\site-packages\azureml\core\image\image.py", line 346, in create
    'Content: {}'.format(resp.status_code, resp.headers, resp.content))
azureml.exceptions._azureml_exception.WebserviceException: Received bad response from Model Management Service:
Response Code: 500
Headers: {'Date': 'Tue, 26 Feb 2019 17:47:53 GMT', 'Content-Type': 'application/json', 'Transfer-Encoding': 'chunked', 'Connection': 'keep-alive', 'api-supported-versions': '2018-03-01-preview, 2018-11-19', 'x-ms-client-request-id': '3cdcf791f1214b9cbac93076ebfb5167', 'x-ms-client-session-id': '', 'Strict-Transport-Security': 'max-age=15724800; includeSubDomains; preload'}
Content: b'{"code":"InternalServerError","statusCode":500,"message":"An internal server error occurred. Please try again. If the problem persists, contact support"}'
```

__CLI__:
```text
ERROR: {'Azure-cli-ml Version': None, 'Error': WebserviceException('Received bad response from Model Management Service:\nResponse Code: 500\nHeaders: {\'Date\': \'Tue, 26 Feb 2019 17:34:05
GMT\', \'Content-Type\': \'application/json\', \'Transfer-Encoding\': \'chunked\', \'Connection\': \'keep-alive\', \'api-supported-versions\': \'2018-03-01-preview, 2018-11-19\', \'x-ms-client-request-id\':
\'bc89430916164412abe3d82acb1d1109\', \'x-ms-client-session-id\': \'\', \'Strict-Transport-Security\': \'max-age=15724800; includeSubDomains; preload\'}\nContent:
b\'{"code":"InternalServerError","statusCode":500,"message":"An internal server error occurred. Please try again. If the problem persists, contact support"}\'',)}
```

Para evitar esse problema, recomendamos que uma das seguintes abordagens:

* Não implante o modelo mais de uma vez para os mesmos parâmetros. Ou exclua os recursos existentes antes de usar o modelo para recriá-los.
* Examinar as políticas de acesso do Cofre de chaves e, em seguida, use essas políticas para definir o `accessPolicies` propriedade do modelo.
* Verifique se o recurso de Cofre de chaves já existe. Se isso acontecer, não recriá-lo por meio do modelo. Por exemplo, adicione um parâmetro que permite que você desabilite a criação do recurso Cofre de chaves, se ele já existe.

## <a name="debug-locally"></a>Depurar localmente

Se você encontrar problemas ao implantar um modelo no ACI ou AKS, tente implantá-lo como um serviço da web local. Usando um serviço da web local torna mais fácil solucionar problemas. A imagem do Docker que contém o modelo é baixada e iniciada no seu sistema local.

> [!IMPORTANT]
> Implantações de serviço da web local requerem uma instalação do Docker em seu sistema local de trabalho. Docker deve estar em execução antes de implantar um serviço web local. Para obter informações sobre como instalar e usar o Docker, consulte [ https://www.docker.com/ ](https://www.docker.com/).

> [!WARNING]
> Não há suporte para implantações de serviço da web local para cenários de produção.

Para implantar localmente, modificar seu código para usar `LocalWebservice.deploy_configuration()` para criar uma configuração de implantação. Em seguida, use `Model.deploy()` para implantar o serviço. O exemplo a seguir implanta um modelo (contido no `model` variável) como um serviço da web local:

```python
from azureml.core.model import InferenceConfig,Model
from azureml.core.webservice import LocalWebservice

# Create inference configuration. This creates a docker image that contains the model.
inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml")

# Create a local deployment, using port 8890 for the web service endpoint
deployment_config = LocalWebservice.deploy_configuration(port=8890)
# Deploy the service
service = Model.deploy(ws, "mymodel", [model], inference_config, deployment_config)
# Wait for the deployment to complete
service.wait_for_deployment(True)
# Display the port that the web service is available on
print(service.port)
```

Neste ponto, você pode trabalhar com o serviço como de costume. Por exemplo, o código a seguir demonstra o envio de dados para o serviço:

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})

test_sample = bytes(test_sample,encoding = 'utf8')

prediction = service.run(input_data=test_sample)
print(prediction)
```

### <a name="update-the-service"></a>Atualizar o serviço

Durante o teste local, talvez você precise atualizar o `score.py` arquivo para adicionar o registro em log ou tentar resolver os problemas que você descobriu. Para recarregar as alterações para o `score.py` do arquivo, use `reload()`. Por exemplo, o código a seguir recarrega o script para o serviço e, em seguida, envia dados para ele. Os dados serão pontuados usando atualizada `score.py` arquivo:

```python
service.reload()
print(service.run(input_data=test_sample))
```

> [!NOTE]
> O script é recarregado do local especificado pelo `InferenceConfig` objeto usado pelo serviço.

Para alterar o modelo, as dependências de Conda ou configuração de implantação, use [Update ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#update--args-). O exemplo a seguir atualiza o modelo usado pelo serviço:

```python
service.update([different_model], inference_config, deployment_config)
```

### <a name="delete-the-service"></a>Excluir o serviço

Para excluir o serviço, use [Delete ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#delete--).

### <a id="dockerlog"></a> Inspecionar o log do Docker

Você pode imprimir mensagens de log do mecanismo do Docker detalhadas do objeto de serviços. Você pode exibir o log de implantações locais de ACI e AKS. O exemplo a seguir demonstra como imprimir os logs.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```

## <a name="service-launch-fails"></a>Falhas na inicialização do serviço

Depois que a imagem é criada com êxito, o sistema tenta iniciar um contêiner usando a sua configuração de implantação. Como parte do processo de inicialização do contêiner, a função `init()` em seu script de pontuação é invocada pelo sistema. Se houver exceções não capturadas na função `init()`, você poderá ver o erro **CrashLoopBackOff** na mensagem de erro.

Use as informações na [inspecionar o log do Docker](#dockerlog) seção verificar os logs.

## <a name="function-fails-getmodelpath"></a>Falha de função: get_model_path()

Muitas vezes, nos `init()` função no script de pontuação [Model.get_model_path()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) função é chamada para localizar um arquivo de modelo ou uma pasta de arquivos de modelo no contêiner. Se o arquivo de modelo ou a pasta não for encontrada, a função falhará. A maneira mais fácil para depurar esse erro é executar o código do Python no shell do contêiner abaixo:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
from azureml.core.model import Model
print(Model.get_model_path(model_name='my-best-model'))
```

Este exemplo imprime o caminho local (relativo ao `/var/azureml-app`) no contêiner em que o seu script de pontuação está esperando para localizar o arquivo de modelo ou a pasta. Em seguida, você pode verificar se o arquivo ou a pasta realmente está no local que deveria.

Definir o nível de log como depuração, informações adicionais a serem registrados, que podem ser útil para identificar a falha pode causar.

## <a name="function-fails-runinputdata"></a>Falha de função: run(input_data)

Se o serviço for implantado com êxito, mas falhar quando você publicar dados no ponto de extremidade de pontuação, você poderá adicionar o erro capturando instrução na função `run(input_data)` de modo que ele retorne a mensagem de erro detalhada em vez disso. Por exemplo:

```python
def run(input_data):
    try:
        data = json.loads(input_data)['data']
        data = np.array(data)
        result = model.predict(data)
        return json.dumps({"result": result.tolist()})
    except Exception as e:
        result = str(e)
        # return error message back to the client
        return json.dumps({"error": result})
```

**Observação**: O retorno de mensagens de erro da chamada `run(input_data)` deve ser feito para apenas para fins de depuração. Por motivos de segurança, você não deve retornar mensagens de erro dessa forma em um ambiente de produção.

## <a name="http-status-code-503"></a>Código de status HTTP 503

Implantações de Kubernetes Service do Azure dão suporte a dimensionamento automático, que permite que as réplicas a ser adicionado para dar suporte à carga adicional. No entanto, o dimensionador automático foi projetado para tratar **gradual** as alterações na carga. Se você receber grandes picos nas solicitações por segundo, os clientes podem receber um código de status HTTP 503.

Há duas coisas que podem ajudar a impedir que os códigos de status 503:

* Alterar o nível de utilização em quais dimensionamento automático cria novas réplicas.
    
    Por padrão, a utilização de destino de dimensionamento automático é definida como 70%, o que significa que o serviço pode lidar com picos de solicitações por segundo (RPS) de até 30%. Você pode ajustar a meta de utilização, definindo o `autoscale_target_utilization` para um valor mais baixo.

    > [!IMPORTANT]
    > Essa alteração não faz com que as réplicas a ser criado *com mais rapidez*. Em vez disso, eles são criados em um limite inferior de utilização. Em vez de esperar até que o serviço seja 70% de utilização, alterando o valor como 30% faz com que as réplicas a ser criado quando ocorre 30% da utilização.
    
    Se o serviço web já está usando as réplicas máximo atuais e você ainda estiver vendo os códigos de status 503, aumente o `autoscale_max_replicas` valor para aumentar o número máximo de réplicas.

* Altere o número mínimo de réplicas. Aumentar as réplicas mínimas fornece um pool maior para lidar com os picos de entrada.

    Para aumentar o número mínimo de réplicas, defina `autoscale_min_replicas` para um valor mais alto. Você pode calcular as réplicas necessárias usando o código a seguir, substituindo os valores pelos valores específicos ao seu projeto:

    ```python
    from math import ceil
    # target requests per second
    targetRps = 20
    # time to process the request (in seconds)
    reqTime = 10
    # Maximum requests per container
    maxReqPerContainer = 1
    # target_utilization. 70% in this example
    targetUtilization = .7

    concurrentRequests = targetRps * reqTime / targetUtilization

    # Number of container replicas
    replicas = ceil(concurrentRequests / maxReqPerContainer)
    ```

    > [!NOTE]
    > Se você receber maior do que as novas réplicas mínimo podem lidar com picos de solicitação, você pode receber 503s ao novamente. Por exemplo, como o tráfego para seu serviço aumenta, você talvez precise aumentar as réplicas mínimo.

Para obter mais informações sobre a configuração `autoscale_target_utilization`, `autoscale_max_replicas`, e `autoscale_min_replicas` , consulte o [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) referência de módulo.


## <a name="advanced-debugging"></a>Depuração avançada

Em alguns casos, talvez você precise depurar interativamente o código Python contido em sua implantação de modelo. Por exemplo, se o script de entrada está falhando e o motivo pelo qual não pode ser determinado pelo registro em log adicional. Usando o Visual Studio Code e as ferramentas Python para Visual Studio (PTVSD), você pode anexar para o código em execução dentro do contêiner do Docker.

> [!IMPORTANT]
> Esse método de depuração não funciona ao usar `Model.deploy()` e `LocalWebservice.deploy_configuration` para implantar um modelo local. Em vez disso, você deve criar uma imagem usando o [ContainerImage](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py) classe. 
>
> Implantações de serviço da web local requerem uma instalação do Docker em seu sistema local de trabalho. Docker deve estar em execução antes de implantar um serviço web local. Para obter informações sobre como instalar e usar o Docker, consulte [ https://www.docker.com/ ](https://www.docker.com/).

### <a name="configure-development-environment"></a>Configurar o ambiente de desenvolvimento

1. Para instalar as ferramentas Python para Visual Studio (PTVSD) em seu ambiente de desenvolvimento local do VS Code, use o seguinte comando:

    ```
    python -m pip install --upgrade ptvsd
    ```

    Para obter mais informações sobre como usar a PTVSD com o VS Code, consulte [depuração remota](https://code.visualstudio.com/docs/python/debugging#_remote-debugging).

1. Para configurar o VS Code para se comunicar com a imagem do Docker, crie uma nova configuração de depuração:

    1. No VS Code, selecione a __Debug__ menu e, em seguida, selecione __abra configurações__. Um arquivo chamado __Launch__ é aberta.

    1. No __Launch. JSON__ do arquivo, localize a linha que contém `"configurations": [`e insira o texto a seguir depois dela:

        ```json
        {
            "name": "Azure Machine Learning service: Docker Debug",
            "type": "python",
            "request": "attach",
            "port": 5678,
            "host": "localhost",
            "pathMappings": [
                {
                    "localRoot": "${workspaceFolder}",
                    "remoteRoot": "/var/azureml-app"
                }
            ]
        }
        ```

        > [!IMPORTANT]
        > Se já houver outras entradas na seção configurações, adicione uma vírgula (,) após o código que você inseriu.

        Esta seção se anexa ao contêiner do Docker usando a porta 5678.

    1. Salvar a __Launch__ arquivo.

### <a name="create-an-image-that-includes-ptvsd"></a>Criar uma imagem que inclui o PTVSD

1. Modificar o ambiente do conda para sua implantação para que ele inclua PTVSD. O exemplo a seguir demonstra a adicioná-lo usando o `pip_packages` parâmetro:

    ```python
    from azureml.core.conda_dependencies import CondaDependencies 
    
    # Usually a good idea to choose specific version numbers
    # so training is made on same packages as scoring
    myenv = CondaDependencies.create(conda_packages=['numpy==1.15.4',            
                                'scikit-learn==0.19.1', 'pandas==0.23.4'],
                                 pip_packages = ['azureml-defaults==1.0.17', 'ptvsd'])
    
    with open("myenv.yml","w") as f:
        f.write(myenv.serialize_to_string())
    ```

1. Para iniciar o PTVSD e aguardar uma conexão quando o serviço é iniciado, adicione o seguinte na parte superior do seu `score.py` arquivo:

    ```python
    import ptvsd
    # Allows other computers to attach to ptvsd on this IP address and port.
    ptvsd.enable_attach(address=('0.0.0.0', 5678), redirect_output = True)
    # Wait 30 seconds for a debugger to attach. If none attaches, the script continues as normal.
    ptvsd.wait_for_attach(timeout = 30)
    print("Debugger attached...")
    ```

1. Durante a depuração, você talvez queira fazer alterações nos arquivos na imagem sem ter que recriá-la. Para instalar um editor de texto (vim) na imagem do Docker, criar um novo arquivo de texto chamado `Dockerfile.steps` e use o seguinte como o conteúdo do arquivo:

    ```text
    RUN apt-get update && apt-get -y install vim
    ```

    Um editor de texto permite que você modifique os arquivos dentro da imagem do docker para testar alterações sem criar uma nova imagem.

1. Para criar uma imagem que usa o `Dockerfile.steps` do arquivo, use o `docker_file` parâmetro ao criar uma imagem. O exemplo a seguir demonstra como fazer isso:

    > [!NOTE]
    > Este exemplo supõe que `ws` aponta para seu espaço de trabalho do Azure Machine Learning e que `model` é o modelo que está sendo implantado. O `myenv.yml` arquivo contém as dependências de conda criadas na etapa 1.

    ```python
    from azureml.core.image import Image, ContainerImage
    image_config = ContainerImage.image_configuration(runtime= "python",
                                 execution_script="score.py",
                                 conda_file="myenv.yml",
                                 docker_file="Dockerfile.steps")

    image = Image.create(name = "myimage",
                     models = [model],
                     image_config = image_config, 
                     workspace = ws)
    # Print the location of the image in the repository
    print(image.image_location)
    ```

Quando a imagem tiver sido criada, a localização da imagem no registro é exibida. O local é semelhante ao seguinte texto:

```text
myregistry.azurecr.io/myimage:1
```

Neste exemplo de texto, é o nome do registro `myregistry` e a imagem é denominada `myimage`. A versão da imagem é `1`.

### <a name="download-the-image"></a>Baixar a imagem

1. Abra um prompt de comando, terminal ou outro shell e use o seguinte [CLI do Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) comando para autenticar a assinatura do Azure que contém seu espaço de trabalho do Azure Machine Learning:

    ```azurecli
    az login
    ```

1. Para autenticar para o Azure contêiner ACR (registro) que contém a imagem, use o comando a seguir. Substitua `myregistry` com aquele retornado quando você registrou a imagem:

    ```azurecli
    az acr login --name myregistry
    ```

1. Para baixar a imagem do Docker local, use o comando a seguir. Substitua `myimagepath` com o local retornado quando você registrou a imagem:

    ```bash
    docker pull myimagepath
    ```

    O caminho da imagem deve ser semelhante ao `myregistry.azurecr.io/myimage:1`. Em que `myregistry` é o registro `myimage` é sua imagem e `1` é a versão da imagem.

    > [!TIP]
    > A autenticação da etapa anterior não são eternos. Se você aguardar tempo suficiente entre o comando de autenticação e o comando de pull, você receberá uma falha de autenticação. Se isso acontecer, autenticar novamente.

    O tempo necessário para concluir o download depende da velocidade da sua conexão de internet. Um status de download é exibido durante o processo. Depois que o download for concluído, você pode usar o `docker images` para verificar se ela foi baixada.

1. Para tornar mais fácil trabalhar com a imagem, use o comando a seguir para adicionar uma marca. Substitua `myimagepath` com o valor do local da etapa 2.

    ```bash
    docker tag myimagepath debug:1
    ```

    Para o restante das etapas, você pode consultar a imagem local como `debug:1` em vez do valor do caminho de imagem completa.

### <a name="debug-the-service"></a>Depurar o serviço

> [!TIP]
> Se você definir um tempo limite para a conexão de PTVSD no `score.py` arquivo, você deve se conectar VS Code para a sessão de depuração antes de expirar o tempo limite. Inicie o VS Code, abra a cópia local do `score.py`, defina um ponto de interrupção e deixá-lo pronto para ir antes de usar as etapas nesta seção.
>
> Para obter mais informações sobre como depurar e definindo pontos de interrupção, consulte [depuração](https://code.visualstudio.com/Docs/editor/debugging).

1. Para iniciar um contêiner do Docker usando a imagem, use o seguinte comando:

    ```bash
    docker run --rm --name debug -p 8000:5001 -p 5678:5678 debug:1
    ```

1. Para anexar o VS Code para PTVSD dentro do contêiner, abra o VS Code e usar chaves ou selecione o F5 __depurar__. Quando solicitado, selecione o __serviço Azure Machine Learning: Depuração de docker__ configuração. Você também pode selecionar o ícone Depurar na barra lateral, o __serviço Azure Machine Learning: Depuração de docker__ entrada do menu suspenso de depuração e, em seguida, use a seta verde para anexar o depurador.

    ![O ícone de depuração, o botão de iniciar depuração e o seletor de configuração](media/how-to-troubleshoot-deployment/start-debugging.png)

Neste ponto, o VS Code se conecta ao PTVSD dentro do contêiner do Docker e para no ponto de interrupção que você definiu anteriormente. Você agora pode percorrer o código enquanto ele é executado, exibir variáveis, etc.

Para obter mais informações sobre como usar o VS Code para depurar o Python, consulte [depurar seu código Python](https://docs.microsoft.com/visualstudio/python/debugging-python-in-visual-studio?view=vs-2019).

<a id="editfiles"></a>
### <a name="modify-the-container-files"></a>Modifique os arquivos de contêiner

Para fazer alterações nos arquivos na imagem, você pode anexar a contêiner em execução e executar um shell bash. A partir daí, você pode usar vim para editar arquivos:

1. Para se conectar ao contêiner em execução e iniciar um shell bash no contêiner, use o seguinte comando:

    ```bash
    docker exec -it debug /bin/bash
    ```

1. Para localizar os arquivos usados pelo serviço, use o seguinte comando no shell bash no contêiner:

    ```bash
    cd /var/azureml-app
    ```

    A partir daqui, você pode usar vim para editar o `score.py` arquivo. Para obter mais informações sobre como usar vim, consulte [usando o editor Vim](https://www.tldp.org/LDP/intro-linux/html/sect_06_02.html).

1. Alterações em um contêiner normalmente não são persistentes. Para salvar as alterações feitas, use o comando a seguir, antes de sair do shell iniciado na etapa anterior (ou seja, em outro shell):

    ```bash
    docker commit debug debug:2
    ```

    Este comando cria uma nova imagem chamada `debug:2` que contém suas edições.

    > [!TIP]
    > Você precisará interromper o contêiner atual e começar a usar a nova versão que as alterações entrem em vigor.

1. Certifique-se de manter as alterações feitas nos arquivos no contêiner em sincronia com os arquivos de locais que usa o VS Code. Caso contrário, a experiência do depurador não funcionará conforme o esperado.

### <a name="stop-the-container"></a>Parar o contêiner

Para interromper o contêiner, use o seguinte comando:

```bash
docker stop debug
```

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre a implantação:

* [Como e onde implantar](how-to-deploy-and-where.md)
* [Tutorial: Treinar e implantar modelos](tutorial-train-models-with-aml.md)
