---
title: V2 do Monitor de Status do Azure - Introdução | Microsoft Docs
description: Um guia de início rápido para o Monitor de Status v2. Monitorar o desempenho do site sem reimplantar o site. Funciona com aplicativos web do ASP.NET hospedados no local, em máquinas virtuais ou no Azure.
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
ms.openlocfilehash: 4da9d8e8efd5f70718f18b2e8e35ea6b5adf6757
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734970"
---
# <a name="get-started-with-status-monitor-v2"></a>Introdução ao Monitor de Status v2

Este artigo contém os comandos de início rápido, esperados que funcione para a maioria dos ambientes.
As instruções dependem da Galeria do PowerShell para distribuir atualizações.
Suportam a esses comandos do PowerShell `-Proxy` parâmetro.

Para obter uma explicação desses comandos, as instruções de personalização e sobre como solucionar problemas, consulte o [instruções detalhadas](status-monitor-v2-detailed-instructions.md).

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

> [!IMPORTANT]
> Status Monitor v2 está atualmente em visualização pública.
> Esta versão de visualização é fornecida sem um contrato de nível de serviço, e não é recomendável para cargas de trabalho de produção. Alguns recursos podem não ter suporte e alguns podem ter recursos restritos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="download-and-install-via-powershell-gallery"></a>Baixe e instale por meio da Galeria do PowerShell

### <a name="install-prerequisites"></a>Instalar pré-requisitos
Execute o PowerShell como administrador.
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
Install-Module -Name PowerShellGet -Force
``` 
Feche o PowerShell.

### <a name="install-status-monitor-v2"></a>Status de instalação monitorar v2
Execute o PowerShell como administrador.
```powershell   
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-Module -Name Az.ApplicationMonitor -AllowPrerelease -AcceptLicense
``` 

### <a name="enable-monitoring"></a>Habilitar o monitoramento
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```
    
        
## <a name="download-and-install-manually-offline-option"></a>Baixe e instale manualmente (opção offline)
### <a name="download-the-module"></a>Baixar o módulo
Baixar manualmente a versão mais recente do módulo [da Galeria do PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor).

### <a name="unzip-and-install-status-monitor-v2"></a>Descompacte e instale o Monitor de Status v2
```powershell
$pathToNupkg = "C:\Users\t\Desktop\Az.ApplicationMonitor.0.3.0-alpha.nupkg"
$pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
$pathToNupkg | rename-item -newname $pathToZip
$pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\Az.ApplicationMonitor"
Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
```
### <a name="enable-monitoring"></a>Habilitar o monitoramento
```powershell
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
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
- [Adicione o SDK do Application Insights ao seu código](../../azure-monitor/app/asp-net.md) para que você possa inserir o rastreamento e registrar chamadas.

Faça mais com v2 de Monitor de Status:

- Examine os [instruções detalhadas](status-monitor-v2-detailed-instructions.md) para obter uma explicação dos comandos encontrados aqui.
- Use nosso guia para [solucionar problemas de](status-monitor-v2-troubleshoot.md) v2 do Monitor de Status.
