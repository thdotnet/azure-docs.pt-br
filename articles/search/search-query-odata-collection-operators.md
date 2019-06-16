---
title: Referência de operador de coleção OData - Azure Search
description: Operadores de coleção OData, toda e qualquer e as expressões lambda em consultas de pesquisa do Azure.
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
ms.openlocfilehash: 7afafe158732b14ebe314eeee5d015acddc55b72
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079932"
---
# <a name="odata-collection-operators-in-azure-search---any-and-all"></a>Operadores de coleção OData no Azure Search - `any` e `all`

Ao escrever uma [expressão de filtro OData](query-odata-filter-orderby-syntax.md) para usar com o Azure Search, geralmente é útil filtrar em campos da coleção. Você pode conseguir isso usando o `any` e `all` operadores.

## <a name="syntax"></a>Sintaxe

A seguir EBNF ([Extended Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) define a gramática de uma expressão de OData que usa `any` ou `all`.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
collection_filter_expression ::=
    field_path'/all(' lambda_expression ')'
    | field_path'/any(' lambda_expression ')'
    | field_path'/any()'

lambda_expression ::= identifier ':' boolean_expression
```

Um diagrama de sintaxe interativa também está disponível:

> [!div class="nextstepaction"]
> [Diagrama de sintaxe do OData para o Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#collection_filter_expression)

> [!NOTE]
> Ver [referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md) para o EBNF completa.

Há três formas de expressão que filtram as coleções.

- As duas primeiras iterar sobre um campo de coleção, aplicando um predicado fornecido na forma de uma expressão lambda para cada elemento da coleção.
  - Uma expressão que usa `all` retorna `true` se o predicado é verdadeiro para todos os elementos da coleção.
  - Uma expressão que usa `any` retorna `true` se o predicado é verdadeiro para pelo menos um elemento da coleção.
- O terceiro formulário de coleção de filtro usa `any` sem uma expressão lambda para testar se um campo de coleção está vazio. Se a coleção tem todos os elementos, ele retorna `true`. Se a coleção está vazia, ele retorna `false`.

Um **expressão lambda** em uma coleção de filtro é como o corpo de um loop em uma linguagem de programação. Ele define uma variável chamada a **variável de intervalo**, que contém o elemento atual da coleção durante a iteração. Ele também define outra expressão booliana que é o critério de filtro para aplicar a variável de intervalo para cada elemento da coleção.

## <a name="examples"></a>Exemplos

Correspondência de documentos cuja `tags` campo contém exatamente é a cadeia de caracteres "wifi":

    tags/any(t: t eq 'wifi')

Correspondência de documentos em que todos os elementos do `ratings` campo se encaixa entre 3 e 5, inclusive:

    ratings/all(r: r ge 3 and r le 5)

Documentos de correspondência onde qualquer uma da área geográfica coordena no `locations` campo está dentro do polígono determinado:

    locations/any(loc: geo.intersects(loc, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))

Correspondência de documentos em que o `rooms` campo está vazio:

    not rooms/any()

Corresponde a documentos em que, para todas as salas, o `rooms/amenities` campo contém "tv" e `rooms/baseRate` é menor que 100:

    rooms/all(room: room/amenities/any(a: a eq 'tv') and room/baseRate lt 100.0)

## <a name="limitations"></a>Limitações

Não, todos os recursos de expressões de filtro estão disponível dentro do corpo de uma expressão lambda. As limitações são diferentes dependendo do tipo de dados do campo de coleção que você deseja filtrar. A tabela a seguir resume as limitações.

[!INCLUDE [Limitations on OData lambda expressions in Azure Search](../../includes/search-query-odata-lambda-limitations.md)]

Para obter mais detalhes sobre essas limitações, bem como exemplos, consulte [solução de problemas de filtros de coleção no Azure Search](search-query-troubleshoot-collection-filters.md). Para obter informações mais detalhadas sobre por que essas limitações existe, consulte [Noções básicas sobre filtros de coleção no Azure Search](search-query-understand-collection-filters.md).

## <a name="next-steps"></a>Próximas etapas  

- [Filtros no Azure Search](search-filters.md)
- [Visão geral de linguagem de expressão do OData para o Azure Search](query-odata-filter-orderby-syntax.md)
- [Referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md)
- [Pesquisar documentos &#40;API REST do Serviço do Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
