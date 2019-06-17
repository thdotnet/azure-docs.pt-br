---
title: Referência de filtro OData - Azure Search
description: Referência de linguagem do OData para a sintaxe de filtro em consultas de pesquisa do Azure.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: Brjohnstmsft
ms.author: brjohnst
ms.manager: cgronlun
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: c44645ebcbf8808cc929bfaa0c3cb0d3ee9c90cd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079906"
---
# <a name="odata-filter-syntax-in-azure-search"></a>Sintaxe de OData $filter no Azure Search

O Azure Search usa [expressões de filtro OData](query-odata-filter-orderby-syntax.md) aplicar critérios adicionais para uma consulta de pesquisa além dos termos de pesquisa de texto completo. Este artigo descreve a sintaxe de filtros em detalhes. Para obter mais informações gerais sobre o que são filtros e como usá-los para realizar cenários de consulta específica, consulte [filtros no Azure Search](search-filters.md).

## <a name="syntax"></a>Sintaxe

Um filtro na linguagem de OData é uma expressão booliana, que por sua vez pode ser um dos vários tipos de expressão, conforme mostrado a seguir EBNF ([Extended Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)):

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
boolean_expression ::=
    collection_filter_expression
    | logical_expression
    | comparison_expression
    | boolean_literal
    | boolean_function_call
    | '(' boolean_expression ')'
    | variable

/* This can be a range variable in the case of a lambda, or a field path. */
variable ::= identifier | field_path
```

Um diagrama de sintaxe interativa também está disponível:

> [!div class="nextstepaction"]
> [Diagrama de sintaxe do OData para o Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#boolean_expression)

> [!NOTE]
> Ver [referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md) para o EBNF completa.

Os tipos de expressões Boolianas incluem:

- Expressões de filtro de coleção usando `any` ou `all`. Isso se aplica critérios de filtro para os campos da coleção. Para obter mais informações, consulte [operadores de coleção OData no Azure Search](search-query-odata-collection-operators.md).
- Expressões lógicas que combinam outras expressões Boolianas usando os operadores `and`, `or`, e `not`. Para obter mais informações, consulte [operadores lógicos de OData no Azure Search](search-query-odata-logical-operators.md).
- Expressões de comparação, o qual comparam os campos ou variáveis para valores constantes usando os operadores de intervalo `eq`, `ne`, `gt`, `lt`, `ge`, e `le`. Para obter mais informações, consulte [operadores de comparação de OData no Azure Search](search-query-odata-comparison-operators.md). Expressões de comparação também são usadas para comparar as distâncias entre coordenadas geoespaciais usando o `geo.distance` função. Para obter mais informações, consulte [funções geoespaciais do OData no Azure Search](search-query-odata-geo-spatial-functions.md).
- Os literais boolianos `true` e `false`. Essas constantes podem ser úteis, às vezes, quando a geração programática de filtros, mas caso contrário, não tendem a ser usada na prática.
- Chamadas para funções booleanas, incluindo:
  - `geo.intersects`, que testa se um determinado ponto está dentro de um polígono determinado. Para obter mais informações, consulte [funções geoespaciais do OData no Azure Search](search-query-odata-geo-spatial-functions.md).
  - `search.in`, que compara uma campo ou variável de intervalo com cada valor em uma lista de valores. Para obter mais informações, consulte [OData `search.in` função no Azure Search](search-query-odata-search-in-function.md).
  - `search.ismatch` e `search.ismatchscoring`, quais executar operações de pesquisa de texto completo em um contexto de filtro. Para obter mais informações, consulte [funções de pesquisa de texto completo do OData no Azure Search](search-query-odata-full-text-search-functions.md).
- Caminhos de campo ou intervalo de variáveis do tipo `Edm.Boolean`. Por exemplo, se o índice tem um campo booleano chamado `IsEnabled` e você quiser retornar todos os documentos em que esse campo é `true`, sua expressão de filtro pode ser apenas o nome `IsEnabled`.
- Expressões Boolianas entre parênteses. O uso de parênteses pode ajudar a determinar explicitamente a ordem das operações em um filtro. Para obter mais informações sobre a precedência padrão dos operadores de OData, consulte a próxima seção.

### <a name="operator-precedence-in-filters"></a>Precedência do operador em filtros

Se você escrever uma expressão de filtro com nenhum seus subexpressões entre parênteses, Azure Search avaliará acordo com um conjunto de regras de precedência de operador. Essas regras são baseadas na qual os operadores são usados para combinar subexpressões. A tabela a seguir lista os grupos de operadores na ordem do mais alto para a menor precedência:

| Agrupar | Es |
| --- | --- |
| Operadores lógicos | `not` |
| Operadores de comparação | `eq`, `ne`, `gt`, `lt`, `ge`, `le` |
| Operadores lógicos | `and` |
| Operadores lógicos | `or` |

Um operador que é superior na tabela acima será "associar o mais próximo" para seus operandos que outros operadores. Por exemplo, `and` tem precedência maior do que `or`, e operadores de comparação são de precedência maior do que qualquer um deles, portanto, as duas expressões a seguir são equivalentes:

    Rating gt 0 and Rating lt 3 or Rating gt 7 and Rating lt 10
    ((Rating gt 0) and (Rating lt 3)) or ((Rating gt 7) and (Rating lt 10))

O `not` operador tem a precedência mais alta de todos os – ainda maior do que os operadores de comparação. É por isso que se você tentar gravar um filtro como esta:

    not Rating gt 5

Você receberá essa mensagem de erro:

    Invalid expression: A unary operator with an incompatible type was detected. Found operand type 'Edm.Int32' for operator kind 'Not'.

Esse erro ocorre porque o operador está associado apenas a `Rating` campo, que é do tipo `Edm.Int32`e não com a expressão de comparação de inteiro. A correção é colocar o operando da `not` entre parênteses:

    not (Rating gt 5)

<a name="bkmk_limits"></a>

### <a name="filter-size-limitations"></a>Limitações de tamanho de filtro

Há limites para o tamanho e a complexidade de expressões de filtro que você pode enviar para o Azure Search. Os limites se baseiam aproximadamente no número de cláusulas na sua expressão de filtro. Uma boa diretriz é que, se você tiver centenas de cláusulas, você está em risco de exceder o limite. É recomendável criar seu aplicativo de forma que ele não gere filtros de tamanho ilimitado.

> [!TIP]
> Usando o [as `search.in` função](search-query-odata-search-in-function.md) desassociações longo de igualdade em vez de comparações podem ajudar a evitar o limite de cláusula de filtro, uma vez que uma chamada de função contará como uma única cláusula.

## <a name="examples"></a>Exemplos

Localize todos os hotéis pelo menos uma sala com uma taxa base menor que US $200 que são classificados ou superior a 4:

    $filter=Rooms/any(room: room/BaseRate lt 200.0) and Rating ge 4

Localize todos os hotéis foram renovados desde 2010 diferente de "Sea exibição Motel":

    $filter=HotelName ne 'Sea View Motel' and LastRenovationDate ge 2010-01-01T00:00:00Z

Localize todos os hotéis foram reformados em 2010 ou posterior. A data e hora literal inclui informações de fuso horário para hora oficial do Pacífico:  

    $filter=LastRenovationDate ge 2010-01-01T00:00:00-08:00

Localize todos os hotéis que têm estacionamento incluído e onde todas as salas são não fumante:

    $filter=ParkingIncluded and Rooms/all(room: not room/SmokingAllowed)

 \- OU -  

    $filter=ParkingIncluded eq true and Rooms/all(room: room/SmokingAllowed eq false)

Localizar todos os hotéis marcados como Luxo ou que incluem estacionamento e têm uma classificação de 5:  

    $filter=(Category eq 'Luxury' or ParkingIncluded eq true) and Rating eq 5

Localize todos os hotéis com a marca "wifi" pelo menos um espaço (em que cada sala tem marcas armazenadas em um `Collection(Edm.String)` campo):  

    $filter=Rooms/any(room: room/Tags/any(tag: tag eq 'wifi'))

Localize todos os hotéis com qualquer salas:  

    $filter=Rooms/any()

Localize todos os hotéis que não têm salas:

    $filter=not Rooms/any()

Localize todos os hotéis dentro de 10 km de um ponto de referência fornecido (onde `Location` é um campo do tipo `Edm.GeographyPoint`):

    $filter=geo.distance(Location, geography'POINT(-122.131577 47.678581)') le 10

Localize todos os hotéis dentro de um viewport determinado descrito como um polígono (onde `Location` é um campo do tipo EDM. geographypoint). O polígono deve ser fechado, o que significa que o primeiro e último conjuntos de pontos devem ser o mesmo. Além disso, [os pontos devem ser listados no sentido anti-horário](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

    $filter=geo.intersects(Location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))')

Localize todos os hotéis em que o campo "Descrição" é nulo. O campo será nulo se ele nunca foi definido, ou se foi definida explicitamente como null:  

    $filter=Description eq null

Localize todos os hotéis com o nome igual a 'Motel exibição Sea' ou 'Hotel orçamento'). Essas frases contenham espaços, e o espaço é um delimitador de padrão. Você pode especificar um delimitador alternativo entre aspas simples como o terceiro parâmetro de cadeia de caracteres:  

    $filter=search.in(HotelName, 'Sea View motel,Budget hotel', ',')

Localize todos os hotéis com o nome igual a 'Motel exibição Sea' ou 'Hotel do orçamento' separados por ' |'):  

    $filter=search.in(HotelName, 'Sea View motel|Budget hotel', '|')

Localize todos os hotéis onde todas as salas tem a marca 'Wi-Fi' ou 'tub':

    $filter=Rooms/any(room: room/Tags/any(tag: search.in(tag, 'wifi, tub'))

Localize uma correspondência em frases em uma coleção, como 'toalha aquecido racks' ou 'hairdryer incluído' de marcas.

    $filter=Rooms/any(room: room/Tags/any(tag: search.in(tag, 'heated towel racks,hairdryer included', ','))

Localizar documentos com as palavras "orla marítima". Essa consulta de filtro é idêntica a uma [solicitação de pesquisa](https://docs.microsoft.com/rest/api/searchservice/search-documents) com `search=waterfront`.

    $filter=search.ismatchscoring('waterfront')

Localizar documentos com a palavra "hostel" e classificação maior ou igual a 4, ou documentos com a palavra "motel" e classificação igual a 5. Esta solicitação não pôde ser expressada sem o `search.ismatchscoring` funcionar, pois ele combina pesquisa de texto completo com operações de filtro usando `or`.

    $filter=search.ismatchscoring('hostel') and rating ge 4 or search.ismatchscoring('motel') and rating eq 5

Localizar documentos sem a palavra "luxo".

    $filter=not search.ismatch('luxury')

Localizar documentos com a frase "vista para o mar" ou classificação igual a 5. A consulta `search.ismatchscoring` será executada apenas em relação aos campos `HotelName` e `Description`. Documentos que correspondem à apenas a segunda cláusula a disjunção serão retornados--hotéis com `Rating` igual a 5. Esses documentos serão retornados com pontuação igual a zero para torná-lo limpar que eles não correspondiam qualquer uma das partes da expressão classificadas.

    $filter=search.ismatchscoring('"ocean view"', 'Description,HotelName') or Rating eq 5

Encontre hotéis onde os termos "hotel" e "aeroporto" são não mais do que cinco palavras separadas na descrição e, em seguida, em que todas as salas são não fumante. Essa consulta usa a [linguagem de consulta Lucene completa](query-lucene-syntax.md).

    $filter=search.ismatch('"hotel airport"~5', 'Description', 'full', 'any') and not Rooms/any(room: room/SmokingAllowed)

## <a name="next-steps"></a>Próximas etapas  

- [Filtros no Azure Search](search-filters.md)
- [Visão geral de linguagem de expressão do OData para o Azure Search](query-odata-filter-orderby-syntax.md)
- [Referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md)
- [Pesquisar documentos &#40;API REST do Serviço do Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
