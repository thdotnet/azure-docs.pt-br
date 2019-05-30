---
title: Localizadores de Transmissão nos Serviços de Mídia do Azure | Microsoft Docs
description: Este artigo fornece uma explicação sobre o que são os Localizadores de Transmissão e como eles são usados pelos Serviços de Mídia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/26/2019
ms.author: juliako
ms.openlocfilehash: 5897b7df2460257784c40eb974c473573ec4003d
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299161"
---
# <a name="streaming-locators"></a>Localizadores de Streaming

Para fazer vídeos na saída do ativo disponível para clientes de reprodução, você precisa criar um [Localizador de Streaming](https://docs.microsoft.com/rest/api/media/streaminglocators) e, em seguida, criar URLs de streaming. Para criar uma URL, você precisará concatenar o nome de host do ponto de extremidade de Streaming e o caminho do localizador de Streaming. Para um exemplo de .NET, veja [Obter um Localizador de Streaming](stream-files-tutorial-with-api.md#get-a-streaming-locator).

O processo de criação de um **Localizador de Streaming** é chamado de publicação. Por padrão, o **Localizador de Streaming** é válido imediatamente após você fazer as chamadas à API e dura até ser excluído, a menos que você configure os horários de início e término opcionais. 

Ao criar uma **localizador de Streaming**, você deve especificar um **ativo** nome e uma **política Streaming** nome. Para mais informações, consulte os seguintes tópicos:

* [Ativos](assets-concept.md)
* [Políticas de Streaming](streaming-policy-concept.md)
* [Políticas de Chave de Conteúdo](content-key-policy-concept.md)

Você também pode especificar a hora de início e término em que o localizador de Streaming, que só permitirá que seu usuário reproduzir o conteúdo entre essas horas (por exemplo, entre 5/1/2019 para 5/5/2019).  

## <a name="considerations"></a>Considerações

* **Os localizadores de streaming** não são atualizáveis. 
* As propriedades de **Localizadores de Streaming** que são do tipo Datetime estão sempre no formato UTC.
* Você deve criar um conjunto limitado de políticas para sua conta de serviço de mídia e reutilizá-los para os Localizadores de Streaming sempre que as mesmas opções forem necessárias. Para saber mais, consulte [Cotas e limitações](limits-quotas-constraints.md).

## <a name="create-streaming-locators"></a>Criar os localizadores de Streaming  

### <a name="not-encrypted"></a>Não criptografado

Se você deseja transmitir seu arquivo no-the-clear (não criptografado), definir a política de streaming clara predefinida: para 'Predefined_ClearStreamingOnly' (no .NET, você pode usar o enum PredefinedStreamingPolicy.ClearStreamingOnly).

```csharp
StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = PredefinedStreamingPolicy.ClearStreamingOnly
    });
```

### <a name="encrypted"></a>Criptografado 

Se você precisar criptografar seu conteúdo com a criptografia CENC, defina sua política para 'Predefined_MultiDrmCencStreaming'. A criptografia Widevine será aplicada a um fluxo de traço e PlayReady a suave. A chave será entregue a um cliente de reprodução com base nas licenças DRM configuradas.

```csharp
StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = "Predefined_MultiDrmCencStreaming",
        DefaultContentKeyPolicyName = contentPolicyName
    });
```

Se você também deseja criptografar seu stream HLS com CBCS (FairPlay), use 'Predefined_MultiDrmStreaming'.

## <a name="associate-filters-with-streaming-locators"></a>Associar filtros localizadores de Streaming

Ver [filtros: associar com localizadores de Streaming](filters-concept.md#associating-filters-with-streaming-locator).

## <a name="filter-order-page-streaming-locator-entities"></a>Filtrar, ordem, entidades de localizador de Streaming de página

Confira [Filtragem, classificação, paginação de entidades dos Serviços de Mídia](entities-overview.md).

## <a name="list-streaming-locators-by-asset-name"></a>Localizadores de Streaming de lista por nome do ativo

Para obter os localizadores de Streaming com base no nome do ativo associado, use as seguintes operações:

|Linguagem|API|
|---|---|
|REST|[liststreaminglocators](https://docs.microsoft.com/rest/api/media/assets/liststreaminglocators)|
|CLI|[az ams asset list-streaming-locators](https://docs.microsoft.com/cli/azure/ams/asset?view=azure-cli-latest#az-ams-asset-list-streaming-locators)|
|.NET|[ListStreamingLocators](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.assetsoperationsextensions.liststreaminglocators?view=azure-dotnet#Microsoft_Azure_Management_Media_AssetsOperationsExtensions_ListStreamingLocators_Microsoft_Azure_Management_Media_IAssetsOperations_System_String_System_String_System_String_)|
|Java|[AssetStreamingLocator](https://docs.microsoft.com/java/api/com.microsoft.azure.management.mediaservices.v2018_07_01.assetstreaminglocator?view=azure-java-stable)|
|Node.js|[listStreamingLocators](https://docs.microsoft.com/javascript/api/azure-arm-mediaservices/assets?view=azure-node-latest#liststreaminglocators-string--string--string--object-)|

## <a name="also-see"></a>Consulte também

* [Ativos](assets-concept.md)
* [Políticas de Streaming](streaming-policy-concept.md)
* [Políticas de Chave de Conteúdo](content-key-policy-concept.md)

## <a name="next-steps"></a>Próximas etapas

[Tutorial: Carregar, codificar e transmitir vídeos usando .NET](stream-files-tutorial-with-api.md)
