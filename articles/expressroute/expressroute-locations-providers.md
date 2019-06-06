---
title: 'Locais e provedores de conectividade: Azure ExpressRoute | Microsoft Docs'
description: Este artigo fornece uma visão geral detalhada dos locais onde os serviços são oferecidos e de como se conectar a regiões do Azure. Classificado por local.
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
ms.assetid: feb67da3-5abc-4acb-bad4-f78e3c541ded
ms.service: expressroute
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: jaredr80
ms.openlocfilehash: c443d4705da83a43af10763f633f340b1eab69d3
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66478152"
---
# <a name="expressroute-partners-and-peering-locations"></a>Locais de emparelhamento e parceiros do ExpressRoute

> [!div class="op_single_selector"]
> * [Locais por provedor](expressroute-locations.md)
> * [Provedores por local](expressroute-locations-providers.md)


As tabelas neste artigo fornecem informações sobre provedores de conectividade do ExpressRoute, a cobertura geográfica do ExpressRoute, serviços em nuvem da Microsoft com suporte ao longo do ExpressRoute e SIs (Integradores de Serviço) do ExpressRoute.

## <a name="partners"></a>Provedores de conectividade do ExpressRoute
Há suporte para o ExpressRoute em todos os locais e regiões do Azure. O mapa a seguir fornece uma lista de locais de ExpressRoute e regiões do Azure. Os locais de ExpressRoute são aqueles onde a Microsoft emparelha-se a vários provedores de serviços.

![Mapa de localização][0]

Você terá acesso aos serviços do Azure em todas as regiões dentro de uma região geopolítica se estiver conectado a pelo menos um local de ExpressRoute dentro da região geopolítica. 

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>Regiões do Azure para locais do ExpressRoute em uma região geopolítica
A tabela a seguir fornece um mapa das regiões do Azure para locais de ExpressRoute em uma região geopolítica.

| **Região Geopolítica** | **Zona** | **Regiões do Azure** | **Locais de ExpressRoute** |
| --- | --- | --- | --- |
| **Governo da Austrália** | 1 | Austrália Central, Austrália Central 2 |Canberra, Canberra2 |
| **Europa** | 1 |França Central, Sul da França, Norte da Europa, Europa Ocidental, Oeste do Reino Unido, Sul do Reino Unido |Amsterdã, Amsterdam2, Dublin, Frankfurt, Londres, London2, em Marselha, newport (Gales), Paris, Zurique |
| **América do Norte** | 1 |Leste dos EUA, Oeste dos EUA, Leste dos EUA 2, Oeste dos EUA 2, Centro dos EUA, Centro-Sul dos EUA, Centro-Norte dos EUA, Centro-Oeste dos EUA, Centro do Canadá, Leste do Canadá |Atlanta, Chicago, Dallas, Denver, Las Vegas, Los Angeles, Miami, Nova York, San Antonio, Seattle, vale do Silício, Valley2 de silício, Washington DC, Washington DC2, Montreal, cidade de Quebec, Toronto |
| **Ásia** | 2 |Ásia Oriental, Sudeste Asiático |Hong Kong SAR, Kuala Lumpur, Singapore, Singapore2, Taipei |
| **Austrália** | 2 |Sudeste da Austrália, Leste da Austrália |Melbourne, Perth, Sydney | 
| **Índia** | 2 |Oeste da Índia, Índia Central, Sul da Índia |Chennai, Chennai2, Mumbai, Mumbai2 |
| **Japão** | 2 |Oeste do Japão, Leste do Japão |Osaka, Tóquio |
| **Coreia do Sul** | 2 |Coreia Central, Sul da Coreia |Busan, Seul|
| **EMIRADOS ÁRABES UNIDOS** | 3 | EAU Central, Norte dos EAU | Dubai, Dubai2 |
| **África do Sul** | 3 |Oeste da África do Sul, Norte da África do Sul |Cidade do Cabo, Joanesburgo |
| **América do Sul** | 3 |Sul do Brasil |São Paulo |

### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>Regiões e limites geopolíticos para nuvens nacionais
A tabela a seguir fornece informações sobre regiões e limites geopolíticos para nuvens nacionais.

| **Região Geopolítica** | **Regiões do Azure** | **Locais de ExpressRoute** |
| --- | --- | --- |
| **Nuvem do Governo dos EUA** |Gov. EUA - Arizona, US Gov Iowa, US Gov - Texas, US Gov Virginia, US DoD Central, US DoD Leste  |Chicago, Dallas, Nova York, Phoenix, San Antonio, Seattle, Vale do Silício, Washington DC |
| **Leste da China** |Leste da China, Leste da China2 |Shanghai, Shanghai2 |
| **Norte da China** |Norte da China, Norte da China2 |Beijing, Beijing2 |
| **Alemanha** |Alemanha Central, Alemanha Oriental |Berlim+, Frankfurt |

Não há suporte para conectividade entre regiões geopolíticas no SKU de ExpressRoute padrão. Você precisará habilitar o complemento premium de ExpressRoute para dar suporte a conectividade global. Não há suporte a conectividade para ambientes de nuvem nacionais. Você pode trabalhar com seu provedor de conectividade se surgir necessidade de fazê-lo.

## <a name="locations"></a>Locais de provedor de conectividade

A tabela a seguir mostra os locais de conectividade e os provedores de serviço para cada local. Se você quiser exibir provedores de serviços e os locais para os quais podem fornecer serviço, confira [Locais pelo provedor de serviços](expressroute-locations.md#locations). 

**Regiões do Azure locais** são aqueles que [Local de ExpressRoute](expressroute-faqs.md) em cada local de emparelhamento pode acessar. **n/d** indica o Local do ExpressRoute não está disponível nesse local de emparelhamento.


### <a name="production-azure"></a>Produção do Azure
| **Localidade** | **Proprietário da localização do emparelhamento** | **Regiões do Azure locais** | **Provedores de Serviço** |
| --- | --- | --- | --- |
| **Amsterdã** | Equinix | Europa Ocidental | Aryaka Networks, AT&T NetBond, British Telecom, Colt, Equinix, euNetworks, GÉANT, InterCloud, Interxion, KPN, IX Reach, Level 3 Communications, Megaport, NTT Communications, Orange, Tata Communications, TeleCity Group, Telefonica, Telenor, Telia Carrier, Verizon, Zayo |
| **Amsterdã2** | Interxion | Europa Ocidental | DE-CIX, Interxion |
| **Atlanta** | Equinix | n/d | Equinix, Megaport |
| **Busan** |LG CNS | Sul da Coreia | LG CNS |
| **Camberra** | CDC | Austrália Central | CDC |
| **Camberra** | CDC | Austrália Central 2| CDC |
| **Cidade do Cabo** | Teraco | Sul da África do Sul | Internet Solutions - Cloud Connect, Liquid Telecom, Teraco |
| **Chennai** | Tata Communications | Sul da Índia | Global CloudXchange (GCX), SIFY, Tata Communications |
| **Chennai2** | Airtel | Sul da Índia | Airtel |
| **Chicago** | Equinix | Centro-Norte dos EUA | Aryaka Networks, AT&T NetBond, Cologix, Comcast, Coresite, Equinix, InterCloud, Internet2, Level 3 Communications, Megaport, PacketFabric, PCCW Global Limited, Sprint, Telia Carrier, Verizon, Zayo |
| **Dallas** | Equinix | n/d | Aryaka Networks, AT&T NetBond, Cologix, Equinix, Internet2, Level 3 Communications, Megaport, Neutrona Networks, Telmex Uninet, Telia Carrier, Transtelco, Verizon, Zayo|
| **Denver** | CoreSite | Centro-Oeste dos EUA | CoreSite, Megaport, Zayo |
| **Dubai** | Etisalat EAU | Norte dos EAU | Etisalat EAU |
| **Dubai2** | datamena DU | Norte dos EAU | DU datamena, Orixcom |
| **Dublim** | Equinix | Norte da Europa | Colt, eir, Equinix, Interxion, Megaport |
| **Frankfurt** | Interxion | n/d | DE-CIX, Interxion |
| **RAE de Hong Kong** | Equinix | Ásia Oriental | Aryaka Networks, British Telecom, China Telecom Global, Equinix, Megaport, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Verizon |
| **Joanesburgo** | Teraco | Norte da África do Sul | Internet Solutions - Cloud Connect, Liquid Telecom, Teraco |
| **Kuala Lumpur** | TIME dotCom | n/d | TIME dotCom |
| **Las Vegas** | Alternar | n/d | CenturyLink Cloud Connect, Megaport |
| **Londres** | Equinix | Sul do Reino Unido | AT&T NetBond, British Telecom, Colt, Equinix, InterCloud, Internet Solutions – Cloud Connect, Interxion, Jisc, Level 3 Communications, Megaport, MTN, NTT Communications, Orange, PCCW Global Limited, Tata Communications, Telecity Group, Telehouse – KDDI, Telenor, Telia Carrier, Verizon, Vodafone, Zayo |
| **London2** | Telehouse | Sul do Reino Unido | Alcance IX, Equinix |
| **Los Angeles** | CoreSite | n/d | CoreSite, Equinix, Megaport, Neutrona Networks, NTT, Zayo |
| **Marselha** |Interxion | Sul da França | Interxion |
| **Melbourne** | NextDC | Sudeste da Austrália | AARNet, Devoli, Equinix, Megaport, NEXTDC, Optus, Telstra Corporation, TPG Telecom |
| **Miami** | Equinix | n/d | C3ntro+, Equinix, Megaport, Neutrona Networks |
| **Montreal** | Cologix | n/d | Bell Canada, Cologix, Telus, Zayo |
| **Mumbai** | Tata Communications | Índia Ocidental | Global CloudXchange (GCX), Reliance Jio, Sify, Tata Communications, Verizon |
| **Mumbai2** | Airtel | Índia Ocidental | Airtel, Sify, Vodafone Idea |
| **Nova Iorque** | Equinix | n/d | CenturyLink Cloud Connect, Coresite, Equinix, InterCloud, Megaport, Packet, Zayo |
| **Newport (País de Gales)** | Dados da Próxima Geração | Oeste do Reino Unido | British Telecom, Colt, Level 3 Communications, dados da próxima geração |
| **Osaka** | Equinix | Oeste do Japão | Equinix, Internet Initiative Japan Inc. - IIJ, NTT Communications, NTT SmartConnect, Softbank |
| **Paris** | Interxion | França Central | Colt, Equinix, Intercloud, Interxion, Orange, Telia Carrier, Zayo |
| **Perth** | NextDC | n/d | Megaport, NextDC |
| **Cidade de Quebec** | 4Degrees | Leste do Canadá | Bell Canada, Megaport |
| **San Antonio** | CyrusOne | Centro-Sul dos Estados Unidos | CenturyLink Cloud Connect, Megaport |
| **São Paulo** | Equinix | Sul do Brasil | Aryaka Networks, Ascenty Data Centers, British Telecom, Equinix, Level 3 Communications, Neutrona Networks, Orange, Tata Communications, Telefonica, UOLDIVEO |
| **Seattle** | Equinix | Oeste dos EUA 2 | Aryaka Networks, Equinix, Level 3 Communications, Megaport, Telus, Zayo |
| **Seul** | KINX | Coreia Central | KINX, LG CNS, Sejong Telecom |
| **Vale do Silício** | Equinix | Oeste dos EUA | Aryaka Networks, AT & T NetBond, British Telecom, CenturyLink Cloud Connect, Comcast, Coresite, Equinix, InterCloud, alcançar IX, pacote, PacketFabric, Level 3 Communications, Megaport, Orange, Sprint, Tata Communications, Verizon, Zayo |
| **Valley2 de silício** | Coresite | Oeste dos EUA | Coresite | 
| **Singapura** | Equinix | Sudeste Asiático | Aryaka Networks, AT&T NetBond, British Telecom, Epsilon Global Communications, Equinix, InterCloud, Level 3 Communications, Megaport, NTT Communications, Orange, SingTel, Tata Communications, Telstra Corporation, Verizon, Vodafone |
| **Cingapura2** | Global Switch | Sudeste Asiático | COLT, Épsilon Global Communications, Megaport, SingTel |
| **Sydney** | Equinix | Leste da Austrália | AARNet, AT&T NetBond, British Telecom, Devoli, Equinix, Kordia, Megaport, NEXTDC, NTT Communications, Optus, Orange, Telstra Corporation, TPG Telecom, Verizon |
| **Taipé** | Chief Telecom | n/d | Diretor de telecomunicações, FarEasTone |
| **Tóquio** | Equinix | Leste do Japão | Aryaka Networks, AT&T NetBond, British Telecom, CenturyLink Cloud Connect, Colt, Equinix, Internet Initiative Japan Inc. – IIJ, NTT Communications, NTT EAST, Orange, Softbank, Verizon |
| **Toronto** | Cologix | Canadá Central | AT&T NetBond, Bell Canada, CenturyLink Cloud Connect, Cologix, Equinix, IX Reach Megaport, Telus, Verizon, Zayo |
| **Washington, D.C.** | Equinix | Leste dos EUA, Leste dos EUA 2 | Aryaka Networks, AT & T NetBond, British Telecom, Cologix, Comcast, Coresite, Equinix, Internet2, InterCloud, Level 3 Communications, Megaport, Neutrona redes, NTT Communications, Orange, PacketFabric, Sprint, Tata Communications, operadora Telia, Verizon, Zayo |
| **Washington DC2** | Coresite | Leste dos EUA, Leste dos EUA 2 |Coresite | 
| **Zurich** | Interxion | n/d | Interxion |

 **+** indica que haverá em breve

### <a name="national-cloud-environments"></a>Ambientes de nuvem nacionais

### <a name="us-government-cloud"></a>Nuvem do Governo dos EUA
| **Localidade** | **Provedores de Serviço** |
| --- | --- |
| **Chicago** |AT&T NetBond, Equinix, Level 3 Communications, Verizon |
| **Dallas** |Equinix, Megaport, Verizon |
| **Nova Iorque** |Equinix, CenturyLink Cloud Connect, Verizon |
| **Phoenix** | AT&T NetBond, CenturyLink Cloud Connect, Megaport |
| **San Antonio** | CenturyLink Cloud Connect, Megaport |
| **Vale do Silício** | Equinix, Level 3 Communications, Verizon |
| **Seattle** | Equinix, Megaport |
| **Washington, D.C.** |AT&T NetBond, Equinix, Level 3 Communications, Megaport, Verizon |

### <a name="china"></a>China
| **Localidade** | **Provedores de Serviço** |
| --- | --- |
| **Pequim** |China Telecom |
| **Beijing2** | China telecomunicações, GDS |
| **Xangai** |China Telecom |
| **Shanghai2** | China telecomunicações, GDS |

Para saber mais, consulte [ExpressRoute na China](http://www.windowsazure.cn/home/features/expressroute/)

### <a name="germany"></a>Alemanha
| **Localidade** | **Provedores de Serviço** |
| --- | --- |
| **Berlim** |e-shelter, Megaport+, T-Systems |
| **Frankfurt** |Colt, Equinix, Interxion |

## <a name="c1partners"></a>Conectividade por meio de outros provedores do Exchange
Se seu provedor de conectividade não estiver listado em seções anteriores, você ainda pode criar uma conexão.

* Verifique com seu provedor de conectividade para ver se eles estão conectados a qualquer um dos Exchanges na tabela acima. Você pode verificar os links a seguir para obter mais informações sobre os serviços oferecidos por provedores do Exchange. Vários provedores de conectividade já estão conectados a Exchanges de Ethernet.
  * [Cologix](https://www.cologix.com/)
  * [CoreSite](https://www.coresite.com/)
  * [Equinix Cloud Exchange](https://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [InterXion](https://www.interxion.com/)
  * [NextDC](https://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/)
  
* Faça com que seu provedor de conectividade estenda sua rede para o local de emparelhamento de sua escolha.
  * Certifique-se de que seu provedor de conectividade estenda sua conectividade de maneira altamente disponível para que não exista nenhum ponto de falha.
* Solicite um circuito do ExpressRoute com o Exchange como o provedor de conectividade para conectar-se à Microsoft.
  * Siga as etapas em [Criar um circuito do ExpressRoute](expressroute-howto-circuit-classic.md) para configurar a conectividade.

## <a name="c1partners"></a>Conectividade por meio de provedores de serviço adicionais
| **Localidade** | **Exchange** | **Provedores de conectividade** |
| --- | --- | --- |
| **Amsterdã** | Equinix, Telecity | BICS, CloudXpress, Eurofiber, Fastweb S.p.A, Gulf Bridge International, MainOne, Nianet, Post, Proximus, TDC Erhverv, Telecom Italia Sparkle, Telia |
| **Cidade do Cabo** | Teraco | MTN |
| **Chicago** | Equinix | Windstream Lightower, Enterprise do espectro, |
| **Dallas** | Equinix, Megaport | Axtel, telecomunicações C3ntro, Cox, dados Foundry espectro empresa comercial Transtelco |
| **Frankfurt** | Telecity | BICS, Cinia, Nianet, QSC AG |
| **Hamburgo** | Equinix | Cinia |
| **RAE de Hong Kong** | Equinix | Chief, Macroview Telecom |
| **Joanesburgo** | Teraco | MTN |
| **Londres** | BICS, Equinix, euNetworks, Telecity | Bezeq International Ltd., CoreAzure, Epsilon Telecommunications Limited, Exponential E, HSO, NexGen Networks, Proximus, Tamares Telecom, Zain |
| **Los Angeles** | Equinix |Spectrum Enterprise, Transtelco |
| **Madrid** | Level3 | Zertia |
| **Montreal** | Cologix, Equinix | Airgate Technologies, Inc. Cogeco Peer 1, Rogers, Zirro |
| **Nova Iorque** |Equinix, Megaport | Empresa de espectro de negócios, Lightower, Altice, Webair |
| **Paris** | Equinix | Proximus |
| **Cidade de Quebec** | Megaport | Fibrenoire |
| **São Paulo** | Equinix | Venha Pra Nuvem |
| **Seattle** |Equinix | Alaska Communications |
| **Vale do Silício** |Equinix | Cox Business, Enterprise do espectro, Windstream |
| **Singapura** |Equinix |1CLOUDSTAR, BICS, Epsilon Telecommunications Limited, LGA Telecom, United Information Highway (UIH) |
| **Slough** | Equinix | HSO|
| **Sydney** | Megaport | Macquarie Telecom Group|
| **Tóquio** | Equinix | ARTERIA Networks Corporation, BroadBand Tower, Inc. |
| **Toronto** | Equinix | Airgate Technologies, Inc. Cogeco Peer 1, IVedha Inc, Rogers, Thinktel, Zirro|
| **Washington, D.C.** |Equinix | Altice Business, BICS, Cox Business, Gtt Communications Inc, Epsilon Telecommunications Limited, Lightower, Masergy, Windstream |

## <a name="expressroute-system-integrators"></a>Integradores de sistema de ExpressRoute
Habilitar a conectividade privada para atender às suas necessidades pode ser desafiador, dependendo da escala de sua rede. Você pode trabalhar com qualquer dos integradores de sistema listados na tabela a seguir para ajudá-lo com integração ao ExpressRoute.

| **Continent** | **Integradores do sistema** |
| --- | --- |
| **Ásia** |Avanade Inc., OneAs1a |
| **Austrália** | Ensyst, IT Consultancy, MOQdigital, Vigilant.IT |
| **Europa** |Avanade Inc., Altogee, Bright Skies GmbH, Inframon, MSG Services, New Signature, Nelite, Orange Networks, sol-tec |
| **América do Norte** |Avanade Inc., Equinix Professional Services, FlexManage, Lightstream, Perficient, Presidio |
| **América do Sul** |Avanade Inc., Venha Pra Nuvem |
## <a name="next-steps"></a>Próximas etapas
* Para obter mais informações sobre o ExpressRoute, consulte [Perguntas Frequentes sobre ExpressRoute](expressroute-faqs.md).
* Certifique-se que todos os pré-requisitos foram atendidos. Consulte [Pré-requisitos do ExpressRoute](expressroute-prerequisites.md).

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "Mapa de localização"
