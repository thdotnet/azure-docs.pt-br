---
title: Políticas de streaming nos Serviços de Mídia do Azure | Microsoft Docs
description: Este artigo fornece uma explicação sobre o que são as políticas de streaming e como são usados pelos Serviços de Mídia do Azure.
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
ms.openlocfilehash: a813c77e81e51bfe13e75ed6c8d0e24b4d0fa645
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66392918"
---
# <a name="streaming-policies"></a>Políticas de Streaming

Nos Serviços de Mídia do Azure v3, as [Políticas de Streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies) permitem definir protocolos de streaming e opções de criptografia para seus [Localizadores de Streaming](streaming-locators-concept.md). Serviços de mídia v3 fornece que algumas predefinidas Streaming políticas para que você pode usá-los diretamente para avaliação ou produção. 

Streaming de políticas predefinidas disponíveis no momento:<br/>
* 'Predefined_DownloadOnly'
* 'Predefined_ClearStreamingOnly'
* 'Predefined_DownloadAndClearStreaming'
* 'Predefined_ClearKey'
* 'Predefined_MultiDrmCencStreaming' 
* 'Predefined_MultiDrmStreaming'

A seguir "árvore de decisão" ajuda a escolher uma política predefinida de Streaming para seu cenário.

> [!IMPORTANT]
> * As propriedades de **Políticas de Streaming** que são do tipo Datetime estão sempre no formato UTC.
> * Você deve criar um conjunto limitado de políticas para sua conta de serviço de mídia e reutilizá-los para os Localizadores de Streaming sempre que as mesmas opções forem necessárias. Para saber mais, consulte [Cotas e limitações](limits-quotas-constraints.md).

## <a name="decision-tree"></a>Árvore de decisão

Clique na imagem para exibi-la em tamanho normal.  

<a href="./media/streaming-policy/large.png" target="_blank"><img src="./media/streaming-policy/large.png"></a> 

Se criptografar seu conteúdo, você precisará criar uma [política de chave de conteúdo](content-key-policy-concept.md), o **política de chave de conteúdo** não é necessária para limpar streaming ou baixar. 

Se você tiver requisitos especiais (por exemplo, se você deseja especificar protocolos diferentes, precisará usar um serviço de distribuição de chaves personalizado, ou precisará usar uma faixa de áudio claro), você poderá [criar](https://docs.microsoft.com/rest/api/media/streamingpolicies/create) uma política personalizada de Streaming. 

## <a name="get-a-streaming-policy-definition"></a>Obter uma definição de política de Streaming  

Se você quiser ver a definição de uma política de Streaming, use [obter](https://docs.microsoft.com/rest/api/media/streamingpolicies/get) e especifique o nome da política. Por exemplo: 

### <a name="rest"></a>REST

Solicitação:

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaServices/contosomedia/streamingPolicies/clearStreamingPolicy?api-version=2018-07-01
```

Resposta:

```
{
  "name": "clearStreamingPolicy",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaservices/contosomedia/streamingPolicies/clearStreamingPolicy",
  "type": "Microsoft.Media/mediaservices/streamingPolicies",
  "properties": {
    "created": "2018-08-08T18:29:30.8501486Z",
    "noEncryption": {
      "enabledProtocols": {
        "download": true,
        "dash": true,
        "hls": true,
        "smoothStreaming": true
      }
    }
  }
}
```

## <a name="filtering-ordering-paging"></a>Filtragem, classificação, paginação

Confira [Filtragem, classificação, paginação de entidades dos Serviços de Mídia](entities-overview.md).

## <a name="next-steps"></a>Próximas etapas

* [Transmitir um arquivo por streaming](stream-files-dotnet-quickstart.md)
* [Usar criptografia dinâmica AES-128 e o serviço de entrega de chave](protect-with-aes128.md)
* [Use criptografia dinâmica DRM e serviço de entrega de licenças](protect-with-drm.md)
