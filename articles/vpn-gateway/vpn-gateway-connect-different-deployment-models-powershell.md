---
title: "Conectar redes virtuais clássicas a VNETs do Azure Resource Manager: PowerShell | Microsoft Docs"
description: "Saiba como criar uma conexão de VPN entre redes virtuais clássicas e redes virtuais do Resource Manager usando o Gateway de VPN e o PowerShell"
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: f17c3bf0-5cc9-4629-9928-1b72d0c9340b
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/12/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 10985eaebd9148ef4fbd22eac0ba50076ca71ef5
ms.openlocfilehash: 430d2c02537e6907da09926db52a3d07b975bb64


---
# <a name="connect-virtual-networks-from-different-deployment-models-using-powershell"></a>Conectar redes virtuais de diferentes modelos de implantação usando o PowerShell
> [!div class="op_single_selector"]
> * [Portal](vpn-gateway-connect-different-deployment-models-portal.md)
> * [PowerShell](vpn-gateway-connect-different-deployment-models-powershell.md)
> 
> 

Atualmente, o Azure tem dois modelos de gerenciamento: o clássico e o RM (Resource Manager). Se você já usa o Azure há algum tempo, provavelmente terá as VMs do Azure e as funções de instância em execução em uma Rede Virtual clássica. Suas VMs e instâncias de função mais recentes podem estar em execução em uma Rede Virtual criada no Resource Manager. Este artigo explica como conectar redes virtuais clássicas a redes virtuais do Resource Manager para permitir que os recursos localizados nos modelos de implantação separados se comuniquem entre si usando uma conexão de gateway.

Você pode criar uma conexão entre redes virtuais em assinaturas e regiões diferentes. Você também pode conectar redes virtuais que já têm conexões com redes locais, desde que o gateway com o qual foram configuradas seja dinâmico ou baseado em rota. Para saber mais sobre conexões de Rede Virtual a Rede Virtual, consulte as [Perguntas frequentes sobre Rede Virtual para Rede Virtual](#faq) no final deste artigo.

### <a name="deployment-models-and-methods-for-connecting-vnets-in-different-deployment-models"></a>Modelos de implantação e métodos para se conectar a redes virtuais em diferentes modelos de implantação
[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

Atualizamos a tabela a seguir conforme os novos artigos, e ferramentas adicionais ficam disponíveis para esta configuração. Quando um artigo estiver disponível, o vincularemos diretamente da tabela.<br><br>

[!INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

**Emparelhamento VNet**

[!INCLUDE [vpn-gateway-vnetpeeringlink](../../includes/vpn-gateway-vnetpeeringlink-include.md)]

## <a name="before-beginning"></a>Antes de começar
As etapas a seguir mostrarão as configurações necessárias para configurar um gateway dinâmico ou baseado em rota para cada Rede Virtual, e para criar uma conexão VPN entre os gateways. Essa configuração não dá suporte a gateways estáticos ou baseados em política.

### <a name="prerequisites"></a>Pré-requisitos
* Ambas as redes virtuais já foram criadas.
* Os intervalos de endereços das redes virtuais não se sobrepõem entre eles ou aos intervalos de outras conexões às quais os gateways podem estar conectados.
* Você instalou os cmdlets do PowerShell mais recentes (1.0.2 ou mais recente). Consulte [Como instalar e configurar o Azure PowerShell](/powershell/azureps-cmdlets-docs) para obter mais informações. Instale os dois cmdlets do Resource Manager (RM) e do Gerenciamento de Serviços (SM). 

### <a name="a-nameexamplerefaexample-settings"></a><a name="exampleref"></a>Configurações de exemplo
Você pode usar as configurações de exemplo como referência.

**Configurações da Rede Virtual clássica**

Nome da rede virtual = ClassicVNet  <br>
Local = Oeste dos EUA <br>
Espaços de Endereço da Rede Virtual = 10.0.0.0/24 <br>
Subnet-1 = 10.0.0.0/27 <br>
GatewaySubnet = 10.0.0.32/29 <br>
Nome da rede local = RMVNetLocal <br>
GatewayType = DynamicRouting

**Configurações de Rede Virtual do Resource Manager**

Nome da VNet = RMVNet  <br>
Grupo de recursos = RG1 <br>
Espaços de Endereço do IP da Rede Virtual = 192.168.0.0/16 <br>
Subnet-1 = 192.168.1.0/24 <br>
GatewaySubnet = 192.168.0.0/26 <br>
Local = Leste dos EUA <br>
Nome do IP Público do Gateway = gwpip <br>
Gateway de Rede Local = ClassicVNetLocal <br>
Nome do Gateway de Rede Virtual = RMGateway <br>
Configuração de endereçamento IP do gateway = gwipconfig

## <a name="a-namecreatesmgwasection-1---configure-the-classic-vnet"></a><a name="createsmgw"></a>Seção 1 - Configurar a rede virtual clássica
### <a name="part-1---download-your-network-configuration-file"></a>Parte 1 - Baixar o arquivo de configuração de rede
1. Faça logon na sua conta do Azure no console do PowerShell com direitos elevados. O cmdlet a seguir solicita as credenciais de logon para sua conta do Azure. Depois de entrar, ele baixa as configurações da conta para que elas estejam disponíveis para o Azure PowerShell. Você usará os cmdlets do SM do PowerShell para concluir essa parte da configuração.
   
        Add-AzureAccount
2. Baixe o arquivo de configuração de rede do Azure executando o comando a seguir. Você pode alterar o local do arquivo a ser exportado para um local diferente, se necessário. Edite o arquivo e importe-o para o Azure.
   
        Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
3. Abra o arquivo .xml baixado para editá-lo. Para obter um exemplo de arquivo de configuração de rede, confira o [Esquema de configuração de rede](https://msdn.microsoft.com/library/jj157100.aspx).

### <a name="part-2--verify-the-gateway-subnet"></a>Parte 2 - Verificar a sub-rede do gateway
No elemento **VirtualNetworkSites** , adicione uma sub-rede de gateway à rede virtual se já não existir uma. Ao trabalhar com o arquivo de configuração de rede, a sub-rede do gateway DEVE ser nomeada como "GatewaySubnet", ou o Azure poderá não reconhecê-la e usá-la como uma sub-rede de gateway.

[!INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)]

**Exemplo:**

    <VirtualNetworkSites>
      <VirtualNetworkSite name="ClassicVNet" Location="West US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/24</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Subnet-1">
            <AddressPrefix>10.0.0.0/27</AddressPrefix>
          </Subnet>
          <Subnet name="GatewaySubnet">
            <AddressPrefix>10.0.0.32/29</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>

### <a name="part-3---add-the-local-network-site"></a>Parte 3 – Adicionar o site de rede local
O site de rede local que adicionar representará a rede virtual do RM ao qual você deseja se conectar. Talvez seja necessário adicionar um elemento **LocalNetworkSites** ao arquivo se ele ainda não existir. Nessa parte da configuração, o VPNGatewayAddress pode ser qualquer endereço IP público válido porque ainda não criamos o gateway para a rede virtual do Resource Manager. Depois de criarmos o gateway, poderemos substituir esse endereço IP de espaço reservado pelo endereço IP público correto que foi atribuído ao gateway do RM.

    <LocalNetworkSites>
      <LocalNetworkSite name="RMVNetLocal">
        <AddressSpace>
          <AddressPrefix>192.168.0.0/16</AddressPrefix>
        </AddressSpace>
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
      </LocalNetworkSite>
    </LocalNetworkSites>

### <a name="part-4---associate-the-vnet-with-the-local-network-site"></a>Parte 4 - Associar a rede virtual ao site de rede local
Nesta seção, podemos especificar o local de rede local ao qual você deseja conectar a rede virtual. Nesse caso, é a rede virtual do Resource Manager que foi mencionada anteriormente. Verifique se os nomes correspondem. Esta etapa não cria um gateway. Ela especifica a rede local a que o gateway será conectado.

        <Gateway>
          <ConnectionsToLocalNetwork>
            <LocalNetworkSiteRef name="RMVNetLocal">
              <Connection type="IPsec" />
            </LocalNetworkSiteRef>
          </ConnectionsToLocalNetwork>
        </Gateway>

### <a name="part-5---save-the-file-and-upload"></a>Parte 5 - Salvar o arquivo e carregar
Salve o arquivo e importe-o para o Azure executando o comando a seguir. Altere o caminho do arquivo conforme for necessário para seu ambiente.

        Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml

Você verá algo semelhante a este resultado mostrando que a importação foi bem-sucedida.

        OperationDescription        OperationId                      OperationStatus                                                
        --------------------        -----------                      ---------------                                                
        Set-AzureVNetConfig        e0ee6e66-9167-cfa7-a746-7casb9    Succeeded 

### <a name="part-6---create-the-gateway"></a>Parte 6 – Criar o gateway
Você pode criar o gateway da VNet usando o portal clássico ou usando o PowerShell.

Antes de executar este exemplo, consulte no arquivo de configuração de rede que você baixou os nomes exatos que o Azure espera ver. O arquivo de configuração de rede contém os valores de suas redes virtuais clássicas. Às vezes, os nomes de redes virtuais clássicas são alterados no arquivo de configuração de rede durante a criação de configurações da rede virtual clássicas no portal do Azure devido a diferenças nos modelos de implantação. Por exemplo, se você usou o portal do Azure para criar uma rede virtual clássica denominada 'Rede virtual clássica' e a criou em um grupo de recursos denominado 'ClassicRG', o nome que está contido no arquivo de configuração de rede é convertido em 'Grupo ClassicRG Rede virtual clássica'. Ao especificar o nome de uma rede virtual que contenha espaços, use aspas ao redor do valor.


Use o exemplo a seguir para criar um gateway de roteamento dinâmico:

    New-AzureVNetGateway -VNetName ClassicVNet -GatewayType DynamicRouting

Você pode verificar o status do gateway usando o cmdlet `Get-AzureVNetGateway` .

## <a name="a-namecreatermgwasection-2-configure-the-rm-vnet-gateway"></a><a name="creatermgw"></a>Seção 2: Configurar o gateway de rede virtual do RM
Para criar um gateway de VPN para rede virtual do RM, siga as instruções abaixo. Não comece as etapas a seguir até recuperar o endereço IP público do gateway da rede virtual clássica. 

1. **Faça logon na sua conta do Azure** no console do PowerShell. O cmdlet a seguir solicita as credenciais de logon para sua conta do Azure. Depois de entrar, as configurações da conta são baixadas para que estejam disponíveis para o Azure PowerShell.
   
        Login-AzureRmAccount 
   
     Obtenha uma lista de suas assinaturas do Azure, se você tiver mais de uma assinatura.
   
        Get-AzureRmSubscription
   
    Especifique a assinatura que você quer usar. 
   
        Select-AzureRmSubscription -SubscriptionName "Name of subscription"
2. **Crie um gateway de rede local**. Em uma rede virtual, o gateway de rede local geralmente se refere ao seu local. Nesse caso, o gateway de rede local refere-se à rede virtual clássica. Dê um nome pelo qual o Azure pode fazer referência a ele e especifique também o prefixo de espaço de endereço. O Azure usa o prefixo de endereço IP que você especifica para identificar qual tráfego enviar a seu local. Se você precisar ajustar as informações aqui posteriormente, antes de criar o gateway, poderá modificar os valores e executar o exemplo novamente.<br><br>
   
   * `-Name` é o nome que você deseja atribuir ao se referir ao gateway de rede local.<br>
   * `-AddressPrefix` é o Espaço de Endereço para a rede virtual clássica.<br>
   * `-GatewayIpAddress` é o endereço IP público do gateway da rede virtual clássica. Não deixe de alterar o exemplo a seguir para refletir o endereço IP correto.
     
           New-AzureRmLocalNetworkGateway -Name ClassicVNetLocal `
           -Location "West US" -AddressPrefix "10.0.0.0/24" `
           -GatewayIpAddress "n.n.n.n" -ResourceGroupName RG1
3. **Solicite um endereço IP público** a ser alocado para o seu gateway de rede virtual para a VNet do Resource Manager. Você não pode especificar o endereço IP que deseja usar. O endereço IP é alocado dinamicamente para o gateway de rede virtual. No entanto, isso não significa que o endereço IP será alterado. A única vez em que o endereço IP do gateway de rede virtual é alterado é quando o gateway é excluído e recriado. Ele não será alterado com o redimensionamento, a redefinição ou outra manutenção/atualização interna do gateway.<br>Nesta etapa, também definimos uma variável usada em uma etapa posterior.

        $ipaddress = New-AzureRmPublicIpAddress -Name gwpip `
        -ResourceGroupName RG1 -Location 'EastUS' `
        -AllocationMethod Dynamic

1. **Verifique se sua rede virtual tem uma sub-rede de gateway**. Se não houver sub-rede de gateway, adicione uma. Verifique se a sub-rede de gateway se chama *GatewaySubnet*.
2. **Recupere a sub-rede** usada para o gateway executando o comando a seguir. Nesta etapa, também definimos uma variável a ser usada na próxima etapa.
   
   * `-Name` é o nome de sua rede virtual do Resource Manager.
   * `-ResourceGroupName` é o grupo de recursos ao qual a rede virtual está associada. A sub-rede de gateway já deve existir para essa rede virtual e deve se chamar *GatewaySubnet* para funcionar corretamente.

            $subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name GatewaySubnet `
            -VirtualNetwork (Get-AzureRmVirtualNetwork -Name RMVNet -ResourceGroupName RG1) 

1. **Crie a configuração de endereçamento IP do gateway**. A configuração do gateway define a sub-rede e o endereço IP público a serem usados. Use o exemplo a seguir para criar a configuração do gateway.<br>Nessa etapa, os parâmetros `-SubnetId` e `-PublicIpAddressId` devem receber a propriedade id da sub-rede e os objetos de endereço IP, respectivamente. Você não pode usar uma cadeia de caracteres simples. Essas variáveis são definidas na etapa de solicitação de um IP público e na etapa de recuperação da sub-rede.
   
        $gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig `
        -Name gwipconfig -SubnetId $subnet.id `
        -PublicIpAddressId $ipaddress.id
2. **Crie o gateway de rede virtual do Resource Manager** executando o comando a seguir. O `-VpnType` deve ser *RouteBased*. Pode demorar 45 minutos ou mais para ser concluído.
   
        New-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1 `
        -Location "EastUS" -GatewaySKU Standard -GatewayType Vpn `
        -IpConfigurations $gwipconfig `
        -EnableBgp $false -VpnType RouteBased
3. **Copie o endereço IP público** depois que o gateway de VPN foi criado. Use-o quando você definir as configurações de rede local para sua Rede Virtual clássica. Você pode usar o cmdlet a seguir para recuperar o endereço IP público. O endereço IP público é listado no retorno como *IpAddress*.
   
        Get-AzureRmPublicIpAddress -Name gwpip -ResourceGroupName RG1

## <a name="section-3-modify-the-classic-vnet-local-site-settings"></a>Seção 3: Modificar as configurações de site local de rede virtual clássicas

Nesta seção, você trabalhará com a rede virtual clássica. Você substituirá o endereço IP de espaço reservado pelo usado ao especificar as configurações de site local que serão utilizadas para conectar com gateway da VNET do Gerenciador de Recursos. 

1. Exportar o arquivo de configuração de rede.

        Get-AzureVNetConfig -ExportToFile C:\AzureNet\NetworkConfig.xml
2. Usando um editor de texto, modifique o valor de VPNGatewayAddress. Substitua o endereço IP de espaço reservado com o endereço IP público do gateway do Resource Manager e salve as alterações.
   
        <VPNGatewayAddress>13.68.210.16</VPNGatewayAddress>
3. Importe o arquivo de configuração de rede modificada para o Azure.

        Set-AzureVNetConfig -ConfigurationPath C:\AzureNet\NetworkConfig.xml

## <a name="a-nameconnectasection-4-create-a-connection-between-the-gateways"></a><a name="connect"></a>Seção 4: Criar uma conexão entre os gateways
A criação de uma conexão entre os gateways requer o PowerShell. Pode ser necessário adicionar sua Conta do Azure para usar os cmdlets do PowerShell clássicos. Para fazer isso, use `Add-AzureAccount`.

1. No console do PowerShell, defina a chave compartilhada. Antes de executar os cmdlets, consulte no arquivo de configuração de rede que você baixou os nomes exatos que o Azure espera ver. Ao especificar o nome de uma rede virtual que contenha espaços, use aspas simples ao redor do valor.<br><br>No exemplo a seguir, `-VNetName` é o nome da Rede Virtual clássica e `-LocalNetworkSiteName` é o nome que você especificou para o site da rede local. O `-SharedKey` é um valor que você pode gerar e especificar. Neste exemplo, usamos ‘abc123’, mas você pode gerar e usar algo mais complexo. O importante é que o valor especificado aqui deve ser o mesmo que você especificará na próxima etapa, ao criar sua conexão. O retorno deve mostrar **Status: êxito**. 
   
        Set-AzureVNetGatewayKey -VNetName ClassicVNet `
        -LocalNetworkSiteName RMVNetLocal -SharedKey abc123
2. Crie a conexão VPN executando os comandos a seguir.
   
    **Defina as variáveis**
   
        $vnet01gateway = Get-AzureRMLocalNetworkGateway -Name ClassicVNetLocal -ResourceGroupName RG1
        $vnet02gateway = Get-AzureRmVirtualNetworkGateway -Name RMGateway -ResourceGroupName RG1
   
    **Crie a conexão**<br> Observe que o `-ConnectionType` é IPsec, não Vnet2Vnet.
   
        New-AzureRmVirtualNetworkGatewayConnection -Name RM-Classic -ResourceGroupName RG1 `
        -Location "East US" -VirtualNetworkGateway1 `
        $vnet02gateway -LocalNetworkGateway2 `
        $vnet01gateway -ConnectionType IPsec -RoutingWeight 10 -SharedKey 'abc123'

## <a name="section-5-verify-your-connections"></a>Seção 5: Verificar suas conexões

### <a name="to-verify-the-connection-from-your-classic-vnet-to-your-resource-manager-vnet"></a>Para verificar a conexão de sua VNET clássica para sua VNET do Resource Manager

####<a name="verify-your-connection-using-powershell"></a>Verificar sua conexão usando o PowerShell


[!INCLUDE [vpn-gateway-verify-connection-ps-classic](../../includes/vpn-gateway-verify-connection-ps-classic-include.md)]


####<a name="verify-your-connection-using-the-azure-portal"></a>Verificar sua conexão usando o Portal do Azure

[!INCLUDE [vpn-gateway-verify-connection-azureportal-classic](../../includes/vpn-gateway-verify-connection-azureportal-classic-include.md)]


###<a name="to-verify-the-connection-from-your-resource-manager-vnet-to-your-classic-vnet"></a>Para verificar a conexão de seu VNET do Resource Manager para sua rede virtual clássica

####<a name="verify-your-connection-using-powershell"></a>Verificar sua conexão usando o PowerShell

[!INCLUDE [vpn-gateway-verify-ps-rm](../../includes/vpn-gateway-verify-connection-ps-rm-include.md)]

####<a name="verify-your-connection-using-the-azure-portal"></a>Verificar sua conexão usando o portal do Azure

[!INCLUDE [vpn-gateway-verify-connection-portal-rm](../../includes/vpn-gateway-verify-connection-portal-rm-include.md)]

## <a name="a-namefaqavnet-to-vnet-considerations"></a><a name="faq"></a>Considerações de Rede Virtual para Rede Virtual

[!INCLUDE [vpn-gateway-vnet-vnet-faq](../../includes/vpn-gateway-vnet-vnet-faq-include.md)]




<!--HONumber=Jan17_HO4-->


