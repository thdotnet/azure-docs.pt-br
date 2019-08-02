---
title: Copiar dados para ou do Azure Data Explorer usando o Azure Data Factory
description: Saiba como copiar dados de ou para o Azure Data Explorer usando uma atividade de cópia em um pipeline do Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/01/2019
ms.author: orspodek
ms.openlocfilehash: a7ac0bdc2bd5eed802f6959a628dee4c8141dbd1
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68720798"
---
# <a name="copy-data-to-or-from-azure-data-explorer-using-azure-data-factory"></a>Copiar dados para ou do Azure Data Explorer usando o Azure Data Factory

Este artigo descreve como usar a atividade de cópia no Azure Data Factory para copiar dados de ou para o [Azure Data Explorer](../data-explorer/data-explorer-overview.md). Ele amplia o artigo [Visão geral da atividade de cópia](copy-activity-overview.md) que apresenta uma visão geral da atividade de cópia.

## <a name="supported-capabilities"></a>Funcionalidades com suporte

Você pode copiar dados de qualquer armazenamento de dados de origem com suporte para o Azure Data Explorer. Você também pode copiar dados do Azure Data Explorer para qualquer armazenamento de dados de coletor com suporte. Para obter uma lista de armazenamentos de dados que têm suporte como fontes ou coletores da atividade de cópia, confira a tabela [Armazenamentos de dados com suporte](copy-activity-overview.md).

>[!NOTE]
>A cópia de dados de/para o Azure Data Explorer de/para o armazenamento de dados local usando o Integration Runtime de hospedagem interna tem suporte desde a versão 3,14.

O conector do Azure Data Explorer permite que você faça o seguinte:

* Copiar dados usando a autenticação de token de aplicativo do Azure AD (Azure Active Directory) com uma **entidade de serviço**.
* Como uma fonte, recupere dados usando uma consulta KQL (Kusto).
* Como um coletor, acrescente dados a uma tabela de destino.

## <a name="getting-started"></a>Introdução

>[!TIP]
>Para obter uma explicação sobre como usar o conector de Data Explorer do Azure, consulte [copiar dados de/para o azure data Explorer usando o Azure data Factory](../data-explorer/data-factory-load-data.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As seções que se seguem fornecem detalhes sobre as propriedades que são usadas para definir entidades do Data Factory específicas ao conector do Azure Data Explorer.

## <a name="linked-service-properties"></a>Propriedades do serviço vinculado

O conector de Data Explorer do Azure usa a autenticação de entidade de serviço. Siga estas etapas para obter uma entidade de serviço e conceder permissões:

1. Registre uma entidade de aplicativo no Azure Active Directory (Azure AD) seguindo [Registre o aplicativo com um locatário do Azure AD](../storage/common/storage-auth-aad-app.md#register-your-application-with-an-azure-ad-tenant). Anote os seguintes valores, que são usados para definir o serviço vinculado:

    - ID do Aplicativo
    - Chave do aplicativo
    - ID do locatário

2. Conceda a permissão apropriada à entidade de serviço no Data Explorer do Azure. Consulte [gerenciar as permissões do banco de dados do Azure data Explorer](../data-explorer/manage-database-permissions.md) com informações detalhadas sobre funções e permissões, bem como instruções sobre como gerenciar permissões. Em geral, você precisa

    - **Como fonte**, conceda pelo menos a função de **Visualizador de banco de dados** ao seu banco de dados.
    - **Como coletor**, conceda pelo menos a função de ingestão de **banco de dados** ao seu banco de dados.

>[!NOTE]
>Ao usar a IU do ADF para criar, sua conta de usuário de logon é usada para listar clusters, bancos de dados e tabelas do Azure Data Explorer. Insira manualmente o nome se você não tiver permissão para essa operação.

As propriedades a seguir têm suporte no serviço vinculado do Azure Data Explorer:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| type | A propriedade **tipo** deve ser definida como **AzureDataExplorer** | Sim |
| ponto de extremidade | URL de ponto de extremidade do cluster do Azure Data Explorer com o formato `https://<clusterName>.<regionName>.kusto.windows.net`. | Sim |
| database | Nome do banco de dados. | Sim |
| tenant | Especifique as informações de locatário (domínio nome ou ID do Locatário) em que o aplicativo reside. Isso é o que você normalmente sabe como "**ID de autoridade**" na cadeia de [conexão Kusto](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties). Recupere-o focalizando com o mouse no canto superior direito do portal do Azure. | Sim |
| servicePrincipalId | Especifique a ID do cliente do aplicativo. Isso é o que você normalmente sabe como "**ID do cliente do aplicativo do AAD**" na cadeia de [conexão Kusto](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties). | Sim |
| servicePrincipalKey | Especifique a chave do aplicativo. Isso é o que você normalmente sabe como "**chave de aplicativo do AAD**" na cadeia de [conexão Kusto](https://docs.microsoft.com/azure/kusto/api/connection-strings/kusto#application-authentication-properties). Marque esse campo como **SecureString** para armazená-lo com segurança no Data Factory ou [referencie um segredo armazenado no Cofre de Chaves do Azure](store-credentials-in-key-vault.md). | Sim |

**Exemplo de Propriedades do Serviço Vinculado:**

```json
{
    "name": "AzureDataExplorerLinkedService",
    "properties": {
        "type": "AzureDataExplorer",
        "typeProperties": {
            "endpoint": "https://<clusterName>.<regionName>.kusto.windows.net ",
            "database": "<database name>",
            "tenant": "<tenant name/id e.g. microsoft.onmicrosoft.com>",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para obter uma lista completa das seções e propriedades disponíveis para definir os conjuntos de dados, confira o artigo sobre [conjuntos de dados](concepts-datasets-linked-services.md). esta seção apresenta uma lista de propriedades compatíveis com o conjunto de dados do Azure Data Explorer.

Para copiar dados para o Azure Data Explorer, defina a propriedade type do conjunto de dados como **AzureDataExplorerTable**.

Há suporte para as seguintes propriedades:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| type | A propriedade **type** deve ser definida como **AzureDataExplorerTable** | Sim |
| table | O nome da tabela à qual o serviço vinculado se refere. | Não para coletor; não para fonte |

**Exemplo de Propriedades do Conjunto de Dados**

```json
{
   "name": "AzureDataExplorerDataset",
    "properties": {
        "type": "AzureDataExplorerTable",
        "typeProperties": {
            "table": "<table name>"
        },
        "schema": [],
        "linkedServiceName": {
            "referenceName": "<Azure Data Explorer linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade de cópia

Para obter uma lista completa das seções e propriedades disponíveis para definir atividades, confia o artigo [Pipelines](concepts-pipelines-activities.md). Esta seção fornece uma lista das propriedades compatíveis com a fonte e o coletor do Azure Data Explorer.

### <a name="azure-data-explorer-as-source"></a>Azure Data Explorer como fonte

Para copiar dados do Azure Data Explorer, defina a propriedade **type** na fonte da atividade de Cópia como **AzureDataExplorerSource**. As propriedades a seguir têm suporte na seção **source** da atividade de cópia:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| type | A propriedade **type** da fonte da atividade de cópia deve ser definida como: **AzureDataExplorerSource** | Sim |
| consultar | Uma solicitação somente leitura fornecida em um [formato KQL](/azure/kusto/query/). Use a consulta KQL personalizada como referência. | Sim |
| queryTimeout | O tempo de espera antes que a solicitação de consulta expire. O valor padrão é 10 min (00:10:00); o valor máximo permitido é 1 hora (01: 00:00). | Não |
| notruncamento | Indica se o conjunto de resultados retornado deve ser truncado. Por padrão, o resultado é truncado após 500.000 registros ou 64MB. O truncamento é altamente recomendado para um comportamento adequado da atividade. |Não |

>[!NOTE]
>Por padrão, a fonte de Data Explorer do Azure tem um limite de tamanho de 500.000 registros ou 64 MB. Para recuperar todos os registros sem truncamento, você pode especificar `set notruncation;` no início da consulta. Consulte [os limites de consulta](https://docs.microsoft.com/azure/kusto/concepts/querylimits) para obter mais detalhes.

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "AzureDataExplorerSource",
                "query": "TestTable1 | take 10",
                "queryTimeout": "00:10:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        },
        "inputs": [
            {
                "referenceName": "<Azure Data Explorer input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

### <a name="azure-data-explorer-as-sink"></a>Azure Data Explorer como coletor

Para copiar dados do Azure Data Explorer, defina a propriedade type no coletor da atividade de cópia como **AzureDataExplorerSink**. As propriedades a seguir têm suporte na seção **sink** da atividade de cópia:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| type | A propriedade **type** do coletor da atividade de cópia deve ser definida como: **AzureDataExplorerSink** | Sim |
| ingestionMappingName | Nome de um **[mapeamento](/azure/kusto/management/mappings#csv-mapping)** criado previamente em uma tabela Kusto. Para mapear as colunas da origem para o Azure Data Explorer-que se aplica a **[todos os repositórios/formatos de origem com suporte](copy-activity-overview.md#supported-data-stores-and-formats)** , incluindo os formatos CSV/JSON/Avro, etc., você pode usar o [mapeamento de coluna](copy-activity-schema-and-type-mapping.md) da atividade de cópia (implicitamente por nome ou explicitamente como configurado) e /ou mapeamentos de Data Explorer do Azure. | Não |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyToAzureDataExplorer",
        "type": "Copy",
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataExplorerSink",
                "ingestionMappingName": "<optional Azure Data Explorer mapping name>"
            }
        },
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Data Explorer output dataset name>",
                "type": "DatasetReference"
            }
        ]
    }
]
```

## <a name="next-steps"></a>Próximas etapas

* Para obter uma lista de armazenamentos de dados com suporte como origens e coletores pela atividade de cópia no Azure Data Factory, consulte [Armazenamentos de dados com suporte](copy-activity-overview.md#supported-data-stores-and-formats).

* Saiba mais sobre como [copiar dados de Azure data Factory para o data Explorer do Azure](/azure/data-explorer/data-factory-load-data).