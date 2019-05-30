---
title: Copiar dados da tabela de SAP usando o Azure Data Factory | Microsoft Docs
description: Saiba como copiar dados da tabela de SAP para armazenamentos de dados de coletor com suporte usando uma atividade de cópia em um pipeline do Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/24/2018
ms.author: jingwang
ms.openlocfilehash: 4dee0e994c9e7be9677a8f1051481850990998e9
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247163"
---
# <a name="copy-data-from-sap-table-using-azure-data-factory"></a>Copiar dados da tabela de SAP usando o Azure Data Factory

Este artigo descreve como usar a atividade de cópia no Azure Data Factory para copiar dados de uma tabela de SAP. Ele amplia o artigo [Visão geral da atividade de cópia](copy-activity-overview.md) que apresenta uma visão geral da atividade de cópia.

## <a name="supported-capabilities"></a>Funcionalidades com suporte

Você pode copiar dados da tabela de SAP para qualquer armazenamento de dados de coletor com suporte. Para obter uma lista de armazenamentos de dados com suporte como origens/coletores da atividade de cópia, confira a tabela [Armazenamentos de dados com suporte](copy-activity-overview.md#supported-data-stores-and-formats).

Especificamente, este conector de tabela SAP dá suporte a:

- Copiar dados da tabela de SAP no **SAP Business Suite com a versão mais alta ou 7.01** (em uma recente SAP suporte pacote pilha liberadas após o ano de 2015) ou **4hana/s**.
- Copiando dados de ambos **tabela transparentes SAP** e **exibição**.
- Cópia de dados usando **autenticação básica** ou **SNC** (proteger comunicações de rede) se SNC está configurado.
- Conectar-se ao **servidor de aplicativos** ou **do servidor de mensagens**.

## <a name="prerequisites"></a>Pré-requisitos

Para usar este conector de tabela de SAP, você precisa:

- Configure um Integration Runtime auto-hospedado com a versão 3.17 ou acima. Consulte o artigo [Self-hosted integration runtime](create-self-hosted-integration-runtime.md) (Integration Runtime auto-hospedado) para obter detalhes.

- Baixar a versão de **64 bits do [SAP .NET Connector 3.0](https://support.sap.com/en/product/connectors/msnet.html)** do site da SAP e instalá-la no computador de IR auto-hospedado. Quando a instalação, na janela de etapas de instalação opcional, verifique se você selecionou o **instalar Assemblies ao GAC** opção. 

    ![Instalar o SAP .NET Connector](./media/connector-sap-business-warehouse-open-hub/install-sap-dotnet-connector.png)

- Usuário do SAP que está sendo usado no conector do SAP tabela da fábrica de dados precisa ter as seguintes permissões: 

    - Autorização para RFC. 
    - Permissões para a atividade do objeto de autorização "S_SDSAUTH" de "Execução".

## <a name="getting-started"></a>Introdução

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As seções a seguir fornecem detalhes sobre as propriedades usadas para definir entidades do Data Factory específicas ao conector de tabela SAP.

## <a name="linked-service-properties"></a>Propriedades do serviço vinculado

As propriedades a seguir têm suporte no serviço vinculado do SAP BW Open Hub (Business Warehouse):

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| type | A propriedade type deve ser definida como: **SapTable** | Sim |
| Servidor | Nome do servidor no qual reside a instância do SAP.<br/>Aplicável se você quiser se conectar ao **servidor de aplicativos SAP**. | Não  |
| systemNumber | Número do sistema do sistema SAP.<br/>Aplicável se você quiser se conectar ao **servidor de aplicativos SAP**.<br/>Valor permitido: número decimal de dois dígitos representado como uma cadeia de caracteres. | Não  |
| messageServer | Nome do host do servidor de mensagens SAP.<br/>Aplicável se você quiser se conectar ao **servidor de mensagens SAP**. | Não  |
| messageServerService | O nome do serviço ou a porta número do servidor de mensagens.<br/>Aplicável se você quiser se conectar ao **servidor de mensagens SAP**. | Não |
| systemId | ID do sistema do sistema SAP em que a tabela está localizada.<br/>Aplicável se você quiser se conectar ao **servidor de mensagens SAP**. | Não |
| logonGroup | O grupo de Logon para o sistema SAP.<br/>Aplicável se você quiser se conectar ao **servidor de mensagens SAP**. | Não  |
| clientId | ID do cliente do cliente no sistema SAP.<br/>Valor permitido: número decimal de três dígitos representado como uma cadeia de caracteres. | Sim |
| language | Idioma que o sistema SAP usa. | Não (o valor padrão é **EN**)|
| userName | Nome do usuário que tem acesso ao servidor SAP. | Sim |
| Senha | Senha do usuário. Marque este campo como uma SecureString para armazená-la com segurança no Data Factory ou [faça referência a um segredo armazenado no Azure Key Vault](store-credentials-in-key-vault.md). | Sim |
| sncMode | Indicador de ativação SNC para acessar o servidor SAP em que a tabela está localizada.<br/>Aplicável se você quiser usar SNC para se conectar ao servidor SAP.<br/>Valores permitidos são: **0** (desativado, padrão) ou **1** (ativado). | Não |
| sncMyName | Nome SNC do iniciador para acessar o servidor SAP em que a tabela está localizada.<br/>Aplicável quando `sncMode` está em. | Não  |
| sncPartnerName | Comunicação SNC nome do parceiro para acessar o servidor SAP em que a tabela está localizada.<br/>Aplicável quando `sncMode` está em. | Não |
| sncLibraryPath | Biblioteca do produto de segurança externas para acessar o servidor SAP onde está localizada a tabela.<br/>Aplicável quando `sncMode` está em. | Não |
| sncQop | O SNC qualidade de proteção.<br/>Aplicável quando `sncMode` está em. <br/>Valores permitidos são: **1** (autenticação), **2** (integridade), **3** (privacidade) **8** (padrão), **9** (máximo). | Não  |
| connectVia | O [Integration Runtime](concepts-integration-runtime.md) a ser usado para se conectar ao armazenamento de dados. É necessário um Integration Runtime auto-hospedado, conforme mencionado nos [Pré-requisitos](#prerequisites). |Sim |

**Exemplo 1: conectar-se ao servidor de aplicativos SAP**

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Exemplo 2: conectar-se ao servidor de mensagens SAP**

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "messageServer": "<message server name>",
            "messageServerService": "<service name or port>",
            "systemId": "<system id>",
            "logonGroup": "<logon group>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Exemplo 3: conectar-se usando SNC**

```json
{
    "name": "SapTableLinkedService",
    "properties": {
        "type": "SapTable",
        "typeProperties": {
            "server": "<server name>",
            "systemNumber": "<system number>",
            "clientId": "<client id>",
            "userName": "<SAP user>",
            "password": {
                "type": "SecureString",
                "value": "<Password for SAP user>"
            },
            "sncMode": 1,
            "sncMyName": "snc myname",
            "sncPartnerName": "snc partner name",
            "sncLibraryPath": "snc library path",
            "sncQop": "8"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para obter uma lista completa das seções e propriedades disponíveis para definir os conjuntos de dados, confira o artigo sobre [Conjuntos de Dados](concepts-datasets-linked-services.md). Esta seção fornece uma lista das propriedades com suporte pelo conjunto de dados de tabela SAP.

Para copiar dados de e para o Hub aberto do SAP BW, há suporte para as propriedades a seguir.

| Propriedade | DESCRIÇÃO | Obrigatório |
|:--- |:--- |:--- |
| type | A propriedade type deve ser definida como **SapTableResource**. | Sim |
| tableName | O nome da tabela para copiar dados do SAP. | Sim |

**Exemplo:**

```json
{
    "name": "SAPTableDataset",
    "properties": {
        "type": "SapTableResource",
        "linkedServiceName": {
            "referenceName": "<SAP Table linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "<SAP table name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade de cópia

Para obter uma lista completa das seções e propriedades disponíveis para definir atividades, confia o artigo [Pipelines](concepts-pipelines-activities.md). Esta seção fornece uma lista das propriedades com suporte pela fonte de tabela SAP.

### <a name="sap-table-as-source"></a>Tabela SAP como origem

Para copiar dados da tabela de SAP, há suporte para as propriedades a seguir.

| Propriedade                         | DESCRIÇÃO                                                  | Obrigatório |
| :------------------------------- | :----------------------------------------------------------- | :------- |
| type                             | A propriedade type deve ser definida como **SapTableSource**.       | Sim      |
| rowCount                         | Número de linhas a serem recuperadas.                              | Não       |
| rfcTableFields                   | Campos a serem copiados da tabela de SAP. Por exemplo: `column0, column1`. | Não        |
| rfcTableOptions                  | Opções para filtrar as linhas na tabela de SAP. Por exemplo: `COLUMN0 EQ 'SOMEVALUE'`. | Não        |
| customRfcReadTableFunctionModule | Módulo personalizado de função RFC que pode ser usado para ler dados da tabela de SAP. | Não        |
| partitionOption                  | O mecanismo de partição para ler da tabela SAP. As opções com suporte incluem: <br/>- **None**<br/>- **PartitionOnInt** (inteiro normal ou valores inteiros com preenchimento de zero à esquerda, como 0000012345)<br/>- **PartitionOnCalendarYear** (4 dígitos no formato "Aaaa")<br/>- **PartitionOnCalendarMonth** (6 dígitos no formato "YYYYMM")<br/>- **PartitionOnCalendarDate** (8 dígitos no formato "AAAAMMDD") | Não        |
| partitionColumnName              | O nome da coluna para particionar os dados. | Não        |
| partitionUpperBound              | O valor máximo da coluna especificada em `partitionColumnName` que será usado para o particionamento de continuar. | Não        |
| partitionLowerBound              | O valor mínimo da coluna especificada em `partitionColumnName` que será usado para o particionamento de continuar. | Não        |
| maxPartitionsNumber              | O número máximo de partições para dividir os dados. | Não        |

>[!TIP]
>- Se sua tabela SAP tem um grande volume de dados, como vários bilhões de linhas, use `partitionOption` e `partitionSetting` para dividir os dados em partições pequenas, caso em que os dados são lidos por partições e cada partição de dados é recuperado do servidor SAP por meio de um único Chamada RFC.<br/>
>- Levando `partitionOption` como `partitionOnInt` por exemplo, o número de linhas em cada partição é calculado por (total de linhas incluído entre *partitionUpperBound* e *partitionLowerBound*) /*maxPartitionsNumber*.<br/>
>- Se você quiser executar ainda mais as partições em paralelo para acelerar a cópia, é altamente recomendável fazer `maxPartitionsNumber` como um múltiplo do valor da `parallelCopies` (Saiba mais sobre [cópia paralela](copy-activity-performance.md#parallel-copy)).

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromSAPTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<SAP Table input dataset name>",
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
                "type": "SapTableSource",
                "partitionOption": "PartitionOnInt",
                "partitionSettings": {
                     "partitionColumnName": "<partition column name>",
                     "partitionUpperBound": "2000",
                     "partitionLowerBound": "1",
                     "maxPartitionsNumber": 500
                 }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-sap-table"></a>Mapeamento de tipo de dados para a tabela SAP

Ao copiar dados da tabela de SAP, os seguintes mapeamentos são usados de tipos de dados de tabela SAP para tipos de dados provisórios do Azure Data Factory. Consulte [Mapeamentos de tipo de dados e esquema](copy-activity-schema-and-type-mapping.md) para saber mais sobre como a atividade de cópia mapeia o tipo de dados e esquema de origem para o coletor.

| Tipo SAP ABAP | Tipo de dados provisório do Data Factory |
|:--- |:--- |
| C (Cadeia de caracteres) | Cadeia de caracteres |
| Eu (Integer) | Int32 |
| F (Flutuante) | Double |
| D (Data) | Cadeia de caracteres |
| T (Hora) | Cadeia de caracteres |
| P (BCD Empacotado, Moeda, Decimal, Qtd) | Decimal |
| N (numérico) | Cadeia de caracteres |
| X (Binário e Bruto) | Cadeia de caracteres |

## <a name="next-steps"></a>Próximas etapas
Para obter uma lista de armazenamentos de dados com suporte como origens e coletores pela atividade de cópia no Azure Data Factory, consulte [Armazenamentos de dados com suporte](copy-activity-overview.md#supported-data-stores-and-formats).
