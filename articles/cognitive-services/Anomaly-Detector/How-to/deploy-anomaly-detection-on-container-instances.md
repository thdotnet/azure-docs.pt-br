---
title: Executar as instâncias de contêiner do Azure
titleSuffix: Azure Cognitive Services
description: Implantar o contêiner do Detector de anomalias em uma instância de contêiner do Azure e testá-lo em um navegador da web.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: b5adc3ed9234050d3977e812202717a0ce83e842
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711687"
---
# <a name="deploy-an-anomaly-detector-container-to-azure-container-instances"></a>Implantar um contêiner do Detector de anomalias em instâncias de contêiner do Azure

Saiba como implantar os serviços Cognitivos [Detector de anomalias](../anomaly-detector-container-howto.md) contêiner do Azure [instâncias de contêiner](https://docs.microsoft.com/azure/container-instances/). Este procedimento demonstra a criação de um recurso do Detector de anomalias. Em seguida, discutiremos a efetuar pull da imagem de contêiner associado. Por fim, podemos destacar a capacidade de exercitar a orquestração dos dois em um navegador. Usando contêineres pode mudar a atenção dos desenvolvedores para longe de gerenciar a infraestrutura para em vez disso, com foco no desenvolvimento de aplicativos.

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-private-container-registry"></a>Solicitar acesso ao registro de contêiner privado

Você deve primeiro concluir e enviar o [formulário de solicitação de contêiner do Detector de anomalias](https://aka.ms/adcontainer) para solicitar acesso ao contêiner.

[!INCLUDE [Request access](../../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Anomaly Detector resource](../includes/create-anomaly-detector-resource.md)]

[!INCLUDE [Create an Anomaly Detector container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../../containers/includes/containers-next-steps.md)]