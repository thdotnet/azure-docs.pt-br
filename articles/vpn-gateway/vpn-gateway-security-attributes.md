---
title: Atributos de segurança para o Gateway de VPN do Azure
description: Uma lista de verificação de atributos de segurança para avaliar o Gateway de VPN do Azure
services: sql-database
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mbaldwin
ms.openlocfilehash: 0e58e7b3f77d9bb673e300aa60ad07ca9dba5153
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67083126"
---
# <a name="security-attributes-for-azure-vpn-gateway"></a>Atributos de segurança para o Gateway de VPN do Azure

Este artigo documenta os atributos de segurança comum embutidos no Gateway de VPN do Azure.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]


## <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso (por exemplo, criptografia do lado do servidor, a criptografia do lado do servidor com chaves gerenciadas pelo cliente e outros recursos de criptografia) | N/D | Dados do cliente de trânsito de gateway VPN, não armazena os dados do cliente |
| Criptografia em trânsito (como criptografia de ExpressRoute, em criptografia de rede virtual e a criptografia de rede virtual-VNet)| Sim | Gateway de VPN criptografar pacotes de cliente entre os gateways de VPN do Azure e dispositivos VPN no local do cliente (S2S) ou clientes VPN (P2S). Gateways de VPN também dá suporte à criptografia de rede virtual para rede virtual. |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| Não | Chaves pré-compartilhadas especificado pelo cliente são criptografadas em repouso; mas não integrado a CMK ainda. |
| Criptografia de nível de coluna (serviços de dados do Azure)| N/D | |
| Chamadas à API criptografadas| Sim | Por meio [o Azure Resource Manager](../azure-resource-manager/index.yml) e HTTPS  |

## <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| N/D | |
| Suporte à injeção de rede virtual| N/D | . |
| Suporte ao isolamento de rede e Firewalling| Sim | Gateways de VPN são instâncias VM dedicadas para cada rede Virtual do cliente  |
| Forçado suporte por túnel| Sim |  |

## <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | Ver [Logs de diagnóstico do Azure Monitor/alerta](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md) & [/alerta de métricas do Azure Monitor](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).  |

## <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Authentication| Sim | [O Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) para gerenciar o serviço e configurar o gateway de VPN do Azure. |
| Autorização| Sim | Suporte à autorização por meio [RBAC](../role-based-access-control/overview.md). |


## <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Auditoria e log de plano de controle e gerenciamento| Sim | Log de atividades do Gerenciador de recursos do Azure. |
| Auditoria e log de plano de dados | Sim | [Logs de diagnóstico do Azure Monitor](../azure-resource-manager/resource-group-audit.md) para conectividade VPN, registro e auditoria. |

## <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim | Para operações de gerenciamento, o estado de uma configuração de gateway de VPN do Azure pode ser exportado como um modelo do Azure Resource Manager e com controle de versão ao longo do tempo. | 