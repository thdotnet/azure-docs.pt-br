---
title: Copiar dados para e da Instância Gerenciada do Banco de Dados SQL do Azure usando o Azure Data Factory | Microsoft Docs
description: Aprenda a mover dados de e para a Instância Gerenciada do Banco de Dados SQL do Azure usando o Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: jingwang
ms.openlocfilehash: 9208ceeb760bba97c12b23a1b6e5bdff7efc9020
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274827"
---
# <a name="copy-data-to-and-from-azure-sql-database-managed-instance-by-using-azure-data-factory"></a>Copiar dados para e da Instância Gerenciada do Banco de Dados SQL do Azure usando o Azure Data Factory

Este artigo descreve como usar a atividade de cópia no Azure Data Factory para copiar dados de e para uma Instância Gerenciada do Banco de Dados SQL do Azure. Ele amplia o artigo [Visão geral da atividade de cópia](copy-activity-overview.md) que apresenta uma visão geral da atividade de cópia.

## <a name="supported-capabilities"></a>Funcionalidades com suporte

Você pode copiar dados de uma Instância Gerenciada do Banco de Dados SQL do Azure para qualquer armazenamento de dados de coletor com suporte. Você também pode copiar dados de qualquer armazenamento de dados de origem com suporte para a instância gerenciada. Para obter uma lista de armazenamentos de dados com suporte como origens e coletores pela atividade de cópia, confira a tabela [Armazenamentos de dados com suporte](copy-activity-overview.md#supported-data-stores-and-formats).

Especificamente, este conector da Instância Gerenciada do Banco de Dados SQL do Azure dá suporte a:

- Copiar dados usando a autenticação do SQL.
- Como uma origem, recuperar dados usando uma consulta SQL ou procedimento armazenado.
- Como o coletor, acrescentar dados a uma tabela de destino ou à invocação de um procedimento armazenado com lógica personalizada durante a cópia.

>[!NOTE]
>Instância de gerenciada do banco de dados de SQL do Azure **[Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-mi-current)** não tem suporte por este conector agora. Solução alternativa, você pode usar [conector ODBC genérico](connector-odbc.md) e o driver ODBC do SQL Server por meio do Integration Runtime auto-hospedado. Siga [neste guia](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=azuresqldb-mi-current) com configurações de cadeia de caracteres ODBC driver download e a conexão.

>[!NOTE]
>Autenticações de identidade gerenciada e entidade de serviço no momento, não são suportadas por este conector e sobre o plano para habilitar o logo depois. Por enquanto, para solucionar esse problema, você pode escolher manualmente e Azure SQL Database connector especificam o servidor de sua instância gerenciada.

## <a name="prerequisites"></a>Pré-requisitos

Para acessar o banco de dados de instância gerenciada do SQL  **[ponto de extremidade público](../sql-database/sql-database-managed-instance-public-endpoint-securely.md)** , você pode usar o ir do ADF gerenciado do Azure. Verifique se você não apenas habilitar o ponto de extremidade público, mas também permitem o tráfego de ponto de extremidade público no grupo de segurança de rede para tornar o ADF, sendo capaz de se conectar ao banco de dados, seguindo [neste guia](../sql-database/sql-database-managed-instance-public-endpoint-configure.md).

Para acessar o banco de dados de instância gerenciada do SQL **pontos de extremidade privados**, configure um [integration runtime auto-hospedado](create-self-hosted-integration-runtime.md) que pode acessar o banco de dados. Se você provisionar o tempo de execução de integração auto-hospedado na mesma rede virtual que sua instância gerenciada, certifique-se de que seu computador do integration runtime está em uma sub-rede diferente de sua instância gerenciada. Se você provisionar seu tempo de execução da integração auto-hospedada em uma rede virtual diferente que sua instância gerenciada, poderá usar um emparelhamento de rede virtual ou rede virtual para a conexão de rede virtual. Para obter mais informações, confira [Conectar seu aplicativo à Instância Gerenciada do Banco de Dados SQL do Azure](../sql-database/sql-database-managed-instance-connect-app.md).

## <a name="get-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As seções que se seguem fornecem detalhes sobre as propriedades que são usadas para definir entidades do Data Factory específicas ao conector da Instância Gerenciada do Banco de Dados SQL do Azure.

## <a name="linked-service-properties"></a>Propriedades do serviço vinculado

As propriedades a seguir têm suporte no serviço vinculado da Instância Gerenciada do Banco de Dados SQL do Azure:

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| type | A propriedade type deve ser definida como **SqlServer**. | Sim. |
| connectionString |Essa propriedade especifica as informações de connectionString necessárias para conectar-se à instância gerenciada usando a autenticação do SQL. Para obter mais informações, confira os exemplos a seguir. <br/>Marque esse campo como SecureString para armazená-lo com segurança no Data Factory. Você também pode colocar uma senha no Azure Key Vault, e se a autenticação for SQL, extraia a `password`configuração da cadeia de conexão. Veja o exemplo de JSON abaixo da tabela e o artigo [Armazenar credenciais no Azure Key Vault](store-credentials-in-key-vault.md) com mais detalhes. |Sim. |
| connectVia | Esse [Integration Runtime](concepts-integration-runtime.md) é usado para se conectar ao armazenamento de dados. Você pode usar o Integration Runtime auto-hospedado ou o Integration Runtime do Azure (se sua instância gerenciada tem o ponto de extremidade público e permitir que o ADF acessar). Se não for especificado, ele usa o Integration Runtime padrão do Azure. |Sim. |

**Exemplo 1: Usar autenticação do SQL** porta padrão é 1433. Se você estiver usando a instância gerenciada do SQL com o ponto de extremidade público, especifica explicitamente a porta 3342.

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<hostname,port>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Exemplo 2: Usar autenticação do SQL com senha no Azure Key Vault** porta padrão é 1433. Se você estiver usando a instância gerenciada do SQL com o ponto de extremidade público, especifica explicitamente a porta 3342.

```json
{
    "name": "AzureSqlMILinkedService",
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Data Source=<hostname,port>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;"
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

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para obter uma lista completa das seções e das propriedades disponíveis para uso para definir conjuntos de dados, confira o artigo sobre conjuntos de dados. Esta seção fornece uma lista das propriedades com suporte pelo conjunto de dados da Instância Gerenciada do Banco de Dados SQL do Azure.

Para copiar dados para e do banco de dados de instância gerenciada do SQL, há suporte para as seguintes propriedades:

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| type | A propriedade type do conjunto de dados deve ser definida como **SqlServerTable**. | Sim. |
| tableName |Essa propriedade é o nome da tabela ou exibição na instância de banco de dados à qual o serviço vinculado se refere. | Não para origem. Sim para coletor. |

**Exemplo**

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade de cópia

Para obter uma lista completa das seções e propriedades disponíveis para uso para definir atividades, confia o artigo [Pipelines](concepts-pipelines-activities.md). Esta seção fornece uma lista das propriedades com suporte da fonte de dados e do coletor da Instância Gerenciada do Banco de Dados SQL do Azure.

### <a name="azure-sql-database-managed-instance-as-a-source"></a>Instância Gerenciada do Banco de Dados SQL do Azure como uma fonte de dados

Para copiar dados da Instância Gerenciada do Banco de Dados SQL do Azure, defina o tipo de fonte na atividade de cópia como **SqlSource**. As propriedades a seguir têm suporte na seção de origem da atividade de cópia:

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| type | A propriedade type da origem da atividade de cópia deve ser definida como **SqlSource**. | Sim. |
| sqlReaderQuery |Essa propriedade usa a consulta SQL personalizada para ler dados. Um exemplo é `select * from MyTable`. |Não. |
| sqlReaderStoredProcedureName |Essa propriedade é o nome do procedimento armazenado que lê dados da tabela de origem. A última instrução SQL deve ser uma instrução SELECT no procedimento armazenado. |Não. |
| storedProcedureParameters |Esses parâmetros são para o procedimento armazenado.<br/>Valores permitidos são pares de nome ou valor. Os nomes e o uso de maiúsculas e minúsculas dos parâmetros devem corresponder aos nomes e o uso de maiúsculas e minúsculas dos parâmetros do procedimento armazenado. |Não. |

Observe os seguintes pontos:

- Se **sqlReaderQuery** for especificado para **SqlSource**, a atividade de cópia executará essa consulta na fonte de dados da instância gerenciada para obter os dados. Você também pode especificar um procedimento armazenado especificando o **sqlReaderStoredProcedureName** e o **storedProcedureParameters** se o procedimento armazenado usa parâmetros.
- Se você não especificar **sqlReaderQuery** ou **sqlReaderStoredProcedureName**, as colunas definidas na seção "structure" do conjunto de dados JSON serão usadas para criar uma consulta. A consulta `select column1, column2 from mytable` é executada em relação à instância gerenciada. Se a definição de conjunto de dados não tiver "structure", todas as colunas serão selecionadas da tabela.

**Exemplo: Usar uma consulta SQL**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
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

**Exemplo: Usar um procedimento armazenado**

```json
"activities":[
    {
        "name": "CopyFromAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Managed Instance input dataset name>",
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

**A definição do procedimento armazenado**

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

### <a name="azure-sql-database-managed-instance-as-a-sink"></a>Instância Gerenciada do Banco de Dados SQL do Azure como uma coletor

> [!TIP]
> Saiba mais sobre os comportamentos de gravação com suporte, configurações e a prática recomendada de [práticas recomendadas para carregar dados no banco de dados de instância gerenciada do SQL](#best-practice-for-loading-data-into-azure-sql-database-managed-instance).

Para copiar dados para a Instância Gerenciada do Banco de Dados SQL do Azure, defina o tipo de coletor na atividade de cópia como **SqlSink**. Há suporte para as seguintes propriedades na seção do coletor da atividade de cópia:

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| type | A propriedade type do coletor de atividade de cópia deve ser definida como **SqlSink**. | Sim. |
| writeBatchSize |Número de linhas para inserções na tabela SQL **por lote**.<br/>Os valores permitidos são inteiros para o número de linhas. Por padrão, o Data Factory determinar dinamicamente o tamanho de lote apropriado com base no tamanho da linha.  |Não |
| writeBatchTimeout |Essa propriedade especifica o tempo de espera para a operação de inserção em lotes a ser concluída antes de atingir o tempo limite.<br/>Os valores permitidos são para o intervalo de tempo. Um exemplo é "00: 30:00", que são 30 minutos. |Não. |
| preCopyScript |Esta propriedade especifica uma consulta SQL para a atividade de cópia a ser executada antes da gravação de dados na instância gerenciada. É chamado apenas uma vez por execução de cópia. Você pode usar essa propriedade para limpar os dados previamente carregados. |Não. |
| sqlWriterStoredProcedureName |Esse nome é para o procedimento armazenado que define como aplicar dados de origem na tabela de destino. <br/>Este procedimento armazenado é *chamado por lote*. Para fazer uma operação executada apenas uma vez e que não tenha relação alguma com os dados de origem, por exemplo, excluir ou truncar, use a propriedade `preCopyScript`. |Não. |
| storedProcedureParameters |Esses parâmetros são usados para o procedimento armazenado.<br/>Valores permitidos são pares de nome ou valor. Os nomes e o uso de maiúsculas e minúsculas dos parâmetros devem corresponder aos nomes e o uso de maiúsculas e minúsculas dos parâmetros do procedimento armazenado. |Não. |
| sqlWriterTableType |Essa propriedade especifica um nome de tipo de tabela a ser usado no procedimento armazenado. A atividade de cópia disponibiliza aqueles dados sendo movidos em uma tabela temporária com esse tipo de tabela. O código de procedimento armazenado pode mesclar os dados que estão sendo copiados com os dados existentes. |Não. |

**Exemplo 1: anexar dados**

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
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

Saiba mais detalhes em [Invocar um procedimento armazenado de um coletor SQL](#invoking-stored-procedure-for-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSqlMI",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Managed Instance output dataset name>",
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

## <a name="best-practice-for-loading-data-into-azure-sql-database-managed-instance"></a>Prática recomendada para carregar dados no banco de dados de instância gerenciada do SQL

Quando você copia dados para o banco de dados de instância gerenciada do SQL, você pode exigir comportamento diferente de gravação:

- **[Acrescente](#append-data)** : minha fonte de dados só tem novos registros;
- **[Upsert](#upsert-data)** : minha fonte de dados tem inserções e atualizações.
- **[Substituir](#overwrite-entire-table)** : Eu quero recarregue a tabela de dimensões todo cada vez;
- **[Escrever com lógica personalizada](#write-data-with-custom-logic)** : Preciso de processamento extra antes da inserção final na tabela de destino.

Consulte as seções respectivamente sobre como configurar no AAD e as práticas recomendadas.

### <a name="append-data"></a>Acrescentar dados

Esse é o comportamento padrão desse conector de coletor do banco de dados instância gerenciada do SQL, e fazer de ADF **inserção em massa** para gravar em sua tabela com eficiência. Você pode simplesmente configurar a fonte e coletor adequadamente na atividade de cópia.

### <a name="upsert-data"></a>Upsert data

**Opção eu** (sugerido especialmente quando você tiver grandes dados a serem copiados): o **abordagem de alto desempenho a maioria dos** fazer upsert é o seguinte: 

- Em primeiro lugar, aproveite um [tabela temporária](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql?view=sql-server-2017#temporary-tables) para carregamento em massa todos os registros usando a atividade de cópia. Como as operações em relação a tabelas temporárias não são registradas, você pode carregar milhões de registros em segundos.
- Executar uma atividade de procedimento armazenado no ADF para aplicar uma [mesclar](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current) (ou inserir/atualizar) a instrução e use o temp de tabela como fonte para executar todas as atualiza ou insere como uma única transação, reduzindo a quantidade de idas e voltas e operações de log. No final da atividade de procedimento armazenado, a tabela temporária pode ser truncada para estar pronto para o próximo ciclo de upsert. 

Por exemplo, no Azure Data Factory, você pode criar um pipeline com uma **atividade de cópia** encadeado com um **atividade de procedimento armazenado** em caso de sucesso. O primeiro copia dados de seu armazenamento de origem em uma tabela temporária, diga " **##UpsertTempTable**" como nome da tabela no conjunto de dados, em seguida, o último invoca um procedimento armazenado para mesclar dados de origem de tabela temporária na tabela de destino e limpar tabela temporária.

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

Semelhante, conforme descrito em [Upsert dados](#upsert-data) seção, quando você precisar aplicar processamento extra antes da inserção final dos dados de origem na tabela de destino, é possível um) de grande escala, de carga para uma tabela temporária e depois invocar um armazenado procedimento ou b) invocar um procedimento armazenado durante a cópia.

## <a name="invoking-stored-procedure-for-sql-sink"></a> Invocar um procedimento armazenado de um coletor SQL

Quando você copia dados para o banco de dados de instância gerenciada do SQL, você também pode configurar e invocar um procedimento armazenado especificado pelo usuário com parâmetros adicionais.

> [!TIP]
> Invocação de procedimento armazenado processa a dados linha por linha, em vez de operação em massa, o que não é sugerida para cópia de grande escala. Saiba mais sobre [práticas recomendadas para carregar dados no banco de dados de instância gerenciada do SQL](#best-practice-for-loading-data-into-azure-sql-database-managed-instance).

Você pode usar um procedimento armazenado quando mecanismos de cópia internos não têm essa finalidade, por exemplo, aplicar processamento extra antes da inserção final dos dados de origem na tabela de destino. Alguns exemplos de processamento extra são colunas de mesclagem, pesquisa valores adicionais e inserção em mais de uma tabela.

O exemplo a seguir mostra como usar um procedimento armazenado para fazer um upsert em uma tabela no banco de dados do SQL Server. Supondo que os dados de entrada e cada tabela **Marketing** do coletor tenham três colunas: **ProfileID**, **State** e **Category**. Faça o upsert com base na coluna **ProfileID** e aplique-o apenas a uma categoria específica.

**Conjunto de dados de saída:** "tableName" deve ser o mesmo nome de parâmetro de tipo de tabela em seu procedimento armazenado (veja abaixo o script de procedimento armazenado).

```json
{
    "name": "AzureSqlMIDataset",
    "properties":
    {
        "type": "SqlServerTable",
        "linkedServiceName": {
            "referenceName": "<Managed Instance linked service name>",
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

No banco de dados, defina o tipo de tabela com o mesmo nome que sqlWriterTableType. O esquema do tipo de tabela é o mesmo que o esquema retornado pelos dados de entrada.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL，
    [Category] [varchar](256) NOT NULL
)
```

O recurso de procedimento armazenado se beneficia de [parâmetros com valores de tabela](https://msdn.microsoft.com/library/bb675163.aspx).

## <a name="data-type-mapping-for-azure-sql-database-managed-instance"></a>Mapeamento do tipo de dados da Instância Gerenciada do Banco de Dados SQL do Azure

Quando dados são copiados para e da Instância Gerenciada do Banco de Dados SQL do Azure, os seguintes mapeamentos são usados de tipos de dados da Instância Gerenciada do Banco de Dados SQL do Azure para tipos de dados provisórios do Azure Data Factory. Para saber mais sobre como a atividade de cópia mapeia do tipo de dados e esquema de origem para o coletor, confira [Mapeamentos de tipo de dados e esquema](copy-activity-schema-and-type-mapping.md).

| Tipo de dados da Instância Gerenciada do Banco de Dados SQL do Azure | Tipo de dados provisório do Azure Data Factory |
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
| tinyint |Int16 |
| uniqueidentifier |Guid |
| varbinary |Byte[] |
| varchar |String, Char[] |
| xml |Xml |

>[!NOTE]
> Para tipos de dados que mapeiam para o tipo provisório Decimal, no momento, o Azure Data Factory dá suporte à precisão de até 28. Se você tiver dados que exijam precisão maior que 28, considere converter para uma cadeia de caracteres em uma consulta SQL.

## <a name="next-steps"></a>Próximas etapas
Para obter uma lista de armazenamentos de dados com suporte como origens e coletores pela atividade de cópia no Azure Data Factory, confira [Armazenamentos de dados com suporte](copy-activity-overview.md##supported-data-stores-and-formats).
