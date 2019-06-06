---
title: Políticas de Chave de Conteúdo nos Serviços de Mídia – Azure | Microsoft Docs
description: Este artigo fornece uma explicação de quais são as Políticas de Chave de Conteúdo e como elas são usadas pelos Serviços de Mídia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/28/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: a597ab3519f4ba1696e111622541bcab89488558
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66425421"
---
# <a name="content-key-policies"></a>Políticas da Chave de Conteúdo

Com os Serviços de Mídia, é possível entregar o conteúdo ao vivo e sob demanda criptografado dinamicamente com a criptografia AES (AES-128) ou qualquer um dos três principais sistemas DRM (gerenciamento de direitos digitais): Microsoft PlayReady, Google Widevine e Apple FairPlay. Os serviços de mídia também fornecem um serviço de distribuição de chaves AES e licenças DRM (PlayReady, Widevine e FairPlay) para os clientes autorizados. 

Para especificar opções de criptografia em seu fluxo, você precisará criar uma [política de Streaming](streaming-policy-concept.md) e associá-la com seus [localizador de Streaming](streaming-locators-concept.md). Você cria o [política de chave de conteúdo](https://docs.microsoft.com/rest/api/media/contentkeypolicies) para configurar como a chave de conteúdo (que fornece acesso seguro aos seus [ativos](assets-concept.md)) é entregue para os clientes finais. Você precisa definir os requisitos (restrições) na política de chave de conteúdo que devem ser atendidos para que as chaves com a configuração especificada a ser entregue aos clientes. Essa política de chave de conteúdo não é necessária para limpar streaming ou baixar. 

Normalmente, você associar seu **política de chave de conteúdo** com seu [localizador de Streaming](streaming-locators-concept.md). Como alternativa, você pode especificar a política de chave de conteúdo dentro de um [política de Streaming](streaming-policy-concept.md) (quando criar uma política personalizada de Streaming para cenários avançados). 

É recomendável para permitir que os serviços de mídia para gerar chaves de conteúdo. Normalmente, você usaria uma chave de vida útil longa e verificar a existência de políticas com **obter**. Para ter acesso à chave, você precisa chamar um método de ação separado para obter segredos ou as credenciais. Confira o exemplo a seguir.

As **Políticas de Chave de Conteúdo** podem ser atualizadas. Pode levar até 15 minutos para que os caches de entrega de Chaves atualizem e selecionem a política atualizada. 

> [!IMPORTANT]
> * As propriedades de **Políticas de Conteúdo de Chave** que são do tipo Datetime estão sempre no formato UTC.
> * Você deve criar um conjunto limitado de políticas para sua conta de serviço de mídia e reutilizá-lo para os Localizadores de Streaming sempre que as mesmas opções forem necessárias. Para saber mais, consulte [Cotas e limitações](limits-quotas-constraints.md).

### <a name="example"></a>Exemplo

Para obter a chave, use **GetPolicyPropertiesWithSecretsAsync**, conforme mostrado na [obter uma chave de assinatura da política existente](get-content-key-policy-dotnet-howto.md#get-contentkeypolicy-with-secrets) exemplo.

## <a name="filtering-ordering-paging"></a>Filtragem, classificação, paginação

Confira [Filtragem, classificação, paginação de entidades dos Serviços de Mídia](entities-overview.md).

## <a name="next-steps"></a>Próximas etapas

* [Usar criptografia dinâmica AES-128 e o serviço de entrega de chave](protect-with-aes128.md)
* [Use criptografia dinâmica DRM e serviço de entrega de licenças](protect-with-drm.md)
* [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted)
