---
title: Tokens de recurso do Azure Cosmos DB com Gremlin
description: Saiba como criar tokens de recurso e usá-los para acessar o banco de dados de grafo
author: olignat
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 09/06/2019
ms.author: olignat
ms.openlocfilehash: fcb18fb14cf787713735da07ca2048d0853fa46c
ms.sourcegitcommit: b8578b14c8629c4e4dea4c2e90164e42393e8064
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70807007"
---
# <a name="azure-cosmos-db-resource-tokens-with-gremlin"></a>Tokens de recurso do Azure Cosmos DB com Gremlin
Este artigo explica como usar [Tokens de Recurso do Cosmos DB](secure-access-to-data.md) para acessar um banco de dados de grafo usando o SDK do Gremlin.

## <a name="create-a-resource-token"></a>Criar um token de recurso

O SDK do TinkerPop Gremlin não tem uma API pata criar tokens de recurso. Token de recurso é um conceito do Cosmos DB. Para criar tokens de recurso, baixe o [SDK do Azure Cosmos DB](sql-api-sdk-dotnet.md). Se o aplicativo precisar criar tokens de recurso e usá-los para acessar o banco de dados de grafo, ele precisará de dois SDKs separados.

Hierarquia de modelo de objeto acima dos tokens de recurso:
- **Conta do Cosmos DB** – entidade de nível superior que tem o DNS associado, por exemplo, `contoso.gremlin.cosmos.azure.com`
  - **Banco de dados do Cosmos DB**
    - **Usuário**
      - **Permissão**
        - *Token* – uma propriedade do objeto de **Permissão** que indica quais ações são permitidas ou negadas.

O token de recurso tem um formato `"type=resource&ver=1&sig=<base64 string>;<base64 string>;"`. Essa cadeia de caracteres é opaca para os clientes e deve ser usada no estado em que se encontra, sem modificação nem interpretação.

```csharp
// Notice that document client is created against .NET SDK end-point rather than Gremlin.
DocumentClient client = new DocumentClient(
  new Uri("https://contoso.documents.azure.com:443/"), 
  "<master key>", 
  new ConnectionPolicy 
  {
    EnableEndpointDiscovery = false, 
    ConnectionMode = ConnectionMode.Direct 
  });

  // Read specific permission to obtain a token.
  // Token will not be returned during ReadPermissionReedAsync() call.
  // This call will succeed only if database id, user id and permission id already exist. 
  // Note that <database id> is not a database name, it is a base64 string that represents database identifier, for example "KalVAA==".
  // Similar comment applies to <user id> and <permission id>
  Permission permission = await client.ReadPermissionAsync(UriFactory.CreatePermissionUri("<database id>", "<user id>", "<permission id>"));

  Console.WriteLine("Obtained token {0}", permission.Token);
}
```

## <a name="use-a-resource-token"></a>Usar um token de recurso
Tokens de recurso podem ser usados diretamente como a propriedade "password" ao construir a classe `GremlinServer`.

```csharp
// Gremlin application needs to be given a resource token. It can't discover the token on its own.
// Token can be obtained for a given permission using Cosmos DB SDK or passed into the application as command line argument or configuration value.
string resourceToken = GetResourceToken();

// Configure gremlin servier to use resource token rather than master key
GremlinServer server = new GremlinServer(
  "contoso.gremlin.cosmosdb.azure.com",
  port: 443,
  enableSsl: true,
  username: "/dbs/<database name>/colls/<collection name>",

  // Format of the token is "type=resource&ver=1&sig=<base64 string>;<base64 string>;"
  password: resourceToken);

  using (GremlinClient gremlinClient = new GremlinClient(server, new GraphSON2Reader(), new GraphSON2Writer(), GremlinClient.GraphSON2MimeType))
  {
      await gremlinClient.SubmitAsync("g.V().limit(1)");
  }
```

A mesma abordagem funciona em todos os SDKs do TinkerPop Gremlin.

```java
Cluster.Builder builder = Cluster.build();

AuthProperties authenticationProperties = new AuthProperties();
authenticationProperties.with(AuthProperties.Property.USERNAME,
    String.format("/dbs/%s/colls/%s", "<database name>", "<collection name>"));

// Format of the token is "type=resource&ver=1&sig=<base64 string>;<base64 string>;"
authenticationProperties.with(AuthProperties.Property.PASSWORD, resourceToken);

builder.authProperties(authenticationProperties);
```

## <a name="limit"></a>Limite

Uma única conta do Gremlin pode emitir um número ilimitado de tokens. No entanto, é possível usar apenas até **100** tokens simultaneamente em **1 hora**. Se o aplicativo ultrapassar o limite de tokens por hora, a solicitação de autenticação será negada com a mensagem de erro `"Exceeded allowed resource token limit of 100 that can be used concurrently"`. Fechar conexões ativas com tokens específicos para liberar slots para novos tokens não será proveitoso. O mecanismo de banco de dados Gremlin do Cosmos DB mantém o controle de tokens distintos na última hora antes da solicitação de autenticação.

## <a name="permission"></a>Permissão

Um erro comum com que os aplicativos se deparam enquanto usam os tokens de recurso é `"Insufficient permissions provided in the authorization header for the corresponding request. Please retry with another authorization header."`. Esse erro é retornado quando a travessia do Gremlin tenta gravar uma borda ou um vértice, mas o token de recurso concede somente permissões `Read`. Inspecione sua travessia se ela contiver alguma das seguintes etapas: `.addV()`, `.addE()`, `.drop()` ou `.property()`.

## <a name="next-steps"></a>Próximas etapas
* [Controle de acesso baseado em função](role-based-access-control.md) no Azure Cosmos DB
* [Saiba como proteger o acesso aos dados](secure-access-to-data.md) no Azure Cosmos DB
