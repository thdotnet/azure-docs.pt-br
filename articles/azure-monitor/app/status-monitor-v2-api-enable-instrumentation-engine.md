---
title: 'Referência da API do v2 de Monitor de Status do Azure: Habilitar mecanismo de instrumentação | Microsoft Docs'
description: Referência de API v2 de Monitor de status. Enable-InstrumentationEngine. Monitorar o desempenho do site sem reimplantar o site. Funciona com aplicativos web do ASP.NET hospedados no local, em máquinas virtuais ou no Azure.
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
ms.openlocfilehash: 79446e6676a35a1b51e5e0839eb539d730b499da
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807125"
---
# <a name="status-monitor-v2-api-enable-instrumentationengine-v040-alpha"></a>API de v2 do Monitor de status: Enable-InstrumentationEngine (v0.4.0-alpha)

Este artigo descreve um cmdlet que é um membro do [módulo do Az.ApplicationMonitor PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Status Monitor v2 está atualmente em visualização pública.
> Esta versão de visualização é fornecida sem um contrato de nível de serviço, e não é recomendável para cargas de trabalho de produção. Alguns recursos podem não ter suporte e alguns podem ter recursos restritos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>DESCRIÇÃO

Permite que o mecanismo de instrumentação, definindo algumas chaves do registro.
Reinicie o IIS para que as alterações entrem em vigor.

O mecanismo de instrumentação pode complementar os dados coletados pelos SDKs .NET.
Ele coleta eventos e mensagens que descrevem a execução de um processo gerenciado. Esses eventos e mensagens incluem os códigos de resultado de dependência, verbos HTTP e texto do comando SQL.

Habilite o mecanismo de instrumentação se:
- Você já tiver habilitado o monitoramento com o cmdlet Enable, mas não tiver habilitado o mecanismo de instrumentação.
- Você equipou manualmente seu aplicativo com os SDKs do .NET e coletar telemetria adicional.

> [!IMPORTANT] 
> Esse cmdlet requer uma sessão do PowerShell com permissões de administrador.

> [!NOTE] 
> - Esse cmdlet requer que você examine e aceite nossa declaração de licença e privacidade.
> - O mecanismo de instrumentação adiciona uma sobrecarga adicional e está desativado por padrão.

## <a name="examples"></a>Exemplos

```powershell
PS C:\> Enable-InstrumentationEngine
```

## <a name="parameters"></a>parâmetros

### <a name="-acceptlicense"></a>-AcceptLicense
**Opcional** Use essa opção para aceitar a declaração de licença e privacidade em instalações sem periféricos.

### <a name="-verbose"></a>-Verbose
**Parâmetro comum.** Use esta opção para gerar logs detalhados.

## <a name="output"></a>Saída


#### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>Exemplo de saída com êxito, habilite o mecanismo de instrumentação

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
```

## <a name="next-steps"></a>Próximas etapas

  Exiba sua telemetria:
 - [Explorar métricas](../../azure-monitor/app/metrics-explorer.md) para monitorar o desempenho e uso.
- [Pesquise eventos e logs](../../azure-monitor/app/diagnostic-search.md) para diagnosticar problemas.
- Use [analytics](../../azure-monitor/app/analytics.md) para obter mais informações de consultas avançadas.
- [Crie painéis](../../azure-monitor/app/overview-dashboard.md).
 
 Adicione mais telemetria:
 - [Criar testes da web](monitor-web-app-availability.md) para garantir que seu site permanece ativo.
- [Adicione telemetria do cliente web](../../azure-monitor/app/javascript.md) para ver as exceções no código de página da web e para habilitar chamadas de rastreamento.
- [Adicione o SDK do Application Insights ao seu código](../../azure-monitor/app/asp-net.md) para que você possa inserir o rastreamento e registrar chamadas.
 
 Faça mais com v2 de Monitor de Status:
 - Use nosso guia para [solucionar problemas de](status-monitor-v2-troubleshoot.md) v2 do Monitor de Status.
 - [Obter a configuração](status-monitor-v2-api-get-config.md) para confirmar que suas configurações foram registradas corretamente.
 - [Obter o status](status-monitor-v2-api-get-status.md) para inspecionar o monitoramento.
