---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: firewall
author: vhorne
ms.service: firewall
ms.topic: include
ms.date: 5/3/2019
ms.author: victorh
ms.custom: include file
ms.openlocfilehash: 8709d4d903bd31ff94d04ec61e226857e4190407
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67172297"
---
| Resource | Limite padrão |
| --- | --- |
| Taxa de transferência de dados |30 Gbps<sup>1</sup> |
|Regras|10.000, regra tipos combinados.|
|Tamanho mínimo de AzureFirewallSubnet |/ 26|
|Intervalo de portas em regras de rede e de aplicativo|0-64.000. Estamos trabalhando para flexibilizar essa limitação.|
|Tabela de rotas|Por padrão, AzureFirewallSubnet tem uma rota 0.0.0.0/0 com o valor de NextHopType definido como **Internet**.<br><br>O Firewall do Azure deve ter conectividade direta com a Internet. Se seu AzureFirewallSubnet aprender uma rota padrão para sua rede local via BGP, você deve substituir isso por um UDR 0.0.0.0/0 com o valor **NextHopType** definido como **Internet** para manter a conectividade direta com a Internet. Por padrão, o Firewall do Azure não dá suporte ao túnel forçado para uma rede local.<br><br>No entanto, se sua configuração exigir um túnel forçado para uma rede local, a Microsoft dará suporte de acordo com cada caso. Entre em contato com o suporte para que possamos analisar seu caso. Se aprovado, vamos colocar sua assinatura na lista de permissões e garantir que a conectividade necessária do firewall com a Internet seja mantida.|

<sup>1</sup>caso você precise aumentar esses limites, entre em contato com o suporte do Azure.
