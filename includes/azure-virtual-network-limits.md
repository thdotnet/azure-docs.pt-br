---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: networking
author: anavinahar
ms.service: networking
ms.topic: include
ms.date: 05/15/2019
ms.author: anavin
ms.custom: include file
ms.openlocfilehash: cb586be1a139b331c259a1197cc62e7ca4f25666
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66238383"
---
<a name="virtual-networking-limits-classic"></a>Os limites a seguir aplicam-se apenas recursos gerenciados por meio de rede a **clássico** modelo de implantação por assinatura. Saiba como [exibir seu uso de recursos atual em relação a seus limites de assinatura](../articles/networking/check-usage-against-limits.md).

| Resource | Limite padrão | Limite máximo |
| --- | --- | --- |
| Redes virtuais |50 |100 |
| Sites de redes locais |20 |Entre em contato com o suporte. |
| Servidores DNS por rede virtual |20 |20 |
| Endereços IP privados por rede virtual |4.096 |4.096 |
| Fluxos TCP ou UDP simultâneos por NIC de uma máquina virtual ou instância de função |500.000 até 1.000.000 para duas ou mais NICs. |500.000 até 1.000.000 para duas ou mais NICs. |
| Grupos de segurança de rede (NSG) |200 |200 |
| Regras de NSG por NSG |1.000 |1.000 |
| Tabelas de rotas definidas pelo usuário |200 |200 |
| Rotas definidas pelo usuário por tabela de rotas |400 |400 |
| Endereços IP públicos (dinâmicos) |5 |Contate o suporte |
| Endereços IP públicos reservados |20 |Contate o suporte |
| VIP público por implantação |5 |Contate o suporte |
| VIP privado (balanceamento de carga interno) por implantação |1 |1 |
| Listas de controle de acesso do ponto de extremidade (ACLs) |50 |50 |

#### <a name="azure-resource-manager-virtual-networking-limits"></a>Limites de rede – Azure Resource Manager
Os limites a seguir aplicam-se apenas recursos gerenciados por meio de rede **do Azure Resource Manager** por região e assinatura. Saiba como [exibir seu uso de recursos atual em relação a seus limites de assinatura](../articles/networking/check-usage-against-limits.md).

> [!NOTE]
> Recentemente, aumentamos todos os limites padrão para seus limites máximos. Se não houver nenhuma coluna de limite máximo, o recurso não tem limites ajustáveis. Se você tivesse esses limites aumentados pelo suporte no passado e não vir os limites atualizados nas tabelas a seguir, [abrir uma solicitação de suporte do cliente online sem custo adicional](../articles/azure-resource-manager/resource-manager-quota-errors.md)

| Resource | Limite máximo/padrão | 
| --- | --- |
| Redes virtuais |1.000 |
| Sub-redes por rede virtual |3.000 |
| Emparelhamentos de rede virtual por rede virtual |500 |
| Servidores DNS por rede virtual |20 |
| Endereços IP privados por rede virtual |65,536 |
| Endereços IP privados por interface de rede |256 |
| Endereços IP privados por máquina virtual |256 |
| Fluxos TCP ou UDP simultâneos por NIC de uma máquina virtual ou instância de função |500,000 |
| Placas de interface de rede |65,536 |
| Grupos de segurança de rede |5.000 |
| Regras de NSG por NSG |1.000 |
| Endereços e intervalos IP especificados para a origem ou o destino em um grupo de segurança |4.000 |
| Grupos de segurança do aplicativo |3.000 |
| Grupos de segurança do aplicativo por configuração de IP, por NIC |20 |
| Configurações de IP por grupo de segurança do aplicativo |4.000 |
| Os grupos de segurança do aplicativo que podem ser especificados em todas as regras de segurança de um grupo de segurança de rede |100 |
| Tabelas de rotas definidas pelo usuário |200 |
| Rotas definidas pelo usuário por tabela de rotas |400 |
| Certificados de raiz de ponto a site por Gateway de VPN do Azure |20 |
| TAPs de rede virtual |100 |
| Configurações de TAP do adaptador de rede por TAP de rede virtual |100 |

#### <a name="publicip-address"></a>Limites do endereço IP público
| Resource | Limite padrão | Limite máximo |
| --- | --- | --- |
| Endereços IP públicos - dinâmicos | 1.000 para Basic. |Entre em contato com o suporte. |
| Endereços IP públicos - estáticos | 1.000 para Basic. |Entre em contato com o suporte. |
| Endereços IP públicos - estáticos | 200 para Standard.|Entre em contato com o suporte. |
| Tamanho de prefixo IP público | / 28 | Entre em contato com o suporte. |

#### <a name="load-balancer"></a>Limites do balanceador de carga
Os limites a seguir aplicam-se apenas a recursos de rede gerenciados pelo Azure Resource Manager por região e assinatura. Saiba como [exibir seu uso de recursos atual em relação a seus limites de assinatura](../articles/networking/check-usage-against-limits.md).

| Resource | Limite máximo/padrão |
| --- | --- |
| Balanceadores de carga | 1.000 | 
| Regras por recurso, Básico | 250 |
| Regras por recurso, Standard | 1.500 | 
| Regras por configuração de IP | 299 |
| Regras por NIC | 500 |
| Configurações de IP de front-end, básico | 200 |
| Configurações de IP de front-end, Standard | 600 |
| Pool de back-end, básico | 100, único conjunto de disponibilidade |
| Pool de back-end, Standard | 1.000, única rede virtual |
| Recursos de back-end por balanceador de carga Standard<sup>1</sup> | 150 |
| Portas de alta disponibilidade, Standard | 1 por interno front-end |

<sup>1</sup>o limite é de até 150 recursos, em qualquer combinação de recursos da máquina virtual autônoma, o conjunto de disponibilidade, recursos e recursos do conjunto de dimensionamento de máquina virtual.

