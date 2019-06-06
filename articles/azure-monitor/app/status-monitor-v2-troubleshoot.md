---
title: Problemas conhecidos e v2 e solução de problemas do Azure Monitor de Status | Microsoft Docs
description: Os problemas conhecidos do Monitor de Status v2 e exemplos de solução de problemas. Monitorar o desempenho do site sem reimplantar o site. Funciona com aplicativos web do ASP.NET hospedados no local, em máquinas virtuais ou no Azure.
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
ms.openlocfilehash: f2115ee14b58030f695c9410870615f03d353cd2
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734959"
---
# <a name="troubleshooting-status-monitor-v2"></a>Solução de problemas de Status monitorar v2

Quando você habilita o monitoramento, você poderá ter problemas que impedem a coleta de dados.
Este artigo lista todos os problemas conhecidos e fornece exemplos de solução de problemas.
Se você se deparar com um problema que não está listado aqui, você pode em contato conosco em [GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues).


> [!IMPORTANT]
> Status Monitor v2 está atualmente em visualização pública.
> Esta versão de visualização é fornecida sem um contrato de nível de serviço, e não é recomendável para cargas de trabalho de produção. Alguns recursos podem não ter suporte e alguns podem ter recursos restritos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="known-issues"></a>Problemas conhecidos

### <a name="conflicting-dlls-in-an-apps-bin-directory"></a>DLLs conflitantes no diretório bin de um aplicativo

Se qualquer uma dessas DLLs estiverem presentes no diretório bin, o monitoramento pode falhar:

- Microsoft.ApplicationInsights.dll
- Microsoft.AspNet.TelemetryCorrelation.dll
- System.Diagnostics.DiagnosticSource.dll

Algumas dessas DLLs são incluídas nos modelos de aplicativo padrão do Visual Studio, mesmo se seu aplicativo não usá-los.
Você pode usar ferramentas de solução de problemas para ver o comportamento de um sintoma:

- PerfView:
    ```
    ThreadID="7,500" 
    ProcessorNumber="0" 
    msg="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ExtVer="2.8.13.5972" 
    SubscriptionId="" 
    AppName="" 
    FormattedMessage="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ```

- IISReset e aplicativo de carga (sem telemetria). Investigue com Sysinternals (Handle.exe e ListDLLs.exe):
    ```
    .\handle64.exe -p w3wp | findstr /I "InstrumentationEngine AI. ApplicationInsights"
    E54: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll

    .\Listdlls64.exe w3wp | findstr /I "InstrumentationEngine AI ApplicationInsights"
    0x0000000009be0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
    0x0000000009b90000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
    0x0000000004d20000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
    ```

### <a name="conflict-with-iis-shared-configuration"></a>Conflito com a configuração compartilhada de IIS

Se você tiver um cluster de servidores web, talvez você esteja usando um [configuração compartilhada](https://docs.microsoft.com/iis/web-hosting/configuring-servers-in-the-windows-web-platform/shared-configuration_211).
O HttpModule não pode ser injetado essa configuração compartilhada.
Execute o comando Enable em cada servidor web para instalar a DLL no GAC de cada servidor.

Depois de executar o comando Enable, conclua estas etapas:
1. Vá para o diretório de configuração compartilhada e localizar o arquivo applicationHost. config.
2. Adicione esta linha à seção de módulos da sua configuração:
    ```
    <modules>
        <!-- Registered global managed http module handler. The 'Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll' must be installed in the GAC before this config is applied. -->
        <add name="ManagedHttpModuleHelper" type="Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" preCondition="managedHandler,runtimeVersionv4.0" />
    </modules>
    ```
## <a name="troubleshooting"></a>solução de problemas
    
### <a name="troubleshooting-powershell"></a>Solução de problemas do PowerShell

#### <a name="determine-which-modules-are-available"></a>Determinar quais módulos estão disponíveis
Você pode usar o `Get-Module -ListAvailable` comando para determinar quais módulos são instalados.

#### <a name="import-a-module-into-the-current-session"></a>Importar um módulo para a sessão atual
Se um módulo não foi carregado para uma sessão do PowerShell, você pode carregá-lo manualmente usando o `Import-Module <path to psd1>` comando.


### <a name="troubleshooting-the-status-monitor-v2-module"></a>O módulo v2 do Monitor de Status de solução de problemas

#### <a name="list-the-commands-available-in-the-status-monitor-v2-module"></a>Listar os comandos disponíveis no módulo de v2 do Monitor de Status
Execute o comando `Get-Command -Module Az.ApplicationMonitor` para obter os comandos disponíveis:

```
Command type     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Disable-ApplicationInsightsMonitoring              0.2.1      Az.ApplicationMonitor
Cmdlet          Disable-InstrumentationEngine                      0.2.1      Az.ApplicationMonitor
Cmdlet          Enable-ApplicationInsightsMonitoring               0.2.1      Az.ApplicationMonitor
Cmdlet          Enable-InstrumentationEngine                       0.2.1      Az.ApplicationMonitor
Cmdlet          Get-ApplicationInsightsMonitoringConfig            0.2.1      Az.ApplicationMonitor
Cmdlet          Get-ApplicationInsightsMonitoringStatus            0.2.1      Az.ApplicationMonitor
Cmdlet          Set-ApplicationInsightsMonitoringConfig            0.2.1      Az.ApplicationMonitor
```

#### <a name="determine-the-current-version-of-the-status-monitor-v2-module"></a>Determinar a versão atual do módulo do Monitor de Status v2
Execute o `Get-ApplicationInsightsMonitoringStatus` comando para exibir as seguintes informações sobre o módulo:
   - Versão do módulo do PowerShell
   - Versão do Application Insights SDK
   - Caminhos de arquivo do módulo do PowerShell
    
Examine os [referência da API](status-monitor-v2-api-get-status.md) para obter uma descrição detalhada de como usar este cmdlet.


### <a name="troubleshooting-running-processes"></a>Solução de problemas de processos em execução

Você pode inspecionar os processos no computador instrumentado para determinar se todas as DLLs são carregadas.
Se o monitoramento está funcionando, pelo menos 12 DLLs devem ser carregados.

Use o `Get-ApplicationInsightsMonitoringStatus -InspectProcess` comando para verificar as DLLs.

Examine os [referência da API](status-monitor-v2-api-get-status.md) para obter uma descrição detalhada de como usar este cmdlet.


### <a name="collect-etw-logs-by-using-perfview"></a>Coletar logs ETW usando PerfView

#### <a name="setup"></a>Configuração

1. Baixar PerfView.exe e PerfView64.exe partir [GitHub](https://github.com/Microsoft/perfview/releases).
2. Inicie PerfView64.exe.
3. Expandir **opções avançadas**.
4. Limpe essas caixas de seleção:
    - **Zip**
    - **mesclagem**
    - **Coleção de símbolos do .NET**
5. Defini-los **provedores adicionais**: `61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,925fa42b-9ef6-5fa7-10b8-56449d7a2040,f7d60e07-e910-5aca-bdd2-9de45b46c560,7c739bb9-7861-412e-ba50-bf30d95eae36,61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,252e28f4-43f9-5771-197a-e8c7e750a984`


#### <a name="collecting-logs"></a>Coleta de logs

1. Em um console de comando com privilégios de administrador, execute o `iisreset /stop` de comando para desativar o IIS e todos os aplicativos web.
2. No PerfView, selecione **iniciar coleta**.
3. Em um console de comando com privilégios de administrador, execute o `iisreset /start` comando para iniciar o IIS.
4. Tente navegar para seu aplicativo.
5. Depois que seu aplicativo é carregado, retorne para o PerfView e selecione **parar coleta**.



## <a name="next-steps"></a>Próximas etapas

- Examine os [referência da API](status-monitor-v2-overview.md#powershell-api-reference) para saber mais sobre parâmetros, você pode ter perdido.
- Se você se deparar com um problema que não está listado aqui, você pode em contato conosco em [GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues).
