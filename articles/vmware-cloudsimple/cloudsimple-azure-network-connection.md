---
title: Solução do VMware CloudSimple - conexões de rede do Azure
description: Saiba mais sobre como conectar sua rede virtual do Azure à sua rede de região CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f2ab82b6c1b4b373c186019eaf96f9864861b9d9
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66497614"
---
# <a name="azure-network-connections-overview"></a>Visão geral de conexões de rede do Azure

Quando você cria um serviço CloudSimple em uma região, ele:

* Cria um circuito de ExpressRoute do Azure e a anexa ao serviço nessa região
* Permite que a conexão de sua rede de região CloudSimple para sua rede virtual do Azure ou sua rede local usando o Azure ExpressRoute
* Fornece serviços de acesso em execução em sua assinatura do Azure ou sua rede local, do seu ambiente de nuvem privada

A conexão é:

* Segurança
* Privado
* Largura de banda alta
* Baixa latência

## <a name="benefits"></a>Benefícios

Conexão de rede do Azure permite que você:

* Use o Azure como um destino de backup para máquinas virtuais em sua nuvem privada.
* Implante servidores KMS em sua assinatura do Azure para criptografar seu armazenamento de dados de vSAN de nuvem privada.
* Use aplicativos híbridos em que a camada da web do aplicativo é executado na nuvem pública, enquanto o aplicativo e as camadas de banco de dados executado em sua nuvem privada.

## <a name="azure-virtual-network-connection"></a>Conexão de rede virtual do Azure

Nuvens privadas podem ser conectados aos recursos do Azure usando o ExpressRoute.  Você pode usar essa conexão para acessar recursos diferentes em execução em sua assinatura do Azure da sua nuvem privada.  Essa conexão permite que você estenda a você a rede de nuvem privada em sua rede virtual do Azure.

![Conexão de ExpressRoute do Azure para rede virtual](media/cloudsimple-azure-network-connection.png)

## <a name="expressroute-connection-to-on-premises-network"></a>Conexão de ExpressRoute à rede local

Você pode conectar o circuito de ExpressRoute do Azure existente para sua região CloudSimple. O recurso de alcance Global do ExpressRoute é usado para se conectar os dois circuitos uns com os outros.  Uma conexão é estabelecida entre o local e os circuitos do CloudSimple ExpressRoute.  Essa conexão permite que você estenda suas redes locais para a rede de nuvem privada.

![Conexão de ExpressRoute do local - alcance Global](media/cloudsimple-global-reach-connection.png)

## <a name="next-steps"></a>Próximas etapas

* [Obter informações sobre o emparelhamento de rede virtual do Azure para conexão CloudSimple](https://docs.azure.cloudsimple.com/virtual-network-connection)
* [Conectar-se do local para CloudSimple usando o ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection)
