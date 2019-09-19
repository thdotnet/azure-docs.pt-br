---
title: Como instalar e executar contêineres - Pesquisa Visual Computacional
titleSuffix: Azure Cognitive Services
description: Como baixar, instalar e executar contêineres para Pesquisa Visual Computacional neste tutorial passo a passo.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 09/18/2019
ms.author: dapine
ms.custom: seodec18
ms.openlocfilehash: 97a9b6c60539191850e8205eed4387565b79f6db
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71129884"
---
# <a name="install-and-run-computer-vision-containers"></a>Instalar e executar contêineres de Pesquisa Visual Computacional

Os contêineres permitem que você execute as APIs de Pesquisa Visual Computacional em seu próprio ambiente. Os contêineres são ótimos para requisitos específicos de segurança e governança de dados. Neste artigo, você aprenderá a baixar, instalar e executar um Pesquisa Visual Computacional contêiner.

Há dois contêineres do Docker disponíveis para Pesquisa Visual Computacional: *Reconhecimento de texto* e *ler*. O contêiner *reconhecimento de texto* permite que você detecte e extraia *texto impresso* de imagens de vários objetos com diferentes superfícies e planos de fundo, como recibos, pôsteres e cartões de visita. No entanto, o contêiner de *leitura* ; também detecta *texto manuscrito* em imagens e fornece suporte a PDF/TIFF/várias páginas. Para obter mais informações, consulte a documentação da [API de leitura](concept-recognizing-text.md#read-api) .

> [!IMPORTANT]
> O contêiner de Reconhecimento de Texto está sendo preterido em favor do contêiner de leitura. O contêiner de leitura é um superconjunto de seu antecessor o contêiner de Reconhecimento de Texto e os consumidores devem migrar para o usando o contêiner de leitura. Ambos os contêineres só funcionam com o inglês.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Você deve atender aos seguintes pré-requisitos antes de usar os contêineres:

|Necessário|Finalidade|
|--|--|
|Mecanismo Docker| É necessário ter o Mecanismo Docker instalado em um [computador host](#the-host-computer). O Docker fornece pacotes que configuram o ambiente do Docker no [macOS](https://docs.docker.com/docker-for-mac/), no [Windows](https://docs.docker.com/docker-for-windows/) e no [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Para instruções sobre conceitos básicos do Docker e de contêiner, consulte a [visão geral do Docker](https://docs.docker.com/engine/docker-overview/).<br><br> O Docker deve ser configurado para permitir que os contêineres conectem-se e enviem dados de cobrança para o Azure. <br><br> **No Windows**, o Docker também deve ser configurado para dar suporte a contêineres do Linux.<br><br>|
|Familiaridade com o Docker | É necessário ter uma compreensão básica de conceitos do Docker, como registros, repositórios, contêineres e imagens de contêiner, bem como conhecimento dos comandos básicos do `docker`.| 
|Pesquisa Visual Computacional recurso |Para usar o contêiner, você precisará ter:<br><br>Um recurso de **Pesquisa Visual computacional** do Azure e a chave de API associada do URI do ponto de extremidade. Ambos os valores estão disponíveis nas páginas visão geral e chaves para o recurso e são necessários para iniciar o contêiner.<br><br>**{API_KEY}** : Uma das duas chaves de recurso disponíveis na página **chaves**<br><br>**{ENDPOINT_URI}** : O ponto de extremidade conforme fornecido na página **visão geral**|

[!INCLUDE [Gathering required container parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="request-access-to-the-private-container-registry"></a>Solicitar acesso ao registro de contêiner privado

[!INCLUDE [Request access to public preview](../../../includes/cognitive-services-containers-request-access.md)]

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

### <a name="the-host-computer"></a>O computador host

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Recomendações e requisitos do contêiner

[!INCLUDE [Container requirements and recommendations](includes/container-requirements-and-recommendations.md)]

## <a name="get-the-container-image-with-docker-pull"></a>Obter a imagem de contêiner com `docker pull`

# <a name="readtabread"></a>[Ler](#tab/read)

As imagens de contêiner para leitura estão disponíveis.

| Contêiner | Registro de contêiner/repositório/nome da imagem |
|-----------|------------|
| Leitura | `containerpreview.azurecr.io/microsoft/cognitive-services-read:latest` |

# <a name="recognize-texttabrecognize-text"></a>[Reconhecimento de Texto](#tab/recognize-text)

Há imagens de contêiner para Reconhecimento de Texto disponíveis.

| Contêiner | Registro de contêiner/repositório/nome da imagem |
|-----------|------------|
| Reconhecimento de Texto | `containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest` |

***

Use o comando [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) para baixar uma imagem de contêiner.

# <a name="readtabread"></a>[Ler](#tab/read)

### <a name="docker-pull-for-the-read-container"></a>Pull do Docker para o contêiner de leitura

```bash
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-read:latest
```

# <a name="recognize-texttabrecognize-text"></a>[Reconhecimento de Texto](#tab/recognize-text)

### <a name="docker-pull-for-the-recognize-text-container"></a>Docker pull para o contêiner de Reconhecimento de Texto

```bash
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest
```

***

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>Como usar o contêiner

Depois que o contêiner estiver no [computador host](#the-host-computer), use o processo a seguir para trabalhar com o contêiner.

1. [Execute o contêiner](#run-the-container-with-docker-run) com as configurações de cobrança necessárias. Há outros [exemplos](computer-vision-resource-container-config.md) do comando `docker run` disponíveis. 
1. [Consulte o ponto de extremidade de previsão do contêiner](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Executar o contêiner com `docker run`

Use o comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para executar o contêiner. Consulte [coletando parâmetros necessários](#gathering-required-parameters) para obter detalhes sobre como obter os `{ENDPOINT_URI}` valores `{API_KEY}` e.

[Exemplos](computer-vision-resource-container-config.md#example-docker-run-commands) do `docker run` comando estão disponíveis.

# <a name="readtabread"></a>[Ler](#tab/read)

```bash
docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
containerpreview.azurecr.io/microsoft/cognitive-services-read \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Esse comando:

* Executa o contêiner de leitura da imagem de contêiner.
* Aloca 8 núcleos de CPU e 16 gigabytes (GB) de memória.
* Expõe a porta TCP 5000 e aloca um pseudo-TTY para o contêiner.
* Remove automaticamente o contêiner depois que ele sai. A imagem de contêiner ainda fica disponível no computador host.

# <a name="recognize-texttabrecognize-text"></a>[Reconhecimento de Texto](#tab/recognize-text)

```bash
docker run --rm -it -p 5000:5000 --memory 16g --cpus 8 \
containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Esse comando:

* Executa o contêiner de Reconhecimento de Texto da imagem de contêiner.
* Aloca 8 núcleos de CPU e 16 gigabytes (GB) de memória.
* Expõe a porta TCP 5000 e aloca um pseudo-TTY para o contêiner.
* Remove automaticamente o contêiner depois que ele sai. A imagem de contêiner ainda fica disponível no computador host.

***

Há outros [exemplos](./computer-vision-resource-container-config.md#example-docker-run-commands) do comando `docker run` disponíveis. 

> [!IMPORTANT]
> As opções `Eula`, `Billing` e `ApiKey` devem ser especificadas para executar o contêiner; caso contrário, o contêiner não será iniciado.  Para mais informações, consulte [Faturamento](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Consultar o ponto de extremidade de previsão do contêiner

O contêiner fornece APIs de ponto de extremidade de previsão de consulta com base em REST. 

Use o host, `http://localhost:5000`, para APIs de contêiner.

# <a name="readtabread"></a>[Ler](#tab/read)

### <a name="asynchronous-read"></a>Leitura assíncrona

Você pode usar as `POST /vision/v2.0/read/core/asyncBatchAnalyze` operações `GET /vision/v2.0/read/operations/{operationId}` e em conjunto para ler de forma assíncrona uma imagem, semelhante a como o serviço de pesquisa Visual computacional usa as operações REST correspondentes. O método post assíncrono retornará um `operationId` que é usado como o identificador para a solicitação HTTP Get.

Na interface do usuário do Swagger, `asyncBatchAnalyze` selecione o para expandi-lo no navegador. Em seguida, selecione **experimentar** > **escolher arquivo**. Neste exemplo, usaremos a imagem a seguir:

![guias vs espaços](media/tabs-vs-spaces.png)

Quando a POSTAgem assíncrona for executada com êxito, ela retornará um código de status **HTTP 202** . Como parte da resposta, há um `operation-location` cabeçalho que contém o ponto de extremidade do resultado para a solicitação.

```http
 content-length: 0
 date: Fri, 13 Sep 2019 16:23:01 GMT
 operation-location: http://localhost:5000/vision/v2.0/read/operations/a527d445-8a74-4482-8cb3-c98a65ec7ef9
 server: Kestrel
```

O `operation-location` é a URL totalmente qualificada e é acessado por meio de um http Get. Aqui está a resposta JSON da execução `operation-location` da URL da imagem anterior:

```json
{
  "status": "Succeeded",
  "recognitionResults": [
    {
      "page": 1,
      "clockwiseOrientation": 2.42,
      "width": 502,
      "height": 252,
      "unit": "pixel",
      "lines": [
        {
          "boundingBox": [
            56,
            39,
            317,
            50,
            313,
            134,
            53,
            123
          ],
          "text": "Tabs VS",
          "words": [
            {
              "boundingBox": [
                90,
                43,
                243,
                53,
                243,
                123,
                94,
                125
              ],
              "text": "Tabs",
              "confidence": "Low"
            },
            {
              "boundingBox": [
                259,
                55,
                313,
                62,
                313,
                122,
                259,
                123
              ],
              "text": "VS"
            }
          ]
        },
        {
          "boundingBox": [
            221,
            148,
            417,
            146,
            417,
            206,
            227,
            218
          ],
          "text": "Spaces",
          "words": [
            {
              "boundingBox": [
                230,
                148,
                416,
                141,
                419,
                211,
                232,
                218
              ],
              "text": "Spaces"
            }
          ]
        }
      ]
    }
  ]
}
```

### <a name="synchronous-read"></a>Leitura síncrona

Você pode usar a `POST /vision/v2.0/read/core/Analyze` operação para ler de forma síncrona uma imagem. Quando a imagem é lida em sua totalidade, e somente em seguida, a API retorna uma resposta JSON. A única exceção a isso é se ocorrer um erro. Quando ocorre um erro, o JSON a seguir é retornado:

```json
{
    status: "Failed"
}
```

O objeto de resposta JSON tem o mesmo grafo de objeto que a versão assíncrona. Se você for um usuário de JavaScript e quiser a segurança de tipo, os tipos a seguir podem ser usados para converter a `AnalyzeResult` resposta JSON como um objeto.

```typescript
export interface AnalyzeResult {
    status: Status;
    recognitionResults?: RecognitionResult[] | null;
}

export enum Status {
    NotStarted = 0,
    Running = 1,
    Failed = 2,
    Succeeded = 3
}

export enum Unit {
    Pixel = 0,
    Inch = 1
}

export interface RecognitionResult {
    page?: number | null;
    clockwiseOrientation?: number | null;
    width?: number | null;
    height?: number | null;
    unit?: Unit | null;
    lines?: Line[] | null;
}

export interface Line {
    boundingBox?: number[] | null;
    text: string;
    words?: Word[] | null;
}

export interface Word {
  boundingBox?: number[] | null;
  text: string;
  confidence?: string | null;
}
```

Para obter um exemplo de caso de uso, consulte a [área restrita do TypeScript aqui](https://aka.ms/ts-read-api-types) e selecione "executar" para visualizar sua facilidade de uso.

# <a name="recognize-texttabrecognize-text"></a>[Reconhecimento de Texto](#tab/recognize-text)

### <a name="asynchronous-text-recognition"></a>Reconhecimento de texto assíncrono

Você pode usar as operações `POST /vision/v2.0/recognizeText` e `GET /vision/v2.0/textOperations/*{id}*` em conjunto para reconhecer assincronamente texto impresso em uma imagem, semelhante a como o serviço de Pesquisa Visual Computacional usa essas operações REST correspondentes. O contêiner de Reconhecimento de Texto reconhece apenas texto impresso, texto não manuscrito, neste momento, portanto, o parâmetro `mode` normalmente especificado para a operação de serviço de Pesquisa Visual Computacional é ignorado pelo contêiner de Reconhecimento de Texto.

### <a name="synchronous-text-recognition"></a>Reconhecimento de texto síncrono

Você pode usar a operação `POST /vision/v2.0/recognizeTextDirect` para, de forma síncrona, reconhecer texto impresso em uma imagem. Como essa operação é síncrona, o corpo da solicitação para essa operação é o mesmo que `POST /vision/v2.0/recognizeText` a operação, mas o corpo da resposta para essa operação é o mesmo que o retornado `GET /vision/v2.0/textOperations/*{id}*` pela operação.

***

## <a name="stop-the-container"></a>Parar o contêiner

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Solução de problemas

Se você executar o contêiner com uma [montagem](./computer-vision-resource-container-config.md#mount-settings) de saída e o registro em log habilitado, o contêiner gerará arquivos de log que são úteis para solucionar problemas que ocorrem durante a inicialização ou execução do contêiner. 

## <a name="billing"></a>Cobrança

Os contêineres de serviços cognitivas enviam informações de cobrança para o Azure, usando o recurso correspondente em sua conta do Azure.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Para obter mais informações sobre essas opções, consulte [Configurar contêineres](./computer-vision-resource-container-config.md).

<!--blogs/samples/video course -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Resumo

Neste artigo, você aprendeu conceitos e fluxo de trabalho para baixar, instalar e executar os contêineres de Pesquisa Visual Computacional. Em resumo:

* Pesquisa Visual Computacional fornece um contêiner do Linux para o Docker, encapsulando Reconhecimento de Texto e ler.
* As imagens de contêiner são baixadas do registro de contêiner "visualização do contêiner" no Azure.
* Imagens de contêiner são executadas no Docker.
* Você pode usar a API REST ou o SDK para chamar operações em Reconhecimento de Texto ou ler contêineres especificando o URI do host do contêiner.
* Você deve especificar informações de faturamento ao instanciar um contêiner.

> [!IMPORTANT]
> Os contêineres dos Serviços Cognitivos não estão licenciados para execução sem estarem conectados ao Azure para medição. Os clientes precisam ativar os contêineres para comunicar informações de cobrança com o serviço de medição em todos os momentos. Os contêineres de Serviços Cognitivos não enviam dados do cliente (por exemplo, a imagem ou o texto que está sendo analisado) para a Microsoft.

## <a name="next-steps"></a>Próximas etapas

* Revise [Configurar contêineres](computer-vision-resource-container-config.md) para configurações
* Examinar [Visão geral da Pesquisa Visual Computacional](Home.md) para saber mais sobre como reconhecer texto impresso e manuscrito
* Veja a [API da Pesquisa Visual Computacional](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) para obter detalhes sobre os métodos compatíveis com o contêiner.
* Veja as [Perguntas frequentes](FAQ.md) para resolver problemas relacionados à funcionalidade de Pesquisa Visual Computacional.
* Usar mais [Contêineres dos Serviços Cognitivos](../cognitive-services-container-support.md)
