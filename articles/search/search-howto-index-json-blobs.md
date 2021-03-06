---
title: Indexando blobs JSON com o indexador de blobs da Pesquisa do Azure
description: Indexando blobs JSON com o indexador de blobs da Pesquisa do Azure
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 57e32e51-9286-46da-9d59-31884650ba99
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 12/15/2016
ms.author: eugenesh
translationtype: Human Translation
ms.sourcegitcommit: fc2f30569acc49dd383ba230271989eca8a14423
ms.openlocfilehash: de7af5419aa423734ad06b236e0edf61fbb0cad1

---

# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indexando blobs JSON com o indexador de blobs da Pesquisa do Azure
Este artigo mostra como configurar o indexador de pesquisa de blob da Pesquisa do Azure para extrair o conteúdo estruturado de blobs que contêm JSON.

## <a name="scenarios"></a>Cenários
Por padrão, o [indexador de blobs da Pesquisa do Azure](search-howto-indexing-azure-blob-storage.md) analisa blobs JSON como um único bloco de texto. Muitas vezes, você deseja preservar a estrutura dos seus documentos JSON. Por exemplo, considerando o documento JSON

    {
        "article" : {
             "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

talvez você queira analisá-lo em uma documento da Pesquisa do Azure com os campos "text", "datePublished" e "tags".

Como alternativa, quando seus blobs contêm uma **matriz de objetos JSON**, convém que cada elemento da matriz se torne um documento separado da Pesquisa do Azure. Por exemplo, um blob com este JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

você pode preencher o índice da Pesquisa do Azure com três documentos separados, cada um com os campos "id" e "text".

> [!IMPORTANT]
> Esta funcionalidade está em preview. Ela está disponível somente na API REST que usa a versão **2015-02-28-Preview**. Lembre-se de que as APIs de visualização servem para teste e avaliação, e não devem ser usadas em ambientes de produção.
>
>

## <a name="setting-up-json-indexing"></a>Configuração da indexação de JSON
Para indexar blobs JSON, defina o parâmetro de configuração `parsingMode` como `json` (para indexar cada blob como um único documento) ou como `jsonArray` (se os seus blobs contiverem uma matriz JSON):

    {
      "name" : "my-json-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "json" | "jsonArray" } }
    }

Se necessário, use **mapeamentos de campo** para selecionar as propriedades do documento JSON de origem usado para preencher o índice de pesquisa de destino.  Isso é descrito em detalhes abaixo.

> [!IMPORTANT]
> Ao usar o modo de análise `json` ou `jsonArray`, o Azure Search pressupõe que todos os blobs em sua fonte de dados serão JSON. Se você precisar dar suporte a uma combinação de blobs JSON e não JSON na mesma fonte de dados, informe-nos em nosso [site UserVoice](https://feedback.azure.com/forums/263029-azure-search).
>
>

## <a name="using-field-mappings-to-build-search-documents"></a>Usando mapeamentos de campo para criar documentos de pesquisa
Atualmente, a Pesquisa do Azure não pode indexar documentos JSON arbitrários diretamente porque dá suporte somente a tipos de dados primitivos, matrizes de cadeia de caracteres e pontos GeoJSON. No entanto, você pode usar **mapeamentos de campo** para separar partes do documento JSON e "elevá-los" para campos de nível superior do documento de pesquisa. Para saber mais sobre os conceitos básicos de mapeamentos de campo, veja [Os mapeamentos de campo do indexador da Pesquisa do Azure reduzem as diferenças entre fontes de dados e índices de pesquisa](search-indexer-field-mappings.md).

Voltando ao nosso documento JSON de exemplo:

    {
        "article" : {
             "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Digamos que você tenha um índice de pesquisa com os seguintes campos: `text` do tipo Edm.String, `date` do tipo Edm.DateTimeOffset e `tags` do tipo Collection(Edm.String). Para mapear seu JSON para a forma desejada, use os seguintes mapeamentos de campo:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

Os nomes dos campos de origem nos mapeamentos são especificados usando a notação [JSON Pointer](http://tools.ietf.org/html/rfc6901) . Comece com uma barra invertida para referir-se à raiz do documento JSON e chegue à propriedade desejada (em nível arbitrário de aninhamento) usando um caminho separado por barra invertida.

Você também pode se referir a elementos individuais da matriz usando um índice com base em zero. Por exemplo, para selecionar o primeiro elemento da matriz "tags" do exemplo anterior, use um mapeamento de campo como este:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Se o nome de um campo de origem em um caminho de mapeamento de campo se referir a uma propriedade que não existe em JSON, esse mapeamento será ignorado sem erro. Isso é feito para que possamos dar suporte a documentos com um esquema diferente (o que é um caso de uso comum). Como não há nenhuma validação, você precisa tomar cuidado para evitar erros de digitação na especificação do mapeamento do campo.
>
>

Se seus documentos JSON contiverem somente propriedades simples de nível superior, talvez você não precise de mapeamentos de campo. Por exemplo, se o seu JSON tiver esta aparência, as propriedades "text", "datePublished" e "tags" de nível superior mapearão diretamente para os campos correspondentes no índice de pesquisa:

    {
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13"
       "tags" : [ "search", "storage", "howto" ]    
     }

## <a name="indexing-nested-json-arrays"></a>Indexação de matrizes aninhadas de JSON
E se você quiser indexar uma matriz de objetos JSON, mas essa matriz estiver aninhada em algum lugar dentro do documento? Você pode escolher qual propriedade contém a matriz usando a propriedade de configuração `documentRoot` . Por exemplo, se o seu blob tiver esta aparência:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" },
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    }

use esta configuração para indexar a matriz contida na propriedade "level2":

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }


## <a name="request-examples"></a>Exemplos de solicitação
Reunindo tudo isso, estes são os exemplos das cargas completas.

Fonte de dados:

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Indexador:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }

## <a name="help-us-make-azure-search-better"></a>Ajude-nos a aprimorar a Pesquisa do Azure
Se você tiver solicitações de recursos ou ideias para o aperfeiçoamentos, entre em contato conosco pelo [site UserVoice](https://feedback.azure.com/forums/263029-azure-search/).



<!--HONumber=Nov16_HO3-->


