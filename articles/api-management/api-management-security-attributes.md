---
title: Atributos de segurança para gerenciamento de API do Azure
description: Uma lista de verificação de atributos de segurança comuns para avaliar o gerenciamento de API
services: api-management
author: msmbaldwin
manager: barbkess
ms.service: api-management
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 3b5826d472b80179c5eb76e0e3a6b1c7ee282487
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66001084"
---
# <a name="security-attributes-for-api-management"></a>Atributos de segurança para gerenciamento de API

Este artigo documenta os atributos de segurança integrados de gerenciamento de API.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso:<ul><li>Criptografia no servidor</li><li>Criptografia do lado do servidor com chaves gerenciadas pelo cliente</li><li>Outros recursos de criptografia (como do lado do cliente, Always Encrypted, etc.)</ul>| Sim (somente para a criptografia no lado do serviço) | Dados confidenciais, como certificados, chaves e valores chamado de segredo são criptografados com serviços gerenciados, por chaves de instância de serviço. |
| Criptografia em trânsito:<ul><li>Criptografia do ExpressRoute</li><li>Na criptografia de rede virtual</li><li>Criptografia de rede virtual a rede virtual</ul>| Sim | [Rota expressa](../expressroute/index.yml) e criptografia de rede virtual é fornecida pela [a rede do Azure](../virtual-network/index.yml). |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| Não | Todas as chaves de criptografia são por instância de serviço e serviço gerenciado. |
| Criptografia de nível de coluna (serviços de dados do Azure)| N/D | |
| Chamadas à API criptografadas| Sim | Chamadas de plano de gerenciamento são feitas por meio [do Azure Resource Manager](../azure-resource-manager/index.yml) via TLS. Um JWT (token Web JSON) válido é necessário.  Chamadas de plano de dados podem ser protegidas com TLS e um dos mecanismos de autenticação com suporte (por exemplo, o certificado do cliente ou o JWT).
 |

## <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| Não | |
| Suporte à injeção de rede virtual| Sim | |
| Isolamento de rede e suporte de firewall| Sim | Usando grupos de segurança de rede (NSG) e o Gateway de aplicativo do Azure (ou outro dispositivo de software), respectivamente. |
| Forçado suporte por túnel| Sim | A rede do Azure fornece túnel forçado. |

## <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | |

## <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Authentication| Sim | |
| Autorização| Sim | |


## <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Auditoria e log de plano de controle e gerenciamento| Sim | [Logs de atividade do Azure Monitor](../azure-monitor/platform/activity-logs-overview.md) |
| Auditoria e log de plano de dados| Sim | [Logs de diagnóstico do Azure Monitor](../azure-monitor/platform/diagnostic-logs-overview.md) e (opcionalmente) [do Azure Application Insights](../azure-monitor/app/app-insights-overview.md).  |

## <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim | Usando o [Kit de recursos de DevOps de gerenciamento de API do Azure](https://aka.ms/apimdevops) |

## <a name="vulnerability-scans-false-positives"></a>Verificações de vulnerabilidade de falsos positivos

Esta seção documenta as vulnerabilidades mais comuns, que não afetam o gerenciamento de API do Azure.

| Vulnerabilidade               | DESCRIÇÃO                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ticketbleed (CVE-2016-9244) | Ticketbleed é uma vulnerabilidade na implementação da extensão TLS SessionTicket encontrado em alguns produtos de F5. Ele permite que o vazamento ("mais avançadas") de até 31 bytes de dados de memória não inicializada. Isso é causado pela pilha de TLS, preenchimento de uma ID de sessão passada do cliente, com os dados para torná-lo 32 bits de comprimento. |
