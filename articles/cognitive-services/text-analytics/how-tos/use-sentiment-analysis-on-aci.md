---
title: Executar as instâncias de contêiner do Azure
titleSuffix: Text Analytics - Azure Cognitive Services
description: Implantar os contêineres de análise de texto com a imagem de análise de sentimento, para a instância de contêiner do Azure e testá-lo em um navegador da web.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: a82b5a1cbed662289d3a406a61fdd19a3ca8861b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454978"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-container-instances-aci"></a>Implantar um contêiner de análise de sentimento para instâncias de contêiner do Azure (ACI)

Saiba como implantar os serviços Cognitivos [análise de texto](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) contêiner com a imagem de análise de sentimento para o Azure [instâncias de contêiner](https://docs.microsoft.com/azure/container-instances/) (ACI). Esse procedimento é um exemplo de criação de um recurso de análise de texto, a criação de uma imagem de análise de sentimento associada e a capacidade de exercitar essa orquestração dos dois em um navegador. Usando contêineres pode mudar a atenção dos desenvolvedores para longe de gerenciar a infraestrutura para em vez disso, com foco no desenvolvimento de aplicativos.

## <a name="prerequisites"></a>Pré-requisitos

* Use uma assinatura do Azure. Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances (ACI)](../../containers/includes/create-aci-resource.md)]

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Próximas etapas 

* Use mais [Contêineres de Serviços Cognitivos](../../cognitive-services-container-support.md)
* Use o [serviço conectado de análise de texto](../vs-text-connected-service.md)
