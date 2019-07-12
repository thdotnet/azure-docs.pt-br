---
title: Atributos de segurança para o banco de dados SQL
description: Uma lista de verificação de atributos de segurança para avaliar o banco de dados SQL
services: sql-database
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mbaldwin
ms.openlocfilehash: 71e87a3295a9cd73dca2f97b3fa04a5d5ff76f84
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798680"
---
# <a name="security-attributes-for-azure-sql-database"></a>Atributos de segurança para o banco de dados SQL

Este artigo documenta os atributos de segurança comuns que são criados no banco de dados SQL.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

Banco de dados SQL inclui tanto [banco de dados individual](sql-database-single-index.yml) e [instância gerenciada](sql-database-managed-instance.md). As entradas a seguir se aplicam a ambas as ofertas, exceto quando indicado o contrário.

## <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso:<ul><li>Criptografia no servidor</li><li>Criptografia do lado do servidor com chaves gerenciadas pelo cliente</li><li>Outros recursos de criptografia, como o lado do cliente ou o Always Encrypted</ul>| Sim | Chamado "criptografia em uso", conforme descrito no artigo [Always Encrypted](sql-database-always-encrypted.md). Criptografia do lado do servidor usa [a transparent data encryption](transparent-data-encryption-azure-sql.md).|
| Criptografia em trânsito:<ul><li>Criptografia de ExpressRoute do Azure</li><li>Criptografia em uma rede virtual</li><li>Criptografia entre redes virtuais</ul>| Sim | Usar HTTPS. |
| Tratando como CMK ou BYOK de chave de criptografia| Sim | Tratamento de chaves gerenciados pelo serviço e gerenciadas pelo cliente são oferecidos. A última opção é oferecida por meio [Azure Key Vault](../key-vault/index.yml). |
| Criptografia em nível de coluna fornecida pelos serviços de dados do Azure| Sim | Por meio [Always Encrypted](sql-database-always-encrypted.md). |
| Chamadas à API criptografadas| Sim | Usando HTTPS/SSL. |

## <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| Sim | Aplica-se ao [banco de dados único](sql-database-single-index.yml) apenas. |
| Suporte à injeção de rede Virtual do Azure| Sim | Aplica-se ao [instância gerenciada](sql-database-managed-instance.md) apenas. |
| Isolamento de rede e suporte ao firewall| Sim | Firewall no nível de servidor e de nível de banco de dados. Isolamento de rede destina [instância gerenciada](sql-database-managed-instance.md) apenas. |
| Forçado suporte por túnel| Sim | [A instância gerenciada](sql-database-managed-instance.md) por meio de um [ExpressRoute](../expressroute/index.yml) VPN. |

## <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento de suporte, como o Log Analytics ou Application Insights do Azure| Sim | SecureSphere, a solução SIEM da Imperva, também tem suporte por meio [Hubs de eventos](../event-hubs/index.yml) integração via [a auditoria do SQL](sql-database-auditing.md). |

## <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Autenticação| Sim | Active Directory do Azure (Azure AD) |
| Autorização| Sim | Nenhum |

## <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Plano de controle e o plano de gerenciamento de log e auditoria| Sim | Sim para apenas alguns eventos |
| Plano de dados de log e auditoria | Sim | Por meio de [auditoria do SQL](sql-database-auditing.md) |

## <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte de gerenciamento de configuração, como o controle de versão da configuração| Não  | Nenhum |

## <a name="additional-security-attributes-for-sql-database"></a>Atributos de segurança adicionais para o banco de dados SQL

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Preventivas: avaliação de vulnerabilidade | Sim | Ver [serviço ajuda você a identificar vulnerabilidades de banco de dados de avaliação de vulnerabilidade de SQL](sql-vulnerability-assessment.md). |
| Preventivas: descoberta de dados e a classificação  | Sim | Ver [descoberta de dados do banco de dados SQL e SQL Data Warehouse e classificação](sql-database-data-discovery-and-classification.md). |
| Detecção: detecção de ameaças | Sim | Ver [proteção avançada contra ameaças do banco de dados SQL do Azure](sql-database-threat-detection-overview.md). |
