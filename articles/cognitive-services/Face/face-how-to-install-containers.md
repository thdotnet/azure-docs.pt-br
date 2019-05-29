---
title: Instalar e executar contêineres
titlesuffix: Face - Azure Cognitive Services
description: Baixar, instalar e executar contêineres para Face neste tutorial passo a passo.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: article
ms.date: 05/28/2019
ms.author: diberry
ms.openlocfilehash: 26ebeb463f42dce06c29a5bd3f69585430a2ee90
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306648"
---
# <a name="install-and-run-face-containers"></a>Instalar e executar os contêineres de detecção facial

Face de serviços Cognitivos do Azure fornece um contêiner do Linux padronizado para Docker que detecta rostos humanos em imagens. Ele também identifica atributos, que incluem marcos faciais como noses e olhos, sexo, idade e outros recursos faciais prevista de máquina. Além de detecção, Face pode verificar se dois rostos na mesma imagem ou imagens diferentes são as mesmas, usando uma pontuação de confiança. Face também pode comparar rostos em relação a um banco de dados para ver se já existe uma face semelhantes ou idêntica. Também ele pode organizar rostos semelhantes em grupos usando traços visual compartilhados.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Você deve atender aos seguintes pré-requisitos antes de usar os contêineres de API de detecção facial.

|Obrigatório|Finalidade|
|--|--|
|Mecanismo Docker| O mecanismo do Docker deve ser instalado em um [computador host](#the-host-computer). O Docker fornece pacotes que configuram o ambiente do Docker no [macOS](https://docs.docker.com/docker-for-mac/), no [Windows](https://docs.docker.com/docker-for-windows/) e no [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Para instruções sobre conceitos básicos do Docker e de contêiner, consulte a [visão geral do Docker](https://docs.docker.com/engine/docker-overview/).<br><br> O Docker deve ser configurado para permitir que os contêineres conectem-se e enviem dados de cobrança para o Azure. <br><br> No Windows, Docker também deve ser configurado para dar suporte a contêineres do Linux.<br><br>|
|Familiaridade com o Docker | Você precisa de uma compreensão básica dos conceitos do Docker, como registros, repositórios, contêineres e imagens de contêiner. Você também precisa de conhecimento do basic `docker` comandos.| 
|Azure `Cognitive Services` recursos |Para usar o contêiner, você deve ter:<br><br>Um recurso dos serviços Cognitivos do Azure e a chave de cobrança associada e o URI de ponto de extremidade cobrança. Ambos os valores estão disponíveis na **visão geral** e **chaves** páginas para o recurso. Eles são necessários para iniciar o contêiner. Adicionar o `face/v1.0` roteamento para o ponto de extremidade do URI, conforme mostrado no exemplo a seguir de BILLING_ENDPOINT_URI: <br><br>**{BILLING_KEY}** : chave do recurso<br><br>**{BILLING_ENDPOINT_URI}** : exemplo URI do ponto de extremidade é `https://westus.api.cognitive.microsoft.com/face/v1.0`|

## <a name="request-access-to-the-private-container-registry"></a>Solicitar acesso ao registro de contêiner privado

[!INCLUDE [Request access to private container registry](../../../includes/cognitive-services-containers-request-access.md)]

### <a name="the-host-computer"></a>O computador host

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]


### <a name="container-requirements-and-recommendations"></a>Recomendações e requisitos do contêiner

A tabela a seguir descreve os núcleos de CPU e a memória mínimos e recomendados a serem alocados para cada contêiner da API de Detecção Facial.

| Contêiner | Mínimo | Recomendado | Transações por segundo<br>(No mínimo, máximo)|
|-----------|---------|-------------|--|
|Detecção Facial | 1 núcleo, 2 GB de memória | 1 núcleo, memória de 4 GB |10, 20|

* Cada núcleo deve ser pelo menos 2,6 GHz ou mais rápido.
* Transações por segundo (TPS).

Memória e núcleo correspondem às configurações `--cpus` e `--memory`, que são usadas como parte do comando `docker run`.

## <a name="get-the-container-image-with-docker-pull"></a>Obter a imagem de contêiner com o pull do docker

Imagens de contêiner para a API de detecção facial estão disponíveis. 

| Contêiner | Repositório |
|-----------|------------|
| Detecção Facial | `containerpreview.azurecr.io/microsoft/cognitive-services-face:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-face-container"></a>Docker pull para o contêiner de Detecção Facial

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-face:latest
```

## <a name="use-the-container"></a>Usar o contêiner

Depois que o contêiner estiver na [computador host](#the-host-computer), use o seguinte processo para trabalhar com o contêiner.

1. [Execute o contêiner](#run-the-container-with-docker-run) com as configurações de cobrança. Há outros [exemplos](./face-resource-container-config.md#example-docker-run-commands) do comando `docker run` disponíveis. 
1. [Consulte o ponto de extremidade de previsão do contêiner](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Execute o contêiner com docker run

Use o comando [docker run](https://docs.docker.com/engine/reference/commandline/run/) para executar qualquer um dos três contêineres. O comando usa os seguintes parâmetros.

| Espaço reservado | Value |
|-------------|-------|
|{BILLING_KEY} | Essa chave é usada para iniciar o contêiner e está disponível no Azure `Cognitive Services` **chaves** página. |
|{BILLING_ENDPOINT_URI} | O valor do URI de ponto de extremidade cobrança está disponível no Azure `Cognitive Services` **visão geral** página. Um exemplo é `https://westus.api.cognitive.microsoft.com/face/v1.0`.|

Adicionar o `face/v1.0` roteamento para o ponto de extremidade do URI, conforme mostrado no exemplo anterior BILLING_ENDPOINT_URI. 

Substitua esses parâmetros com seus próprios valores a seguir `docker run` exemplo de comando:

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-face \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Esse comando:

* Execute um contêiner de face da imagem do contêiner.
* Aloca um núcleo de CPU e 4 GB de memória.
* Expõe a porta TCP 5000 e aloca uma pseudo TTY para o contêiner.
* Remove automaticamente o contêiner depois que ele sai. A imagem de contêiner ainda fica disponível no computador host. 

Há outros [exemplos](./face-resource-container-config.md#example-docker-run-commands) do comando `docker run` disponíveis. 

> [!IMPORTANT]
> O `Eula`, `Billing`, e `ApiKey` opções devem ser especificadas para executar o contêiner ou o contêiner não será iniciado. Para mais informações, consulte [Faturamento](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]


## <a name="query-the-containers-prediction-endpoint"></a>Consultar o ponto de extremidade de previsão do contêiner

O contêiner fornece APIs de ponto de extremidade de previsão de consulta com base em REST. 

Use o host, `https://localhost:5000`, para APIs de contêiner.


<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>Parar o contêiner

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>solução de problemas

Se você executar o contêiner com uma saída [montar](./face-resource-container-config.md#mount-settings) e registro em log estiver habilitado, o contêiner gera arquivos de log que são úteis para solucionar problemas que ocorrem enquanto você iniciar ou executar o contêiner. 


## <a name="billing"></a>Cobrança

Os contêineres de API de detecção facial enviam informações de cobrança para o Azure por meio de um recurso da API de detecção facial em sua conta do Azure. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Para obter mais informações sobre essas opções, consulte [Configurar contêineres](./face-resource-container-config.md).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Resumo

Neste artigo, você aprendeu conceitos e fluxo de trabalho para saber como baixar, instalar e executar os contêineres de API de detecção facial. Em resumo:

* API de detecção facial fornece três contêineres do Linux para o Docker que fornecem a extração de frases-chave, detecção de idioma e análise de sentimento.
* Imagens de contêiner são baixadas do registro de contêiner do Azure.
* Imagens de contêiner são executadas no Docker.
* Você pode usar a API REST ou o SDK para chamar as operações em contêineres de API de detecção facial, especificando o URI do contêiner de host.
* Quando você criar uma instância de um contêiner, você deve especificar informações de cobrança.

> [!IMPORTANT]
> Contêineres de serviços cognitivos não são licenciados para execução sem estar conectado ao Azure para a medição. Os clientes devem habilitar os contêineres comunicar informações de cobrança com o serviço de medição em todos os momentos. Os contêineres dos Serviços Cognitivos não enviam dados do cliente, como imagem ou texto que está sendo analisado, para a Microsoft.

## <a name="next-steps"></a>Próximas etapas

* Para definições de configuração, consulte [configurar os contêineres](face-resource-container-config.md).
* Para saber mais sobre como detectar e identificar faces, consulte [visão geral de Face](Overview.md).
* Para obter informações sobre os métodos com suporte pelo contêiner, consulte o [API de detecção facial](//westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).
* Para usar mais contêineres de serviços Cognitivos, consulte [contêineres de serviços Cognitivos](../cognitive-services-container-support.md).
