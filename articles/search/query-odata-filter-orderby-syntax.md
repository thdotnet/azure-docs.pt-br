---
title: Visão geral da linguagem de OData - Azure Search
description: Visão geral da linguagem de OData para filtros, selecione e classificar por para consultas de pesquisa do Azure.
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
ms.openlocfilehash: 166c23088fe0388199ca51efde05153bb5697e38
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063697"
---
# <a name="odata-language-overview-for-filter-orderby-and-select-in-azure-search"></a>Visão geral da linguagem para OData `$filter`, `$orderby`, e `$select` no Azure Search

O Azure Search dá suporte a um subconjunto da sintaxe de expressão OData **$filter**, **$orderby**, e **$select** expressões. As expressões Filter são avaliadas durante a análise da consulta, restringindo a pesquisa a campos específicos ou adicionando critérios de correspondência usados durante as verificações de índice. Expressões Order by são aplicadas como uma etapa de pós-processamento ao longo de um conjunto de resultados para classificar os documentos que são retornados. Expressões de determinam quais campos de documento serão incluídos no conjunto de resultados. A sintaxe dessas expressões é diferente dos [simples](query-simple-syntax.md) ou [completo](query-lucene-syntax.md) que é usada na sintaxe de consulta o **pesquisa** parâmetro, embora haja alguma sobreposição na sintaxe campos de referência.

Este artigo fornece uma visão geral da linguagem de expressão OData usada em filtros, order by e expressões. O idioma é apresentado "baixo para cima", começando com os elementos mais básicos e criação neles. A sintaxe de nível superior para cada parâmetro é descrita em outro artigo:

- [sintaxe de $filter](search-query-odata-filter.md)
- [sintaxe de $orderby](search-query-odata-orderby.md)
- [sintaxe de $select](search-query-odata-select.md)

Intervalo de expressões de OData de simples a altamente complexas, mas todos compartilham elementos comuns. As partes mais básicas de uma expressão de OData no Azure Search são:

- **Campo caminhos**, que se refere a campos específicos do índice.
- **Constantes**, que são valores literais de um determinado tipo de dados.

> [!NOTE]
> Terminologia no Azure Search difere de [padrão OData](https://www.odata.org/documentation/) de algumas maneiras. O que chamamos de um **campo** no Azure Search é chamado um **propriedade** no OData e o mesmo **caminho de campo** versus **caminho da propriedade**. Uma **índice** contendo **documentos** no Azure Search é chamado de modo geral no OData como um **conjunto de entidades** contendo **entidades**. A terminologia do Azure Search é usada ao longo dessa referência.

## <a name="field-paths"></a>Caminhos de campo

A seguir EBNF ([Extended Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) define a gramática de caminhos de campo.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
field_path ::= identifier('/'identifier)*

identifier ::= [a-zA-Z_][a-zA-Z_0-9]*
```

Um diagrama de sintaxe interativa também está disponível:

> [!div class="nextstepaction"]
> [Diagrama de sintaxe do OData para o Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#field_path)

> [!NOTE]
> Ver [referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md) para o EBNF completa.

Um caminho de campo é composto de um ou mais **identificadores** separados por barras "/". Cada identificador é uma sequência de caracteres que deve começar com uma letra ASCII ou sublinhado e conter somente ASCII letras, dígitos ou sublinhados. As letras podem ser superior ou em letras minúsculas.

Um identificador pode referir-se para o nome de um campo ou para um **variável de intervalo** no contexto de uma [expressão de coleção](search-query-odata-collection-operators.md) (`any` ou `all`) em um filtro. Uma variável de intervalo é como uma variável de loop que representa o elemento atual da coleção. Para coleções complexas, essa variável representa um objeto, por isso, você pode usar caminhos de campo para se referir a subcampos da variável. Isso é análogo a notação de ponto em várias linguagens de programação.

Exemplos de caminhos de campo são mostrados na tabela a seguir:

| Caminho de campo | DESCRIÇÃO |
| --- | --- |
| `HotelName` | Refere-se a um campo de nível superior do índice |
| `Address/City` | Refere-se ao `City` campo abaixo de um campo complexo no índice; `Address` é do tipo `Edm.ComplexType` neste exemplo |
| `Rooms/Type` | Refere-se ao `Type` campo abaixo de um campo de coleção complexa no índice; `Rooms` é do tipo `Collection(Edm.ComplexType)` neste exemplo |
| `Stores/Address/Country` | Refere-se para o `Country` campo abaixo do `Address` campo abaixo de um campo de coleção complexa no índice; `Stores` é do tipo `Collection(Edm.ComplexType)` e `Address` é do tipo `Edm.ComplexType` neste exemplo |
| `room/Type` | Refere-se para o `Type` campo abaixo do `room` variável de intervalo, por exemplo, na expressão de filtro `Rooms/any(room: room/Type eq 'deluxe')` |
| `store/Address/Country` | Refere-se ao `Country` campo abaixo do `Address` campo abaixo do `store` variável de intervalo, por exemplo, na expressão de filtro `Stores/any(store: store/Address/Country eq 'Canada')` |

O significado de um caminho de campo é diferente dependendo do contexto. Em filtros, um caminho de campo se refere ao valor de um *única instância* de um campo no documento atual. Em outros contextos, como **$orderby**, **$select**, ou no [pesquisa por campo em que a sintaxe completa do Lucene](query-lucene-syntax.md#bkmk_fields), um caminho de campo refere-se para o próprio campo. Essa diferença possui algumas consequências de como usar caminhos de campo em filtros.

Considere o caminho de campo `Address/City`. Em um filtro, isso se refere a uma cidade única para o documento atual, como "San Francisco". Em contraste, `Rooms/Type` refere-se ao `Type` campo inferior para salas de muitos (como "padrão" para o primeiro espaço, "luxo" para a segunda sala e assim por diante). Uma vez que `Rooms/Type` não se referir a um *instância única* do campo subpropriedades `Type`, ele não pode ser usado diretamente em um filtro. Em vez disso, para filtrar o tipo de espaço, você usaria um [expressão lambda](search-query-odata-collection-operators.md) com uma variável de intervalo, como este:

    Rooms/any(room: room/Type eq 'deluxe')

Neste exemplo, a variável de intervalo `room` aparece no `room/Type` caminho do campo. Dessa forma, `room/Type` refere-se para o tipo da sala atual no documento atual. Isso é uma única instância da `Type` subpropriedades de campo, para que possa ser usado diretamente no filtro.

### <a name="using-field-paths"></a>Usando caminhos de campo

Caminhos de campo são usados em diversos parâmetros dos [API do Azure Search](https://docs.microsoft.com/rest/api/searchservice/). A tabela a seguir lista todos os locais onde eles podem ser usados, além de quaisquer restrições no uso:

| API | Nome do parâmetro | Restrições |
| --- | --- | --- |
| [Crie](https://docs.microsoft.com/rest/api/searchservice/create-index) ou [atualização](https://docs.microsoft.com/rest/api/searchservice/update-index) índice | `suggesters/sourceFields` | Nenhum |
| [Crie](https://docs.microsoft.com/rest/api/searchservice/create-index) ou [atualização](https://docs.microsoft.com/rest/api/searchservice/update-index) índice | `scoringProfiles/text/weights` | Só pode se referir **pesquisável** campos |
| [Crie](https://docs.microsoft.com/rest/api/searchservice/create-index) ou [atualização](https://docs.microsoft.com/rest/api/searchservice/update-index) índice | `scoringProfiles/functions/fieldName` | Só pode se referir **filtráveis** campos |
| [Search](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `search` Quando `queryType` é `full` | Só pode se referir **pesquisável** campos |
| [Search](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `facet` | Só pode se referir **facetável** campos |
| [Search](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `highlight` | Só pode se referir **pesquisável** campos |
| [Search](https://docs.microsoft.com/rest/api/searchservice/search-documents) | `searchFields` | Só pode se referir **pesquisável** campos |
| [Sugerir](https://docs.microsoft.com/rest/api/searchservice/suggestions) e [preenchimento automático](https://docs.microsoft.com/rest/api/searchservice/autocomplete) | `searchFields` | Só pode se referir aos campos que fazem parte de um [encarregado da sugestão](index-add-suggesters.md) |
| [Pesquisa](https://docs.microsoft.com/rest/api/searchservice/search-documents), [sugerir](https://docs.microsoft.com/rest/api/searchservice/suggestions), e [preenchimento automático](https://docs.microsoft.com/rest/api/searchservice/autocomplete) | `$filter` | Só pode se referir **filtráveis** campos |
| [Pesquisa](https://docs.microsoft.com/rest/api/searchservice/search-documents) e [sugerir](https://docs.microsoft.com/rest/api/searchservice/suggestions) | `$orderby` | Só pode se referir **classificável** campos |
| [Pesquisa](https://docs.microsoft.com/rest/api/searchservice/search-documents), [sugerir](https://docs.microsoft.com/rest/api/searchservice/suggestions), e [pesquisa](https://docs.microsoft.com/rest/api/searchservice/lookup-document) | `$select` | Só pode se referir **recuperáveis** campos |

## <a name="constants"></a>Constantes

Constantes do OData são valores literais de um determinado [modelo de dados de entidade](https://docs.microsoft.com/dotnet/framework/data/adonet/entity-data-model) tipo (EDM). Ver [tipos de dados com suporte](https://docs.microsoft.com/rest/api/searchservice/supported-data-types) para obter uma lista dos tipos com suporte no Azure Search. Não há suporte para constantes de tipos de coleção.

A tabela a seguir mostra exemplos de constantes para cada um dos tipos de dados suportados pelo Azure Search:

| Tipo de dados | Constantes de exemplo |
| --- | --- |
| `Edm.Boolean` | `true`, `false` |
| `Edm.DateTimeOffset` | `2019-05-06T12:30:05.451Z` |
| `Edm.Double` | `3.14159`, `-1.2e7`, `NaN`, `INF`, `-INF` |
| `Edm.GeographyPoint` | `geography'POINT(-122.131577 47.678581)'` |
| `Edm.GeographyPolygon` | `geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'` |
| `Edm.Int32` | `123`, `-456` |
| `Edm.Int64` | `283032927235` |
| `Edm.String` | `'hello'` |

A seguir EBNF ([Extended Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) define a gramática para a maioria das constantes mostradas na tabela acima. A gramática para tipos geoespaciais pode ser encontrada no [funções geoespaciais do OData no Azure Search](search-query-odata-geo-spatial-functions.md).

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
constant ::=
    string_literal
    | date_time_offset_literal
    | integer_literal
    | float_literal
    | boolean_literal
    | 'null'

string_literal ::= "'"([^'] | "''")*"'"

date_time_offset_literal ::= date_part'T'time_part time_zone

date_part ::= year'-'month'-'day

time_part ::= hour':'minute(':'second('.'fractional_seconds)?)?

zero_to_fifty_nine ::= [0-5]digit

digit ::= [0-9]

year ::= digit digit digit digit

month ::= '0'[1-9] | '1'[0-2]

day ::= '0'[1-9] | [1-2]digit | '3'[0-1]

hour ::= [0-1]digit | '2'[0-3]

minute ::= zero_to_fifty_nine

second ::= zero_to_fifty_nine

fractional_seconds ::= integer_literal

time_zone ::= 'Z' | sign hour':'minute

sign ::= '+' | '-'

/* In practice integer literals are limited in length to the precision of
the corresponding EDM data type. */
integer_literal ::= digit+

float_literal ::=
    sign? whole_part fractional_part? exponent?
    | 'NaN'
    | '-INF'
    | 'INF'

whole_part ::= integer_literal

fractional_part ::= '.'integer_literal

exponent ::= 'e' sign? integer_literal

boolean_literal ::= 'true' | 'false'
```

Um diagrama de sintaxe interativa também está disponível:

> [!div class="nextstepaction"]
> [Diagrama de sintaxe do OData para o Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#constant)

> [!NOTE]
> Ver [referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md) para o EBNF completa.

## <a name="building-expressions-from-field-paths-and-constants"></a>Compilando expressões de constantes e caminhos de campo

Constantes e os caminhos de campo são a parte mais básica de uma expressão de OData, mas ainda estiver completas expressões elas mesmas. Na verdade, o **$select** parâmetro no Azure Search é nada além de uma lista separada por vírgulas de caminhos de campo, e **$orderby** não é muito mais complicado que **$select**. Se você tiver um campo do tipo `Edm.Boolean` em seu índice, você pode até escrever um filtro que não é nada, mas o caminho do campo. As constantes `true` e `false` da mesma forma são filtros válidos.

No entanto, na maioria das vezes você precisará expressões mais complexas que se referem a mais de um campo e constante. Essas expressões são criadas de maneiras diferentes dependendo do parâmetro.

A seguir EBNF ([Extended Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) define a gramática para o **$filter**, **$orderby**, e **$select** parâmetros. Elas são criadas de expressões mais simples que se referem a constantes e os caminhos de campo:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
filter_expression ::= boolean_expression

order_by_expression ::= order_by_clause(',' order_by_clause)*

select_expression ::= '*' | field_path(',' field_path)*
```

Um diagrama de sintaxe interativa também está disponível:

> [!div class="nextstepaction"]
> [Diagrama de sintaxe do OData para o Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#filter_expression)

> [!NOTE]
> Ver [referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md) para o EBNF completa.

O **$orderby** e **$select** parâmetros são as duas listas separada por vírgulas de expressões mais simples. O **$filter** parâmetro é uma expressão booliana que é composta de subexpressões mais simples. Essas expressões subpropriedades são combinadas usando operadores lógicos, como [ `and`, `or`, e `not` ](search-query-odata-logical-operators.md), os operadores de comparação, como [ `eq`, `lt`, `gt`e assim por diante](search-query-odata-comparison-operators.md)e operadores de coleção, como [ `any` e `all` ](search-query-odata-collection-operators.md).

O **$filter**, **$orderby**, e **$select** parâmetros são explorados mais detalhadamente nos artigos a seguir:

- [Sintaxe de OData $filter no Azure Search](search-query-odata-filter.md)
- [Sintaxe de OData $orderby no Azure Search](search-query-odata-orderby.md)
- [Sintaxe de OData $select no Azure Search](search-query-odata-select.md)

## <a name="see-also"></a>Consulte também  

- [Navegação facetada no Azure Search](search-faceted-navigation.md)
- [Filtros no Azure Search](search-filters.md)
- [Pesquisar documentos &#40;API REST do Serviço do Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
- [Sintaxe de consulta Lucene](query-lucene-syntax.md)
- [Sintaxe de consulta simples no Azure Search](query-simple-syntax.md)
