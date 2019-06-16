---
title: Principais conceitos para administrar a solução CloudSimple do VMware do Azure
description: Descreve os principais conceitos para administrar a solução CloudSimple do VMware do Azure
author: sharaths-cs
ms.author: b-shsury
ms.date: 4/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0d890553ee145ca6aafed5a34d158c6a34d9af36
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66358162"
---
# <a name="key-concepts-for-administration-of-azure-vmware-solution-by-cloudsimple"></a>Principais conceitos para administração de solução CloudSimple do VMware do Azure

Administrando a solução VMware do Azure CloudSimple exige noções básicas sobre os conceitos a seguir.

* CloudSimple Service (exibido como solução de VMware do Azure por CloudSimple - serviço)
* Nó CloudSimple (por CloudSimple - nó, exibido como solução de VMware do Azure)
* Nuvem privada de CloudSimple
* Serviço de rede
* Máquina de Virtual CloudSimple (exibido como solução de VMware do Azure por CloudSimple - Máquina Virtual)

## <a name="cloudsimple-service"></a>Serviço CloudSimple

O serviço CloudSimple permite que você crie e gerencie todos os recursos associados às soluções do VMware por CloudSimple do portal do Azure. Crie um recurso de serviço em todas as regiões em que você pretende usar o serviço. 

Saiba mais sobre o [CloudSimple Service](cloudsimple-service.md)

## <a name="cloudsimple-node"></a>Nó CloudSimple

Um nó de CloudSimple é um dedicado e bare-metal, o host de computação e armazenamento de hiperconvergente em que o hipervisor VMware ESXi está implantado. Esse nó, em seguida, é incorporado no VMware vSphere vCenter, vSAN e plataformas NSX. Serviços de rede CloudSimple e serviços de rede de borda também são habilitados. Cada nó serve como uma unidade de capacidade de computação e armazenamento que você pode comprar para criar [nuvens privadas CloudSimple](cloudsimple-private-cloud.md). Comprar ou reserva de nós em uma região em que o serviço de CloudSimple está disponível.


Saiba mais sobre [CloudSimple nós](cloudsimple-node.md)

## <a name="cloudsimple-private-cloud"></a>Nuvem privada de CloudSimple

Uma nuvem privada CloudSimple é um ambiente isolado de pilha VMware gerenciado por um servidor do vCenter em seu próprio domínio de gerenciamento. Pilha do VMware inclui hosts ESXi, vSphere, vCenter, vSAN e NSX.  As execuções de pilha em (dedicado e isolado hardware bare metal) de nós dedicados e é consumido por usuários por meio de ferramentas nativas do VMware que incluem o vCenter e NSX Manager. Nós dedicados são implantados em locais do Azure e são gerenciados pelo Azure. Cada nuvem privada podem ser segmentado e protegido usando serviços como VLANs/sub-redes de rede e tabelas de firewall.  Conexões com o seu ambiente local e a rede do Azure são criadas usando VPN segura e privada e as conexões do ExpressRoute do Azure.

Saiba mais sobre [nuvem privada CloudSimple](cloudsimple-private-cloud.md)

## <a name="service-networking"></a>Rede de serviço

O serviço de CloudSimple fornece uma rede por região em que o seu serviço CloudSimple é implantado. A rede é um único espaço de endereço de TCP de camada 3 com roteamento ativado por padrão. Todas as nuvens privadas e as sub-redes criadas nessa região se comunicam entre si sem qualquer configuração adicional. Você pode criar grupos de portas distribuído no vCenter usando as VLANs.  Você pode usar os seguintes recursos de rede para configurar e proteger seus recursos de carga de trabalho em sua nuvem privada.

* [VLANs/sub-redes](cloudsimple-vlans-subnets.md)
* [Tabelas de firewall](cloudsimple-firewall-tables.md)
* [Gateways de VPN](cloudsimple-vpn-gateways.md)
* [IP público](cloudsimple-public-ip-address.md)
* [Conexão de rede do Azure](cloudsimple-azure-network-connection.md)

## <a name="cloudsimple-virtual-machine"></a>Máquina de virtual CloudSimple

O serviço CloudSimple permite que você gerencie máquinas virtuais VMware no portal do Azure. Um ou mais clusters ou pools de recursos do seu ambiente do vSphere podem ser mapeados para a assinatura na qual o serviço é criado.

Saiba mais sobre:

* [Máquinas virtuais de CloudSimple](cloudsimple-virtual-machines.md)
* [Mapeamento de assinatura do Azure](https://docs.azure.cloudsimple.com/azure-subscription-mapping/)
