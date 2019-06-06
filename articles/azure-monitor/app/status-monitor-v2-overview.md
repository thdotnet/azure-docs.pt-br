---
title: Visão geral do Azure v2 de Monitor de Status | Microsoft Docs
description: Uma visão geral do Monitor de Status v2. Monitorar o desempenho do site sem reimplantar o site. Funciona com aplicativos web do ASP.NET hospedados no local, em máquinas virtuais ou no Azure.
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
ms.openlocfilehash: 2126408222433e6339723dc2da0d2611bb234fe8
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734153"
---
# <a name="status-monitor-v2"></a>Status Monitor v2

Status Monitor v2 é um PowerShell módulo publicado para o [da Galeria do PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor).
Ele substitui [Status Monitor](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now).
O módulo fornece instrumentação sem código do .NET de aplicativos web hospedados com o IIS.
A telemetria é enviada para o portal do Azure, onde você pode [monitor](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) seu aplicativo.

> [!IMPORTANT]
> Status Monitor v2 está atualmente em visualização pública.
> Esta versão de visualização é fornecida sem um contrato de nível de serviço, e não é recomendável para cargas de trabalho de produção. Alguns recursos podem não ter suporte e alguns podem ter recursos restritos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="powershell-gallery"></a>Galeria do PowerShell

A Galeria do PowerShell está localizada aqui: https://www.powershellgallery.com/packages/Az.ApplicationMonitor.


## <a name="instructions"></a>Instruções
- Consulte a [instruções de Introdução](status-monitor-v2-get-started.md) para obter uma introdução com exemplos de código conciso.
- Consulte a [instruções detalhadas](status-monitor-v2-detailed-instructions.md) para uma análise aprofundada sobre como começar.

## <a name="powershell-api-reference"></a>Referência de API do PowerShell
- [Disable-ApplicationInsightsMonitoring](status-monitor-v2-api-disable-monitoring.md)
- [Disable-InstrumentationEngine](status-monitor-v2-api-disable-instrumentation-engine.md)
- [Enable-ApplicationInsightsMonitoring](status-monitor-v2-api-enable-monitoring.md)
- [Enable-InstrumentationEngine](status-monitor-v2-api-enable-instrumentation-engine.md)
- [Get-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-get-config.md)
- [Get-ApplicationInsightsMonitoringStatus](status-monitor-v2-api-get-status.md)
- [Set-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-set-config.md)

## <a name="troubleshooting"></a>solução de problemas
- [Solução de problemas](status-monitor-v2-troubleshoot.md)
- [Problemas conhecidos](status-monitor-v2-troubleshoot.md#known-issues)


## <a name="faq"></a>Perguntas frequentes

- Monitor de Status v2 dá suporte a instalações de proxy?

  *Sim*. Há várias maneiras de baixar o Monitor de Status v2. Se o computador tiver acesso à internet, você pode integrar a Galeria do PowerShell usando `-Proxy` parâmetros.
Você pode baixar manualmente o módulo e instale-o em seu computador ou usá-lo diretamente.
Cada uma dessas opções é descrita de [instruções detalhadas](status-monitor-v2-detailed-instructions.md).
  
- Como verificar se a ativação foi bem-sucedida?

   Não há nenhum cmdlet para verificar a ativação foi bem-sucedida.
É recomendável que você use [Live Metrics](https://docs.microsoft.com/azure/azure-monitor/app/live-stream) para determinar rapidamente se o seu aplicativo está enviando telemetria.

   Você também pode usar [do Log Analytics](../log-query/get-started-portal.md) para listar todas as funções de nuvem atualmente enviando telemetria:
   ```Kusto
   union * | summarize count() by cloud_RoleName, cloud_RoleInstance
   ```

## <a name="next-steps"></a>Próximas etapas

Exiba sua telemetria:

* [Explorar métricas](../../azure-monitor/app/metrics-explorer.md) para monitorar o desempenho e uso.
* [Pesquise eventos e logs](../../azure-monitor/app/diagnostic-search.md) para diagnosticar problemas.
* [Use a análise](../../azure-monitor/app/analytics.md) para obter mais informações de consultas avançadas.
* [Crie painéis](../../azure-monitor/app/overview-dashboard.md).

Adicione mais telemetria:

* [Criar testes da web](monitor-web-app-availability.md) para garantir que seu site permanece ativo.
* [Adicione telemetria do cliente web](../../azure-monitor/app/javascript.md) para ver as exceções no código de página da web e para habilitar chamadas de rastreamento.
* [Adicione o SDK do Application Insights ao seu código](../../azure-monitor/app/asp-net.md) para que você possa inserir o rastreamento e registrar chamadas.

