---
title: Atributos comuns de segurança para o Azure ExpressRoute
description: Uma lista de verificação de atributos de segurança comuns para avaliar o Azure ExpressRoute
services: expressroute
ms.service: expressroute
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: mbaldwin
ms.openlocfilehash: d6156715fb87831d465197fd8eec59d245221e48
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67083269"
---
# <a name="common-security-attributes-for-azure-expressroute"></a>Atributos comuns de segurança para o Azure ExpressRoute

A segurança é integrada a todos os aspectos de um serviço do Azure. Este artigo documenta os atributos de segurança comuns integrados do Azure ExpressRoute.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso:<ul><li>Criptografia no servidor</li><li>Criptografia do lado do servidor com chaves gerenciadas pelo cliente</li><li>Outros recursos de criptografia (como do lado do cliente, Always Encrypted, etc.)</ul>|  N/D | ExpressRoute não armazena os dados do cliente. |
| Criptografia em trânsito:<ul><li>Criptografia do ExpressRoute</li><li>Na criptografia de VNET</li><li>Criptografia de rede virtual a rede virtual</ul>| Não | |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| N/D |  |
| Criptografia de nível de coluna (serviços de dados do Azure)| N/D | |
| Chamadas à API criptografadas| Sim | Por meio [o Azure Resource Manager](../azure-resource-manager/index.yml) e HTTPS. |

## <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| N/D |  |
| suporte à injeção de rede virtual| N/D | |
| Isolamento de rede e suporte de firewall| Sim | Cada cliente é contido em seu próprio domínio de roteamento e encapsulado para sua própria rede virtual |
| Forçado suporte por túnel| N/D | Por meio do Border Gateway Protocol (BGP). |

## <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | Ver [ExpressRoute monitoramento, alertas e métricas](expressroute-monitoring-metrics-alerts.md).|

## <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Authentication| Sim | Conta de serviço do Gateway para o Microsoft (GWM) (controlador); Acesso just in Time (JIT) para desenvolvimento e operacional |
| Autorização|  Sim |Conta de serviço do Gateway para o Microsoft (GWM) (controlador); Acesso just in Time (JIT) para desenvolvimento e operacional |


## <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações| 
|---|---|--|
| Auditoria e log de plano de controle e gerenciamento| Sim |  |
| Auditoria e log de plano de dados| Não |   |

## <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim | Por meio do provedor de recursos de rede (NRP). |
