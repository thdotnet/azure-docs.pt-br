---
title: Atributos de segurança para o balanceador de carga do Azure
description: Uma lista de verificação de atributos de segurança para avaliar o balanceador de carga
services: load-balancer
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mbaldwin
ms.openlocfilehash: 1892b918ed35221bc73d5070d5a73ecc359aa4c2
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800075"
---
# <a name="security-attributes-for-azure-load-balancer"></a>Atributos de segurança para o balanceador de carga do Azure

Este artigo documenta os atributos de segurança comum embutidos no balanceador de carga do Azure.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso (por exemplo, criptografia do lado do servidor, a criptografia do lado do servidor com chaves gerenciadas pelo cliente e outros recursos de criptografia) | N/D | |
| Criptografia em trânsito (como criptografia de ExpressRoute, em criptografia de rede virtual e a criptografia de rede virtual-VNet)| N/D | |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| N/D | |
| Criptografia de nível de coluna (serviços de dados do Azure)| N/D | |
| Chamadas à API criptografadas| Sim | Por meio de [o Azure Resource Manager](../azure-resource-manager/index.yml). |

## <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| N/D | |
| Suporte à injeção de rede virtual| N/D | . |
| Suporte ao isolamento de rede e Firewalling| N/D |  |
| Forçado suporte por túnel| N/D | |

## <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | Ver [logs do Azure Monitor para Load Balancer básico](load-balancer-monitor-log.md). |

## <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Autenticação| N/D |  |
| Autorização| N/D |  |


## <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Auditoria e log de plano de controle e gerenciamento| Sim | Ver [logs do Azure Monitor para Load Balancer básico](load-balancer-monitor-log.md). |
| Auditoria e log de plano de dados | N/D |  |

## <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| N/D |  | 
