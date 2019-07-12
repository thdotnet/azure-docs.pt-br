---
title: Configurar um túnel VPN Always On para o Gateway de VPN
description: Etapas para configurar o túnel de VPN Always On para o Gateway de VPN
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptional
ms.date: 07/09/2019
ms.author: cherylmc
ms.openlocfilehash: 81822297dcf9370fc8ce7f7ce0285689c31606ce
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67695779"
---
# <a name="configure-an-always-on-vpn-device-tunnel"></a>Configurar um túnel de dispositivo VPN Always On

Um dos novos recursos do cliente Windows 10 Virtual VPN (rede privada) é a capacidade de manter uma conexão VPN. Always On é um recurso do Windows 10 que permite que o perfil VPN ativo para se conectar automaticamente e permanecer conectado com base nos gatilhos — ou seja, entrada do usuário, alteração de estado da rede ou Active Directory de tela do dispositivo.

Gateways de rede virtual do Azure podem ser usados com o Windows 10 Always On para estabelecer túneis de usuário persistente, bem como túneis de dispositivo para o Azure. Este artigo ajudará você a configurar um túnel de dispositivo Always ON VPN.

Conexões de VPN Always On incluem dois tipos de túneis:

* **Túnel de dispositivo** conecta-se aos servidores VPN de especificado antes dos usuários entram no dispositivo. Cenários de conectividade de pré-logon e fins de gerenciamento de dispositivo usam túnel do dispositivo.

* **Túnel do usuário** conecta-se somente após uma entrada do usuário do dispositivo. Túnel do usuário permite que os usuários acessem recursos da organização por meio de servidores VPN.

Túnel de dispositivo e o túnel do usuário operam de forma independente com seus perfis VPN. Eles podem ser conectados ao mesmo tempo e podem usar diferentes métodos de autenticação e outras definições de configuração de VPN conforme apropriado.

## <a name="1-configure-the-gateway"></a>1. Configurar o gateway

Configurar o gateway VPN para usar IKEv2 e autenticação baseada em certificado usando esse [artigo point-to-site](vpn-gateway-howto-point-to-site-resource-manager-portal.md).

## <a name="2-configure-the-user-tunnel"></a>2. Configurar o túnel do usuário

1. Instalar certificados de cliente no cliente Windows 10, conforme mostrado nesta [artigo de cliente VPN ponto a site](point-to-site-how-to-vpn-client-install-azure-cert.md). O certificado precisa estar no atual usuário Store
2. Configurar o cliente VPN Always On, por meio do PowerShell, o SCCM ou do Intune usando [estas instruções](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections).

## <a name="3-configure-the-device-tunnel"></a>3. Configurar o túnel de dispositivo

Os seguintes requisitos devem ser atendidos para com sucesso estabelecer um túnel do dispositivo:

* O dispositivo deve ser um computador de ingressado no domínio executando o Windows 10 Enterprise ou educação versão 1709 ou posterior.
* O túnel só é configurável para a solução VPN interna do Windows e é estabelecido usando IKEv2 com a autenticação de certificado do computador. 
* Túnel de apenas um dispositivo pode ser configurado por dispositivo.

1. Instalar certificados de cliente no cliente Windows 10, conforme mostrado nesta [artigo de cliente VPN ponto a site](point-to-site-how-to-vpn-client-install-azure-cert.md). O certificado precisa estar no repositório do computador Local.
1. Use [estas instruções](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-device-tunnel-config#vpn-device-tunnel-configuration) para criar um perfil de VPN e configurar o túnel de dispositivo no contexto da conta sistema LOCAL.

### <a name="configuration-example-for-device-tunnel"></a>Exemplo de configuração de túnel do dispositivo

Depois que você tiver configurado o gateway de rede virtual e instalado o certificado do cliente no repositório do computador Local no cliente Windows 10, use os exemplos a seguir para configurar um túnel de dispositivo do cliente.

1. Copie o seguinte texto e salve-o como ***devicecert.ps1***.

   ```
   Param(
   [string]$xmlFilePath,
   [string]$ProfileName
   )

   $a = Test-Path $xmlFilePath
   echo $a

   $ProfileXML = Get-Content $xmlFilePath

   echo $XML

   $ProfileNameEscaped = $ProfileName -replace ' ', '%20'

   $Version = 201606090004

   $ProfileXML = $ProfileXML -replace '<', '&lt;'
   $ProfileXML = $ProfileXML -replace '>', '&gt;'
   $ProfileXML = $ProfileXML -replace '"', '&quot;'

   $nodeCSPURI = './Vendor/MSFT/VPNv2'
   $namespaceName = "root\cimv2\mdm\dmmap"
   $className = "MDM_VPNv2_01"

   $session = New-CimSession

   try
   {
   $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", 'String', 'Key')
   $newInstance.CimInstanceProperties.Add($property)
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", 'String', 'Key')
   $newInstance.CimInstanceProperties.Add($property)
   $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", 'String', 'Property')
   $newInstance.CimInstanceProperties.Add($property)

   $session.CreateInstance($namespaceName, $newInstance)
   $Message = "Created $ProfileName profile."
   Write-Host "$Message"
   }
   catch [Exception]
   {
   $Message = "Unable to create $ProfileName profile: $_"
   Write-Host "$Message"
   exit
   }
   $Message = "Complete."
   Write-Host "$Message"
   ```
1. Copie o seguinte texto e salve-o como ***VPNProfile.xml*** na mesma pasta que **devicecert.ps1**. Edite o texto a seguir para corresponder ao seu ambiente.

   * `<Servers>azuregateway-1234-56-78dc.cloudapp.net</Servers>`
   * `<Address>192.168.3.5</Address>`
   * `<Address>192.168.3.4</Address>`

   ```
   <VPNProfile>  
     <NativeProfile>  
   <Servers>azuregateway-1234-56-78dc.cloudapp.net</Servers>  
   <NativeProtocolType>IKEv2</NativeProtocolType>  
   <Authentication>  
     <MachineMethod>Certificate</MachineMethod>  
   </Authentication>  
   <RoutingPolicyType>SplitTunnel</RoutingPolicyType>  
    <!-- disable the addition of a class based route for the assigned IP address on the VPN interface -->
   <DisableClassBasedDefaultRoute>true</DisableClassBasedDefaultRoute>  
     </NativeProfile> 
     <!-- use host routes(/32) to prevent routing conflicts -->  
     <Route>  
   <Address>192.168.3.5</Address>  
   <PrefixSize>32</PrefixSize>  
     </Route>  
     <Route>  
   <Address>192.168.3.4</Address>  
   <PrefixSize>32</PrefixSize>  
     </Route>  
   <!-- need to specify always on = true --> 
     <AlwaysOn>true</AlwaysOn> 
   <!-- new node to specify that this is a device tunnel -->  
    <DeviceTunnel>true</DeviceTunnel>
   <!--new node to register client IP address in DNS to enable manage out -->
   <RegisterDNS>true</RegisterDNS>
   </VPNProfile>
   ```
1. Baixe **PsExec** partir [Sysinternals](https://docs.microsoft.com/sysinternals/downloads/psexec) e extraia os arquivos a serem **C:\PSTools**.
1. Em um prompt CMD administrador, inicie o PowerShell, executando:

   ```
   C:\PsTools\PsExec.exe Powershell for 32-bit Windows
   C:\PsTools\PsExec64.exe Powershell for 64-bit Windows
   ```

   ![PowerShell](./media/vpn-gateway-howto-always-on-device-tunnel/powershell.png)
1. No PowerShell, alterne para a pasta em que **devicecert.ps1** e **VPNProfile.xml** estão localizados e execute o seguinte comando:

   ```powershell
   C:\> .\devicecert.ps1 .\VPNProfile.xml MachineCertTest
   ```
   
   ![MachineCertTest](./media/vpn-gateway-howto-always-on-device-tunnel/machinecerttest.png)
1. Execute **rasphone**.

   ![Rasphone](./media/vpn-gateway-howto-always-on-device-tunnel/rasphone.png)
1. Procure os **MachineCertTest** entrada e clique em **Connect**.

   ![Connect](./media/vpn-gateway-howto-always-on-device-tunnel/connect.png)
1. Se a conexão for bem-sucedida, reinicialize o computador. O túnel se conectará automaticamente.

## <a name="cleanup"></a>Limpeza

Para remover o perfil, execute o seguinte comando:

![Limpeza](./media/vpn-gateway-howto-always-on-device-tunnel/cleanup.png)

## <a name="next-steps"></a>Próximas etapas

Para solucionar o problema, consulte [problemas de conexão de ponto a site do Azure](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)
