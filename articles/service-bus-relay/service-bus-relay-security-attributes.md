---
title: Atributos de segurança comuns para a retransmissão do barramento de serviço do Azure
description: Uma lista de verificação de atributos de segurança comuns para avaliar a retransmissão do barramento de serviço do Azure
services: service-bus-relay
ms.service: service-bus-relay
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: mbaldwin
ms.openlocfilehash: d8ce3c995e8e0f20ed6d694f481cc5fc9fde4fa7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66000161"
---
# <a name="security-attributes-for-azure-service-bus-relay"></a>Atributos de segurança para a retransmissão do barramento de serviço do Azure

Este artigo documenta os atributos de segurança incorporados à retransmissão do barramento de serviço do Azure.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso:<ul><li>Criptografia no servidor</li><li>Criptografia do lado do servidor com chaves gerenciadas pelo cliente</li><li>Outros recursos de criptografia (como do lado do cliente, Always Encrypted, etc.)</ul>|  N/D | Retransmissão é um soquete da web e não manter os dados. |
| Criptografia em trânsito:<ul><li>Criptografia do ExpressRoute</li><li>Na criptografia de rede virtual</li><li>Criptografia de rede virtual a rede virtual</ul>| Sim | O serviço exige TLS. |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| Não | Usa somente os certificados de TLS Microsoft.  |
| Criptografia de nível de coluna (serviços de dados do Azure)| N/D | |
| Chamadas à API criptografadas| Sim | HTTPS. |

## <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| Não |  |
| Isolamento de rede e suporte de firewall| Não |  |
| Forçado suporte por túnel| N/D | A retransmissão é o túnel TLS  |

## <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | |

## <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Authentication| Sim | Por meio do SAS. |
| Autorização|  Sim | Por meio do SAS. |


## <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Auditoria e log de plano de controle e gerenciamento| Sim | Por meio [o Azure Resource Manager](../azure-resource-manager/index.yml). |
| Auditoria e log de plano de dados| Sim | Êxito de Conexão / erros e falhas e registrado.  |

## <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim | Por meio [o Azure Resource Manager](../azure-resource-manager/index.yml).|
