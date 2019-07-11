---
title: Como configurar vários mestres no Azure Cosmos DB
description: Saiba como configurar vários mestres nos aplicativos no Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/02/2019
ms.author: mjbrown
ms.openlocfilehash: 73b4ada713e264aaa2504fe4d4f504e07ae45181
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538090"
---
# <a name="configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Configurar vários mestres nos aplicativos que usam o Azure Cosmos DB

Quando uma conta tiver sido criada com várias regiões de gravação habilitadas, faça duas alterações em seu aplicativo em ConnectionPolicy para DocumentClient a fim de habilitar os recursos de vários mestres e a hospedagem múltipla no Azure Cosmos DB. Em ConnectionPolicy, defina UseMultipleWriteLocations como true e passe o nome da região em que o aplicativo é implantado em SetCurrentLocation. Isso preencherá a propriedade PreferredLocations com base na proximidade geográfica do local passado. Se uma nova região posteriormente é adicionada à conta, o aplicativo não precisa ser atualizado ou reimplantado, ele detectará automaticamente a região mais próxima e será automaticamente iniciado em caso de evento regional.

> [!TIP]
> As contas do Cosmos criadas e não configuradas inicialmente para vários mestres podem ser migradas sem nenhum tempo de inatividade. Para saber mais, consulte [Configurar a gravação de várias regiões](how-to-manage-database-account.md#configure-multiple-write-regions)

## <a id="netv2"></a>SDK do .NET v2

Para habilitar vários mestres em seu aplicativo, defina `UseMultipleWriteLocations` como `true`. Além disso, defina `SetCurrentLocation` como a região na qual o aplicativo está sendo implantado e em que o Azure Cosmos DB está replicado:

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("West US 2");
```

## <a id="netv3"></a>SDK do .NET v3 (versão prévia)

Para habilitar vários mestres no seu aplicativo, defina `UseCurrentRegion` como a região na qual o aplicativo está sendo implantado e o Cosmos DB está replicado:

```csharp
CosmosConfiguration config = new CosmosConfiguration("endpoint", "key");
config.UseCurrentRegion("West US");
CosmosClient client = new CosmosClient(config);
```

## <a id="java"></a>SDK de Java Assíncrono

Para habilitar vários mestres no seu aplicativo, defina `policy.setUsingMultipleWriteLocations(true)` e defina `policy.setPreferredLocations` como a região na qual o aplicativo está sendo implantado e o Cosmos DB está replicado:

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setUsingMultipleWriteLocations(true);
policy.setPreferredLocations(Collections.singletonList(region));

AsyncDocumentClient client =
    new AsyncDocumentClient.Builder()
        .withMasterKeyOrResourceToken(this.accountKey)
        .withServiceEndpoint(this.accountEndpoint)
        .withConsistencyLevel(ConsistencyLevel.Eventual)
        .withConnectionPolicy(policy).build();
```

## <a id="javascript"></a>SDK do Node.js, do JavaScript e do TypeScript

Para habilitar vários mestres em seu aplicativo, defina `connectionPolicy.UseMultipleWriteLocations` como `true`. Além disso, defina `connectionPolicy.PreferredLocations` como a região na qual o aplicativo está sendo implantado e em que o Cosmos DB está replicado:

```javascript
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.UseMultipleWriteLocations = true;
connectionPolicy.PreferredLocations = [region];

const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy,
  consistencyLevel: ConsistencyLevel.Eventual
});
```

## <a id="python"></a>SDK do Python

Para habilitar vários mestres em seu aplicativo, defina `connection_policy.UseMultipleWriteLocations` como `true`. Além disso, defina `connection_policy.PreferredLocations` como a região na qual o aplicativo está sendo implantado e em que o Cosmos DB está replicado.

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="next-steps"></a>Próximas etapas

Leia os seguintes artigos:

* [Usar tokens de sessão para gerenciar a consistência no Azure Cosmos DB](how-to-manage-consistency.md#utilize-session-tokens)
* [Tipos de conflitos e políticas de resolução no Azure Cosmos DB](conflict-resolution-policies.md)
* [Alta disponibilidade no Azure Cosmos DB](high-availability.md)
* [Níveis de coerência no Azure Cosmos DB](consistency-levels.md)
* [Escolher o nível de consistência correto no Azure Cosmos DB](consistency-levels-choosing.md)
* [Compensações de consistência, disponibilidade e desempenho no Azure Cosmos DB](consistency-levels-tradeoffs.md)
* [Equilíbrio entre disponibilidade e desempenho para vários níveis de coerência](consistency-levels-tradeoffs.md)
* [Taxa de transferência provisionada para dimensionamento global](scaling-throughput.md)
* [Distribuição global: Nos bastidores](global-dist-under-the-hood.md)
