---
title: Referência de operador de comparação OData - Azure Search
description: Operadores de comparação de OData, eq, ne, gt, lt, ge e le, em consultas de pesquisa do Azure.
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
ms.openlocfilehash: b51bf3d77283ae828f47fdb0355d2deb43f071a1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079919"
---
# <a name="odata-comparison-operators-in-azure-search---eq-ne-gt-lt-ge-and-le"></a>Operadores de comparação de OData no Azure Search - `eq`, `ne`, `gt`, `lt`, `ge`, e `le`

A operação mais básica em um [expressão de filtro OData](query-odata-filter-orderby-syntax.md) no Azure Search é comparar um campo para um determinado valor. Dois tipos de comparação são possíveis – comparação de igualdade e comparação de intervalo. Você pode usar os seguintes operadores para comparar um campo para um valor constante:

Operadores de igualdade:

- `eq`: Testar se é um campo **igual a** um valor constante
- `ne`: Testar se é um campo **não é igual a** um valor constante

Operadores de intervalo:

- `gt`: Testar se é um campo **maior que** um valor constante
- `lt`: Testar se é um campo **menor que** um valor constante
- `ge`: Testar se é um campo **maior que ou igual a** um valor constante
- `le`: Testar se é um campo **menor que ou igual a** um valor constante

Você pode usar os operadores de intervalo em combinação com o [operadores lógicos](search-query-odata-logical-operators.md) para testar se um campo está em um determinado intervalo de valores. Consulte a [exemplos](#examples) mais adiante neste artigo.

> [!NOTE]
> Se você preferir, você pode colocar o valor da constante no lado esquerdo do operador e o nome do campo no lado direito. Operadores de intervalo, o significado da comparação é revertido. Por exemplo, se o valor da constante é à esquerda, `gt` testa se o valor da constante é maior que o campo. Você também pode usar os operadores de comparação para comparar o resultado de uma função, tais como `geo.distance`, com um valor. Para Boolean funções como `search.ismatch`, comparando o resultado `true` ou `false` é opcional.

## <a name="syntax"></a>Sintaxe

A seguir EBNF ([Extended Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) define a gramática de uma expressão de OData que usa os operadores de comparação.

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
comparison_expression ::=
    variable_or_function comparison_operator constant |
    constant comparison_operator variable_or_function

variable_or_function ::= variable | function_call

comparison_operator ::= 'gt' | 'lt' | 'ge' | 'le' | 'eq' | 'ne'
```

Um diagrama de sintaxe interativa também está disponível:

> [!div class="nextstepaction"]
> [Diagrama de sintaxe do OData para o Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#comparison_expression)

> [!NOTE]
> Ver [referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md) para o EBNF completa.

Há duas formas de expressões de comparação. A única diferença entre eles é se a constante é exibido na esquerda – ou direita lado do operador. A expressão do outro lado do operador deve ser um **variável** ou uma chamada de função. Uma variável pode ser um nome de campo ou uma variável de intervalo no caso de uma [expressão lambda](search-query-odata-collection-operators.md).

## <a name="data-types-for-comparisons"></a>Tipos de dados para comparações

Os tipos de dados em ambos os lados do operador de comparação devem ser compatíveis. Por exemplo, se o lado esquerdo é um campo do tipo `Edm.DateTimeOffset`, em seguida, à direita deve ser uma constante de data e hora. Tipos de dados numéricos são mais flexíveis. Você pode comparar as variáveis e funções de qualquer tipo numérico com constantes de qualquer outro tipo numérico, com algumas limitações, conforme descrito na tabela a seguir.

| Tipo de variável ou função | Tipo de valor constante | Limitações |
| --- | --- | --- |
| `Edm.Double` | `Edm.Double` | Comparação está sujeito à [especial de regras para `NaN`](#special-case-nan) |
| `Edm.Double` | `Edm.Int64` | Constante é convertida em `Edm.Double`, resultando em uma perda de precisão para valores de grande magnitude |
| `Edm.Double` | `Edm.Int32` | n/d |
| `Edm.Int64` | `Edm.Double` | As comparações `NaN`, `-INF`, ou `INF` não são permitidos |
| `Edm.Int64` | `Edm.Int64` | n/d |
| `Edm.Int64` | `Edm.Int32` | Constante é convertida em `Edm.Int64` antes da comparação |
| `Edm.Int32` | `Edm.Double` | As comparações `NaN`, `-INF`, ou `INF` não são permitidos |
| `Edm.Int32` | `Edm.Int64` | n/d |
| `Edm.Int32` | `Edm.Int32` | n/d |

Para comparações que não são permitidas, como um campo do tipo de comparação `Edm.Int64` para `NaN`, a API REST do Azure Search retornará um "HTTP 400: Erro de solicitação incorreta".

> [!IMPORTANT]
> Mesmo que as comparações de tipo numérico são flexíveis, é altamente recomendável escrevendo comparações em filtros para que o valor da constante é do mesmo tipo de dados como a variável ou função ao qual ele está sendo comparado. Isso é especialmente importante quando uma combinação de ponto flutuante e os valores inteiros, onde as conversões implícitas que perder a precisão são possíveis.

<a name="special-case-nan"></a>

### <a name="special-cases-for-null-and-nan"></a>Casos de especial para `null` e `NaN`

Ao usar operadores de comparação, é importante lembrar-se de que todos os campos não seja de coleção no Azure Search podem ser potencialmente `null`. A tabela a seguir mostra todos os possíveis resultados de uma expressão de comparação em que ambos os lados podem ser `null`:

| Operador | Resultado quando é apenas o campo ou variável `null` | Resultado quando é apenas a constante `null` | Resultado quando o campo ou variável e a constante são `null` |
| --- | --- | --- | --- |
| `gt` | `false` | HTTP 400: Erro de solicitação inválido | HTTP 400: Erro de solicitação inválido |
| `lt` | `false` | HTTP 400: Erro de solicitação inválido | HTTP 400: Erro de solicitação inválido |
| `ge` | `false` | HTTP 400: Erro de solicitação inválido | HTTP 400: Erro de solicitação inválido |
| `le` | `false` | HTTP 400: Erro de solicitação inválido | HTTP 400: Erro de solicitação inválido |
| `eq` | `false` | `false` | `true` |
| `ne` | `true` | `true` | `false` |

Em resumo, `null` é igual somente a mesmo e é não menor que ou maior do que qualquer outro valor.

Se o índice tem campos do tipo `Edm.Double` e você carregar `NaN` valores para esses campos, você precisará considerar que ao escrever filtros. O Azure Search implementa o padrão IEEE 754 para manipulação `NaN` valores e as comparações com valores tão produzem resultados não óbvias, como mostrado na tabela a seguir.

| Operador | Resultado quando pelo menos um operando é `NaN` |
| --- | --- |
| `gt` | `false` |
| `lt` | `false` |
| `ge` | `false` |
| `le` | `false` |
| `eq` | `false` |
| `ne` | `true` |

Em resumo, `NaN` não é igual a qualquer valor, incluindo ele próprio.

### <a name="comparing-geo-spatial-data"></a>Comparando dados geoespaciais

Você não pode comparar diretamente um campo do tipo `Edm.GeographyPoint` com um valor constante, mas você pode usar o `geo.distance` função. Essa função retorna um valor do tipo `Edm.Double`, portanto, você pode compará-lo com um numérico constante para filtrar com base na distância das coordenadas geoespaciais constante. Consulte a [exemplos](#examples) abaixo.

### <a name="comparing-string-data"></a>Comparando dados de cadeia de caracteres

Cadeias de caracteres podem ser comparadas em filtros para correspondências exatas usando o `eq` e `ne` operadores. Essas comparações diferenciam maiusculas de minúsculas.

## <a name="examples"></a>Exemplos

Correspondência de documentos em que o `Rating` campo está entre 3 e 5, inclusive:

    Rating ge 3 and Rating le 5

Correspondência de documentos em que o `Location` campo é menor que 2 km da latitude e longitude:

    geo.distance(Location, geography'POINT(-122.031577 47.578581)') lt 2.0

Correspondência de documentos em que o `LastRenovationDate` campo é maior que ou igual a 1º de janeiro de 2015, meia-noite UTC:

    LastRenovationDate ge 2015-01-01T00:00:00.000Z

Correspondência de documentos em que o `Details/Sku` Pole není `null`:

    Details/Sku ne null

Corresponde a documentos para hotéis em que pelo menos uma sala tem tipo de "Sala Deluxe", em que a cadeia de caracteres da `Rooms/Type` campo corresponde ao filtro exatamente:

    Rooms/any(room: room/Type eq 'Deluxe Room')

## <a name="next-steps"></a>Próximas etapas  

- [Filtros no Azure Search](search-filters.md)
- [Visão geral de linguagem de expressão do OData para o Azure Search](query-odata-filter-orderby-syntax.md)
- [Referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md)
- [Pesquisar documentos &#40;API REST do Serviço do Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
