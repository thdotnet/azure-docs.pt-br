---
title: Obter métricas de desempenho e a execução de consulta SQL
description: Saiba como recuperar as métricas de execução de consulta SQL e o perfil de desempenho da consulta SQL de solicitações do Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: girobins
ms.openlocfilehash: b4017666956d0e01ea19781fb4f1ce2dde15fff5
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481558"
---
# <a name="get-sql-query-execution-metrics-and-analyze-query-performance-using-net-sdk"></a>Obter métricas de execução de consulta SQL e analisar o desempenho de consulta usando o SDK do .NET

Este artigo apresenta como criar o perfil de desempenho de consultas SQL no Azure Cosmos DB. Essa criação de perfil pode ser feita usando `QueryMetrics` recuperados do SDK do .NET e é detalhada aqui. [QueryMetrics](https://msdn.microsoft.com/library/microsoft.azure.documents.querymetrics.aspx) é um objeto fortemente tipado com informações sobre a execução da consulta de back-end. Essas métricas são documentadas em mais detalhes os [ajustar o desempenho da consulta](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-metrics) artigo.

## <a name="set-the-feedoptions-parameter"></a>Defina o parâmetro FeedOptions

Todas as sobrecargas para [DocumentClient.CreateDocumentQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentquery.aspx) levar em um recurso opcional [FeedOptions](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.aspx) parâmetro. Essa opção é o que permite a execução de consulta a ser ajustado e com parâmetros. 

Para coletar as métricas de execução de consulta Sql, você deve definir o parâmetro [PopulateQueryMetrics](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.populatequerymetrics.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.PopulateQueryMetrics) na [FeedOptions](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.aspx) para `true`. Definindo `PopulateQueryMetrics` para true irá torná-lo para que o `FeedResponse` conterá o relevantes `QueryMetrics`. 

## <a name="get-query-metrics-with-asdocumentquery"></a>Obter métricas de consulta com AsDocumentQuery()
O exemplo de código a seguir mostra como recuperar as métricas ao usar [AsDocumentQuery()](https://msdn.microsoft.com/library/microsoft.azure.documents.linq.documentqueryable.asdocumentquery.aspx) método:

```csharp
// Initialize this DocumentClient and Collection
DocumentClient documentClient = null;
DocumentCollection collection = null;

// Setting PopulateQueryMetrics to true in the FeedOptions
FeedOptions feedOptions = new FeedOptions
{
    PopulateQueryMetrics = true
};

string query = "SELECT TOP 5 * FROM c";
IDocumentQuery<dynamic> documentQuery = documentClient.CreateDocumentQuery(Collection.SelfLink, query, feedOptions).AsDocumentQuery();

while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    
    // This dictionary maps the partitionId to the QueryMetrics of that query
    IReadOnlyDictionary<string, QueryMetrics> partitionIdToQueryMetrics = feedResponse.QueryMetrics;
    
    // At this point you have QueryMetrics which you can serialize using .ToString()
    foreach (KeyValuePair<string, QueryMetrics> kvp in partitionIdToQueryMetrics)
    {
        string partitionId = kvp.Key;
        QueryMetrics queryMetrics = kvp.Value;
        
        // Do whatever logging you need
        DoSomeLoggingOfQueryMetrics(query, partitionId, queryMetrics);
    }
}
```
## <a name="aggregating-querymetrics"></a>Agregando QueryMetrics

Na seção anterior, observe que havia várias chamadas para [ExecuteNextAsync](https://msdn.microsoft.com/library/azure/dn850294.aspx) método. Cada chamada retornou um `FeedResponse` objeto que tem um dicionário de `QueryMetrics`; um para cada continuação da consulta. O exemplo a seguir mostra como agregar esses `QueryMetrics` usando LINQ:

```csharp
List<QueryMetrics> queryMetricsList = new List<QueryMetrics>();

while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    
    // This dictionary maps the partitionId to the QueryMetrics of that query
    IReadOnlyDictionary<string, QueryMetrics> partitionIdToQueryMetrics = feedResponse.QueryMetrics;
    queryMetricsList.AddRange(partitionIdToQueryMetrics.Values);
}

// Aggregate the QueryMetrics using the + operator overload of the QueryMetrics class.
QueryMetrics aggregatedQueryMetrics = queryMetricsList.Aggregate((curr, acc) => curr + acc);
Console.WriteLine(aggregatedQueryMetrics);
```

## <a name="grouping-query-metrics-by-partition-id"></a>Métricas de consulta de agrupamento por ID de partição

Você pode agrupar o `QueryMetrics` pela ID de partição. O agrupamento por ID de partição permite que você veja se uma partição específica está causando problemas de desempenho em comparação com outras pessoas. O exemplo a seguir mostra como agrupar `QueryMetrics` com LINQ:

```csharp
List<KeyValuePair<string, QueryMetrics>> partitionedQueryMetrics = new List<KeyValuePair<string, QueryMetrics>>();
while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    
    // This dictionary is maps the partitionId to the QueryMetrics of that query
    IReadOnlyDictionary<string, QueryMetrics> partitionIdToQueryMetrics = feedResponse.QueryMetrics;
    partitionedQueryMetrics.AddRange(partitionIdToQueryMetrics.ToList());
}

// Now we are able to group the query metrics by partitionId
IEnumerable<IGrouping<string, KeyValuePair<string, QueryMetrics>>> groupedByQueryMetrics = partitionedQueryMetrics
    .GroupBy(kvp => kvp.Key);

// If we wanted to we could even aggregate the groupedby QueryMetrics
foreach(IGrouping<string, KeyValuePair<string, QueryMetrics>> grouping in groupedByQueryMetrics)
{
    string partitionId = grouping.Key;
    QueryMetrics aggregatedQueryMetricsForPartition = grouping
        .Select(kvp => kvp.Value)
        .Aggregate((curr, acc) => curr + acc);
    DoSomeLoggingOfQueryMetrics(query, partitionId, aggregatedQueryMetricsForPartition);
}
```

## <a name="linq-on-documentquery"></a>LINQ em DocumentQuery

Você também pode obter o `FeedResponse` de uma consulta LINQ usando o `AsDocumentQuery()` método:

```csharp
IDocumentQuery<Document> linqQuery = client.CreateDocumentQuery(collection.SelfLink, feedOptions)
    .Take(1)
    .Where(document => document.Id == "42")
    .OrderBy(document => document.Timestamp)
    .AsDocumentQuery();
FeedResponse<Document> feedResponse = await linqQuery.ExecuteNextAsync<Document>();
IReadOnlyDictionary<string, QueryMetrics> queryMetrics = feedResponse.QueryMetrics;
```

## <a name="expensive-queries"></a>Consultas caras

Você pode capturar as unidades de solicitação consumidas por cada consulta para investigar consultas caras ou consultas que consomem a alta taxa de transferência. Você pode obter o encargo de solicitação usando o [RequestCharge](https://msdn.microsoft.com/library/azure/dn948712.aspx) propriedade em `FeedResponse`. Para saber mais sobre como obter o encargo de solicitação usando o portal do Azure e SDKs diferentes, consulte [localizar o encargo de unidades de solicitação](find-request-unit-charge.md) artigo.

```csharp
string query = "SELECT * FROM c";
IDocumentQuery<dynamic> documentQuery = documentClient.CreateDocumentQuery(Collection.SelfLink, query, feedOptions).AsDocumentQuery();

while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    double requestCharge = feedResponse.RequestCharge
    
    // Log the RequestCharge how ever you want.
    DoSomeLogging(requestCharge);
}
```

## <a name="get-the-query-execution-time"></a>Obter o tempo de execução de consulta

Ao calcular o tempo necessário para executar uma consulta do lado do cliente, certifique-se de que você só pode incluir a hora de chamar o `ExecuteNextAsync` método e não outras partes da sua base de código. Apenas essas chamadas de ajudam você a calcular quanto tempo demorou para a execução da consulta conforme mostrado no exemplo a seguir:

```csharp
string query = "SELECT * FROM c";
IDocumentQuery<dynamic> documentQuery = documentClient.CreateDocumentQuery(Collection.SelfLink, query, feedOptions).AsDocumentQuery();
Stopwatch queryExecutionTimeEndToEndTotal = new Stopwatch();
while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    queryExecutionTimeEndToEndTotal.Start();
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    queryExecutionTimeEndToEndTotal.Stop();
}

// Log the elapsed time
DoSomeLogging(queryExecutionTimeEndToEndTotal.Elapsed);
```

## <a name="scan-queries-commonly-slow-and-expensive"></a>Verificar consultas (normalmente lentas e dispendiosas)

Uma consulta de varredura refere-se a uma consulta que não foi atendida por índice, devido a isso, muitos documentos são carregados antes de retornar o conjunto de resultados.

Abaixo está um exemplo de uma consulta de verificação:

```sql
SELECT VALUE c.description 
FROM   c 
WHERE UPPER(c.description) = "BABYFOOD, DESSERT, FRUIT DESSERT, WITHOUT ASCORBIC ACID, JUNIOR"
```

Filtro dessa consulta usa a função de sistema superior, que não seja servido do índice. A execução desta consulta em relação a uma grande coleção produzido as seguintes métricas de consulta para a primeira continuação:

```
QueryMetrics

Retrieved Document Count                 :          60,951
Retrieved Document Size                  :     399,998,938 bytes
Output Document Count                    :               7
Output Document Size                     :             510 bytes
Index Utilization                        :            0.00 %
Total Query Execution Time               :        4,500.34 milliseconds
  Query Preparation Times
    Query Compilation Time               :            0.09 milliseconds
    Logical Plan Build Time              :            0.05 milliseconds
    Physical Plan Build Time             :            0.04 milliseconds
    Query Optimization Time              :            0.01 milliseconds
  Index Lookup Time                      :            0.01 milliseconds
  Document Load Time                     :        4,177.66 milliseconds
  Runtime Execution Times
    Query Engine Times                   :          322.16 milliseconds
    System Function Execution Time       :           85.74 milliseconds
    User-defined Function Execution Time :            0.00 milliseconds
  Document Write Time                    :            0.01 milliseconds
Client Side Metrics
  Retry Count                            :               0
  Request Charge                         :        4,059.95 RUs
```

Observe os seguintes valores de saída de métricas da consulta:

```
Retrieved Document Count                 :          60,951
Retrieved Document Size                  :     399,998,938 bytes
```

Essa consulta carregada 60,951 documentos que totalizados 399,998,938 bytes. Ao carregar este número de bytes resulta em alta encargo de unidades de solicitação ou custo. Ele também leva muito tempo para executar a consulta, que é criptografada com a propriedade de tempo total gasto:

```
Total Query Execution Time               :        4,500.34 milliseconds
```

Que significa que a consulta levou 4.5 segundos para ser executada (e isso era apenas uma continuação).

Para otimizar essa consulta de exemplo, evite o uso de superior no filtro. Em vez disso, quando documentos são criados ou atualizados, o `c.description` valores devem ser inseridos em todos os caracteres maiusculos. Em seguida, torna-se a consulta: 

```sql
SELECT VALUE c.description 
FROM   c 
WHERE c.description = "BABYFOOD, DESSERT, FRUIT DESSERT, WITHOUT ASCORBIC ACID, JUNIOR"
```

Essa consulta agora é capaz de ser servido do índice.

Para saber mais sobre como ajustar o desempenho da consulta, consulte a [ajustar o desempenho da consulta](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-metrics) artigo.

## <a id="References"></a>Referências

- [Especificação do SQL no Azure Cosmos DB](https://go.microsoft.com/fwlink/p/?LinkID=510612)
- [ANSI SQL 2011](https://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
- [JSON](https://json.org/)
- [LINQ](/previous-versions/dotnet/articles/bb308959(v=msdn.10)) 

## <a name="next-steps"></a>Próximas etapas

- [Ajustar o desempenho da consulta](sql-api-query-metrics.md)
- [Visão geral de indexação](index-overview.md)
- [Amostras do .NET no Azure Cosmos DB](https://github.com/Azure/azure-cosmosdb-dotnet)
