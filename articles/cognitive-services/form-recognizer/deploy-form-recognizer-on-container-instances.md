---
title: Executar as instâncias de contêiner do Azure
titleSuffix: Azure Cognitive Services
description: Implantar o contêiner do reconhecedor de formulário a uma instância de contêiner do Azure e testá-lo em um navegador da web.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: conceptual
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 3c424465678a9989940d92910c5d288fa2fb1cab
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711390"
---
# <a name="deploy-the-form-recognizer-container-to-azure-container-instances"></a>Implantar o contêiner do reconhecedor de formulário para instâncias de contêiner do Azure

Saiba como implantar os serviços Cognitivos [formulário reconhecedor](form-recognizer-container-howto.md) contêiner do Azure [instâncias de contêiner](https://docs.microsoft.com/azure/container-instances/). Este procedimento demonstra a criação de um recurso do reconhecedor de formulário do Azure. Em seguida, discutiremos a efetuar pull da imagem de contêiner associado. Por fim, podemos destacar a capacidade de exercitar a orquestração dos dois em um navegador. Usando contêineres pode mudar a atenção dos desenvolvedores para longe de gerenciar a infraestrutura para em vez disso, com foco no desenvolvimento de aplicativos.

[!INCLUDE [Prerequisites](../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-container-registry"></a>Solicitar acesso ao Registro de contêiner

Primeiro, é necessário preencher e enviar o [formulário de solicitação de acesso a Contêineres do Reconhecimento de Formulários dos Serviços Cognitivos](https://aka.ms/FormRecognizerRequestAccess) para solicitar acesso ao contêiner. Fazer isso também fará sua inscrição na Pesquisa Visual Computacional. Você não precisa se inscrever no formulário de solicitação da Pesquisa Visual Computacional separadamente. 

[!INCLUDE [Request access](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Form Recognizer resource](includes/create-resource.md)]

[!INCLUDE [Create an Form Recognizer container on Azure Container Instances](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Containers Next Steps](../containers/includes/containers-next-steps.md)]
