---
title: Integração da Central de segurança com produtos de segurança do Azure | Microsoft Docs
description: Este tópico apresenta os produtos de segurança do Azure que foram integrados com a Central de segurança do Azure.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: ad4b0373-08ee-46ca-a573-638ed93a647c
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 805c770f1a7e9bb4e0619b27ac937a2451421dc6
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571732"
---
# <a name="security-center-integration-with-azure-security-products-in-asc"></a>Integração da Central de segurança com produtos de segurança do Azure no ASC

A Central de segurança oferece aos clientes com licenças adicionais da Microsoft para integrar suas descobertas à Central de segurança e exibi-los de maneira consolidada.

* [Azure WAF](#azure-waf)
* [Azure DDoS](#azure-ddos)

## Azure WAF <a name="azure-waf"></a>

O Gateway de Aplicativo do Azure oferece um WAF (firewall do aplicativo Web) que fornece proteção centralizada de seus aplicativos Web contra vulnerabilidades e explorações comuns.

Aplicativos Web cada vez mais são alvo de ataques mal-intencionados que exploram vulnerabilidades conhecidas com frequência. O WAF do Gateway de aplicativo se baseia em Core regra CRS (conjunto) 3.0 ou 2.2.9 do Open Web OWASP Application Security Project (). O WAF é atualizado automaticamente para se proteger contra novas vulnerabilidades, sem nenhuma configuração adicional necessária. Alertas gerados por WAF são transmitidos à Central de segurança. Para obter mais informações sobre os alertas gerados pelo WAF, consulte este [artigo](https://docs.microsoft.com/azure/application-gateway/application-gateway-crs-rulegroups-rules?tabs=owasp3#crs911).

## Azure DDoS <a name="azure-ddos"></a>

Distribuído ataques de negação de serviço (DDoS) são conhecidos por serem fáceis de executar. Portanto, eles se tornaram uma preocupação de segurança excelente para os clientes que estão migrando seus aplicativos para a nuvem. 

Um ataque de DDoS tenta esgotar os recursos de um aplicativo, fazendo com que o aplicativo não fique disponível para usuários legítimos. Ataques de DDoS podem direcionar qualquer ponto de extremidade que pode ser alcançado por meio da Internet.

A proteção contra DDoS do Azure, combinada com as práticas recomendadas de design de aplicativo, forneça uma linha de defesa contra ataques de DDoS. A proteção contra DDoS do Azure fornece diferentes camadas de serviço. Para obter mais informações, consulte [visão geral da proteção de DDoS do Azure](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview).

A Proteção contra DDoS Standard pode mitigar os tipos de ataques a seguir:

> [!div class="mx-tableFixed"]

|Alerta|DESCRIÇÃO|
|---|---|
|**Ataque volumétrico detectado**|Meta desse ataque é inundar a camada de rede com uma quantidade significativa de tráfego aparentemente legítimo. Eles incluem inundações de UDP, inundações de amplificação e outras inundações de pacotes falsificados. A Proteção contra DDoS Standard mitiga esses ataques de potencialmente vários gigabytes absorvendo-os e depurando-os com a escala de rede global do Azure automaticamente.|
|**Ataque de protocolo detectado**|Esses ataques renderizam um destino inacessível, explorando uma vulnerabilidade na camada 3 e a pilha de protocolo de 4 de camada. Eles incluem ataques de inundação SYN, ataques de reflexão e outros ataques de protocolo. A Proteção contra DDoS Standard mitiga esses ataques diferenciando entre o tráfego mal-intencionado e o legítimo, interagindo com o cliente e bloqueando o tráfego mal-intencionado.|
|**Ataque de camada de recursos (aplicativo) detectado**|Esses ataques são direcionados a pacotes de aplicativo web para interromper a transmissão de dados entre os hosts. Esses ataques incluem violações de protocolo HTTP, injeção de SQL, scripts intersites e outros ataques de camada 7. Use o firewall do aplicativo web Gateway de aplicativo do Azure, com proteção contra DDoS Standard, para proteger contra esses ataques. Também há ofertas de firewall de aplicativo web de terceiros disponível no Azure Marketplace.|
