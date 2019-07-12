---
title: 'Referência da API do v2 de Monitor de Status do Azure: Iniciar rastreamento | Microsoft Docs'
description: Referência de API v2 de Monitor de status. Rastreamento de início. Colete logs ETW do Monitor de Status e o SDK do Application Insights.
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: b6787134707273a76290adb723a9bc9012252ebd
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807052"
---
# <a name="status-monitor-v2-api-start-applicationinsightsmonitoringtrace-v040-alpha"></a>API de v2 do Monitor de status: Start-ApplicationInsightsMonitoringTrace (v0.4.0-alpha)

Este artigo descreve um cmdlet que é um membro do [módulo do Az.ApplicationMonitor PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Status Monitor v2 está atualmente em visualização pública.
> Esta versão de visualização é fornecida sem um contrato de nível de serviço, e não é recomendável para cargas de trabalho de produção. Alguns recursos podem não ter suporte e alguns podem ter recursos restritos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>DESCRIÇÃO

Coleta [eventos ETW](https://docs.microsoft.com/windows/desktop/etw/event-tracing-portal) desde sem o código a conexão em tempo de execução. Esse cmdlet é uma alternativa à execução [PerfView](https://github.com/microsoft/perfview).

Os eventos coletados serão impressas no console em tempo real e salvo em um arquivo ETL. O arquivo ETL de saída pode ser aberto pelo [PerfView](https://github.com/microsoft/perfview) para uma investigação adicional.

Esse cmdlet será executado até que ela atinja a duração de tempo limite (padrão de 5 minutos) ou é interrompida manualmente (`Ctrl + C`).

> [!IMPORTANT] 
> Esse cmdlet requer uma sessão do PowerShell com permissões de administrador.

## <a name="examples"></a>Exemplos

### <a name="how-to-collect-events"></a>Como coletar eventos

Normalmente, seria solicitaremos que você colete eventos para investigar por que seu aplicativo não está sendo instrumentado.

Tempo de execução de anexar sem o código emite eventos de ETW quando o IIS é iniciado e quando seu aplicativo é iniciado.

Para coletar esses eventos:
1. Em um console de cmd com privilégios de administrador, execute `iisreset /stop` para desativar o IIS e todos os aplicativos web.
2. Execute este cmdlet
3. Em um console de cmd com privilégios de administrador, execute `iisreset /start` para iniciar o IIS.
4. Tente navegar para seu aplicativo.
5. Quando seu aplicativo termina de carregar, você pode interrompê-lo manualmente (`Ctrl + C`) ou aguardar o tempo limite.

### <a name="what-events-to-collect"></a>Eventos a serem coletados

Quando a coleta de eventos, você tem três opções:
1. Use a opção `-CollectSdkEvents` para coletar eventos emitidos do SDK do Application Insights.
2. Use a opção `-CollectRedfieldEvents` para coletar eventos emitidos pelo Monitor de Status e o Runtime de Redfield. Esses logs são úteis ao diagnosticar o IIS e inicialização do aplicativo.
3. Use ambas as opções para coletar os dois tipos de evento.
4. Por padrão, se nenhuma opção for especificada, os dois tipos de evento serão coletados.


## <a name="parameters"></a>parâmetros

### <a name="-maxdurationinminutes"></a>-MaxDurationInMinutes
**Opcional** Use esse parâmetro para definir quanto tempo esse script deve coletar eventos. O padrão é de 5 minutos.

### <a name="-logdirectory"></a>-LogDirectory
**Opcional** Use essa opção para definir o diretório de saída do arquivo ETL. Por padrão, esse arquivo será criado no diretório de módulos do PowerShell. O caminho completo será exibido durante a execução do script.


### <a name="-collectsdkevents"></a>-CollectSdkEvents
**Opcional** Use essa opção para coletar eventos do SDK do Application Insights.

### <a name="-collectredfieldevents"></a>-CollectRedfieldEvents
**Opcional** Use essa opção para coletar eventos do Monitor de Status e o tempo de execução Redfield.

### <a name="-verbose"></a>-Verbose
**Parâmetro comum.** Use esta opção para gerar logs detalhados.



## <a name="output"></a>Saída


### <a name="example-of-application-startup-logs"></a>Exemplo de logs de inicialização do aplicativo
```
PS C:\Windows\system32> Start-ApplicationInsightsMonitoringTrace -ColectRedfieldEvents
Starting...
Log File: C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\logs\20190627_144217_ApplicationInsights_ETW_Trace.etl
Tracing enabled, waiting for events.
Tracing will timeout in 5 minutes. Press CTRL+C to cancel.

2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:31 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 70 ms
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Current assembly 'Microsoft.ApplicationInsights.RedfieldIISModule, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3' location 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Matched filter '.*'~'STATUSMONITORTE', '.*'~'DemoWithSql'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace Lightup assembly calculated path: 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.Redfield.Lightup.dll'
2:42:31 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Loaded applicationInsights.config from assembly's resource Microsoft.ApplicationInsights.Redfield.Lightup, Version=2.8.18.27202, Culture=neutral, PublicKeyToken=f23a46de0be5d6f3/Microsoft.ApplicationInsights.Redfield.Lightup.ApplicationInsights-recommended.config
2:42:34 PM EVENT: Microsoft-ApplicationInsights-FrameworkLightup Trace Successfully attached ApplicationInsights SDK
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.LoadLightupAssemblyAndGetLightupHttpModuleClass, success, 2687 ms
2:42:34 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:34 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 3288 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftAppInsights_ManagedHttpModulePath='C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll', MicrosoftAppInsights_ManagedHttpModuleType='Microsoft.ApplicationInsights.RedfieldIISModule.RedfieldIISModule'
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Resolved variables to: MicrosoftDiagnosticServices_ManagedHttpModulePath2='', MicrosoftDiagnosticServices_ManagedHttpModuleType2=''
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace Environment variable 'MicrosoftDiagnosticServices_ManagedHttpModulePath2' or 'MicrosoftDiagnosticServices_ManagedHttpModuleType2' is null, skipping managed dll loading
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace MulticastHttpModule.constructor, success, 0 ms
2:42:35 PM EVENT: Microsoft-ApplicationInsights-RedfieldIISModule Trace RedfieldIISModule.CreateAndInitializeApplicationInsightsHttpModules(lightupHttpModuleClass), success
2:42:35 PM EVENT: Microsoft-ApplicationInsights-IIS-ManagedHttpModuleHelper Trace ManagedHttpModuleHelper, multicastHttpModule.Init() success, 0 ms
Timeout Reached. Stopping...
```


## <a name="next-steps"></a>Próximas etapas

Solução de problemas adicionais:

- Examine as etapas de solução de problemas adicionais aqui: https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-troubleshoot
- Examine os [referência da API](status-monitor-v2-overview.md#powershell-api-reference) para saber mais sobre parâmetros, você pode ter perdido.
- Se você precisar de ajuda adicional, você pode em contato conosco em [GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues).



 Faça mais com v2 de Monitor de Status:
 - Use nosso guia para [solucionar problemas de](status-monitor-v2-troubleshoot.md) v2 do Monitor de Status.
 - [Obter a configuração](status-monitor-v2-api-get-config.md) para confirmar que suas configurações foram registradas corretamente.
 - [Obter o status](status-monitor-v2-api-get-status.md) para inspecionar o monitoramento.
