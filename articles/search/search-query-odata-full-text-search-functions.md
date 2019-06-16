---
title: Referência de função de pesquisa de texto completo do OData - Azure Search
description: Funções de pesquisa de texto completo do OData, IsMatch e ismatchscoring em consultas de pesquisa do Azure.
ms.date: 06/13/2019
services: search
ms.service: search
ms.topic: conceptual
author: brjohnstmsft
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
ms.openlocfilehash: 158312a7afe88e7b9885376c5d28b01958acbbfb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079802"
---
# <a name="odata-full-text-search-functions-in-azure-search---searchismatch-and-searchismatchscoring"></a>Funções de pesquisa de texto completo do OData no Azure Search - `search.ismatch` e `search.ismatchscoring`

O Azure Search dá suporte à pesquisa de texto completo no contexto de [expressões de filtro OData](query-odata-filter-orderby-syntax.md) por meio de `search.ismatch` e `search.ismatchscoring` funções. Essas funções permitem combinar pesquisa de texto completo com a filtragem de booliano estrita de maneiras que não são possíveis apenas por meio de nível superior `search` parâmetro do [API de pesquisa](https://docs.microsoft.com/rest/api/searchservice/search-documents).

> [!NOTE]
> O `search.ismatch` e `search.ismatchscoring` funções têm suporte apenas em filtros na [API de pesquisa](https://docs.microsoft.com/rest/api/searchservice/search-documents). Eles não têm suporte nas [sugerir](https://docs.microsoft.com/rest/api/searchservice/suggestions) ou [AutoCompletar](https://docs.microsoft.com/rest/api/searchservice/autocomplete) APIs.

## <a name="syntax"></a>Sintaxe

A seguir EBNF ([Extended Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) define a gramática da `search.ismatch` e `search.ismatchscoring` funções:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
search_is_match_call ::=
    'search.ismatch'('scoring')?'(' search_is_match_parameters ')'

search_is_match_parameters ::=
    string_literal(',' string_literal(',' query_type ',' search_mode)?)?

query_type ::= "'full'" | "'simple'"

search_mode ::= "'any'" | "'all'"
```

Um diagrama de sintaxe interativa também está disponível:

> [!div class="nextstepaction"]
> [Diagrama de sintaxe do OData para o Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#search_is_match_call)

> [!NOTE]
> Ver [referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md) para o EBNF completa.

### <a name="searchismatch"></a>search.ismatch

O `search.ismatch` função avalia uma consulta de pesquisa de texto completo como parte de uma expressão de filtro. Os documentos que correspondem à consulta da pesquisa serão retornados no conjunto de resultados. As seguintes sobrecargas desta função estão disponíveis:

- `search.ismatch(search)`
- `search.ismatch(search, searchFields)`
- `search.ismatch(search, searchFields, queryType, searchMode)`

Os parâmetros são definidos na tabela a seguir:

| Nome do parâmetro | Type | DESCRIÇÃO |
| --- | --- | --- |
| `search` | `Edm.String` | A consulta de pesquisa (no [simples](query-simple-syntax.md) ou [completo](query-lucene-syntax.md) sintaxe de consulta Lucene). |
| `searchFields` | `Edm.String` | Lista separada por vírgulas de campos de pesquisa para pesquisar na; o padrão é todos os campos pesquisáveis no índice. Ao usar [respondidas pesquisa](query-lucene-syntax.md#bkmk_fields) no `search` parâmetro, os especificadores de campo na consulta Lucene substituem todos os campos especificados nesse parâmetro. |
| `queryType` | `Edm.String` | `'simple'` ou `'full'`; o padrão é `'simple'`. Especifica qual idioma de consulta foi usado no parâmetro `search`. |
| `searchMode` | `Edm.String` | `'any'` ou `'all'`, o padrão é `'any'`. Indica se algum ou todos a pesquisa de termos no `search` parâmetro deve ser iguais para considerar o documento como uma correspondência. Ao usar o [operadores booleanos Lucene](query-lucene-syntax.md#bkmk_boolean) no `search` parâmetro, eles terão precedência sobre esse parâmetro. |

Todos os parâmetros acima são equivalentes para os respectivos [parâmetros de solicitação de pesquisa na API de pesquisa](https://docs.microsoft.com/rest/api/searchservice/search-documents).

O `search.ismatch` função retorna um valor do tipo `Edm.Boolean`, que permite que você compô-la com outras expressões de subpropriedades de filtro usando o valor booliano [operadores lógicos](search-query-odata-logical-operators.md).

> [!NOTE]
> O Azure Search não oferece suporte ao uso `search.ismatch` ou `search.ismatchscoring` dentro de expressões lambda. Isso significa que não é possível para filtros de gravação nas coleções de objetos que podem correlacionar as correspondências de pesquisa de texto completo com correspondências de filtro estrito no mesmo objeto. Para obter mais detalhes sobre essa limitação, bem como exemplos, consulte [solução de problemas de filtros de coleção no Azure Search](search-query-troubleshoot-collection-filters.md). Para obter informações mais detalhadas sobre por que essa limitação existe, consulte [Noções básicas sobre filtros de coleção no Azure Search](search-query-understand-collection-filters.md).


### <a name="searchismatchscoring"></a>search.ismatchscoring

O `search.ismatchscoring` função, como o `search.ismatch` retornos de função, `true` para documentos que correspondem à consulta de pesquisa de texto completo passado como um parâmetro. A diferença entre eles é que a pontuação de relevância dos documentos que correspondem à consulta `search.ismatchscoring` também contribui para a pontuação geral do documento, enquanto que no caso de `search.ismatch`, a pontuação de documento não se altera. As seguintes sobrecargas dessa função estão disponíveis com parâmetros idênticos aos de `search.ismatch`:

- `search.ismatchscoring(search)`
- `search.ismatchscoring(search, searchFields)`
- `search.ismatchscoring(search, searchFields, queryType, searchMode)`

Tanto a `search.ismatch` e `search.ismatchscoring` funções podem ser usadas na mesma expressão de filtro.

## <a name="examples"></a>Exemplos

Localizar documentos com as palavras "orla marítima". Essa consulta de filtro é idêntica a uma [solicitação de pesquisa](https://docs.microsoft.com/rest/api/searchservice/search-documents) com `search=waterfront`.

    search.ismatchscoring('waterfront')

Localizar documentos com a palavra "hostel" e classificação maior ou igual a 4, ou documentos com a palavra "motel" e classificação igual a 5. Observe que essa solicitação não pode ser expressa sem a função `search.ismatchscoring`.

    search.ismatchscoring('hostel') and Rating ge 4 or search.ismatchscoring('motel') and Rating eq 5

Localizar documentos sem a palavra "luxo".

    not search.ismatch('luxury')

Localizar documentos com a frase "vista para o mar" ou classificação igual a 5. A consulta `search.ismatchscoring` será executada apenas em relação aos campos `HotelName` e `Rooms/Description`.

Documentos que correspondem à apenas a segunda cláusula a disjunção serão retornados--hotéis com `Rating` igual a 5. Para torná-lo claro que esses documentos não correspondem a qualquer uma das partes da expressão classificadas, serão retornados com pontuação igual a zero.

    search.ismatchscoring('"ocean view"', 'Rooms/Description,HotelName') or Rating eq 5

Localizar documentos, onde os termos "hotel" e "aeroporto" estão dentro de 5 palavras uns dos outros na descrição do hotel e onde fumar não é permitido em pelo menos algumas das salas. Essa consulta usa a [linguagem de consulta Lucene completa](query-lucene-syntax.md).

    search.ismatch('"hotel airport"~5', 'Description', 'full', 'any') and Rooms/any(room: not room/SmokingAllowed)

## <a name="next-steps"></a>Próximas etapas  

- [Filtros no Azure Search](search-filters.md)
- [Visão geral de linguagem de expressão do OData para o Azure Search](query-odata-filter-orderby-syntax.md)
- [Referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md)
- [Pesquisar documentos &#40;API REST do Serviço do Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
