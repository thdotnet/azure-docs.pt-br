---
title: Selecione referência OData - Azure Search
description: Referência de linguagem do OData para selecionar a sintaxe em consultas de pesquisa do Azure.
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
ms.openlocfilehash: 9383ae725fffac55854488ffbc6aeb161ae7e0c2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079672"
---
# <a name="odata-select-syntax-in-azure-search"></a>Sintaxe de OData $select no Azure Search

 Você pode usar o [OData **$select** parâmetro](query-odata-filter-orderby-syntax.md) para escolher quais campos serão incluídos nos resultados da pesquisa do Azure Search. Este artigo descreve a sintaxe da **$select** em detalhes. Para obter mais informações sobre como usar **$select** ao apresentar os resultados da pesquisa, consulte [como trabalhar com a pesquisa resulta no Azure Search](search-pagination-page-layout.md).

## <a name="syntax"></a>Sintaxe

O **$select** parâmetro determina quais campos para cada documento são retornados no conjunto de resultados de consulta. A seguir EBNF ([Extended Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) define a gramática para o **$select** parâmetro:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
select_expression ::= '*' | field_path(',' field_path)*

field_path ::= identifier('/'identifier)*
```

Um diagrama de sintaxe interativa também está disponível:

> [!div class="nextstepaction"]
> [Diagrama de sintaxe do OData para o Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#select_expression)

> [!NOTE]
> Ver [referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md) para o EBNF completa.

O **$select** parâmetro vem em duas formas:

1. Um único asterisco (`*`), indicando que todos os campos recuperáveis devem ser retornados, ou
1. Uma lista separada por vírgulas de caminhos de campo, identificar quais campos deve ser retornada.

Ao usar o segundo formulário, você somente pode especificar campos recuperáveis na lista.

Se você listar um campo complexo sem especificar seus sub-campos explicitamente, todos os campos recuperáveis de subpropriedades serão incluídos no conjunto de resultados de consulta. Por exemplo, suponha que o índice tem um `Address` campo com `Street`, `City`, e `Country` subcampos são todos recuperáveis. Se você especificar `Address` na **$select**, os resultados da consulta incluirá todos os três campos de subpropriedades.

## <a name="examples"></a>Exemplos

Incluir o `HotelId`, `HotelName`, e `Rating` campos de nível superior nos resultados, bem como o `City` campo abaixo de `Address`:

    $select=HotelId, HotelName, Rating, Address/City

Um resultado de exemplo pode ter esta aparência:

```json
{
  "HotelId": "1",
  "HotelName": "Secret Point Motel",
  "Rating": 4,
  "Address": {
    "City": "New York"
  }
}
```

Incluir o `HotelName` campo de nível superior nos resultados, bem como todos os campos de subpropriedades da `Address`e o `Type` e `BaseRate` subcampos de cada objeto no `Rooms` coleção:

    $select=HotelName, Address, Rooms/Type, Rooms/BaseRate

Um resultado de exemplo pode ter esta aparência:

```json
{
  "HotelName": "Secret Point Motel",
  "Rating": 4,
  "Address": {
    "StreetAddress": "677 5th Ave",
    "City": "New York",
    "StateProvince": "NY",
    "Country": "USA",
    "PostalCode": "10022"
  },
  "Rooms": [
    {
      "Type": "Budget Room",
      "BaseRate": 9.69
    },
    {
      "Type": "Budget Room",
      "BaseRate": 8.09
    }
  ]
}
```

## <a name="next-steps"></a>Próximas etapas  

- [Como trabalhar com a pesquisa resulta no Azure Search](search-pagination-page-layout.md)
- [Visão geral de linguagem de expressão do OData para o Azure Search](query-odata-filter-orderby-syntax.md)
- [Referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md)
- [Pesquisar documentos &#40;API REST do Serviço do Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
