---
title: Copiar dados para ou do Banco de Dados SQL do Azure usando o Data Factory | Microsoft Docs
description: Saiba como copiar dados de armazenamentos de dados de origem com suporte para o Banco de Dados SQL do Azure ou do Banco de Dados SQL do Azure para armazenamentos de dados de coletor com suporte, usando o Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/14/2019
ms.author: jingwang
ms.openlocfilehash: 90126a607065bdb10e2ff81ce6ab52809ecc3f36
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273752"
---
# <a name="copy-data-to-or-from-azure-sql-database-by-using-azure-data-factory"></a>Copiar dados de ou para o Banco de Dados SQL do Azure usando o Azure Data Factory
> [!div class="op_single_selector" title1="Selecione a versão do serviço Data Factory que você usar:"]
> * [Versão 1](v1/data-factory-azure-sql-connector.md)
> * [Versão atual](connector-azure-sql-database.md)

Este artigo descreve como copiar dados para e do banco de dados SQL. Para saber mais sobre o Azure Data Factory, leia as [artigo introdutório](introduction.md).

## <a name="supported-capabilities"></a>Funcionalidades com suporte

Este conector do banco de dados SQL tem suporte para as seguintes atividades:

- [Atividade de cópia](copy-activity-overview.md) com [suporte para a matriz de origem/coletor](copy-activity-overview.md) tabela
- [Mapeamento de fluxo de dados](concepts-data-flow-overview.md)
- [Atividade de pesquisa](control-flow-lookup-activity.md)
- [Atividade GetMetadata](control-flow-get-metadata-activity.md)

Especificamente, este conector de banco de dados SQL do Azure dá suporte a estas funções:

- Copie os dados usando a autenticação de token do Aplicativo Azure AD (Azure Active Directory) e autenticação do SQL com uma entidade de serviço ou identidades gerenciadas para recursos do Azure.
- Como uma fonte, recupere dados usando uma consulta SQL ou procedimento armazenado.
- Como um coletor, anexe os dados a uma tabela de destino ou chame um procedimento armazenado com lógica personalizada durante a cópia.

>[!NOTE]
>Banco de dados SQL do Azure **[Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-current)** não tem suporte por este conector agora. Solução alternativa, você pode usar [conector ODBC genérico](connector-odbc.md) e o driver ODBC do SQL Server por meio do Integration Runtime auto-hospedado. Siga [neste guia](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=azuresqldb-current) com configurações de cadeia de caracteres ODBC driver download e a conexão.

> [!IMPORTANT]
> Se você copiar dados usando o Tempo de Execução de Integração do Azure Data Factory, configure um [firewall do servidor SQL do Azure](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) para que os Serviços do Azure possam acessar o servidor.
> Se você copiar dados usando um tempo de execução de integração auto-hospedado, configure o firewall do servidor SQL do Azure para permitir o intervalo de IP apropriado. Esse intervalo inclui o IP da máquina usado para se conectar ao Banco de Dados SQL do Azure.

## <a name="get-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As seções a seguir fornecem detalhes sobre as propriedades usadas para definir entidades do Data Factory específicas de um conector do Banco de Dados SQL do Azure.

## <a name="linked-service-properties"></a>Propriedades do serviço vinculado

Essas propriedades são suportadas por um serviço vinculado ao Banco de Dados SQL do Azure:

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| tipo | A propriedade  **tipo** deve ser definida como **AzureSqlDatabase**. | Sim |
| connectionString | Especifique as informações necessárias para se conectar à instância do Banco de Dados SQL do Azure para a propriedade **connectionString**. <br/>Marque esse campo como SecureString para armazená-lo com segurança no Data Factory. Você também pode colocar uma senha/chave da entidade de serviço no Azure Key Vault e se sua autenticação do SQL efetua pull da configuração da `password` da cadeia de conexão. Veja o exemplo de JSON abaixo da tabela e o artigo [Armazenar credenciais no Azure Key Vault](store-credentials-in-key-vault.md) que fornece mais detalhes. | Sim |
| servicePrincipalId | Especifique a ID do cliente do aplicativo. | Sim, quando você usa a autenticação do Azure AD com uma entidade de serviço. |
| servicePrincipalKey | Especifique a chave do aplicativo. Marque esse campo como **SecureString** para armazená-lo com segurança no Data Factory ou [referencie um segredo armazenado no Cofre de Chaves do Azure](store-credentials-in-key-vault.md). | Sim, quando você usa a autenticação do Azure AD com uma entidade de serviço. |
| tenant | Especifique as informações de locatário (domínio nome ou ID do Locatário) em que o aplicativo reside. Para recuperá-lo, passe o mouse no canto superior direito do portal do Azure. | Sim, quando você usa a autenticação do Azure AD com uma entidade de serviço. |
| connectVia | O [Integration Runtime](concepts-integration-runtime.md) a ser usado para se conectar ao armazenamento de dados. Você pode usar o Tempo de Execução de Integração do Azure ou um tempo de execução de integração auto-hospedado se o seu armazenamento de dados estiver localizado em uma rede privada. Se não for especificado, ele usa o Integration Runtime padrão do Azure. | Não |

Para diferentes tipos de autenticação, consulte as seções a seguir sobre pré-requisitos e amostras JSON, respectivamente:

- [Autenticação do SQL](#sql-authentication)
- [Autenticação de token do aplicativo Azure AD: Entidade de serviço](#service-principal-authentication)
- [Autenticação de token do aplicativo Azure AD: identidades gerenciadas para recursos do Azure](#managed-identity)

>[!TIP]
>Se ocorrer erro com código de erro como "UserErrorFailedToConnectToSqlServer" e mensagem como "O limite da sessão para o banco de dados é XXX e foi atingido.", adicione `Pooling=false` à cadeia de conexão e tente novamente.

### <a name="sql-authentication"></a>Autenticação do SQL

#### <a name="linked-service-example-that-uses-sql-authentication"></a>Exemplo de serviço vinculado que usa autenticação SQL

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Senha no Azure Key Vault:** 

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            },
            "password": { 
                "type": "AzureKeyVaultSecret", 
                "store": { 
                    "referenceName": "<Azure Key Vault linked service name>", 
                    "type": "LinkedServiceReference" 
                }, 
                "secretName": "<secretName>" 
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Autenticação de entidade de serviço

Para usar uma autenticação de token de aplicativo do Azure AD baseada no serviço principal, siga estas etapas:

1. **[Crie um aplicativo do Azure Active Directory](../active-directory/develop/howto-create-service-principal-portal.md#create-an-azure-active-directory-application)** no portal do Azure. Anote o nome do aplicativo e os seguintes valores que definem o serviço vinculado:

    - ID do aplicativo
    - Chave do aplicativo
    - ID do locatário

2. **[Provisione um administrador do Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)** para o servidor SQL do Azure no Portal do Azure, caso ainda não tenha feito isso. O administrador do Azure AD deve ser um usuário do Azure AD ou um grupo do Azure AD, mas não pode ser um diretor de serviços. Essa etapa é feita para que, na próxima etapa, você possa usar uma identidade do Azure AD para criar um usuário de banco de dados contido para a entidade de serviço.

3. **[ Crie usuários de banco de dados contidos](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)** para o diretor de serviços. Conecte-se ao banco de dados de ou para o qual você deseja copiar dados usando ferramentas como SSMS, com uma identidade do Azure AD que tenha pelo menos a permissão ALTER ANY USER. Execute o seguinte T-SQL: 
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. **Conceda ao principal de serviço as permissões necessárias**, como faria normalmente para usuários do SQL ou outros. Execute o seguinte código, ou consulte para obter mais opções [aqui](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017).

    ```sql
    EXEC sp_addrolemember [role name], [your application name];
    ```

5. **Configure um serviço vinculado do Banco de Dados SQL do Azure** no Azure Data Factory.


#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Exemplo de serviço vinculado que usa autenticação principal de serviço

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            },
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Identidades gerenciadas para autenticação de recursos do Azure

Um data factory pode ser associado a uma [identidade gerenciada para recursos do Azure](data-factory-service-identity.md), que representa esse data factory específico. Você pode usar essa identidade gerenciada para a autenticação de banco de dados SQL. A fábrica designada pode acessar e copiar dados de ou para seu banco de dados usando essa identidade.

Para usar a autenticação de identidade gerenciada, siga estas etapas:

1. **[Provisione um administrador do Azure Active Directory](../sql-database/sql-database-aad-authentication-configure.md#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)** para o servidor SQL do Azure no Portal do Azure, caso ainda não tenha feito isso. O administrador do Azure AD pode ser um usuário do AD do Azure ou um grupo do Azure AD. Se você conceder ao grupo com uma função de administrador de identidade gerenciada, ignore as etapas 3 e 4. O administrador terá acesso total ao banco de dados.

2. **[Criar usuários de banco de dados independente](../sql-database/sql-database-aad-authentication-configure.md#create-contained-database-users-in-your-database-mapped-to-azure-ad-identities)**  para a identidade gerenciada do Data Factory. Conecte-se ao banco de dados de ou para o qual você deseja copiar dados usando ferramentas como SSMS, com uma identidade do Azure AD que tenha pelo menos a permissão ALTER ANY USER. Execute o seguinte T-SQL: 
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER;
    ```

3. **Conceda as permissões necessárias da identidade gerenciada do Data Factory** como faria normalmente para usuários do SQL e outros. Execute o seguinte código, ou consulte para obter mais opções [aqui](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017).

    ```sql
    EXEC sp_addrolemember [role name], [your Data Factory name];
    ```

4. **Configure um serviço vinculado do Banco de Dados SQL do Azure** no Azure Data Factory.

**Exemplo:**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;Connection Timeout=30"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para obter uma lista completa das seções e propriedades disponíveis para definir os conjuntos de dados, confira o artigo sobre [Conjuntos de Dados](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services). Esta seção fornece uma lista de propriedades com suporte pelo conjunto de dados do banco de dados SQL do Azure.

Para copiar dados de ou para o banco de dados SQL, há suporte para as seguintes propriedades:

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| type | A propriedade **tipo** do conjunto de dados deve ser definida como **AzureSqlTable**. | Sim |
| tableName | O nome da tabela ou exibição na instância do Banco de Dados SQL do Azure à qual o serviço vinculado se refere. | Não para fonte, Sim para o coletor |

#### <a name="dataset-properties-example"></a>Exemplo de propriedades do conjunto de dados

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da Atividade de Cópia

Para obter uma lista completa das seções e propriedades disponíveis para definir atividades, confia o artigo [Pipelines](concepts-pipelines-activities.md). Esta seção fornece uma lista de propriedades suportadas pela origem e pelo coletor do Banco de Dados SQL do Azure.

### <a name="azure-sql-database-as-the-source"></a>Banco de Dados SQL do Azure como a origem

Para copiar dados do Banco de Dados SQL do Azure, defina a propriedade **tipo** na origem da Atividade de Cópia para **SqlSource**. As seguintes propriedades são suportadas na seção **source** da atividade de cópia:

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| tipo | A propriedade **tipo** da origem da Atividade de Cópia deve ser definida como **SqlSource**. | Sim |
| sqlReaderQuery | Utiliza a consulta SQL personalizada para ler os dados. Exemplo: `select * from MyTable`. | Não |
| sqlReaderStoredProcedureName | O nome do procedimento armazenado que lê dados da tabela de origem. A última instrução SQL deve ser uma instrução SELECT no procedimento armazenado. | Não |
| storedProcedureParameters | Parâmetros para o procedimento armazenado.<br/>Valores permitidos são pares de nome ou valor. Nomes e uso de maiúsculas e minúsculas de parâmetros devem corresponder aos nomes e o uso de maiúsculas e minúsculas dos parâmetros do procedimento armazenado. | Não |

### <a name="points-to-note"></a>Pontos a serem observados

- Se **sqlReaderQuery** for especificado para o **SqlSource**, o Copy Activity executará essa consulta em relação à origem do Banco de Dados SQL do Azure para obter os dados. Ou você pode especificar um procedimento armazenado. Especifique **sqlReaderStoredProcedureName** e **storedProcedureParameters** se o procedimento armazenado receber parâmetros.
- Se você não especificar **sqlReaderQuery** ou **sqlReaderStoredProcedureName**, as colunas definidas na seção **structure** do conjunto de dados JSON serão usadas para construir uma consulta. `select column1, column2 from mytable`é executado no Banco de Dados SQL do Azure. Se a definição do conjunto de dados não tiver a **estrutura**, todas as colunas serão selecionadas da tabela.

#### <a name="sql-query-example"></a>Exemplo de consulta SQL

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="stored-procedure-example"></a>Exemplo de procedimento armazenado

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "SqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="stored-procedure-definition"></a>Definição do procedimento armazenado

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="azure-sql-database-as-the-sink"></a>Banco de Dados SQL do Azure como o coletor

> [!TIP]
> Saiba mais sobre os comportamentos de gravação com suporte, configurações e a prática recomendada de [práticas recomendadas para carregar dados no banco de dados SQL](#best-practice-for-loading-data-into-azure-sql-database).

Para copiar dados para o banco de dados SQL do Azure, defina o **tipo** do coletor de propriedade na atividade de cópia para **SqlSink**. As seguintes propriedades são suportadas na seção Copy Activity **sink**:

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| type | O **tipo** do coletor de atividade de cópia deve ser definida como **SqlSink**. | Sim |
| writeBatchSize | Número de linhas para inserções na tabela SQL **por lote**.<br/> O valor permitido é **inteiro** (número de linhas). Por padrão, o Data Factory determinar dinamicamente o tamanho de lote apropriado com base no tamanho da linha. | Não |
| writeBatchTimeout | O tempo de espera para o lote inserir operação seja concluída antes de expirar.<br/> O valor permitido é **timespan**. Exemplo: “00:30:00” (30 minutos). | Não |
| preCopyScript | Especifique uma consulta SQL para que a Atividade de Cópia seja executada antes de gravar dados no Banco de Dados SQL do Azure. É invocado apenas uma vez por cópia. Use essa propriedade para limpar os dados pré-carregados. | Não |
| sqlWriterStoredProcedureName | O nome do procedimento armazenado que define como aplicar dados de origem em uma tabela de destino. <br/>Este procedimento armazenado é **chamado por lote**. Para operações que somente executar uma vez e não têm nada a fonte de dados, por exemplo, excluir ou truncar, use o `preCopyScript` propriedade. | Não |
| storedProcedureParameters |Parâmetros para o procedimento armazenado.<br/>Valores permitidos são pares de nome e valor. Nomes e uso de maiúsculas e minúsculas de parâmetros devem corresponder aos nomes e o uso de maiúsculas e minúsculas dos parâmetros do procedimento armazenado. | Não |
| sqlWriterTableType | Especifique um nome do tipo de tabela a ser usado no procedimento armazenado. A Atividade de Cópia torna os dados movidos disponíveis em uma tabela temporária com este tipo de tabela. O código de procedimento armazenado pode mesclar os dados sendo copiados com dados existentes. | Não |

**Exemplo 1: anexar dados**

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Exemplo 2: invocar um procedimento armazenado durante a cópia**

Saiba mais detalhes em [Invocando o procedimento armazenado do SQL Sink](#invoking-stored-procedure-for-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "sqlWriterTableType": "CopyTestTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="best-practice-for-loading-data-into-azure-sql-database"></a>Prática recomendada para carregar dados no banco de dados SQL

Quando você copia dados para o banco de dados SQL, você pode exigir comportamento diferente de gravação:

- **[Acrescente](#append-data)** : minha fonte de dados só tem novos registros;
- **[Upsert](#upsert-data)** : minha fonte de dados tem inserções e atualizações.
- **[Substituir](#overwrite-entire-table)** : Eu quero recarregue a tabela de dimensões todo cada vez;
- **[Escrever com lógica personalizada](#write-data-with-custom-logic)** : Preciso de processamento extra antes da inserção final na tabela de destino.

Consulte as seções respectivamente sobre como configurar no AAD e as práticas recomendadas.

### <a name="append-data"></a>Acrescentar dados

Esse é o comportamento padrão desse conector de coletor do banco de dados SQL e fazer de ADF **inserção em massa** para gravar em sua tabela com eficiência. Você pode simplesmente configurar a fonte e coletor adequadamente na atividade de cópia.

### <a name="upsert-data"></a>Upsert data

**Opção eu** (sugerido especialmente quando você tiver grandes dados a serem copiados): o **abordagem de alto desempenho a maioria dos** fazer upsert é o seguinte: 

- Em primeiro lugar, aproveite um [tabela temporária no escopo do banco de dados](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql?view=azuresqldb-current#database-scoped-global-temporary-tables-azure-sql-database) para carregamento em massa todos os registros usando a atividade de cópia. Como escopo de operações no banco de dados temporário tabelas não são registradas, você pode carregar milhões de registros em segundos.
- Executar uma atividade de procedimento armazenado no ADF para aplicar uma [mesclar](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current) (ou inserir/atualizar) a instrução e use o temp de tabela como fonte para executar todas as atualiza ou insere como uma única transação, reduzindo a quantidade de idas e voltas e operações de log. No final da atividade de procedimento armazenado, a tabela temporária pode ser truncada para estar pronto para o próximo ciclo de upsert. 

Por exemplo, no Azure Data Factory, você pode criar um pipeline com uma **atividade de cópia** encadeado com um **atividade de procedimento armazenado** em caso de sucesso. O primeiro copia dados de seu armazenamento de origem em uma tabela temporária do banco de dados SQL, diga " **##UpsertTempTable**" como nome da tabela no conjunto de dados, em seguida, o último invoca um procedimento armazenado para mesclar a fonte de dados da tabela temp no destino tabela e limpar a tabela temporária.

![Upsert](./media/connector-azure-sql-database/azure-sql-database-upsert.png)

No banco de dados, defina um procedimento armazenado com lógica de mesclagem, semelhante à seguinte, que é apontada da atividade de procedimento armazenado acima. Supondo que o destino **Marketing** tabela com três colunas: **ProfileID**, **estado**, e **categoria**, e fazer o upsert com base no **ProfileID** coluna.

```sql
CREATE PROCEDURE [dbo].[spMergeData]
AS
BEGIN
    MERGE TargetTable AS target
    USING ##UpsertTempTable AS source
    ON (target.[ProfileID] = source.[ProfileID])
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT matched THEN
        INSERT ([ProfileID], [State], [Category])
      VALUES (source.ProfileID, source.State, source.Category);
    
    TRUNCATE TABLE ##UpsertTempTable
END
```

**Opção II:** como alternativa, você pode optar por [invocar um procedimento armazenado dentro de atividade de cópia](#invoking-stored-procedure-for-sql-sink), enquanto essa abordagem é executada para cada linha na tabela de origem em vez de em massa de aproveitamento de nota inserir como a abordagem padrão na atividade de cópia, portanto, ele não se ajustar para upsert em larga escala.

### <a name="overwrite-entire-table"></a>Substituir a tabela inteira

Você pode configurar **preCopyScript** coletor de propriedade na atividade de cópia, nesse caso, para cada execução de atividade de cópia, ADF executa o script pela primeira vez, em seguida, execute a cópia para inserir os dados. Por exemplo, para substituir a tabela inteira pelos dados mais recentes, você pode especificar um script para excluir primeiro todos os registros, antes de carregar em massa os novos dados da origem.

### <a name="write-data-with-custom-logic"></a>Gravar dados com lógica personalizada

Semelhante, conforme descrito em [Upsert dados](#upsert-data) seção, quando você precisar aplicar processamento extra antes da inserção final dos dados de origem na tabela de destino, é possível um) para grande escala, de carga para uma tabela temporária com escopo de banco de dados e depois invocar um procedimento armazenado, ou b) invocar um procedimento armazenado durante a cópia.

## <a name="invoking-stored-procedure-for-sql-sink"></a> Invocar o procedimento armazenado do coletor SQL

Quando você copia dados para o banco de dados SQL, você também pode configurar e invocar um procedimento armazenado especificado pelo usuário com parâmetros adicionais.

> [!TIP]
> Invocação de procedimento armazenado processa a dados linha por linha, em vez de operação em massa, o que não é sugerida para cópia de grande escala. Saiba mais sobre [práticas recomendadas para carregar dados no banco de dados SQL](#best-practice-for-loading-data-into-azure-sql-database).

Você pode usar um procedimento armazenado quando mecanismos de cópia internos não têm essa finalidade, por exemplo, aplicar processamento extra antes da inserção final dos dados de origem na tabela de destino. Alguns exemplos de processamento extra são colunas de mesclagem, pesquisa valores adicionais e inserção em mais de uma tabela.

A amostra a seguir mostra como usar um procedimento armazenado para fazer um upsert em uma tabela no Banco de Dados SQL do Azure. Supondo que os dados de entrada e cada tabela **Marketing** do coletor tenham três colunas: **ProfileID**, **State** e **Category**. Faça o upsert com base na coluna **ProfileID** e aplique-o apenas a uma categoria específica.

**Conjunto de dados de saída:** "tableName" deve ser o mesmo nome de parâmetro de tipo de tabela em seu procedimento armazenado (veja abaixo o script de procedimento armazenado).

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "Marketing"
        }
    }
}
```

Definir as **coletor SQL** seção na atividade de cópia da seguinte maneira.

```json
"sink": {
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing",
    "storedProcedureParameters": {
        "category": {
            "value": "ProductA"
        }
    }
}
```

Em seu banco de dados, defina o procedimento armazenado com o mesmo nome que o **SqlWriterStoredProcedureName**. Ele manipula os dados de entrada da sua origem especificada e mescla na tabela de saída. O nome do parâmetro do tipo de tabela no procedimento armazenado deve ser o mesmo que o **tableName** definido no conjunto de dados.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
AS
BEGIN
  MERGE [dbo].[Marketing] AS target
  USING @Marketing AS source
  ON (target.ProfileID = source.ProfileID and target.Category = @category)
  WHEN MATCHED THEN
      UPDATE SET State = source.State
  WHEN NOT MATCHED THEN
      INSERT (ProfileID, State, Category)
      VALUES (source.ProfileID, source.State, source.Category);
END
```

No banco de dados, definir o tipo de tabela com o mesmo nome que o **sqlWriterTableType**. O esquema do tipo de tabela deve ser o mesmo que o esquema retornado pelos dados de entrada.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL,
    [Category] [varchar](256) NOT NULL
)
```

O recurso de procedimento armazenado se beneficia de [parâmetros com valores de tabela](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="mapping-data-flow-properties"></a>Propriedades do mapeamento de fluxo de dados

Obtenha detalhes em [transformação de origem](data-flow-source.md) e [transformação do coletor](data-flow-sink.md) no mapeamento de fluxo de dados.

## <a name="data-type-mapping-for-azure-sql-database"></a>Mapeamento do tipo de dados do Banco de Dados SQL do Azure

Quando você copia dados de ou para o Banco de Dados SQL do Azure, os seguintes mapeamentos são usados dos tipos de dados do Banco de Dados SQL do Azure para os tipos de dados temporários do Azure Data Factory. Consulte [Esquema e mapeamentos de tipo de dados](copy-activity-schema-and-type-mapping.md) para saber como a Atividade de Cópia mapeia o esquema de origem e o tipo de dados para o coletor.

| Tipo de dados do Banco de Dados SQL do Azure | Tipo de dados provisório do Data Factory |
|:--- |:--- |
| bigint |Int64 |
| binary |Byte[] |
| bit |Boolean |
| char |String, Char[] |
| date |DateTime |
| DateTime |DateTime |
| datetime2 |DateTime |
| Datetimeoffset |DateTimeOffset |
| Decimal |Decimal |
| FILESTREAM attribute (varbinary(max)) |Byte[] |
| Float |Double |
| image |Byte[] |
| int |Int32 |
| money |Decimal |
| nchar |String, Char[] |
| ntext |String, Char[] |
| numeric |Decimal |
| nvarchar |String, Char[] |
| real |Single |
| rowversion |Byte[] |
| smalldatetime |DateTime |
| smallint |Int16 |
| smallmoney |Decimal |
| sql_variant |Object |
| text |String, Char[] |
| time |TimeSpan |
| timestamp |Byte[] |
| tinyint |Byte |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| xml |xml |

>[!NOTE]
> Em mapas de tipos de dados para o tipo provisório Decimal, atualmente o ADF dá suporte a uma precisão de até 28. Se você tiver dados com precisão maior do que 28, considere a conversão da cadeia de caracteres em consulta SQL.

## <a name="next-steps"></a>Próximas etapas
Para obter uma lista de repositórios de dados com suporte como fontes e repositórios por Atividade de Cópia no Azure Data Factory, consulte [repositórios de dados e formatos compatíveis](copy-activity-overview.md##supported-data-stores-and-formats).
