---
title: Limites de Azure Cosmos DB Gremlin
description: Documentação de referência para limitações de tempo de execução do mecanismo de grafo
author: olignat
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: reference
ms.date: 09/10/2019
ms.author: olignat
ms.openlocfilehash: f1fe4cfac22a651f44338986d0a767fe3e1a360b
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910973"
---
# <a name="azure-cosmos-db-gremlin-limits"></a>Azure Cosmos DB limites de Gremlin
Este artigo fala sobre os limites do mecanismo de Azure Cosmos DB Gremlin e explica como eles podem afetar as passagens dos clientes.

Cosmos DB Gremlin é criado sobre a infraestrutura Cosmos DB por que todos os limites no [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/concepts-limits) ainda se aplicam. 

## <a name="limits"></a>Limites

Quando o limite de Gremlin é atingido, a passagem é cancelada com **x-MS-status-code** = 429 indicando o erro de limitação.

**Recurso**    | **Limite padrão** | **Explicação**
--- | --- | ---
*Memória por solicitação* | **2 GB** | Memória máxima que uma solicitação pode consumir durante o processamento. As solicitações que precisam calcular grandes conjuntos de dados consumirão memória adicional. Considere as solicitações de escopo para conjuntos de dados menores para evitar cruzar esse limite ou usar soluções OLAP.
*Tamanho do script* | **64 KB** | Comprimento máximo de um script de passagem do Gremlin por solicitação.
*Profundidade do operador* | **400** |  Número total de etapas exclusivas em uma passagem. Por exemplo, ```g.V().out()``` tem a contagem de operadores de 2: V () e out () ```g.V('label').repeat(out()).times(100)``` tem profundidade do operador de 3: V (), REPEAT () e out () porque ```.times(100)``` é um parâmetro para ```.repeat()``` Operator.
*Grau de paralelismo* | **32** | Número máximo de partições de armazenamento consultado em uma única solicitação para a camada de armazenamento. Grafos com centenas de partições serão afetados por esse limite.
*Limite de repetição* | **32** | Número máximo de iterações que um operador ```.repeat()``` pode executar. Cada iteração ```.repeat()``` da etapa na maioria dos casos executa a passagem em primeiro lugar, o que significa que qualquer percurso é limitado a no máximo 32 saltos entre vértices.
*Tempo limite de percurso* | **30 segundos** | A passagem será cancelada quando exceder esse tempo. O Cosmos DB Graph é um banco de dados OLTP com a grande maioria das passagens sendo concluídas em milissegundos. Para executar consultas OLAP em Cosmos DB grafo, use [Apache Spark](https://azure.microsoft.com/services/cosmos-db/) com [quadros de dados do grafo](https://spark.apache.org/docs/latest/sql-programming-guide.html#datasets-and-dataframes) e [Cosmos DB conector do Spark](https://github.com/Azure/azure-cosmosdb-spark).
*Limite de predicado* | **20** | Contagem de ```.has()``` ou ```.hasNot()``` etapas aplicadas em um único vértice ou borda. Quando esse limite é ocorrências de erro exibidas para o aplicativo é ```The SQL query exceeded the maximum number of joins. The allowed limit is 20```. É uma inconveniência temporária, pois a equipe está trabalhando para aumentar esse limite. 
*Tempo limite de conexão ociosa* | **5 horas** | A quantidade de tempo que o Graph Server manterá a conexão WebSocket aberta sem tráfego. Os pacotes Keep-Alive TCP ou as solicitações de Keep-Alive HTTP não estendem a duração da conexão além desse limite, no entanto, se não forem enviadas, a infraestrutura do Azure subjacente poderá fechar a conexão ainda mais cedo. Cosmos DB mecanismo de grafo considera ocioso se não houver nenhuma passagem Gremlin em execução.
*Token de recurso por hora* | **100** | Número de tokens de recurso exclusivos usados pelos clientes Gremlin para se conectarem à conta Gremlin em uma região. Quando o aplicativo excede o limite de token exclusivo `"Exceeded allowed resource token limit of 100 that can be used concurrently"` por hora, será retornado na próxima solicitação de autenticação.

## <a name="next-steps"></a>Próximas etapas
* [Azure Cosmos DB cabeçalhos de resposta do Gremlin](gremlin-headers.md) 
* [Azure Cosmos DB tokens de recurso com Gremlin](how-to-use-resource-tokens-gremlin.md)
