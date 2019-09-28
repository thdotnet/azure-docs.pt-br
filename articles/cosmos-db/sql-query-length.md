---
title: COMPRIMENTO na linguagem de consulta Azure Cosmos DB
description: Saiba mais sobre o comprimento da função do sistema SQL no Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: aa430152415b1662a73a388a03ba6d4721c730f0
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71349760"
---
# <a name="length-azure-cosmos-db"></a>COMPRIMENTO (Azure Cosmos DB)
 Retorna o número de caracteres da expressão de cadeia de caracteres especificada.  
  
## <a name="syntax"></a>Sintaxe
  
```sql
LENGTH(<str_expr>)  
```  
  
## <a name="arguments"></a>Argumentos
  
*str_expr*  
   É a expressão de cadeia de caracteres a ser avaliada.  
  
## <a name="return-types"></a>Tipos de retorno
  
  Retorna uma expressão numérica.  
  
## <a name="examples"></a>Exemplos
  
  O exemplo a seguir retorna o comprimento de uma cadeia de caracteres.  
  
```sql
SELECT LENGTH("abc") AS len 
```  
  
 Este é o conjunto de resultados.  
  
```json
[{"len": 3}]  
```  

## <a name="next-steps"></a>Próximas etapas

- [Funções de cadeia de caracteres Azure Cosmos DB](sql-query-string-functions.md)
- [Funções do sistema Azure Cosmos DB](sql-query-system-functions.md)
- [Introdução ao Azure Cosmos DB](introduction.md)
