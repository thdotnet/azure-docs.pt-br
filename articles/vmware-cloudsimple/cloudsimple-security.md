---
title: Segurança para serviços CloudSimple
description: Descreve os modelos de responsabilidade compartilhada para a segurança dos serviços de CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 4/27/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f5f3fe32e03a9a2bb0186854a83917f8918c6647
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358117"
---
# <a name="cloudsimple-security-overview"></a>Visão geral de segurança CloudSimple

Este artigo fornece uma visão geral de como a segurança é implementada no serviço CloudSimple, infraestrutura e datacenter.  Você aprenderá sobre a proteção de dados e segurança, segurança de rede e como vulnerabilidades e patches são gerenciados.

## <a name="shared-responsibility"></a>Responsabilidade compartilhada

Solução de VMware do Azure por CloudSimple usa um modelo de responsabilidade compartilhada para a segurança. Segurança confiável na nuvem é obtida por meio das responsabilidades compartilhadas de clientes e a Microsoft como um provedor de serviços. Esta matriz de responsabilidade fornece maior segurança e elimina pontos únicos de falha. 

## <a name="azure-infrastructure"></a>Infraestrutura do Azure 

Considerações de segurança de infraestrutura do Azure incluem o local de data centers e equipamento. 

### <a name="datacenter-security"></a>Segurança de Datacenter 

A Microsoft tem uma divisão inteira dedicada a projetar, compilar e operar as instalações físicas com suporte do Azure. Essa equipe está investida em manter a segurança física de última geração. Para obter detalhes sobre a segurança física, consulte [recursos do Azure, o local e a segurança física](https://docs.microsoft.com/azure/security/azure-physical-security).

### <a name="equipment-location"></a>Localização do equipamento

O equipamento de hardware bare metal que executa suas nuvens privadas é hospedado em locais de datacenter do Azure.  Os compartimentos onde é este equipamento, requer uma autenticação biométrica de dois fatores com base para obter acesso.

## <a name="dedicated-hardware"></a>Hardware dedicado

Como parte do serviço CloudSimple, todos os clientes CloudSimple obtém hosts dedicados do bare metal com discos anexados locais que estão fisicamente isolados de outros hardwares de locatário. Um hipervisor de ESXi com vSAN é executado em cada nó. Os nós são gerenciados por meio de cliente dedicado VMware vCenter e NSX. Compartilhar o hardware entre locatários não fornece uma camada adicional de proteção de segurança e isolamento.

## <a name="data-security"></a>Segurança de dados

Clientes mantêm o controle e a propriedade de seus dados. Administração de dados dos dados do cliente é de responsabilidade do cliente.

### <a name="data-protection-for-data-at-rest-and-data-in-motion-within-internal-networks"></a>Proteção de dados para dados em repouso e dados em movimento em redes internas

Para dados em repouso no ambiente de nuvem privada, você pode usar criptografia vSAN. criptografia de vSAN funciona com o VMware certificada servidores de gerenciamento de chaves externas (KMS) em sua própria rede virtual ou no local.  Você controla as chaves de criptografia de dados por conta própria. Para dados em movimento dentro da nuvem privada, o vSphere dá suporte à criptografia de dados durante a transmissão para todo o tráfego vmkernel (incluindo o tráfego de vMotion).

### <a name="data-protection-for-data-that-is-required-to-move-through-public-networks"></a>Proteção de dados para dados que são necessários para se mover pelas redes públicas

Para proteger os dados que se movimentam através de redes públicas, você pode criar IPsec e SSL VPN túneis para suas nuvens privadas. Métodos de criptografia comum têm suporte, incluindo AES 128 e 256 bytes. Dados em trânsito (incluindo autenticação, acesso administrativo e os dados do cliente) são criptografados com mecanismos de criptografia padrão (SSH, TLS 1.2 e proteger RDP). A comunicação que transporta informações confidenciais utiliza os mecanismos de criptografia padrão.

### <a name="secure-disposal"></a>Eliminação segura 

Se seu serviço CloudSimple expira ou for encerrado, você é responsável por remover ou excluir seus dados. CloudSimple será cooperar com você para excluir ou retornar todos os dados do cliente, conforme fornecido no contrato do cliente, exceto para a extensão CloudSimple é exigido pela lei aplicável, para manter alguns ou todos os dados pessoais. Se necessário, para manter todos os dados pessoais, o CloudSimple arquivar os dados e implementar medidas razoáveis para impedir que os dados do cliente nenhum processamento adicional.

### <a name="data-location"></a>Local dos dados

Durante a configuração de suas nuvens privadas, você pode escolher a região do Azure, onde serão implantados. Dados da máquina virtual VMware não são movidos de naquele datacenter físico, a menos que você executar a migração de dados ou backup de dados externo. Você também pode hospedar cargas de trabalho e armazenar dados em várias regiões do Azure, se apropriado para suas necessidades.

Os dados do cliente que é residentes em nós hiperconvergentes de nuvem privada não atravessam locais sem ação explícita do administrador de locatários. É sua responsabilidade implementar suas cargas de trabalho de maneira altamente disponível.

### <a name="data-backups"></a>Backups de dados
CloudSimple não fazer backup ou arquivar os dados do cliente. CloudSimple executar backup periódico do vCenter e dados NSX para fornecer alta disponibilidade de servidores de gerenciamento. Antes do backup, todos os dados são criptografados em fonte de vCenter usando APIs do VMware. Os dados criptografados são transportados e armazenados em BLOBs do Azure. Chaves de criptografia para backups são armazenadas em um cofre de gerenciado CloudSimple altamente seguro em execução na rede virtual CloudSimple no Azure.

## <a name="network-security"></a>Segurança de rede

A solução CloudSimple se baseia em camadas de segurança de rede.

### <a name="azure-edge-security"></a>Segurança de borda do Azure

Os serviços CloudSimple são criados sobre a segurança da rede base fornecida pelo Azure. Azure aplica técnicas de defesa em profundidade para detecção e resposta em tempo hábil a ataques de rede associado à entrada anormal ou padrões de tráfego de saída e ataques de (DDoS) de negação de serviço distribuído. Esse controle de segurança se aplica a ambientes de nuvem privada e o software de plano de controle desenvolvidos pela CloudSimple.

### <a name="segmentation"></a>Segmentação

O serviço de CloudSimple tem redes de camada 2 logicamente separadas que restringem o acesso a suas próprias redes privadas em seu ambiente de nuvem privada. Você pode proteger ainda mais suas redes de nuvem privada usando um firewall. O Portal de CloudSimple permite que você defina OVA e NS tráfego controles regras de rede para todo o tráfego de rede, incluindo dentro do próprio tráfego de nuvem privada, o tráfego de nuvem privada entre, o tráfego geral para a Internet e tráfego de rede para o local por VPN IPsec ou Conexão de ExpressRoute.

## <a name="vulnerability-and-patch-management"></a>Vulnerabilidade e gerenciamento de patches 

CloudSimple é responsável pela aplicação de patch de segurança periódica de softwares gerenciados de VMware (ESXi vCenter e NSX).

## <a name="identity-and-access-management"></a>Gerenciamento de identidade e acesso

Os clientes podem autenticar em sua conta do Azure (no Azure AD) usando a autenticação multifator ou SSO como preferencial. No portal do Azure, você pode iniciar o Portal de CloudSimple sem precisar reinserir as credenciais.

CloudSimple dá suporte a configuração opcional de uma fonte de identidade para o vCenter de nuvem privada. Você pode usar um [fonte de identidade local](https://docs.azure.cloudsimple.com/set-vcenter-identity), uma nova fonte de identidade para a nuvem privada, ou [do Azure AD](https://docs.azure.cloudsimple.com/azure-ad).

Por padrão, os clientes recebem os privilégios necessários para as operações diárias do vCenter dentro da nuvem privada. Esse nível de permissão não inclui o acesso administrativo ao vCenter. Se for necessário acesso administrativo, você poderá [escalonar os privilégios](https://docs.azure.cloudsimple.com/escalate-private-cloud-privileges) por um período limitado, enquanto você concluir as tarefas administrativas.

## <a name="next-steps"></a>Próximas etapas

* Saiba como [criar um serviço CloudSimple no Azure](quickstart-create-cloudsimple-service.md)
* Saiba como [criar uma nuvem privada](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Saiba como [configurar um ambiente de nuvem privada](quickstart-create-private-cloud.md)
