---
title: Tutorial para uso de sinalizadores de recurso em um aplicativo .NET Core | Microsoft Docs
description: Neste tutorial, você aprenderá a implementar sinalizadores de recurso em aplicativos .NET Core
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 04/19/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 28ba4397ca5a5fd3c281555238fc7eec8a82943d
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65413662"
---
# <a name="tutorial-use-feature-flags-in-a-net-core-app"></a>Tutorial: Usar sinalizadores de recursos em um aplicativo .NET Core

As bibliotecas de Gerenciamento de Recursos do .NET Core fornecem suporte idiomático para implementação de sinalizadores de recursos em um aplicativo .NET ou ASP.NET Core. Elas permitem que você adicione sinalizadores de recurso ao código de forma mais declarativa, de modo que você não precise escrever todas as instruções `if` para elas manualmente. Elas gerenciam os ciclos de vida do sinalizador de recurso (por exemplo, atualizam e armazenam em cache os estados do sinalizador, garantem um estado de sinalizador imutável durante uma chamada de solicitação) nos bastidores. Além disso, a biblioteca ASP.NET Core oferece integrações prontas para uso, incluindo ações do controlador MVC, exibições, rotas e middleware.

O Início Rápido [Adicionar sinalizadores de recurso a um aplicativo ASP.NET Core](./quickstart-feature-flag-aspnet-core.md) mostra várias maneiras de adicionar sinalizadores de recurso a um aplicativo ASP.NET Core. Este tutorial explica-os mais detalhadamente. Confira a [documentação do gerenciamento de recursos do ASP.NET Core](https://go.microsoft.com/fwlink/?linkid=2091410) para obter uma referência completa.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Adicionar sinalizadores de recursos aos componentes principais de seu aplicativo para controlar a disponibilidade de recursos.
> * Integrá-los à Configuração de Aplicativos ao usá-la para gerenciar sinalizadores de recurso.

## <a name="setup"></a>Configuração

O gerenciador de recursos `IFeatureManager` do .NET Core obtém os sinalizadores de recursos do sistema de configuração nativa da estrutura. Como resultado, você pode definir sinalizadores de recurso de seu aplicativo usando qualquer fonte de configuração compatível com o .NET Core, incluindo o arquivo *appsettings.json* local ou variáveis de ambiente. O gerenciador de recursos se baseia na injeção de dependência do .NET Core. Você pode registrar os serviços de gerenciamento de recursos usando as convenções padrão.

```csharp
using Microsoft.FeatureManagement;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement();
    }
}
```

O gerenciador de recursos recupera os sinalizadores de recursos da seção "FeatureManagement" dos dados de configuração do .NET Core por padrão. O exemplo a seguir instrui-o a fazer a leitura de outra seção chamada "MyFeatureFlags".

```csharp
using Microsoft.FeatureManagement;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement(options =>
        {
                options.UseConfiguration(Configuration.GetSection("MyFeatureFlags"));
        });
    }
}
```

Se você usar um filtro nos sinalizadores de recursos, precisará incluir uma biblioteca adicional e registrá-la. O exemplo a seguir mostra como usar um filtro de recurso interno chamado **PercentageFilter"**.

```csharp
using Microsoft.FeatureManagement;
using Microsoft.FeatureManagement.FeatureFilters;

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement()
                .AddFeatureFilter<PercentageFilter>();
    }
}
```

Para operar com eficiência, você deve manter os sinalizadores de recurso fora do aplicativo e gerenciá-los separadamente. Isso permite que você modifique os estados de sinalizador a qualquer momento e implemente essas alterações no aplicativo imediatamente. A Configuração de Aplicativos fornece um local centralizado para organizar e controlar todos os sinalizadores de recurso por meio de uma interface do usuário de portal dedicado e fornece os sinalizadores ao seu aplicativo diretamente por meio de suas bibliotecas de clientes .NET Core. A maneira mais fácil de conectar seu aplicativo ASP.NET Core à Configuração de Aplicativos é por meio do provedor de configuração `Microsoft.Extensions.Configuration.AzureAppConfiguration`. Use esse pacote NuGet no código adicionando o seguinte ao arquivo *Program.cs*:

```csharp
using Microsoft.Extensions.Configuration.AzureAppConfiguration;

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
           .ConfigureAppConfiguration((hostingContext, config) => {
               var settings = config.Build();
               config.AddAzureAppConfiguration(options => {
                   options.Connect(settings["ConnectionStrings:AppConfig"])
                          .UseFeatureFlags();
                });
           })
           .UseStartup<Startup>();
```

Os valores de sinalizadores de recurso devem ser alterados ao longo do tempo. Por padrão, o gerenciador de recursos atualizará os valores de sinalizadores de recursos a cada 30 segundos. Você pode usar outro intervalo de sondagem na chamada `options.UseFeatureFlags()` acima.

```csharp
config.AddAzureAppConfiguration(options => {
    options.Connect(settings["ConnectionStrings:AppConfig"])
           .UseFeatureFlags(featureFlagOptions => {
                featureFlagOptions.PollInterval = TimeSpan.FromSeconds(5);
           });
});
```

## <a name="feature-flag-declaration"></a>Declaração do sinalizador de recurso

Cada sinalizador de recurso tem duas partes: um nome e uma lista de um ou mais filtros que são usados para avaliar se o estado de um recurso é *ativado* (ou seja, quando seu valor é `True`). Um filtro define um caso de uso para o qual um recurso deve ser ativado. Se um sinalizador de recurso tem vários filtros, a lista de filtro é percorrida na ordem até um dos filtros determinar que o recurso deve ser habilitado. Neste ponto, o sinalizador de recurso é considerado *ativado* e os resultados restantes do filtro são ignorados. Se nenhum filtro indicar que o recurso deve ser habilitado, o sinalizador de recurso estará *desativado*.

O gerenciador de recursos dá suporte ao *appsettings.json* como uma fonte de configuração para sinalizadores de recursos. O exemplo a seguir mostra como configurar sinalizadores de recurso em um arquivo JSON.

```JSON
"FeatureManagement": {
    "FeatureX": true, // Feature flag set to on
    "FeatureY": false, // Feature flag set to off
    "FeatureC": {
        "EnabledFor": [
            {
                "Name": "Percentage",
                "Parameters": {
                    "Value": 50
                }
            }
        ]
    }
}
```

Por convenção, a seção `FeatureManagement` deste documento JSON é usada para configurações de sinalizadores de recurso. O exemplo acima mostra três sinalizadores de recurso com seus filtros definidos na propriedade *EnabledFor*:

* **FeatureA** está *ativado*.
* **FeatureB** está *desativado*.
* **FeatureC** especifica um filtro chamado *Percentage* com uma propriedade *Parameters*. *Percentage* é um exemplo de um filtro configurável e especifica uma probabilidade de 50% para o sinalizador **FeatureC** estar *ativado*.

## <a name="referencing"></a>Referência

Embora não seja necessário, os sinalizadores de recurso devem ser definidos como variáveis `enum` para que possam ser referenciados com facilidade no código.

```csharp
public enum MyFeatureFlags
{
    FeatureA,
    FeatureB,
    FeatureC
}
```

## <a name="feature-flag-check"></a>Verificação do sinalizador de recurso

O padrão básico de gerenciamento de recursos é primeiro verificar se um sinalizador de recursos está definido como *ativado* e, em seguida, executar as ações embutidas, se for o caso.

```csharp
IFeatureManager featureManager;
...
if (featureManager.IsEnabled(nameof(MyFeatureFlags.FeatureA)))
{
    // Run the following code
}
```

## <a name="dependency-injection"></a>Injeção de dependência

No ASP.NET Core MVC, o gerenciador de recursos `IFeatureManager` pode ser acessado por meio da injeção de dependência.

```csharp
public class HomeController : Controller
{
    private readonly IFeatureManager _featureManager;

    public HomeController(IFeatureManager featureManager)
    {
        _featureManager = featureManager;
    }
}
```

## <a name="controller-action"></a>Ação do controlador

Em controladores MVC, um atributo `Feature` pode ser usado para controlar se uma classe de controlador inteira ou uma ação específica será habilitada. O controlador `HomeController` a seguir exige que *FeatureA* seja *ativado* antes que qualquer ação que ele contenha possa ser executada.

```csharp
[Feature(MyFeatureFlags.FeatureA)]
public class HomeController : Controller
{
    ...
}
```

A ação `Index` a seguir exige que *FeatureA* seja *ativado* para ser executado.

```csharp
[Feature(MyFeatureFlags.FeatureA)]
public IActionResult Index()
{
    return View();
}
```

Quando uma ação ou um controlador MVC está bloqueado devido ao sinalizador de recurso de controle estar *desativado*, um `IDisabledFeatureHandler` registrado é chamado. O `IDisabledFeatureHandler` padrão retorna um código de status 404 para o cliente sem nenhum corpo de resposta.

## <a name="view"></a>Visualizar

Em exibições do MVC, uma marca `<feature>` pode ser usada para renderizar o conteúdo com base em se um sinalizador de recurso está habilitado ou não.

```html
<feature name="FeatureA">
    <p>This can only be seen if 'FeatureA' is enabled.</p>
</feature>
```

## <a name="mvc-filter"></a>Filtro do MVC

Os filtros do MVC podem ser configurados de modo que sejam ativados com base no estado de um sinalizador de recurso. O exemplo a seguir adiciona um filtro do MVC chamado `SomeMvcFilter`. Esse filtro só é disparado no pipeline do MVC se *FeatureA* está habilitado.

```csharp
using Microsoft.FeatureManagement.FeatureFilters;

IConfiguration Configuration { get; set;}

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options => {
        options.Filters.AddForFeature<SomeMvcFilter>(nameof(MyFeatureFlags.FeatureA));
    });
}
```

## <a name="route"></a>Rota

As rotas podem ser expostas dinamicamente com base nos sinalizadores de recurso. O exemplo a seguir adiciona uma rota, que define `Beta` como o controlador padrão, somente quando *FeatureA* está habilitado.

```csharp
app.UseMvc(routes => {
    routes.MapRouteForFeature(nameof(MyFeatureFlags.FeatureA), "betaDefault", "{controller=Beta}/{action=Index}/{id?}");
});
```

## <a name="middleware"></a>Middleware

Os sinalizadores de recurso podem ser usados para adicionar branches de aplicativos e middleware condicionalmente. O exemplo a seguir insere um componente de middleware no pipeline de solicitação somente quando *FeatureA* está habilitado.

```csharp
app.UseMiddlewareForFeature<ThirdPartyMiddleware>(nameof(MyFeatureFlags.FeatureA));
```

Isso cria a funcionalidade mais genérica para gerar um branch de todo o aplicativo com base em um sinalizador de recurso.

```csharp
app.UseForFeature(featureName, appBuilder => {
    appBuilder.UseMiddleware<T>();
});
```

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a implementar sinalizadores de recurso em seu aplicativo ASP.NET Core utilizando as bibliotecas `Microsoft.FeatureManagement`. Confira os recursos a seguir para obter mais informações sobre o suporte de gerenciamento de recursos no ASP.NET Core e na Configuração de Aplicativos.

* [Código de exemplo do sinalizador de recurso do ASP.NET Core]()
* [Documentação de Microsoft.FeatureManagement]()
* [Gerenciar sinalizadores de recursos](./manage-feature-flags.md)
