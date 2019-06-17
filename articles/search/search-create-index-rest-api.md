---
title: 'Início Rápido: PowerShell e APIs REST - Azure Search'
description: Criar, carregar e consultar um índice usando o PowerShell Invoke-RestMethod e a API REST do Azure Search.
ms.date: 06/10/2019
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 7121bfceb177a7dc06d1c2a65b7c3edfca1d8c31
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063645"
---
# <a name="quickstart-create-an-azure-search-index-using-powershell"></a>Início Rápido: Criar um índice de Azure Search usando o PowerShell
> [!div class="op_single_selector"]
> * [PowerShell (REST)](search-create-index-rest-api.md)
> * [C#](search-create-index-dotnet.md)
> * [Postman (REST)](search-fiddler.md)
> * [Python](search-get-started-python.md)
> * [Portal](search-create-index-portal.md)
> 

Este artigo orienta você pelo processo de criar, carregar e consultar uma Azure Search [índice](search-what-is-an-index.md) usando o PowerShell e o [as APIs REST do Azure Search Service](https://docs.microsoft.com/rest/api/searchservice/). A definição de índice e o conteúdo pesquisável são fornecidos no corpo da solicitação como conteúdo JSON bem formado.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar e, em seguida, [inscreva-se no Azure Search](search-create-service-portal.md).

## <a name="prerequisites"></a>Pré-requisitos

Os serviços e as ferramentas a seguir são usados neste Início Rápido. 

+ [PowerShell 5.1 ou posterior](https://github.com/PowerShell/PowerShell), usando [Invoke-RestMethod](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Utility/Invoke-RestMethod) para etapas sequenciais e interativas.

+ [Crie um serviço Azure Search](search-create-service-portal.md) ou [localize um serviço existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) na assinatura atual. É possível usar um serviço gratuito para este início rápido. 

## <a name="get-a-key-and-url"></a>Obter uma chave e uma URL

As chamadas REST exigem a URL do serviço e uma chave de acesso em cada solicitação. Um serviço de pesquisa é criado com ambos, portanto, se você adicionou o Azure Search à sua assinatura, siga estas etapas para obter as informações necessárias:

1. [Entre no portal do Azure](https://portal.azure.com/) e, na página **Visão Geral** do serviço de pesquisa, obtenha a URL. Um ponto de extremidade de exemplo pode parecer com `https://mydemo.search.windows.net`.

2. Em **Configurações** > **Chaves**, obtenha uma chave de administração para adquirir todos os direitos sobre o serviço. Há duas chaves de administração intercambiáveis, fornecidas para a continuidade dos negócios, caso seja necessário sobrepor uma. É possível usar a chave primária ou secundária em solicitações para adicionar, modificar e excluir objetos.

![Obter um ponto de extremidade HTTP e uma chave de acesso](media/search-fiddler/get-url-key.png "Obter um ponto de extremidade HTTP e uma chave de acesso")

Todas as solicitações requerem uma chave de api em cada pedido enviado ao serviço. Ter uma chave válida estabelece a relação de confiança, para cada solicitação, entre o aplicativo que envia a solicitação e o serviço que lida com ela.

## <a name="connect-to-azure-search"></a>Conectar-se ao Azure Search

1. No PowerShell, crie uma **$headers** objeto para armazenar o tipo de conteúdo e a chave de API. Substitua a chave de API de administração (YOUR-ADMIN-API-KEY) com uma chave que é válida para o serviço de pesquisa. Você só precisa definir esse cabeçalho de uma vez para a duração da sessão, mas ela será adicionada a cada solicitação. 

    ```powershell
    $headers = @{
    'api-key' = '<YOUR-ADMIN-API-KEY>'
    'Content-Type' = 'application/json' 
    'Accept' = 'application/json' }
    ```

2. Criar uma **$url** coleção de índices do objeto que especifica o serviço. Substitua o nome do serviço (YOUR-SEARCH-SERVICE-NAME) com um serviço de pesquisa válida.

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/indexes?api-version=2019-05-06"
    ```

3. Execute **Invoke-RestMethod** para enviar uma solicitação GET para o serviço e verifique se a conexão. Adicione **ConvertTo-Json** para que você pode exibir as respostas enviadas do serviço.

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers | ConvertTo-Json
    ```

   Se o serviço está vazio e não tiver nenhum índice, os resultados são semelhantes ao exemplo a seguir. Caso contrário, você verá uma representação JSON de definições de índice.

    ```
    {
        "@odata.context":  "https://mydemo.search.windows.net/$metadata#indexes",
        "value":  [

                ]
    }
    ```

## <a name="1---create-an-index"></a>1 - Criar um índice

A menos que você estiver usando o portal, um índice deve existir no serviço antes de carregar dados. Esta etapa define o índice e envia por push para o serviço. O [criar API de REST do índice](https://docs.microsoft.com/rest/api/searchservice/create-index) é usado para esta etapa.

Os elementos necessários de um índice incluem um nome e uma coleção de campos. A coleção de campos define a estrutura de um *documento*. Cada campo tem um nome, tipo e atributos que determinam como ele é usado (por exemplo, se ele é texto completo pesquisável, filtrável ou recuperável nos resultados da pesquisa). Dentro de um índice, um dos campos do tipo `Edm.String` deve ser designado como o *chave* para a identidade do documento.

Esse índice chamado "hotéis-quickstart" e tem as definições de campo que você vê abaixo. Ele é um subconjunto de uma maior [índice de hotéis](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/Hotels_IndexDefinition.JSON) usados em outra instruções passo a passo. Ele é cortado neste início rápido para fins de brevidade.

1. Cole este exemplo no PowerShell para criar uma **$body** objeto que contém o esquema de índice.

    ```powershell
    $body = @"
    {
        "name": "hotels-quickstart",  
        "fields": [
            {"name": "HotelId", "type": "Edm.String", "key": true, "filterable": true},
            {"name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": true, "facetable": false},
            {"name": "Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.lucene"},
            {"name": "Category", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
            {"name": "Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "sortable": false, "facetable": true},
            {"name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "sortable": true, "facetable": true},
            {"name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": true, "sortable": true, "facetable": true},
            {"name": "Rating", "type": "Edm.Double", "filterable": true, "sortable": true, "facetable": true},
            {"name": "Address", "type": "Edm.ComplexType", 
            "fields": [
            {"name": "StreetAddress", "type": "Edm.String", "filterable": false, "sortable": false, "facetable": false, "searchable": true},
            {"name": "City", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
            {"name": "StateProvince", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
            {"name": "PostalCode", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
            {"name": "Country", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true}
            ]
         }
      ]
    }
    "@
    ```

2. Definir o URI para a coleção de índices em seu serviço e o *guia de início rápido hotéis* índice.

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart?api-version=2019-05-06"
    ```

3. Execute o comando com **$url**, **$headers**, e **$body** para criar o índice no serviço. 

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers -Method Put -Body $body | ConvertTo-Json
    ```

    Resultados devem ser semelhantes a este (truncado para os primeiros dois campos para fins de brevidade):

    ```
    {
        "@odata.context":  "https://mydemo.search.windows.net/$metadata#indexes/$entity",
        "@odata.etag":  "\"0x8D6EDE28CFEABDA\"",
        "name":  "hotels-quickstart",
        "defaultScoringProfile":  null,
        "fields":  [
                    {
                        "name":  "HotelId",
                        "type":  "Edm.String",
                        "searchable":  true,
                        "filterable":  true,
                        "retrievable":  true,
                        "sortable":  true,
                        "facetable":  true,
                        "key":  true,
                        "indexAnalyzer":  null,
                        "searchAnalyzer":  null,
                        "analyzer":  null,
                        "synonymMaps":  ""
                    },
                    {
                        "name":  "HotelName",
                        "type":  "Edm.String",
                        "searchable":  true,
                        "filterable":  false,
                        "retrievable":  true,
                        "sortable":  true,
                        "facetable":  false,
                        "key":  false,
                        "indexAnalyzer":  null,
                        "searchAnalyzer":  null,
                        "analyzer":  null,
                        "synonymMaps":  ""
                    },
    . . .
    ```

> [!Tip]
> Para verificação, você também pode verificar a lista de índices no portal do.

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2 - Carregar documentos

Para enviar por push documentos, use uma solicitação HTTP POST para o ponto de extremidade de URL do seu índice. A API REST para essa tarefa é [adicionar, atualizar ou excluir documentos](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents).

1. Cole este exemplo no PowerShell para criar uma **$body** objeto que contém os documentos que você deseja carregar. 

    Essa solicitação inclui dois completo e um registro parcial. O registro parcial demonstra que você pode carregar documentos incompletos. O `@search.action` parâmetro especifica como a indexação é feita. Os valores válidos incluem carregar, mesclar, mergeOrUpload e delete. O comportamento de mergeOrUpload ou cria um novo documento para hotelId = 3 ou atualiza o conteúdo se ele já existe.

    ```powershell
    $body = @"
    {
        "value": [
        {
        "@search.action": "upload",
        "HotelId": "1",
        "HotelName": "Secret Point Motel",
        "Description": "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
        "Category": "Boutique",
        "Tags": [ "pool", "air conditioning", "concierge" ],
        "ParkingIncluded": false,
        "LastRenovationDate": "1970-01-18T00:00:00Z",
        "Rating": 3.60,
        "Address": 
            {
            "StreetAddress": "677 5th Ave",
            "City": "New York",
            "StateProvince": "NY",
            "PostalCode": "10022",
            "Country": "USA"
            } 
        },
        {
        "@search.action": "upload",
        "HotelId": "2",
        "HotelName": "Twin Dome Motel",
        "Description": "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
        "Category": "Boutique",
        "Tags": [ "pool", "free wifi", "concierge" ],
        "ParkingIncluded": false,
        "LastRenovationDate": "1979-02-18T00:00:00Z",
        "Rating": 3.60,
        "Address": 
            {
            "StreetAddress": "140 University Town Center Dr",
            "City": "Sarasota",
            "StateProvince": "FL",
            "PostalCode": "34243",
            "Country": "USA"
            } 
        },
        {
        "@search.action": "upload",
        "HotelId": "3",
        "HotelName": "Triple Landscape Hotel",
        "Description": "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
        "Category": "Resort and Spa",
        "Tags": [ "air conditioning", "bar", "continental breakfast" ],
        "ParkingIncluded": true,
        "LastRenovationDate": "2015-09-20T00:00:00Z",
        "Rating": 4.80,
        "Address": 
            {
            "StreetAddress": "3393 Peachtree Rd",
            "City": "Atlanta",
            "StateProvince": "GA",
            "PostalCode": "30326",
            "Country": "USA"
            } 
        },
        {
        "@search.action": "upload",
        "HotelId": "4",
        "HotelName": "Sublime Cliff Hotel",
        "Description": "Sublime Cliff Hotel is located in the heart of the historic center of Sublime in an extremely vibrant and lively area within short walking distance to the sites and landmarks of the city and is surrounded by the extraordinary beauty of churches, buildings, shops and monuments. Sublime Cliff is part of a lovingly restored 1800 palace.",
        "Category": "Boutique",
        "Tags": [ "concierge", "view", "24-hour front desk service" ],
        "ParkingIncluded": true,
        "LastRenovationDate": "1960-02-06T00:00:00Z",
        "Rating": 4.60,
        "Address": 
            {
            "StreetAddress": "7400 San Pedro Ave",
            "City": "San Antonio",
            "StateProvince": "TX",
            "PostalCode": "78216",
            "Country": "USA"
            }
        }
    ]
    }
    "@
    ```

1. Definir o ponto de extremidade o *guia de início rápido hotéis* coleção docs e incluir a operação de índice (índices/hotéis-quickstart/docs/index).

    ```powershell
    $url = "https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs/index?api-version=2019-05-06"
    ```

1. Execute o comando com **$url**, **$headers**, e **$body** para carregar documentos no índice de hotéis-quickstart.

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers -Method Post -Body $body | ConvertTo-Json
    ```
    Resultados devem ser semelhantes ao exemplo a seguir. Você deve ver uma [código de status de 201](https://docs.microsoft.com/rest/api/searchservice/HTTP-status-codes).

    ```
    {
        "@odata.context":  "https://mydemo.search.windows.net/indexes(\u0027hotels-quickstart\u0027)/$metadata#Collection(Microsoft.Azure.Search.V2019_05_06.IndexResult)",
        "value":  [
                    {
                        "key":  "1",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    },
                    {
                        "key":  "2",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    },
                    {
                        "key":  "3",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    },
                    {
                        "key":  "4",
                        "status":  true,
                        "errorMessage":  null,
                        "statusCode":  201
                    }
                ]
    }
    ```

## <a name="3---search-an-index"></a>3 - Pesquisar um índice

Esta etapa mostra como consultar um índice usando o [API de pesquisa de documentos](https://docs.microsoft.com/rest/api/searchservice/search-documents).

Certifique-se de usar aspas simples na pesquisa $urls. Cadeias de caracteres de consulta incluem **$** caracteres e você pode omitir a necessidade de escape-los se a cadeia de caracteres inteira é colocada entre aspas simples...

1. Definir o ponto de extremidade o *hotéis-quickstart* coleção docs e adicione um **pesquisa** parâmetro para passar uma cadeia de caracteres de consulta. 
  
   Essa cadeia de caracteres executa uma pesquisa vazia (pesquisa = *), retornando uma lista unranked (pontuação de pesquisa = 1,0) dos documentos arbitrários. Por padrão, o Azure Search retorna 50 correspondências por vez. Como a estruturadas, esta consulta retorna uma estrutura de todo o documento e os valores. Adicione **$count = true** para obter uma contagem de todos os documentos nos resultados.

    ```powershell
    $url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=*&$count=true'
    ```

1. Execute o comando para enviar o **$url** para o serviço.

    ```powershell
    Invoke-RestMethod -Uri $url -Headers $headers | ConvertTo-Json
    ```

    Resultados devem ser semelhantes à seguinte saída.

    ```
    {
    "@odata.context":  "https://mydemo.search.windows.net/indexes(\u0027hotels-quickstart\u0027)/$metadata#docs(*)",
    "@odata.count":  4,
    "value":  [
                  {
                      "@search.score":  0.1547872,
                      "HotelId":  "2",
                      "HotelName":  "Twin Dome Motel",
                      "Description":  "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
                      "Category":  "Boutique",
                      "Tags":  "pool free wifi concierge",
                      "ParkingIncluded":  false,
                      "LastRenovationDate":  "1979-02-18T00:00:00Z",
                      "Rating":  3.6,
                      "Address":  "@{StreetAddress=140 University Town Center Dr; City=Sarasota; StateProvince=FL; PostalCode=34243; Country=USA}"
                  },
                  {
                      "@search.score":  0.009068266,
                      "HotelId":  "3",
                      "HotelName":  "Triple Landscape Hotel",
                      "Description":  "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel\u0027s restaurant services.",
                      "Category":  "Resort and Spa",
                      "Tags":  "air conditioning bar continental breakfast",
                      "ParkingIncluded":  true,
                      "LastRenovationDate":  "2015-09-20T00:00:00Z",
                      "Rating":  4.8,
                      "Address":  "@{StreetAddress=3393 Peachtree Rd; City=Atlanta; StateProvince=GA; PostalCode=30326; Country=USA}"
                  },
                . . . 
    ```

Experimente alguns outros exemplos de consultas para ter uma noção do que a sintaxe. Você pode fazer uma pesquisa de cadeia de caracteres, consultas $filter textuais, limitar o conjunto de resultados, o escopo de pesquisa para campos específicos e muito mais.

```powershell
# Query example 1
# Search the entire index for the terms 'hotels' and 'wifi'
# Return only the HotelName and HotelId fields
$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=hotels wifi&$count=true&$select=HotelName,HotelId'

# Query example 2 
# Apply a filter to the index to find hotels rated 4 or highter
# Returns the HotelId and Description. Two documents match.
$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=*&$filter=Rating gt 4&$select=HotelId,HotelName,Description,Rating'

# Query example 3
# Take the top two results, and show only HotelId,HotelName,Description in the results
$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=boutique&$top=2&$select=HotelId,HotelName,Description,Category'

# Query example 4
# Sort by a specific field (`lastRenovationDate`) in descending order

$url = 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/hotels-quickstart/docs?api-version=2019-05-06&search=pool&$orderby=Address/City&$select=HotelId, HotelName, Address/City, Address/StateProvince, Tags'
```
## <a name="clean-up"></a>Limpar 

Você deve excluir o índice se você não precisar mais dela. Um serviço gratuito é limitado a três índices. Você talvez queira excluir quaisquer índices que não estiver usando ativamente para que você possa percorrer os outros tutoriais.

```powershell
# Set the URI to the hotel index
$url = 'https://mydemo.search.windows.net/indexes/hotels-quickstart?api-version=2019-05-06'

# Delete the index
Invoke-RestMethod -Uri $url -Headers $headers -Method Delete
```

## <a name="next-steps"></a>Próximas etapas

Tente adicionar descrições de francês para o índice. O exemplo a seguir inclui sequências em francês e demonstra as ações de pesquisa adicionais. Use mergeOrUpload para criar ou adicionar a campos existentes. As cadeias de caracteres a seguir precisam ser codificados em UTF-8.

```json
{
    "value": [
        {
            "@search.action": "mergeOrUpload",
            "hotelId": "1",
            "description_fr": "Meilleur hôtel en ville"
        },
        {
            "@search.action": "merge",
            "hotelId": "2",
            "description_fr": "Hôtel le moins cher en ville"
        }
    ]
}
```
