---
title: Executar as instâncias de contêiner do Azure
titleSuffix: Azure Cognitive Services
description: Implantar o contêiner de serviço de fala em uma instância de contêiner do Azure e testá-lo em um navegador da web.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 062765be22135b12abb29ff6f7ce8a772c67adae
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711511"
---
# <a name="deploy-the-speech-service-container-to-azure-container-instances"></a>Implantar o contêiner de serviço de fala em instâncias de contêiner do Azure

Saiba como implantar os serviços Cognitivos [serviço de fala](speech-container-howto.md) contêiner do Azure [instâncias de contêiner](https://docs.microsoft.com/azure/container-instances/). Este procedimento demonstra a criação de um recurso do serviço de fala do Azure. Em seguida, discutiremos a efetuar pull da imagem de contêiner associado. Por fim, podemos destacar a capacidade de exercitar a orquestração dos dois em um navegador. Usando contêineres pode mudar a atenção dos desenvolvedores para longe de gerenciar a infraestrutura para em vez disso, com foco no desenvolvimento de aplicativos.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-container-registry"></a>Solicitar acesso ao Registro de contêiner

Você deve primeiro concluir e enviar o [formulário de solicitação de contêineres de fala de serviços Cognitivos](https://aka.ms/speechcontainerspreview/) para solicitar acesso ao contêiner. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Speech Service resource](includes/create-speech-resource.md)]

[!INCLUDE [Create an Speech Service container on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]
