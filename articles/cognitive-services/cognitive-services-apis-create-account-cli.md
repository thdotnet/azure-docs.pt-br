---
title: Criar uma conta de serviços Cognitivos usando a CLI do Azure
titlesuffix: Azure Cognitive Services
description: Como criar uma conta de APIs de serviços Cognitivos do Azure usando a CLI do Azure.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: aahi
ms.openlocfilehash: 26f7f3ab60347d9ec5f2a144410ad3de436f5b5c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454890"
---
# <a name="create-a-cognitive-services-account-using-the-azure-command-line-interfacecli"></a>Criar uma conta de serviços Cognitivos usando o Interface(CLI) de linha de comando do Azure

Neste início rápido, você aprenderá como se inscrever para serviços Cognitivos do Azure e criar uma conta que tenha uma assinatura de serviço único ou vários serviço, usando o [Interface(CLI) de linha de comando do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Esses serviços são representados pelo Azure [recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal), que permitem que você se conectar a um ou mais das APIs de serviços Cognitivos do Azure.

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura válida do Azure. [Criar uma conta](https://azure.microsoft.com/free/) gratuitamente.
* O [Interface(CLI) de linha de comando do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="install-the-azure-cli-and-sign-in"></a>Instalar a CLI do Azure e entrar no 

Instale a [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Para entrar em sua instalação local da CLI, execute as [login az](https://docs.microsoft.com/cli/azure/reference-index#az-login) comando:

```console
az login
```

Você também pode usar o verde **Experimente** botão para executar esses comandos em seu navegador.
 
## <a name="create-a-new-azure-cognitive-services-resource-group"></a>Criar um novo grupo de recursos de serviços Cognitivos do Azure

Suas assinaturas para os serviços Cognitivos são representadas por recursos do Azure. Cada conta dos serviços Cognitivos (e seus recursos do Azure associados) devem pertencer a um grupo de recursos do Azure.

### <a name="choose-your-resource-group-location"></a>Escolha o local do grupo de recursos

Para criar um recurso, será necessário um dos locais do Azure disponíveis para sua assinatura. Você pode recuperar uma lista de locais disponíveis com o [locais de lista az conta](/cli/azure/account#az_account_list) comando. A maioria dos serviços Cognitivos pode ser acessado de vários locais. Escolha a opção mais próxima de você ou ver quais locais estão disponíveis para o serviço.

> [!IMPORTANT]
> * Lembre-se o local do Azure, pois você precisará ao chamar os serviços Cognitivos do Azure.
> * A disponibilidade de alguns serviços Cognitivos pode variar por região. Para obter mais informações, consulte [produtos Azure por região](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services).  

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

Depois de ter seu local do azure, crie um novo grupo de recursos na CLI do Azure usando o [criar grupo de az](/cli/azure/group#az_group_create) comando.

No exemplo a seguir, substitua o local do azure `westus2` com um dos locais disponíveis para sua assinatura do Azure.

```azurecli-interactive
az group create \
    --name cognitive-services-resource-group \
    --location westus2
```

## <a name="create-a-cognitive-services-resource"></a>Criar um recurso dos Serviços Cognitivos

### <a name="choose-a-cognitive-service-and-pricing-tier"></a>Escolha um serviço cognitivo e o tipo de preço

Ao criar um novo recurso, você precisará saber o "tipo" de serviço que você deseja usar, juntamente com o [tipo de preço](https://azure.microsoft.com/pricing/details/cognitive-services/) (ou sku) desejado. Você usará essa e outras informações como parâmetros ao criar o recurso.

> [!NOTE]
> Muitos serviços Cognitivos tem uma camada gratuita, que você pode usar para experimentar o serviço. Para usar a camada gratuita, use `F0` como o sku do recurso.

### <a name="vision"></a>Visão

| Serviço                    | Tipo                      |
|----------------------------|---------------------------|
| Visual Computacional            | `ComputerVision`          |
| Visão personalizada - previsão | `CustomVision.Prediction` |
| Personalizado de visão - treinamento   | `CustomVision.Training`   |
| API de Detecção Facial                   | `Face`                    |
| Reconhecimento de Formulários            | `FormRecognizer`          |
| Reconhecimento de Tinta Digital             | `InkRecognizer`           |

### <a name="search"></a>Search

| Serviço            | Tipo                  |
|--------------------|-----------------------|
| Sugestão Automática do Bing   | `Bing.Autosuggest.v7` |
| Pesquisa Personalizada do Bing | `Bing.CustomSearch`   |
| Pesquisa de Entidade do Bing | `Bing.EntitySearch`   |
| Pesquisa do Bing        | `Bing.Search.v7`      |
| Verificação Ortográfica do Bing   | `Bing.SpellCheck.v7`  |

### <a name="speech"></a>Fala

| Serviço            | Tipo                 |
|--------------------|----------------------|
| Serviços de Fala    | `SpeechServices`     |
| Reconhecimento de fala | `SpeakerRecognition` |

### <a name="language"></a>Linguagem

| Serviço            | Tipo                |
|--------------------|---------------------|
| Noções básicas sobre o formulário | `FormUnderstanding` |
| LUIS               | `LUIS`              |
| QnA Maker          | `QnAMaker`          |
| Análise de texto     | `TextAnalytics`     |
| Tradução de Texto   | `TextTranslation`   |

### <a name="decision"></a>Decisão

| Serviço           | Tipo               |
|-------------------|--------------------|
| Detector de Anomalias  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Personalizador      | `Personalizer`     |

Você pode encontrar uma lista de serviços Cognitivos disponíveis "tipos" com o [lista de tipos de conta do cognitiveservices az](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-list-kinds) comando:

```azurecli-interactive
az cognitiveservices account list-kinds
```

### <a name="add-a-new-resource-to-your-resource-group"></a>Adicione um novo recurso ao grupo de recursos

Para criar e assinar um novo recurso de serviços Cognitivos, use o [criar uma conta do cognitiveservices az](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create) comando. Este comando adiciona um novo recurso faturável para o grupo de recursos criado anteriormente. Ao criar o novo recurso, você precisará saber o "tipo" de serviço que você deseja usar, juntamente com seu tipo de preço (ou sku) e um local do Azure:

Você pode criar um recurso de F0 (gratuito) para o Detector de anomalias, chamado `anomaly-detector-resource` com o comando a seguir.

```azurecli-interactive
az cognitiveservices account create \
    --name anomaly-detector-resource \
    --group cognitive-services-resource-group \
    --kind AnomalyDetector \
    --sku F0 \
    --location westus2 \
    --yes
```

## <a name="get-the-keys-for-your-subscription"></a>Obter as chaves para sua assinatura

Para fazer logon em sua instalação local do Interface(CLI) a linha de comando, use o [login az](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) comando.

```console
az login
```

Use o [lista de chaves de conta do cognitiveservices az](https://docs.microsoft.com/cli/azure/cognitiveservices/account/keys?view=azure-cli-latest#az-cognitiveservices-account-keys-list) comando para obter as chaves do recurso de serviço cognitivo.

```azurecli-interactive
    az cognitiveservices account keys list \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group
```

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>Limpar recursos

Se você quiser limpar e remover uma assinatura dos serviços Cognitivos, você pode excluir o recurso ou grupo de recursos. Também é excluir o grupo de recursos exclui todos os outros recursos associados com o grupo de recursos.

Para remover o grupo de recursos e seus recursos associados, incluindo a nova conta de armazenamento, use o comando de exclusão do grupo de az.

```azurecli-interactive
az group delete --name storage-resource-group
```

## <a name="see-also"></a>Consulte também

* [Autenticar solicitações para os Serviços Cognitivos do Azure](authentication.md)
* [O que é serviços Cognitivos do Azure?](Welcome.md)
* [Suporte de idioma natural](language-support.md)
* [Suporte de contêiner do docker](cognitive-services-container-support.md)
