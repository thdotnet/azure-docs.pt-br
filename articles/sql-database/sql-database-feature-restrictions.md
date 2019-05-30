---
title: As restrições de recurso de banco de dados SQL do Azure | Microsoft Docs
description: As restrições de recurso de banco de dados SQL do Azure aprimora sua segurança de banco de dados, restringindo os recursos no seu banco de dados que podem ser por invasores para obter acesso a informações neles.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: vainolo
ms.author: arib
ms.reviewer: vanto
manager: craigg
ms.date: 03/22/2019
ms.openlocfilehash: ac7a074e78def504a10b4daa07971f919f414a88
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66259446"
---
# <a name="azure-sql-database-feature-restrictions"></a>Restrições de recurso de banco de dados SQL do Azure

Uma fonte comum de ataques de SQL Server é por meio de aplicativos web que acessam o banco de dados onde as várias formas de ataques de injeção de SQL são usadas para reunir informações sobre o banco de dados.  O ideal é que o código do aplicativo é desenvolvido para que ele não permite a injeção de SQL.  No entanto, em grandes-bases de código que incluem o código herdado e externo, um nunca pode ser-se de que todos os casos foram resolvidos, portanto, injeções de SQL são um fato da vida que temos para proteger contra.  O objetivo de restrições de recursos é impedir que algumas formas de injeção de SQL contra vazamento de informações sobre o banco de dados, mesmo quando a injeção de SQL é bem-sucedida.

## <a name="enabling-feature-restrictions"></a>Habilitar restrições de recursos

Habilitar restrições de recursos é feito usando o `sp_add_feature_restriction` procedimento armazenado da seguinte maneira:

```sql
EXEC sp_add_feature_restriction <feature>, <object_class>, <object_name>
```

Os recursos a seguir podem ser restritos:

| Recurso          | DESCRIÇÃO |
|------------------|-------------|
| N'ErrorMessages' | Quando a restrita, qualquer dado de usuário dentro da mensagem de erro será mascarado. Consulte [mensagens de erro de restrição de recursos](#error-messages-feature-restriction) |
| N'Waitfor'       | Quando a restrita, o comando retornará imediatamente sem qualquer atraso. Consulte [restrição de recurso WAITFOR](#waitfor-feature-restriction) |

O valor de `object_class` pode ser `N'User'` ou `N'Role'` para indicar se `object_name` é um nome de usuário ou um nome de função no banco de dados.

O exemplo a seguir fará com que todas as mensagens de erro para o usuário `MyUser` a ser mascarado:

```sql
EXEC sp_add_feature_restriction N'ErrorMessages', N'User', N'MyUser'
```

## <a name="disabling-feature-restrictions"></a>Desabilitando restrições de recursos

Desabilitar restrições de recursos é feito usando o `sp_drop_feature_restriction` procedimento armazenado da seguinte maneira:

```sql
EXEC sp_drop_feature_restriction <feature>, <object_class>, <object_name>
```

O exemplo a seguir desabilita o mascaramento de mensagem de erro para o usuário `MyUser`:

```sql
EXEC sp_drop_feature_restriction N'ErrorMessages', N'User', N'MyUser'
```

## <a name="viewing-feature-restrictions"></a>Restrições de recursos de visualização

O `sys.sql_feature_restrictions` exibição apresenta todas as restrições de recurso definidos atualmente no banco de dados. Ele tem as seguintes colunas:

| Nome da coluna | Tipo de dados | DESCRIÇÃO |
|-------------|-----------|-------------|
| class       | nvarchar(128) | Classe de objeto ao qual a restrição se aplica |
| objeto      | nvarchar(256) | Nome do objeto ao qual a restrição se aplica |
| Recurso     | nvarchar(128) | Recurso que é restrito |

## <a name="feature-restrictions"></a>Restrições de recursos

### <a name="error-messages-feature-restriction"></a>Restrição de recurso de mensagens de erro

Um método comum de ataque de injeção SQL é injetar o código que causa um erro.  Examinando a mensagem de erro, um invasor pode saber informações sobre o sistema, permitindo ataques mais direcionadas adicionais.  Esse ataque pode ser especialmente útil quando o aplicativo não exibe os resultados de uma consulta, mas exibir mensagens de erro.

Considere um aplicativo web que tem uma solicitação na forma de:

```html
http://www.contoso.com/employee.php?id=1
```

Que executa a consulta de banco de dados a seguir:

```sql
SELECT Name FROM EMPLOYEES WHERE Id=$EmpId
```

Se o valor passado como o `id` parâmetro para a solicitação do aplicativo web é copiado para substituir $EmpId na consulta de banco de dados, um invasor pode fazer a solicitação a seguir:

```html
http://www.contoso.com/employee.php?id=1 AND CAST(DB_NAME() AS INT)=0
```

E o seguinte erro será retornado, permitindo que o invasor saber o nome do banco de dados:

```sql
Conversion failed when converting the nvarchar value 'HR_Data' to data type int.
```

Depois de habilitar mensagens de erro de restrição para o usuário do aplicativo no banco de dados de recursos, a mensagem de erro retornada é mascarada, de modo que nenhuma informação interna sobre o banco de dados é vazada:

```sql
Conversion failed when converting the ****** value '******' to data type ******.
```

Da mesma forma, o invasor pode fazer a solicitação a seguir:

```html
http://www.contoso.com/employee.php?id=1 AND CAST(Salary AS TINYINT)=0
```

E o seguinte erro será retornado, permitindo que o invasor saiba o salário do funcionário:

```sql
Arithmetic overflow error for data type tinyint, value = 140000.
```

Usando a restrição de recurso de mensagens de erro, o banco de dados retornará:

```sql
Arithmetic overflow error for data type ******, value = ******.
```

### <a name="waitfor-feature-restriction"></a>Restrição de recurso WAITFOR

Uma injeção de SQL cega é quando um aplicativo não fornece ao invasor com os resultados do SQL injetado ou com uma mensagem de erro, mas o invasor pode deduzir informações do banco de dados, criando uma consulta condicional na qual duas ramificações condicionais levar uma quantidade diferente de tempo para ser executado. Comparando o tempo de resposta, o invasor pode saber qual branch foi executado e, portanto, para obter informações sobre o sistema. A variante mais simples desse ataque é usando o `WAITFOR` instrução para introduzir o atraso.

Considere um aplicativo web que tem uma solicitação na forma de:

```html
http://www.contoso.com/employee.php?id=1
```

que executa a consulta de banco de dados a seguir:

```sql
SELECT Name FROM EMPLOYEES WHERE Id=$EmpId
```

Se o valor passado como o parâmetro de id para as solicitações de aplicativo da web é copiado para substituir $EmpId na consulta de banco de dados, um invasor pode fazer a solicitação a seguir:

```html
http://www.contoso.com/employee.php?id=1; IF SYSTEM_USER='sa' WAITFOR DELAY '00:00:05'
```

E a consulta levaria um adicional de 5 segundos se o `sa` conta estava sendo usada. Se `WAITFOR` restrição de recurso está desabilitada no banco de dados, o `WAITFOR` instrução será ignorada e não informações são vazadas usando esse ataque.