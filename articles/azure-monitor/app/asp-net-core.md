---
title: O Azure Application Insights para aplicativos ASP.NET Core | Microsoft Docs
description: Monitorar aplicativos web ASP.NET Core de disponibilidade, desempenho e uso.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: mbullwin
ms.openlocfilehash: 5ea7ec41ccc721e8eafda56aa7463505ba089845
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827802"
---
# <a name="application-insights-for-aspnet-core-applications"></a>Application Insights para aplicativos ASP.NET Core

Este artigo descreve como habilitar o Application Insights para um [ASP.NET Core](https://docs.microsoft.com/aspnet/core) aplicativo. Quando você concluir as instruções neste artigo, o Application Insights coletará solicitações, dependências, exceções, contadores de desempenho, as pulsações e logs de seu aplicativo ASP.NET Core. 

O exemplo, usaremos aqui é um [aplicativo MVC](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app) que tem como destino `netcoreapp2.2`. Você pode aplicar estas instruções para todos os aplicativos ASP.NET Core.

## <a name="supported-scenarios"></a>Cenários com suporte

O [SDK do Application Insights para ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) pode monitorar seus aplicativos, independentemente de onde ou como executarem. Se seu aplicativo está em execução e tem conectividade de rede para o Azure, a telemetria pode ser coletada. Monitoramento do Application Insights tem suporte em qualquer lugar, há suporte para .NET Core. O suporte abrange:
* **Sistema operacional**: Windows, Linux ou Mac.
* **Método de hospedagem**: No processo ou fora do processo. 
* **Método de implantação**: Estrutura dependente ou autocontida.
* **Servidor Web**: O IIS (Internet Information Server) ou o Kestrel. 
* **Plataforma de hospedagem**: O recurso de aplicativos Web do serviço de aplicativo do Azure, VM do Azure, Docker, serviço de Kubernetes do Azure (AKS) e assim por diante.
* **IDE**: Visual Studio, VS Code ou linha de comando.

## <a name="prerequisites"></a>Pré-requisitos

- Um aplicativo ASP.NET Core funcional. Se você precisar criar um aplicativo ASP.NET Core, siga este [tutorial do ASP.NET Core](https://docs.microsoft.com/aspnet/core/getting-started/).
- Uma chave de instrumentação do Application Insights válida. Essa chave é necessária para enviar qualquer telemetria ao Application Insights. Se você precisar criar um novo recurso do Application Insights para obter uma chave, consulte de instrumentação [criar um recurso do Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource).

## <a name="enable-application-insights-server-side-telemetry-visual-studio"></a>Habilitar a telemetria do lado do servidor do Application Insights (Visual Studio)

1. Abra o projeto no Visual Studio.

    > [!TIP]
    > Se você quiser, você pode definir o controle de origem para o seu projeto para que você possa acompanhar todas as alterações que torna o Application Insights. Para habilitar o controle do código-fonte, selecione **arquivo** > **adicionar ao controle do código-fonte**.

2. Selecione **Projeto** > **Adicionar Application Insights Telemetry**.

3. Selecione **Introdução**. Texto da seleção, este pode variar, dependendo da sua versão do Visual Studio. Algumas versões anteriores usam um **iniciar gratuitamente** botão em vez disso.

4. Selecione sua assinatura. Em seguida, selecione **Resource** > **registrar**.

5. Depois de adicionar o Application Insights ao seu projeto, verifique para confirmar que você está usando a versão estável mais recente do SDK. Vá para **Project** > **gerenciar pacotes NuGet** > **aspnetcore**. Se for necessário, escolha **atualização**.

     ![Captura de tela mostrando onde selecionar o pacote do Application Insights para atualização](./media/asp-net-core/update-nuget-package.png)

6. Se você seguido a dica opcional e adicionados a seu projeto ao controle de origem, acesse **modo de exibição** > **Team Explorer** > **alterações**. Em seguida, selecione cada arquivo para ver uma exibição de comparação das alterações feitas por telemetria do Application Insights.

## <a name="enable-application-insights-server-side-telemetry-no-visual-studio"></a>Habilitar a telemetria do Application Insights do lado do servidor (sem o Visual Studio)

1. Instalar o [pacote do NuGet do SDK do Application Insights para ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore). É recomendável que você sempre use a versão estável mais recente. Encontra as notas de versão completa para o SDK na [repositório GitHub de código-fonte aberto](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases).

    O exemplo de código a seguir mostra as alterações a serem adicionados ao seu projeto `.csproj` arquivo.

    ```xml
        <ItemGroup>
          <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.7.0" />
        </ItemGroup>
    ```

2. Adicione `services.AddApplicationInsightsTelemetry();` para o `ConfigureServices()` método na sua `Startup` classe, como neste exemplo:

    ```csharp
        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            // The following line enables Application Insights telemetry collection.
            services.AddApplicationInsightsTelemetry();
    
            // This code adds other services for your application.
            services.AddMvc();
        }
    ```

3. Configure a chave de instrumentação.

    Embora você possa fornecer a chave de instrumentação como um argumento para `AddApplicationInsightsTelemetry`, recomendamos que você especifique a chave de instrumentação na configuração. O exemplo de código a seguir mostra como especificar uma chave de instrumentação em `appsettings.json`. Certifique-se de `appsettings.json` é copiado para a pasta raiz do aplicativo durante a publicação.

    ```json
        {
          "ApplicationInsights": {
            "InstrumentationKey": "putinstrumentationkeyhere"
          },
          "Logging": {
            "LogLevel": {
              "Default": "Warning"
            }
          }
        }
    ```

    Como alternativa, especifique a chave de instrumentação em qualquer uma das seguintes variáveis de ambiente:

    * `APPINSIGHTS_INSTRUMENTATIONKEY`

    * `ApplicationInsights:InstrumentationKey`

    Por exemplo:

    * `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

    * `SET APPINSIGHTS_INSTRUMENTATIONKEY=putinstrumentationkeyhere`

    Normalmente, `APPINSIGHTS_INSTRUMENTATIONKEY` Especifica a chave de instrumentação para aplicativos implantados para os aplicativos Web.

    > [!NOTE]
    > Uma chave de instrumentação especificada no código wins sobre a variável de ambiente `APPINSIGHTS_INSTRUMENTATIONKEY`, qual vence sobre outras opções.

## <a name="run-your-application"></a>Execute seu aplicativo.

Executar o aplicativo e fazer solicitações a ele. Telemetria agora deve fluir para o Application Insights. SDK do Application Insights coleta automaticamente a telemetria a seguir.

|Solicitações/dependências |Detalhes|
|---------------|-------|
|Requests | Solicitações de entrada da web ao seu aplicativo. |
|HTTP ou HTTPS | Chamadas feitas com `HttpClient`. |
|SQL | Chamadas feitas com `SqlClient`. |
|[Armazenamento do Azure](https://www.nuget.org/packages/WindowsAzure.Storage/) | Chamadas feitas com o cliente de armazenamento do Azure. |
|[SDK do cliente de hubs de eventos](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | Versão 1.1.0 e versões posteriores. |
|[SDK do cliente do barramento de serviço](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus)| Versão 3.0.0 e posterior. |
|Azure Cosmos DB | Acompanhada automaticamente somente se o HTTP/HTTPS é usado. Application Insights não captura o modo TCP. |

### <a name="performance-counters"></a>contadores de desempenho

Suporte para [contadores de desempenho](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/) no ASP.NET Core é limitada:

   * Versões do SDK 2.4.1 e posteriormente coletar contadores se o aplicativo estiver em execução nos aplicativos Web (Windows).
   * 2\.7.0-beta3 de versões do SDK e posterior desempenho coletar contadores de se o aplicativo estiver em execução no Windows e destinos `NETSTANDARD2.0` ou posterior.
   * Para aplicativos destinados ao .NET Framework, todas as versões do SDK oferece suporte a contadores de desempenho.
 
Este artigo será atualizado quando o suporte de contador de desempenho no Linux é adicionado.

### <a name="ilogger-logs"></a>Logs de ILogger

[Logs de ILogger](https://docs.microsoft.com/azure/azure-monitor/app/ilogger) de gravidade `Warning` ou superior são automaticamente capturados no SDK versões 2.7.0-beta3 e versões posteriores.

### <a name="live-metrics"></a>Métricas em tempo real

Pode levar alguns minutos antes do início de telemetria que aparecem no portal. Para fazer rapidamente se tudo está funcionando, é melhor usar [Live Metrics](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream) quando você faz solicitações para o aplicativo em execução.

## <a name="enable-client-side-telemetry-for-web-applications"></a>Habilitar a telemetria do lado do cliente para aplicativos da web

As etapas anteriores são suficientes para ajudar você a começar a coleta de telemetria do lado do servidor. Se seu aplicativo tiver componentes do lado do cliente, siga as próximas etapas para iniciar a coleta [telemetria de uso](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview).

1. No `_ViewImports.cshtml`, injeção de adicionar:

    ```cshtml
        @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
    ```

2. Na `_Layout.cshtml`, insira `HtmlHelper` no final do `<head>` seção, mas antes de qualquer outro script. Se você desejar relatar qualquer telemetria personalizada do JavaScript na página, injetá-la após este trecho de código:

    ```cshtml
        @Html.Raw(JavaScriptSnippet.FullScript)
        </head>
    ```

O `.cshtml` nomes de arquivo mencionados anteriormente são de um modelo de aplicativo do MVC padrão. Por fim, se você quiser habilitar o monitoramento do lado do cliente para seu aplicativo corretamente, o trecho de JavaScript deve aparecer no `<head>` seção de cada página do seu aplicativo que você deseja monitorar. Você pode atingir essa meta para este modelo de aplicativo adicionando o trecho de JavaScript para `_Layout.cshtml`. 

Se seu projeto não inclua `_Layout.cshtml`, você pode adicionar ainda [monitoramento do lado do cliente](https://docs.microsoft.com/azure/azure-monitor/app/website-monitoring). Você pode fazer isso adicionando o trecho de JavaScript para um arquivo equivalente que controla o `<head>` de todas as páginas dentro de seu aplicativo. Ou você pode adicionar o trecho de código para várias páginas, mas essa solução é difícil de manter e geralmente não é recomendável.

## <a name="configure-the-application-insights-sdk"></a>Configurar o SDK do Application Insights

Você pode personalizar o SDK do Application Insights para ASP.NET Core alterar a configuração padrão. Os usuários do SDK do Application Insights ASP.NET podem estar familiarizados com a alteração de configuração por meio `ApplicationInsights.config` ou modificando `TelemetryConfiguration.Active`. Você alterar a configuração de forma diferente para o ASP.NET Core. Adicionar o SDK do ASP.NET Core para o aplicativo e configurá-lo usando o ASP.NET Core internos [injeção de dependência](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Fazer quase todas as alterações de configuração na `ConfigureServices()` método de sua `Startup.cs` de classe, a menos que você está direcionado de outra forma. As seções a seguir oferecem mais informações.

> [!NOTE]
> Em aplicativos ASP.NET Core, alterando a configuração modificando `TelemetryConfiguration.Active` não tem suporte.

### <a name="using-applicationinsightsserviceoptions"></a>Usando ApplicationInsightsServiceOptions

Você pode modificar algumas configurações comuns, passando `ApplicationInsightsServiceOptions` para `AddApplicationInsightsTelemetry`, como neste exemplo:

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions aiOptions
                    = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
        // Disables adaptive sampling.
        aiOptions.EnableAdaptiveSampling = false;

        // Disables QuickPulse (Live Metrics stream).
        aiOptions.EnableQuickPulseMetricStream = false;
        services.AddApplicationInsightsTelemetry(aiOptions);
    }
```

Para obter mais informações, consulte o [definições configuráveis na `ApplicationInsightsServiceOptions` ](https://github.com/microsoft/ApplicationInsights-aspnetcore/blob/develop/src/Microsoft.ApplicationInsights.AspNetCore/Extensions/ApplicationInsightsServiceOptions.cs).

### <a name="sampling"></a>amostragem

SDK do Application Insights para ASP.NET Core dá suporte a taxa fixa e a amostragem adaptável. Amostragem adaptável está habilitada por padrão. 

Para obter mais informações, consulte [configurar a amostragem adaptável para aplicativos ASP.NET Core](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications).

### <a name="adding-telemetryinitializers"></a>Adicionando TelemetryInitializers

Use [inicializadores de telemetria](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#add-properties-itelemetryinitializer) quando você deseja definir propriedades globais que são enviadas com toda a telemetria.

Adicionar qualquer nova `TelemetryInitializer` para o `DependencyInjection` contêiner conforme mostrado no código a seguir. O SDK escolherá automaticamente qualquer `TelemetryInitializer` que é adicionado para o `DependencyInjection` contêiner.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
    }
```

### <a name="removing-telemetryinitializers"></a>Removendo TelemetryInitializers

Inicializadores de telemetria estão presentes por padrão. Para remover todos os ou inicializadores de telemetria específico, use o seguinte código de exemplo *após* chamar `AddApplicationInsightsTelemetry()`.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // Remove a specific built-in telemetry initializer
        var tiToRemove = services.FirstOrDefault<ServiceDescriptor>
                         (t => t.ImplementationType == typeof(AspNetCoreEnvironmentTelemetryInitializer));
        if (tiToRemove != null)
        {
            services.Remove(tiToRemove);
        }

        // Remove all initializers
        // This requires importing namespace by using Microsoft.Extensions.DependencyInjection.Extensions;
        services.RemoveAll(typeof(ITelemetryInitializer));
    }
```

### <a name="adding-telemetry-processors"></a>Adição de processadores de telemetria

Você pode adicionar processadores de telemetria personalizada para `TelemetryConfiguration` usando o método de extensão `AddApplicationInsightsTelemetryProcessor` em `IServiceCollection`. Você usa os processadores de telemetria no [cenários de filtragem avançada](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#filtering-itelemetryprocessor) para permitir o controle mais direto sobre o que tem incluídos ou excluídos da telemetria que você envia para o serviço do Application Insights. Use o exemplo a seguir.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // ...
        services.AddApplicationInsightsTelemetry();
        services.AddApplicationInsightsTelemetryProcessor<MyFirstCustomTelemetryProcessor>();

        // If you have more processors:
        services.AddApplicationInsightsTelemetryProcessor<MySecondCustomTelemetryProcessor>();
    }
```

### <a name="configuring-or-removing-default-telemetrymodules"></a>Configuração ou remoção de padrão TelemetryModules

O Application Insights usa módulos de telemetria [coletar automaticamente informações úteis](https://docs.microsoft.com/azure/azure-monitor/app/auto-collect-dependencies) sobre cargas de trabalho específicas sem a necessidade de configuração adicional.

Os seguintes módulos de coleção automáticas são habilitados por padrão. Esses módulos são responsáveis por coletar automaticamente a telemetria. Você pode desabilitar ou configurá-los para alterar seu comportamento padrão.

* `RequestTrackingTelemetryModule`
* `DependencyTrackingTelemetryModule`
* `PerformanceCollectorModule`
* `QuickPulseTelemetryModule`
* `AppServicesHeartbeatTelemetryModule`
* `AzureInstanceMetadataTelemetryModule`

Para configurar qualquer padrão `TelemetryModule`, use o método de extensão `ConfigureTelemetryModule<T>` em `IServiceCollection`, conforme mostrado no exemplo a seguir.

```csharp
using Microsoft.ApplicationInsights.DependencyCollector;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following configures DependencyTrackingTelemetryModule.
        // Similarly, any other default modules can be configured.
        services.ConfigureTelemetryModule<DependencyTrackingTelemetryModule>((module, o) =>
                        {
                            module.EnableW3CHeadersInjection = true;
                        });

        // The following removes PerformanceCollectorModule to disable perf-counter collection.
        // Similarly, any other default modules can be removed.
        var performanceCounterService = services.FirstOrDefault<ServiceDescriptor>(t => t.ImplementationType == typeof(PerformanceCollectorModule));
        if (performanceCounterService != null)
        {
         services.Remove(performanceCounterService);
        }
    }
```

### <a name="configuring-a-telemetry-channel"></a>Configurar um canal de telemetria

O canal padrão é `ServerTelemetryChannel`. Você pode substituí-la como mostra o exemplo a seguir.

```csharp
using Microsoft.ApplicationInsights.Channel;

    public void ConfigureServices(IServiceCollection services)
    {
        // Use the following to replace the default channel with InMemoryChannel.
        // This can also be applied to ServerTelemetryChannel.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

### <a name="disable-telemetry-dynamically"></a>Desabilitar a telemetria dinamicamente

Se você quiser desabilitar condicionalmente e dinamicamente a telemetria, você pode resolver `TelemetryConfiguration` de instância com o contêiner de injeção de dependência do ASP.NET Core em qualquer lugar no seu código e defina `DisableTelemetry` sinalizador nele.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env, TelemetryConfiguration configuration)
    {
        configuration.DisableTelemetry = true;
        ...
    }
```

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="how-can-i-track-telemetry-thats-not-automatically-collected"></a>Como posso acompanhar telemetria coletada automaticamente?

Obtenha uma instância de `TelemetryClient` usando a injeção de construtor e chamar o necessária `TrackXXX()` método nele. Não recomendamos criar novos `TelemetryClient` instâncias em um aplicativo ASP.NET Core. Uma instância singleton da `TelemetryClient` já está registrado na `DependencyInjection` contêiner, que compartilha `TelemetryConfiguration` com o restante da telemetria. Criando um novo `TelemetryClient` instância é recomendada somente se precisar de uma configuração que é separada do restante da telemetria. 

O exemplo a seguir mostra como acompanhar a telemetria adicional de um controlador.

```csharp
using Microsoft.ApplicationInsights;

public class HomeController : Controller
{
    private TelemetryClient telemetry;

    // Use constructor injection to get a TelemetryClient instance.
    public HomeController(TelemetryClient telemetry)
    {
        this.telemetry = telemetry;
    }

    public IActionResult Index()
    {
        // Call the required TrackXXX method.
        this.telemetry.TrackEvent("HomePageRequested");
        return View();
    }
```

Para obter mais informações sobre relatórios do Application Insights de dados personalizados, consulte [métricas personalizadas do Application Insights referência da API](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics/).

### <a name="some-visual-studio-templates-used-the-useapplicationinsights-extension-method-on-iwebhostbuilder-to-enable-application-insights-is-this-usage-still-valid"></a>Alguns modelos do Visual Studio usado o método de extensão UseApplicationInsights() no IWebHostBuilder para habilitar o Application Insights. Esse uso ainda é válida?

Sim, habilitar o Application Insights com esse método é válido. Essa técnica é usada na integração do Visual Studio e nas extensões de aplicativos Web. No entanto, é recomendável usar `services.AddApplicationInsightsTelemetry()` porque ele fornece sobrecargas para controlar algumas configurações. Ambos os métodos de fazem a mesma coisa internamente, portanto, se você não precisa aplicar a configuração personalizada, você pode chamar qualquer um dos métodos.

### <a name="im-deploying-my-aspnet-core-application-to-web-apps-should-i-still-enable-the-application-insights-extension-from-web-apps"></a>Eu estou implantando meu aplicativo ASP.NET Core nos aplicativos Web. Devo habilitar a extensão do Application Insights de aplicativos Web ainda?

Se o SDK está instalado no momento da compilação, conforme mostrado neste artigo, você não precisa habilitar a extensão do Application Insights do portal do serviço de aplicativo. Mesmo se a extensão é instalada, fará logoff quando detecta que o SDK já foi adicionado ao aplicativo. Se você habilitar o Application Insights a extensão, você não precisa instalar e atualizar o SDK. Mas se você habilitar o Application Insights seguindo as instruções neste artigo, você tem mais flexibilidade porque:

   * Telemetria do Application Insights continuarão funcionando:
       * Todos os sistemas operacionais, incluindo Windows, Linux e Mac.
       * Todos os modos, de publicação incluindo independente ou dependente do framework.
       * Todas as estruturas de destino, incluindo o .NET Framework completo.
       * Todas as opções de hospedagem, incluindo aplicativos Web, máquinas virtuais, Linux, contêineres, serviço Kubernetes do Azure e não Azure hospeda.
   * Você pode ver a telemetria localmente quando você estiver depurando no Visual Studio.
   * Você pode acompanhar a telemetria personalizada adicional usando o `TrackXXX()` API.
   * Você tem controle total sobre a configuração.

### <a name="can-i-enable-application-insights-monitoring-by-using-tools-like-status-monitor"></a>Posso habilitar o monitoramento do Application Insights usando ferramentas como o Monitor de Status?

Nº [Monitor de status](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) e [v2 do Monitor de Status](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-overview) atualmente dão suporte ao ASP.NET 4. x somente.

### <a name="is-application-insights-automatically-enabled-for-my-aspnet-core-20-application"></a>Application Insights é habilitado automaticamente para o meu aplicativo ASP.NET Core 2.0?

O `Microsoft.AspNetCore.All` metapacote 2.0 incluídos SDK do Application Insights (versão 2.1.0). Se você executar o aplicativo no depurador do Visual Studio, o Visual Studio permite que o Application Insights e mostra a telemetria localmente no próprio IDE. A telemetria não foi enviada para o serviço do Application Insights, a menos que uma chave de instrumentação foi especificada. É recomendável seguir as instruções neste artigo para habilitar o Application Insights, mesmo para 2.0 aplicativos.

### <a name="if-i-run-my-application-in-linux-are-all-features-supported"></a>Se eu executar meu aplicativo no Linux, todos os recursos têm suporte?

Sim. Suporte de recurso para o SDK é a mesma em todas as plataformas, com as seguintes exceções:

* Contadores de desempenho têm suporte apenas no Windows.
* Embora `ServerTelemetryChannel` está habilitado por padrão, se o aplicativo estiver em execução no Linux ou MacOS, o canal não cria automaticamente uma pasta de armazenamento local para manter a telemetria temporariamente se houver problemas de rede. Devido a essa limitação, a telemetria é perdida quando há problemas temporários de rede ou servidor. Para contornar esse problema, configure uma pasta local para o canal:

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

    public void ConfigureServices(IServiceCollection services)
    {
        // The following will configure the channel to use the given folder to temporarily
        // store telemetry items during network or Application Insights server issues.
        // User should ensure that the given folder already exists
        // and that the application has read/write permissions.
        services.AddSingleton(typeof(ITelemetryChannel),
                                new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="open-source-sdk"></a>SDK do código-fonte aberto

[Ler e contribuir para o código](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates).

## <a name="video"></a>Vídeo

- Confira este vídeo passo a passo externo para [configurar o Application Insights com o .NET Core e o Visual Studio](https://www.youtube.com/watch?v=NoS9UhcR4gA&t) do zero.
- Confira este vídeo passo a passo externo para [configurar o Application Insights com o .NET Core e o Visual Studio Code](https://youtu.be/ygGt84GDync) do zero.

## <a name="next-steps"></a>Próximas etapas

* [Explore os fluxos de usuário](../../azure-monitor/app/usage-flows.md) para entender como os usuários navegam por meio de seu aplicativo.
* [Configurar uma coleção de instantâneo](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) para ver o estado do código-fonte e variáveis no momento em que uma exceção será lançada.
* [Usar a API](../../azure-monitor/app/api-custom-events-metrics.md) para enviar seus próprios eventos e métricas para uma exibição detalhada do desempenho e uso de seu aplicativo.
* Use os [Testes de disponibilidade](../../azure-monitor/app/monitor-web-app-availability.md) para verificar seu aplicativo constante em todo o mundo.
* [Injeção de dependência no ASP.NET Core](https://docs.microsoft.com/aspnet/fundamentals/dependency-injection)
