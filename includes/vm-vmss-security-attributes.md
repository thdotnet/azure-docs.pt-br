---
author: msmbaldwin
ms.service: virtual-machines
ms.topic: include
ms.date: 07/10/2019
ms.author: mbaldwin
ms.openlocfilehash: df11493fa9663d3fcbf0a2f74a5acbead55a25fb
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800270"
---
## <a name="preventative"></a>Preventivo

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Criptografia em repouso (por exemplo, criptografia do lado do servidor, a criptografia do lado do servidor com chaves gerenciadas pelo cliente e outros recursos de criptografia) | Sim | Ver [como criptografar uma máquina de virtual do Linux no Azure](/azure/virtual-machines/linux/encrypt-disks.md) e [criptografar discos virtuais em uma VM Windows](/azure/virtual-machines/windows/encrypt-disks.md). |
| Criptografia em trânsito (como criptografia de ExpressRoute, em criptografia de rede virtual e a criptografia de rede virtual-VNet)| Sim | Oferece suporte de máquinas virtuais do Azure [ExpressRoute](/azure/expressroute) e criptografia de rede virtual. Ver [criptografia em VMs em trânsito](/azure/security/security-azure-encryption-overview.md#in-transit-encryption-in-vms). |
| Tratamento de chaves de criptografia (CMK, BYOK, etc.)| Sim | Chaves gerenciadas pelo cliente é um cenário de criptografia do Azure com suporte; ver [visão geral da criptografia do Azure](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms.md).|
| Criptografia de nível de coluna (serviços de dados do Azure)| N/D | |
| Chamadas à API criptografadas| Sim | Por meio de HTTPS e SSL. |

## <a name="network-segmentation"></a>Segmentação de rede

| Atributo de segurança | Sim/Não | Observações |
|---|---|--|
| Suporte de ponto de extremidade de serviço| Sim | |
| Suporte à injeção de rede virtual| Sim | . |
| Suporte ao isolamento de rede e Firewalling| Sim |  |
| Forçado suporte por túnel| Sim | Ver [configurar o túnel usando o modelo de implantação do Azure Resource Manager forçado](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm.md). |

## <a name="detection"></a>Detecção

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Monitoramento (Log analytics, insights de aplicativo, etc.) de suporte do Azure| Sim | Ver [monitorar e atualizar uma máquina de virtual do Linux no Azure](/azure/virtual-machines/linux/tutorial-monitoring.md) e [monitorar e atualizar uma máquina de virtual do Windows no Azure](/azure/virtual-machines/windows/tutorial-monitoring.md). |

## <a name="identity-and-access-management"></a>Gerenciamento de identidade e de acesso

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Autenticação| Sim |  |
| Autorização| Sim |  |


## <a name="audit-trail"></a>Trilha de auditoria

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Auditoria e log de plano de controle e gerenciamento| Sim |  |
| Auditoria e log de plano de dados | Não |  |

## <a name="configuration-management"></a>Gerenciamento de configuração

| Atributo de segurança | Sim/Não | Observações|
|---|---|--|
| Suporte ao gerenciamento de configuração (controle de versão de configuração, etc.)| Sim |  | 
