---
title: Usar injeção de dependência no .NET do Azure Functions
description: Saiba como usar a injeção de dependência para registrar e usar os serviços em funções do .NET
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: azure functions, funções, arquitetura sem servidor
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/28/2019
ms.author: jehollan, glenga, cshoe
ms.openlocfilehash: b1a6751f0d788c26af60b28eee994dc9b3877f00
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66693256"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>Usar injeção de dependência no .NET do Azure Functions

O Azure Functions dá suporte ao padrão de design a dependência DI (injeção) software, que é uma técnica para alcançar [inversão de controle (IoC)](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) entre classes e suas dependências.

O Azure Functions se baseia nos recursos de injeção de dependência do ASP.NET Core. Esteja ciente de que os serviços, tempos de vida e padrões de design de [injeção de dependência do ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) antes de usar recursos de injeção de dependência em um Azure Functions é recomendável o aplicativo.

## <a name="prerequisites"></a>Pré-requisitos

Antes de poder usar injeção de dependência, você deve instalar os seguintes pacotes NuGet:

- [Microsoft.Azure.Functions.Extensions](https://www.nuget.org/packages/Microsoft.Azure.Functions.Extensions/)

- [Pacote de microsoftnetsdkfunctions](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/) versão 1.0.28 ou posterior

## <a name="register-services"></a>Serviços de registro

Para registrar os serviços, você pode criar um método para configurar e adicionar componentes a um `IFunctionsHostBuilder` instância.  O host do Azure Functions cria uma instância de `IFunctionsHostBuilder` e passá-lo diretamente em seu método.

Para registrar o método, adicione o `FunctionsStartup` atributo de assembly que especifica o nome do tipo usado durante a inicialização.

```csharp
[assembly: FunctionsStartup(typeof(MyNamespace.Startup))]

namespace MyNamespace
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            builder.Services.AddHttpClient();
            builder.Services.AddSingleton((s) => {
                return new CosmosClient(Environment.GetEnvironmentVariable("COSMOSDB_CONNECTIONSTRING"));
            });
            builder.Services.AddSingleton<ILoggerProvider, MyLoggerProvider>();
        }
    }
}
```

## <a name="use-injected-dependencies"></a>Usar dependências injetadas

O ASP.NET Core usa a injeção de construtor para disponibilizar suas dependências para sua função. O exemplo a seguir demonstra como o `IMyService` e `HttpClient` dependências são injetadas em uma função disparada por HTTP.

```csharp
namespace MyNamespace
{
    public class HttpTrigger
    {
        private readonly IMyService _service;
        private readonly HttpClient _client;

        public HttpTrigger(IMyService service, IHttpClientFactory httpClientFactory)
        {
            _service = service;
            _client = httpClientFactory.CreateClient();;
        }

        [FunctionName("GetPosts")]
        public async Task<IActionResult> Get(
            [HttpTrigger(AuthorizationLevel.Function, "get", Route = "posts")] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            var res = await _client.GetAsync("https://microsoft.com");
            await _service.AddResponse(res);

            return new OkResult();
        }
    }
}
```

O uso de injeção de construtor significa que você não deve usar funções estáticas, se você quiser tirar proveito de injeção de dependência.

## <a name="service-lifetimes"></a>Tempos de vida do serviço

Os aplicativos de funções do Azure fornecem os mesmos tempos de vida do serviço como [injeção de dependência do ASP.NET](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes): temporário, escopo, singleton e.

Em um aplicativo de funções, um tempo de vida do serviço com escopo corresponde a um tempo de vida de execução de função. Os serviços com escopo são criados uma vez por execução. As solicitações posteriores para o serviço durante a execução reutilizar a instância de serviço existente. Um tempo de vida do serviço singleton corresponde ao tempo de vida do host e é reutilizado em execuções de função nessa instância.

Serviços de tempo de vida singleton são recomendados para conexões de clientes e, por exemplo `SqlConnection`, `CloudBlobClient`, ou `HttpClient` instâncias.

Exibir ou baixar uma [exemplo de tempos de vida de serviço diferentes](https://aka.ms/functions/di-sample) no GitHub.

## <a name="logging-services"></a>Serviços de registro em log

Se você precisar de seu próprio provedor de registro em log, a maneira recomendada é registrar um `ILoggerProvider` instância. Application Insights é adicionado automaticamente pelo Azure Functions.

> [!WARNING]
> Não adicione `AddApplicationInsightsTelemetry()` à coleção de serviços, como ele registra serviços em conflito com os serviços fornecidos pelo ambiente.

## <a name="function-app-provided-services"></a>Serviços de aplicativo fornecida da função

O host de função registra vários serviços. Os seguintes serviços estão seguros como uma dependência em seu aplicativo:

|Tipo de serviço|Tempo de vida|DESCRIÇÃO|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|singleton|Configuração de tempo de execução|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|singleton|Responsável por fornecer a ID da instância do host|

Se houver outros serviços que você deseja assumir uma dependência, [criar um problema e propõe-se no GitHub](https://github.com/azure/azure-functions-host).

### <a name="overriding-host-services"></a>Substituindo serviços de host

Substituir serviços fornecidos pelo host não é suportado atualmente.  Se houver serviços que você deseja substituir, [criar um problema e propõe-se no GitHub](https://github.com/azure/azure-functions-host).

## <a name="next-steps"></a>Próximas etapas

Para saber mais, consulte os recursos a seguir:

- [Como monitorar seu aplicativo de funções](functions-monitoring.md)
- [Práticas recomendadas para funções](functions-best-practices.md)
