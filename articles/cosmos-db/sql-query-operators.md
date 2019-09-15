---
title: Operadores de consulta SQL para Azure Cosmos DB
description: Saiba mais sobre operadores SQL para Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: mjbrown
ms.openlocfilehash: 899355ad7331a3df8cd5d647a573dc15e3a0bb14
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/15/2019
ms.locfileid: "71003381"
---
# <a name="operators-in-azure-cosmos-db"></a>Operadores no Azure Cosmos DB

Este artigo fornece detalhes sobre os vários operadores com suporte pelo Azure Cosmos DB.

## <a name="equality-and-comparison-operators"></a>Operadores de igualdade e comparação

A tabela a seguir mostra o resultado de comparações de igualdade na API do SQL entre dois tipos JSON quaisquer.

| **Op** | **Indefinido** | **Nulo** | **Booliano** | **Número** | **Cadeia de caracteres** | **Object** | **Matriz** |
|---|---|---|---|---|---|---|---|
| **Indefinido** | Indefinido | Indefinido | Indefinido | Indefinido | Indefinido | Indefinido | Indefinido |
| **Nulo** | Indefinido | **Ok** | Indefinido | Indefinido | Indefinido | Indefinido | Indefinido |
| **Booliano** | Indefinido | Indefinido | **Ok** | Indefinido | Indefinido | Indefinido | Indefinido |
| **Número** | Indefinido | Indefinido | Indefinido | **Ok** | Indefinido | Indefinido | Indefinido |
| **Cadeia de caracteres** | Indefinido | Indefinido | Indefinido | Indefinido | **Ok** | Indefinido | Indefinido |
| **Object** | Indefinido | Indefinido | Indefinido | Indefinido | Indefinido | **Ok** | Indefinido |
| **Matriz** | Indefinido | Indefinido | Indefinido | Indefinido | Indefinido | Indefinido | **Ok** |

Para operadores de comparação como `>` `!=`, `>=` `<`,, e `<=`, a comparação entre os tipos ou entre dois objetos ou matrizes produz `Undefined`.  

Se o resultado da expressão escalar for `Undefined`, o item não será incluído no resultado, porque `Undefined` não é igual `true`a.

## <a name="logical-and-or-and-not-operators"></a>Operadores lógicos (AND, OR e NOT)

Operadores lógicos funcionam em valores boolianos. As tabelas a seguir mostram as tabelas lógicas da verdade para esses operadores:

**Operador OU**

| OU | verdadeiro | False | Indefinido |
| --- | --- | --- | --- |
| verdadeiro |verdadeiro |verdadeiro |verdadeiro |
| False |verdadeiro |False |Indefinido |
| Indefinido |verdadeiro |Indefinido |Indefinido |

**E o operador**

| E | verdadeiro | False | Indefinido |
| --- | --- | --- | --- |
| verdadeiro |verdadeiro |False |Indefinido |
| False |False |False |False |
| Indefinido |Indefinido |False |Indefinido |

**NÃO operador**

| NÃO |  |
| --- | --- |
| verdadeiro |False |
| False |verdadeiro |
| Indefinido |Indefinido |


## <a name="-operator"></a>* operador

O operador especial * projeta o item inteiro como está. Quando usado, ele deve ser o único campo projetado. Uma consulta como `SELECT * FROM Families f` é válida, `SELECT *, f.id FROM Families f` mas `SELECT VALUE * FROM Families f` não é válida.

## <a name="-and--operators"></a>? e?? Operações

Você pode usar os operadores ternário (?) e de União (??) para criar expressões condicionais, como em linguagens C# de programação como o e o JavaScript. 

Você pode usar o? operador para construir novas propriedades JSON em tempo real. Por exemplo, a consulta a seguir classifica os níveis `elementary` de `other`nível em ou:

```sql
     SELECT (c.grade < 5)? "elementary": "other" AS gradeLevel
     FROM Families.children[0] c
```

Você também pode aninhar chamadas para o? como na consulta a seguir: 

```sql
    SELECT (c.grade < 5)? "elementary": ((c.grade < 9)? "junior": "high") AS gradeLevel
    FROM Families.children[0] c
```

Como ocorre com outros operadores de consulta, o? o operador exclui itens se as propriedades referenciadas estiverem ausentes ou os tipos que estão sendo comparados forem diferentes.

Usar o?? para verificar com eficiência uma propriedade em um item ao consultar dados semiestruturados ou de tipo misto. Por exemplo, a consulta a seguir `lastName` retorna se presente, `surname` ou `lastName` se não estiver presente.

```sql
    SELECT f.lastName ?? f.surname AS familyName
    FROM Families f
```

## <a name="next-steps"></a>Próximas etapas

- [Amostras do .NET no Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Palavras-chave](sql-query-keywords.md)
- [Cláusula SELECT](sql-query-select.md)
