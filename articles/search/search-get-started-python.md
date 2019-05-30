---
title: 'Início Rápido: Python e APIs REST - Azure Search'
description: Criar, carregar e consultar um índice usando o Python, Jupyter Notebooks e a API REST do Azure Search.
ms.date: 05/23/2019
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: rest-api
ms.topic: conceptual
ms.custom: seodec2018
ms.openlocfilehash: 99b4ec0be8e9fa631c5081edd42474ea89dc5dc3
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244787"
---
# <a name="quickstart-create-an-azure-search-index-using-jupyter-python-notebooks"></a>Início Rápido: Criar um índice de Azure Search usando blocos de anotações Jupyter Python
> [!div class="op_single_selector"]
> * [Python (REST)](search-get-started-python.md)
> * [PowerShell (REST)](search-create-index-rest-api.md)
> * [C#](search-create-index-dotnet.md)
> * [Postman (REST)](search-fiddler.md)
> * [Portal](search-create-index-portal.md)
> 

Criar um bloco de anotações do Jupyter que cria, carrega e consulta o índice de Azure Search usando o Python e o [as APIs REST do Azure Search](https://docs.microsoft.com/rest/api/searchservice/). Este artigo explica como criar um bloco de anotações passo a passo, começar do zero. Como alternativa, você pode executar um bloco de anotações concluído. Para baixar uma cópia, acesse [repositório Azure-Search-python-samples](https://github.com/Azure-Samples/azure-search-python-samples).

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Os serviços e as ferramentas a seguir são usados neste Início Rápido. 

+ [Anaconda 3. x](https://www.anaconda.com/distribution/#download-section), fornecendo o Python 3.x e Notebooks Jupyter.

+ [Crie um serviço Azure Search](search-create-service-portal.md) ou [localize um serviço existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) na assinatura atual. Você pode usar a camada gratuita para este início rápido. 

## <a name="get-a-key-and-url"></a>Obter uma chave e uma URL

As chamadas REST exigem a URL do serviço e uma chave de acesso em cada solicitação. Um serviço de pesquisa é criado com ambos, portanto, se você adicionou o Azure Search à sua assinatura, siga estas etapas para obter as informações necessárias:

1. [Entre no portal do Azure](https://portal.azure.com/) e, na página **Visão Geral** do serviço de pesquisa, obtenha a URL. Um ponto de extremidade de exemplo pode parecer com `https://mydemo.search.windows.net`.

1. Em **Configurações** > **Chaves**, obtenha uma chave de administração para adquirir todos os direitos sobre o serviço. Há duas chaves de administração intercambiáveis, fornecidas para a continuidade dos negócios, caso seja necessário sobrepor uma. É possível usar a chave primária ou secundária em solicitações para adicionar, modificar e excluir objetos.

![Obter um ponto de extremidade HTTP e uma chave de acesso](media/search-fiddler/get-url-key.png "Obter um ponto de extremidade HTTP e uma chave de acesso")

Todas as solicitações requerem uma chave de api em cada pedido enviado ao serviço. Ter uma chave válida estabelece a relação de confiança, para cada solicitação, entre o aplicativo que envia a solicitação e o serviço que lida com ela.

## <a name="connect-to-azure-search"></a>Conectar-se ao Azure Search

Nesta tarefa, inicie um notebook Jupyter e verifique se que você pode se conectar ao Azure Search. Você fará isso ao solicitar uma lista de índices do seu serviço. No Windows com o Anaconda3, você pode usar o navegador de Anaconda para iniciar um bloco de anotações.

1. Crie um novo notebook Python3.

1. Na primeira célula, carrega as bibliotecas usadas para trabalhar com JSON e formular solicitações HTTP.

   ```python
   import json
   import requests
   from pprint import pprint
   ```

1. Na segunda célula, os elementos de solicitação que serão constantes em todas as solicitações de entrada. Substitua o nome do serviço de pesquisa (YOUR-SEARCH-SERVICE-NAME) e a chave de API de administração (YOUR-ADMIN-API-KEY) com valores válidos. 

   ```python
   endpoint = 'https://<YOUR-SEARCH-SERVICE-NAME>.search.windows.net/'
   api_version = '?api-version=2019-05-06'
   headers = {'Content-Type': 'application/json',
           'api-key': '<YOUR-ADMIN-API-KEY>' }
   ```

1. Na terceira célula, formule a solicitação. Essa solicitação GET tem como alvo a coleção de índices de seu serviço de pesquisa e seleciona a propriedade de nome dos índices existentes.

   ```python
   url = endpoint + "indexes" + api_version + "&$select=name"
   response  = requests.get(url, headers=headers)
   index_list = response.json()
   pprint(index_list)
   ```

1. Execute cada etapa. Se existirem índices, a resposta contém uma lista de nomes de índice. Na captura de tela abaixo, o serviço já tem um índice de azureblob e um índice realestate-us-sample.

   ![Solicitações de script Python no bloco de anotações do Jupyter com HTTP para o Azure Search](media/search-get-started-python/connect-azure-search.png "solicitações de script Python no bloco de anotações do Jupyter com HTTP para o Azure Search")

   Em contraste, uma coleção vazia de índice retorna a seguinte resposta: `{'@odata.context': 'https://mydemo.search.windows.net/$metadata#indexes(name)', 'value': []}`

> [!Tip]
> Em um serviço gratuito, você está limitado a três índices, indexadores e fontes de dados. Neste início rápido cria um de cada um. Certifique-se de que ter espaço para criar novos objetos antes de prosseguir.

## <a name="1---create-an-index"></a>1 - Criar um índice

A menos que você estiver usando o portal, um índice deve existir no serviço antes de carregar dados. Esta etapa usa a [criar API de REST do índice](https://docs.microsoft.com/rest/api/searchservice/create-index) para enviar por push a um esquema de índice para o serviço.

Os elementos necessários de um índice incluem um nome, uma coleção de campos e uma chave. A coleção de campos define a estrutura de um *documento*. Cada campo tem um nome, tipo e atributos que determinam como o campo é usado (por exemplo, se ele é texto completo pesquisável, filtrável ou recuperável nos resultados da pesquisa). Dentro de um índice, um dos campos do tipo `Edm.String` deve ser designado como o *chave* para a identidade do documento.

Esse índice chamado "hotéis py" e tem as definições de campo que você vê abaixo. Ele é um subconjunto de uma maior [índice de hotéis](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/Hotels_IndexDefinition.JSON) usados em outra instruções passo a passo. Ele é cortado neste início rápido para fins de brevidade.

1. Na próxima célula, cole o exemplo a seguir em uma célula para fornecer o esquema. 

    ```python
    index_schema = {
       "name": "hotels-py",  
       "fields": [
         {"name": "HotelId", "type": "Edm.String", "key": "true", "filterable": "true"},
         {"name": "HotelName", "type": "Edm.String", "searchable": "true", "filterable": "false", "sortable": "true", "facetable": "false"},
         {"name": "Description", "type": "Edm.String", "searchable": "true", "filterable": "false", "sortable": "false", "facetable": "false", "analyzer": "en.lucene"},
         {"name": "Description_fr", "type": "Edm.String", "searchable": "true", "filterable": "false", "sortable": "false", "facetable": "false", "analyzer": "fr.lucene"},
         {"name": "Category", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Tags", "type": "Collection(Edm.String)", "searchable": "true", "filterable": "true", "sortable": "false", "facetable": "true"},
         {"name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Rating", "type": "Edm.Double", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Address", "type": "Edm.ComplexType", 
         "fields": [
         {"name": "StreetAddress", "type": "Edm.String", "filterable": "false", "sortable": "false", "facetable": "false", "searchable": "true"},
         {"name": "City", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "StateProvince", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "PostalCode", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"},
         {"name": "Country", "type": "Edm.String", "searchable": "true", "filterable": "true", "sortable": "true", "facetable": "true"}
        ]
       }
      ]
    }
    ```

2. Em outra célula, formule a solicitação. Isso, coloque solicitação tem como alvo a coleção de índices de seu serviço de pesquisa e cria um índice com base no esquema de índice que você forneceu na célula anterior.

   ```python
   url = endpoint + "indexes" + api_version
   response  = requests.post(url, headers=headers, json=index_schema)
   index = response.json()
   pprint(index)
   ```

3. Execute cada etapa.

   A resposta inclui a representação JSON do esquema. Captura de tela a seguir mostra apenas uma parte da resposta.

    ![Solicitação para criar um índice](media/search-get-started-python/create-index.png "solicitação para criar um índice")

> [!Tip]
> Outra maneira de verificar a criação de índice é verificar a lista de índices no portal.

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2 - Carregar documentos

Para enviar por push documentos, use uma solicitação HTTP POST para o ponto de extremidade de URL do seu índice. A API REST está [adicionar, atualizar ou excluir documentos](https://docs.microsoft.com/rest/api/searchservice/addupdate-or-delete-documents). Documentos originam [HotelsData](https://github.com/Azure-Samples/azure-search-sample-data/blob/master/hotels/HotelsData_toAzureSearch.JSON) no GitHub.

1. Em uma nova célula, fornece quatro documentos que estão em conformidade com o esquema de índice. Especifique uma ação de carregamento para cada documento.

    ```python
    documents = {
        "value": [
        {
        "@search.action": "upload",
        "HotelId": "1",
        "HotelName": "Secret Point Motel",
        "Description": "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
        "Description_fr": "L'hôtel est idéalement situé sur la principale artère commerciale de la ville en plein cœur de New York. A quelques minutes se trouve la place du temps et le centre historique de la ville, ainsi que d'autres lieux d'intérêt qui font de New York l'une des villes les plus attractives et cosmopolites de l'Amérique.",
        "Category": "Boutique",
        "Tags": [ "pool", "air conditioning", "concierge" ],
        "ParkingIncluded": "false",
        "LastRenovationDate": "1970-01-18T00:00:00Z",
        "Rating": 3.60,
        "Address": {
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
        "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
        "Category": "Boutique",
        "Tags": [ "pool", "free wifi", "concierge" ],
        "ParkingIncluded": "false",
        "LastRenovationDate": "1979-02-18T00:00:00Z",
        "Rating": 3.60,
        "Address": {
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
        "Description_fr": "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
        "Category": "Resort and Spa",
        "Tags": [ "air conditioning", "bar", "continental breakfast" ],
        "ParkingIncluded": "true",
        "LastRenovationDate": "2015-09-20T00:00:00Z",
        "Rating": 4.80,
        "Address": {
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
        "Description_fr": "Le sublime Cliff Hotel est situé au coeur du centre historique de sublime dans un quartier extrêmement animé et vivant, à courte distance de marche des sites et monuments de la ville et est entouré par l'extraordinaire beauté des églises, des bâtiments, des commerces et Monuments. Sublime Cliff fait partie d'un Palace 1800 restauré avec amour.",
        "Category": "Boutique",
        "Tags": [ "concierge", "view", "24-hour front desk service" ],
        "ParkingIncluded": "true",
        "LastRenovationDate": "1960-02-06T00:00:00Z",
        "Rating": 4.60,
        "Address": {
            "StreetAddress": "7400 San Pedro Ave",
            "City": "San Antonio",
            "StateProvince": "TX",
            "PostalCode": "78216",
            "Country": "USA"
            }
        }
    ]
    }
    ```   

2. Em outra célula, formule a solicitação. Esta solicitação POST tem como alvo a coleção de documentos do índice de hotéis py e envia os documentos fornecidos na etapa anterior.

   ```python
   url = endpoint + "indexes/hotels-py/docs/index" + api_version
   response  = requests.post(url, headers=headers, json=documents)
   index_content = response.json()
   pprint(index_content)
   ```

3. Execute cada etapa para enviar por push os documentos em um índice no serviço de pesquisa. Resultados devem ser semelhantes ao exemplo a seguir. 

    ![Enviar documentos para um índice](media/search-get-started-python/load-index.png "enviar documentos para um índice")

## <a name="3---search-an-index"></a>3 - Pesquisar um índice

Esta etapa mostra como consultar um índice usando o [API REST de pesquisa de documentos](https://docs.microsoft.com/rest/api/searchservice/search-documents).


1. Em uma nova célula, fornece uma expressão de consulta. O exemplo a seguir pesquisa sobre os termos "hotéis" e "wifi". Ele também retorna um *contagem* de documentos que correspondam, e *seleciona* quais campos serão incluídos nos resultados da pesquisa.

   ```python
   searchstring = '&search=hotels wifi&$count=true&$select=HotelId,HotelName'
   ```

2. Em outra célula, formule uma solicitação. Essa solicitação GET visa a coleção de documentos do índice de hotéis py e anexa a consulta que você especificou na etapa anterior.

   ```python
   url = endpoint + "indexes/hotels-py/docs" + api_version + searchstring
   response  = requests.get(url, headers=headers, json=searchstring)
   query = response.json()
   pprint(query)
   ```

3. Execute cada etapa. Resultados devem ser semelhantes à seguinte saída. 

    ![Um índice de pesquisa](media/search-get-started-python/search-index.png "um índice de pesquisa")

4. Experimente alguns outros exemplos de consultas para ter uma noção do que a sintaxe. Você pode substituir a searchstring com os exemplos a seguir e, em seguida, execute novamente a solicitação de pesquisa. 

   Aplica um filtro: 

   ```python
   searchstring = '&search=*&$filter=Rating gt 4&$select=HotelId,HotelName,Description'
   ```

   Execute os dois principais resultados:

   ```python
   searchstring = '&search=boutique&$top=2&$select=HotelId,HotelName,Description'
   ```

    Ordenar por um campo específico:

   ```python
   searchstring = '&search=pool&$orderby=Address/City&$select=HotelId, HotelName, Address/City, Address/StateProvince'
   ```

## <a name="clean-up"></a>Limpar 

Você deve excluir o índice se você não precisar mais dela. Um serviço gratuito é limitado a três índices. Você talvez queira excluir qualquer índice que você não estiver usando ativamente para liberar espaço para outros tutoriais.

   ```python
  url = endpoint + "indexes/hotels-py" + api_version
  response  = requests.delete(url, headers=headers)
   ```

Você pode verificar a exclusão de índice, retornando uma lista dos índices existentes. Se hotéis py for perdido, você sabe sua solicitação foi bem-sucedida.

```python
url = endpoint + "indexes" + api_version + "&$select=name"

response  = requests.get(url, headers=headers)
index_list = response.json()
pprint(index_list)
```

## <a name="next-steps"></a>Próximas etapas

Como simplificação, este início rápido usa uma versão abreviada do índice de hotéis. Você pode criar a versão completa para experimentar consultas mais interessantes. Para obter a versão completa e todos os documentos de 50, execute as **importar dados** assistente, selecionando *exemplo hotéis* das fontes de dados de exemplo interno.

> [!div class="nextstepaction"]
> [Início Rápido: Criar um índice no portal do Azure](search-get-started-portal.md)
