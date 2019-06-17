---
title: Noções básicas sobre filtros de coleção OData - Azure Search
description: Entender como funcionam os filtros de coleção OData em consultas de pesquisa do Azure.
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
ms.openlocfilehash: 7af1b0ab95d04249d6d74e3324dbeb30eda6e1de
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079594"
---
# <a name="understanding-odata-collection-filters-in-azure-search"></a>Noções básicas sobre filtros de coleção OData no Azure Search

Para [filtro](query-odata-filter-orderby-syntax.md) nos campos da coleção no Azure Search, você pode usar o [ `any` e `all` operadores](search-query-odata-collection-operators.md) junto com **expressões lambda**. As expressões lambda são expressões Boolianas que se referem a um **variável de intervalo**. O `any` e `all` operadores são análogos a um `for` loop na maioria das linguagens de programação, com a variável de intervalo, levando a função de variável de loop e a expressão lambda como o corpo do loop. A variável de intervalo assume o valor "atual" da coleção durante a iteração do loop.

Pelo menos essa como ele funciona conceitualmente. Na realidade, o Azure Search implementa filtros em uma maneira muito diferente como `for` faz um loop de trabalho. Idealmente, essa diferença seria invisível para você, mas em determinadas situações não é. O resultado final é que há regras que você precise seguir ao escrever expressões lambda.

Este artigo explica por que as regras de filtros de coleção existem ao explorar como o Azure Search executa esses filtros. Se você estiver escrevendo filtros avançados com expressões lambda complexas, talvez este artigo útil na criação de sua compreensão do que é possível em filtros e por quê.

Para obter informações sobre o que as regras para filtros de coleção são, incluindo exemplos, consulte [filtros de coleção de solução de problemas de OData no Azure Search](search-query-troubleshoot-collection-filters.md).

## <a name="why-collection-filters-are-limited"></a>Por que os filtros de coleção são limitados

Há três motivos subjacentes por que não há suporte para todos os tipos de coleções de todos os recursos de filtro:

1. Somente determinados operadores têm suporte para determinados tipos de dados. Por exemplo, não faz sentido para comparar os valores boolianos `true` e `false` usando `lt`, `gt`e assim por diante.
1. Não dá suporte a Azure Search **pesquisa correlacionada** em campos do tipo `Collection(Edm.ComplexType)`.
1. O Azure usa pesquisa invertida índices para executar filtros sobre todos os tipos de dados, incluindo coleções.

O primeiro motivo é apenas uma consequência de como o idioma de OData e o sistema de tipo EDM são definidos. As duas últimas são explicadas em mais detalhes no restante deste artigo.

## <a name="correlated-versus-uncorrelated-search"></a>Correlacionadas em comparação com a pesquisa não correlacionada

Ao aplicar vários critérios de filtro em uma coleção de objetos complexos, os critérios são **correlacionado** uma vez que elas se aplicam aos *cada objeto na coleção*. Por exemplo, o filtro a seguir retornará os hotéis que têm pelo menos uma sala deluxe com uma taxa menor que 100:

    Rooms/any(room: room/Type eq 'Deluxe Room' and room/BaseRate lt 100)

Se a filtragem foi *não correlacionadas*, o filtro acima pode retornar hotéis onde uma sala é deluxe e uma sala diferente tem uma base de taxa menor do que 100. Isso não faz sentido, já que ambas as cláusulas de expressão lambda se aplicam à variável de intervalo, ou seja, `room`. É por isso esses filtros são correlacionados.

No entanto, para pesquisa de texto completo, não há nenhuma maneira de se referir a uma variável de intervalo específico. Se você usar a pesquisa por campo para emitir uma [consulta do Lucene completa](query-lucene-syntax.md) como este:

    Rooms/Type:deluxe AND Rooms/Description:"city view"

Você pode receber hotéis volta onde uma sala deluxe, e uma sala diferente menciona "exibição de cidade" na descrição. Por exemplo, o documento abaixo com `Id` de `1` corresponderia a consulta:

```json
{
  "value": [
    {
      "Id": "1",
      "Rooms": [
        { "Type": "deluxe", "Description": "Large garden view suite" },
        { "Type": "standard", "Description": "Standard city view room" }
      ]
    },
    {
      "Id": "2",
      "Rooms": [
        { "Type": "deluxe", "Description": "Courtyard motel room" }
      ]
    }
  ]
}
```

A razão é que `Rooms/Type` refere-se a todos os termos analisados do `Rooms/Type` campo em todo o documento e o mesmo `Rooms/Description`, conforme mostrado nas tabelas a seguir.

Como `Rooms/Type` é armazenado para pesquisa de texto completo:

| Termo no `Rooms/Type` | IDs de documento |
| --- | --- |
| Deluxe | 1, 2 |
| padrão | 1 |

Como `Rooms/Description` é armazenado para pesquisa de texto completo:

| Termo no `Rooms/Description` | IDs de documento |
| --- | --- |
| Pátio | 2 |
| city | 1 |
| jardim | 1 |
| Grande | 1 |
| motel | 2 |
| Sala | 1, 2 |
| padrão | 1 |
| Suite | 1 |
| view | 1 |

Portanto, ao contrário de filtro, que basicamente diz "corresponde a documentos em que tem uma sala `Type` igual a 'Deluxe sala' e **essa mesma sala** tem `BaseRate` menos de 100", diz que a consulta de pesquisa "correspondência de documentos em que `Rooms/Type`tem o termo "luxo" e `Rooms/Description` tem a frase "exibição de cidade". Não há nenhum conceito de salas individuais cujos campos possam ser correlacionados no último caso.

> [!NOTE]
> Se você quiser oferecer suporte para pesquisa correlacionada adicionada ao Azure Search, vote [este item de voz do usuário](https://feedback.azure.com/forums/263029-azure-search/suggestions/37735060-support-correlated-search-on-complex-collections).

## <a name="inverted-indexes-and-collections"></a>Coleções e índices invertidos

Você talvez tenha observado que há muito menos restrições em expressões lambda em coleções complexas do que há para coleções simples, como `Collection(Edm.Int32)`, `Collection(Edm.GeographyPoint)`e assim por diante. Isso ocorre porque o Azure Search armazena coleções complexas como coleções reais de sub-documentos, enquanto coleções simples não são armazenadas como coleções.

Por exemplo, considere um campo de coleção de cadeia de caracteres que podem ser filtradas como `seasons` em um índice para uma loja online. Alguns documentos carregados para esse índice podem ter esta aparência:

```json
{
  "value": [
    {
      "id": "1",
      "name": "Hiking boots",
      "seasons": ["spring", "summer", "fall"]
    },
    {
      "id": "2",
      "name": "Rain jacket",
      "seasons": ["spring", "fall", "winter"]
    },
    {
      "id": "3",
      "name": "Parka",
      "seasons": ["winter"]
    }
  ]
}
```

Os valores da `seasons` campo são armazenados em uma estrutura chamada um **índice invertido**, que pode ter esta aparência:

| Termo | IDs de documento |
| --- | --- |
| Spring | 1, 2 |
| verão | 1 |
| Outono | 1, 2 |
| inverno | 2, 3 |

Essa estrutura de dados foi projetada para responder a uma pergunta com grande velocidade: No qual os documentos que aparece a um determinado termo? Resposta a essa pergunta funciona mais como uma verificação de igualdade simples que um loop em uma coleção. Na verdade, isso é por isso para coleções de cadeia de caracteres, o Azure Search só permite `eq` como um operador de comparação dentro de uma expressão lambda para `any`.

Criar de igualdade, em seguida, examinaremos como é possível combinar várias verificações de igualdade na variável de intervalo com `or`. Ele funciona, graças a álgebra e [a propriedade distributiva de quantificadores](https://en.wikipedia.org/wiki/Existential_quantification#Negation). Esta expressão:

    seasons/any(s: s eq 'winter' or s eq 'fall')

é equivalente a:

    seasons/any(s: s eq 'winter') or seasons/any(s: s eq 'fall')

e cada uma das duas `any` subexpressões podem ser executadas com eficiência usando o índice invertido. Além disso, graças à [a lei de negação de quantificadores](https://en.wikipedia.org/wiki/Existential_quantification#Negation), esta expressão:

    seasons/all(s: s ne 'winter' and s ne 'fall')

é equivalente a:

    not seasons/any(s: s eq 'winter' or s eq 'fall')

Por isso, é possível usar `all` com `ne` e `and`.

> [!NOTE]
> Embora os detalhes estão além do escopo deste documento, esses mesmos princípios se estender para o [testes de distância e a interseção para coleções de pontos geoespaciais](search-query-odata-geo-spatial-functions.md) também. É por isso que, em `any`:
>
> - `geo.intersects` não pode ser negada
> - `geo.distance` devem ser comparados usando `lt` ou `le`
> - expressões devem ser combinadas com `or`, e não `and`
>
> O inverso de regras se aplicam a `all`.

Uma variedade maior de expressões são permitidas quando a filtragem em coleções de dados de tipos que oferecem suporte a `lt`, `gt`, `le`, e `ge` operadores, tais como `Collection(Edm.Int32)` por exemplo. Especificamente, você pode usar `and` , bem como `or` na `any`, desde que as expressões de comparação subjacentes são combinadas em **comparações de intervalo** usando `and`, que, em seguida, são ainda mais combinados usando `or`. Essa estrutura de expressões Boolianas é chamada [disjuntiva Normal formulário (DNF)](https://en.wikipedia.org/wiki/Disjunctive_normal_form), também conhecido como "ORs de ANDs". Por outro lado, as expressões lambda para `all` para esses dados de tipos devem estar no [Conjuntiva Normal formulário (CNF)](https://en.wikipedia.org/wiki/Conjunctive_normal_form), também conhecido como "ANDs de ORs". O Azure Search permite que essas comparações de intervalo porque ele pode executá-los usando invertida índices com eficiência, assim como ele pode fazer pesquisa de termos rápida para cadeias de caracteres.

Em resumo, aqui estão as regras básicas para o que é permitido em uma expressão lambda:

- Dentro de `any`, *verificações positivas* são sempre permitidos, como igualdade, comparações de intervalo `geo.intersects`, ou `geo.distance` em comparação com `lt` ou `le` (pense "proximidade" como se fosse Igualdade quando se trata de verificação de distância).
- Dentro de `any`, `or` sempre é permitido. Você pode usar `and` somente para tipos de dados que podem expressar as verificações de intervalo e somente se você usar ORs de ANDs (DNF).
- Dentro de `all`, as regras são revertidas – só *negativo verificações* são permitidos, você pode usar `and` sempre, e você pode usar `or` apenas para verificações de intervalo, expresso como ANDs de ORs (CNF).

Na prática, estes são os tipos de filtros que você tem mais probabilidade de qualquer forma de usar. Ele ainda é útil entender os limites do que é possível, no entanto.

Para obter exemplos específicos de quais tipos de filtros são permitidos e que não são, consulte [como válido de coleção de filtros de gravação](search-query-troubleshoot-collection-filters.md#bkmk_examples).

## <a name="next-steps"></a>Próximas etapas  

- [Solução de problemas de filtros de coleção OData no Azure Search](search-query-troubleshoot-collection-filters.md)
- [Filtros no Azure Search](search-filters.md)
- [Visão geral de linguagem de expressão do OData para o Azure Search](query-odata-filter-orderby-syntax.md)
- [Referência de sintaxe de expressão OData para o Azure Search](search-query-odata-syntax-reference.md)
- [Pesquisar documentos &#40;API REST do Serviço do Azure Search&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
