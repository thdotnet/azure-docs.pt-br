---
title: Atributos de segurança para o serviço de aplicativo do Azure
description: Uma lista de verificação de atributos de segurança para avaliar o serviço de aplicativo do Azure
services: app-service
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: mbaldwin
ms.openlocfilehash: d22fca27943be7ac7db8b8edd5882b9fa4ab3ab9
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65607252"
---
# <a name="security-attributes-for-azure-app-service"></a>Atributos de segurança para o serviço de aplicativo do Azure

Este artigo documenta os atributos comuns de segurança integrados ao serviço de aplicativo do Azure.

[!INCLUDE [Security attributes header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso (por exemplo, criptografia do lado do servidor, a criptografia do lado do servidor com chaves gerenciadas pelo cliente e outros recursos de criptografia) | Sim | Conteúdo do arquivo de site é armazenado no armazenamento do Azure, que criptografa automaticamente o conteúdo em repouso. Ver [criptografia do armazenamento do Azure para dados em repouso](../storage/common/storage-service-encryption.md).<br><br>Os segredos do cliente fornecido são criptografados em repouso. Os segredos sejam criptografados em repouso enquanto armazenados em bancos de dados de configuração de serviço de aplicativo.<br><br>Discos conectados localmente, opcionalmente, podem ser usados como armazenamento temporário por sites (D:\local e % TMP %). Discos conectados localmente não são criptografados em repouso. |
| Criptografia em trânsito (como criptografia de ExpressRoute, em criptografia de rede virtual e a criptografia de rede virtual-VNet)| Sim | Os clientes podem configurar os sites da web para exigir e usar HTTPS para tráfego de entrada. Confira a postagem [como fazer um HTTPS de serviço de aplicativo do Azure apenas](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/). |
| Manipulação de chaves de criptografia (CMK, BYOK, etc.)| Sim | Os clientes podem optar por armazenar segredos do aplicativo no cofre de chaves e recuperá-los em tempo de execução. Ver [faz referência a usar o Key Vault para o serviço de aplicativo e o Azure Functions (visualização)](app-service-key-vault-references.md).|
| Criptografia de nível de coluna (Serviços de Dados do Azure)| N/D | |
| Chamadas à API criptografadas| Sim | As chamadas de gerenciamento para configurar o serviço de aplicativo ocorrem por meio [do Azure Resource Manager](../azure-resource-manager/index.yml) chamadas via HTTPS. |

## <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte para ponto de extremidade de serviço| Sim | Disponível atualmente em versão prévia para o serviço de aplicativo. Ver [restrições de acesso do serviço de aplicativo do Azure](app-service-ip-restrictions.md). |
| Suporte à injeção de VNET| Sim | Ambientes do serviço de aplicativo são implementações particulares do serviço de aplicativo dedicado a um único cliente injetado em uma rede virtual do cliente. Ver [Introdução aos ambientes de serviço de aplicativo](environment/intro.md). |
| Suporte ao isolamento de rede e Firewalling| Sim | A variação de multilocatário público do serviço de aplicativo, os clientes podem configurar ACLs (restrições de IP) para bloquear o tráfego de entrada permitida de rede.  Ver [restrições de acesso do serviço de aplicativo do Azure](app-service-ip-restrictions.md).  Ambientes de serviço de aplicativo é implantado diretamente em redes virtuais e, portanto, podem ser protegido com NSGs. |
| Suporte para túnel forçado | Sim | Ambientes de serviço de aplicativo podem ser implantado em uma rede virtual do cliente em que o túnel forçado é configurado. Os clientes precisam seguir as instruções em [configurar seu ambiente de serviço de aplicativo com o túnel forçado](environment/forced-tunnel-support.md). |

## <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | O serviço de aplicativo se integra com o Application Insights para linguagens que dão suporte ao Application Insights (.NET Framework completo, .NET Core, Java e Node. js).  Ver [desempenho do serviço de aplicativo do Azure Monitor](../azure-monitor/app/azure-web-apps.md). O serviço de aplicativo também envia as métricas de aplicativo para o Azure Monitor. Ver [monitorar aplicativos no serviço de aplicativo do Azure](web-sites-monitor.md). |

## <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Authentication| Sim | Os clientes podem criar aplicativos no serviço de aplicativo que se integram automaticamente [Azure Active Directory (AD do Azure)](../active-directory/index.md) , bem como outros provedores de identidade compatível de OAuth; consulte [autenticação e autorização no O serviço de aplicativo do Azure](overview-authentication-authorization.md). Para acesso de gerenciamento aos ativos do serviço de aplicativo, todo o acesso é controlado por uma combinação de entidade do Azure AD autenticado e funções de RBAC do Azure Resource Manager. |
| Autorização| Sim | Para acesso de gerenciamento aos ativos do serviço de aplicativo, todo o acesso é controlado por uma combinação de entidade do Azure AD autenticado e funções de RBAC do Azure Resource Manager.  |


## <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Registro em log e auditoria de plano de controle/gerenciamento| Sim | Todas as operações de gerenciamento executadas em objetos de serviço de aplicativo ocorrem por meio [do Azure Resource Manager](../azure-resource-manager/index.yml). Os logs históricos dessas operações estão disponíveis no portal e por meio da CLI; ver [operações de provedor de recursos do Azure Resource Manager](../role-based-access-control/resource-provider-operations.md#microsoftweb) e [log de atividades do monitor az](/cli/azure/monitor/activity-log). |
| Registro em log e auditoria de plano de dados | Não | O plano de dados para o serviço de aplicativo é um compartilhamento de arquivo remoto que contém o conteúdo do site implantado de um cliente.  Não há nenhuma auditoria do compartilhamento de arquivo remoto. |

## <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim | Para operações de gerenciamento, o estado de uma configuração de serviço de aplicativo pode ser exportado como um modelo do Azure Resource Manager e com controle de versão ao longo do tempo. Para operações de tempo de execução, os clientes podem manter várias versões diferentes de ao vivo de um aplicativo usando o recurso de slots de implantação do serviço de aplicativo. | 

