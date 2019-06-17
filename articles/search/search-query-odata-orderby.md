---
title: Classificar por referência de OData - Azure Search
description: Referência de linguagem do OData para a sintaxe de ordem em consultas de pesquisa do Azure.
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
ms.openlocfilehash: 1ced35dc73e6d596fbeda32590ab0b69df396c5c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079750"
---
# <a name="odata-orderby-syntax-in-azure-search"></a>Sintaxe de OData $orderby no Azure Search

 Você pode usar o [OData **$orderby** parâmetro](query-odata-filter-orderby-syntax.md) para aplicar uma ordem de classificação personalizada para resultados da pesquisa no Azure Search. Este artigo descreve a sintaxe da **$orderby** em detalhes. Para obter mais informações sobre como usar **$orderby** ao apresentar os resultados da pesquisa, consulte [como trabalhar com a pesquisa resulta no Azure Search](search-pagination-page-layout.md).

## <a name="syntax"></a>Sintaxe

O **$orderby** parâmetro aceita uma lista separada por vírgulas de até 32 **cláusulas order by**. A sintaxe da cláusula order by é descrita pela seguir EBNF ([Extended Backus-Naur Form](https://en.wikipedia.org/wiki/Extended_Backus–Naur_form)):

<!-- Upload this EBNF using https://bottlecaps.de/rr/ui to create a downloadable railroad diagram. -->

```
order_by_clause ::= (field_path | sortable_function) ('asc' | 'desc')?

sortable_function ::= geo_distance_call | 'search.score()'
```

Um diagrama de sintaxe interativa também está disponível:

> [!div class="nextstepaction"]
> [Diagrama de sintaxe do OData para o Azure Search](https://azuresearch.github.io/odata-syntax-diagram/#order_by_clause)

> [!NOTE]
> Ver [referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md) para o EBNF completa.

Cada cláusula tem critérios de classificação, opcionalmente seguidos por uma direção de classificação (`asc` crescente ou `desc` para decrescente). Se você não especificar uma direção, o padrão é crescente. Os critérios de classificação podem ser o caminho de um `sortable` campo ou uma chamada para o [ `geo.distance` ](search-query-odata-geo-spatial-functions.md) ou o [ `search.score` ](search-query-odata-search-score-function.md) funções.

Se vários documentos têm os mesmos critérios de classificação e o `search.score` função não é usada (por exemplo, se você classificar por um numero `Rating` campo e três documentos têm uma classificação de 4), os empates serão resolvidos pela pontuação de documento em ordem decrescente. Quando as pontuações do documento são as mesmas (por exemplo, quando não há nenhuma consulta de pesquisa de texto completo especificada na solicitação), em seguida, a ordenação relativa dos documentos empatados é indeterminada.

Você pode especificar vários critérios de classificação. A ordem das expressões determina a ordem de classificação final. Por exemplo, para classificar em ordem decrescente pela pontuação, seguida por classificação, a sintaxe seria `$orderby=search.score() desc,Rating desc`.

A sintaxe para `geo.distance` em **$orderby** é a mesma que em **$filter**. Ao usar `geo.distance` em **$orderby**, o campo ao qual a operação se aplica deve ser do tipo `Edm.GeographyPoint` e também deve ser `sortable`.

A sintaxe para `search.score` em **$orderby** é `search.score()`. A função `search.score` não usa parâmetros.

## <a name="examples"></a>Exemplos

Organizar os hotéis em ordem crescente por taxa base:

    $orderby=BaseRate asc

Organizar os hotéis em ordem decrescente por classificação e, em seguida, em ordem crescente por taxa base (lembre-se de que a ordem crescente é o padrão):

    $orderby=Rating desc,BaseRate

Classificar em ordem decrescente por classificação, em seguida, em ordem crescente por distância das coordenadas determinadas de hotéis:

    $orderby=Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

Classificar hotéis em ordem decrescente pela search.score e a classificação e, em seguida, em ordem crescente pela distância de coordenadas especificadas. Entre dois hotéis com pontuações de relevância idênticos e classificações, mais próximo é listado primeiro:

    $orderby=search.score() desc,Rating desc,geo.distance(Location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Próximas etapas  

- [Como trabalhar com a pesquisa resulta no Azure Search](search-pagination-page-layout.md)
- [Visão geral de linguagem de expressão do OData para o Azure Search](query-odata-filter-orderby-syntax.md)
- [Referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md)
- [Pesquisar documentos &#40;API REST do Serviço do Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
