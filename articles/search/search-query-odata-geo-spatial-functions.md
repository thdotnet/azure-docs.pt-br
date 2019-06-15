---
title: Referência de função geoespaciais do OData - Azure Search
description: OData funções geoespaciais, geo e intersects em consultas de pesquisa do Azure.
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
ms.openlocfilehash: 0ce63ab1143c784eb3e10f47c20ef2b5034d63a7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079789"
---
# <a name="odata-geo-spatial-functions-in-azure-search---geodistance-and-geointersects"></a>Funções geoespaciais do OData no Azure Search - `geo.distance` e `geo.intersects`

O Azure Search dá suporte a consultas geoespaciais no [expressões de filtro OData](query-odata-filter-orderby-syntax.md) por meio de `geo.distance` e `geo.intersects` funções. O `geo.distance` função retorna a distância em quilômetros entre dois pontos, sendo um campo ou variável de intervalo e é uma constante passada como parte do filtro. O `geo.intersects` retornos de função `true` se um determinado ponto estiver dentro de um polígono determinado, onde o ponto é uma campo ou variável de intervalo e o polígono é especificado como uma constante passada como parte do filtro.

O `geo.distance` função também pode ser usada em de [ **$orderby** parâmetro](search-query-odata-orderby.md) para classificar os resultados por distância de um determinado ponto. A sintaxe para `geo.distance` em **$orderby** é a mesma que em **$filter**. Ao usar `geo.distance` na **$orderby**, o campo ao qual se aplica deve ser do tipo `Edm.GeographyPoint` e também deve ser **classificável**.

## <a name="syntax"></a>Sintaxe

A seguir EBNF ([Extended Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)) define a gramática da `geo.distance` e `geo.intersects` funções, bem como os valores geoespaciais nos quais eles operam:

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
geo_distance_call ::=
    'geo.distance(' variable ',' geo_point ')'
    | 'geo.distance(' geo_point ',' variable ')'

geo_point ::= "geography'POINT(" lon_lat ")'"

lon_lat ::= float_literal ' ' float_literal

geo_intersects_call ::=
    'geo.intersects(' variable ',' geo_polygon ')'

/* You need at least four points to form a polygon, where the first and
last points are the same. */
geo_polygon ::=
    "geography'POLYGON((" lon_lat ',' lon_lat ',' lon_lat ',' lon_lat_list "))'"

lon_lat_list ::= lon_lat(',' lon_lat)*
```

Um diagrama de sintaxe interativa também está disponível:

> [!div class="nextstepaction"]
> [Diagrama de sintaxe do OData para o Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#geo_distance_call)

> [!NOTE]
> Ver [referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md) para o EBNF completa.

### <a name="geodistance"></a>Geo

O `geo.distance` função utiliza dois parâmetros de tipo `Edm.GeographyPoint` e retorna um `Edm.Double` valor que é a distância entre eles em quilômetros. Isso é diferente de outros serviços que dão suporte a operações geoespaciais do OData, que normalmente retornam distâncias em metros.

Um dos parâmetros para `geo.distance` deve ser uma constante de ponto de Geografia e o outro deve ser um caminho de campo (ou uma variável de intervalo no caso de um filtro de iteração em um campo do tipo `Collection(Edm.GeographyPoint)`). Não importa a ordem desses parâmetros.

A constante de ponto de Geografia é da forma `geography'POINT(<longitude> <latitude>)'`, onde a longitude e latitude são constantes numéricas.

> [!NOTE]
> Ao usar `geo.distance` em um filtro, você deve comparar a distância retornada pela função com uma constante usando `lt`, `le`, `gt`, ou `ge`. Os operadores `eq` e `ne` não são compatíveis ao comparar as distâncias. Por exemplo, este é um uso correto dos `geo.distance`: `$filter=geo.distance(location, geography'POINT(-122.131577 47.678581)') le 5`.

### <a name="geointersects"></a>geo.intersects

O `geo.intersects` função usa uma variável do tipo `Edm.GeographyPoint` e uma constante `Edm.GeographyPolygon` e retorna um `Edm.Boolean`  --  `true` se o ponto está dentro dos limites do polígono, `false` caso contrário.

O polígono é uma superfície bidimensional armazenada como uma sequência de pontos que define um anel delimitador (consulte a [exemplos](#examples) abaixo). O polígono precisa estar fechado, o que significa que os conjuntos de primeiro e último pontos devem ser os mesmos. [Os pontos em um polígono devem estar no sentido anti-horário](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

### <a name="geo-spatial-queries-and-polygons-spanning-the-180th-meridian"></a>Consultas geoespaciais e polígonos abrangência do Meridiano 180th

Para consultas geoespaciais muitas bibliotecas formular uma consulta que inclui o Meridiano 180th (perto do Meridiano de data) está fora dos limites ou exige uma solução alternativa, como dividir o polígono em dois, um em ambos os lados do Meridiano.

No Azure Search, as consultas geoespaciais que incluem 180 graus de longitude funcionará conforme o esperado se a forma de consulta é retangular e suas coordenadas alinham a um layout de grade ao longo de longitude e latitude (por exemplo, `geo.intersects(location, geography'POLYGON((179 65, 179 66, -179 66, -179 65, 179 65))'`). Caso contrário, para formas não retangulares ou desalinhadas, considere a abordagem de polígono dividido.  

### <a name="geo-spatial-functions-and-null"></a>Funções geoespaciais e `null`

Como todos os outros campos não seja de coleção no Azure Search, os campos do tipo `Edm.GeographyPoint` pode conter `null` valores. Quando o Azure Search avalia `geo.intersects` para um campo que está `null`, o resultado será sempre `false`. O comportamento de `geo.distance` nesse caso depende do contexto:

- Em filtros, `geo.distance` de um `null` campo resulta em `null`. Isso significa que o documento não corresponderão porque `null` valor comparado como qualquer não nulo é avaliada como `false`.
- Ao classificar os resultados usando **$orderby**, `geo.distance` de um `null` resultados na distância máxima possível de campo. Classificação de documentos com esse campo serão menor do que todos os outros quando a direção de classificação `asc` é usado (padrão) e maior do que todas as outras pessoas quando a direção é `desc`.

## <a name="examples"></a>Exemplos

### <a name="filter-examples"></a>Exemplos de filtro

Localize todos os hotéis dentro de 10 km de um ponto de referência fornecido (em que local é um campo do tipo `Edm.GeographyPoint`):

    geo.distance(location, geography'POINT(-122.131577 47.678581)') le 10

Localize todos os hotéis dentro de um viewport determinado descrito como um polígono (onde o local é um campo do tipo `Edm.GeographyPoint`). Observe que o polígono está fechado (o primeiro e o último pontos devem ser o mesmo) e [os pontos devem estar listados no sentido anti-horário](https://docs.microsoft.com/rest/api/searchservice/supported-data-types#Anchor_1).

    geo.intersects(location, geography'POLYGON((-122.031577 47.578581, -122.031577 47.678581, -122.131577 47.678581, -122.031577 47.578581))')

### <a name="order-by-examples"></a>Exemplos de Order-by

Classificar em ordem decrescente por hotéis `rating`, em seguida, em ordem crescente por distância das coordenadas determinadas:

    rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

Classificar hotéis em ordem decrescente pela `search.score` e `rating`e, em seguida, em ordem crescente pela distância de coordenadas especificadas para que entre dois hotéis com classificações idênticas, mais próximo é listado primeiro:

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Próximas etapas  

- [Filtros no Azure Search](search-filters.md)
- [Visão geral de linguagem de expressão do OData para o Azure Search](query-odata-filter-orderby-syntax.md)
- [Referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md)
- [Pesquisar documentos &#40;API REST do Serviço do Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
