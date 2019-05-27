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
ms.date: 05/22/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 68778cea51144ec33efd4d5843a51b489ea17ca4
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155733"
---
# <a name="content-key-policies"></a>Políticas da Chave de Conteúdo

Com os Serviços de Mídia, é possível entregar o conteúdo ao vivo e sob demanda criptografado dinamicamente com a criptografia AES (AES-128) ou qualquer um dos três principais sistemas DRM (gerenciamento de direitos digitais): Microsoft PlayReady, Google Widevine e Apple FairPlay. Os serviços de mídia também fornecem um serviço de distribuição de chaves AES e licenças DRM (PlayReady, Widevine e FairPlay) para os clientes autorizados.

Para especificar opções de criptografia em seu fluxo, você precisará criar uma [política de Streaming](streaming-policy-concept.md) e associá-la com seus [localizador de Streaming](streaming-locators-concept.md). Você precisa criar uma [política de chave de conteúdo](https://docs.microsoft.com/rest/api/media/contentkeypolicies) para configurar como a chave de conteúdo (que fornece acesso seguro aos seus [ativos](assets-concept.md)) é entregue para os clientes finais. O **política de chave de conteúdo** também está associado ao seu **localizador de Streaming**. Você precisa definir os requisitos (restrições) na política de chave de conteúdo que devem ser atendidos para que as chaves com a configuração especificada a ser entregue aos clientes. 

É recomendável para permitir que os serviços de mídia para gerar chaves de conteúdo. Normalmente, você usaria uma chave de vida útil longa e verificar a existência de políticas com **obter**. Para ter acesso à chave, você precisa chamar um método de ação separado para obter segredos ou as credenciais. Confira o exemplo a seguir.

As **Políticas de Chave de Conteúdo** podem ser atualizadas. Por exemplo, você poderá querer atualizar a política se precisar fazer uma rotação de chaves. Você pode atualizar a chave de verificação principal e a lista de chaves de verificação alternativas na política existente. Pode levar até 15 minutos para que os caches de entrega de Chaves atualizem e selecionem a política atualizada. 

> [!IMPORTANT]
> * As propriedades de **Políticas de Conteúdo de Chave** que são do tipo Datetime estão sempre no formato UTC.
> * Você deve criar um conjunto limitado de políticas para sua conta de serviço de mídia e reutilizá-lo para os Localizadores de Streaming sempre que as mesmas opções forem necessárias. Para saber mais, consulte [Cotas e limitações](limits-quotas-constraints.md).

## <a name="example"></a>Exemplo

Para obter a chave, use **GetPolicyPropertiesWithSecretsAsync**, conforme mostrado no exemplo a seguir.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="filtering-ordering-paging"></a>Filtragem, classificação, paginação

Confira [Filtragem, classificação, paginação de entidades dos Serviços de Mídia](entities-overview.md).

## <a name="next-steps"></a>Próximas etapas

* [Usar criptografia dinâmica AES-128 e o serviço de entrega de chave](protect-with-aes128.md)
* [Use criptografia dinâmica DRM e serviço de entrega de licenças](protect-with-drm.md)
* [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted)
