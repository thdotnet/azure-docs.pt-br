---
title: Receitas do contêiner de composição do docker
titleSuffix: Azure Cognitive Services
description: Saiba como implantar vários contêineres de serviços Cognitivos. Este procedimento mostra como coordenar várias imagens de contêiner do Docker com o Docker Compose.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 8afb7e866bc2a5fefe28a71653c4a2a87fdc7a5b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445785"
---
# <a name="use-multiple-containers-in-a-private-network-with-docker-compose"></a>Usar vários contêineres em uma rede privada com o Docker Compose

Saiba como implantar vários contêineres de serviços Cognitivos. Este procedimento mostra como coordenar várias imagens de contêiner do Docker com o Docker Compose.

> [O docker Compose](https://docs.docker.com/compose/) é uma ferramenta para definir e executar aplicativos de vários contêineres do Docker. Com o redigir, você pode usar um arquivo YAML para configurar os serviços do seu aplicativo. Em seguida, com um único comando, criar e iniciar todos os serviços da sua configuração.

Quando apropriado, coordenando várias imagens de contêiner em um computador host único pode ser atraente. Neste artigo, vamos retirar o serviço reconhecer texto e o serviço de formulário reconhecedor juntos.

## <a name="prerequisites"></a>Pré-requisitos

Este procedimento requer várias ferramentas que devem ser instaladas e executadas localmente.

* Use uma assinatura do Azure. Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.
* [Mecanismo do Docker](https://www.docker.com/products/docker-engine) e valide se a CLI do Docker funciona em uma janela do console.
* Um recurso do Azure com o tipo de preço correto. Nem todos os tipos de preços funcionam com esse contêiner:
  * **Pesquisa Visual computacional** recurso com F0 ou preço padrão somente camadas.
  * **Formulário reconhecedor** recurso com F0 ou preço padrão somente camadas.
  * O recurso **Serviços Cognitivos** com o tipo de preço S0.

## <a name="request-access-to-the-container-registry"></a>Solicitar acesso ao Registro de contêiner

Conclua e envie os [formulário de solicitação de contêineres de fala de serviços Cognitivos](https://aka.ms/speechcontainerspreview/) para solicitar acesso ao contêiner. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="docker-compose-file"></a>Docker compose arquivo

O arquivo YAML define todos os serviços a serem implantados. Esses serviços dependem de qualquer um uma `DockerFile` ou uma imagem de contêiner existente, nesse caso, vamos usar duas imagens de visualização. Copie e cole o arquivo YAML a seguir e salve-o como *docker-Compose. YAML*. Fornecer apropriado _apikey_, _cobrança_, e _ponto de extremidade de URI_ valores no _docker-Compose. yml_ arquivo abaixo.

```yaml
version: '3.7'
services:
  forms:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer"
    environment:
       eula: accept
       billing: # < Your form recognizer billing URL >
       apikey: # < Your form recognizer API key >
       FormRecognizer__ComputerVisionApiKey: # < Your form recognizer API key >
       FormRecognizer__ComputerVisionEndpointUri: # < Your form recognizer URI >
    volumes:
       - type: bind
         source: e:\publicpreview\output
         target: /output
       - type: bind
         source: e:\publicpreview\input
         target: /input
    ports:
      - "5010:5000"

  ocr:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text"
    environment:
      eula: accept
      apikey: # < Your recognize text API key >
      billing: # < Your recognize text billing URL >
    ports:
      - "5021:5000"
```

> [!IMPORTANT]
> Criar os diretórios no computador host que são especificados sob o `volumes` nó. Isso é necessário porque os diretórios devem existir antes da tentativa de montar uma imagem com associações de volume.

## <a name="start-the-configured-docker-compose-services"></a>Iniciar o docker configurado compor serviços

Docker compose arquivo permite o gerenciamento de ciclos de vida todos definidos do serviço; Iniciando/interrompendo e recriação de serviços, exibindo o status do serviço e streaming de log. Abra uma interface de linha de comando do diretório do projeto (no qual o *docker-Compose. YAML* fica de arquivo).

> [!NOTE]
> Para evitar erros, certifique-se de que o computador host está compartilhando corretamente unidades com o **mecanismo do Docker**. Por exemplo, se *e:\publicpreview* é usado como um diretório na *docker-Compose. YAML* compartilhar os *E unidade* com o docker.

Na interface de linha de comando, execute o seguinte comando para iniciar (ou reiniciar) em todos os serviços definidos na *docker-Compose. YAML*:

```console
docker-compose up
```

Primeiro tempo executando o `docker-compose up` comando com essa configuração, **Docker** extrairá as imagens configuradas sob o `services` nó – baixando/montagem eles:

```console
Pulling forms (containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer:)...
latest: Pulling from microsoft/cognitive-services-form-recognizer
743f2d6c1f65: Pull complete
72befba99561: Pull complete
2a40b9192d02: Pull complete
c7715c9d5c33: Pull complete
f0b33959f1c4: Pull complete
b8ab86c6ab26: Pull complete
41940c21ed3c: Pull complete
e3d37dd258d4: Pull complete
cdb5eb761109: Pull complete
fd93b5f95865: Pull complete
ef41dcbc5857: Pull complete
4d05c86a4178: Pull complete
34e811d37201: Pull complete
Pulling ocr (containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:)...
latest: Pulling from microsoft/cognitive-services-recognize-text
f476d66f5408: Already exists
8882c27f669e: Already exists
d9af21273955: Already exists
f5029279ec12: Already exists
1a578849dcd1: Pull complete
45064b1ab0bf: Download complete
4bb846705268: Downloading [=========================================>         ]  187.1MB/222.8MB
c56511552241: Waiting
e91d2aa0f1ad: Downloading [==============================================>    ]  162.2MB/176.1MB
```

As imagens são baixadas, em seguida, os serviços de imagem são iniciados.

```console
Starting docker_ocr_1   ... done
Starting docker_forms_1 ... doneAttaching to docker_ocr_1, docker_forms_1forms_1  | forms_1  | forms_1  | Notice: This Preview is made available to you on the condition that you agree to the Supplemental Terms of Use for Microsoft Azure Previews [https://go.microsoft.com/fwlink/?linkid=2018815], which supplement your agreement [https://go.microsoft.com/fwlink/?linkid=2018657] governing your use of Azure. If you do not have an existing agreement governing your use of Azure, you agree that your agreement governing use of Azure is the Microsoft Online Subscription Agreement [https://go.microsoft.com/fwlink/?linkid=2018755] (which incorporates the Online Services Terms [https://go.microsoft.com/fwlink/?linkid=2018760]). By using the Preview you agree to these terms.
forms_1  | 
forms_1  | 
forms_1  | Using '/input' for reading models and other read-only data.
forms_1  | Using '/output/forms/812d811d1bcc' for writing logs and other output data.
forms_1  | Logging to console.
forms_1  | Submitting metering to 'https://westus2.api.cognitive.microsoft.com/'.
forms_1  | WARNING: No access control enabled!
forms_1  | warn: Microsoft.AspNetCore.Server.Kestrel[0]
forms_1  |       Overriding address(es) 'http://+:80'. Binding to endpoints defined in UseKestrel() instead.
forms_1  | Hosting environment: Production
forms_1  | Content root path: /app/forms
forms_1  | Now listening on: http://0.0.0.0:5000
forms_1  | Application started. Press Ctrl+C to shut down.
ocr_1    | 
ocr_1    | 
ocr_1    | Notice: This Preview is made available to you on the condition that you agree to the Supplemental Terms of Use for Microsoft Azure Previews [https://go.microsoft.com/fwlink/?linkid=2018815], which supplement your agreement [https://go.microsoft.com/fwlink/?linkid=2018657] governing your use of Azure. If you do not have an existing agreement governing your use of Azure, you agree that your agreement governing use of Azure is the Microsoft Online Subscription Agreement [https://go.microsoft.com/fwlink/?linkid=2018755] (which incorporates the Online Services Terms [https://go.microsoft.com/fwlink/?linkid=2018760]). By using the Preview you agree to these terms.
ocr_1    |
ocr_1    | 
ocr_1    | Logging to console.
ocr_1    | Submitting metering to 'https://westcentralus.api.cognitive.microsoft.com/'.
ocr_1    | WARNING: No access control enabled!
ocr_1    | Hosting environment: Production
ocr_1    | Content root path: /
ocr_1    | Now listening on: http://0.0.0.0:5000
ocr_1    | Application started. Press Ctrl+C to shut down.
```

## <a name="verify-the-service-availability"></a>Verificar a disponibilidade do serviço

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

Abaixo está um exemplo de saída:

```
IMAGE ID            REPOSITORY                                                                 TAG
2ce533f88e80        containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer   latest
4be104c126c5        containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text    latest
```

### <a name="test-the-recognize-text-container"></a>Testar o contêiner de texto do recognize

Abra um navegador no computador host e navegue até `localhost` com a porta especificada do *docker-Compose. YAML*, por exemplo, `http://localhost:5021/swagger/index.html`. Você pode usar a tentativa de recurso da API para testar o ponto de extremidade de texto do recognize.

![Reconhecer texto Swagger](media/recognize-text-swagger-page.png)

### <a name="test-the-form-recognizer-container"></a>Testar o contêiner do reconhecedor de formulário

Abra um navegador no computador host e navegue até `localhost` com a porta especificada do *docker-Compose. YAML*, por exemplo, `http://localhost:5010/swagger/index.html`. Você pode usar a tentativa de recurso da API para testar o ponto de extremidade do reconhecedor de formulário.

![Formulário reconhecedor Swagger](media/form-recognizer-swagger-page.png)

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Contêineres de serviços cognitivos](../cognitive-services-container-support.md)