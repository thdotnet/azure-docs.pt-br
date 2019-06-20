---
title: Diretrizes para arquivos do Azure NetApp planejamento de rede | Microsoft Docs
description: Descreve as diretrizes que podem ajudar você a criar uma arquitetura de rede em vigor por meio de arquivos do Azure NetApp.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: b-juche
ms.openlocfilehash: bf2262d8a222cec6c5d0d7e53ded7b2994481656
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67205687"
---
# <a name="guidelines-for-azure-netapp-files-network-planning"></a>Diretrizes para planejamento de rede do Azure NetApp Files

Planejamento da arquitetura de rede é um elemento fundamental da criação de qualquer infraestrutura do aplicativo. Este artigo ajuda você a criar uma arquitetura de rede em vigor para suas cargas de trabalho para se beneficiar de recursos avançados dos arquivos do Azure NetApp.

Volumes de arquivos NetApp do Azure são projetados para estar contidos em uma sub-rede de finalidade especial chamada [delegada sub-rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet) em sua rede Virtual do Azure. Portanto, você pode acessar os volumes diretamente da sua rede virtual, redes virtuais emparelhadas na mesma região ou local ao longo de um Gateway de rede Virtual (ExpressRoute ou Gateway de VPN) conforme necessário. A sub-rede é dedicada a arquivos do Azure NetApp e nenhuma conectividade com a internet ou outros serviços do Azure.

## <a name="considerations"></a>Considerações  

Você deve entender algumas considerações ao planejar para a rede de arquivos do Azure NetApp.

### <a name="constraints"></a>Restrições

Os recursos a seguir atualmente não há suportados para arquivos do NetApp do Azure: 

* Grupos de segurança de rede (NSGs) na sub-rede
* Definido pelo usuário UDRs (rotas) com o próximo salto como sub-rede de arquivos do Azure NetApp
* Políticas do Azure (por exemplo, políticas de nomenclatura personalizadas) na interface de arquivos do Azure NetApp
* Balanceadores de carga para tráfego de arquivos do Azure NetApp

As restrições de rede a seguir se aplicam aos arquivos do NetApp do Azure:

* O número de IPs em uso em uma rede virtual com arquivos do Azure NetApp (incluindo VNets emparelhadas) não pode exceder 1000.
* Em cada VNet (Rede virtual do Azure), apenas uma sub-rede pode ser delegada para o Azure NetApp Files.


### <a name="supported-network-topologies"></a>Topologias de rede com suporte

A tabela a seguir descreve as topologias de rede com suporte pelos arquivos do Azure NetApp.  Ele também descreve as soluções alternativas para as topologias sem suporte. 

|    Topologias    |    há suporte para    |     Solução alternativa    |
|-------------------------------------------------------------------------------------------------------------------------------|--------------------|-----------------------------------------------------------------------------|
|    Conectividade com o volume em uma rede virtual local    |    Sim    |         |
|    Conectividade com o volume em uma rede virtual emparelhada (mesma região)    |    Sim    |         |
|    Conectividade com o volume em uma rede virtual emparelhada (cruz região ou emparelhamento global)    |    Não    |    Nenhum    |
|    Conectividade com um volume ao longo do gateway de ExpressRoute    |    Sim    |         |
|    Conectividade do local para um volume em um rede virtual spoke sobre o gateway de ExpressRoute e emparelhamento com o trânsito de gateway de rede virtual    |    Não    |    Criar uma sub-rede de delegados na VNet (VNet do Azure com o Gateway) do hub    |
|    Conectividade do local para um volume em um rede virtual spoke sobre gateway de VPN    |    Sim    |         |
|    Conectividade do local para um volume em um rede virtual spoke sobre o gateway de VPN e rede virtual emparelhamento com o trânsito de gateway    |    Sim    |         |


## <a name="virtual-network-for-azure-netapp-files-volumes"></a>Rede virtual para volumes de arquivos do Azure NetApp

Esta seção explica conceitos que ajudam com o planejamento de rede virtual.

### <a name="azure-virtual-networks"></a>Redes virtuais do Azure

Antes de provisionar um volume de arquivos do Azure NetApp, você precisa criar uma rede virtual do Azure (VNet) ou usar uma já existente em sua assinatura. A rede virtual define o limite de rede do volume.  Para obter mais informações sobre como criar redes virtuais, consulte a [documentação da rede Virtual do Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

### <a name="subnets"></a>Sub-redes

Subredes segmentar a rede virtual em espaços de endereço separados que podem ser usados pelos recursos do Azure-los.  Volumes de arquivos NetApp do Azure estão contidos em uma sub-rede de finalidade especial chamada [delegada sub-rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-subnet). 

Delegação de sub-rede dá permissões explícitas para o serviço de arquivos do Azure NetApp para criar recursos específicos do serviço na sub-rede.  Ele usa um identificador exclusivo na implantação do serviço. Nesse caso, uma interface de rede é criada para habilitar a conectividade para arquivos do Azure NetApp.

Se você usar uma nova rede virtual, você pode criar uma sub-rede e delegar a sub-rede NetApp o arquivos do Azure seguindo as instruções em [delegar uma sub-rede para arquivos do Azure NetApp](azure-netapp-files-delegate-subnet.md). Você também pode delegar uma sub-rede vazia existente que já não é delegada aos outros serviços.

Se a rede virtual está emparelhada com outra rede virtual, você não pode expandir o espaço de endereço de rede virtual. Por esse motivo, a nova sub-rede delegada precisa ser criado no espaço de endereço de rede virtual. Se você precisa estender o espaço de endereço, você deve excluir o emparelhamento antes de expandir o espaço de endereço de VNet.

### <a name="udrs-and-nsgs"></a>UDRs e NSGs

Grupos de segurança de rede (NSGs) com um próximo salto não podem ser usados como delegadas sub-redes para arquivos do Azure NetApp. Da mesma forma, as rotas definidas pelo usuário (UDRs) também não têm suporte. 

Como alternativa, você pode aplicar NSGs para outras sub-redes que permitem ou negam o tráfego de e para a sub-rede de arquivos do Azure NetApp delegada.  

## <a name="azure-native-environments"></a>Ambientes nativos do Azure

O diagrama a seguir ilustra um ambiente nativo do Azure:

![Ambiente de rede nativa do Azure](../media/azure-netapp-files/azure-netapp-files-network-azure-native-environment.png)

### <a name="local-vnet"></a>Rede virtual local

Um cenário básico é criar ou se conectar a um volume de arquivos do Azure NetApp de uma máquina virtual (VM) na mesma rede virtual. Para rede virtual 2 no diagrama acima, o Volume 1 é criado em uma sub-rede de delegado e podem ser montados em 1 VM na sub-rede padrão.

### <a name="vnet-peering"></a>Emparelhamento VNet

Se você tiver redes virtuais adicionais na mesma região que precisam acessar recursos uns dos outros, as redes virtuais podem ser conectadas usando [emparelhamento VNet](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) para habilitar a conectividade segura por meio da infraestrutura do Azure. 

Considere a rede virtual 2 e 3 de rede virtual no diagrama acima. Se VM 1 precisa se conectar à VM 2 e Volume 2, ou se o VM 2 precisa se conectar à VM 1 ou Volume 1, em seguida, você precisa habilitar o emparelhamento VNet entre a rede virtual 2 e 3 de rede virtual. 

Além disso, considere um cenário em que 1 rede virtual está emparelhada com vnet2 e vnet2 é emparelhada com a rede virtual 3 na mesma região. Os recursos de rede virtual 1 podem se conectar aos recursos na rede virtual 2, mas ele não pode se conectar aos recursos na rede virtual 3, a menos que a rede virtual 1 e 3 de rede virtual estejam emparelhadas. 

No diagrama acima, embora 3 VM pode se conectar ao Volume 1, 4 de VM não pode se conectar ao Volume 2.  A razão para isso é que as redes virtuais spoke não emparelhadas, e _não há suporte para roteamento de tráfego por meio do emparelhamento de VNet_.

## <a name="hybrid-environments"></a>Ambientes híbridos

O diagrama a seguir ilustra um ambiente híbrido: 

![Ambiente de rede híbrida](../media/azure-netapp-files/azure-netapp-files-network-hybrid-environment.png)

No cenário híbrido, aplicativos de locais de data centers precisam acessar os recursos no Azure.  Esse é o caso se você deseja estender seu data center do Azure, ou você deseja usar serviços nativos do Azure ou recuperação de desastres. Ver [opções de planejamento do Gateway de VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways?toc=%2fazure%2fvirtual-network%2ftoc.json#planningtable) sobre como conectar vários recursos locais para recursos no Azure por meio de uma VPN site a site ou ExpressRoute.

Em uma topologia hub-spoke híbrida, o hub de rede virtual no Azure atua como um ponto central de conectividade para sua rede local. Os raios são VNets emparelhadas com o hub e eles podem ser usados para isolar as cargas de trabalho.

Dependendo da configuração. Você pode conectar recursos do local a recursos no hub quanto nos spokes.

Na topologia ilustrada acima, a rede local está conectada a um hub de rede virtual no Azure e há spoke 2 redes virtuais na mesma região é emparelhada com a VNet do hub.  Nesse cenário, as opções de conectividade com suporte para volumes de arquivos do Azure NetApp são da seguinte maneira:

* Recursos de local VM 1 e 2 da VM pode se conectar para Volume 1 no hub usando uma VPN de site a site ou rota expressa. 
* Recursos locais VM 1 e 2 da VM podem se conectar como Volume 2 ou 3 de Volume ao longo de uma VPN site a site e o emparelhamento de Vnet regional.
* VM 3 no hub de rede virtual pode se conectar ao volume 2 no spoke 1 rede virtual e o Volume 3 no spoke 2 da rede virtual.
* 4 de VM de spoke 1 rede virtual e 5 de VM de spoke 2 da rede virtual podem se conectar ao Volume 1 na VNet do hub.

4 de VM no spoke 1 rede virtual não pode se conectar ao Volume 3 no spoke 2 da rede virtual. Além disso, 5 de VM no spoke VNet2 não é possível se conectar ao Volume 2 no spoke 1 rede virtual. Esse é o caso, porque as redes virtuais spoke não emparelhadas e _não há suporte para roteamento de tráfego por meio do emparelhamento de VNet_.

## <a name="next-steps"></a>Próximas etapas

[Delegar uma sub-rede ao Azure NetApp Files](azure-netapp-files-delegate-subnet.md)
