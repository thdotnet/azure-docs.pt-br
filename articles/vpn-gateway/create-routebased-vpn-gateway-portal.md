---
title: 'Criar um gateway de VPN baseado em rota: Portal do Azure | Microsoft Docs'
description: Criar um Gateway de VPN baseado em rota usando o portal do Azure
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 08/02/2019
ms.author: cherylmc
ms.openlocfilehash: 2a04c0fa2d92514103377c2aef420290d1bdd057
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68781179"
---
# <a name="create-a-route-based-vpn-gateway-using-the-azure-portal"></a>Criar um gateway de VPN baseado em rotas usando o portal do Azure

Este artigo ajuda você a criar rapidamente um gateway de VPN do Azure baseadas em rota usando o portal do Azure.  Um gateway de VPN é usado ao criar uma conexão VPN à rede local. Você também pode usar um gateway de VPN para se conectar a VNets. 

As etapas neste artigo criarão uma rede virtual, uma sub-rede, uma sub-rede do gateway e um gateway VPN com base em rota (gateway de rede virtual). Concluída a criação de gateway, você pode criar conexões. Estas etapas exigem uma assinatura do Azure. Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="vnet"></a>Criar uma rede virtual

[!INCLUDE [create-gateway](../../includes/vpn-gateway-create-virtual-network-portal-include.md)]

## <a name="gwsubnet"></a>Adicione uma sub-rede de gateway

[!INCLUDE [gateway subnet](../../includes/vpn-gateway-add-gateway-subnet-portal-include.md)]

## <a name="gwvalues"></a>Configurar e criar o gateway

[!INCLUDE [create-gateway](../../includes/vpn-gateway-add-gw-p2s-rm-portal-include.md)]

>[!NOTE]
>O SKU do gateway básico não oferece suporte à autenticação IKEv2 ou RADIUS. Se você planeja ter clientes Mac conectados à sua rede virtual, não use a SKU básica.

## <a name="viewgw"></a>Veja o Gateway de VPN

1. Depois de criar o gateway, navegue para VNet1 no portal. O gateway de VPN é exibido na página Visão geral de como um dispositivo conectado.

   ![Dispositivos conectados](./media/create-routebased-vpn-gateway-portal/view-connected-devices.png "dispositivos conectados")

2. Na lista de dispositivos, clique em **VNet1GW** para exibir mais informações.

   ![Gateway VPN do modo de exibição](./media/create-routebased-vpn-gateway-portal/view-gateway.png "gateway VPN do modo de exibição")

## <a name="next-steps"></a>Próximas etapas

Uma vez que o gateway tenha sido criado, você pode criar uma conexão entre sua rede virtual e outra rede virtual. Ou criar uma conexão entre sua rede virtual e redes locais.

> [!div class="nextstepaction"]
> [Criar uma conexão site a site](vpn-gateway-howto-site-to-site-resource-manager-portal.md)<br><br>
> [Criar uma conexão ponto a site](vpn-gateway-howto-point-to-site-resource-manager-portal.md)<br><br>
> [Criar uma conexão com outra rede virtual](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
