---
title: Atributos de segurança do Azure Cosmos DB
description: Uma lista de verificação de atributos de segurança para avaliar o Azure Cosmos DB
services: cosmos-db
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: mbaldwin
ms.openlocfilehash: ffeb60f5476a540e3da46a82c240b0dda9aa6be2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66480473"
---
# <a name="security-attributes-for-azure-cosmos-db"></a>Atributos de segurança do Azure Cosmos DB

Este artigo documenta os atributos de segurança comuns criados no Azure Cosmos DB.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/não | Observações |
|---|---|--|
| Criptografia em repouso (por exemplo, criptografia do lado do servidor, a criptografia do lado do servidor com chaves gerenciadas pelo cliente e outros recursos de criptografia) | Sim | Todos os bancos de dados do Cosmos DB e os backups são criptografados por padrão. ver [criptografia de dados no Azure Cosmos DB](database-encryption-at-rest.md). Não há suporte para a criptografia do lado do servidor com chaves gerenciadas pelo cliente. |
| Criptografia em trânsito (como criptografia de ExpressRoute, em criptografia de rede virtual e a criptografia de rede virtual-VNet)| Sim | Todos os dados do Azure Cosmos DB são criptografados em trânsito. |
| Manipulação de chaves de criptografia (CMK, BYOK, etc.)| Não |  |
| Criptografia de nível de coluna (Serviços de Dados do Azure)| Sim | Apenas no Premium de API de tabelas. Nem todas as APIs dão suporte a esse recurso. Consulte [Introdução ao Azure Cosmos DB: API de tabela](table-introduction.md). |
| Chamadas criptografadas à API| Sim | Todas as conexões com o Azure Cosmos DB dá suporte a HTTPS. O Azure Cosmos DB também dá suporte a conexões de TLS 1.2, mas isso ainda não é imposto. Se os clientes desativem TLS de nível inferior do seu lado, eles podem assegurar para conectar-se ao Cosmos DB.  |

## <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/não | Observações |
|---|---|--|
| Suporte para ponto de extremidade de serviço| Sim |  |
| Suporte a injeção de VNET| Sim | Com o ponto de extremidade de serviço de rede virtual, você pode configurar uma conta do Azure Cosmos DB para permitir o acesso somente de uma sub-rede específica de uma rede virtual (VNet). Você também pode combinar o acesso de rede virtual com as regras de firewall.  Ver [acesso do Azure Cosmos DB das redes virtuais](vnet-service-endpoint.md). |
| Suporte ao isolamento de rede e Firewalling| Sim | Com o suporte de firewall, você pode configurar sua conta do Cosmos do Azure para permitir o acesso somente de um conjunto aprovado de endereços IP, um intervalo de endereços IP e/ou serviços de nuvem. Ver [IP de configurar o firewall no Azure Cosmos DB](how-to-configure-firewall.md).|
| Suporte para túnel forçado | Sim | Pode ser configurado no lado do cliente na rede virtual onde se encontram as máquinas virtuais.   |

## <a name="detection"></a>Detecção

| Atributo de segurança | Sim/não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | Todas as solicitações são enviadas para o Azure Cosmos DB são registradas. [O monitoramento do Azure](../azure-monitor/overview.md), métricas do Azure, o log de auditoria do Azure têm suporte.  Você pode registrar informações correspondentes para solicitações de plano de dados, estatísticas de tempo de execução de consulta, o texto da consulta, as solicitações MongoDB. Você também pode configurar alertas. |

## <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/não | Observações|
|---|---|--|
| Authentication| Sim | Sim no nível da conta de banco de dados; no nível de plano de dados, o Cosmos DB usa tokens de recurso e o acesso à chave. |
| Autorização| Sim | Suporte para a conta do Cosmos do Azure com chaves mestras (primária e secundária) e tokens de recurso. Você pode obter de leitura/gravação ou acesso a dados com chaves mestras de somente leitura. Tokens de recurso permitem o acesso de tempo limitado a recursos como documentos e contêineres. |


## <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/não | Observações|
|---|---|--|
| Registro em log e auditoria de plano de controle/gerenciamento| Sim | Log de atividades do Azure para operações de nível de conta, como Firewalls, redes virtuais, IAM e chaves de acesso. |
| Registro em log e auditoria de plano de dados | Sim | Diagnóstico de monitoramento de log para operações de nível de contêiner, como cria o contêiner, provisionar a produtividade, indexação de políticas e operações CRUD nos documentos. |

## <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Não  | | 

## <a name="additional-security-attributes-for-cosmos-db"></a>Atributos de segurança adicional para o Cosmos DB

| Atributo de segurança | Sim/não | Observações|
|---|---|--|
| Cross Origin Resource Sharing (CORS) | Sim | Ver [configurar Cross-Origin Resource Sharing (CORS)](how-to-configure-cross-origin-resource-sharing.md). |
