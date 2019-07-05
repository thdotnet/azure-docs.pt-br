---
title: Acompanhamento de dependência no Azure Application Insights | Microsoft Docs
description: Monitorar chamadas de dependência de seu local ou de um aplicativo de web do Microsoft Azure com o Application Insights.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: mbullwin
ms.openlocfilehash: d8ba5b19ad5d8f03203e9a028fbc5aec84e5ec06
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/04/2019
ms.locfileid: "67565381"
---
# <a name="dependency-tracking-in-azure-application-insights"></a>Acompanhamento de dependência no Azure Application Insights 

Um *dependência* é um componente externo que é chamado por seu aplicativo. Normalmente, ele é um serviço chamado usando HTTP, um banco de dados ou um sistema de arquivos. [Application Insights](../../azure-monitor/app/app-insights-overview.md) calcula a duração das chamadas de dependência, se sua falha ou não, juntamente com informações adicionais, como o nome da dependência e assim por diante. Você pode investigar chamadas de dependência específica e correlacioná-las a solicitações e exceções.

## <a name="automatically-tracked-dependencies"></a>Dependências controladas automaticamente

SDKs do Application Insights para .NET e .NET Core acompanha o `DependencyTrackingTelemetryModule` que é um módulo de telemetria que coleta automaticamente as dependências. Esta coleção de dependência é habilitada automaticamente para [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net) e [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) aplicativos, quando configurado de acordo com os documentos oficiais vinculados. `DependencyTrackingTelemetryModule` é enviado como [isso](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) pacote do NuGet e é colocado automaticamente ao usar qualquer um dos pacotes NuGet `Microsoft.ApplicationInsights.Web` ou `Microsoft.ApplicationInsights.AspNetCore`.

 `DependencyTrackingTelemetryModule` Atualmente, rastreia automaticamente as seguintes dependências:

|Dependências |Detalhes|
|---------------|-------|
|Http/Https | Local ou remoto chamadas http/https |
|Chamadas do WCF| Acompanhada automaticamente somente se associações baseadas em Http são usadas.|
|SQL | Chamadas feitas com `SqlClient`. Ver [isso](##advanced-sql-tracking-to-get-full-sql-query) para capturar SQL de consulta.  |
|[Armazenamento do Azure (Blob, tabela, fila)](https://www.nuget.org/packages/WindowsAzure.Storage/) | Chamadas feitas com o cliente de armazenamento do Azure. |
|[SDK do cliente do hub de eventos](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | Versão 1.1.0 e superior. |
|[SDK do Cliente do Barramento de Serviço](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus)| Versão 3.0.0 e superior. |
|Azure Cosmos DB | Acompanhada automaticamente somente se o HTTP/HTTPS é usado. O modo TCP não será capturado pelo Application Insights. |


## <a name="setup-automatic-dependency-tracking-in-console-apps"></a>Configurar o rastreamento em aplicativos de Console automático de dependência

Para controlar automaticamente as dependências de aplicativos de console Core.NET/.NET, instale o pacote Nuget `Microsoft.ApplicationInsights.DependencyCollector`e inicializar `DependencyTrackingTelemetryModule` da seguinte maneira:

```csharp
    DependencyTrackingTelemetryModule depModule = new DependencyTrackingTelemetryModule();
    depModule.Initialize(TelemetryConfiguration.Active);
```

### <a name="how-automatic-dependency-monitoring-works"></a>Dependência automática monitoramento funciona?

As dependências são coletadas automaticamente, usando uma das seguintes técnicas:

* Usando a instrumentação de código de bytes em torno de métodos de seleção. (InstrumentationEngine do StatusMonitor ou extensão do aplicativo Web do Azure)
* Retornos de chamada de EventSource
* Retornos de chamada de DiagnosticSource (nos SDKs do Core.NET/.NET mais recente)

## <a name="manually-tracking-dependencies"></a>Acompanhamento de dependências manualmente

A seguir estão alguns exemplos de dependências, que não são coletadas automaticamente e, portanto, exigem o controle manual.

* O Azure Cosmos DB é acompanhado automaticamente somente se [HTTP/HTTPS](../../cosmos-db/performance-tips.md#networking) é usado. O modo TCP não será capturado pelo Application Insights.
* Redis

Para que essas dependências coletadas automaticamente pelo SDK, você pode acompanhá-los manualmente usando o [API TrackDependency](api-custom-events-metrics.md#trackdependency) que é usado pelos módulos de coleção padrão automaticamente.

Por exemplo, se você criar seu código com um assembly que não escreveu, poderá determinar o tempo de todas as chamadas nele, para descobrir qual sua contribuição aos tempos de resposta. Para que esses dados sejam exibidos nos gráficos de dependência no Application Insights, envie-os usando `TrackDependency`.

```csharp

    var startTime = DateTime.UtcNow;
    var timer = System.Diagnostics.Stopwatch.StartNew();
    try
    {
        // making dependency call
        success = dependency.Call();
    }
    finally
    {
        timer.Stop();
        telemetryClient.TrackDependency("myDependencyType", "myDependencyCall", "myDependencyData",  startTime, timer.Elapsed, success);
    }
```

Como alternativa, `TelemetryClient` fornece métodos de extensão `StartOperation` e `StopOperation` que pode ser usado para acompanhar manualmente as dependências, como mostrado [aqui](custom-operations-tracking.md#outgoing-dependencies-tracking)

Se você quiser desativar o módulo de acompanhamento de dependência padrão, remova a referência para DependencyTrackingTelemetryModule em [applicationinsights. config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) para aplicativos ASP.NET. Para aplicativos ASP.NET Core, siga as instruções [aqui](asp-net-core.md#configuring-or-removing-default-telemetrymodules).

## <a name="tracking-ajax-calls-from-web-pages"></a>Acompanhando chamadas AJAX de páginas da Web

Páginas da web, o SDK do JavaScript do Application Insights coleta automaticamente as chamadas AJAX como dependências conforme descrito [aqui](javascript.md#ajax-performance). Este documento se concentra em dependências de componentes de servidor.

## <a name="advanced-sql-tracking-to-get-full-sql-query"></a>Avançadas de controle para obter a consulta SQL completa SQL

Para chamadas SQL, o nome do servidor e do banco de dados sempre é coletado e armazenado como nome da coletados `DependencyTelemetry`. Há um campo adicional chamado 'dados', que podem conter o texto da consulta SQL completo.

Para aplicativos ASP.NET Core, não há nenhuma etapa adicional necessária para obter a consulta SQL completa.

Para aplicativos ASP.NET, a consulta SQL completa é coletada com a Ajuda da instrumentação de código de bytes, que exige que o mecanismo de instrumentação. Específico da plataforma, conforme descrito abaixo, são necessárias etapas adicionais.

| Plataforma | Etapas necessárias para obter a consulta SQL completa |
| --- | --- |
| Aplicativo Web do Azure |No painel de controle de aplicativo web, [abra a folha Application Insights](../../azure-monitor/app/azure-web-apps.md) e habilitar os comandos SQL no .NET |
| Servidor IIS (VM do Azure, locais e assim por diante). | [Instalar o Status Monitor no servidor onde o aplicativo está sendo executado](../../azure-monitor/app/monitor-performance-live-website-now.md) e reinicie o IIS.
| Serviço de Nuvem do Azure | Adicionar [tarefa de inicialização para instalar StatusMonitor](../../azure-monitor/app/cloudservices.md#set-up-status-monitor-to-collect-full-sql-queries-optional) <br> Seu aplicativo deve ser integrado ao SDK do ApplicationInsights no momento da compilação instalando pacotes NuGet para [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net) ou [aplicativos ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) |
| IIS Express | Sem suporte

Nos casos acima, a maneira correta de validar que o mecanismo de instrumentação é instalado corretamente é por meio da validação de que a versão do SDK do coletados `DependencyTelemetry` é 'rddp'. 'rdddsd' ou 'rddf' indica que as dependências são coletadas por meio de retornos de chamada DiagnosticSource ou EventSource e, portanto, a consulta SQL completa não ser capturada.

## <a name="where-to-find-dependency-data"></a>Onde encontrar dados de dependência

* O [Mapa do Aplicativo](app-map.md) visualiza as dependências entre seu aplicativo e os componentes de vizinhança.
* [Diagnóstico de transação](transaction-diagnostics.md) mostra unificada, correlacionada dados do servidor.
* [Guia navegadores](javascript.md#ajax-performance) mostra chamadas AJAX de navegadores dos usuários.
* Clique pelas solicitações com falha ou lentas para verificar a dependência de chamadas.
* O [Analytics](#logs-analytics) pode ser usado para consultar dados de dependência.

## <a name="diagnosis"></a> Diagnosticar solicitações lentas

Cada evento de solicitação está associado com as chamadas de dependência, exceções e outros eventos que são rastreados enquanto seu aplicativo está processando a solicitação. Portanto, se algumas solicitações estão fazendo mal, você pode descobrir seja devido à lentidão nas respostas de uma dependência.

### <a name="tracing-from-requests-to-dependencies"></a>Rastreamento de solicitações de dependências

Abra o **desempenho** guia e navegue até a **dependências** guia na parte superior, ao lado de operações.

Clique em uma **nome da dependência** em geral. Depois de selecionar uma dependência de um gráfico de distribuição dessa dependência de durações aparecerá à direita.

![No desempenho do guia, clique na guia na parte superior, em seguida, um nome de dependência no gráfico de dependência](./media/asp-net-dependencies/2-perf-dependencies.png)

Clique em azul **exemplos** botão no canto inferior direito e, em seguida, em uma amostra para ver os detalhes de transação de ponta a ponta.

![Clique em uma amostra para ver os detalhes de transação de ponta a ponta](./media/asp-net-dependencies/3-end-to-end.png)

### <a name="profile-your-live-site"></a>Perfil de seu site ativo

Não sabe para onde o tempo vai? O [Application Insights profiler](../../azure-monitor/app/profiler.md) rastreamentos HTTP chama para seu site ativo e mostra as funções que mais demoradas em seu código.

## <a name="failed-requests"></a>Solicitações falhas

As solicitações com falha também podem ser associadas a chamadas com falha para as dependências.

Podemos ir para o **falhas** guia à esquerda e, em seguida, clique no **dependências** guia na parte superior.

![Clique no gráfico de solicitações com falha](./media/asp-net-dependencies/4-fail.png)

Aqui, você poderá ver a contagem de dependência com falha. Para obter mais detalhes sobre uma falha na ocorrência tentar clicar em um nome de dependência na tabela a parte inferior. Você pode clicar em azul **dependências** botão no canto inferior direito para obter os detalhes de transação de ponta a ponta.

## <a name="logs-analytics"></a>Logs (análise)

Você pode rastrear dependências na [linguagem de consulta Kusto](/azure/kusto/query/). Veja alguns exemplos.

* Localize todas as chamadas com falha de dependência:

``` Kusto

    dependencies | where success != "True" | take 10
```

* Localize as chamadas AJAX:

``` Kusto

    dependencies | where client_Type == "Browser" | take 10
```

* Localize as chamadas de dependência associadas a solicitações:

``` Kusto

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* Localize as chamadas do AJAX associadas a exibições de página:

``` Kusto 

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="how-does-automatic-dependency-collector-report-failed-calls-to-dependencies"></a>*Como relatório de coletor de dependência automática faz chamadas com falha para as dependências?*

* Chamadas de dependência com falha terá o campo 'êxito' definido como falso. `DependencyTrackingTelemetryModule` não relata `ExceptionTelemetry`. O modelo de dados completa para a dependência é descrito [aqui](data-model-dependency-telemetry.md).

## <a name="open-source-sdk"></a>SDK do código-fonte aberto
Como cada SDK do Application Insights, o módulo de coleta de dependência também é um software livre. Ler e contribuir para o código ou relatar problemas na [repositório GitHub oficial](https://github.com/Microsoft/ApplicationInsights-dotnet-server).

## <a name="next-steps"></a>Próximas etapas

* [Exceções](../../azure-monitor/app/asp-net-exceptions.md)
* [Dados do usuário e da página](../../azure-monitor/app/javascript.md)
* [Disponibilidade](../../azure-monitor/app/monitor-web-app-availability.md)
