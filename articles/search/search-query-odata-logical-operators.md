---
title: Referência de operador lógico de OData - Azure Search
description: Operadores lógicos de OData e, ou e não, em consultas de pesquisa do Azure.
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
ms.openlocfilehash: de93765117b4cafe70e5ad277e32ca0a1fa8d90a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079763"
---
# <a name="odata-logical-operators-in-azure-search---and-or-not"></a>Operadores lógicos de OData no Azure Search - `and`, `or`, `not`

[Expressões de filtro OData](query-odata-filter-orderby-syntax.md) no Azure Search são expressões Boolianas que avaliam `true` ou `false`. Você pode escrever um filtro complexo, escrevendo uma série de [filtros mais simples](search-query-odata-comparison-operators.md) e compondo-los usando os operadores lógicos da [algebra booleano](https://en.wikipedia.org/wiki/Boolean_algebra):

- `and`: Um operador binário que é avaliada como `true` se ambos os seus subexpressões esquerda e direita forem avaliados como `true`.
- `or`: Um operador binário que é avaliada como `true` se qualquer um dos seus subexpressões esquerdas ou direita é avaliada como `true`.
- `not`: Um operador unário que é avaliada como `true` se seu subexpressão for avaliada como `false`e vice-versa.

Dessas, junto com o [operadores de coleção `any` e `all` ](search-query-odata-collection-operators.md), permitem que você construa filtros que podem expressar os critérios de pesquisa muito complexas.

## <a name="syntax"></a>Sintaxe

A seguir EBNF ([Extended Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) define a gramática de uma expressão de OData que usa os operadores lógicos.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
logical_expression ::=
    boolean_expression ('and' | 'or') boolean_expression
    | 'not' boolean_expression
```

Um diagrama de sintaxe interativa também está disponível:

> [!div class="nextstepaction"]
> [Diagrama de sintaxe do OData para o Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#logical_expression)

> [!NOTE]
> Ver [referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md) para o EBNF completa.

Há duas formas de expressões lógicas: binário (`and`/`or`), em que há duas subexpressões e unário (`not`), em que há apenas um. As expressões de subpropriedades podem ser expressões Boolianas de qualquer tipo:

- Variáveis de campos ou intervalo do tipo `Edm.Boolean`
- Funções que retornam valores do tipo `Edm.Boolean`, tais como `geo.intersects` ou `search.ismatch`
- [Expressões de comparação](search-query-odata-comparison-operators.md), tais como `rating gt 4`
- [Expressões de coleção](search-query-odata-collection-operators.md), tais como `Rooms/any(room: room/Type eq 'Deluxe Room')`
- Os literais boolianos `true` ou `false`.
- Outras expressões lógicas construídos usando `and`, `or`, e `not`.

> [!IMPORTANT]
> Há algumas situações em que nem todos os tipos de subexpressão podem ser usados com `and` / `or`, particularmente no expressões lambda. Ver [operadores de coleção OData no Azure Search](search-query-odata-collection-operators.md#limitations) para obter detalhes.

### <a name="logical-operators-and-null"></a>Operadores lógicos e `null`

A maioria das expressões Boolianas como funções e comparações não podem produzir `null` valores e os operadores lógicos não podem ser aplicado a `null` literal diretamente (por exemplo, `x and null` não é permitido). No entanto, podem ser os campos boolianos `null`, portanto, você precisa estar ciente de como o `and`, `or`, e `not` operadores se comportam na presença de nulo. Isso está resumido na tabela a seguir, onde `b` é um campo do tipo `Edm.Boolean`:

| Expression | Resultado quando `b` é `null` |
| --- | --- |
| `b` | `false` |
| `not b` | `true` |
| `b eq true` | `false` |
| `b eq false` | `false` |
| `b eq null` | `true` |
| `b ne true` | `true` |
| `b ne false` | `true` |
| `b ne null` | `false` |
| `b and true` | `false` |
| `b and false` | `false` |
| `b or true` | `true` |
| `b or false` | `false` |

Quando um campo booleano `b` aparece sozinho em uma expressão de filtro, ele se comporta como se ele tivesse sido escrito `b eq true`, portanto, se `b` é `null`, a expressão é avaliada como `false`. Da mesma forma, `not b` se comporta como `not (b eq true)`, portanto, ele será avaliado como `true`. Dessa forma, `null` campos possuem o mesmo comportam `false`. Isso é consistente com como eles se comportam quando combinado com outras expressões usando `and` e `or`, conforme mostrado na tabela acima. Apesar disso, uma comparação direta ao `false` (`b eq false`) ainda será avaliada como `false`. Em outras palavras, `null` não é igual a `false`, mesmo que ele se comporta como ele em expressões Boolianas.

## <a name="examples"></a>Exemplos

Correspondência de documentos em que o `rating` campo está entre 3 e 5, inclusive:

    rating ge 3 and rating le 5

Correspondência de documentos em que todos os elementos do `ratings` campo é menor que 3 ou maior que 5:

    ratings/all(r: r lt 3 or r gt 5)

Correspondência de documentos em que o `location` campo é dentro do polígono determinado, e o documento não contém o termo "public".

    geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))') and not search.ismatch('public')

Corresponde a documentos para hotéis em Vancouver, Canadá onde há uma sala deluxe com uma base de dados de taxa menor que 160:

    Address/City eq 'Vancouver' and Address/Country eq 'Canada' and Rooms/any(room: room/Type eq 'Deluxe Room' and room/BaseRate lt 160)

## <a name="next-steps"></a>Próximas etapas  

- [Filtros no Azure Search](search-filters.md)
- [Visão geral de linguagem de expressão do OData para o Azure Search](query-odata-filter-orderby-syntax.md)
- [Referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md)
- [Pesquisar documentos &#40;API REST do Serviço do Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
