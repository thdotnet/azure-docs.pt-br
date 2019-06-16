---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 05/28/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 9f7f9b1504533e0ec503e97c086c380da71671a0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66299699"
---
| Resource | Limite máximo/padrão |
| --- | --- |
| Circuitos do ExpressRoute por assinatura |10 |
| Circuitos do ExpressRoute por região por assinatura, com o Azure Resource Manager |10 |
| Número máximo de rotas anunciadas para o emparelhamento privado do Azure com o padrão do ExpressRoute |4\.000 |
| Número máximo de rotas anunciadas para o emparelhamento privado do Azure com o complemento ExpressRoute Premium |10\.000 |
| Número máximo de rotas anunciadas do emparelhamento privado do Azure do espaço de endereço de rede virtual para uma conexão de ExpressRoute |200 |
| Número máximo de rotas anunciadas para emparelhamento da Microsoft com o padrão do ExpressRoute |200 |
| Número máximo de rotas anunciadas para emparelhamento da Microsoft com o complemento ExpressRoute Premium |200 |
| Número máximo de circuitos do ExpressRoute vinculado à mesma rede virtual no mesmo local de emparelhamento |4 |
| Número máximo de circuitos do ExpressRoute vinculados à mesma rede virtual em diferentes locais de emparelhamento |4 |
| Número de links de rede virtual permitido por circuito do ExpressRoute |Consulte a [número de redes virtuais por circuito de ExpressRoute](#vnetpercircuit) tabela.  |

#### <a name="vnetpercircuit"></a> Número de redes virtuais por circuito do ExpressRoute
| **Tamanho do circuito** | **Número de links de rede virtual para padrão** | **Número de links de rede virtual com o complemento Premium** |
| --- | --- | --- |
| 50 Mbps |10 |20 |
| 100 Mbps |10 |25 |
| 200 Mbps |10 |25 |
| 500 Mbps |10 |40 |
| 1 Gbps |10 |50 |
| 2 Gbps |10 |60 |
| 5 Gbps |10 |75 |
| 10 Gbps |10 |100 |
| 40 Gbps * |10 |100 |
| Gbps de 100 * |10 |100 |

\* Somente diretos do ExpressRoute
