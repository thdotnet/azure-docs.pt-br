---
title: Instruções detalhadas de v2 de Monitor de Status do Azure | Microsoft Docs
description: Instruções detalhadas para a guia de Introdução do Monitor de Status v2. Monitorar o desempenho do site sem reimplantar o site. Funciona com aplicativos web do ASP.NET hospedados no local, em máquinas virtuais ou no Azure.
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
ms.openlocfilehash: d0960c749d74903acc778c0f21d5c49f380195ae
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734192"
---
# <a name="status-monitor-v2-detailed-instructions"></a>V2 do Monitor de status: Instruções detalhadas

Este artigo descreve como fazer a integração para a Galeria do PowerShell e o download do módulo ApplicationMonitor.
Ele descreve os parâmetros mais comuns que você precisará começar a usar.
Ele também inclui instruções manuais caso você não tenha acesso à internet.

> [!IMPORTANT]
> Status Monitor v2 está atualmente em visualização pública.
> Esta versão de visualização é fornecida sem um contrato de nível de serviço, e não é recomendável para cargas de trabalho de produção. Alguns recursos podem não ter suporte e alguns podem ter recursos restritos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="get-an-instrumentation-key"></a>Obter uma chave de instrumentação

Para começar, você precisa de uma chave de instrumentação. Para obter mais informações, consulte [criar um recurso do Application Insights](create-new-resource.md#copy-the-instrumentation-key).

## <a name="run-powershell-as-admin-with-an-elevated-execution-policy"></a>Execute o PowerShell como administrador com uma política de execução com privilégios elevados

**Executar como administrador**

PowerShell precisa de permissões de nível de administrador para fazer alterações no seu computador.

**Política de execução**
- Descrição: Por padrão, a execução de scripts do PowerShell está desabilitado. É recomendável permitir que scripts RemoteSigned somente no escopo atual.
- Referência: [Sobre as políticas de execução](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6) e [Set-ExecutionPolicy](
https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-6
).
- O comando: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process`.
- Parâmetro opcional:
    - `-Force`. Ignora o prompt de confirmação.

**Erros de exemplo**

```
Install-Module : The 'Install-Module' command was found in the module 'PowerShellGet', but the module could not be
loaded. For more information, run 'Import-Module PowerShellGet'.
    
Import-Module : File C:\Program Files\WindowsPowerShell\Modules\PackageManagement\1.3.1\PackageManagement.psm1 cannot
be loaded because running scripts is disabled on this system. For more information, see about_Execution_Policies at
https:/go.microsoft.com/fwlink/?LinkID=135170.
```


## <a name="prerequisites-for-powershell"></a>Pré-requisitos para PowerShell

Auditoria de sua instância do PowerShell, executando o `$PSVersionTable` comando.
Esse comando gera a seguinte saída:


```
Name                           Value
----                           -----
PSVersion                      5.1.17763.316
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.17763.316
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
```

Estas instruções foram escritas e testadas em um computador executando o Windows 10 e versões listadas acima.

## <a name="prerequisites-for-powershell-gallery"></a>Pré-requisitos para a Galeria do PowerShell

Essas etapas irá preparar seu servidor para baixar os módulos da Galeria do PowerShell.

> [!NOTE] 
> Galeria do PowerShell há suporte para Windows 10, Windows Server 2016 e 6 do PowerShell.
> Para obter informações sobre versões anteriores, consulte [instalar o PowerShellGet](https://docs.microsoft.com/powershell/gallery/installing-psget).


1. Execute o PowerShell como administrador com uma política de execução com privilégios elevados.
2. Instale o provedor de pacote do NuGet.
    - Descrição: Você precisa desse provedor para interagir com repositórios baseados no NuGet, como a Galeria do PowerShell.
    - Referência: [Install-PackageProvider](https://docs.microsoft.com/powershell/module/packagemanagement/install-packageprovider?view=powershell-6).
    - O comando: `Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201`.
    - Parâmetros opcionais:
        - `-Proxy`. Especifica um servidor proxy para a solicitação.
        - `-Force`. Ignora o prompt de confirmação.
    
    Você receberá esse aviso se o NuGet não está configurado para:
        
        NuGet provider is required to continue
        PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with NuGet-based repositories. The NuGet
         provider must be available in 'C:\Program Files\PackageManagement\ProviderAssemblies' or
        'C:\Users\t\AppData\Local\PackageManagement\ProviderAssemblies'. You can also install the NuGet provider by running
        'Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import
         the NuGet provider now?
        [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
    
3. Configure a Galeria do PowerShell como um repositório confiável.
    - Descrição: Por padrão, a Galeria do PowerShell é um repositório não confiável.
    - Referência: [Set-PSRepository](https://docs.microsoft.com/powershell/module/powershellget/set-psrepository?view=powershell-6).
    - O comando: `Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted`.
    - Parâmetro opcional:
        - `-Proxy`. Especifica um servidor proxy para a solicitação.

    Você receberá esse aviso se a Galeria do PowerShell não é confiável:

        Untrusted repository
        You are installing the modules from an untrusted repository. If you trust this repository, change its
        InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure you want to install the modules from
        'PSGallery'?
        [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"):

    Você pode confirmar essa alteração e todas as PSRepositories de auditoria executando o `Get-PSRepository` comando.

4. Instale a versão mais recente do PowerShellGet.
    - Descrição: Esse módulo contém as ferramentas usadas para obter outros módulos da Galeria do PowerShell. Versão 1.0.0.1 é fornecido com o Windows 10 e Windows Server. Versão 1.6.0 ou posterior é necessário. Para determinar qual versão está instalada, execute o `Get-Command -Module PowerShellGet` comando.
    - Referência: [Instalar o PowerShellGet](https://docs.microsoft.com/powershell/gallery/installing-psget).
    - O comando: `Install-Module -Name PowerShellGet`.
    - Parâmetros opcionais:
        - `-Proxy`. Especifica um servidor proxy para a solicitação.
        - `-Force`. Ignora o aviso "já está instalado" e instala a versão mais recente.

    Se você não estiver usando a versão mais recente do PowerShellGet, você receberá esse erro:
    
        Install-Module : A parameter cannot be found that matches parameter name 'AllowPrerelease'.
        At line:1 char:20
        Install-Module abc -AllowPrerelease
                           ~~~~~~~~~~~~~~~~
            CategoryInfo          : InvalidArgument: (:) [Install-Module], ParameterBindingException
            FullyQualifiedErrorId : NamedParameterNotFound,Install-Module
    
5. Reinicie o PowerShell. Você não pode carregar a nova versão na sessão atual. Novas sessões do PowerShell carregará a versão mais recente do PowerShellGet.

## <a name="download-and-install-the-module-via-powershell-gallery"></a>Baixe e instale o módulo por meio da Galeria do PowerShell

Essas etapas baixará o módulo Az.ApplicationMonitor da Galeria do PowerShell.

1. Certifique-se de que todos os pré-requisitos para a Galeria do PowerShell sejam atendidos.
2. Execute o PowerShell como administrador com uma política de execução com privilégios elevados.
3. Instale o módulo Az.ApplicationMonitor.
    - Referência: [Install-Module](https://docs.microsoft.com/powershell/module/powershellget/install-module?view=powershell-6).
    - O comando: `Install-Module -Name Az.ApplicationMonitor`.
    - Parâmetros opcionais:
        - `-Proxy`. Especifica um servidor proxy para a solicitação.
        - `-AllowPrerelease`. Permite a instalação das versões alfa e beta.
        - `-AcceptLicense`. Ignora o prompt "Aceitar licença"
        - `-Force`. Ignora o aviso de "Repositório não confiável".

## <a name="download-and-install-the-module-manually-offline-option"></a>Baixe e instale o módulo manualmente (opção offline)

Se por algum motivo, você não pode se conectar o módulo do PowerShell, você pode manualmente baixar e instalar o módulo Az.ApplicationMonitor.

### <a name="manually-download-the-latest-nupkg-file"></a>Baixar manualmente o arquivo nupkg mais recente

1. Vá para https://www.powershellgallery.com/packages/Az.ApplicationMonitor.
2. Selecione a versão mais recente do arquivo na **histórico de versão** tabela.
3. Sob **opções de instalação**, selecione **Download Manual**.

### <a name="option-1-install-into-a-powershell-modules-directory"></a>Opção 1: Instalar em um diretório de módulos do PowerShell
Instale o módulo do PowerShell baixado manualmente em um diretório do PowerShell para que seja detectável pelo sessões do PowerShell.
Para obter mais informações, consulte [instalando um módulo do PowerShell](https://docs.microsoft.com/powershell/developer/module/installing-a-powershell-module).


#### <a name="unzip-nupkg-as-a-zip-file-by-using-expand-archive-v1010"></a>Descompacte o nupkg como um arquivo zip usando Expand-Archive (v1.0.1.0)

- Descrição: A versão base do Microsoft.PowerShell.Archive (v1.0.1.0) não é possível descompactar arquivos nupkg. Renomeie o arquivo com a extensão. zip.
- Referência: [Expanda-Archive](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6).
- Comando:

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.3.0-alpha.nupkg"
    $pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
    $pathToNupkg | rename-item -newname $pathToZip
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
    ```

#### <a name="unzip-nupkg-by-using-expand-archive-v1100"></a>Descompacte o nupkg usando Expand-Archive (v1.1.0.0)

- Descrição: Use uma versão atual do Expand-Archive para descompactar arquivos nupkg sem alterar a extensão.
- Referência: [Expanda-Archive](https://docs.microsoft.com/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-6) e [Microsoft.PowerShell.Archive](https://www.powershellgallery.com/packages/Microsoft.PowerShell.Archive/1.1.0.0).
- Comando:

    ```
    $pathToNupkg = "C:\az.applicationmonitor.0.2.1-alpha.nupkg"
    $pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\az.applicationmonitor"
    Expand-Archive -LiteralPath $pathToNupkg -DestinationPath $pathInstalledModule
    ```

### <a name="option-2-unzip-and-import-nupkg-manually"></a>Opção 2: Descompacte e importar nupkg manualmente
Instale o módulo do PowerShell baixado manualmente em um diretório do PowerShell para que seja detectável pelo sessões do PowerShell.
Para obter mais informações, consulte [instalando um módulo do PowerShell](https://docs.mircrosoft.com/powershell/developer/module/installing-a-powershell-module).

Se você estiver instalando o módulo em qualquer outro diretório, importe manualmente o módulo usando [Import-Module](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/import-module?view=powershell-6).

> [!IMPORTANT] 
> DLLs serão instalado por meio de caminhos relativos.
> Store o conteúdo do pacote em seu diretório do tempo de execução desejado e confirme que as permissões de acesso permitem ler, mas não gravação.

1. Altere a extensão para. zip"e extraia o conteúdo do pacote em seu diretório de instalação pretendida.
2. Localize o caminho de arquivo do Az.ApplicationMonitor.psd1.
3. Execute o PowerShell como administrador com uma política de execução com privilégios elevados.
4. Carregar o módulo usando o `Import-Module Az.ApplicationMonitor.psd1` comando.
    

## <a name="route-traffic-through-a-proxy"></a>Rotear o tráfego por meio de um proxy

Ao monitorar um computador na sua intranet privada, você precisará rotear o tráfego HTTP por meio de um proxy.

Os comandos do PowerShell para baixar e instalar Az.ApplicationMonitor da Galeria do PowerShell dão suporte um `-Proxy` parâmetro.
Quando você escreve seus scripts de instalação, examine as instruções anteriores.

SDK do Application Insights será necessário enviar telemetria do seu aplicativo à Microsoft. É recomendável que você defina configurações de proxy para seu aplicativo em seu arquivo Web. config. Para obter mais informações, consulte [perguntas frequentes sobre o Application Insights: Passagem de proxy](https://docs.microsoft.com/azure/azure-monitor/app/troubleshoot-faq#proxy-passthrough).


## <a name="enable-monitoring"></a>Habilitar o monitoramento

Use o `Enable-ApplicationInsightsMonitoring` comando para habilitar o monitoramento.

Consulte a [referência da API](status-monitor-v2-api-enable-monitoring.md) para obter uma descrição detalhada de como usar este cmdlet.



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

- Use nosso guia para [solucionar problemas de](status-monitor-v2-troubleshoot.md) v2 do Monitor de Status.
