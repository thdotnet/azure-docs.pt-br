---
title: 'Referência da API do v2 de Monitor de Status do Azure: Desabilitar o monitoramento | Microsoft Docs'
description: Referência de API v2 de Monitor de status. Disable-ApplicationInsightsMonitoring. Monitorar o desempenho do site sem reimplantar o site. Funciona com aplicativos web do ASP.NET hospedados no local, em máquinas virtuais ou no Azure.
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
ms.openlocfilehash: 98171692284f5f28fb189e2658d207fb80d428ba
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807140"
---
# <a name="status-monitor-v2-api-disable-applicationinsightsmonitoring-v040-alpha"></a>API de v2 do Monitor de status: Disable-ApplicationInsightsMonitoring (v0.4.0-alpha)

Este artigo descreve um cmdlet que é um membro do [módulo do Az.ApplicationMonitor PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Status Monitor v2 está atualmente em visualização pública.
> Esta versão de visualização é fornecida sem um contrato de nível de serviço, e não é recomendável para cargas de trabalho de produção. Alguns recursos podem não ter suporte e alguns podem ter recursos restritos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>DESCRIÇÃO

Desabilita o monitoramento no computador de destino.
Este cmdlet removerá edições para o IIS applicationHost. config e remover chaves do registro.

> [!IMPORTANT] 
> Esse cmdlet requer uma sessão do PowerShell com permissões de administrador.

## <a name="examples"></a>Exemplos

```powershell
PS C:\> Disable-ApplicationInsightsMonitoring
```

## <a name="parameters"></a>parâmetros 

### <a name="-verbose"></a>-Verbose
**Parâmetro comum.** Use essa opção para exibir os logs detalhados.

## <a name="output"></a>Saída


#### <a name="example-output-from-successfully-disabling-monitoring"></a>Exemplo de saída da desabilitação do monitoramento com êxito

```
Initiating Disable Process
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-00z'
in :1,237
No element in the source document matches '/configuration/location[@path='']/system.webServer/modules/add[@name='ManagedHttpModuleHelper']'
Not executing RemoveAll (transform line 1, 546)
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'disable'
GAC Module will not be removed, since this operation might cause IIS instabilities
Configuring IIS Environment for codeless attach...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring IIS Environment for instrumentation engine...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring registry for instrumentation engine...
Successfully disabled Application Insights Status Monitor
```


## <a name="next-steps"></a>Próximas etapas

 Faça mais com v2 de Monitor de Status:
 - Use nosso guia para [solucionar problemas de](status-monitor-v2-troubleshoot.md) v2 do Monitor de Status.
