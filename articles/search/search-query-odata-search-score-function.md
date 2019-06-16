---
title: Referência de função do OData search.score - Azure Search
description: Função search.score do OData em consultas de pesquisa do Azure.
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
ms.openlocfilehash: dc444216c4677b9970b867e92aa5ae259a197220
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079685"
---
# <a name="odata-searchscore-function-in-azure-search"></a>OData `search.score` função no Azure Search

Quando você envia uma consulta para o Azure Search sem o [ **$orderby** parâmetro](search-query-odata-orderby.md), os resultados retornados serão classificados em ordem decrescente pela pontuação de relevância. Mesmo quando você usa **$orderby**, a pontuação de relevância será usada para desempatar. No entanto, às vezes é útil usar a pontuação de relevância como um critério de classificação inicial e alguns outros critérios, como o desempate. O `search.score` função permite que você faça isso.

## <a name="syntax"></a>Sintaxe

A sintaxe para `search.score` em **$orderby** é `search.score()`. A função `search.score` não aceita nenhum parâmetro. Ele pode ser usado com o `asc` ou `desc` especificador de ordem de classificação, assim como qualquer outra cláusula na **$orderby** parâmetro. Ele pode aparecer em qualquer lugar na lista de critérios de classificação.

## <a name="example"></a>Exemplo

Classificar hotéis em ordem decrescente pela `search.score` e `rating`e, em seguida, em ordem crescente pela distância de coordenadas especificadas para que entre dois hotéis com classificações idênticas, mais próximo é listado primeiro:

    search.score() desc,rating desc,geo.distance(location, geography'POINT(-122.131577 47.678581)') asc

## <a name="next-steps"></a>Próximas etapas  

- [Visão geral de linguagem de expressão do OData para o Azure Search](query-odata-filter-orderby-syntax.md)
- [Referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md)
- [Pesquisar documentos &#40;API REST do Serviço do Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
