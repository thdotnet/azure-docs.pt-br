---
title: Endpoint protection soluções descoberta e avaliação de integridade na Central de segurança do Azure | Microsoft Docs
description: Como as soluções de proteção de ponto de extremidade são descobertas e identificadas como íntegras.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
ms.assetid: 2730a2f5-20bc-4027-a1c2-db9ed0539532
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2019
ms.author: v-mohabe
ms.openlocfilehash: b17e5f16b988bfa562b00bc6f5b9dfd34be4ca43
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247958"
---
# <a name="endpoint-protection-assessment-and-recommendations-in-azure-security-center"></a>Avaliação de proteção de ponto de extremidade e as recomendações na Central de segurança do Azure

Avaliação de proteção de ponto de extremidade e as recomendações na Central de segurança do Azure detecta e fornece avaliação de integridade do [suporte](https://docs.microsoft.com/azure/security-center/security-center-os-coverage#supported-platforms-for-windows-computers-and-vms) versões das soluções de proteção de ponto de extremidade. Este tópico explica os cenários que geram as seguintes recomendações de dois para soluções de proteção de ponto de extremidade pela Central de segurança do Azure.

* **Instalar soluções de proteção de ponto de extremidade em sua máquina virtual**
* **Resolver problemas de integridade do endpoint protection em seus computadores**

## <a name="windows-defender"></a>Windows Defender

* O **"Instalar soluções de proteção de ponto de extremidade na máquina virtual"** recomendação é gerada quando [Get-MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) execuções e o resultado é **AMServiceEnabled: False**

* O **"Resolver problemas de integridade do endpoint protection em suas máquinas"** recomendação é gerada quando [Get-MpComputerStatus](https://docs.microsoft.com/powershell/module/defender/get-mpcomputerstatus?view=win10-ps) ocorre é executado e uma ou ambas das seguintes opções:

  * Pelo menos uma das propriedades a seguir é false:

     **AMServiceEnabled**

     **AntispywareEnabled**

     **RealTimeProtectionEnabled**

     **BehaviorMonitorEnabled**

     **IoavProtectionEnabled**

     **OnAccessProtectionEnabled**

  * Se uma ou ambas as propriedades a seguir for maior ou igual a 7.

     **AntispywareSignatureAge**

     **AntivirusSignatureAge**

## <a name="microsoft-system-center-endpoint-protection"></a>Proteção de ponto de extremidade do Microsoft System Center

* O **"Instalar soluções de proteção de ponto de extremidade na máquina virtual"** recomendação for gerada durante a importação de **SCEPMpModule ("$env: ProgramFiles\Microsoft segurança Client\MpProvider\MpProvider.psd1")** e em execução **Get-MProtComputerStatus** os resultados com **AMServiceEnabled = false**

* O **"Resolver problemas de integridade do endpoint protection em suas máquinas"** recomendação é gerada quando **Get-MprotComputerStatus** ocorre é executado e uma ou ambas das seguintes opções:

    * Pelo menos uma das propriedades a seguir é false:

       **AMServiceEnabled**
    
       **AntispywareEnabled**
    
       **RealTimeProtectionEnabled**
    
       **BehaviorMonitorEnabled**
    
       **IoavProtectionEnabled**
    
       **OnAccessProtectionEnabled**
          
    * Se uma ou ambas as seguintes atualizações de assinatura é maior ou igual a 7. 

       **AntispywareSignatureAge**
    
       **AntivirusSignatureAge**

## <a name="trend-micro"></a>Trend Micro

* O **"Instalar soluções de proteção de ponto de extremidade na máquina virtual"** recomendação é gerada se um ou mais dos seguintes verificações não forem atendidos:
    * **O agente de segurança HKLM:\SOFTWARE\TrendMicro\Deep** existe
    * **HKLM:\SOFTWARE\TrendMicro\Deep segurança Agent\InstallationFolder** existe
    * O **dsq_query.cmd** arquivo é encontrado na pasta de instalação
    * Executando **dsa_query.cmd** os resultados com **Component.AM.mode: no - Trend Micro Deep Security Agent detectado**

## <a name="symantec-endpoint-protection"></a>Symantec endpoint protection
O **"Instalar soluções de proteção de ponto de extremidade na máquina virtual"** recomendação será gerada se qualquer um dos seguintes verificações não forem atendidos:

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

Ou

* **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\CurrentVersion\PRODUCTNAME = "Symantec Endpoint Protection"**

* **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\ASRunningStatus = 1**

O **"Resolver problemas de integridade do endpoint protection em suas máquinas"** recomendação será gerada se qualquer um dos seguintes verificações não forem atendidos:  

* Check Symantec Version >= 12:  Local do registro: **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion" -Value "PRODUCTVERSION"**

* Verificar o status de proteção em tempo Real: **HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection\AV\Storages\Filesystem\RealTimeScan\OnOff == 1**

* Verificar o status de atualização de assinatura: **Ponto de extremidade HKLM\Software\Symantec\Symantec Protection\CurrentVersion\public-opstate\LatestVirusDefsDate < = 7 dias**

* Verificar o status de verificação completa: **HKLM:\Software\Symantec\Symantec Endpoint Protection\CurrentVersion\public-opstate\LastSuccessfulScanDateTime <= 7 days**

* Localize o número de versão de assinatura caminho para a versão de assinatura de 12 da Symantec: **Registry Paths+ "CurrentVersion\SharedDefs" -Value "SRTSP"** 

* Caminho para a versão da assinatura da Symantec 14: **Registry Paths+ "CurrentVersion\SharedDefs\SDSDefs" -Value "SRTSP"**

Caminhos de registro:

**"HKLM:\Software\Symantec\Symantec Endpoint Protection" + $Path;** 
 **"HKLM:\Software\Wow6432Node\Symantec\Symantec Endpoint Protection" + $Path**

## <a name="mcafee-endpoint-protection-for-windows"></a>McAfee endpoint protection para Windows

O **"Instalar soluções de proteção de ponto de extremidade na máquina virtual"** recomendação será gerada se as seguintes verificações não forem atendidas:

* **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion** existe

* **HKLM:\SOFTWARE\McAfee\AVSolution\MCSHIELDGLOBAL\GLOBAL\enableoas = 1**

O **"Resolver problemas de integridade do endpoint protection em suas máquinas"** recomendação será gerada se as seguintes verificações não forem atendidas:

* McAfee versão: **HKLM:\SOFTWARE\McAfee\Endpoint\AV\ProductVersion >= 10**

* Localize a versão da assinatura: **HKLM:\Software\McAfee\AVSolution\DS\DS -Value "dwContentMajorVersion"**

* Localize a data da assinatura: **HKLM:\Software\McAfee\AVSolution\DS\DS -Value "szContentCreationDate" >= 7 days**

* Encontre a data de verificação: **HKLM:\Software\McAfee\Endpoint\AV\ODS -Value "LastFullScanOdsRunTime" >= 7 days**

## <a name="troubleshoot-and-support"></a>Solução de problemas e suporte

### <a name="troubleshoot"></a>Solução de problemas

Logs de extensão de Antimalware da Microsoft estão disponíveis em:  
**%Systemdrive%\WindowsAzure\Logs\Plugins\Microsoft.Azure.Security.IaaSAntimalware(Or PaaSAntimalware)\1.5.5.x(version#)\CommandExecution.log**

### <a name="support"></a>Suporte

Caso precise de mais ajuda em qualquer ponto deste artigo, entre em contato com os especialistas do Azure nos [fóruns do Azure e do Stack Overflow no MSDN](https://azure.microsoft.com/support/forums/). Ou, você pode registrar um incidente de suporte do Azure. Vá para o [site de suporte do Azure](https://azure.microsoft.com/support/options/) e selecione Obter suporte. Para saber mais sobre como usar o suporte do Azure, leia as [Perguntas frequentes sobre o suporte do Microsoft Azure](https://azure.microsoft.com/support/faq/).
