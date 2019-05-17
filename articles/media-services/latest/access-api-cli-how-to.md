---
title: Acessar a API dos Serviços de Mídia do Azure – CLI do Azure | Microsoft Docs
description: Execute as etapas desta instrução para acessar a API dos Serviços de Mídia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: mvc
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 5dbcf446a609adcd0f1902fcca2ac19ad87f17b1
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65779663"
---
# <a name="access-azure-media-services-api-with-the-azure-cli"></a>Acessar a API dos Serviços de Mídia do Azure com a CLI do Azure
 
Para usar a autenticação de entidade de serviço do AD do Azure para conectar-se à API de serviços de mídia do Azure, seu aplicativo precisa solicitar um token do AD do Azure que tem os seguintes parâmetros:

* Ponto de extremidade de locatário do Azure AD
* URI de recursos dos Serviços de Mídia
* URI de recurso dos Serviços de Mídia REST
* Valores do aplicativo do Azure AD: a ID do cliente e o segredo do cliente

Para obter explicações detalhadas, consulte [acessando os serviços de mídia v3 APIs](media-services-apis-overview.md#accessing-the-azure-media-services-api).

Este artigo mostra como usar a CLI do Azure para criar um aplicativo e uma entidade de serviço do Azure AD e obter os valores necessários para acessar os recursos dos Serviços de Mídia do Azure.

## <a name="prerequisites"></a>Pré-requisitos 

[Crie uma conta de Serviços de Mídia](create-account-cli-how-to.md).

Lembre-se dos valores que você usou para o nome do grupo de recursos e o nome da conta de Serviços de Mídia.
 
[!INCLUDE [media-services-cli-instructions](../../../includes/media-services-cli-instructions.md)]

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="see-also"></a>Consulte também

- [Dimensionar as unidades reservadas para mídia – CLI](media-reserved-units-cli-how-to.md)
- [Criar uma conta dos Serviços de Mídia – CLI](create-account-cli-how-to.md) 
- [Redefinir as credenciais da conta – CLI](cli-reset-account-credentials.md)
- [Criar ativos – CLI](cli-create-asset.md)
- [Carregar um arquivo – CLI](cli-upload-file-asset.md)
- [Criar transformações – CLI](cli-create-transform.md)
- [Codificar com uma transformação personalizada - CLI](custom-preset-cli-howto.md)
- [Criar trabalhos – CLI](cli-create-jobs.md)
- [Criar uma EventGrid – CLI](job-state-events-cli-how-to.md)
- [Publicar um ativo – CLI](cli-publish-asset.md)
- [Filtro – CLI](filters-dynamic-manifest-cli-howto.md)
- [CLI do Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)

## <a name="next-steps"></a>Próximas etapas

O ponto de extremidade de Streaming do qual você deseja transmitir o conteúdo deve estar no estado em execução. O seguinte comando CLI começa seu ponto de extremidade de Streaming padrão:

`az ams streaming-endpoint start -n default -a <amsaccount> -g <amsResourceGroup>`

