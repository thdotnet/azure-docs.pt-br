---
title: Atributos de segurança para serviços do Azure
description: Uma lista de verificação de atributos de segurança comuns para avaliar o Microsoft Azure Service Fabric
services: security
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: security
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: mbaldwin
ms.openlocfilehash: 460d8756c437a1212aef054cf069be2bccac8c8a
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66001356"
---
# <a name="security-attributes-for-azure-services"></a>Atributos de segurança para serviços do Azure

Este artigo coleta os atributos de segurança comuns para serviços do Azure selecionados. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="azure-api-managementapi-managementapi-management-security-attributesmd"></a>[Gerenciamento de API do Azure](../api-management/api-management-security-attributes.md)

### <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso:<ul><li>Criptografia no servidor</li><li>Criptografia do lado do servidor com chaves gerenciadas pelo cliente</li><li>Outros recursos de criptografia (como do lado do cliente, Always Encrypted, etc.)</ul>| Sim (somente para a criptografia no lado do serviço) | Dados confidenciais, como certificados, chaves e valores chamado de segredo são criptografados com serviços gerenciados, por chaves de instância de serviço. |
| Criptografia em trânsito:<ul><li>Criptografia do ExpressRoute</li><li>Na criptografia de rede virtual</li><li>Criptografia de rede virtual a rede virtual</ul>| Sim | [Rota expressa](../expressroute/index.yml) e criptografia de rede virtual é fornecida pela [a rede do Azure](../virtual-network/index.yml). |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| Não | Todas as chaves de criptografia são por instância de serviço e serviço gerenciado. |
| Criptografia de nível de coluna (serviços de dados do Azure)| N/D | |
| Chamadas à API criptografadas| Sim | Chamadas de plano de gerenciamento são feitas por meio [do Azure Resource Manager](../azure-resource-manager/index.yml) via TLS. Um JWT (token Web JSON) válido é necessário.  Chamadas de plano de dados podem ser protegidas com TLS e um dos mecanismos de autenticação com suporte (por exemplo, o certificado do cliente ou o JWT).
 |

### <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| Não | |
| Suporte à injeção de rede virtual| Sim | |
| Isolamento de rede e suporte de firewall| Sim | Usando grupos de segurança de rede (NSG) e o Gateway de aplicativo do Azure (ou outro dispositivo de software), respectivamente. |
| Forçado suporte por túnel| Sim | A rede do Azure fornece túnel forçado. |

### <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | |

### <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Authentication| Sim | |
| Autorização| Sim | |


### <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Auditoria e log de plano de controle e gerenciamento| Sim | [Logs de atividade do Azure Monitor](../azure-monitor/platform/activity-logs-overview.md) |
| Auditoria e log de plano de dados| Sim | [Logs de diagnóstico do Azure Monitor](../azure-monitor/platform/diagnostic-logs-overview.md) e (opcionalmente) [do Azure Application Insights](../azure-monitor/app/app-insights-overview.md).  |

### <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim | Usando o [Kit de recursos de DevOps de gerenciamento de API do Azure](https://aka.ms/apimdevops) |


## <a name="azure-app-serviceapp-serviceapp-service-security-attributesmd"></a>[Serviço de Aplicativo do Azure](../app-service/app-service-security-attributes.md)

### <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso (por exemplo, criptografia do lado do servidor, a criptografia do lado do servidor com chaves gerenciadas pelo cliente e outros recursos de criptografia) | Sim | Conteúdo do arquivo de site é armazenado no armazenamento do Azure, que criptografa automaticamente o conteúdo em repouso. Ver [criptografia do armazenamento do Azure para dados em repouso](../storage/common/storage-service-encryption.md).<br><br>Os segredos do cliente fornecido são criptografados em repouso. Os segredos sejam criptografados em repouso enquanto armazenados em bancos de dados de configuração de serviço de aplicativo.<br><br>Discos conectados localmente, opcionalmente, podem ser usados como armazenamento temporário por sites (D:\local e % TMP %). Discos conectados localmente não são criptografados em repouso. |
| Criptografia em trânsito (como criptografia de ExpressRoute, em criptografia de rede virtual e a criptografia de rede virtual-VNet)| Sim | Os clientes podem configurar os sites da web para exigir e usar HTTPS para tráfego de entrada. Confira a postagem [como fazer um HTTPS de serviço de aplicativo do Azure apenas](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/). |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| Sim | Os clientes podem optar por armazenar segredos do aplicativo no cofre de chaves e recuperá-los em tempo de execução. Ver [faz referência a usar o Key Vault para o serviço de aplicativo e o Azure Functions (visualização)](../app-service/app-service-key-vault-references.md).|
| Criptografia de nível de coluna (serviços de dados do Azure)| N/D | |
| Chamadas à API criptografadas| Sim | As chamadas de gerenciamento para configurar o serviço de aplicativo ocorrem por meio [do Azure Resource Manager](../azure-resource-manager/index.yml) chamadas via HTTPS. |

### <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| Sim | Disponível atualmente em versão prévia para o serviço de aplicativo. Ver [restrições de acesso do serviço de aplicativo do Azure](../app-service/app-service-ip-restrictions.md). |
| Suporte à injeção de rede virtual| Sim | Ambientes do serviço de aplicativo são implementações particulares do serviço de aplicativo dedicado a um único cliente injetado em uma rede virtual do cliente. Ver [Introdução aos ambientes de serviço de aplicativo](../app-service/environment/intro.md). |
| Suporte ao isolamento de rede e Firewalling| Sim | A variação de multilocatário público do serviço de aplicativo, os clientes podem configurar ACLs (restrições de IP) para bloquear o tráfego de entrada permitida de rede.  Ver [restrições de acesso do serviço de aplicativo do Azure](../app-service/app-service-ip-restrictions.md).  Ambientes de serviço de aplicativo é implantado diretamente em redes virtuais e, portanto, podem ser protegido com NSGs. |
| Forçado suporte por túnel| Sim | Ambientes de serviço de aplicativo podem ser implantado em uma rede virtual do cliente em que o túnel forçado é configurado. Os clientes precisam seguir as instruções em [configurar seu ambiente de serviço de aplicativo com o túnel forçado](../app-service/environment/forced-tunnel-support.md). |

### <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | O serviço de aplicativo se integra com o Application Insights para linguagens que dão suporte ao Application Insights (.NET Framework completo, .NET Core, Java e Node. js).  Ver [desempenho do serviço de aplicativo do Azure Monitor](../azure-monitor/app/azure-web-apps.md). O serviço de aplicativo também envia as métricas de aplicativo para o Azure Monitor. Ver [monitorar aplicativos no serviço de aplicativo do Azure](../app-service/web-sites-monitor.md). |

### <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Authentication| Sim | Os clientes podem criar aplicativos no serviço de aplicativo que se integram automaticamente [Azure Active Directory (AD do Azure)](../active-directory/index.md) , bem como outros provedores de identidade compatível de OAuth; consulte [autenticação e autorização no O serviço de aplicativo do Azure](../app-service/overview-authentication-authorization.md). Para acesso de gerenciamento aos ativos do serviço de aplicativo, todo o acesso é controlado por uma combinação de entidade do Azure AD autenticado e funções de RBAC do Azure Resource Manager. |
| Autorização| Sim | Para acesso de gerenciamento aos ativos do serviço de aplicativo, todo o acesso é controlado por uma combinação de entidade do Azure AD autenticado e funções de RBAC do Azure Resource Manager.  |


### <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Auditoria e log de plano de controle e gerenciamento| Sim | Todas as operações de gerenciamento executadas em objetos de serviço de aplicativo ocorrem por meio [do Azure Resource Manager](../azure-resource-manager/index.yml). Os logs históricos dessas operações estão disponíveis no portal e por meio da CLI; ver [operações de provedor de recursos do Azure Resource Manager](../role-based-access-control/resource-provider-operations.md#microsoftweb) e [log de atividades do monitor az](/cli/azure/monitor/activity-log). |
| Auditoria e log de plano de dados | Não | O plano de dados para o serviço de aplicativo é um compartilhamento de arquivo remoto que contém um cliente s implantadas conteúdo de site.  Não há nenhuma auditoria do compartilhamento de arquivo remoto. |

### <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim | Para operações de gerenciamento, o estado de uma configuração de serviço de aplicativo pode ser exportado como um modelo do Azure Resource Manager e com controle de versão ao longo do tempo. Para operações de tempo de execução, os clientes podem manter várias versões diferentes de ao vivo de um aplicativo usando o recurso de slots de implantação do serviço de aplicativo. | 


## <a name="azure-resource-managerazure-resource-managerazure-resource-manager-security-attributesmd"></a>[Azure Resource Manager](../azure-resource-manager/azure-resource-manager-security-attributes.md)

### <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso:<ul><li>Criptografia no servidor</li><li>Criptografia do lado do servidor com chaves gerenciadas pelo cliente</li><li>Outros recursos de criptografia (como do lado do cliente, Always Encrypted, etc.)</ul>| Sim |  |
| Criptografia em trânsito:<ul><li>Criptografia do ExpressRoute</li><li>Na criptografia de rede virtual</li><li>Criptografia de rede virtual a rede virtual</ul>| Sim | HTTPS/TLS. |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| N/D | O Azure Resource Manager não armazena nenhum conteúdo do cliente, apenas os dados de controle. |
| Criptografia de nível de coluna (serviços de dados do Azure)| Sim | |
| Chamadas criptografadas à API| Sim | |

### <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| Não | |
| Suporte à injeção de rede virtual| Sim | |
| Isolamento de rede e suporte de firewall| Não |  |
| Forçado suporte por túnel| Não |  |

### <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Não | |

### <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Authentication| Sim | [O Azure Active Directory](/azure/active-directory) com base.|
| Autorização| Sim | |


### <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Auditoria e log de plano de controle e gerenciamento| Sim | Exponha todas as operações (PUT, POST, DELETE) realizadas em seus recursos; de gravação de logs de atividades ver [exibir logs de atividades para auditar ações em recursos](../azure-resource-manager/resource-group-audit.md). |
| Auditoria e log de plano de dados| N/D | |

### <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim |  |


## <a name="azure-backupbackupbackup-security-attributesmd"></a>[Serviço de Backup do Azure](../backup/backup-security-attributes.md)

### <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso:<ul><li>Criptografia no servidor</li><li>Criptografia do lado do servidor com chaves gerenciadas pelo cliente</li><li>Outros recursos de criptografia (como do lado do cliente, Always Encrypted, etc.)</ul>| Sim | Usar a criptografia do serviço de armazenamento para contas de armazenamento. |
| Criptografia em trânsito:<ul><li>Criptografia do ExpressRoute</li><li>Na criptografia de rede virtual</li><li>Criptografia de rede virtual a rede virtual</ul>| Não | Usar HTTPS. |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| Não |  |
| Criptografia de nível de coluna (serviços de dados do Azure)| Não |  |
| Chamadas criptografadas à API| Sim |  |

### <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| Não |  |
| Suporte à injeção de rede virtual| Não |  |
| Isolamento de rede e suporte de firewall| Sim | Há suporte para túnel forçado no backup de VM. Não há suporte para túnel forçado em cargas de trabalho em execução dentro de VMs. |
| Forçado suporte por túnel| Não |  |

### <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | Há suporte para o Log Analytics por meio dos logs de diagnóstico. Confira Monitorar cargas de trabalho protegidas do Backup do Azure usando o Log Analytics (https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) para obter mais informações. |

### <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Authentication| Sim | A autenticação é feita pelo Azure Active Directory. |
| Autorização| Sim | São usadas regras de RBAC internas e criadas pelo cliente. Consulte controle de acesso baseado em função para gerenciar pontos de recuperação de Backup do Azure (/ azure/backup/rbac-rs-Cofre de backup) para obter mais informações. |


### <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Auditoria e log de plano de controle e gerenciamento| Sim | Todas as ações disparadas pelo cliente no portal do Azure são registradas em logs de atividades. |
| Auditoria e log de plano de dados| Não | O plano de dados do Backup do Azure não pode ser acessado diretamente.  |

### <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim|  |


## <a name="azure-key-vaultkey-vaultkey-vault-security-attributesmd"></a>[Cofre da Chave do Azure](../key-vault/key-vault-security-attributes.md)

### <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso:<ul><li>Criptografia no servidor</li><li>Criptografia do lado do servidor com chaves gerenciadas pelo cliente</li><li>Outros recursos de criptografia (como do lado do cliente, Always Encrypted, etc.)</ul>| Sim | Todos os objetos são criptografados. |
| Criptografia em trânsito:<ul><li>Criptografia do ExpressRoute</li><li>Na criptografia de rede virtual</li><li>Criptografia de rede virtual a rede virtual</ul>| Sim | Toda a comunicação é por meio de chamadas à API criptografadas |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| Sim | O cliente controla todas as chaves em seu Key Vault. Quando as chaves de HSM (módulo) com suporte de segurança de hardware são especificadas, um HSM do FIPS nível 2 protege a chave, o certificado ou o segredo. |
| Criptografia de nível de coluna (serviços de dados do Azure)| N/D |  |
| Chamadas à API criptografadas| Sim | Usar HTTPS. |

### <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| Sim | Usando pontos de extremidade de serviço de rede Virtual (VNet). |
| Suporte à injeção de rede virtual| Não |  |
| Isolamento de rede e suporte de firewall| Sim | Usando regras de firewall de rede virtual. |
| Forçado suporte por túnel| Não |  |

### <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | Usando o Log Analytics. |

### <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Authentication| Sim | A autenticação é feita pelo Azure Active Directory. |
| Autorização| Sim | Usando a política de acesso ao Key Vault. |


### <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Registro em log e auditoria de plano de controle/gerenciamento| Sim | Usando o Log Analytics. |
| Auditoria e log de plano de dados| Sim | Usando o Log Analytics. |

### <a name="access-controls"></a>Controles de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Controles de acesso do plano de controle/gerenciamento | Sim | RBAC (Controle de Acesso Baseado em Função) do Azure Resource Manager |
| Controles de acesso do plano de dados (em cada nível de serviço) | Sim | Política de acesso ao Key Vault |

## <a name="azure-service-bus-messagingservice-bus-messagingservice-bus-messaging-security-attributesmd"></a>[Azure mensagens do barramento de serviço](../service-bus-messaging/service-bus-messaging-security-attributes.md)

### <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso:<ul><li>Criptografia no servidor</li><li>Criptografia do lado do servidor com chaves gerenciadas pelo cliente</li><li>Outros recursos de criptografia (como do lado do cliente, Always Encrypted, etc.)</ul>|  Sim para criptografia em repouso do lado do servidor por padrão. | Chaves gerenciadas pelo cliente e o BYOK ainda não têm suporte. Criptografia do cliente é responsabilidade do cliente |
| Criptografia em trânsito:<ul><li>Criptografia do ExpressRoute</li><li>Na criptografia de rede virtual</li><li>Criptografia de rede virtual a rede virtual</ul>| Sim | Dá suporte ao mecanismo HTTPS/TLS padrão. |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| Não |   |
| Criptografia de nível de coluna (serviços de dados do Azure)| N/D | |
| Chamadas à API criptografadas| Sim | Chamadas à API são feitas por meio [do Azure Resource Manager](../azure-resource-manager/index.yml) e HTTPS. |

### <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| Sim (somente para a camada Premium) | Há suporte para pontos de extremidade de serviço de rede virtual para [camada Premium do barramento de serviço](../service-bus-messaging/service-bus-premium-messaging.md) apenas. |
| Suporte à injeção de rede virtual| Não | |
| Isolamento de rede e suporte de firewall| Sim (somente para a camada Premium) |  |
| Forçado suporte por túnel| Não |  |

### <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | Com suporte via [o Azure Monitor e alertas](../service-bus-messaging/service-bus-metrics-azure-monitor.md). |

### <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Authentication| Sim | Gerenciado por meio [Azure Active Directory Managed Service Identity](../service-bus-messaging/service-bus-managed-service-identity.md); consulte [do barramento de serviço de autenticação e autorização](../service-bus-messaging/service-bus-authentication-and-authorization.md).|
| Autorização| Sim | Oferece suporte à autorização por meio [RBAC](../service-bus-messaging/service-bus-role-based-access-control.md) (versão prévia) e o token de SAS, consulte [do barramento de serviço de autenticação e autorização](../service-bus-messaging/service-bus-authentication-and-authorization.md). |

### <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Auditoria e log de plano de controle e gerenciamento| Sim | Logs de operações estão disponíveis; ver [logs de diagnóstico do barramento de serviço](../service-bus-messaging/service-bus-diagnostic-logs.md).  |
| Auditoria e log de plano de dados| Não |  |

### <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim | Dá suporte a controle de versão de provedor de recursos por meio de [API do Azure Resource Manager](/rest/api/resources/).|


## <a name="azure-service-bus-relayservice-bus-relayservice-bus-relay-security-attributesmd"></a>[Retransmissão do barramento de serviço do Azure](../service-bus-relay/service-bus-relay-security-attributes.md)

### <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso:<ul><li>Criptografia no servidor</li><li>Criptografia do lado do servidor com chaves gerenciadas pelo cliente</li><li>Outros recursos de criptografia (como do lado do cliente, Always Encrypted, etc.)</ul>|  N/D | Retransmissão é um soquete da web e não manter os dados. |
| Criptografia em trânsito:<ul><li>Criptografia do ExpressRoute</li><li>Na criptografia de rede virtual</li><li>Criptografia de rede virtual a rede virtual</ul>| Sim | O serviço exige TLS. |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| Não | Usa somente os certificados de TLS Microsoft.  |
| Criptografia de nível de coluna (serviços de dados do Azure)| N/D | |
| Chamadas à API criptografadas| Sim | HTTPS. |

### <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| Não |  |
| Isolamento de rede e suporte de firewall| Não |  |
| Forçado suporte por túnel| N/D | A retransmissão é o túnel TLS  |

### <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | |

### <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Authentication| Sim | Por meio do SAS. |
| Autorização|  Sim | Por meio do SAS. |


### <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Auditoria e log de plano de controle e gerenciamento| Sim | Por meio [o Azure Resource Manager](../azure-resource-manager/index.yml). |
| Auditoria e log de plano de dados| Sim | Êxito de Conexão / erros e falhas e registrado.  |

### <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim | Por meio [o Azure Resource Manager](../azure-resource-manager/index.yml).|


## <a name="azure-service-fabricservice-fabricservice-fabric-security-attributesmd"></a>[Azure Service Fabric](../service-fabric/service-fabric-security-attributes.md)

### <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso:<ul><li>Criptografia no servidor</li><li>Criptografia do lado do servidor com chaves gerenciadas pelo cliente</li><li>Outros recursos de criptografia (como do lado do cliente, Always Encrypted, etc.)</ul>| Sim | O cliente é proprietário do cluster e do conjunto de dimensionamento de VMs (máquinas virtuais) em que o cluster se baseia. Criptografia de disco do Azure pode ser habilitada no conjunto de dimensionamento de máquina virtual. |
| Criptografia em trânsito:<ul><li>Criptografia do ExpressRoute</li><li>Na criptografia de rede virtual</li><li>Criptografia de rede virtual a rede virtual</ul>| Sim |  |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| Sim | O cliente é proprietário do cluster e do conjunto de dimensionamento de VMs (máquinas virtuais) em que o cluster se baseia. Criptografia de disco do Azure pode ser habilitada no conjunto de dimensionamento de máquina virtual. |
| Criptografia de nível de coluna (serviços de dados do Azure)| N/D |  |
| Chamadas à API criptografadas| Sim | Chamadas à API do Service Fabric são feitas por meio do Azure Resource Manager. Um JWT (token Web JSON) válido é necessário. |

### <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| Sim |  |
| Suporte à injeção de rede virtual| Sim |  |
| Isolamento de rede e suporte de firewall| Sim | Usando NSGs (Grupos de Segurança de Rede). |
| Forçado suporte por túnel| Sim | A rede do Azure fornece túnel forçado. |

### <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | Usando o suporte de terceiros e suporte de monitoramento do Azure. |

### <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Authentication| Sim | A autenticação é feita pelo Azure Active Directory. |
| Autorização| Sim | IAM (Gerenciamento de identidade e de acesso) para chamadas via SFRP. Chamadas diretamente para o ponto de extremidade de cluster dão suporte a duas funções: Usuário e administrador. O cliente pode mapear as APIs para qualquer uma dessas duas funções. |

### <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Auditoria e log de plano de controle e gerenciamento| Sim | Todas as operações de plano de controle são executadas por meio de processos para auditoria e aprovações. |
| Auditoria e log de plano de dados| N/D | O cliente é proprietário do cluster.  |

### <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim | O controle de versão e a implantação da configuração de serviço são realizados pelo Azure Deploy. O controle de versão do código (aplicativo e tempo de execução) é realizado usando o Azure Build.
 |

## <a name="azure-sql-databasesql-databasesql-database-security-attributesmd"></a>[Banco de Dados SQL do Azure](../sql-database/sql-database-security-attributes.md)

### <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso:<ul><li>Criptografia no servidor</li><li>Criptografia do lado do servidor com chaves gerenciadas pelo cliente</li><li>Outros recursos de criptografia (como do lado do cliente, Always Encrypted, etc.)</ul>| Sim | Conhecido como "criptografia em uso", conforme descrito no artigo [Always Encrypted](../sql-database/sql-database-always-encrypted.md). Criptografia do lado do serviço usa [a transparent data encryption](../sql-database/transparent-data-encryption-azure-sql.md) (TDE).|
| Criptografia em trânsito:<ul><li>Criptografia de ExpressRoute</li><li>Na criptografia de rede virtual</li><li>Criptografia de rede virtual a rede virtual</ul>| Sim | Usar HTTPS. |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| Sim | Tratamento de chaves gerenciados pelo serviço e gerenciadas pelo cliente são oferecidos (o último por meio [Azure Key Vault](../key-vault/index.yml). |
| Criptografia de nível de coluna (serviços de dados do Azure)| Sim | Por meio [Always Encrypted](../sql-database/sql-database-always-encrypted.md). |
| Chamadas criptografadas à API| Sim | Usando HTTPS/SSL. |

### <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| Sim | Aplica-se ao [banco de dados único](../sql-database/sql-database-single-index.yml) apenas. |
| Suporte à injeção de rede virtual| Sim | Aplica-se ao [instância gerenciada](../sql-database/sql-database-managed-instance.md) apenas. |
| Isolamento de rede e suporte de firewall| Sim | Firewall em ambos os banco de dados – e nível de servidor; isolamento de rede [instância gerenciada](../sql-database/sql-database-managed-instance.md) apenas |
| Forçado suporte por túnel| Sim | [a instância gerenciada](../sql-database/sql-database-managed-instance.md) via [do Azure ExpressRoute](../expressroute/index.yml) VPN |

### <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | A solução SIEM de terceiros da Imperva (SecureSphere) também é suportada, por meio [Hubs de eventos](../event-hubs/index.yml) integração via [auditoria SQL](../sql-database/sql-database-auditing.md). |

### <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Authentication| Sim | Azure Active Directory. |
| Autorização| Sim |  |


### <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Auditoria e log de plano de controle e gerenciamento| Sim | Sim para apenas alguns eventos. |
| Auditoria e log de plano de dados | Sim | Por meio [auditoria SQL](../sql-database/sql-database-auditing.md). |

### <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Não  | | 

### <a name="additional-security-attributes-for-sql-database"></a>Atributos de segurança adicionais para o banco de dados SQL

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Preventivas: avaliação de vulnerabilidade | Sim | Ver [serviço ajuda você a identificar vulnerabilidades de banco de dados de avaliação de vulnerabilidade de SQL](../sql-database/sql-vulnerability-assessment.md). |
| Preventivas: descoberta de dados e a classificação  | Sim | Ver [descoberta de dados do banco de dados SQL e SQL Data Warehouse e classificação](../sql-database/sql-database-data-discovery-and-classification.md). |
| Detecção: detecção de ameaças | Sim | Ver [proteção avançada contra ameaças do banco de dados SQL do Azure](../sql-database/sql-database-threat-detection-overview.md). |

## <a name="azure-storagestoragecommonstorage-security-attributesmd"></a>[Armazenamento do Azure](../storage/common/storage-security-attributes.md)

### <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso:<ul><li>Criptografia no servidor</li><li>Criptografia do lado do servidor com chaves gerenciadas pelo cliente</li><li>Outros recursos de criptografia (como do lado do cliente, Always Encrypted, etc.)</ul>| Sim |  |
| Criptografia em trânsito:<ul><li>Criptografia do ExpressRoute</li><li>Na criptografia de rede virtual</li><li>Criptografia de rede virtual a rede virtual</ul>| Sim | Suporte a mecanismos HTTPS/TLS padrão.  Os usuários também podem criptografar dados antes de ser transmitido para o serviço. |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| Sim | Ver [criptografia do serviço de armazenamento usando chaves gerenciadas pelo cliente no Azure Key Vault](../storage/common/storage-service-encryption-customer-managed-keys.md).|
| Criptografia de nível de coluna (serviços de dados do Azure)| N/D |  |
| Chamadas à API criptografadas| Sim |  |

### <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| Sim |  |
| Suporte à injeção de rede virtual| N/D |  |
| Isolamento de rede e suporte de firewall| Sim | |
| Forçado suporte por túnel| N/D |  |

### <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | Métricas do Azure Monitor disponível agora, registra a visualização inicial |

### <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Authentication| Sim | Azure Active Directory, chave compartilhada, o token de acesso compartilhado. |
| Autorização| Sim | Autorização de suporte por meio de RBAC, ACLs POSIX e Tokens de SAS |


### <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Auditoria e log de plano de controle e gerenciamento | Sim | Log de atividades do Azure Resource Manager |
| Auditoria e log de plano de dados| Sim | Os Logs de diagnóstico do serviço e registro em log do Azure Monitor visualização inicial  |

### <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim | Suporte a controle de versão do provedor de recursos por meio de APIs do Gerenciador de recursos do Azure |