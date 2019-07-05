---
title: Consultar dados no Azure Data Lake usando o Data Explorer do Azure
description: Saiba como consultar dados no Azure Data Lake usando o Data Explorer do Azure.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/25/2019
ms.openlocfilehash: d6a58d144482e17f7e4b615134115d1da46af6f0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67453171"
---
# <a name="query-data-in-azure-data-lake-using-azure-data-explorer-preview"></a>Consultar dados no Azure Data Lake usando o Gerenciador de dados do Azure (visualização)

O armazenamento do Azure Data Lake é uma solução de lago de dados altamente escalonável e econômica para análise de big data. Ele combina o poder de um sistema de arquivos de alto desempenho com grande escala e economia para ajudá-lo a acelerar seu tempo de insights. O Data Lake Store Gen2 estende as funcionalidades do Armazenamento de Blobs do Azure e é otimizado para cargas de trabalho de análise.
 
O Gerenciador de dados do Azure integra-se com o armazenamento de BLOBs do Azure e Azure Data Lake armazenamento Gen2, fornecendo rápido e armazenados em cache e indexados acesso aos dados no lake. Você pode analisar e consultar dados no lake sem ingestão anterior no Data Explorer do Azure. Você também pode consultar em dados ingeridos e uningested lake nativo ao mesmo tempo.  

> [!TIP]
> O melhor desempenho de consulta necessita de ingestão de dados no Data Explorer do Azure. A capacidade de consultar dados no armazenamento do Azure Data Lake Gen2 sem ingestão anterior só deve ser usada para dados históricos ou dados que raramente são consultados.
 
## <a name="optimize-query-performance-in-the-lake"></a>Otimizar o desempenho de consulta no lake 

* Dados de partição para melhorar o desempenho e a hora de consulta otimizado.
* Compacte os dados para melhorar o desempenho (gzip para obter a melhor compactação, lz4 para melhor desempenho).
* Use o armazenamento de BLOBs do Azure ou Azure Data Lake armazenamento Gen2 com a mesma região que seu cluster do Gerenciador de dados do Azure. 

## <a name="create-an-external-table"></a>Criar uma tabela externa

1. Use o `.create external table` comando para criar uma tabela externa no Gerenciador de dados do Azure. Comandos de tabela externa adicional, como `.show`, `.drop`, e `.alter` estão documentadas na [comandos de tabela externa](/azure/kusto/management/externaltables).

    ```Kusto
    .create external table ArchivedProducts(
    Timestamp:datetime,
    ProductId:long, ProductDescription:string) 
    kind=blob
    partition by bin(Timestamp, 1d) 
    dataformat=csv (h@'http://storageaccount.blob.core.windows.net/container1;secretKey') 
    with (compressed = true)  
    ```

    Essa consulta cria partições diárias *container1/yyyy/MM/dd/all_exported_blobs.csv*. Melhorar o desempenho é esperado com o particionamento mais granular. Por exemplo, as consultas em tabelas externas com partições diárias, como mostrado acima, terá um desempenho melhor que as consultas com tabelas particionadas mensais.

    > [!NOTE]
    > Contas de armazenamento com suporte no momento são o armazenamento de BLOBs do Azure ou Azure Data Lake armazenamento Gen2. Formatos de dados atualmente com suporte são csv, tsv e txt.

1. A tabela externa é visível no painel esquerdo da interface do usuário da Web

    ![tabela externa na IU da web](media/data-lake-query-data/external-tables-web-ui.png)
 
### <a name="external-table-permissions"></a>Permissões de tabela externa
 
* O usuário de banco de dados pode criar uma tabela externa. O criador da tabela automaticamente se torna o administrador de tabela.
* O cluster, o banco de dados ou o administrador de tabela pode editar uma tabela existente.
* Qualquer usuário de banco de dados ou o leitor pode consultar uma tabela externa.
 
## <a name="query-an-external-table"></a>Consultar uma tabela externa
 
Para consultar uma tabela externa, use o `external_table()` funcionar e fornecer o nome da tabela como o argumento da função. O restante da consulta é uma linguagem de consulta Kusto padrão.

```Kusto
external_table("ArchivedProducts") | take 100
```

> [!TIP]
> Atualmente, não há suporte para IntelliSense em consultas de tabela externa.

## <a name="query-external-and-ingested-data-together"></a>Consultar dados externos e ingeridos juntas

Você pode consultar tabelas externas e tabelas de dados ingeridos dentro da mesma consulta. Você [ `join` ](/azure/kusto/query/joinoperator) ou [ `union` ](/azure/kusto/query/unionoperator) a tabela externa com os dados adicionais do Gerenciador de dados do Azure, SQL servers ou outras fontes. Use uma [ `let( ) statement` ](/azure/kusto/query/letstatement) para atribuir um nome abreviado para uma referência de tabela externa.

No exemplo a seguir, *produtos* é uma tabela de dados ingeridos e *ArchivedProducts* é uma tabela externa que contém dados em que o armazenamento do Azure Data Lake Gen2:

```kusto
let T1 = external_table("ArchivedProducts") |  where TimeStamp > ago(100d);
let T = Products; //T is an internal table
T1 | join T on ProductId | take 10
```

## <a name="query-taxirides-external-table-in-the-help-cluster"></a>Consulta *TaxiRides* tabela externa no cluster de ajuda

O *TaxiRides* conjunto de dados de exemplo contém dados de táxi de Nova York do [táxi de NYC e Limusines comissão](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page).

### <a name="create-external-table-taxirides"></a>Criar tabela externa *TaxiRides* 

> [!NOTE]
> Esta seção descreve a consulta usada para criar o *TaxiRides* tabela externa na *ajudar* cluster. Uma vez que essa tabela já foi criada, você pode ignorá-la e executar [consulta *TaxiRides* dados da tabela externa](#query-taxirides-external-table-data). 

1. A consulta a seguir foi usada para criar a tabela externa *TaxiRides* no cluster de Ajuda. 

    ```kusto
    .create external table TaxiRides
    (
    trip_id: long,
    vendor_id: string, 
    pickup_datetime: datetime,
    dropoff_datetime: datetime,
    store_and_fwd_flag: string,
    rate_code_id: int,
    pickup_longitude: real,
    pickup_latitude: real,
    dropoff_longitude: real,
    dropoff_latitude: real,
    passenger_count: int,
    trip_distance: real,
    fare_amount: real,
    extra: real,
    mta_tax: real,
    tip_amount: real,
    tolls_amount: real,
    ehail_fee: real,
    improvement_surcharge: real,
    total_amount: real,
    payment_type: string,
    trip_type: int,
    pickup: string,
    dropoff: string,
    cab_type: string,
    precipitation: int,
    snow_depth: int,
    snowfall: int,
    max_temperature: int,
    min_temperature: int,
    average_wind_speed: int,
    pickup_nyct2010_gid: int,
    pickup_ctlabel: string,
    pickup_borocode: int,
    pickup_boroname: string,
    pickup_ct2010: string,
    pickup_boroct2010: string,
    pickup_cdeligibil: string,
    pickup_ntacode: string,
    pickup_ntaname: string,
    pickup_puma: string,
    dropoff_nyct2010_gid: int,
    dropoff_ctlabel: string,
    dropoff_borocode: int,
    dropoff_boroname: string,
    dropoff_ct2010: string,
    dropoff_boroct2010: string,
    dropoff_cdeligibil: string,
    dropoff_ntacode: string,
    dropoff_ntaname: string,
    dropoff_puma: string
    )
    kind=blob 
    partition by bin(pickup_datetime, 1d)
    dataformat=csv
    ( 
    h@'https://externalkustosamples.blob.core.windows.net/taxiridesbyday?st=2019-06-18T14%3A59%3A00Z&se=2029-06-19T14%3A59%3A00Z&sp=rl&sv=2016-05-31&sr=c&sig=yEaO%2BrzFHzAq7lvd4d9PeQ%2BTi3AWnho8Rn8hGU0X30M%3D'
    )
    ```
1. A tabela resultante foi criada na *ajudar* cluster:

    ![Tabela externa do TaxiRides](media/data-lake-query-data/taxirides-external-table.png) 

### <a name="query-taxirides-external-table-data"></a>Consulta *TaxiRides* dados da tabela externa 

Entrar no [ https://dataexplorer.azure.com/clusters/help/databases/Samples ](https://dataexplorer.azure.com/clusters/help/databases/Samples) para consultar o *TaxiRides* tabela externa. 

#### <a name="query-taxirides-external-table-without-partitioning"></a>Consulta *TaxiRides* tabela externa sem particionamento

[Execute esta consulta](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAAx3LSwqAMAwFwL3gHYKreh1xL7F9YrCtElP84OEV9zM4DZo5DsZjhGt6PqWTgL1p6+qhvaTEKjeI/FqyuZbGiwJf63QAi9vEL2UbAhtMEv6jyAH6+VhS9jOr1dULfUgAm2cAAAA=) na tabela externa *TaxiRides* para retratar passeios para cada dia da semana, em todo o conjunto de dados. 

```kusto
external_table("TaxiRides")
| summarize count() by dayofweek(pickup_datetime)
| render columnchart
```

Esta consulta mostra o dia mais ocupado da semana. Uma vez que os dados não estão particionados, esta consulta pode levar muito tempo para retornar resultados (até vários minutos).

![processar a consulta não particionada](media/data-lake-query-data/taxirides-no-partition.png)

#### <a name="query-taxirides-external-table-with-partitioning"></a>Consultar a tabela externa TaxiRides com o particionamento 

[Execute esta consulta](https://dataexplorer.azure.com/clusters/help/databases/Samples?query=H4sIAAAAAAAAA13NQQqDMBQE0L3gHT6ukkVF3fQepXv5SQYMNWmIP6ilh68WuinM6jHMYBPkyPMobGao5s6bv3mHpdF19aZ1QgYlbx8ljY4F4gPIQFYgkvqJGrr+eun6I5ralv58OP27t5QQOPsXiOyzRFGazE6WzSh7wtnIiA75uISdOEtdfQDLWmP+ogAAAA==) na tabela externa *TaxiRides* que mostra os tipos de cab do táxi (amarelo ou verde) usado em janeiro de 2017. 

```kusto
external_table("TaxiRides")
| where pickup_datetime between (datetime(2017-01-01) .. datetime(2017-02-01))
| summarize count() by cab_type
| render piechart
```

Essa consulta usa o particionamento, que otimiza o desempenho e a hora da consulta. A consulta filtra em uma coluna particionada (pickup_datetime) e retorna os resultados em poucos segundos.

![renderizar consulta particionada](media/data-lake-query-data/taxirides-with-partition.png)
  
Você pode escrever consultas adicionais para serem executadas na tabela externa *TaxiRides* e saiba mais sobre os dados. 

## <a name="next-steps"></a>Próximas etapas

Consulte os dados no Azure Data Lake usando o Data Explorer do Azure. Saiba como [escrever consultas](write-queries.md) e derivar informações adicionais de seus dados.