---
title: "Pré-requisitos para a adoção do Azure ExpressRoute| Microsoft Docs"
description: "Esta página fornece uma lista dos requisitos a serem atendidos antes que você pode solicitar um circuito de Rota Expressa do Azure."
documentationcenter: na
services: expressroute
author: cherylmc
manager: timlt
editor: 
ms.assetid: f872d25e-acfd-405d-9d1b-dcb9f323a2ff
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 1/6/2017
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 1cb57e5156dab976599ddfa9a58f26ca8ef1ee0e
ms.openlocfilehash: 304acc7d2bfdefffde700ecd469b6335b08b85e6
ms.lasthandoff: 02/22/2017


---
# <a name="expressroute-prerequisites--checklist"></a>Pré-requisitos e lista de verificação da Rota Expressa
Para se conectar aos serviços de nuvem da Microsoft usando o ExpressRoute, você precisa verificar se os requisitos listados nas seções a seguir foram atendidos.

[!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Conta do Azure
* Uma conta válida e ativa do Microsoft Azure. Esta conta é necessária para configurar o circuito do ExpressRoute. Os circuitos da Rota Expressa são recursos das assinaturas do Azure. Uma assinatura do Azure será um requisito mesmo se a conectividade estiver limitada a serviços de nuvem da Microsoft que não sejam do Azure, como os serviços do Office 365 e o CRM online.
* Uma assinatura ativa do Office 365 (se estiver usando os serviços do Office 365). Para mais informações, consulte a seção [Requisitos específicos do Office 365](#office-365-specific-requirements) deste artigo.

## <a name="connectivity-provider"></a>Provedor de conectividade
* Você pode trabalhar com um [parceiro de conectividade de Rota Expressa](expressroute-locations.md#partners) para se conecta à nuvem da Microsoft. Você pode configurar uma conexão entre sua rede local e a Microsoft de [três maneiras](expressroute-introduction.md). 
* Se seu provedor não for um parceiro de conectividade da Rota Expressa, você ainda poderá se conectar à nuvem da Microsoft por meio de um [provedor de troca de nuvem](expressroute-locations.md#nonpartners).

## <a name="network-requirements"></a>Requisitos de rede
* **Conectividade redundante**: não há nenhum requisito de redundância de conectividade física entre você e seu provedor. A Microsoft não exige que sessões BGP redundantes sejam configuradas entre os roteadores da Microsoft e os roteadores emparelhados, mesmo quando você tiver apenas [uma conexão física com uma troca de nuvem](expressroute-faqs.md#onep2plink). 
* **Roteamento**: dependendo de como você se conecta ao Microsoft Cloud, você ou seu provedor precisarão configurar e gerenciar as sessões BGP para [domínios de roteamento](expressroute-circuit-peerings.md). Algum provedor de conectividade Ethernet ou o provedor de troca de nuvem pode oferecer gerenciamento BGP como um serviço de valor agregado.
* **NAT**: a Microsoft só aceita endereços IP públicos por meio de emparelhamento da Microsoft. Se você estiver usando endereços IP privados em sua rede local, você ou seu provedor precisará converter os endereços IP privados em endereços IP públicos [usando NAT](expressroute-nat.md).
* **QoS**: o Skype for Business tem vários serviços (por exemplo: voz, vídeo, texto) que exigem tratamento diferenciado de QoS. Você e seu provedor devem seguir os [requisitos de QoS](expressroute-qos.md).
* **Segurança de rede**: considere a [segurança de rede](../best-practices-network-security.md) ao se conectar ao Microsoft Cloud pelo ExpressRoute.

## <a name="office-365"></a>Office 365
Se você planeja habilitar o Office 365 no ExpressRoute, consulte os documentos a seguir para saber mais sobre os requisitos do Office 365.

* [Visão geral do ExpressRoute para Office 365](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
* [Roteamento com o ExpressRoute para Office 365](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
* [Intervalos de endereços IP e URLs do Office 365](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
* [Planejamento da rede e ajuste de desempenho para Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
* [Ferramentas e calculadoras da largura de banda da rede](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
* [Integração do Office 365 com ambientes locais](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)
* [Vídeos de treinamento avançados do ExpressRoute no Office 365](https://channel9.msdn.com/series/aer/)

## <a name="crm-online"></a>CRM Online
Se você planeja habilitar o CRM Online no ExpressRoute, consulte os documentos a seguir para saber mais sobre os requisitos do CRM Online

* [URLs do CRM Online](https://support.microsoft.com/kb/2655102) e [Intervalos de endereços IP](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>Próximas etapas
* Para obter mais informações sobre a Rota Expressa, consulte [Perguntas Frequentes sobre Rota Expressa](expressroute-faqs.md).
* Localize um provedor de conectividade da Rota Expressa. Consulte [Parceiros e locais de emparelhamento da Rota Expressa](expressroute-locations.md).
* Consulte os requisitos de [Roteamento](expressroute-routing.md), [NAT](expressroute-nat.md) e [QoS](expressroute-qos.md).
* Configurar sua conexão da Rota Expressa.
  * [Criar um circuito da Rota Expressa](expressroute-howto-circuit-classic.md)
  * [Configurar o roteamento](expressroute-howto-routing-classic.md)
  * [Vincular uma Rede Virtual a um circuito de Rota Expressa](expressroute-howto-linkvnet-classic.md)


