---
title: Sobre gateways de rede virtual do ExpressRoute – Azure | Microsoft Docs
description: Saiba mais sobre os gateways de rede virtual para ExpressRoute. Este artigo inclui informações sobre tipos e SKUs de gateway.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: 18615cf737eedcd188fd59d2aa98482210b9333a
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991581"
---
# <a name="expressroute-virtual-network-gateway-and-fastpath"></a>Gateway de rede virtual e FastPath do ExpressRoute
Para conectar sua rede virtual do Azure e sua rede local por meio do ExpressRoute, você deve criar primeiro um gateway de rede virtual. Um gateway de rede virtual tem duas finalidades: rotas IP do exchange entre as redes e rotear o tráfego de rede. Este artigo explica os tipos de gateway, SKUs de gateway e desempenho estimado por SKU. Este artigo também explica o ExpressRoute [FastPath](#fastpath), um recurso que permite o tráfego de rede da sua rede local para ignorar o gateway de rede virtual para melhorar o desempenho.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="gateway-types"></a>Tipos de gateway

Quando você cria um gateway de rede virtual, precisa especificar várias configurações. Uma das configurações necessárias, '-GatewayType', especifica se o gateway é usado para tráfego de VPN ou ExpressRoute. Os dois tipos de gateway são:

* **Vpn** - Para enviar tráfego criptografado pela Internet pública, use o tipo de gateway 'Vpn'. Isso também é chamado de gateway de VPN. As conexões Site a Site, Ponto a Site e VNet a VNet usam um gateway VPN.

* **ExpressRoute** - Para enviar tráfego em uma conexão privada, use o tipo de gateway 'ExpressRoute'. Isso também é chamado de gateway ExpressRoute e é o tipo de gateway usado na configuração do ExpressRoute.

Cada rede virtual pode ter apenas um gateway de rede virtual por tipo de gateway. Por exemplo, você pode ter um gateway de rede virtual que usa - GatewayType Vpn, e outro que usa -GatewayType ExpressRoute.

## <a name="gwsku"></a>SKUs do Gateway
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Se você quiser atualizar o gateway para um SKU de gateway mais potente, na maioria dos casos, você pode usar o cmdlet do PowerShell 'Resize-AzVirtualNetworkGateway'. Isso funcionará em atualizações para as SKUs Standard e HighPerformance. No entanto, para atualizar para a SKU UltraPerformance, você precisará recriar o gateway. Recriar um gateway incorre em tempo de inatividade.

### <a name="aggthroughput"></a>Desempenhos estimados por SKU de gateway
A tabela a seguir mostra os tipos de gateway e os desempenhos estimados. Esta tabela aplica-se a ambos os modelos de implantação do Gerenciador de Recursos e clássico.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> O desempenho do aplicativo depende de vários fatores, como a latência de ponta a ponta e o número de fluxos de tráfego abertos pelo aplicativo. Os números na tabela representam o limite superior que o aplicativo, teoricamente, pode atingir em um ambiente ideal.
>
>

### <a name="zrgw"></a>SKUs de gateway redundantes de zona

Também é possível implantar gateways do ExpressRoute em Zonas de Disponibilidade do Azure. Isso separa fisicamente e logicamente em Zonas de disponibilidade diferentes, protegendo a conectividade de rede local com o Azure de falhas de nível de zona.

![Gateway do ExpressRoute com redundância de zona](./media/expressroute-about-virtual-network-gateways/zone-redundant.png)

Gateways com redundância de zona usam novas SKUs de gateways específicas para gateway do ExpressRoute.

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

As novas SKUs de gateway também dão suporte a outras opções de implantação para melhor atender às suas necessidades. Ao criar um gateway de rede virtual usando as novas SKUs de gateway, você também terá a opção de implantar o gateway em uma zona específica. Isso é referido como um gateway de VPN. Ao implantar um gateway em zona, todas as instâncias do gateway são implantadas na mesma zona de disponibilidade.

## <a name="fastpath"></a>FastPath
Gateway de rede virtual do ExpressRoute foi projetado para troca de rotas de rede e rotear o tráfego de rede. FastPath foi projetado para melhorar o desempenho do caminho de dados entre sua rede local e sua rede virtual. Quando habilitado, FastPath envia o tráfego de rede diretamente às máquinas virtuais na rede virtual, ignorando o gateway. 

FastPath está disponível no [diretos do ExpressRoute](expressroute-erdirect-about.md) somente. Em outras palavras, você pode permitir que somente-se neste recurso você [conectar sua rede virtual](expressroute-howto-linkvnet-arm.md) a um circuito de ExpressRoute criado em uma porta direta de ExpressRoute. FastPath ainda requer um gateway de rede virtual a ser criado para troca de rotas entre a rede virtual e rede local. O gateway de rede virtual deve ser ultradesempenho ou ErGw3AZ.

FastPath não oferece suporte para os seguintes recursos:
* UDR na sub-rede do Gateway: se você aplicar uma UDR à sub-rede de Gateway da sua rede virtual, o tráfego de rede da sua rede local continuará a ser enviada ao gateway de rede virtual.
* Emparelhamento de rede virtual: se você tiver outros redes virtuais emparelhadas com aquele que está conectado ao ExpressRoute, o tráfego de rede de sua rede local para as outras redes virtuais (ou seja, o chamado "Spoke") continuarão a ser enviado para a rede virtual gateway. A solução alternativa é conectar todas as redes virtuais ao circuito do ExpressRoute diretamente.

## <a name="resources"></a>Cmdlets do PowerShell e APIs REST
Para obter recursos técnicos adicionais e requisitos de sintaxe específicos ao usar cmdlets do PowerShell e APIs REST para configurações do gateway de rede virtual, veja as páginas a seguir:

| **Clássico** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#azure) |[PowerShell](https://docs.microsoft.com/powershell/module/az.network#networking) |
| [API REST](https://msdn.microsoft.com/library/jj154113.aspx) |[API REST](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Próximas etapas
Consulte [Visão geral de ExpressRoute](expressroute-introduction.md) para saber mais sobre as configurações de conexão disponíveis.

Veja [Como criar um gateway de rede virtual para ExpressRoute](expressroute-howto-add-gateway-resource-manager.md) para obter mais informações sobre a criação de gateways do ExpressRoute.

Ver [criar um gateway de rede virtual com redundância de zona](../../articles/vpn-gateway/create-zone-redundant-vnet-gateway.md) para obter mais informações sobre como configurar gateways com redundância de zona.

Ver [Link de rede virtual para ExpressRoute](expressroute-howto-linkvnet-arm.md) para obter mais informações sobre como habilitar FastPath. 
