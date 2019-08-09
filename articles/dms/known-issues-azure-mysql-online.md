---
title: Artigo sobre as limitações de migração e os problemas conhecidos das migrações online para o Banco de Dados do Azure para MySQL | Microsoft Docs
description: Saiba mais sobre as limitações de migração e os problemas conhecidos das migrações online para o Banco de Dados do Azure para MySQL.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 08/06/2019
ms.openlocfilehash: fc5565ab9e3be21b96ce5aa5a938cf22ec3caeb0
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848495"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-to-azure-db-for-mysql"></a>Limitações de migração e problemas conhecidos das migrações online para o BD do Azure para MySQL

Os problemas conhecidos e as limitações associados às migrações online do MySQL para o Banco de Dados do Azure para MySQL são descritos nas seções a seguir.

## <a name="online-migration-configuration"></a>Configuração de migração online

- A versão do MySQL Server de origem precisa ser a 5.6.35, a 5.7.18 ou posteriores
- O Banco de Dados do Azure para MySQL dá suporte a:
  - MySQL Community Edition
  - Mecanismo InnoDB
- Migração de mesma versão. Não há suporte para a migração do MySQL 5.6 para o Banco de Dados do Azure para MySQL 5.7.
- Habilite o log binário em my.ini (Windows) ou em my.cnf (Unix)
  - Defina Server_id para qualquer número maior ou igual a 1, por exemplo, Server_id=1 (somente para o MySQL 5.6)
  - Definir log-bin = \<Path > (somente para MySQL 5,6)
  - Defina binlog_format = row
  - Expire_logs_days = 5 (recomendado, apenas para o MySQL 5.6)
- O usuário precisa ter a função ReplicationAdmin.
- As ordenações definidas para o banco de dados MySQL de origem devem ser as mesmas que as definidos no Banco de Dados do Azure para MySQL de destino.
- O esquema precisa corresponder entre o banco de dados MySQL de origem e o banco de dados de destino no Banco de Dados do Azure para MySQL.
- O esquema no Banco de Dados do Azure para MySQL de destino não pode ter chaves estrangeiras. Use a consulta a seguir para remover as chaves estrangeiras:
    ```
    SET group_concat_max_len = 8192;
    SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery
    FROM
    (SELECT 
    KCU.REFERENCED_TABLE_SCHEMA as SchemaName, KCU.TABLE_NAME, KCU.COLUMN_NAME,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery,
        CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery
        FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC
        WHERE
          KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME
          AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA
      AND KCU.REFERENCED_TABLE_SCHEMA = ['schema_name']) Queries
      GROUP BY SchemaName;
    ```

    Execute a remoção de chave estrangeira (que é a segunda coluna) no resultado da consulta.
- O esquema no Banco de Dados do Azure para MySQL de destino não pode ter nenhum gatilho. Para remover os gatilhos no banco de dados de destino:
    ```
    SELECT Concat('DROP TRIGGER ', Trigger_Name, ';') FROM  information_schema.TRIGGERS WHERE TRIGGER_SCHEMA = 'your_schema';
    ```

## <a name="datatype-limitations"></a>Limitações de tipo de dados

- **Limitação**: Se houver um tipo de dados JSON no banco de dados MySQL de origem, a migração falhará durante a sincronização contínua.

    **Solução alternativa**: Modifique o tipo de dados JSON para texto médio ou longtext no banco de dados MySQL de origem.

- **Limitação**: Se não houver nenhuma chave primária nas tabelas, a sincronização contínua falhará.

    **Solução alternativa**: Defina temporariamente uma chave primária para a tabela para migração continuar. Você poderá remover a chave primária após a conclusão da migração de dados.

## <a name="lob-limitations"></a>Limitações de LOB

As colunas de LOB (Objeto Grande) são aquelas cujo tamanho pode aumentar muito. Para MySQL, texto médio, longtext, BLOB, mediumblob, longblob, etc., são alguns dos tipos de texto de LOB.

- **Limitação**: Se tipos de dados LOB forem usados como chaves primárias, a migração falhará.

    **Solução alternativa**: Substitua a chave primária por outros tipos de dados ou colunas que não sejam LOB.

- **Limitação**: Se o tamanho da coluna LOB (Objeto Grande) for maior que 32 KB, os dados poderão ser truncados no destino. Você pode verificar o tamanho da coluna de LOB usando esta consulta:
    ```
    SELECT max(length(description)) as LEN from catalog;
    ```

    **Solução alternativa**: Se você tiver um objeto LOB maior que 32 KB, entre em contato com a equipe de engenharia em [solicitar migrações de banco de dados do Azure](mailto:AskAzureDatabaseMigrations@service.microsoft.com). 

## <a name="limitations-when-migrating-online-from-aws-rds-mysql"></a>Limitações ao migrar online do AWS RDS MySQL

Ao tentar executar uma migração online do AWS RDS MySQL para o banco de dados do Azure para MySQL, você pode vir os erros a seguir.

- **Erro:** O banco{0}de dados ' ' tem chave (s) estrangeira (ões) no destino. Corrija o destino e inicie uma nova atividade de migração de dados. Execute o script abaixo no destino para listar as chaves estrangeiras

  **Limitação**: Se você tiver chaves estrangeiras em seu esquema, o carregamento inicial e a sincronização contínua da migração falharão.
  **Solução alternativa**: Execute o seguinte script no MySQL Workbench para extrair o script de soltar a chave estrangeira e adicionar um script de chave estrangeira:

  ```
  SET group_concat_max_len = 8192; SELECT SchemaName, GROUP_CONCAT(DropQuery SEPARATOR ';\n') as DropQuery, GROUP_CONCAT(AddQuery SEPARATOR ';\n') as AddQuery FROM (SELECT KCU.REFERENCED_TABLE_SCHEMA as SchemaName, KCU.TABLE_NAME, KCU.COLUMN_NAME, CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' DROP FOREIGN KEY ', KCU.CONSTRAINT_NAME) AS DropQuery, CONCAT('ALTER TABLE ', KCU.TABLE_NAME, ' ADD CONSTRAINT ', KCU.CONSTRAINT_NAME, ' FOREIGN KEY (`', KCU.COLUMN_NAME, '`) REFERENCES `', KCU.REFERENCED_TABLE_NAME, '` (`', KCU.REFERENCED_COLUMN_NAME, '`) ON UPDATE ',RC.UPDATE_RULE, ' ON DELETE ',RC.DELETE_RULE) AS AddQuery FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE KCU, information_schema.REFERENTIAL_CONSTRAINTS RC WHERE KCU.CONSTRAINT_NAME = RC.CONSTRAINT_NAME AND KCU.REFERENCED_TABLE_SCHEMA = RC.UNIQUE_CONSTRAINT_SCHEMA AND KCU.REFERENCED_TABLE_SCHEMA = 'SchemaName') Queries GROUP BY SchemaName;
  ```

- **Erro:** O banco{0}de dados ' ' não existe no servidor. O servidor de origem do MySQL fornecido diferencia maiúsculas de minúsculas. Verifique o nome do banco de dados.

  **Limitação**: Ao migrar um banco de dados MySQL no Azure usando a Interface de linha de comando (CLI), os usuários podem receber esse erro. O serviço não pôde localizar o banco de dados no servidor de origem, o que pode ser porque você pode ter fornecido um nome de banco de dados incorreto ou o banco de dados não existe no servidor listado. Observação os nomes de banco de dados diferenciam maiúsculas de minúsculas.

  **Solução alternativa**: Forneça o nome exato do banco de dados e tente novamente.

- **Erro:** Há tabelas com o mesmo nome no banco de dados ' {database} '. O Banco de Dados do Azure para MySQL não oferece suporte a diferenciação de maiúsculas de minúsculas para tabelas.

  **Limitação**: Esse erro ocorre quando você tem duas tabelas com o mesmo nome no banco de dados de origem. O banco de dados do Azure para MySQL não dá suporte a tabelas com diferenciação de maiúsculas

  **Solução alternativa**: Atualize os nomes de tabela para serem exclusivos e tente novamente.

- **Erro:** O banco de dados de destino {Database} está vazio. Migre o esquema.

  **Limitação**: Esse erro ocorre quando o banco de dados de destino do banco de dados do Azure para MySQL não tem o esquema necessário. A migração de esquema é necessária para habilitar a migração de dados para o destino.

  **Solução alternativa**: [Migre o esquema](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online#migrate-the-sample-schema) do banco de dados de origem para o banco de dados de destino.

## <a name="other-limitations"></a>Outras limitações

- Não há suporte para cadeia de caracteres de senha com colchetes {} de abertura e fechamento em seu início e final. Essa limitação aplica-se tanto à conexão com o MySQL de origem quanto com o Banco de Dados do Azure para MySQL de destino.
- Não há suporte para as DDLs a seguir:
  - Todas as DDLs de partição
  - Remover tabela
  - Renomear tabela
- Não há suporte para usar a instrução *alter table <table_name> add column <column_name>* para adicionar colunas ao início ou ao meio de uma tabela. A instrução *alter table <table_name> add column <column_name>* adiciona a coluna ao final da tabela.
- Não há suporte para índices criados com apenas uma parte dos dados da coluna. A instrução a seguir é um exemplo que cria um índice usando apenas uma parte dos dados da coluna:

    ``` 
    CREATE INDEX partial_name ON customer (name(10));
    ```

- No DMS, o limite de bancos de dados a serem migrados em uma única atividade de migração é quatro.
