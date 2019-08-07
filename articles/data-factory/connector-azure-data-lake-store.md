---
title: Copiar dados de ou para Azure Data Lake Storage Gen1 usando Data Factory | Microsoft Docs
description: Saiba como copiar dados de armazenamentos de dados de origem com suporte para o Azure Data Lake Store ou do Data Lake Store para armazenamentos de coletor com suporte usando o Data Factory.
services: data-factory
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: jingwang
ms.openlocfilehash: e0626d847b22c11ce5acca5633c9b1291c03742d
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68839862"
---
# <a name="copy-data-to-or-from-azure-data-lake-storage-gen1-using-azure-data-factory"></a>Copiar dados de ou para Azure Data Lake Storage Gen1 usando Azure Data Factory
> [!div class="op_single_selector" title1="Selecione a versão do Azure Data Factory que você está usando:"]
> * [Versão 1](v1/data-factory-azure-datalake-connector.md)
> * [Versão atual](connector-azure-data-lake-store.md)

Este artigo descreve como copiar dados de e para Azure Data Lake Storage Gen1. Para saber mais sobre o Azure Data Factory, leia as [artigo introdutório](introduction.md).

## <a name="supported-capabilities"></a>Funcionalidades com suporte

Este conector de Azure Data Lake Storage Gen1 tem suporte para as seguintes atividades:

- [Atividade de cópia](copy-activity-overview.md) com a [origem com suporte ou a matriz de coletor](copy-activity-overview.md)
- [Mapeando fluxo de dados](concepts-data-flow-overview.md)
- [Atividade de pesquisa](control-flow-lookup-activity.md)
- [Atividade GetMetadata](control-flow-get-metadata-activity.md)

Especificamente, com esse conector, você pode:

- Copie arquivos usando um dos seguintes métodos de autenticação: entidade de serviço ou identidades gerenciadas para recursos do Azure.
- Copie os arquivos como estão ou analise ou gere arquivos com os [formatos de arquivo e codecs de compactação com suporte](supported-file-formats-and-compression-codecs.md).

> [!IMPORTANT]
> Se você copiar dados usando o tempo de execução de integração auto-hospedado, configure o firewall corporativo para permitir o `<ADLS account name>.azuredatalakestore.net` tráfego `login.microsoftonline.com/<tenant>/oauth2/token` de saída para e na porta 443. O último é o Serviço de Token de Segurança do Azure com que o tempo de execução de integração precisa se comunicar para obter o token de acesso.

## <a name="get-started"></a>Introdução

> [!TIP]
> Para obter um passo a passo de como usar o conector de Azure Data Lake Store, consulte [carregar dados em Azure data Lake Store](load-azure-data-lake-store.md).

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

As seções a seguir fornecem informações sobre as propriedades que são usadas para definir Data Factory entidades específicas para Azure Data Lake Store.

## <a name="linked-service-properties"></a>Propriedades do serviço vinculado

As propriedades a seguir têm suporte no serviço vinculado do Azure Data Lake Store:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| type | A propriedade `type` deve ser definida como: **AzureDataLakeStore**. | Sim |
| dataLakeStoreUri | Informações sobre a conta do Azure Data Lake Store. Essas informações usam um dos seguintes formatos: `https://[accountname].azuredatalakestore.net/webhdfs/v1` ou `adl://[accountname].azuredatalakestore.net/`. | Sim |
| subscriptionId | A ID de assinatura do Azure à qual a conta do Data Lake Store pertence. | Obrigatório para coletor |
| resourceGroupName | O nome do grupo de recursos do Azure ao qual a conta do Data Lake Store pertence. | Obrigatório para coletor |
| connectVia | O [Integration Runtime](concepts-integration-runtime.md) a ser usado para se conectar ao armazenamento de dados. Você pode usar o tempo de execução de integração do Azure ou um tempo de execução de integração auto-hospedado se o armazenamento de dados estiver localizado em uma rede privada. Se essa propriedade não for especificada, o tempo de execução de integração do Azure padrão será usado. |Não |

### <a name="use-service-principal-authentication"></a>Usar a autenticação de entidade de serviço

Para usar a autenticação de entidade de serviço, registre uma entidade de aplicativo no Azure Active Directory e conceda a ela o acesso ao Data Lake Store. Para encontrar as etapas detalhadas, consulte [Autenticação de serviço a serviço](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Anote os seguintes valores, que são usados para definir o serviço vinculado:

- ID do Aplicativo
- Chave do aplicativo
- ID do locatário

>[!IMPORTANT]
> Conceda a permissão apropriada à entidade de serviço no Data Lake Store:
>- **Como origem**: Em **Data Explorer** > **Acesso**, dê pelo menos uma permissão de **Leitura + Execução** para listar e copiar os arquivos em pastas e subpastas. Outra opção é conceder permissão de **Leitura** para copiar um único arquivo. Você pode optar por adicionar a **Esta pasta e todos os filhos** para recursivo e adicionar como **uma permissão de acesso e uma entrada de permissão padrão**. Não há nenhum requisito no controle de acesso no nível da conta (IAM).
>- **Como coletor**: Em **Data Explorer** > **Acesso**, dê pelo menos uma permissão **Gravação + Execução** para criar itens filho na pasta. Você pode optar por adicionar a **Esta pasta e todos os filhos** para recursivo e adicionar como **uma permissão de acesso e uma entrada de permissão padrão**. Se você usar um tempo de execução de integração do Azure para copiar (tanto a origem quanto o coletor estão na nuvem), em IAM, conceda pelo menos a função **leitor** para permitir que data Factory detecte a região para data Lake Store. Se você quiser evitar essa função IAM, escolha explicitamente [criar um Azure Integration Runtime](create-azure-integration-runtime.md#create-azure-ir) com o local do Data Lake Store. Por exemplo, se o Data Lake Store estiver em Europa Ocidental, crie um tempo de execução de integração do Azure com o local definido como "Europa Ocidental". Associe-os no serviço vinculado Data Lake Store, conforme mostrado no exemplo a seguir.

>[!NOTE]
>Para listar as pastas começando na raiz, você deve definir a permissão da entidade de serviço que está sendo concedida **no nível raiz com a permissão "Executar"** . Isso é verdadeiro quando você usa o:
>- **Ferramenta copiar dados** para criar pipeline de cópia.
>- **Interface do Usuário do Data Factory** para testar a conexão e navegar por pastas durante a criação.
>Se você tiver preocupações sobre a concessão de permissão no nível da raiz, durante a criação, ignorar a conexão de teste e inserir um caminho paraent com a permissão concedida, escolha procurar o caminho especificado. A atividade de cópia funciona contanto que a entidade de serviço seja concedida com a permissão adequada nos arquivos a serem copiados.

Há suporte para as seguintes propriedades:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| servicePrincipalId | Especifique a ID do cliente do aplicativo. | Sim |
| servicePrincipalKey | Especifique a chave do aplicativo. Marque este campo como uma `SecureString` para armazená-la com segurança no Data Factory ou [faça referência a um segredo armazenado no Azure Key Vault](store-credentials-in-key-vault.md). | Sim |
| tenant | Especifique as informações de locatário, como nome de domínio ou ID de locatário, sob a qual seu aplicativo reside. É possível recuperá-las focalizando o mouse no canto superior direito do Portal do Azure. | Sim |

**Exemplo:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identity"></a> Usar identidades gerenciadas para autenticação de recursos do Azure

Um data factory pode ser associado a uma [identidade gerenciada para recursos do Azure](data-factory-service-identity.md), que representa esse data factory específico. Você pode usar essa identidade gerenciada diretamente para Data Lake Store autenticação, semelhante ao uso de sua própria entidade de serviço. Ele permite que este alocador designado acesse e copie dados de ou para o seu Data Lake Store.

Para usar identidades gerenciadas para autenticação de recursos do Azure:

1. [Recupere o data Factory informações de identidade gerenciadas](data-factory-service-identity.md#retrieve-managed-identity) copiando o valor da "ID do aplicativo de identidade de serviço" gerado junto com sua fábrica.
2. Conceda acesso de identidade gerenciada a Data Lake Store, da mesma maneira que você faz para a entidade de serviço, seguindo estas notas.

>[!IMPORTANT]
> Certifique-se de conceder a permissão apropriada de identidade gerenciada data factory no Data Lake Store:
>- **Como origem**: Em **Data Explorer** > **Acesso**, dê pelo menos uma permissão de **Leitura + Execução** para listar e copiar os arquivos em pastas e subpastas. Outra opção é conceder permissão de **Leitura** para copiar um único arquivo. Você pode optar por adicionar a **Esta pasta e todos os filhos** para recursivo e adicionar como **uma permissão de acesso e uma entrada de permissão padrão**. Não há nenhum requisito no controle de acesso no nível da conta (IAM).
>- **Como coletor**: Em **Data Explorer** > **Acesso**, dê pelo menos uma permissão **Gravação + Execução** para criar itens filho na pasta. Você pode optar por adicionar a **Esta pasta e todos os filhos** para recursivo e adicionar como **uma permissão de acesso e uma entrada de permissão padrão**. Se você usar um tempo de execução de integração do Azure para copiar (tanto a origem quanto o coletor estão na nuvem), em IAM, conceda pelo menos a função **leitor** para permitir que data Factory detecte a região para data Lake Store. Se você quiser evitar essa função IAM, escolha explicitamente [criar um Azure Integration Runtime](create-azure-integration-runtime.md#create-azure-ir) com o local do Data Lake Store. Associe-os no serviço vinculado Data Lake Store, conforme mostrado no exemplo a seguir.

>[!NOTE]
>Para listar pastas a partir da raiz, você deve definir a permissão da identidade gerenciada que está sendo concedida **no nível raiz com a permissão "executar"** . Isso é verdadeiro quando você usa o:
>- **Ferramenta copiar dados** para criar pipeline de cópia.
>- **Interface do Usuário do Data Factory** para testar a conexão e navegar por pastas durante a criação.
>Se você tiver preocupações sobre a concessão de permissão no nível da raiz, durante a criação, ignorar a conexão de teste e inserir um caminho pai com a permissão concedida, escolha procurar o caminho especificado. A atividade de cópia funciona contanto que a entidade de serviço seja concedida com a permissão adequada nos arquivos a serem copiados.

No Azure Data Factory, você não precisa especificar nenhuma propriedade além das informações do Data Lake Store gerais no serviço vinculado.

**Exemplo:**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propriedades do conjunto de dados

Para obter uma lista completa das seções e propriedades disponíveis para definir os conjuntos de dados, confira o artigo sobre [Conjuntos de Dados](concepts-datasets-linked-services.md). 

- Para **parquet, texto delimitado e formato binário**, consulte a seção [parquet, texto delimitado e o conjunto de DataSet de formato binário](#format-based-dataset) .
- Para outros formatos, como o **formato Orc/Avro/JSON**, consulte outra seção do [conjunto](#other-format-dataset) de exemplo.

### <a name="format-based-dataset"></a>Parquet, texto delimitado e conjunto de DataSet de formato binário

Para copiar dados de e para **parquet, texto delimitado ou formato binário**, consulte [formato de parquet](format-parquet.md), [formato de texto delimitado](format-delimited-text.md) e artigo de [formato binário](format-binary.md) em conjunto de dados com base em formato e configurações com suporte.
As propriedades a seguir têm suporte para Azure data Lake Store Gen1 `location` em configurações no conjunto de base de formato:

| Propriedade   | Descrição                                                  | Necessário |
| ---------- | ------------------------------------------------------------ | -------- |
| type       | A propriedade Type em `location` no DataSet deve ser definida como **AzureDataLakeStoreLocation**. | Sim      |
| folderPath | O caminho para uma pasta. Se você quiser usar um curinga para filtrar pastas, ignore essa configuração e especifique-a nas configurações de origem da atividade. | Não       |
| fileName   | O nome do arquivo sob o folderPath fornecido. Se você quiser usar um curinga para filtrar arquivos, ignore essa configuração e especifique-a nas configurações de origem da atividade. | Não       |

> [!NOTE]
>
> O conjunto de **AzureDataLakeStoreFile** do tipo parquet ou o formato de texto mencionado na seção a seguir ainda tem suporte como é para a atividade de cópia, pesquisa e GetMetadata para compatibilidade com versões anteriores. Mas ele não funciona com o recurso de fluxo de dados de mapeamento. É recomendável que você use esse novo modelo no futuro. A interface do usuário de criação de Data Factory gera esses novos tipos.

**Exemplo:**

```json
{
    "name": "DelimitedTextDataset",
    "properties": {
        "type": "DelimitedText",
        "linkedServiceName": {
            "referenceName": "<ADLS Gen1 linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, auto retrieved during authoring > ],
        "typeProperties": {
            "location": {
                "type": "AzureDataLakeStoreLocation",
                "folderPath": "root/folder/subfolder"
            },
            "columnDelimiter": ",",
            "quoteChar": "\"",
            "firstRowAsHeader": true,
            "compressionCodec": "gzip"
        }
    }
}
```

### <a name="other-format-dataset"></a>Outro conjunto de DataSet de formato

Para copiar dados de e para Azure Data Lake Store Gen1 no **formato Orc/Avro/JSON**, há suporte para as seguintes propriedades:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| type | A propriedade Type do conjunto de conjuntos deve ser definida como **AzureDataLakeStoreFile**. |Sim |
| folderPath | Caminho para a pasta no Data Lake Store. Se não especificado, apontará para a raiz. <br/><br/>Há suporte para o filtro curinga. Curingas permitidos são `*` (corresponde a zero ou mais caracteres) `?` e (corresponde a zero ou a um único caractere). Use `^` para escapar se o nome real da pasta tiver um curinga ou esse caractere de escape dentro. <br/><br/>Por exemplo: RootFolder/subfolder/. Veja mais exemplos em [Exemplos de filtro de pastas e arquivos](#folder-and-file-filter-examples). |Não |
| fileName | Filtro de nome ou curinga para os arquivos sob o "folderPath" especificado. Se você não especificar um valor para essa propriedade, o conjunto de dados apontará para todos os arquivos na pasta. <br/><br/>Para o filtro, os curingas permitidos `*` são (corresponde a zero ou mais caracteres `?` ) e (corresponde a zero ou a um único caractere).<br/>– Exemplo 1: `"fileName": "*.csv"`<br/>– Exemplo 2: `"fileName": "???20180427.txt"`<br/>Use `^` para escapar se o nome de arquivo real tiver um curinga ou este caractere de escape dentro de.<br/><br/>Quando fileName não for especificado para um conjunto de dados de saída e **preserveHierarchy** não for especificada no coletor de atividade, a atividade de cópia gerará automaticamente o nome do arquivo com o seguinte padrão: "*Dados. [GUID de ID de execução de atividade]. [GUID If FlattenHierarchy]. [Formatar se configurado]. [compactação se configurada]* ", por exemplo," Data. 0a405f8a-93ff-4c6f-b3be-f69616f1df7a. txt. gz ". Se você copiar de uma fonte de tabela usando um nome de tabela em vez de uma consulta, o padrão de nome será " *[nome da tabela]. [ formato]. [compactação se configurada]* ", por exemplo," MyTable. csv ". |Não |
| modifiedDatetimeStart | Filtro de arquivos com base no atributo modificado pela última vez. Os arquivos serão selecionados se a hora da última modificação estiver dentro do intervalo de `modifiedDatetimeStart` tempo `modifiedDatetimeEnd`entre e. A hora é aplicada ao fuso horário UTC no formato "2018-12-01T05:00:00Z". <br/><br/> O desempenho geral da movimentação de dados é afetado pela habilitação dessa configuração quando você deseja fazer o filtro de arquivo com grandes quantidades de arquivos. <br/><br/> As propriedades podem ser nulas, o que significa que nenhum filtro de atributo de arquivo é aplicado ao conjunto de valores. Quando `modifiedDatetimeStart` tem um valor de DateTime `modifiedDatetimeEnd` , mas é nulo, significa que os arquivos cujo último atributo modificado é maior ou igual ao valor de data e hora são selecionados. Quando `modifiedDatetimeEnd` tem um valor de DateTime `modifiedDatetimeStart` , mas é nulo, significa que os arquivos cujo último atributo modificado é menor que o valor de data e hora são selecionados.| Não |
| modifiedDatetimeEnd | Filtro de arquivos com base no atributo modificado pela última vez. Os arquivos serão selecionados se a hora da última modificação estiver dentro do intervalo de `modifiedDatetimeStart` tempo `modifiedDatetimeEnd`entre e. A hora é aplicada ao fuso horário UTC no formato "2018-12-01T05:00:00Z". <br/><br/> O desempenho geral da movimentação de dados é afetado pela habilitação dessa configuração quando você deseja fazer o filtro de arquivo com grandes quantidades de arquivos. <br/><br/> As propriedades podem ser nulas, o que significa que nenhum filtro de atributo de arquivo é aplicado ao conjunto de valores. Quando `modifiedDatetimeStart` tem um valor de DateTime `modifiedDatetimeEnd` , mas é nulo, significa que os arquivos cujo último atributo modificado é maior ou igual ao valor de data e hora são selecionados. Quando `modifiedDatetimeEnd` tem um valor de DateTime `modifiedDatetimeStart` , mas é nulo, significa que os arquivos cujo último atributo modificado é menor que o valor de data e hora são selecionados.| Não |
| format | Se você quiser copiar arquivos como estão entre repositórios baseados em arquivo (cópia binária), ignore a seção de formato nas definições de conjunto de dados de entrada e saída.<br/><br/>Se você quer analisar ou gerar arquivos com um formato específico, os seguintes tipos de formato de arquivo são compatíveis: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** e **ParquetFormat**. Defina a propriedade **type** sob **format** para um desses valores. Para saber mais, veja as seções [Formato de texto](supported-file-formats-and-compression-codecs.md#text-format), [Formato JSON](supported-file-formats-and-compression-codecs.md#json-format), [Formato Avro](supported-file-formats-and-compression-codecs.md#avro-format), [Formato Orc](supported-file-formats-and-compression-codecs.md#orc-format) e [Formato Parquet](supported-file-formats-and-compression-codecs.md#parquet-format). |Não (somente para o cenário de cópia binária) |
| compression | Especifique o tipo e o nível de compactação para os dados. Para obter mais informações, consulte [Formatos de arquivo e codecs de compactação com suporte](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Os tipos com suporte são: **GZip**, **Deflate**, **BZip2** e **ZipDeflate**.<br/>Os níveis de suporte são **Ideal** e **Mais rápido**. |Não |


>[!TIP]
>Para copiar todos os arquivos em uma pasta, especifique **folderPath** somente.<br>Para copiar um único arquivo com um nome específico, especifique **FolderPath** com uma parte da pasta e o nome do arquivo com um nome de arquivos.<br>Para copiar um subconjunto de arquivos em uma pasta, especifique **FolderPath** com uma parte de pasta e um **nome de arquivo** com um filtro curinga. 

**Exemplo:**

```json
{
    "name": "ADLSDataset",
    "properties": {
        "type": "AzureDataLakeStoreFile",
        "linkedServiceName":{
            "referenceName": "<ADLS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "datalake/myfolder/",
            "fileName": "*",
            "modifiedDatetimeStart": "2018-12-01T05:00:00Z",
            "modifiedDatetimeEnd": "2018-12-01T06:00:00Z",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propriedades da atividade de cópia

Para obter uma lista completa das seções e propriedades disponíveis para definir as atividades, veja [Pipelines](concepts-pipelines-activities.md). Esta seção fornece uma lista das propriedades com suporte pela origem e pelo coletor do Azure Data Lake Store.

### <a name="azure-data-lake-store-as-source"></a>Azure Data Lake Store como fonte

- Para copiar de **parquet, texto delimitado e formato binário**, consulte a seção [parquet, texto delimitado e fonte de formato binário](#format-based-source) .
- Para copiar de outros formatos, como o **formato Orc/Avro/JSON**, consulte [outra seção fonte de formato](#other-format-source) .

#### <a name="format-based-source"></a>Parquet, texto delimitado e fonte de formato binário

Para copiar dados de **parquet, texto delimitado ou formato binário**, consulte o [formato parquet](format-parquet.md), o [formato de texto delimitado](format-delimited-text.md) e o artigo de [formato binário](format-binary.md) na fonte da atividade de cópia baseada em formato e nas configurações com suporte.  As propriedades a seguir têm suporte para Azure data Lake Store Gen1 `storeSettings` em configurações na fonte de cópia baseada em formato:

| Propriedade                 | Descrição                                                  | Necessário                                      |
| ------------------------ | ------------------------------------------------------------ | --------------------------------------------- |
| type                     | A propriedade Type em `storeSettings` deve ser definida como **AzureDataLakeStoreReadSetting**. | Sim                                           |
| recursive                | Indica se os dados são lidos recursivamente das subpastas ou somente da pasta especificada. Quando recursivo é definido como true e o coletor é um armazenamento baseado em arquivo, uma pasta ou subpasta vazia não é copiada ou criada no coletor. Os valores permitidos são **true** (padrão) e **false**. | Não                                            |
| wildcardFolderPath       | O caminho da pasta com caracteres curinga para filtrar as pastas de origem. <br>Curingas permitidos são `*` (corresponde a zero ou mais caracteres) `?` e (corresponde a zero ou a um único caractere). Use `^` para escapar se o nome real da pasta tiver um curinga ou esse caractere de escape dentro. <br>Veja mais exemplos em [Exemplos de filtro de pastas e arquivos](#folder-and-file-filter-examples). | Não                                            |
| wildcardFileName         | O nome do arquivo com caracteres curinga sob o folderPath/wildcardFolderPath fornecido para filtrar os arquivos de origem. <br>Curingas permitidos são `*` (corresponde a zero ou mais caracteres) `?` e (corresponde a zero ou a um único caractere). Use `^` para escapar se o nome real da pasta tiver um curinga ou esse caractere de escape dentro. Veja mais exemplos em [Exemplos de filtro de pastas e arquivos](#folder-and-file-filter-examples). | Sim se `fileName` não for especificado no DataSet |
| modifiedDatetimeStart    | Filtro de arquivos com base no atributo modificado pela última vez. Os arquivos serão selecionados se a hora da última modificação estiver dentro do intervalo de `modifiedDatetimeStart` tempo `modifiedDatetimeEnd`entre e. A hora é aplicada ao fuso horário UTC no formato "2018-12-01T05:00:00Z". <br> As propriedades podem ser nulas, o que significa que nenhum filtro de atributo de arquivo é aplicado ao conjunto de valores. Quando `modifiedDatetimeStart` tem um valor de DateTime `modifiedDatetimeEnd` , mas é nulo, significa que os arquivos cujo último atributo modificado é maior ou igual ao valor de data e hora são selecionados. Quando `modifiedDatetimeEnd` tem um valor de DateTime `modifiedDatetimeStart` , mas é nulo, significa que os arquivos cujo último atributo modificado é menor que o valor de data e hora são selecionados. | Não                                            |
| modifiedDatetimeEnd      | Mesmo que acima.                                               | Não                                            |
| maxConcurrentConnections | O número de conexões a serem conectadas ao repositório de armazenamento simultaneamente. Especifique somente quando quiser limitar a conexão simultânea com o armazenamento de dados. | Não                                            |

> [!NOTE]
> Para o formato de texto parquet ou delimitado, a fonte da atividade de cópia do tipo **AzureDataLakeStoreSource** mencionada na seção a seguir ainda é suportada como é para compatibilidade com versões anteriores. É recomendável que você use esse novo modelo no futuro. A interface do usuário de criação de Data Factory gera esses novos tipos.

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen1",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Delimited text input dataset name>",
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
                "type": "DelimitedTextSource",
                "formatSettings":{
                    "type": "DelimitedTextReadSetting",
                    "skipLineCount": 10
                },
                "storeSettings":{
                    "type": "AzureDataLakeStoreReadSetting",
                    "recursive": true,
                    "wildcardFolderPath": "myfolder*A",
                    "wildcardFileName": "*.csv"
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="other-format-source"></a>Outra fonte de formato

Para copiar dados de Azure Data Lake Store Gen1 no **formato Orc, Avro ou JSON**, as propriedades a seguir têm suporte na seção **origem** da atividade de cópia:

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| type | A `type` propriedade da fonte da atividade de cópia deve ser definida como **AzureDataLakeStoreSource**. |Sim |
| recursive | Indica se os dados são lidos recursivamente das subpastas ou somente da pasta especificada. Quando `recursive` é definido como true e o coletor é um armazenamento baseado em arquivo, uma pasta ou subpasta vazia não é copiada ou criada no coletor. Os valores permitidos são **true** (padrão) e **false**. | Não |
| maxConcurrentConnections | O número de conexões para se conectar ao repositório de dados simultaneamente. Especifique somente quando quiser limitar a conexão simultânea com o armazenamento de dados. | Não |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyFromADLSGen1",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ADLS Gen1 input dataset name>",
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
                "type": "AzureDataLakeStoreSource",
                "recursive": true
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="azure-data-lake-store-as-sink"></a>Azure Data Lake Store como coletor

- Para copiar para **parquet, texto delimitado ou formato binário**, consulte a seção [parquet, texto delimitado e coletor de formato binário](#format-based-sink) .
- Para copiar para outros formatos, como o **formato Orc/Avro/JSON**, consulte outra seção de [coletor de formato](#other-format-sink) .

#### <a name="format-based-sink"></a>Parquet, texto delimitado e coletor de formato binário

Para copiar dados para **parquet, texto delimitado ou formato binário**, consulte [formato de parquet](format-parquet.md), [formato de texto delimitado](format-delimited-text.md) e artigo de [formato binário](format-binary.md) no coletor de atividade de cópia com base em formato e configurações com suporte.  As propriedades a seguir têm suporte para Azure data Lake Store Gen1 `storeSettings` em configurações no coletor de cópia com base em formato:

| Propriedade                 | Descrição                                                  | Necessário |
| ------------------------ | ------------------------------------------------------------ | -------- |
| type                     | A propriedade Type em `storeSettings` deve ser definida como **AzureDataLakeStoreWriteSetting**. | Sim      |
| copyBehavior             | Define o comportamento de cópia quando a fonte for de arquivos de um armazenamento de dados baseado em arquivo.<br/><br/>Valores permitidos são:<br/><b>– PreserveHierarchy (padrão)</b>: Preserva a hierarquia de arquivos na pasta de destino. O caminho relativo do arquivo de origem para a pasta de origem é idêntico ao caminho relativo do arquivo de destino para a pasta de destino.<br/><b>– FlattenHierarchy</b>: Todos os arquivos da pasta de origem estão no primeiro nível da pasta de destino. Os arquivos de destino têm os nomes gerados automaticamente. <br/><b>– MergeFiles</b>: Mescla todos os arquivos da pasta de origem em um arquivo. Se o nome do arquivo for especificado, o nome do arquivo mesclado será o nome especificado. Caso contrário, ele será um nome de arquivo gerado automaticamente. | Não       |
| maxConcurrentConnections | O número de conexões para se conectar ao repositório de dados simultaneamente. Especifique somente quando quiser limitar a conexão simultânea com o armazenamento de dados. | Não       |

> [!NOTE]
> Para o formato de texto parquet ou delimitado, o coletor de atividade de cópia do tipo **AzureDataLakeStoreSink** mencionado na seção a seguir ainda tem suporte como é para compatibilidade com versões anteriores. É recomendável que você use esse novo modelo no futuro. A interface do usuário de criação de Data Factory gera esses novos tipos.

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyToADLSGen1",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Parquet output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "ParquetSink",
                "storeSettings":{
                    "type": "AzureDataLakeStoreWriteSetting",
                    "copyBehavior": "PreserveHierarchy"
                }
            }
        }
    }
]
```

#### <a name="other-format-sink"></a>Outro coletor de formato

Para copiar dados para Azure Data Lake Store Gen1 no **formato Orc, Avro ou JSON**, há suporte para as seguintes propriedades na seção **Sink** :

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| type | A `type` Propriedade do coletor da atividade de cópia deve ser definida como **AzureDataLakeStoreSink**. |Sim |
| copyBehavior | Define o comportamento de cópia quando a fonte for de arquivos de um armazenamento de dados baseado em arquivo.<br/><br/>Valores permitidos são:<br/><b>– PreserveHierarchy (padrão)</b>: Preserva a hierarquia de arquivos na pasta de destino. O caminho relativo do arquivo de origem para a pasta de origem é idêntico ao caminho relativo do arquivo de destino para a pasta de destino.<br/><b>– FlattenHierarchy</b>: Todos os arquivos da pasta de origem estão no primeiro nível da pasta de destino. Os arquivos de destino têm os nomes gerados automaticamente. <br/><b>– MergeFiles</b>: Mescla todos os arquivos da pasta de origem em um arquivo. Se o nome do arquivo for especificado, o nome do arquivo mesclado será o nome especificado. Caso contrário, o nome do arquivo será gerado automaticamente. | Não |
| maxConcurrentConnections | O número de conexões para se conectar ao repositório de dados simultaneamente. Especifique somente quando quiser limitar a conexão simultânea com o armazenamento de dados. | Não |

**Exemplo:**

```json
"activities":[
    {
        "name": "CopyToADLSGen1",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<ADLS Gen1 output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureDataLakeStoreSink",
                "copyBehavior": "PreserveHierarchy"
            }
        }
    }
]
```

### <a name="folder-and-file-filter-examples"></a>Exemplos de filtro de pasta e arquivo

Esta seção descreve o comportamento resultante do caminho da pasta e do nome de arquivo com filtros curinga.

| folderPath | fileName | recursive | Estrutura da pasta de origem e resultado do filtro (os arquivos em **negrito** são recuperados)|
|:--- |:--- |:--- |:--- |
| `Folder*` | (Vazio, usar padrão) | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Arquivo1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Arquivo2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subpasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo5.csv<br/>OutraPastaB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo6.csv |
| `Folder*` | (Vazio, usar padrão) | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Arquivo1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Arquivo2.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subpasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Arquivo3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Arquivo4.json**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Arquivo5.csv**<br/>OutraPastaB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo6.csv |
| `Folder*` | `*.csv` | false | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Arquivo1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subpasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo3.csv<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo5.csv<br/>OutraPastaB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo6.csv |
| `Folder*` | `*.csv` | true | FolderA<br/>&nbsp;&nbsp;&nbsp;&nbsp;**Arquivo1.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo2.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subpasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Arquivo3.csv**<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo4.json<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Arquivo5.csv**<br/>OutraPastaB<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo6.csv |

### <a name="examples-of-behavior-of-the-copy-operation"></a>Exemplos de comportamento da operação de cópia

Esta seção descreve o comportamento resultante da operação de Cópia para diferentes combinações de valores `recursive` e `copyBehavior`.

| recursive | copyBehavior | Estrutura de pasta de origem | Destino resultante |
|:--- |:--- |:--- |:--- |
| true |preserveHierarchy | Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subpasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo5 | A Pasta1 de destino é criada com a mesma estrutura da origem:<br/><br/>Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subpasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo5. |
| true |flattenHierarchy | Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subpasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo5 | A Pasta1 de destino é criada com a seguinte estrutura: <br/><br/>Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;nome gerado automaticamente para o Arquivo1<br/>&nbsp;&nbsp;&nbsp;&nbsp;nome gerado automaticamente para o Arquivo2<br/>&nbsp;&nbsp;&nbsp;&nbsp;nome gerado automaticamente para o Arquivo3<br/>&nbsp;&nbsp;&nbsp;&nbsp;nome gerado automaticamente para o Arquivo4<br/>&nbsp;&nbsp;&nbsp;&nbsp;nome gerado automaticamente para o Arquivo5 |
| true |mergeFiles | Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subpasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo5 | A Pasta1 de destino é criada com a seguinte estrutura: <br/><br/>Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Os conteúdos de file1 + arquivo2 + arquivo3 + Arquivo4 + Arquivo5 são mesclados em um arquivo, com um nome de arquivo gerado automaticamente. |
| false |preserveHierarchy | Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subpasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo5 | A Pasta1 de destino é criada com a seguinte estrutura:<br/><br/>Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo2<br/><br/>Subpasta1 com arquivo3, Arquivo4 e Arquivo5 não são selecionados. |
| false |flattenHierarchy | Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subpasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo5 | A Pasta1 de destino é criada com a seguinte estrutura:<br/><br/>Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;nome gerado automaticamente para o Arquivo1<br/>&nbsp;&nbsp;&nbsp;&nbsp;nome gerado automaticamente para o Arquivo2<br/><br/>Subpasta1 com arquivo3, Arquivo4 e Arquivo5 não são selecionados. |
| false |mergeFiles | Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Arquivo2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subpasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arquivo5 | A Pasta1 de destino é criada com a seguinte estrutura:<br/><br/>Pasta1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Os conteúdos de file1 + arquivo2 são mesclados em um arquivo com o nome de arquivo gerado automaticamente. nome gerado automaticamente para o Arquivo1<br/><br/>Subpasta1 com arquivo3, Arquivo4 e Arquivo5 não são selecionados. |

## <a name="preserve-acls-to-data-lake-storage-gen2"></a>Preservar ACLs para Data Lake Storage Gen2

Se você quiser replicar as listas de controle de acesso (ACLs) junto com os arquivos de dados ao atualizar de Data Lake Storage Gen1 para Data Lake Storage Gen2, consulte [preservar ACLs de data Lake Storage Gen1](connector-azure-data-lake-storage.md#preserve-acls-from-data-lake-storage-gen1).

## <a name="mapping-data-flow-properties"></a>Mapeando Propriedades de fluxo de dados

Saiba mais sobre [transformação de origem](data-flow-source.md) e [transformação de coletor](data-flow-sink.md) no recurso de fluxo de dados de mapeamento.

## <a name="next-steps"></a>Próximas etapas

Para obter uma lista de armazenamentos de dados com suporte como origens e coletores pela atividade de cópia no Azure Data Factory, consulte [Armazenamentos de dados com suporte](copy-activity-overview.md##supported-data-stores-and-formats).
