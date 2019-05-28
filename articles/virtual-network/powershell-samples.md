---
title: Exemplos de Azure PowerShell para Rede Virtual | Microsoft Docs
description: Exemplos de Azure PowerShell para rede virtual.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: ''
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 04/17/2019
ms.author: kumud
ms.openlocfilehash: e10f7ad4ae8995d9efffec97a37dc64b28fa37b0
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64688033"
---
# <a name="azure-powershell-samples-for-virtual-network"></a>Exemplos de Azure PowerShell para rede virtual

A tabela a seguir inclui links para scripts de Azure PowerShell:

| | |
|----|----|
| [Criar uma rede virtual para aplicativos de várias camadas](./scripts/virtual-network-powershell-sample-multi-tier-application.md) | Cria uma rede virtual com sub-redes de front-end e back-end. O tráfego para a sub-rede de front-end é limitado a HTTP, enquanto o tráfego para a sub-rede de back-end é limitado a SQL, porta 1433. |
| [Emparelhar duas redes virtuais](./scripts/virtual-network-powershell-sample-peer-two-virtual-networks.md) | Cria e conecta duas redes virtuais na mesma região. |
| [Rotear o tráfego por meio de uma solução de virtualização de rede](./scripts/virtual-network-powershell-sample-route-traffic-through-nva.md) | Cria uma rede virtual com sub-redes de front-end e back-end e uma VM capaz de rotear o tráfego entre as duas sub-redes. |
| [Filtrar o tráfego de entrada e saída de rede da VM](./scripts/virtual-network-powershell-sample-filter-network-traffic.md) | Cria uma rede virtual com sub-redes de front-end e back-end. O tráfego de rede de entrada para a sub-rede de front-end é limitado a HTTP e HTTPS. O tráfego de saída para a Internet da sub-rede de back-end não é permitido. |
|[Configurar rede virtual de pilha dual de IPv4 + IPv6](./scripts/virtual-network-powershell-sample-ipv6-dual-stack.md)|Implanta a rede virtual de pilha dual (IPv4 + IPv6) com duas VMs e um Azure Load Balancer básico com endereços IP públicos do IPv4 e IPv6. |
