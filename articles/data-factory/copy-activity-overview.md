---
title: Atividade de cópia no Azure Data Factory | Microsoft Docs
description: Saiba mais sobre a atividade de cópia no Azure Data Factory que você pode usar apenas para copiar dados de um armazenamento de dados de origem com suporte para um armazenamento de dados de coletor com suporte.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: a8265496c475566ec7a87a19eab6d975838e9da4
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68966398"
---
# <a name="copy-activity-in-azure-data-factory"></a>Atividade de cópia no Azure Data Factory

## <a name="overview"></a>Visão geral

> [!div class="op_single_selector" title1="Selecione a versão do serviço Data Factory que você está usando:"]
> * [Versão 1](v1/data-factory-data-movement-activities.md)
> * [Versão atual](copy-activity-overview.md)

No Azure Data Factory, você pode usar a atividade de cópia para copiar dados entre armazenamentos de dados locais e na nuvem. Depois que os dados são copiados, eles podem ser mais transformados e analisados usando outras atividades. Também é possível usar a Atividade de Cópia a fim de publicar resultados de análise e transformação para consumo do aplicativo e BI (business intelligence).

![Função da Atividade de Cópia](media/copy-activity-overview/copy-activity.png)

A atividade de cópia é executada em um [Integration Runtime](concepts-integration-runtime.md). Para diferentes cenários de cópia de dados, diferentes tipos de Integration Runtime podem ser aproveitados:

* Ao copiar dados entre armazenamentos de dados que podem ser acessados publicamente por meio da Internet de qualquer IPs, a atividade de cópia pode ser habilitada pelo **Azure Integration Runtime**, que é seguro, confiável, escalonável e [disponível globalmente](concepts-integration-runtime.md#integration-runtime-location).
* Ao copiar dados de/para armazenamentos de dados locais ou em uma rede com controle de acesso (por exemplo, Rede Virtual do Azure), você precisa configurar um **Tempo de Execução Integrado auto-hospedado** para capacitar a cópia de dados.

O Integration Runtime precisa ser associado a cada armazenamento de dados de origem e de coletor. Obtenha detalhes sobre como a atividade de cópia [determina qual IR usar](concepts-integration-runtime.md#determining-which-ir-to-use).

A atividade de cópia passa pelos seguintes estágios para copiar dados de uma origem para um coletor. O serviço que capacita a Atividade de Cópia:

1. Lê dados de um armazenamento de dados de origem.
2. Executa a serialização/desserialização, a compactação/descompactação, o mapeamento de coluna etc. Ele realiza essas operações com base nas configurações do conjunto de dados de entrada, no conjunto de dados de saída e na Atividade de Cópia.
3. Grava dados no armazenamento de dados de coletor/destino.

![Visão Geral da Atividade de Cópia](media/copy-activity-overview/copy-activity-overview.png)

## <a name="supported-data-stores-and-formats"></a>Fontes de dados e formatos com suporte

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

### <a name="supported-file-formats"></a>Formatos de arquivo com suporte

Você pode usar a atividade de cópia para **copiar arquivos como estão** entre dois armazenamentos de dados baseados em arquivo, caso em que os dados são copiados de forma eficiente sem nenhuma serialização/desserialização.

A Atividade de cópia também dá suporte à leitura e à gravação de arquivos em formatos especificados: **Text, JSON, Avro, Orc e parquet**, e compactando e descompactando arquivos com os seguintes codecs: **Gzip, deflate, BZIP2 e ZipDeflate**. Consulte [Formatos de arquivo e compactação com suporte](supported-file-formats-and-compression-codecs.md) para obter detalhes.

Por exemplo, você pode fazer as seguintes atividades de cópia:

* Copie dados no local SQL Server e grave em Azure Data Lake Storage Gen2 no formato parquet.
* Copiar arquivos no formato de texto (CSV) do Sistema de Arquivos local e gravá-los no Blob do Azure no formato Avro.
* Copie arquivos compactados do sistema de arquivos local e descompacte, em seguida, para Azure Data Lake Storage Gen2.
* Copiar dados em formato de texto (CSV) compactado por GZip do Blob do Azure e gravá-los no Banco de Dados SQL do Azure.
* E muitos outros casos com necessidade de serialização/desserialização ou compactação/descompactação.

## <a name="supported-regions"></a>Regiões com suporte

O serviço que possibilita a Atividade de Cópia está disponível globalmente nas regiões e regiões geográficas listadas em [Localizações do Azure Integration Runtime](concepts-integration-runtime.md#integration-runtime-location). A topologia globalmente disponível garante a movimentação de dados eficiente, o que geralmente evita saltos entre regiões. Confira [Serviços por região](https://azure.microsoft.com/regions/#services) para ver a disponibilidade do Data Factory e da Movimentação de Dados em uma região.

## <a name="configuration"></a>Configuração

Para usar a atividade de cópia no Azure Data Factory, você precisa:

1. **Criar serviços vinculados para armazenamentos de dados de origem e de coletor.** Consulte a seção “Propriedades de serviço vinculadas” do artigo do conector sobre como configurar e as propriedades com suporte. Você pode encontrar a lista de conectores com suporte na seção [Formatos e armazenamentos de dados com suporte](#supported-data-stores-and-formats).
2. **Criar conjuntos de dados de origem e do coletor.** Consulte a seção "Propriedades do conjunto de notícias" do conector de origem e do coletor sobre como configurar e suas propriedades com suporte.
3. **Criar um pipeline com atividade de cópia.** A próxima seção fornece um exemplo.

### <a name="syntax"></a>Sintaxe

O modelo a seguir de uma atividade de cópia contém uma lista exaustiva das propriedades com suporte. Especifique as adequadas para o seu cenário.

```json
"activities":[
    {
        "name": "CopyActivityTemplate",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<source dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<sink dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>",
                <properties>
            },
            "sink": {
                "type": "<sink type>"
                <properties>
            },
            "translator":
            {
                "type": "TabularTranslator",
                "columnMappings": "<column mapping>"
            },
            "dataIntegrationUnits": <number>,
            "parallelCopies": <number>,
            "enableStaging": true/false,
            "stagingSettings": {
                <properties>
            },
            "enableSkipIncompatibleRow": true/false,
            "redirectIncompatibleRowSettings": {
                <properties>
            }
        }
    }
]
```

### <a name="syntax-details"></a>Detalhes da sintaxe

| Propriedade | Descrição | Necessário |
|:--- |:--- |:--- |
| type | A propriedade type de uma atividade de cópia deve ser definida como: **Copy** | Sim |
| inputs | Especifique o conjunto de dados criado que aponta para os dados de origem. A atividade de cópia dá suporte a apenas uma entrada. | Sim |
| outputs | Especifique o conjunto de dados criado que aponta para os dados do coletor. A atividade de cópia dá suporte a apenas uma saída. | Sim |
| typeProperties | Um grupo de propriedades para configurar a atividade de cópia. | Sim |
| source | Especifique o tipo de origem de cópia e as propriedades correspondentes sobre como recuperar dados.<br/><br/>Saiba mais detalhes na seção "Propriedades da atividade de cópia” no artigo do conector listado em [Formatos e armazenamentos de dados com suporte](#supported-data-stores-and-formats). | Sim |
| sink | Especifique o tipo de coletor de cópia e as propriedades correspondentes sobre como gravar dados.<br/><br/>Saiba mais detalhes na seção "Propriedades da atividade de cópia” no artigo do conector listado em [Formatos e armazenamentos de dados com suporte](#supported-data-stores-and-formats). | Sim |
| translator | Especifique mapeamentos de coluna explícita da origem para o coletor. Aplica-se quando o comportamento de cópia padrão não pode atender às suas necessidades.<br/><br/>Obtenha detalhes do [Mapeamento de tipo de dados e de esquema](copy-activity-schema-and-type-mapping.md). | Não |
| dataIntegrationUnits | Especifique o poder do [Integration Runtime do Azure](concepts-integration-runtime.md) para capacitar a cópia de dados. Anteriormente conhecido como Cloud Data Movement Units (DMU). <br/><br/>Aprenda detalhes de [Data Integration Units](copy-activity-performance.md#data-integration-units). | Não |
| parallelCopies | Especifique o paralelismo que você deseja que a atividade de cópia use ao ler dados da origem e gravar dados no coletor.<br/><br/>Obtenha detalhes em [Cópia paralela](copy-activity-performance.md#parallel-copy). | Não |
| enableStaging<br/>stagingSettings | Opte por preparar os dados provisórios em um armazenamento de BLOB em vez de copiar dados diretamente da origem para o coletor.<br/><br/>Conheça os cenários úteis e os detalhes de configuração em [Cópia em etapas](copy-activity-performance.md#staged-copy). | Não |
| enableSkipIncompatibleRow<br/>redirectIncompatibleRowSettings| Escolha como lidar com linhas incompatíveis ao copiar dados da origem para o coletor.<br/><br/>Obtenha detalhes em [Tolerância a falhas](copy-activity-fault-tolerance.md). | Não |

## <a name="monitoring"></a>Monitorando

É possível monitorar a atividade de cópia em execução na interface do usuário "Autor e Monitor" do Azure Data Factory ou programaticamente. Você pode comparar o desempenho e a configuração do seu cenário com a [referência de desempenho](copy-activity-performance.md#performance-reference) da atividade de cópia de testes internos.

### <a name="monitor-visually"></a>Monitorar visualmente

Para monitorar visualmente a execução da atividade de cópia, acesse o data factory -> **Autor e Monitor** -> **Guia Monitor** e você verá uma lista de pipeline executada com uma link "Exibir Execuções da Atividade" na coluna **Ações**.

![Monitorar execuções de pipeline](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)

Clique para ver a lista das atividades no pipeline executado. Na coluna **Ações**, há links para a entrada, saída, erros da atividade de cópia (se a execução da atividade de cópia falhar) e detalhes.

![Monitorar execuções de atividade](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

Clique o link "**Detalhes**" em **Ações** para ver os detalhes de execução da atividade de cópia e as características de desempenho. Ele mostra as informações, incluindo o volume/linhas/arquivos de dados copiados da fonte para o coletor, a taxa de transferência, as etapas realizadas com a duração correspondente e as configurações usadas para o cenário da sua cópia.

>[!TIP]
>Para alguns cenários, você também verá "**dicas de ajuste de desempenho**" na parte superior da página de monitoramento de cópia, que informa o afunilamento identificado e orienta você sobre o que mudar para aumentar a produtividade da cópia, consulte um exemplo com detalhes [aqui](#performance-and-tuning).

**Exemplo: cópia da Amazon S3 para o Azure Data Lake Store**
![Monitorar os detalhes da execução de atividade](./media/copy-activity-overview/monitor-activity-run-details-adls.png)

**Exemplo: copiar do Banco de Dados SQL do Azure para o SQL Data Warehouse do Microsoft Azure usando cópia preparada**
![Monitorar os detalhes da execução de atividade](./media/copy-activity-overview/monitor-activity-run-details-sql-dw.png)

### <a name="monitor-programmatically"></a>Monitorar programaticamente

Os detalhes de execução da atividade de cópia e as características de desempenho também são retornados na seção resultado da execução da atividade de cópia-> saída. Abaixo é apresentada uma lista exaustiva. Somente o que for aplicável ao seu cenário de cópia será exibido. Saiba como monitorar a execução da atividade na [seção de monitoramento de início rápido](quickstart-create-data-factory-dot-net.md#monitor-a-pipeline-run).

| Nome da propriedade  | Descrição | Unidade |
|:--- |:--- |:--- |
| dataRead | Leitura do tamanho de dados da origem | Valor Int64 em **bytes** |
| dataWritten | Tamanho dos dados gravado no coletor | Valor Int64 em **bytes** |
| filesRead | Número de arquivos copiados, ao copiar dados do armazenamento de arquivos. | Valor Int64 (nenhuma unidade) |
| filesWritten | Número de arquivos copiados, ao copiar dados para armazenamento de arquivos. | Valor Int64 (nenhuma unidade) |
| sourcePeakConnections | Número máximo de conexões simultâneas estabelecidas com o repositório de dados de origem durante a execução da atividade de cópia. | Valor Int64 (nenhuma unidade) |
| sinkPeakConnections | Número máximo de conexões simultâneas estabelecidas com o armazenamento de dados do coletor durante a execução da atividade de cópia. | Valor Int64 (nenhuma unidade) |
| rowsRead | Número de linhas sendo lidas da origem (não aplicável à cópia binária). | Valor Int64 (nenhuma unidade) |
| rowsCopied | Número de linhas sendo copiadas para o coletor (não aplicável para cópia binária). | Valor Int64 (nenhuma unidade) |
| rowsSkipped | Número de linhas incompatíveis que está sendo ignoradas. Você pode ativar o recurso definindo "enableSkipIncompatibleRow" como true. | Valor Int64 (nenhuma unidade) |
| copyDuration | A duração da cópia. | Valor Int32 em segundos |
| throughput | Taxa na qual os dados são transferidos. | Número de Ponto Flutuante em **KB/s** |
| sourcePeakConnections | Número máximo de conexões simultâneas estabelecidas com o armazenamento de dados de origem durante a cópia. | Valor Int32 |
| sinkPeakConnections| Número máximo de conexões simultâneas estabelecidas com o armazenamento de dados do coletor durante a cópia.| Valor Int32 |
| sqlDwPolyBase | Se o PolyBase é usado ao copiar dados para o SQL Data Warehouse. | Boolean |
| redshiftUnload | Se UNLOAD é usado ao copiar dados do Redshift. | Boolean |
| hdfsDistcp | Se DistCp é usado ao copiar dados do HDFS. | Boolean |
| effectiveIntegrationRuntime | Mostre quais Integration Runtime(s) são usados para capacitar a execução de atividade, no formato de `<IR name> (<region if it's Azure IR>)`. | Texto (cadeia de caracteres) |
| usedDataIntegrationUnits | As unidades de integração de dados efetivas durante a cópia. | Valor Int32 |
| usedParallelCopies | ParallelCopies efetivos durante a cópia. | Valor Int32 |
| redirectRowPath | Caminho para o log de linhas incompatíveis ignoradas no armazenamento de blobs que você configurou em "redirectIncompatibleRowSettings". Consulte o exemplo abaixo. | Texto (cadeia de caracteres) |
| executionDetails | Mais detalhes sobre a atividade de cópia dos estágios passam e as etapas correspondentes, duração, configurações usadas, etc. Não é recomendável analisar essa seção, pois pode ser alterada.<br/><br/>O ADF também relata as durações detalhadas (em segundos) gastas nas respectivas etapas `detailedDurations`em. As durações dessas etapas são exclusivas e somente as que se aplicam à execução da atividade de cópia fornecida serão mostradas:<br/>- **Duração** do enfileiramento (`queuingDuration`): O tempo decorrido até que a atividade de cópia realmente seja iniciada no Integration Runtime. Se você usar o IR auto-hospedado e esse valor for grande, sugira para verificar a capacidade e o uso do IR e escalar verticalmente de acordo com sua carga de trabalho. <br/>- **Duração da cópia prévia do script** (`preCopyScriptDuration`): O tempo decorrido entre a atividade de cópia a partir do IR e a atividade de cópia concluindo a execução do script de pré-cópia no repositório de dados do coletor. Aplique quando você configurar o script de pré-cópia. <br/>- **Tempo até o primeiro byte** (`timeToFirstByte`): O tempo decorrido entre o fim da etapa anterior e o IR recebendo o primeiro byte do armazenamento de dados de origem. Aplicar a uma fonte não baseada em arquivo. Se esse valor for grande, sugira para verificar e otimizar a consulta ou o servidor.<br/>- **Duração da transferência** (`transferDuration`): O tempo decorrido entre o fim da etapa anterior e o IR transferindo todos os dados da origem para o coletor. | Array |
| perfRecommendation | Copiar dicas de ajuste de desempenho. Consulte a seção [desempenho e ajuste](#performance-and-tuning) em detalhes. | Array |

```json
"output": {
    "dataRead": 6198358,
    "dataWritten": 19169324,
    "filesRead": 1,
    "sourcePeakConnections": 1,
    "sinkPeakConnections": 2,
    "rowsRead": 39614,
    "rowsCopied": 39614,
    "copyDuration": 1325,
    "throughput": 4.568,
    "errors": [],
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)",
    "usedDataIntegrationUnits": 4,
    "usedParallelCopies": 1,
    "executionDetails": [
        {
            "source": {
                "type": "AzureBlobStorage"
            },
            "sink": {
                "type": "AzureSqlDatabase"
            },
            "status": "Succeeded",
            "start": "2019-08-06T01:01:36.7778286Z",
            "duration": 1325,
            "usedDataIntegrationUnits": 4,
            "usedParallelCopies": 1,
            "detailedDurations": {
                "queuingDuration": 2,
                "preCopyScriptDuration": 12,
                "transferDuration": 1311
            }
        }
    ],
    "perfRecommendation": [
        {
            "Tip": "Sink Azure SQL Database: The DTU utilization was high during the copy activity run. To achieve better performance, you are suggested to scale the database to a higher tier than the current 1600 DTUs.",
            "ReferUrl": "https://go.microsoft.com/fwlink/?linkid=2043368",
            "RuleName": "AzureDBTierUpgradePerfRecommendRule"
        }
    ]
}
```

## <a name="schema-and-data-type-mapping"></a>Mapeamento de tipo de dados e de esquema

Consulte o [Mapeamento de tipo de dados e de esquema](copy-activity-schema-and-type-mapping.md), que descreve como a atividade de cópia mapeia os dados de origem com o coletor.

## <a name="fault-tolerance"></a>Tolerância a falhas

Por padrão, a atividade de cópia interrompe a cópia de dados e retorna uma falha quando encontra dados incompatíveis entre a origem e o coletor. Você pode configurar explicitamente para ignorar e registrar e log as linhas incompatíveis e copiar apenas os dados compatíveis para fazer com que a cópia seja bem-sucedida. Consulte a [Tolerância a falhas da Atividade de Cópia](copy-activity-fault-tolerance.md) para obter mais detalhes.

## <a name="performance-and-tuning"></a>Desempenho e ajuste

Confira o artigo [Guia de desempenho e ajuste da Atividade de Cópia](copy-activity-performance.md)que descreve os principais fatores que afetam o desempenho da movimentação de dados (Atividade de Cópia) no Azure Data Factory. Ele também lista o desempenho observado durante os testes internos e discute várias maneiras de otimizar o desempenho da Atividade de Cópia.

Em alguns casos, quando você executa uma atividade de cópia no ADF, verá diretamente "**Dicas de ajuste de desempenho**" na parte superior da [página de monitoramento de atividade de cópia](#monitor-visually) conforme mostra o exemplo a seguir. Ele não apenas informa o gargalo identificado para a execução de cópia determinada, como também o orienta sobre o que mudar para impulsionar a taxa de transferência de cópia. As dicas de ajuste de desempenho atualmente dão sugestões como usar o PolyBase ao copiar dados para o SQL Data Warehouse do Azure, aumentar a RU do Azure Cosmos DB ou a DTU de BD SQL do Azure, quando o recurso no lado de armazenamento de dados é o gargalo, remover a cópia em etapas desnecessária, etc. As regras de ajuste de desempenho serão aprimoradas gradualmente também.

**Exemplo: cópia para o BD SQL do Azure com dicas de ajuste de desempenho**

Neste exemplo, durante a execução da cópia, o ADF observa que o banco de informações do SQL Azure do coletor atinge uma alta utilização de DTU que reduz as operações de gravação, de modo que a sugestão é aumentar a camada do BD SQL do Azure com mais DTU.

![Monitoramento de cópia com dicas de ajuste de desempenho](./media/copy-activity-overview/copy-monitoring-with-performance-tuning-tips.png)

## <a name="incremental-copy"></a>Cópia incremental
O Data Factory dá suporte a cenários de cópia incremental de dados Delta de um armazenamento de dados de origem para um armazenamento de dados de coletor. Consulte [Tutorial: cópia incremental de dados](tutorial-incremental-copy-overview.md).

## <a name="read-and-write-partitioned-data"></a>Ler e gravar dados particionados
Na versão 1, com suporte do Azure Data Factory ler ou gravar dados particionados usando variáveis de sistema SliceStart/SliceEnd/WindowStart/WindowEnd. Na versão atual, você pode obter esse comportamento usando um parâmetro de pipeline e o horário de início/horário agendado do acionador como um valor do parâmetro. Para obter mais informações, consulte [Como ler e gravar dados de partição](how-to-read-write-partitioned-data.md).

## <a name="next-steps"></a>Próximas etapas
Consulte os seguintes guias de início rápido, tutoriais e exemplos:

- [Copiar dados de um local para outro no mesmo Armazenamento de Blobs do Azure](quickstart-create-data-factory-dot-net.md)
- [Copiar dados do Armazenamento de Blobs do Azure para o Banco de Dados SQL do Azure](tutorial-copy-data-dot-net.md)
- [Copiar dados do SQL Server local para o Azure](tutorial-hybrid-copy-powershell.md)
