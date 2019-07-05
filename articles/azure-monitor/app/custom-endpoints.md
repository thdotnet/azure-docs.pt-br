---
title: O Azure Monitor - Azure Application Insights substituir pontos de extremidade padrão do SDK | Microsoft Docs
description: Modificar pontos de extremidade padrão do Azure Application Insights SDK para regiões, como o Azure governamental.
services: application-insights
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: mbullwin
ms.openlocfilehash: d086815373b84c0f2a70144a505108875fc04981
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443316"
---
 # <a name="application-insights-overriding-default-endpoints"></a>Substituindo os pontos de extremidade padrão do Application Insights

Para enviar dados do Application Insights para determinadas regiões, você precisará substituir os endereços de ponto de extremidade padrão. Cada SDK requer modificações ligeiramente diferentes, que são descritos neste artigo. Estas alterações exigem ajustando o código de exemplo e substituindo os valores de espaço reservado para `QuickPulse_Endpoint_Address`, `TelemetryChannel_Endpoint_Address`, e `Profile_Query_Endpoint_address` com os endereços de ponto de extremidade real para a sua região específica. O final deste artigo contém links para os endereços de ponto de extremidade para regiões em que essa configuração é necessária.

## <a name="sdk-code-changes"></a>Alterações de código do SDK

### <a name="net-with-applicationinsightsconfig"></a>.NET com o applicationinsights. config

> [!NOTE]
> O arquivo applicationinsights. config será substituído automaticamente sempre que uma atualização do SDK é executada. Depois de executar uma atualização do SDK Certifique-se de inserir novamente os valores de ponto de extremidade específico da região.

```xml
<ApplicationInsights>
  ...
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <QuickPulseServiceEndpoint>Custom_QuickPulse_Endpoint_Address</QuickPulseServiceEndpoint>
    </Add>
  </TelemetryModules>
    ...
  <TelemetryChannel>
     <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </TelemetryChannel>
  ...
  <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
    <ProfileQueryEndpoint>Profile_Query_Endpoint_address</ProfileQueryEndpoint>
  </ApplicationIdProvider>
  ...
</ApplicationInsights>
```

### <a name="net-core"></a>.NET Core

Modifique o arquivo appSettings. JSON em seu projeto da seguinte forma para ajustar o principal ponto de extremidade:

```json
"ApplicationInsights": {
    "InstrumentationKey": "instrumentationkey",
    "TelemetryChannel": {
      "EndpointAddress": "TelemetryChannel_Endpoint_Address"
    }
  }
```

Os valores de métricas ao vivo e o ponto de extremidade do perfil de consulta só podem ser definidos por meio de código. Para substituir os valores padrão para todos os valores de ponto de extremidade por meio de código, faça as seguintes alterações na `ConfigureServices` método da `Startup.cs` arquivo:

```csharp
using Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse; //place at top of Startup.cs file

   services.ConfigureTelemetryModule<QuickPulseTelemetryModule>((module, o) => module.QuickPulseServiceEndpoint="QuickPulse_Endpoint_Address");

   services.AddSingleton(new ApplicationInsightsApplicationIdProvider() { ProfileQueryEndpoint = "Profile_Query_Endpoint_address" });

   services.AddSingleton<ITelemetryChannel>(new ServerTelemetryChannel() { EndpointAddress = "TelemetryChannel_Endpoint_Address" });

    //place in ConfigureServices method. If present, place this prior to   services.AddApplicationInsightsTelemetry("instrumentation key");
```

### <a name="java"></a>Java

Modifique o arquivo applicationinsights. XML para alterar o endereço do ponto de extremidade padrão.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <InstrumentationKey>ffffeeee-dddd-cccc-bbbb-aaaa99998888</InstrumentationKey>
  <TelemetryModules>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebSessionTrackingTelemetryModule"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebUserTrackingTelemetryModule"/>
  </TelemetryModules>
  <TelemetryInitializers>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationIdTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebOperationNameTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebSessionTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserTelemetryInitializer"/>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebUserAgentTelemetryInitializer"/>
  </TelemetryInitializers>
  <!--Add the following Channel value to modify the Endpoint address-->
  <Channel type="com.microsoft.applicationinsights.channel.concrete.inprocess.InProcessTelemetryChannel">
  <EndpointAddress>TelemetryChannel_Endpoint_Address</EndpointAddress>
  </Channel>
</ApplicationInsights>
```

### <a name="spring-boot"></a>Spring Boot

Modificar o `application.properties` arquivo e adicione:

```yaml
azure.application-insights.channel.in-process.endpoint-address= TelemetryChannel_Endpoint_Address
```

### <a name="nodejs"></a>Node.js

```javascript
var appInsights = require("applicationinsights");
appInsights.setup('INSTRUMENTATION_KEY');
appInsights.defaultClient.config.endpointUrl = "TelemetryChannel_Endpoint_Address"; // ingestion
appInsights.defaultClient.config.profileQueryEndpoint = "Profile_Query_Endpoint_address"; // appid/profile lookup
appInsights.defaultClient.config.quickPulseHost = "QuickPulse_Endpoint_Address"; //live metrics
appInsights.Configuration.start();
```

Os pontos de extremidade também podem ser configurados por meio de variáveis de ambiente:

```
Instrumentation Key: "APPINSIGHTS_INSTRUMENTATIONKEY"
Profile Endpoint: "Profile_Query_Endpoint_address"
Live Metrics Endpoint: "QuickPulse_Endpoint_Address"
```

### <a name="javascript"></a>JavaScript

```javascript
<script type="text/javascript">
   var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){
      function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/next/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t
   }({
      instrumentationKey:"INSTRUMENTATION_KEY"
      endpointUrl: "TelemetryChannel_Endpoint_Address"
   });

   window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
</script>
```

## <a name="regions-that-require-endpoint-modification"></a>Regiões que requerem modificação do ponto de extremidade

No momento, a região que exigem modificações de ponto de extremidade são [do Azure governamental](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights) e [Azure China](https://docs.microsoft.com/azure/china/resources-developer-guide).

|Região |  Nome do Ponto de Extremidade | Value |
|-----------------|:------------|:-------------|
| Azure China | Canal de telemetria | `https://dc.applicationinsights.azure.cn/v2/track` |
| Azure China | QuickPulse (métricas em tempo real) |`https://quickpulse.applicationinsights.azure.cn/QuickPulseService.svc` |
| Azure China | Consulta de perfil |`https://dc.applicationinsights.azure.cn/api/profiles/{0}/appId`  |
| Azure Government | Canal de telemetria |`https://dc.applicationinsights.us/v2/track` |
| Azure Government | QuickPulse (métricas em tempo real) |`https://quickpulse.applicationinsights.us/QuickPulseService.svc` |
| Azure Government | Consulta de perfil |`https://dc.applicationinsights.us/api/profiles/{0}/appId` |

> [!NOTE]
> Sem código/extensão do agente com base em monitoramento para serviços de aplicativo do Azure está **atualmente não tem suporte** nessas regiões. Este artigo será atualizado assim que essa funcionalidade torna-se disponível.

## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre as modificações personalizadas para o Azure governamental, consulte a orientação detalhada para [monitoramento e gerenciamento do Azure](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#application-insights).
- Para saber mais sobre o Azure China, consulte a [guia estratégico do Azure na China](https://docs.microsoft.com/azure/china/).