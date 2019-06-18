---
author: brjohnstmsft
ms.service: search
ms.topic: include
ms.date: 06/13/2018
ms.author: brjohnst
ms.openlocfilehash: b4d0bc73e652a1cd6ef59589318b92f63727c7f5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079633"
---
| Tipo de dados | Recursos permitidos em expressões lambda com `any` | Recursos permitidos em expressões lambda com `all` |
|---|---|---|
| `Collection(Edm.ComplexType)` | Tudo, exceto `search.ismatch` e `search.ismatchscoring` | Idêntico |
| `Collection(Edm.String)` | As comparações com `eq` ou `search.in` <br/><br/> Combinando subexpressões com `or` | As comparações com `ne` ou `not search.in()` <br/><br/> Combinando subexpressões com `and` |
| `Collection(Edm.Boolean)` | As comparações com `eq` ou `ne` | Idêntico |
| `Collection(Edm.GeographyPoint)` | Usando o `geo.distance` com `lt` ou `le` <br/><br/> `geo.intersects` <br/><br/> Combinando subexpressões com `or` | Usando o `geo.distance` com `gt` ou `ge` <br/><br/> `not geo.intersects(...)` <br/><br/> Combinando subexpressões com `and` |
| `Collection(Edm.DateTimeOffset)`, `Collection(Edm.Double)`, `Collection(Edm.Int32)`, `Collection(Edm.Int64)` | As comparações usando `eq`, `ne`, `lt`, `gt`, `le`, ou `ge` <br/><br/> Combinando as comparações com outros subexpressões usando `or` <br/><br/> Combinando as comparações, exceto `ne` com outras expressões subpropriedades usando `and` <br/><br/> Expressões que usam as combinações de `and` e `or` em [disjuntiva Normal formulário (DNF)](https://en.wikipedia.org/wiki/Disjunctive_normal_form) | As comparações usando `eq`, `ne`, `lt`, `gt`, `le`, ou `ge` <br/><br/> Combinando as comparações com outros subexpressões usando `and` <br/><br/> Combinando as comparações, exceto `eq` com outras expressões subpropriedades usando `or` <br/><br/> Expressões que usam as combinações de `and` e `or` em [Conjuntiva Normal formulário (CNF)](https://en.wikipedia.org/wiki/Conjunctive_normal_form) |
