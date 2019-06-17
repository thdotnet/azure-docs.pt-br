---
title: Azure Application Insights para ASP.NET Core | Microsoft Docs
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
ms.openlocfilehash: cb7ace20fd0a59dafff3d7f8240f54c3c8e12492
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66226406"
---
# <a name="application-insights-for-aspnet-core-applications"></a>Application Insights para aplicativos ASP.NET Core

Este artigo descreve como habilitar o Application Insights para um [ASP.NET Core](https://docs.microsoft.com/aspnet/core) aplicativo. Quando você concluir as instruções neste artigo, o Application Insights iniciará a coleta de solicitações, dependências, exceções, contadores de desempenho, as pulsações e logs de seu aplicativo ASP.NET Core. O aplicativo de exemplo é um [aplicativo MVC](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app) direcionamento `netcoreapp2.2`, mas essas instruções são aplicáveis a todos os aplicativos ASP.NET Core.

## <a name="supported-scenarios"></a>Cenários com suporte

O [Application Insights SDK (Software Development Kit) para o ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) pode monitorar seus aplicativos, independentemente de onde ou como o aplicativo é executado. Se seu aplicativo está em execução e tem conectividade de rede para o Azure, a telemetria pode ser coletada. Isso significa que monitoramento do Application Insights tem suporte em qualquer lugar, há suporte para .NET Core. Esse suporte inclui qualquer sistema operacional (Windows, Linux, Mac), o método de hospedagem (em processo versus fora do processo), o método de implantação dependente de estrutura vs (independentes), servidor Web (IIS, o Kestrel), plataforma de hospedagem (aplicativos Web do Azure, VM do Azure, Docker, Serviço de Kubernetes do Azure (AKS) e assim por diante.) ou IDE (linha de comando do Visual Studio, VS Code).

## <a name="prerequisites"></a>Pré-requisitos

- Um aplicativo do ASP.NET Core está funcionando. Siga as [guia de Introdução ao ASP.NET Core obtendo](https://docs.microsoft.com/aspnet/core/getting-started/) para criar um aplicativo ASP.NET Core, se necessário.
- Uma válido Application Insights chave de instrumentação, que é necessário para enviar qualquer telemetria ao serviço Application Insights. Siga as [criar um recurso instruções](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) para criar um novo recurso do Application Insights, se necessário e obter uma chave de instrumentação.

## <a name="enable-application-insights-server-side-telemetry-visual-studio"></a>Habilitar a telemetria do lado do servidor do Application Insights (Visual Studio)

1. Abra o projeto no Visual Studio.

    > [!TIP]
    > Enquanto não é uma etapa obrigatória, pode ser útil configurar o controle de origem para o seu projeto para que você possa acompanhar todas as alterações feitas pelo Application Insights. Para habilitar a seleção do controle de fonte de **arquivo** > **adicionar ao controle do código-fonte**.

2. Selecione **Projeto** > **Adicionar Application Insights Telemetry**.

3. Selecione **Introdução**. (Dependendo de sua versão do Visual Studio, o texto pode variar um pouco. Algumas versões mais antigas têm um botão **Iniciar gratuitamente**.)

4. Selecione sua assinatura e, em seguida, selecione **Recurso** > **Registrar**.

5. Depois de adicionar o Application Insights ao seu projeto, verifique para confirmar que você está usando a versão estável mais recente do SDK. Vá para **Project** > **Manage NuGet Packages** > **aspnetcore** > Se for necessário escolher **Atualização**.

     ![Captura de tela de gerenciar a tela de pacote do NuGet com selecionados para atualização de pacote do Application Insights](./media/asp-net-core/update-nuget-package.png)

6. Se você seguido a dica opcional e adicionados a seu projeto ao controle de origem, em seguida, você pode ir para **modo de exibição** > **Team Explorer** > **alterações** e Selecione cada arquivo individual para um modo de exibição de comparação das alterações feitas da adição de telemetria do Application Insights.

## <a name="enable-application-insights-server-side-telemetry-without-visual-studio"></a>Habilitar a telemetria do Application Insights do lado do servidor (sem o Visual Studio)

1. Instalar o [pacote do NuGet do SDK do Application Insights para ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore). É recomendável usar sempre a versão estável mais recente. Notas de versão completo para o SDK podem ser encontradas na [abrir repositório GitHub de código-fonte](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases).

    O trecho a seguir mostra as alterações a serem adicionados ao seu projeto `.csproj` arquivo.

    ```xml
        <ItemGroup>
          <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.6.1" />
        </ItemGroup>
    ```

2. Adicione `services.AddApplicationInsightsTelemetry();` para o `ConfigureServices()` método na sua `Startup` classe. Exemplo completo abaixo.

    ```csharp
        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            // The following line enables Application Insights telemetry collection.
            services.AddApplicationInsightsTelemetry();
    
            // code adding other services for your application
            services.AddMvc();
        }
    ```

3. Configure a chave de instrumentação.

    Embora seja possível fornecer a chave de instrumentação como um argumento para `AddApplicationInsightsTelemetry`, recomendamos que você especifique a chave de instrumentação na configuração. A seguir mostra como especificar uma chave de instrumentação em `appsettings.json`. Certifique-se de `appsettings.json` é copiado para a pasta raiz de aplicativo durante a publicação.

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

    Como alternativa, a chave de instrumentação também pode ser especificada em qualquer uma das seguintes variáveis de ambiente.

    APPINSIGHTS_INSTRUMENTATIONKEY

    ApplicationInsights:InstrumentationKey

    Exemplo:

    `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

    `SET APPINSIGHTS_INSTRUMENTATIONKEY=putinstrumentationkeyhere`

    `APPINSIGHTS_INSTRUMENTATIONKEY` normalmente é usado para especificar a chave de instrumentação para aplicativos implantados para aplicativos Web do Azure.

    > [!NOTE]
    > Uma chave de instrumentação especificada no código wins pela variável de ambiente `APPINSIGHTS_INSTRUMENTATIONKEY`, qual vence sobre outras opções.

## <a name="run-your-application"></a>Execute seu aplicativo.

 Executar o aplicativo e fazer solicitações a ele. Telemetria agora deve começar a fluir para o Application Insights. A telemetria a seguir é coletada automaticamente pelo SDK do Application Insights.

|Solicitações/dependências |Detalhes|
|---------------|-------|
|Requests | Solicitações de entrada da web ao seu aplicativo. |
|Http/Https | Chamadas feitas com `HttpClient`. |
|SQL | Chamadas feitas com `SqlClient`. |
|[Armazenamento do Azure](https://www.nuget.org/packages/WindowsAzure.Storage/) | Chamadas feitas com o cliente de armazenamento do Azure. |
|[SDK do cliente do hub de eventos](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | Versão 1.1.0 e superior. |
|[SDK do Cliente do Barramento de Serviço](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus)| Versão 3.0.0 e superior. |
|Azure Cosmos DB | Acompanhada automaticamente somente se o HTTP/HTTPS é usado. O modo TCP não será capturado pelo Application Insights. |

### <a name="performance-counters"></a>Contadores de desempenho

Suporte para [contadores de desempenho](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/) no ASP.NET Core é limitado ao seguinte

   * O SDK versão 2.4.1 e acima coleta contadores de desempenho se o aplicativo estiver em execução no Azure Web App (Windows)
   * SDK versão 2.7.0-beta3 e acima coleta contadores de desempenho se o aplicativo está em execução no Windows e direcionamento `NETSTANDARD2.0` ou superior.
   * Para aplicativos destinados ao .NET Framework, os contadores de desempenho têm suporte em todas as versões do SDK.
   * Este artigo será atualizado quando o suporte de contador de desempenho no Linux é adicionado.

### <a name="ilogger-logs"></a>Logs de ILogger

[Logs de ILogger](https://docs.microsoft.com/azure/azure-monitor/app/ilogger) de gravidade `Warning` ou acima são automaticamente capturados do SDK versão 2.7.0-beta3 ou superior.

### <a name="live-metrics"></a>Métricas em tempo real

Ele pode levar alguns minutos para que a telemetria começar a aparecer no portal. Para verificar rapidamente se tudo está funcionando, é melhor usar [Live Metrics](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream), enquanto a tomada de solicitações para o aplicativo em execução.

## <a name="enable-client-side-telemetry-for-web-applications"></a>Habilitar a telemetria do lado do cliente para aplicativos Web

As etapas acima são suficientes para iniciar a coleta de telemetria do lado do servidor. Se seu aplicativo tiver componentes do lado do cliente, siga as etapas abaixo para iniciar a coleta [telemetria de uso](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview) a partir daí.

1. No `_ViewImports.cshtml`, injeção de adicionar:

    ```cshtml
        @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
    ```

2. Na `_Layout.cshtml`, insira HtmlHelper ao final da `<head>` seção, mas antes de qualquer outro script. Qualquer telemetria JavaScript personalizada que você deseja relatar a partir da página deve ser inserida após este trecho de código:

    ```cshtml
        @Html.Raw(JavaScriptSnippet.FullScript)
        </head>
    ```

O `.cshtml` nomes de arquivos mencionados acima são de um modelo de aplicativo do MVC padrão. Por fim, para habilitar o monitoramento do lado do cliente para seu aplicativo corretamente, você precisa o trecho de JavaScript estejam presentes no `<head>` seção de cada página do seu aplicativo que você deseja monitorar. Para este modelo de aplicativo adicionando o trecho de Javascript `_Layout.cshtml` será efetivamente atingir essa meta. Se seu projeto não tiver esse arquivo específico, ainda, você pode adicionar [monitoramento do lado do cliente](https://docs.microsoft.com/azure/azure-monitor/app/website-monitoring). Você apenas precisa como adicionaria o JavaScript para um arquivo equivalente que controla o `<head>` de todas as páginas dentro de seu aplicativo, ou como alternativa, você pode adicionar o trecho de código, a pessoa várias páginas, embora isso seria difícil de manter e geralmente não é recomendado.

## <a name="configuring-application-insights-sdk"></a>Configurando o SDK do Application Insights

SDK do Application Insights para ASP.NET Core pode ser personalizado para alterar a configuração padrão. Os usuários do SDK do ASP.NET do Application Insights podem estar familiarizados com a configuração usando `ApplicationInsights.config`, ou modificando `TelemetryConfiguration.Active`. Para o ASP.NET Core, configuração é feita de forma diferente. O SDK do ASP.NET Core é adicionado ao aplicativo e configurado usando internos do ASP.NET Core [injeção de dependência](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Quase todas as alterações são feitas na `ConfigureServices()` método de sua `Startup.cs` de classe, a menos que indicado de outra forma. Siga as seções abaixo para saber mais.

> [!NOTE]
>  Alterando a configuração modificando `TelemetryConfiguration.Active` não é recomendado em aplicativos ASP.NET Core.

### <a name="configuring-using-applicationinsightsserviceoptions"></a>Configurar usando ApplicationInsightsServiceOptions

É possível modificar algumas configurações comuns, passando `ApplicationInsightsServiceOptions` para `AddApplicationInsightsTelemetry`. Um exemplo é mostrado abaixo.

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

A lista exata de definições configuráveis na `ApplicationInsightsServiceOptions` pode ser encontrado [aqui](https://github.com/microsoft/ApplicationInsights-aspnetcore/blob/develop/src/Microsoft.ApplicationInsights.AspNetCore/Extensions/ApplicationInsightsServiceOptions.cs).

### <a name="sampling"></a>amostragem

SDK do Application Insights para ASP.NET Core dá suporte a FixedRate e amostragem adaptável. Amostragem adaptável está habilitada por padrão. Siga nossos [orientações sobre a amostragem adaptável](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications), para aprender a configurar a amostragem para aplicativos ASP.NET Core.

### <a name="adding-telemetryinitializers"></a>Adicionando TelemetryInitializers

[Inicializadores de telemetria](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#add-properties-itelemetryinitializer) são usadas quando você deseja definir propriedades globais que são enviadas com toda a telemetria.

Para adicionar uma nova `TelemetryInitializer`, adicioná-lo no contêiner DependencyInjection, conforme mostrado abaixo. `TelemetryInitializer`s adicionado ao contêiner DependencyInjection será captado pelo SDK automaticamente.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
    }
```

### <a name="removing-telemetryinitializers"></a>Removendo TelemetryInitializers

Para remover todos os ou TelemetryInitializers específicos, que estão presentes por padrão, use o seguinte código de exemplo **após** chamar `AddApplicationInsightsTelemetry()`.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // Remove a specific built-in TelemetryInitializer
        var tiToRemove = services.FirstOrDefault<ServiceDescriptor>
                         (t => t.ImplementationType == typeof(AspNetCoreEnvironmentTelemetryInitializer));
        if (tiToRemove != null)
        {
            services.Remove(tiToRemove);
        }

        // Remove all initializers
        // This requires importing namespace using Microsoft.Extensions.DependencyInjection.Extensions;
        services.RemoveAll(typeof(ITelemetryInitializer));
    }
```

### <a name="adding-telemetryprocessors"></a>Adicionando TelemetryProcessors

Processadores de telemetria personalizada podem ser adicionados para o `TelemetryConfiguration` usando o método de extensão `AddApplicationInsightsTelemetryProcessor` em `IServiceCollection`. Processadores de telemetria são usados em [cenários de filtragem avançada](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#filtering-itelemetryprocessor) para permitir o controle mais direto sobre o que é incluído ou excluído da telemetria que você envia para o serviço do Application Insights. Use o exemplo a seguir.

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

O Application Insights usa módulos de telemetria como uma forma de [-coleta automaticamente informações úteis](https://docs.microsoft.com/azure/azure-monitor/app/auto-collect-dependencies) sobre cargas de trabalho específicas sem a necessidade de configuração adicional.

Os seguintes módulos de coleta automática são habilitados por padrão e são responsáveis por coletar automaticamente a telemetria. Pode ser desabilitados e configurados para alterar o comportamento padrão.

* `RequestTrackingTelemetryModule`
* `DependencyTrackingTelemetryModule`
* `PerformanceCollectorModule`
* `QuickPulseTelemetryModule`
* `AppServicesHeartbeatTelemetryModule`
* `AzureInstanceMetadataTelemetryModule`

Para configurar qualquer padrão `TelemetryModule`, use o método de extensão `ConfigureTelemetryModule<T>` em `IServiceCollection` conforme mostrado no exemplo a seguir.

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

### <a name="configuring-telemetry-channel"></a>Configurando o canal de telemetria

O canal padrão usado é o `ServerTelemetryChannel`. Ele pode ser substituído pelo exemplo a seguir.

```csharp
using Microsoft.ApplicationInsights.Channel;

    public void ConfigureServices(IServiceCollection services)
    {
        // use the following to replace the default channel with InMemoryChannel.
        // this can also be applied to ServerTelemetryChannel as well.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="i-want-to-track-additional-telemetry-other-than-the-auto-collected-telemetry-how-do-i-do-it"></a>Eu quero acompanhar telemetria adicional que não seja a telemetria coletado automaticamente. Como fazer isso?

Obtenha uma instância de `TelemetryClient` usando a injeção de construtor e chamar o necessária `TrackXXX()` método nele. Não é recomendável criar um novo `TelemetryClient` instâncias do aplicativo do ASP.NET Core, como uma instância singleton da `TelemetryClient` já está registrado no contêiner de DI compartilha `TelemetryConfiguration` com o restante da telemetria. Criando um novo `TelemetryClient` instância é recomendada somente se ele deve ter uma configuração separada do restante da telemetria. O exemplo a seguir mostra como acompanhar a telemetria adicional de um controlador.

```csharp
using Microsoft.ApplicationInsights;

public class HomeController : Controller
{
    private TelemetryClient telemetry;

    // use constructor injection to get TelemetryClient instance
    public HomeController(TelemetryClient telemetry)
    {
        this.telemetry = telemetry;
    }

    public IActionResult Index()
    {
        // call required TrackXXX method.
        this.telemetry.TrackEvent("HomePageRequested");
        return View();
    }
```

 Consulte a [métricas personalizadas do Application Insights referência da API](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics/) para obter uma descrição de relatórios do Application Insights de dados personalizados.

### <a name="some-visual-studio-templates-used-useapplicationinsights-extension-method-on-iwebhostbuilder-to-enable-application-insights-is-this-usage-still-valid"></a>Alguns modelos do Visual Studio usado o método de extensão UseApplicationInsights() no IWebHostBuilder para habilitar o Application Insights. Esse uso ainda é válida?

Habilitar o Application Insights com esse método é válido e é usado na integração do Visual Studio e nas extensões do aplicativo Web do Azure. No entanto, é recomendável usar `services.AddApplicationInsightsTelemetry()` pois ele fornece sobrecargas para controlar algumas configurações. Ambos os métodos internamente fazem a mesma coisa, portanto, se não há nenhuma configuração personalizada a ser aplicado, chamar qualquer uma serve.

### <a name="i-am-deploying-my-aspnet-core-application-to-azure-web-apps-should-i-still-enable-the-application-insights-extension-from-web-apps"></a>Eu estou implantando meu aplicativo ASP.NET Core, aplicativos Web do Azure. Devo habilitar a extensão do Application Insights de aplicativos Web ainda?

Se o SDK está instalado no momento da compilação, conforme mostrado neste artigo, não é necessário para habilitar a extensão do Application Insights do portal do serviço de aplicativo. Mesmo se a extensão é instalada, fará logoff quando detecta que o SDK já foi adicionado ao aplicativo. Habilitar o Application Insights da extensão libera você da instalação e atualização do SDK. No entanto, habilitar o Application Insights, de acordo com este artigo é mais flexível para motivos abaixo.
   * Telemetria do Application Insights continuarão funcionando:
       * Todos os sistemas operacionais - Mac Windows, Linux.
       * Todos os modos de - de publicação independente ou dependente do Framework.
       * Todas as estruturas de destino, incluindo o .NET Framework completo.
       * Todas as opções de hospedagem - aplicativo Web do Azure, VMs, Linux, contêineres, AKS, não do Azure.
   * Telemetria pode ser vista localmente, durante a depuração do Visual Studio.
   * Permite o uso do acompanhamento de telemetria personalizada adicional `TrackXXX()` API.
   * Você tem controle total sobre a configuração.

### <a name="can-i-enable-application-insights-monitoring-using-tools-like-status-monitor"></a>Posso habilitar o monitoramento do Application Insights usando ferramentas como o Monitor de Status?

Não. [Monitor de status](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) e sua substituição futura [v2 do Monitor de Status](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-overview) atualmente dá suporte a ASP.NET 4. x somente.

### <a name="i-have-an-aspnet-core-20-application-isnt-application-insights-automatically-enabled-without-me-doing-anything"></a>Eu tenho um aplicativo do ASP.NET Core 2.0. Application Insights não está automaticamente habilitada sem eu fazer nada?

`Microsoft.AspNetCore.All` metapacote 2.0 incluído o SDK do Application Insights (versão 2.1.0) e se você executar o aplicativo no depurador do Visual Studio, Visual Studio permite que o Application Insights e mostra a telemetria localmente no próprio IDE. Telemetria não foi enviada para o serviço do Application Insights, a menos que uma chave de instrumentação é especificada explicitamente. É recomendável seguir as instruções neste artigo para habilitar o Application Insights, mesmo para 2.0 aplicativos.

### <a name="i-run-my-application-in-linux-are-all-features-supported-in-linux-as-well"></a>Executo meu aplicativo no Linux. Todos os recursos também têm suporte no Linux?

* Sim. Suporte de recurso para o SDK é o mesmo em todas as plataformas, com as seguintes exceções:

    * Contadores de desempenho ainda não têm suporte no Windows não.
    * Embora `ServerTelemetryChannel` está habilitado por padrão, se o aplicativo estiver em execução no Linux ou MacOS, o canal não cria automaticamente uma pasta de armazenamento local para manter a telemetria temporariamente se houver problemas de rede. Essa limitação faz com que a telemetria serão perdidos se houver problemas de servidor ou de rede temporária. A solução alternativa para esse problema é para o usuário configure uma pasta local para o canal, conforme mostrado abaixo.

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

    public void ConfigureServices(IServiceCollection services)
    {
        // The following will configure channel to use the given folder to temporarily
        // store telemetry items during network or application insights server issues.
        // User should ensure that the given folder already exists,
        // and that application has read/write permissions.
        services.AddSingleton(typeof(ITelemetryChannel),
                                new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="open-source-sdk"></a>SDK do código-fonte aberto
[Ler e contribuir para o código](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates).

## <a name="video"></a>Vídeo

- Vídeo externo de passo a passo sobre [configuração do Application Insights com um .NET Core e Visual Studio](https://www.youtube.com/watch?v=NoS9UhcR4gA&t) a partir do zero.
- Vídeo externo de passo a passo sobre [configuração do Application Insights com um .NET Core e Visual Studio Code](https://youtu.be/ygGt84GDync) a partir do zero.

## <a name="next-steps"></a>Próximas etapas
* [Explorar os Fluxos dos Usuários](../../azure-monitor/app/usage-flows.md) para entender como os usuários navegam por meio de seu aplicativo.
* [Configure a Coleção de Instantâneos](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) para ver o estado do código-fonte e as variáveis no momento em que uma exceção é lançada.
* [Use a API](../../azure-monitor/app/api-custom-events-metrics.md) para enviar seus próprios eventos e métricas para uma exibição mais detalhada do desempenho e do uso do aplicativo.
* Use os [Testes de disponibilidade](../../azure-monitor/app/monitor-web-app-availability.md) para verificar seu aplicativo constante em todo o mundo.
