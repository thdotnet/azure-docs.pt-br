---
title: Indexar blobs CSV com o indexador de blobs do Azure Search – Azure Search
description: Rastrear blobs CSV no armazenamento de blobs do Azure para pesquisa de texto completo usando um índice do Azure Search. Os indexadores automatizam a ingestão de dados para fontes de dados selecionadas, como o armazenamento de blobs do Azure.
ms.date: 05/02/2019
author: mgottein
manager: nitinme
ms.author: magottei
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: b135fd1a0758567a7b504996bf442a913741fe59
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69656768"
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indexando blobs CSV com o indexador de blobs do Azure Search

> [!Note]
> o modo de análise delimitedText está em visualização e não se destina ao uso em produção. A [API REST versão 2019-05-06-versão prévia](search-api-preview.md) fornece esse recurso. Não há suporte para SDK do .NET no momento.
>

Por padrão, o [indexador de blobs do Azure Search](search-howto-indexing-azure-blob-storage.md) analisa blobs de texto delimitado como um único bloco de texto. No entanto, com blobs contendo dados CSV, o ideal é tratar cada linha no blob como um documento separado. Por exemplo, considerando o seguinte texto delimitado, você pode querer analisá-lo em dois documentos, cada um contendo os campos "id", "datePublished" e "tags": 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

Neste artigo, você aprenderá a analisar BLOBs CSV com um Azure Search blob indexerby definindo o modo de `delimitedText` análise. 

> [!NOTE]
> Siga as recomendações de configuração do indexador em indexação de [um para muitos](search-howto-index-one-to-many-blobs.md) para produzir vários documentos de pesquisa de um blob do Azure.

## <a name="setting-up-csv-indexing"></a>Configurando a indexação de CSV
Para indexar BLOBs CSV, crie ou atualize uma definição de indexador `delimitedText` com o modo de análise em uma solicitação [criar indexador](https://docs.microsoft.com/rest/api/searchservice/create-indexer) :

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

`firstLineContainsHeaders` indica que a primeira linha (não vazia) de cada blob contém cabeçalhos.
Se os blobs não contêm uma linha de cabeçalho inicial, os cabeçalhos devem ser especificados na configuração do indexador: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

É possível personalizar o caractere delimitador usando a configuração `delimitedTextDelimiter`. Por exemplo:

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextDelimiter" : "|" } }

> [!NOTE]
> Atualmente, apenas a codificação UTF-8 tem suporte. Se você precisar de suporte para outras codificações, vote nele em [UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [!IMPORTANT]
> Quando você usa o modo de análise de texto delimitado, o Azure Search pressupõe que todos os blobs em sua fonte de dados serão CSV. Se você precisar dar suporte a uma combinação de blobs CSV e não CSV na mesma fonte de dados, vote nela em [UserVoice](https://feedback.azure.com/forums/263029-azure-search).
> 
> 

## <a name="request-examples"></a>Exemplos de solicitação
Reunindo tudo isso, estes são os exemplos de carga completa. 

Fonte de dados: 

    POST https://[service name].search.windows.net/datasources?api-version=2019-05-06-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indexador:

    POST https://[service name].search.windows.net/indexers?api-version=2019-05-06-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Ajude-nos a aprimorar o Azure Search
Caso você tenha solicitações de recursos ou ideias para melhorias, dê sua opinião em [site UserVoice](https://feedback.azure.com/forums/263029-azure-search/).

