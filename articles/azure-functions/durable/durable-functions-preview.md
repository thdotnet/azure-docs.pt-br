---
title: Recursos - Azure Functions de visualização de funções duráveis
description: Saiba mais sobre recursos de visualização para funções duráveis.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: article
ms.date: 07/08/2019
ms.author: azfuncdf
ms.openlocfilehash: 7101519aa4a87995dac3a7f11046eed84a2c09b6
ms.sourcegitcommit: af31deded9b5836057e29b688b994b6c2890aa79
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812771"
---
# <a name="durable-functions-20-preview-azure-functions"></a>Versão prévia 2.0 de funções durável (Azure Functions)

As *Funções Duráveis* são uma extensão do [Azure Functions](../functions-overview.md) e do [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md) que permitem que você escreva funções com estado em um ambiente sem servidor. A extensão gerencia estado, pontos de verificação e reinicializações para você. Se você já não estiver familiarizado com as funções duráveis, consulte o [documentação de visão geral](durable-functions-overview.md).

As funções duráveis 1.x é um recurso de GA (disponibilidade geral) do Azure Functions, mas também contém vários sub-recursos que estão atualmente em visualização pública. Este artigo descreve os recursos de visualização recém-lançado e apresenta detalhes sobre como eles funcionam e como você pode começar a usá-los.

> [!NOTE]
> Estes recursos de visualização são parte de uma versão 2.0 de funções duráveis, o que é atualmente uma **versão de qualidade de visualização** com várias alterações significativas. O Azure Functions durável compilações do pacote de extensão podem ser encontrada em nuget.org com versões na forma de **2.0.0-betaX**. Essas compilações não se destinam para cargas de trabalho de produção e versões posteriores podem conter alterações significativas adicionais.

## <a name="breaking-changes"></a>Alterações de última hora

Várias alterações significativas são apresentadas no 2.0 de funções duráveis. Os aplicativos existentes não devem ser compatíveis com 2.0 de funções duráveis sem alterações de código. Esta seção lista algumas das alterações:

### <a name="hostjson-schema"></a>Esquema de host. JSON

O trecho a seguir mostra o novo esquema para o host. JSON. As principais alterações a serem consideradas são as subseções a nova:

* `"storageProvider"` (e o `"azureStorage"` subseção) para a configuração de armazenamento específico
* `"tracking"` para acompanhamento e a configuração de registro em log
* `"notifications"` (e o `"eventGrid"` subseção) para a configuração de notificação de grade de eventos

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "azureStorage": {
          "connectionStringName": <string>,
          "controlQueueBatchSize": <int?>,
          "partitionCount": <int?>,
          "controlQueueVisibilityTimeout": <hh:mm:ss?>,
          "workItemQueueVisibilityTimeout": <hh:mm:ss?>,
          "trackingStoreConnectionStringName": <string?>,
          "trackingStoreNamePrefix": <string?>,
          "maxQueuePollingInterval": <hh:mm:ss?>
        }
      },
      "tracking": {
        "traceInputsAndOutputs": <bool?>,
        "traceReplayEvents": <bool?>,
      },
      "notifications": {
        "eventGrid": {
          "topicEndpoint": <string?>,
          "keySettingName": <string?>,
          "publishRetryCount": <string?>,
          "publishRetryInterval": <hh:mm:ss?>,
          "publishRetryHttpStatus": <int[]?>,
          "publishEventTypes": <string[]?>,
        }
      },
      "maxConcurrentActivityFunctions": <int?>,
      "maxConcurrentOrchestratorFunctions": <int?>,
      "extendedSessionsEnabled": <bool?>,
      "extendedSessionIdleTimeoutInSeconds": <int?>,
      "customLifeCycleNotificationHelperType": <string?>
  }
}
```

Como durável Functions 2.0 continua a se estabilizar, mais alterações serão apresentaremos o `durableTask` seção host. JSON. Para obter mais informações sobre essas alterações, consulte [esse problema de GitHub](https://github.com/Azure/azure-functions-durable-extension/issues/641).

### <a name="public-interface-changes"></a>Alterações de interface pública

Os diversos objetos de "contexto" compatíveis com as funções duráveis tinham classes base abstratas destinadas para uso em testes de unidade. Como parte do 2.0 de funções duráveis, essas classes base abstratas foram substituídos com interfaces. Código de função que usa os tipos concretos diretamente não são afetados.

A tabela a seguir representa as principais alterações:

| Tipo antigo | Novo tipo |
|----------|----------|
| DurableOrchestrationClientBase | IDurableOrchestrationClient |
| DurableOrchestrationContextBase | IDurableOrchestrationContext |
| DurableActivityContextBase | IDurableActivityContext |

No caso em que uma classe base abstrata contido métodos virtuais, esses métodos virtuais foram substituídos por métodos de extensão definidos em `DurableContextExtensions`.

## <a name="entity-functions"></a>Funções da entidade

Definem as funções da entidade operações para ler e atualizar pequenos pedaços de estado, conhecido como *entidades duráveis*. Como funções de orquestrador, funções de entidade são funções com um tipo especial de gatilho, *disparador entidade*. Diferentemente das funções de orquestrador, funções da entidade não tem quaisquer restrições de código específico. Funções de entidade também gerenciam estado explicitamente em vez de implicitamente que representa o estado por meio do fluxo de controle.

### <a name="net-programing-models"></a>Modelos de programação do .NET

Há dois modelos de programação opcionais para a criação de entidades duráveis. O código a seguir é um exemplo de um simples *contador* entidade implementada como uma função padrão. Essa função define três *operações*, `add`, `reset`, e `get`, cada do que operam em um valor de estado de inteiro, `currentValue`.

```csharp
[FunctionName("Counter")]
public static void Counter([EntityTrigger] IDurableEntityContext ctx)
{
    int currentValue = ctx.GetState<int>();

    switch (ctx.OperationName.ToLowerInvariant())
    {
        case "add":
            int amount = ctx.GetInput<int>();
            currentValue += operand;
            break;
        case "reset":
            currentValue = 0;
            break;
        case "get":
            ctx.Return(currentValue);
            break;
    }

    ctx.SetState(currentValue);
}
```

Esse modelo funciona melhor para implementações de entidade simples ou implementações que têm um conjunto dinâmico de operações. No entanto, há também um modelo de programação baseado em classe que é útil para entidades que são estáticos, mas têm implementações mais complexas. O exemplo a seguir é uma implementação equivalente do `Counter` entidade usando métodos e classes do .NET.

```csharp
public class Counter
{
    [JsonProperty("value")]
    public int CurrentValue { get; set; }

    public void Add(int amount) => this.CurrentValue += amount;
    
    public void Reset() => this.CurrentValue = 0;
    
    public int Get() => this.CurrentValue;

    [FunctionName(nameof(Counter))]
    public static Task Run([EntityTrigger] IDurableEntityContext ctx)
        => ctx.DispatchAsync<Counter>();
}
```

O modelo baseado em classe é semelhante ao modelo de programação popularizado por [Orleans](https://www.microsoft.com/research/project/orleans-virtual-actors/). Nesse modelo, um tipo de entidade é definido como uma classe .NET. Cada método da classe é uma operação que pode ser invocada por um cliente externo. No entanto, ao contrário de Orleans, interfaces .NET são opcionais. Anterior *contador* exemplo não tiver usado uma interface, mas ele ainda pode ser invocado por meio de outras funções ou por meio de chamadas de API HTTP.

Entidade *instâncias* são acessadas por meio de um identificador exclusivo, o *ID da entidade*. Uma ID de entidade é simplesmente um par de cadeias de caracteres que identifica exclusivamente uma instância de entidade. Ele consiste em:

* Uma **nome da entidade**: um nome que identifica o tipo da entidade (por exemplo, "Counter").
* Uma **chave de entidade**: uma cadeia de caracteres que identifica exclusivamente a entidade entre todas as outras entidades do mesmo nome (por exemplo, um GUID).

Por exemplo, uma *contador* função de entidade pode ser usada para manter a pontuação em um jogo online. Cada instância do jogo terá uma ID de entidade exclusivo, como `@Counter@Game1`, `@Counter@Game2`e assim por diante.

### <a name="comparison-with-virtual-actors"></a>Comparação com atores virtuais

O design de entidades durável é amplamente influenciado pelo [modelo de ator](https://en.wikipedia.org/wiki/Actor_model). Se você já estiver familiarizado com os atores, os conceitos por trás de entidades duráveis devem ser familiares para você. Em particular, as entidades duráveis são semelhantes às [atores virtuais](https://research.microsoft.com/projects/orleans/) de várias maneiras:

* Entidades duráveis são endereçáveis por meio de um *ID da entidade*.
* Entidade durável operações são executadas em série, um de cada vez, para evitar condições de corrida.
* Entidades duráveis são criadas automaticamente quando eles são chamados ou sinalizados.
* Quando a não execução de operações, entidades duráveis são silenciosamente descarregadas da memória.

Há algumas diferenças importantes, no entanto, que vale a pena observar:

* Entidades duráveis priorizar *durabilidade* pela *latência*e, portanto, pode não ser adequado para aplicativos com requisitos estritos de latência.
* As mensagens enviadas entre as entidades são entregues com confiança e em ordem.
* Entidades duráveis podem ser usadas junto com orquestrações duráveis e podem servir como bloqueios distribuídos, que são descritos neste artigo.
* Padrões de solicitação/resposta nas entidades estão limitados a orquestrações. Para a comunicação de entidade de entidade, apenas a mensagens unidirecionais (também conhecido como "sinalização") são permitidas, como no modelo de ator original. Esse comportamento impede deadlocks distribuídos.

### <a name="durable-entity-net-apis"></a>Entidade durável APIs do .NET

Suporte ao Entity envolve várias APIs. Por exemplo, há uma nova API para definir funções de entidade, como mostrado acima, que especificam o que deve acontecer quando uma operação é invocada em uma entidade. Além disso, as APIs existentes para clientes e orquestrações foram atualizadas com a nova funcionalidade para interação com entidades.

#### <a name="implementing-entity-operations"></a>Implementar operações de entidade

A execução de uma operação em uma entidade pode chamar esses membros no objeto de contexto (`IDurableEntityContext` no .NET):

* **OperationName**: obtém o nome da operação.
* **GetInput\<TInput >** : obtém a entrada para a operação.
* **GetState\<TState >** : obtém o estado atual da entidade.
* **SetState**: atualiza o estado da entidade.
* **SignalEntity**: envia uma mensagem unidirecional para uma entidade.
* **Self**: obtém a ID da entidade.
* **Retornar**: retorna um valor para o cliente ou uma orquestração que chamou a operação.
* **IsNewlyConstructed**: retorna `true` se a entidade não existia antes da operação.
* **DestructOnExit**: exclui a entidade depois de concluir a operação.

As operações são menos restritas que orquestrações:

* Operações podem chamar e/s externos, usando as APIs síncronas ou assíncronas (é recomendável usar apenas as assíncronas).
* Operações podem ser não determinístico. Por exemplo, é seguro chamar `DateTime.UtcNow`, `Guid.NewGuid()` ou `new Random()`.

#### <a name="accessing-entities-from-clients"></a>Acessar as entidades de clientes

Entidades duráveis podem ser chamadas de funções comuns por meio de `orchestrationClient` associação (`IDurableOrchestrationClient` no .NET). Há suporte para os seguintes métodos:

* **ReadEntityStateAsync\<T >** : lê o estado de uma entidade.
* **SignalEntityAsync**: envia uma mensagem unidirecional para uma entidade e aguarda até que ele seja enfileirado.
* **SignalEntityAsync\<T >** : igual `SignalEntityAsync` , mas usa um objeto de proxy gerado do tipo `T`.

Anterior `SignalEntityAsync` chamada requer especificando o nome da operação de entidade como uma `string` e a carga da operação como um `object`. O código de exemplo a seguir é um exemplo desse padrão:

```csharp
EntityId id = // ...
object amount = 5;
context.SignalEntityAsync(id, "Add", amount);
```

Também é possível gerar um objeto de proxy para acesso fortemente tipado. Para gerar um proxy de tipo seguro, o tipo de entidade deve implementar uma interface. Por exemplo, suponha que o `Counter` implementado entidade mencionada anteriormente um `ICounter` interface, definida da seguinte maneira:

```csharp
public interface ICounter
{
    void Add(int amount);
    void Reset();
    int Get();
}

public class Counter : ICounter
{
    // ...
}
```

Código do cliente pode usar `SignalEntityAsync<T>` e especifique o `ICounter` interface como parâmetro de tipo para gerar um proxy de tipo seguro. Esse uso de proxies de tipo seguro é demonstrado no exemplo de código a seguir:

```csharp
[FunctionName("UserDeleteAvailable")]
public static async Task AddValueClient(
    [QueueTrigger("my-queue")] string message,
    [OrchestrationClient] IDurableOrchestrationClient client)
{
    int amount = int.Parse(message);
    var target = new EntityId(nameof(Counter), "MyCounter");
    await client.SignalEntityAsync<ICounter>(target, proxy => proxy.Add(amount));
}
```

No exemplo anterior, o `proxy` parâmetro é uma instância gerada dinamicamente do `ICounter`, que converte internamente a chamada para `Add` para o equivalente (sem tipo) chamar `SignalEntityAsync`.

> [!NOTE]
> É importante observar que o `ReadEntityStateAsync` e `SignalEntityAsync` métodos de `IDurableOrchestrationClient` priorizar desempenho sobre a consistência. `ReadEntityStateAsync` pode retornar um valor obsoleto, e `SignalEntityAsync` pode retornar antes da operação for concluída.

#### <a name="accessing-entities-from-orchestrations"></a>Acessar as entidades de orquestrações

Orquestrações podem acessar entidades usando o `IDurableOrchestrationContext` objeto. Eles podem escolher entre comunicação unidirecional (disparar e esquecer) e a comunicação bidirecional (solicitação e resposta). Os respectivos métodos são:

* **SignalEntity**: envia uma mensagem unidirecional para uma entidade.
* **CallEntityAsync**: envia uma mensagem para uma entidade e aguarda uma resposta que indica se a operação foi concluída.
* **CallEntityAsync\<T >** : envia uma mensagem a uma entidade e aguarda uma resposta que contém um resultado do tipo T.

Ao usar a comunicação bidirecional, todas as exceções geradas durante a execução da operação também são transmitidas novamente para a orquestração chamada e gerada novamente. Por outro lado, ao usar disparar e esquecer, as exceções não são observadas.

Para acesso de segurança de tipos, funções de orquestração podem gerar proxies com base em uma interface. O `CreateEntityProxy` método de extensão pode ser usado para essa finalidade:

```csharp
public interface IAsyncCounter
{
    Task AddAsync(int amount);
    Task ResetAsync();
    Task<int> GetAsync();
}

[FunctionName("CounterOrchestration)]
public static async Task Run(
    [OrchestrationTrigger] IDurableOrchestrationContext context)
{
    // ...
    IAsyncCounter proxy = context.CreateEntityProxy<IAsyncCounter>("MyCounter");
    await proxy.AddAsync(5);
    int newValue = await proxy.GetAsync();
    // ...
}
```

No exemplo anterior, uma entidade "counter" foi considerada existe que implementa o `IAsyncCounter` interface. A orquestração, em seguida, foi capaz de usar o `IAsyncCounter` definição para gerar um tipo de proxy para interação de forma síncrona com a entidade de tipo.

### <a name="locking-entities-from-orchestrations"></a>Entidades de bloqueio de orquestrações

Orquestrações podem bloquear entidades. Esse recurso fornece uma maneira simples de evitar corridas indesejadas usando *seções críticas*.

O objeto de contexto fornece os seguintes métodos:

* **LockAsync**: adquire bloqueios em uma ou mais entidades.
* **IsLocked**: retorna true se estiver atualmente em uma seção crítica, false caso contrário.

A seção crítica termina e todos os bloqueios são liberados, quando termina a orquestração. No .NET, `LockAsync` retorna um `IDisposable` termina a seção crítica quando descartado, o que pode ser usada junto com um `using` cláusula para obter uma representação sintática da seção crítica.

Por exemplo, considere uma orquestração que precisa para testar se dois jogadores estão disponíveis e, em seguida, atribuí-los para um jogo. Essa tarefa pode ser implementada usando uma seção crítica da seguinte maneira:

```csharp
[FunctionName("Orchestrator")]
public static async Task RunOrchestrator(
    [OrchestrationTrigger] IDurableOrchestrationContext ctx)
{
    EntityId player1 = /* ... */;
    EntityId player2 = /* ... */;

    using (await ctx.LockAsync(player1, player2))
    {
        bool available1 = await ctx.CallEntityAsync<bool>(player1, "is-available");
        bool available2 = await ctx.CallEntityAsync<bool>(player2, "is-available");

        if (available1 && available2)
        {
            Guid gameId = ctx.NewGuid();

            await ctx.CallEntityAsync(player1, "assign-game", gameId);
            await ctx.CallEntityAsync(player2, "assign-game", gameId);
        }
    }
}
```

Dentro da seção crítica, ambas as entidades de player são bloqueadas, que significa que elas não estiverem em execução a todas as operações que não sejam aqueles que são chamados de dentro da seção crítica). Esse comportamento impede corridas com operações conflitantes, como players de que está sendo atribuídos a outro jogo ou assinatura desativada.

Podemos impõe várias restrições em seções críticas como pode ser usado. Essas restrições servem para evitar deadlocks e reentrância.

* Seções críticas não podem ser aninhadas.
* Seções críticas não é possível criar suborchestrations.
* Seções críticas podem chamar apenas as entidades que eles bloqueados.
* Seções críticas não é possível chamar a mesma entidade usando várias chamadas paralelas.
* Seções críticas podem sinalizar apenas as entidades que eles não bloqueados.

## <a name="alternate-storage-providers"></a>Provedores de armazenamento alternativo

O Framework de tarefa durável dá suporte a vários provedores de armazenamento atualmente, incluindo [armazenamento do Azure](https://github.com/Azure/durabletask/tree/master/src/DurableTask.AzureStorage), [do barramento de serviço do Azure](https://github.com/Azure/durabletask/tree/master/src/DurableTask.ServiceBus), um [emulador na memória](https://github.com/Azure/durabletask/tree/master/src/DurableTask.Emulator)e um experimental [Redis](https://github.com/Azure/durabletask/tree/redis/src/DurableTask.Redis) provedor. No entanto, até agora, a extensão de tarefa durável do Azure Functions só tem suporte do provedor de armazenamento do Azure. A partir do 2.0 de funções duráveis, suporte para provedores de armazenamento alternativo está sendo adicionada, começando com o provedor do Redis.

> [!NOTE]
> Durável Functions 2.0 dá suporte apenas a provedores de .NET Standard 2.0 compatível. No momento da escrita, o provedor de barramento de serviço do Azure não oferece suporte a .NET Standard 2.0 e, portanto, não está disponível como um provedor de armazenamento alternativo.

### <a name="emulator"></a>Emulador

O [DurableTask.Emulator](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Emulator/) provedor é uma memória local, o provedor de armazenamento não durável adequado para cenários de teste locais. Ele pode ser configurado usando o seguinte mínimo **host. JSON** esquema:

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "emulator": { }
      }
    }
  }
}
```

### <a name="redis-experimental"></a>Redis (experimental)

O [DurableTask.Redis](https://www.nuget.org/packages/Microsoft.Azure.DurableTask.Redis/) provedor persiste todos os estados de orquestração para um cluster Redis configurado.

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": <string>,
      "storageProvider": {
        "redis": {
          "connectionStringName": <string>,
        }
      }
    }
  }
}
```

O `connectionStringName` deve fazer referência ao nome de uma variável de ambiente ou configuração de aplicativo. Essa variável de ambiente ou configuração de aplicativo deve conter um valor de cadeia de caracteres de conexão do Redis na forma de *porta do servidor:* . Por exemplo, `localhost:6379` para se conectar a um cluster local do Redis.

> [!NOTE]
> O provedor de Redis está atualmente experimental e só oferece suporte a aplicativos de função em execução em um único nó. Não há garantia de que o provedor de Redis nunca será feito em disponibilidade geral, e ele pode ser removido em uma versão futura.
