---
title: Solução de problemas de filtros de coleção OData - Azure Search
description: Solução de problemas de erros de filtro de coleção OData em consultas de pesquisa do Azure.
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
ms.openlocfilehash: c7fa00c82eea03a50bae22fcb1ad16e230aa5bcb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079620"
---
# <a name="troubleshooting-odata-collection-filters-in-azure-search"></a>Solução de problemas de filtros de coleção OData no Azure Search

Para [filtro](query-odata-filter-orderby-syntax.md) nos campos da coleção no Azure Search, você pode usar o [ `any` e `all` operadores](search-query-odata-collection-operators.md) junto com **expressões lambda**. Uma expressão lambda é um subfiltro que é aplicado a cada elemento de uma coleção.

Não, todos os recursos de expressões de filtro estão disponível dentro de uma expressão lambda. Quais recursos estão disponíveis varia dependendo do tipo de dados do campo de coleção que você deseja filtrar. Isso pode resultar em um erro se você tentar usar um recurso em uma expressão lambda que não tem suporte nesse contexto. Se você estiver encontrando tais erros ao tentar gravar um filtro complexo em campos de coleção, este artigo o ajudará a solucionar o problema.

## <a name="common-collection-filter-errors"></a>Erros comuns de filtro de coleção

A tabela a seguir lista os erros que podem ocorrer ao tentar executar um filtro de coleção. Esses erros ocorrem quando você usa um recurso de expressões de filtro que não tem suporte dentro de uma expressão lambda. Cada erro fornece algumas diretrizes sobre como você pode reescrever seu filtro para evitar o erro. A tabela também inclui um link para a seção relevante deste artigo que fornece mais informações sobre como evitar esse erro.

| Mensagem de erro | Situação | Para obter mais informações, consulte |
| --- | --- | --- |
| A função 'ismatch' não tem nenhum parâmetro associado à variável de intervalo '. Só associado campo referências têm suporte dentro de expressões lambda ('any' ou 'todos'). Por favor, altere o filtro para que a função 'ismatch' está fora da expressão lambda e tente novamente. | Usando o `search.ismatch` ou `search.ismatchscoring` dentro de uma expressão lambda | [Regras de filtragem de coleções complexas](#bkmk_complex) |
| Expressão lambda inválido. Encontrado um teste de igualdade ou desigualdade em que o oposto era esperado em uma expressão lambda que itera em um campo do tipo Collection. Para 'any', use expressões do formulário 'x eq y' ou 'search.in(...)'. Para 'all', use expressões do formulário 'x ne y', 'não (x y eq)' ou 'não search.in(...)'. | Filtrando um campo de tipo `Collection(Edm.String)` | [Regras de filtragem de coleções de cadeia de caracteres](#bkmk_strings) |
| Expressão lambda inválido. Encontrado um formato sem suporte da expressão booliana complexa. Para 'any', use expressões que são 'ORs de ANDs', também conhecido como formato de Normal disjuntiva. Por exemplo: '(a and b) ou (c e d)' onde a, b, c e d são subexpressões comparação ou de igualdade. Para 'all', use expressões que são 'ANDs de ORs', também conhecido como formato de Normal Conjuntiva. Por exemplo: '(a or b) e (c ou d)' onde a, b, c e d são subexpressões comparação ou de desigualdade. Exemplos de expressões de comparação: ' x gt 5 ° ' x le 2'. Exemplo de uma expressão de igualdade: ' x eq 5 °. Exemplo de uma expressão de desigualdade: ' x ne 5 °. | Filtragem em campos do tipo `Collection(Edm.DateTimeOffset)`, `Collection(Edm.Double)`, `Collection(Edm.Int32)`, ou `Collection(Edm.Int64)` | [Regras de filtragem de coleções comparáveis](#bkmk_comparables) |
| Expressão lambda inválido. Encontrado um uso sem suporte de geo.distance() ou geo.intersects() em uma expressão lambda que itera em um campo do tipo Collection(Edm.GeographyPoint). Para 'any', verifique se você comparar geo.distance() usando os operadores 'lt' ou 'le' e certifique-se de que qualquer uso do geo.intersects() não é negado. Para 'all', verifique se você comparar geo.distance() usando os operadores 'gt' ou 'ge' e certifique-se de que qualquer uso do geo.intersects() é negado. | Filtrando um campo de tipo `Collection(Edm.GeographyPoint)` | [Regras de filtragem de coleções de GeographyPoint](#bkmk_geopoints) |
| Expressão lambda inválido. Não há suporte para expressões booleanas complexas em expressões lambda que iteram em campos do tipo Collection(Edm.GeographyPoint). Junte subexpressões com para 'any', 'ou'; 'e' não tem suporte. Para 'all', Junte-se subexpressões com 'e'; 'ou' não tem suporte. | Filtragem em campos do tipo `Collection(Edm.String)` ou `Collection(Edm.GeographyPoint)` | [Regras de filtragem de coleções de cadeia de caracteres](#bkmk_strings) <br/><br/> [Regras de filtragem de coleções de GeographyPoint](#bkmk_geopoints) |
| Expressão lambda inválido. Encontrado um operador de comparação (um dos 'lt', 'le', 'gt' ou 'ge'). Somente os operadores de igualdade são permitidos em expressões lambda que iterar sobre os campos do tipo Collection. Para 'any', use expressões do formulário 'x eq y'. Para 'all', use expressões do formulário 'x ne y' ou 'não (x y eq)'. | Filtrando um campo de tipo `Collection(Edm.String)` | [Regras de filtragem de coleções de cadeia de caracteres](#bkmk_strings) |

<a name="bkmk_examples"></a>

## <a name="how-to-write-valid-collection-filters"></a>Como escrever filtros de coleção válido

As regras para escrever os filtros de coleção válido são diferentes para cada tipo de dados. As seções a seguir descrevem as regras, mostrando os exemplos de filtro que há suporte para recursos e que não são:

- [Regras de filtragem de coleções de cadeia de caracteres](#bkmk_strings)
- [Regras de filtragem coleções Boolianas](#bkmk_bools)
- [Regras de filtragem de coleções de GeographyPoint](#bkmk_geopoints)
- [Regras de filtragem de coleções comparáveis](#bkmk_comparables)
- [Regras de filtragem de coleções complexas](#bkmk_complex)

<a name="bkmk_strings"></a>

## <a name="rules-for-filtering-string-collections"></a>Regras de filtragem de coleções de cadeia de caracteres

Dentro de expressões lambda para coleções de cadeia de caracteres, são os operadores de comparação única que podem ser usados `eq` e `ne`.

> [!NOTE]
> O Azure Search não oferece suporte a `lt` / `le` / `gt` / `ge` operadores para cadeias de caracteres, se dentro ou fora de uma expressão lambda.

O corpo de um `any` só pode testar a igualdade enquanto o corpo de um `all` só pode testar quanto à desigualdade.

Também é possível combinar várias expressões, por meio `or` no corpo de uma `any`e por meio `and` no corpo de um `all`. Uma vez que o `search.in` função é equivalente à combinação de verificações de igualdade com `or`, ele também é permitido no corpo de um `any`. Por outro lado, `not search.in` é permitida no corpo de um `all`.

Por exemplo, essas expressões são permitidas:

- `tags/any(t: t eq 'books')`
- `tags/any(t: search.in(t, 'books, games, toys'))`
- `tags/all(t: t ne 'books')`
- `tags/all(t: not (t eq 'books'))`
- `tags/all(t: not search.in(t, 'books, games, toys'))`
- `tags/any(t: t eq 'books' or t eq 'games')`
- `tags/all(t: t ne 'books' and not (t eq 'games'))`

Embora essas expressões não são permitidas:

- `tags/any(t: t ne 'books')`
- `tags/any(t: not search.in(t, 'books, games, toys'))`
- `tags/all(t: t eq 'books')`
- `tags/all(t: search.in(t, 'books, games, toys'))`
- `tags/any(t: t eq 'books' and t ne 'games')`
- `tags/all(t: t ne 'books' or not (t eq 'games'))`

<a name="bkmk_bools"></a>

## <a name="rules-for-filtering-boolean-collections"></a>Regras de filtragem coleções Boolianas

O tipo `Edm.Boolean` dá suporte apenas a `eq` e `ne` operadores. Como tal, ele não faz muito sentido para permitir que essas cláusulas que verificam a mesma variável de intervalo com a combinação `and` / `or` , pois isso resultaria sempre a tautologies ou contradições.

Aqui estão alguns exemplos de filtros boolianos coleções que são permitidos:

- `flags/any(f: f)`
- `flags/all(f: f)`
- `flags/any(f: f eq true)`
- `flags/any(f: f ne true)`
- `flags/all(f: not f)`
- `flags/all(f: not (f eq true))`

Ao contrário das coleções de cadeia de caracteres, Boolianas coleções não têm limites em que o operador pode ser usado em qual tipo de expressão lambda. Ambos `eq` e `ne` pode ser usado no corpo da `any` ou `all`.

Expressões, como a seguir não são permitidas para coleções de Boolean:

- `flags/any(f: f or not f)`
- `flags/any(f: f or f)`
- `flags/all(f: f and not f)`
- `flags/all(f: f and f eq true)`

<a name="bkmk_geopoints"></a>

## <a name="rules-for-filtering-geographypoint-collections"></a>Regras de filtragem de coleções de GeographyPoint

Valores do tipo `Edm.GeographyPoint` em uma coleção não podem ser comparados diretamente umas às outras. Em vez disso, eles devem ser usados como parâmetros para o `geo.distance` e `geo.intersects` funções. O `geo.distance` função por sua vez deve ser comparada a um valor de distância usando um dos operadores de comparação `lt`, `le`, `gt`, ou `ge`. Essas regras também se aplicam aos campos de EDM. geographypoint não seja de coleção.

Como coleções de cadeia de caracteres, `Edm.GeographyPoint` coleções têm algumas regras para como as funções geoespaciais podem ser usadas e combinadas em diferentes tipos de expressões lambda:

- Quais operadores de comparação, você pode usar com o `geo.distance` função depende do tipo de expressão lambda. Para `any`, você pode usar apenas `lt` ou `le`. Para `all`, você pode usar apenas `gt` ou `ge`. Você pode negar expressões que envolvem `geo.distance`, mas você terá de alterar o operador de comparação (`geo.distance(...) lt x` se torna `not (geo.distance(...) ge x)` e `geo.distance(...) le x` se torna `not (geo.distance(...) gt x)`).
- No corpo de uma `all`, o `geo.intersects` função deve ser negada. Por outro lado, no corpo de uma `any`, o `geo.intersects` função não deve ser negada.
- No corpo de uma `any`, geoespacial expressões podem ser combinadas usando `or`. No corpo de uma `all`, tais expressões podem ser combinados usando `and`.

As limitações acima existirem por motivos semelhantes, como a limitação de igualdade/desigualdade em coleções de cadeia de caracteres. Ver [filtros de coleção de Noções básicas sobre OData no Azure Search](search-query-understand-collection-filters.md) para uma análise mais profunda sobre esses motivos.

Aqui estão alguns exemplos de filtros de `Edm.GeographyPoint` coleções que são permitidas:

- `locations/any(l: geo.distance(l, geography'POINT(-122 49)') lt 10)`
- `locations/any(l: not (geo.distance(l, geography'POINT(-122 49)') ge 10) or geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/all(l: geo.distance(l, geography'POINT(-122 49)') ge 10 and not geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`

Expressões, como a seguir não são permitidas para `Edm.GeographyPoint` coleções:

- `locations/any(l: l eq geography'POINT(-122 49)')`
- `locations/any(l: not geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/all(l: geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/any(l: geo.distance(l, geography'POINT(-122 49)') gt 10)`
- `locations/all(l: geo.distance(l, geography'POINT(-122 49)') lt 10)`
- `locations/any(l: geo.distance(l, geography'POINT(-122 49)') lt 10 and geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`
- `locations/all(l: geo.distance(l, geography'POINT(-122 49)') le 10 or not geo.intersects(l, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))'))`

<a name="bkmk_comparables"></a>

## <a name="rules-for-filtering-comparable-collections"></a>Regras de filtragem de coleções comparáveis

Esta seção se aplica a todos os seguintes tipos de dados:

- `Collection(Edm.DateTimeOffset)`
- `Collection(Edm.Double)`
- `Collection(Edm.Int32)`
- `Collection(Edm.Int64)`

Tipos, como `Edm.Int32` e `Edm.DateTimeOffset` dão suporte a todos os seis dos operadores de comparação: `eq`, `ne`, `lt`, `le`, `gt`, e `ge`. Expressões lambda nas coleções desses tipos podem conter expressões simples usando qualquer um desses operadores. Isso se aplica a ambos `any` e `all`. Por exemplo, esses filtros são permitidos:

- `ratings/any(r: r ne 5)`
- `dates/any(d: d gt 2017-08-24T00:00:00Z)`
- `not margins/all(m: m eq 3.5)`

No entanto, há limitações sobre como essas expressões de comparação podem ser combinadas em expressões mais complexas dentro de uma expressão lambda:

- Regras para `any`:
  - Expressões simples de desigualdade utilmente não podem ser combinadas com quaisquer outras expressões. Por exemplo, essa expressão é permitida:
    - `ratings/any(r: r ne 5)`

    mas não é esta expressão:
    - `ratings/any(r: r ne 5 and r gt 2)`

    e, embora essa expressão é permitida, não é útil porque as condições estão sobrepostos:
    - `ratings/any(r: r ne 5 or r gt 7)`
  - Expressões de comparação simples envolva `eq`, `lt`, `le`, `gt`, ou `ge` pode ser combinado com `and` / `or`. Por exemplo:
    - `ratings/any(r: r gt 2 and r le 5)`
    - `ratings/any(r: r le 5 or r gt 7)`
  - Expressões de comparação é combinado com `and` (associações) podem ser combinadas ainda mais usando `or`. Esse formulário é conhecido em lógica booleana como "[formulário de Normal disjuntiva](https://en.wikipedia.org/wiki/Disjunctive_normal_form)" (DNF). Por exemplo:
    - `ratings/any(r: (r gt 2 and r le 5) or (r gt 7 and r lt 10))`
- Regras para `all`:
  - Expressões de igualdade simples utilmente não podem ser combinadas com as outras expressões. Por exemplo, essa expressão é permitida:
    - `ratings/all(r: r eq 5)`

    mas não é esta expressão:
    - `ratings/all(r: r eq 5 or r le 2)`

    e, embora essa expressão é permitida, não é útil porque as condições estão sobrepostos:
    - `ratings/all(r: r eq 5 and r le 7)`
  - Expressões de comparação simples envolva `ne`, `lt`, `le`, `gt`, ou `ge` pode ser combinado com `and` / `or`. Por exemplo:
    - `ratings/all(r: r gt 2 and r le 5)`
    - `ratings/all(r: r le 5 or r gt 7)`
  - Expressões de comparação é combinado com `or` (desassociações) podem ser combinadas ainda mais usando `and`. Esse formulário é conhecido em lógica booleana como "[formulário de Normal Conjuntiva](https://en.wikipedia.org/wiki/Conjunctive_normal_form)" (CNF). Por exemplo:
    - `ratings/all(r: (r le 2 or gt 5) and (r lt 7 or r ge 10))`

<a name="bkmk_complex"></a>

## <a name="rules-for-filtering-complex-collections"></a>Regras de filtragem de coleções complexas

As expressões lambda em coleções complexas dão suporte a uma sintaxe muito mais flexível do que as expressões lambda em coleções de tipos primitivos. Você pode usar qualquer constructo de filtro dentro de uma expressão lambda que pode ser usado fora de um, com apenas duas exceções.

Primeiro, as funções `search.ismatch` e `search.ismatchscoring` não têm suporte dentro de expressões lambda. Para obter mais informações, consulte [filtros de coleção de Noções básicas sobre OData no Azure Search](search-query-understand-collection-filters.md).

Em segundo lugar, fazendo referência a campos que não sejam *acoplado* à variável de intervalo (chamados *liberar variáveis*) não é permitido. Por exemplo, considere as seguintes dois equivalentes expressões de filtro OData:

1. `stores/any(s: s/amenities/any(a: a eq 'parking')) and details/margin gt 0.5`
1. `stores/any(s: s/amenities/any(a: a eq 'parking' and details/margin gt 0.5))`

A primeira expressão será permitida, enquanto o segundo formulário será rejeitado porque `details/margin` não está associada à variável de intervalo `s`.

Essa regra também se estende para expressões que têm variáveis associadas em um escopo externo. Essas variáveis são gratuitos em relação ao escopo em que aparecem. Por exemplo, a primeira expressão é permitida, enquanto o segundo equivalentes à expressão não não permitida porque `s/name` é gratuito em relação ao escopo da variável de intervalo `a`:

1. `stores/any(s: s/amenities/any(a: a eq 'parking') and s/name ne 'Flagship')`
1. `stores/any(s: s/amenities/any(a: a eq 'parking' and s/name ne 'Flagship'))`

Essa limitação não deve ser um problema na prática, pois ele sempre é possível construir filtros, de modo que as expressões lambda contêm apenas as variáveis associadas.

## <a name="cheat-sheet-for-collection-filter-rules"></a>Folha de consulta para as regras de filtro de coleção

A tabela a seguir resume as regras para a criação de filtros válidos para cada tipo de dados de coleção.

[!INCLUDE [Limitations on OData lambda expressions in Azure Search](../../includes/search-query-odata-lambda-limitations.md)]

Para obter exemplos de como construir filtros válidos para cada caso, consulte [como válido de coleção de filtros de gravação](#bkmk_examples).

Se você filtros de gravação com frequência e Noções básicas sobre as regras de princípios ajudariam a mais do que apenas memorizá-los, consulte [filtros de coleção de Noções básicas sobre OData no Azure Search](search-query-understand-collection-filters.md).

## <a name="next-steps"></a>Próximas etapas  

- [Noções básicas sobre filtros de coleção OData no Azure Search](search-query-understand-collection-filters.md)
- [Filtros no Azure Search](search-filters.md)
- [Visão geral de linguagem de expressão do OData para o Azure Search](query-odata-filter-orderby-syntax.md)
- [Referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md)
- [Pesquisar documentos &#40;API REST do Serviço do Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
