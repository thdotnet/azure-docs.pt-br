---
title: Instalar o Office em uma imagem de VHD mestre - Azure
description: Como instalar e personalizar o Office em uma imagem mestre de visualização de área de trabalho Virtual do Windows Azure.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 05/02/2019
ms.author: v-chjenk
ms.openlocfilehash: 6d2bb7efdd5567da377e1e15fec4935b7d4a3a6f
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444128"
---
# <a name="install-office-on-a-master-vhd-image"></a>Instalar o Office em uma imagem de VHD mestre

Este artigo informa como instalar o Office 365 ProPlus, OneDrive e outros aplicativos comuns em uma imagem de disco de rígido virtual (VHD) mestre para carregar no Azure. Se os usuários precisam acessar determinados aplicativos de linha de negócios (LOB), é recomendável que instalá-los depois de concluir as instruções neste artigo.

Este artigo pressupõe que você já tiver criado uma máquina virtual (VM). Caso contrário, consulte [preparar e personalizar uma imagem de VHD mestre](set-up-customize-master-image.md#create-a-vm)

Este artigo também pressupõe que têm acesso elevado na VM, se ela é provisionada no Azure ou o Gerenciador do Hyper-V. Caso contrário, consulte [elevar o acesso para gerenciar todos os grupos de gerenciamento e a assinatura do Azure](https://docs.microsoft.com/azure/role-based-access-control/elevate-access-global-admin).

>[!NOTE]
>Essas instruções são para uma configuração específica de visualização de área de trabalho Virtual do Windows que pode ser usada com os processos existentes da sua organização.

## <a name="install-office-in-shared-computer-activation-mode"></a>Instalar o Office no modo de ativação de computador compartilhado

Ativação de computador compartilhado permite que você implantar o Office 365 ProPlus em um computador em sua organização que é acessada por vários usuários. Para obter mais informações sobre a ativação de computador compartilhado, consulte [visão geral da ativação de computador compartilhado do Office 365 ProPlus](https://docs.microsoft.com/DeployOffice/overview-of-shared-computer-activation-for-office-365-proplus).

Use o [ferramenta de implantação do Office](https://www.microsoft.com/download/details.aspx?id=49117) para instalar o Office. Várias sessões do Windows 10 Enterprise só suporta as seguintes versões do Office:
- Office 365 ProPlus
- Office 365 Business vem com uma assinatura do Microsoft 365 Business

A ferramenta de implantação do Office requer um arquivo XML de configuração. Para personalizar o exemplo a seguir, consulte a [opções de configuração para a ferramenta de implantação do Office](https://docs.microsoft.com/deployoffice/configuration-options-for-the-office-2016-deployment-tool).

Este exemplo de configuração XML que fornecemos fará o seguinte:

- Instalar o Office do canal Insiders e fornecer atualizações do canal Insiders quando forem executados.
- Arquitetura de uso x64.
- Desabilite as atualizações automáticas.
- Instale o Visio e o projeto.
- Remova quaisquer instalações existentes do Office e migrar suas configurações.
- Habilite a ativação de computador compartilhado.

>[!NOTE]
>Recurso de pesquisa de estêncil do Visio não opera em área de trabalho Virtual do Windows durante a configuração de versão prévia.

Aqui está o que não fazer esse exemplo de configuração XML:

- Instalar o Skype for Business
- Instale o OneDrive em modo por usuário. Para obter mais informações, consulte [OneDrive instalar no modo de por máquina](#install-onedrive-in-per-machine-mode).

>[!NOTE]
>Ativação de computador compartilhado pode ser configurada por meio de configurações do registro ou objetos de diretiva de grupo (GPOs). O GPO está localizado em **configuração do computador\\diretivas\\modelos administrativos\\(máquina) do Microsoft Office 2016\\configurações de licenciamento**

A ferramenta de implantação do Office contém setup.exe. Para instalar o Office, execute o seguinte comando em uma linha de comando:

```batch
Setup.exe /configure configuration.xml
```

#### <a name="sample-configurationxml"></a>XML de exemplo

O exemplo XML a seguir instalará a versão de especialistas.

```xml
<Configuration>
    <Add OfficeClientEdition="64" SourcePath="https://officecdn.microsoft.com/pr/5440fd1f-7ecb-4221-8110-145efaa6372f">
        <Product ID="O365ProPlusRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Groove" />
            <ExcludeApp ID="Lync" />
            <ExcludeApp ID="OneDrive" />
            <ExcludeApp ID="Teams" />
        </Product>
        <Product ID="VisioProRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Teams" /> 
        </Product>
        <Product ID="ProjectProRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Teams" />
        </Product>
    </Add>
    <RemoveMSI All="True" />
    <Updates Enabled="FALSE" UpdatePath="https://officecdn.microsoft.com/pr/5440fd1f-7ecb-4221-8110-145efaa6372f" />
    <Display Level="None" AcceptEULA="TRUE" />
    <Logging Level="Verbose" Path="%temp%\WVDOfficeInstall" />
    <Property Value="TRUE" Name="FORCEAPPSHUTDOWN"/>
    <Property Value="1" Name="SharedComputerLicensing"/>
    <Property Value="TRUE" Name="PinIconsToTaskbar"/>
</Configuration>
```

>[!NOTE]
>A equipe do Office recomenda o uso de instalação de 64 bits para o **OfficeClientEdition** parâmetro.

Depois de instalar o Office, você pode atualizar o comportamento do Office padrão. Execute os seguintes comandos individualmente ou em um arquivo em lotes para atualizar o comportamento.

```batch
rem Mount the default user registry hive
reg load HKU\TempDefault C:\Users\Default\NTUSER.DAT
rem Must be executed with default registry hive mounted.
reg add HKU\TempDefault\SOFTWARE\Policies\Microsoft\office\16.0\common /v InsiderSlabBehavior /t REG_DWORD /d 2 /f
rem Set Outlook's Cached Exchange Mode behavior
rem Must be executed with default registry hive mounted.
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v enable /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v syncwindowsetting /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v CalendarSyncWindowSetting /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v CalendarSyncWindowSettingMonths  /t REG_DWORD /d 1 /f
rem Unmount the default user registry hive
reg unload HKU\TempDefault

rem Set the Office Update UI behavior.
reg add HKLM\SOFTWARE\Policies\Microsoft\office\16.0\common\officeupdate /v hideupdatenotifications /t REG_DWORD /d 1 /f
reg add HKLM\SOFTWARE\Policies\Microsoft\office\16.0\common\officeupdate /v hideenabledisableupdates /t REG_DWORD /d 1 /f
```

## <a name="install-onedrive-in-per-machine-mode"></a>Instalar o OneDrive no modo de por máquina

OneDrive é normalmente instalado por usuário. Nesse ambiente, ele deve ser instalado por máquina.

Aqui está como instalar o OneDrive no modo de por máquina:

1. Primeiro, crie um local para preparar o instalador do OneDrive. Uma pasta de disco local ou [\\\\unc] (file://unc) local está tudo bem.

2. Baixe OneDriveSetup.exe para seu local de preparo com este link: <https://aka.ms/OneDriveWVD-Installer>

3. Se você instalar o office com o OneDrive, omitindo  **\<ExcludeApp ID = "OneDrive" /\>** , desinstale qualquer existente OneDrive as instalações por usuário em um prompt de comando elevado executando o seguinte comando:
    
    ```batch
    "[staged location]\OneDriveSetup.exe" /uninstall
    ```

4. Execute este comando em um prompt de comando elevado para definir a **AllUsersInstall** valor do registro:

    ```batch
    REG ADD "HKLM\Software\Microsoft\OneDrive" /v "AllUsersInstall" /t REG_DWORD /d 1 /reg:64
    ```

5. Execute este comando para instalar o OneDrive no modo de por máquina:

    ```batch
    Run "[staged location]\OneDriveSetup.exe" /allusers
    ```

6. Execute este comando para configurar o OneDrive para iniciar na entrada para todos os usuários:

    ```batch
    REG ADD "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" /v OneDrive /t REG_SZ /d "C:\Program Files (x86)\Microsoft OneDrive\OneDrive.exe /background" /f
    ```

7. Habilitar **silenciosamente configurar conta de usuário** executando o comando a seguir.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "SilentAccountConfig" /t REG_DWORD /d 1 /f
    ```

8. Redirecionar e mover conhecido pastas no OneDrive, executando o seguinte comando do Windows.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "KFMSilentOptIn" /t REG_SZ /d "<your-AzureAdTenantId>" /f
    ```

## <a name="teams-and-skype"></a>As equipes e o Skype

Área de trabalho Virtual do Windows não dá suporte a Skype para empresas e equipes.

## <a name="next-steps"></a>Próximas etapas

Agora que você adicionou o Office na imagem, você pode continuar a personalizar a imagem VHD mestre. Ver [preparar e personalizar uma imagem de VHD mestre](set-up-customize-master-image.md).
