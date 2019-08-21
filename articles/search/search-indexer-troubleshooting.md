---
title: Solução de problemas comuns do indexador de pesquisa – Azure Search
description: Corrija erros e problemas comuns com indexadores no Azure Search, incluindo conexão da fonte de dados, firewall e documentos ausentes.
author: mgottein
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: magottei
ms.openlocfilehash: 4692be287e9b38cf116107d2e7c1043f23a6b34b
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69640606"
---
# <a name="troubleshooting-common-indexer-issues-in-azure-search"></a>Solução de problemas comuns no Azure Search

Indexadores poderão ser executados em um número de problemas durante a indexação de dados para o Azure Search. As principais categorias de dados são:

* [Conectar-se a fontes de dados](#data-source-connection-errors)
* [Processamento de documentos](#document-processing-errors)
* [Ingestão de documentos para um índice](#index-errors)

## <a name="data-source-connection-errors"></a>Erros de Conexão da Fonte de Dados

### <a name="blob-storage"></a>Armazenamento de Blob

#### <a name="storage-account-firewall"></a>Failover da conta de Armazenamento

O Armazenamento do Microsoft Azure fornece um firewall configurável. Por padrão, o firewall está desabilitado para que o Azure Search possa se conectar à sua conta de armazenamento.

Não há nenhuma mensagem de erro específica quando o firewall estiver habilitado. Normalmente, os erros de firewall são semelhantes a `The remote server returned an error: (403) Forbidden`.

Você pode verificar se o firewall está habilitado no [portal](https://docs.microsoft.com/azure/storage/common/storage-network-security#azure-portal). A única solução suportada é desabilitar o firewall escolhendo permitir o acesso de [' todas as redes '](https://docs.microsoft.com/azure/storage/common/storage-network-security#azure-portal).

Se o seu indexador não tiver um qualificable anexado, você _poderá_ tentar [Adicionar uma exceção](https://docs.microsoft.com/azure/storage/common/storage-network-security#managing-ip-network-rules) para os endereços IP do serviço de pesquisa. No entanto, esse cenário não tem suporte e não tem a garantia de funcionar.

Você pode descobrir o endereço IP do serviço de pesquisa executando ping no FQDN (`<your-search-service-name>.search.windows.net`).

### <a name="cosmos-db"></a>Cosmos DB

#### <a name="indexing-isnt-enabled"></a>A indexação não está habilitada

O Azure Search tem uma dependência implícita sobre a indexação do Azure Cosmos DB. Se você desativar a indexação automática no Azure Cosmos DB, o Azure Search retorna um estado de êxito, mas não ao conteúdo do contêiner de índice. Para obter instruções sobre como verificar as configurações e ative a indexação, consulte [Gerenciar a indexação no Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/how-to-manage-indexing-policy#use-the-azure-portal).

## <a name="document-processing-errors"></a>Erros de processamento do documento

### <a name="unprocessable-or-unsupported-documents"></a>Documentos não processável ou sem suporte

Os documentos do indexador [de BLOB cujos formatos de documento são explicitamente compatíveis.](search-howto-indexing-azure-blob-storage.md#supported-document-formats). Às vezes, um contêiner de armazenamento de Blob contém documentos não compatíveis. Outras vezes, pode haver um documentos problemáticos. Você pode evitar interromper seu indexador nesses documentos ao [Alternar as opções de configuração](search-howto-indexing-azure-blob-storage.md#dealing-with-errors):

```
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
Content-Type: application/json
api-key: [admin key]

{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "failOnUnsupportedContentType" : false, "failOnUnprocessableDocument" : false } }
}
```

### <a name="missing-document-content"></a>Conteúdo do documento ausente

O indexador de Blob [localiza e extrai o texto de blobs em um contêiner](search-howto-indexing-azure-blob-storage.md#how-azure-search-indexes-blobs). Alguns problemas com a extração de texto incluem:

* O documento contém apenas imagens digitalizadas. Blobs PDF que têm conteúdo não textual, como imagens digitalizadas (JPGs), não produzem resultados em um pipeline de indexação de Blob padrão. Se você tiver conteúdo de imagem com elementos de texto, poderá usar a [pesquisa cognitiva](cognitive-search-concept-image-scenarios.md) para localizar e extrair o texto.
* O indexador de Blob está configurado para metadados do índice. Para extrair o conteúdo, o indexador de Blob deve ser configurado para [extrair o conteúdo e metadados](search-howto-indexing-azure-blob-storage.md#controlling-which-parts-of-the-blob-are-indexed):

```
PUT https://[service name].search.windows.net/indexers/[indexer name]?api-version=2019-05-06
Content-Type: application/json
api-key: [admin key]

{
  ... other parts of indexer definition
  "parameters" : { "configuration" : { "dataToExtract" : "contentAndMetadata" } }
}
```

## <a name="index-errors"></a>Erros de índice

### <a name="missing-documents"></a>Documentos ausentes

Os indexadores encontram documentos de uma [fonte de dados](https://docs.microsoft.com/rest/api/searchservice/create-data-source). Às vezes, um documento da fonte de dados que deve ter sido indexado, aparece faltando em um índice. Há duas razões comuns para que esses erros ocorram:

* O documento ainda não foi indexado. Verifique o portal para uma execução bem-sucedida do indexador.
* O documento foi atualizado após o execução do indexador. Se o indexador estiver em um [agendamento](https://docs.microsoft.com/rest/api/searchservice/create-indexer#indexer-schedule), eventualmente será executado novamente e pegará o documento.
* A [consulta](https://docs.microsoft.com/rest/api/searchservice/create-data-source#request-body-syntax) especificada nos dados de origem exclui o documento. Os indexadores não podem indexar documentos que não fazem parte da fonte de dados.
* [Mapeamentos de campo](https://docs.microsoft.com/rest/api/searchservice/create-indexer#fieldmappings) ou [pesquisa cognitiva](https://docs.microsoft.com/azure/search/cognitive-search-concept-intro) alteraram o documento e parecem diferentes do esperado.
* Use a [API Procurar documento](https://docs.microsoft.com/rest/api/searchservice/lookup-document) para localizar seu documento.
