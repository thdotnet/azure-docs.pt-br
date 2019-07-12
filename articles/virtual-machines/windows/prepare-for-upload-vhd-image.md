---
title: Preparar um VHD do Windows para carregar no Azure | Microsoft Docs
description: Saiba como preparar um VHD do Windows ou o VHDX para carregá-lo no Azure
services: virtual-machines-windows
documentationcenter: ''
author: glimoli
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 7802489d-33ec-4302-82a4-91463d03887a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2019
ms.author: genli
ms.openlocfilehash: f40b3e0d2a49f6522149a977572d4f3c12e34255
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67720055"
---
# <a name="prepare-a-windows-vhd-or-vhdx-to-upload-to-azure"></a>Preparar um VHD ou VHDX do Windows para carregar no Azure

Antes de carregar uma máquina virtual do Windows (VM) do local para o Azure, você deve preparar o disco rígido virtual (VHD ou VHDX). O Azure suporta geração 1 e geração 2 VMs que estão no formato de arquivo do VHD e que têm um disco de tamanho fixo. O tamanho máximo permitido para o VHD é 1.023 GB. 

Em uma geração 1 VM, você pode converter um sistema de arquivos VHDX em VHD. Você também pode converter um disco de expansão dinâmica em um disco de tamanho fixo. No entanto, não é possível alterar a geração de uma VM. Para obter mais informações, consulte [devo criar uma geração 1 ou 2 VM no Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v) e [suporte do Azure para a geração 2 VMs (visualização)](generation-2.md).

Para obter informações sobre a política de suporte para VMs do Azure, consulte [suporte de software de servidor da Microsoft para máquinas virtuais do Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

> [!NOTE]
> As instruções neste artigo se aplicam para a versão de 64 bits do Windows Server 2008 R2 e sistemas de operacionais posteriores do Windows Server. Para obter informações sobre como executar um sistema operacional de 32 bits no Azure, consulte [suporte para sistemas operacionais de 32 bits em VMs do Azure](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines).

## <a name="convert-the-virtual-disk-to-a-fixed-size-and-to-vhd"></a>Converter o disco virtual para um tamanho fixo e VHD 
Se precisar converter o disco virtual em um formato necessário para o Azure, use um dos métodos desta seção. Fazer backup da VM antes de converter o disco virtual. Verifique se que o VHD do Windows funciona corretamente no servidor local. Em seguida, resolva quaisquer erros na própria VM antes de tentar converter ou carregá-lo no Azure.

Depois de converter o disco, crie uma VM que usa o disco. Inicie e entre VM para concluir a preparação para carregar.

### <a name="use-hyper-v-manager-to-convert-the-disk"></a>Use o Gerenciador do Hyper-V para converter o disco 
1. Abra o Gerenciador do Hyper-V e selecione o computador local à esquerda. No menu acima da lista de computador, selecione **ação** > **Editar disco**.
2. Sobre o **localizar disco rígido Virtual** , selecione o disco virtual.
3. Sobre o **escolher ação** página, selecione **converter** > **próximo**.
4. Se você precisar converter de VHDX, selecione **VHD** > **próximo**.
5. Se você precisar converter de um disco de expansão dinâmica, selecione **tamanho fixo** > **próximo**.
6. Localize e selecione um caminho no qual salvar o novo arquivo VHD.
7. Selecione **Concluir**.

> [!NOTE]
> Use uma sessão do PowerShell com privilégios elevados para executar os comandos neste artigo.

### <a name="use-powershell-to-convert-the-disk"></a>Usar o PowerShell para converter o disco 
Você pode converter um disco virtual usando o comando [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) no Windows PowerShell. Escolha **Executar como administrador** ao iniciar o PowerShell. 

O comando de exemplo a seguir converte o disco de VHDX em VHD. O comando também converte o disco de um disco de expansão dinâmica em um disco de tamanho fixo.

```Powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

Neste comando, substitua o valor de `-Path` com o caminho para o disco rígido virtual que você deseja converter. Substitua o valor de `-DestinationPath` com o novo caminho e o nome do disco convertido.

### <a name="convert-from-vmware-vmdk-disk-format"></a>Converter do formato de disco VMware VMDK
Se você tiver uma imagem de VM do Windows na [formato de arquivo VMDK](https://en.wikipedia.org/wiki/VMDK), use o [Microsoft Virtual Machine Converter](https://www.microsoft.com/download/details.aspx?id=42497) convertê-la em formato VHD. Para obter mais informações, consulte [como converter a VMware VMDK para VHD do Hyper-V](https://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx).

## <a name="set-windows-configurations-for-azure"></a>Definir configurações do Windows para o Azure

Na VM que você pretende carregar no Azure, execute os seguintes comandos de um [janela de prompt de comando com privilégios elevados](https://technet.microsoft.com/library/cc947813.aspx):

1. Remova qualquer rota persistente estática na tabela de roteamento:
   
   * Para exibir a tabela de rotas, execute `route print` no prompt de comando.
   * Verifique o `Persistence Routes` seções. Se houver uma rota persistente, use o `route delete` comando removê-lo.
2. Remova o proxy de WinHTTP:
   
    ```PowerShell
    netsh winhttp reset proxy
    ```

    Se a VM precisa trabalhar com um proxy específico, adicione uma exceção de proxy para o endereço IP do Azure ([168.63.129.16](https://blogs.msdn.microsoft.com/mast/2015/05/18/what-is-the-ip-address-168-63-129-16/
)) para a máquina virtual possa se conectar ao Azure:
    ```
    $proxyAddress="<your proxy server>"
    $proxyBypassList="<your list of bypasses>;168.63.129.16"

    netsh winhttp set proxy $proxyAddress $proxyBypassList
    ```

3. Defina a política de SAN do disco para [ `Onlineall` ](https://technet.microsoft.com/library/gg252636.aspx):
   
    ```PowerShell
    diskpart 
    ```
    Na janela do prompt de comando aberta, digite os seguintes comandos:

     ```DISKPART
    san policy=onlineall
    exit   
    ```

4. Definir tempo de tempo Universal Coordenado (UTC) para Windows. Também defina o tipo de inicialização do serviço de tempo do Windows (`w32time`) para `Automatic`:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\TimeZoneInformation' -name "RealTimeIsUniversal" -Value 1 -Type DWord -force

    Set-Service -Name w32time -StartupType Automatic
    ```
5. Defina o perfil de energia para alto desempenho:

    ```PowerShell
    powercfg /setactive SCHEME_MIN
    ```
6. Verifique se as variáveis ambientais `TEMP` e `TMP` são definidos para seus valores padrão:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -name "TEMP" -Value "%SystemRoot%\TEMP" -Type ExpandString -force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -name "TMP" -Value "%SystemRoot%\TEMP" -Type ExpandString -force
    ```

## <a name="check-the-windows-services"></a>Verificar os serviços Windows
Certifique-se de que cada um dos seguintes serviços do Windows é definida como os valores de padrão do Windows. Esses serviços são o mínimo que deve ser configurado para garantir a conectividade VM. Para redefinir as configurações de inicialização, execute os seguintes comandos:
   
```PowerShell
Set-Service -Name bfe -StartupType Automatic
Set-Service -Name dhcp -StartupType Automatic
Set-Service -Name dnscache -StartupType Automatic
Set-Service -Name IKEEXT -StartupType Automatic
Set-Service -Name iphlpsvc -StartupType Automatic
Set-Service -Name netlogon -StartupType Manual
Set-Service -Name netman -StartupType Manual
Set-Service -Name nsi -StartupType Automatic
Set-Service -Name termService -StartupType Manual
Set-Service -Name MpsSvc -StartupType Automatic
Set-Service -Name RemoteRegistry -StartupType Automatic
```

## <a name="update-remote-desktop-registry-settings"></a>Atualizar configurações de registro de área de trabalho remota
Verifique se que as seguintes configurações estão configuradas corretamente para acesso remoto:

>[!NOTE] 
>Você pode receber uma mensagem de erro ao executar `Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services -name &lt;object name&gt; -value &lt;value&gt;`. Você pode ignorar com segurança esta mensagem. Isso significa apenas que o domínio não está enviando por push que a configuração por meio de um objeto de diretiva de grupo.

1. O protocolo RDP está habilitado:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0 -Type DWord -force

    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDenyTSConnections" -Value 0 -Type DWord -force
    ```
   
2. A porta do RDP está configurada corretamente. A porta padrão é 3389:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "PortNumber" -Value 3389 -Type DWord -force
    ```
    Ao implantar uma VM, as regras padrão são criadas em relação à porta 3389. Se você quiser alterar o número da porta, faça-o após a VM é implantada no Azure.

3. O ouvinte escuta em todos os adaptadores de rede:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "LanAdapter" -Value 0 -Type DWord -force
   ```
4. Configure o modo de autenticação no nível de rede (NLA) para as conexões de RDP:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" -Value 1 -Type DWord -force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SecurityLayer" -Value 1 -Type DWord -force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "fAllowSecProtocolNegotiation" -Value 1 -Type DWord -force
     ```

5. Defina o valor de keep alive:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveEnable" -Value 1  -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveInterval" -Value 1  -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "KeepAliveTimeout" -Value 1 -Type DWord -force
    ```
6. Reconectar-se:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDisableAutoReconnect" -Value 0 -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fInheritReconnectSame" -Value 1 -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fReconnectSame" -Value 0 -Type DWord -force
    ```
7. Limite o número de conexões simultâneas:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "MaxInstanceCount" -Value 4294967295 -Type DWord -force
    ```
8. Remova todos os certificados autoassinados vinculados ao ouvinte do RDP:
    
    ```PowerShell
    Remove-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SSLCertificateSHA1Hash" -force
    ```
    Esse código garante que você pode conectar no início, quando você implanta a VM. Se você precisar examinar isso mais tarde, você pode fazer isso depois que a VM é implantada no Azure.

9. Se a VM for fazer parte de um domínio, verifique as políticas a seguir para verificar se que as configurações anteriores não são revertidas. 
    
    | Objetivo                                     | Política                                                                                                                                                       | Valor                                                                                    |
    |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
    | RDP está habilitado                           | Configuração do Computador\Diretivas\Configurações do Windows\Modelos Administrativos\Componentes\Serviços de Área de Trabalho Remota\Host de Sessão da Área de Trabalho Remota\Conexões         | Permitir que os usuários se conectem remotamente usando a Área de Trabalho Remota                                  |
    | Diretiva de grupo do NLA                         | Configurações\Modelos Administrativos\Componentes\Serviços de Área de Trabalho Remota\Host de Sessão da Área de Trabalho Remota\Segurança                                                    | Exigir autenticação do usuário para acesso remoto usando o NLA |
    | Configurações de keep alive                      | Configuração do Computador\Políticas\Configurações do Windows\Modelos Administrativos\Componentes do Windows\ Serviços de Área de Trabalho Remota\Host da Sessão da Área de Trabalho Remota\Conexões | Configurar o intervalo de conexão keep-alive                                                 |
    | Configurações de reconexão                       | Configuração do Computador\Políticas\Configurações do Windows\Modelos Administrativos\Componentes do Windows\ Serviços de Área de Trabalho Remota\Host da Sessão da Área de Trabalho Remota\Conexões | Reconectar-se automaticamente                                                                   |
    | Número limitado de configurações de conexão | Configuração do Computador\Políticas\Configurações do Windows\Modelos Administrativos\Componentes do Windows\ Serviços de Área de Trabalho Remota\Host da Sessão da Área de Trabalho Remota\Conexões | Limitar o número de conexões                                                              |

## <a name="configure-windows-firewall-rules"></a>Configure as regras do Firewall do Windows
1. Ative Firewall do Windows nos três perfis (domínio, padrão e público):

   ```PowerShell
    Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True
   ```

2. Execute o seguinte comando do PowerShell para permitir o WinRM por meio de três perfis de firewall (domínio, privado e público) e habilitar o serviço remoto do PowerShell:
   
   ```PowerShell
    Enable-PSRemoting -force

    Set-NetFirewallRule -DisplayName "Windows Remote Management (HTTP-In)" -Enabled True
   ```
3. Ative as seguintes regras de firewall para permitir o tráfego RDP:

   ```PowerShell
    Set-NetFirewallRule -DisplayGroup "Remote Desktop" -Enabled True
   ```   
4. Habilite a regra para o compartilhamento de impressora e para a VM possa responder a um comando ping dentro da rede virtual:

   ```PowerShell
   Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -Enabled True
   ``` 
5. Se a VM for fazer parte de um domínio, verifique as seguintes políticas de AD do Azure para garantir que as configurações anteriores não são revertidas. 

    | Objetivo                                 | Política                                                                                                                                                  | Valor                                   |
    |--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
    | Habilitar os perfis do Firewall do Windows | Configuração do Computador\Políticas\Configurações do Windows\Modelos Administrativos\Rede\Conexão de Rede\Firewall do Windows\Perfil de Domínio\Firewall do Windows   | Proteger todas as conexões de rede         |
    | Habilitar o RDP                           | Configuração do Computador\Políticas\Configurações do Windows\Modelos Administrativos\Rede\Conexão de Rede\Firewall do Windows\Perfil de Domínio\Firewall do Windows   | Permitir exceções de área de trabalho remota de entrada |
    |                                      | Configuração do Computador\Políticas\Configurações do Windows\Modelos Administrativos\Rede\Conexão de Rede\Firewall do Windows\Perfil Padrão\ Firewall do Windows | Permitir exceções de área de trabalho remota de entrada |
    | Habilitar o ICMP-V4                       | Configuração do Computador\Políticas\Configurações do Windows\Modelos Administrativos\Rede\Conexão de Rede\Firewall do Windows\Perfil de Domínio\Firewall do Windows   | Permitir exceções do ICMP                   |
    |                                      | Configuração do Computador\Políticas\Configurações do Windows\Modelos Administrativos\Rede\Conexão de Rede\Firewall do Windows\Perfil Padrão\ Firewall do Windows | Permitir exceções do ICMP                   |

## <a name="verify-the-vm"></a>Verificar a VM 

Verifique se a VM está íntegro e seguro e RDP acessível: 

1. Para verificar se que o disco está íntegro e consistente, verifique o disco na próxima reinicialização da VM:

    ```PowerShell
    Chkdsk /f
    ```
    Verifique se que o relatório mostra um disco íntegro e limpo.

2. Defina as configurações de BCD (Dados de Configuração da Inicialização). 

    > [!NOTE]
    > Use uma janela elevada do PowerShell para executar esses comandos.
   
   ```powershell
    cmd

    bcdedit /set {bootmgr} integrityservices enable
    bcdedit /set {default} device partition=C:
    bcdedit /set {default} integrityservices enable
    bcdedit /set {default} recoveryenabled Off
    bcdedit /set {default} osdevice partition=C:
    bcdedit /set {default} bootstatuspolicy IgnoreAllFailures

    #Enable Serial Console Feature
    bcdedit /set {bootmgr} displaybootmenu yes
    bcdedit /set {bootmgr} timeout 5
    bcdedit /set {bootmgr} bootems yes
    bcdedit /ems {current} ON
    bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200

    exit
   ```
3. O log de despejo pode ser útil para solucionar problemas de falha do Windows. Habilite a coleta de log de despejo:

    ```powershell
    # Set up the guest OS to collect a kernel dump on an OS crash event
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name CrashDumpEnabled -Type DWord -force -Value 2
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name DumpFile -Type ExpandString -force -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name NMICrashDump -Type DWord -force -Value 1

    # Set up the guest OS to collect user mode dumps on a service crash event
    $key = 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps'
    if ((Test-Path -Path $key) -eq $false) {(New-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting' -Name LocalDumps)}
    New-ItemProperty -Path $key -name DumpFolder -Type ExpandString -force -Value "c:\CrashDumps"
    New-ItemProperty -Path $key -name CrashCount -Type DWord -force -Value 10
    New-ItemProperty -Path $key -name DumpType -Type DWord -force -Value 2
    Set-Service -Name WerSvc -StartupType Manual
    ```
4. Verifique se o repositório do Windows Management Instrumentation (WMI) é consistente:

    ```PowerShell
    winmgmt /verifyrepository
    ```
    Se o repositório estiver corrompido, consulte [WMI: O repositório está corrompido ou não](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

5. Verifique se que nenhum outro aplicativo está usando a porta 3389. Esta porta é usada para o serviço de RDP no Azure. Para ver quais portas são usadas na VM, execute `netstat -anob`:

    ```PowerShell
    netstat -anob
    ```

6. Para carregar um VHD do Windows que seja um controlador de domínio:

   * Siga [estas etapas extras](https://support.microsoft.com/kb/2904015) para preparar o disco.

   * Verifique se que você souber a senha do modo de restauração dos serviços de diretório (DSRM), caso você precise iniciar a VM no DSRM em algum momento. Para obter mais informações, consulte [definir uma senha DSRM](https://technet.microsoft.com/library/cc754363(v=ws.11).aspx).

7. Verifique se que você sabe a conta de administrador interna e a senha. Você talvez queira redefinir a senha de administrador local atual e verifique se que você pode usar essa conta para entrar no Windows por meio de conexão de RDP. Essa permissão de acesso é controlada pelo objeto de política de grupo "Permitir logon pelos serviços de área de trabalho remota". Exiba esse objeto no grupo de Editor de diretiva Local aqui:

    Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment

8. Verifique as seguintes políticas de AD do Azure para garantir que você não está bloqueando o acesso de RDP por meio de RDP ou da rede:

    - Configuração do Computador\Configurações do Windows\Configurações de Segurança\Políticas Locais\Atribuição de direitos de usuário\Negar acesso a este computador pela rede

    - Configuração do Computador\Configurações do Windows\Configurações de Segurança\Políticas Locais\Atribuição de direitos de usuário\Negar logon pelos Serviços de Área de Trabalho Remota


9. Verifique a seguinte política do Azure AD para se certificar de que você não remove qualquer uma das contas de acesso necessário:

   - Computador Configuration\Windows Settings\Security locais \ Atribuição direitos Assignment\Access este computador pela rede

   A política deve listar os seguintes grupos:

   - Administradores

   - Operadores de Backup

   - Todos

   - Usuários

10. Reinicie a VM para certificar-se de que o Windows ainda está íntegro e podem ser contatado por meio de conexão de RDP. Neste ponto, você talvez queira criar uma VM em seu local do Hyper-V para garantir que a VM iniciar completamente. Em seguida, teste para certificar-se de que você pode acessar a VM via RDP.

11. Remova os filtros de Interface de Driver de transporte (TDI) extra. Por exemplo, remover o software que analisa TCP pacotes ou firewalls extras. Se você precisar examinar isso mais tarde, você pode fazer isso depois que a VM é implantada no Azure.

12. Desinstale qualquer software de terceiros ou driver de componentes relacionados físicos ou qualquer outra tecnologia de virtualização.

### <a name="install-windows-updates"></a>Instalar atualizações do Windows
Idealmente, você deve manter a máquina atualizada na *nível do patch*. Se isso não for possível, verifique se que as seguintes atualizações são instaladas:

| Componente               | Binary         | Windows 7 SP1, Windows Server 2008 R2 SP1 | Windows 8, Windows Server 2012               | Windows 8.1, Windows Server 2012 R2 | Windows 10 v1607, Windows Server 2016 v1607 | Windows 10 v1703    | Windows 10 v1709, Windows Server 2016 v1709 | Windows 10 v1803, Windows Server 2016 v1803 |
|-------------------------|----------------|-------------------------------------------|---------------------------------------------|------------------------------------|---------------------------------------------------------|----------------------------|-------------------------------------------------|-------------------------------------------------|
| Armazenamento                 | disk.sys       | 6.1.7601.23403 – KB3125574                | 6.2.9200.17638/6.2.9200.21757 – KB3137061 | 6.3.9600.18203 – KB3137061         | -                                                       | -                          | -                                               | -                                               |
|                         | storport.sys   | 6.1.7601.23403 – KB3125574                | 6.2.9200.17188/6.2.9200.21306 – KB3018489 | 6.3.9600.18573 – KB4022726         | 10.0.14393.1358 – KB4022715                             | 10.0.15063.332             | -                                               | -                                               |
|                         | ntfs.sys       | 6.1.7601.23403 – KB3125574                | 6.2.9200.17623/6.2.9200.21743 – KB3121255 | 6.3.9600.18654 – KB4022726         | 10.0.14393.1198 – KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | Iologmsg.dll   | 6.1.7601.23403 – KB3125574                | 6.2.9200.16384 – KB2995387                  | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | Classpnp.sys   | 6.1.7601.23403 – KB3125574                | 6.2.9200.17061/6.2.9200.21180 – KB2995387 | 6.3.9600.18334 – KB3172614         | 10.0.14393.953 – KB4022715                              | -                          | -                                               | -                                               |
|                         | Volsnap.sys    | 6.1.7601.23403 – KB3125574                | 6.2.9200.17047/6.2.9200.21165 – KB2975331 | 6.3.9600.18265 – KB3145384         | -                                                       | 10.0.15063.0               | -                                               | -                                               |
|                         | partmgr.sys    | 6.1.7601.23403 – KB3125574                | 6.2.9200.16681 – KB2877114                  | 6.3.9600.17401 – KB3000850         | 10.0.14393.953 – KB4022715                              | 10.0.15063.0               | -                                               | -                                               |
|                         | volmgr.sys     |                                           |                                             |                                    |                                                         | 10.0.15063.0               | -                                               | -                                               |
|                         | Volmgrx.sys    | 6.1.7601.23403 – KB3125574                | -                                           | -                                  | -                                                       | 10.0.15063.0               | -                                               | -                                               |
|                         | Msiscsi.sys    | 6.1.7601.23403 – KB3125574                | 6.2.9200.21006 – KB2955163                  | 6.3.9600.18624 – KB4022726         | 10.0.14393.1066 – KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | Msdsm.sys      | 6.1.7601.23403 – KB3125574                | 6.2.9200.21474 – KB3046101                  | 6.3.9600.18592 – KB4022726         | -                                                       | -                          | -                                               | -                                               |
|                         | Mpio.sys       | 6.1.7601.23403 – KB3125574                | 6.2.9200.21190 – KB3046101                  | 6.3.9600.18616 – KB4022726         | 10.0.14393.1198 – KB4022715                             | -                          | -                                               | -                                               |
|                         | vmstorfl.sys   | 6.3.9600.18907 - KB4072650                | 6.3.9600.18080 - KB3063109                  | 6.3.9600.18907 - KB4072650         | 10.0.14393.2007 - KB4345418                             | 10.0.15063.850 - KB4345419 | 10.0.16299.371 - KB4345420                      | -                                               |
|                         | Fveapi.dll     | 6.1.7601.23311 – KB3125574                | 6.2.9200.20930 – KB2930244                  | 6.3.9600.18294 – KB3172614         | 10.0.14393.576 – KB4022715                              | -                          | -                                               | -                                               |
|                         | Fveapibase.dll | 6.1.7601.23403 – KB3125574                | 6.2.9200.20930 – KB2930244                  | 6.3.9600.17415 – KB3172614         | 10.0.14393.206 – KB4022715                              | -                          | -                                               | -                                               |
| Rede                 | netvsc.sys     | -                                         | -                                           | -                                  | 10.0.14393.1198 – KB4022715                             | 10.0.15063.250 – KB4020001 | -                                               | -                                               |
|                         | mrxsmb10.sys   | 6.1.7601.23816 – KB4022722                | 6.2.9200.22108 – KB4022724                  | 6.3.9600.18603 – KB4022726         | 10.0.14393.479 – KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | mrxsmb20.sys   | 6.1.7601.23816 – KB4022722                | 6.2.9200.21548 – KB4022724                  | 6.3.9600.18586 – KB4022726         | 10.0.14393.953 – KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | mrxsmb.sys     | 6.1.7601.23816 – KB4022722                | 6.2.9200.22074 – KB4022724                  | 6.3.9600.18586 – KB4022726         | 10.0.14393.953 – KB4022715                              | 10.0.15063.0               | -                                               | -                                               |
|                         | tcpip.sys      | 6.1.7601.23761 – KB4022722                | 6.2.9200.22070 – KB4022724                  | 6.3.9600.18478 – KB4022726         | 10.0.14393.1358 – KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | http.sys       | 6.1.7601.23403 – KB3125574                | 6.2.9200.17285 – KB3042553                  | 6.3.9600.18574 – KB4022726         | 10.0.14393.251 – KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | vmswitch.sys   | 6.1.7601.23727 – KB4022719                | 6.2.9200.22117 – KB4022724                  | 6.3.9600.18654 – KB4022726         | 10.0.14393.1358 – KB4022715                             | 10.0.15063.138             | -                                               | -                                               |
| Core                    | ntoskrnl.exe   | 6.1.7601.23807 – KB4022719                | 6.2.9200.22170 – KB4022718                  | 6.3.9600.18696 – KB4022726         | 10.0.14393.1358 – KB4022715                             | 10.0.15063.483             | -                                               | -                                               |
| Serviços da Área de Trabalho Remota | rdpcorets.dll  | 6.2.9200.21506 – KB4022719                | 6.2.9200.22104 – KB4022724                  | 6.3.9600.18619 – KB4022726         | 10.0.14393.1198 – KB4022715                             | 10.0.15063.0               | -                                               | -                                               |
|                         | termsrv.dll    | 6.1.7601.23403 – KB3125574                | 6.2.9200.17048 – KB2973501                  | 6.3.9600.17415 – KB3000850         | 10.0.14393.0 – KB4022715                                | 10.0.15063.0               | -                                               | -                                               |
|                         | termdd.sys     | 6.1.7601.23403 – KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | win32k.sys     | 6.1.7601.23807 – KB4022719                | 6.2.9200.22168 – KB4022718                  | 6.3.9600.18698 – KB4022726         | 10.0.14393.594 – KB4022715                              | -                          | -                                               | -                                               |
|                         | rdpdd.dll      | 6.1.7601.23403 – KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | rdpwd.sys      | 6.1.7601.23403 – KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
| Segurança                | MS17-010       | KB4012212                                 | KB4012213                                   | KB4012213                          | KB4012606                                               | KB4012606                  | -                                               | -                                               |
|                         |                |                                           | KB4012216                                   |                                    | KB4013198                                               | KB4013198                  | -                                               | -                                               |
|                         |                | KB4012215                                 | KB4012214                                   | KB4012216                          | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         |                |                                           | KB4012217                                   |                                    | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         | CVE-2018-0886  | KB4103718               | KB4103730                | KB4103725       | KB4103723                                               | KB4103731                  | KB4103727                                       | KB4103721                                       |
|                         |                | KB4103712          | KB4103726          | KB4103715|                                                         |                            |                                                 |                                                 |
       
### Determinar quando usar o Sysprep <a id="step23"></a>    

Ferramenta de preparação do sistema (Sysprep) é um processo que você pode executar para redefinir uma instalação do Windows. Sysprep fornece uma experiência "pronta", removendo todos os dados pessoais e redefinição de vários componentes. 

Normalmente, você executar o Sysprep para criar um modelo do qual você pode implantar várias outras VMs que têm uma configuração específica. O modelo é chamado um *imagem generalizada*.

Se você quiser criar apenas uma VM a partir de um disco, você não precisa usar o Sysprep. Em vez disso, você pode criar a VM de um *imagem especializada*. Para obter informações sobre como criar uma máquina virtual de um disco especializado, consulte:

- [Criar uma VM com base em um disco especializado](create-vm-specialized.md)
- [Criar uma VM com base em um disco VHD](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal?branch=master)

Se você quiser criar uma imagem generalizada, você precisa executar o Sysprep. Para obter mais informações, consulte [como usar Sysprep: Uma introdução](https://technet.microsoft.com/library/bb457073.aspx). 

Nem toda função ou um aplicativo que é instalado em um computador baseado em Windows dá suporte a imagens generalizadas. Portanto, antes de executar este procedimento, verifique se o que Sysprep oferece suporte à função do computador. Para obter mais informações, confira [Sysprep support for server role](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles) (Suporte do Sysprep para funções de servidor).

### <a name="generalize-a-vhd"></a>Generalizar um VHD

>[!NOTE]
> Depois de executar `sysprep.exe` nas etapas a seguir, desligue a VM. Não ativá-lo novamente até que você crie uma imagem no Azure.

1. Entre na VM Windows.
1. Execute o **Prompt de Comando** como administrador. 
1. Altere o diretório para `%windir%\system32\sysprep`. Em seguida, execute `sysprep.exe`.
1. Na caixa de diálogo **Ferramenta de Preparação do Sistema**, selecione **Entrar na Configuração Inicial pelo Usuário do Sistema (OOBE)** e verifique se a caixa de seleção **Generalizar** está marcada.

    ![Ferramenta de Preparação do Sistema](media/prepare-for-upload-vhd-image/syspre.png)
1. Em **Opções de Desligamento**, selecione **Desligar**.
1. Selecione **OK**.
1. Quando a conclusão do Sysprep, desligue a VM. Não use **reiniciar** para desligar a VM.

Agora o VHD está pronto para ser carregado. Para obter mais informações sobre como criar uma VM de um disco generalizado, consulte [carregar um VHD generalizado e usá-lo para criar uma nova VM no Azure](sa-upload-generalized.md).


>[!NOTE]
> Um personalizado *Unattend. XML* não há suporte para o arquivo. Embora há suporte para o `additionalUnattendContent` propriedade, que fornece apenas suporte limitado para adicionar [microsoft-windows-shell-setup](https://docs.microsoft.com/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup) opções para o *Unattend. XML* do arquivo que o provisionamento do Azure o Agent usa. Você pode usar, por exemplo, [additionalUnattendContent](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.compute.models.additionalunattendcontent?view=azure-dotnet) adicionar FirstLogonCommands e LogonCommands. Para obter mais informações, consulte [additionalUnattendContent FirstLogonCommands exemplo](https://github.com/Azure/azure-quickstart-templates/issues/1407).


## <a name="complete-the-recommended-configurations"></a>Conclua as configurações recomendadas
As seguintes configurações não afetam o carregamento do VHD. No entanto, é altamente recomendável que você as configure.

* Instalar o [agente de máquina Virtual do Azure](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Em seguida, você pode habilitar as extensões de VM. As extensões de VM implementam a maioria dos recursos essenciais que você talvez queira usar com suas VMs. Você precisará as extensões, por exemplo, redefinir senhas ou configurar o RDP. Para obter mais informações, consulte [visão geral do agente de máquina Virtual do Azure](../extensions/agent-windows.md).
* Depois de criar a VM no Azure, é recomendável que você coloque o arquivo de paginação na *volume da unidade temporal* para melhorar o desempenho. Você pode configurar o posicionamento de arquivo da seguinte maneira:

   ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management' -name "PagingFiles" -Value "D:\pagefile.sys" -Type MultiString -force
   ```
  Se um disco de dados é anexado à VM, a letra do volume da unidade temporal é normalmente *1!d*. Essa designação poderia ser diferente, dependendo das suas configurações e o número de unidades disponíveis.

## <a name="next-steps"></a>Próximas etapas
* [Carregar uma imagem de VM Windows no Azure para implantações do Resource Manager](upload-generalized-managed.md)
* [Solucionar problemas de ativação de VM do Windows Azure](troubleshoot-activation-problems.md)

