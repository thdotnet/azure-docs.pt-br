---
title: 'Referência da API do v2 de Monitor de Status do Azure: Definir a configuração de | Microsoft Docs'
description: Referência de API v2 de Monitor de status. Set-ApplicationInsightsMonitoringConfig. Monitorar o desempenho do site sem reimplantar o site. Funciona com aplicativos web do ASP.NET hospedados no local, em máquinas virtuais ou no Azure.
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
ms.openlocfilehash: 562ce8a4267370be9b049e3b56f213f82deb89c0
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734997"
---
# <a name="status-monitor-v2-api-set-applicationinsightsmonitoringconfig-v021-alpha"></a>API de v2 do Monitor de status: Set-ApplicationInsightsMonitoringConfig (v0.2.1-alpha)

Este documento descreve um cmdlet que é um membro do [módulo do Az.ApplicationMonitor PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Status Monitor v2 está atualmente em visualização pública.
> Esta versão de visualização é fornecida sem um contrato de nível de serviço, e não é recomendável para cargas de trabalho de produção. Alguns recursos podem não ter suporte e alguns podem ter recursos restritos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>DESCRIÇÃO

Define o arquivo de configuração sem fazer uma reinstalação completa.
Reinicie o IIS para que as alterações entrem em vigor.

> [!IMPORTANT] 
> Esse cmdlet requer uma sessão do PowerShell com permissões de administrador.


## <a name="examples"></a>Exemplos

### <a name="example-with-a-single-instrumentation-key"></a>Exemplo com uma única chave de instrumentação
Neste exemplo, todos os aplicativos no computador atual serão atribuídos uma única chave de instrumentação.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### <a name="example-with-an-instrumentation-key-map"></a>Exemplo com um mapa de chave de instrumentação
Neste exemplo:
- `MachineFilter` corresponde o computador atual usando o `'.*'` curinga.
- `AppFilter='WebAppExclude'` Fornece um `null` chave de instrumentação. O aplicativo especificado não será instrumentado.
- `AppFilter='WebAppOne'` atribui o aplicativo especificado com uma chave de instrumentação exclusivo.
- `AppFilter='WebAppTwo'` atribui o aplicativo especificado com uma chave de instrumentação exclusivo.
- Por fim, `AppFilter` também usa o `'.*'` curinga para corresponder a todos os aplicativos web que não são correspondidos pelas regras anteriores e atribuir uma chave de instrumentação padrão.
- Espaços são adicionados para legibilidade.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap 
    @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
      @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'},
      @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'},
      @{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'})

```


## <a name="parameters"></a>parâmetros

### <a name="-instrumentationkey"></a>-InstrumentationKey
**Obrigatório.** Use esse parâmetro para fornecer uma única chave de instrumentação para uso por todos os aplicativos no computador de destino.

### <a name="-instrumentationkeymap"></a>-InstrumentationKeyMap
**Obrigatório.** Use esse parâmetro para fornecer várias chaves de instrumentação e um mapeamento das chaves de instrumentação usada por cada aplicativo.
Você pode criar um script de instalação única para vários computadores, definindo `MachineFilter`.

> [!IMPORTANT]
> Aplicativos corresponderá em relação às regras na ordem em que as regras são fornecidas. Portanto, você deve especificar as regras mais específicas primeiro e último as regras mais genéricas.

#### <a name="schema"></a>Esquema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'})`

- **MachineFilter** é um necessária C# regex do computador ou nome da VM.
    - '. *' corresponderá a todos
    - 'ComputerName' corresponderá apenas computadores com o nome especificado.
- **AppFilter** é um necessária C# regex do computador ou nome da VM.
    - '. *' corresponderá a todos
    - 'ApplicationName' corresponderá apenas aplicativos do IIS com o nome especificado.
- **InstrumentationKey** é necessária para habilitar o monitoramento dos aplicativos que correspondem aos duas filtros anteriores.
    - Deixe esse valor nulo se você quiser definir regras para excluir o monitoramento.


### <a name="-verbose"></a>-Verbose
**Parâmetro comum.** Use essa opção para exibir os logs detalhados.


## <a name="output"></a>Saída

Por padrão, nenhuma saída.

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkey"></a>Exemplo detalhado de saída desde a configuração de arquivo de configuração por meio de-InstrumentationKey

```
VERBOSE: Operation: InstallWithIkey
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkeymap"></a>Exemplo detalhado de saída desde a configuração de arquivo de configuração por meio de-InstrumentationKeyMap

```
VERBOSE: Operation: InstallWithIkeyMap
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

## <a name="next-steps"></a>Próximas etapas

  Exiba sua telemetria:
 - [Explorar métricas](../../azure-monitor/app/metrics-explorer.md) para monitorar o desempenho e uso.
- [Pesquise eventos e logs](../../azure-monitor/app/diagnostic-search.md) para diagnosticar problemas.
- [Use a análise](../../azure-monitor/app/analytics.md) para obter mais informações de consultas avançadas.
- [Crie painéis](../../azure-monitor/app/overview-dashboard.md).
 
 Adicione mais telemetria:
 - [Criar testes da web](monitor-web-app-availability.md) para garantir que seu site permanece ativo.
- [Adicione telemetria do cliente web](../../azure-monitor/app/javascript.md) para ver as exceções no código de página da web e para habilitar chamadas de rastreamento.
- [Adicione o SDK do Application Insights ao seu código](../../azure-monitor/app/asp-net.md) para que você possa inserir o rastreamento e registrar chamadas
 
 Faça mais com v2 de Monitor de Status:
 - Use nosso guia para [solucionar problemas de](status-monitor-v2-troubleshoot.md) v2 do Monitor de Status.
 - [Obter a configuração](status-monitor-v2-api-get-config.md) para confirmar que suas configurações foram registradas corretamente.
 - [Obter o status](status-monitor-v2-api-get-status.md) para inspecionar o monitoramento.
