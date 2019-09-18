---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/22/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 65c1011e6e005c190d1ae5d51fdd009f66a20956
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70919635"
---
|**SKU**   | **S2S/VNet para VNet<br>Túneis** | **P2S<br> Conexões SSTP** | **P2S<br> Conexões IKEv2/OpenVPN** | **Parâmetro de comparação<br>de taxa de transferência total** | **BGP** | **Com redundância de zona** |
|---       | ---        | ---       | ---            | ---       | --- | --- |
|**Básico** | Máx. 10    | Máx. 128  | Sem suporte  | 100 Mbps  | Sem suporte| Não |
|**VpnGw1**| Máx. 30*   | Máx. 128  | Máx. 250       | 650 Mbps  | Com suporte | Não |
|**VpnGw2**| Máx. 30*   | Máx. 128  | Máx. 500       | 1 Gbps    | Com suporte | Não |
|**VpnGw3**| Máx. 30*   | Máx. 128  | Máx. 1000      | 1,25 Gbps | Com suporte | Não |
|**VpnGw1AZ**| Máx. 30*   | Máx. 128  | Máx. 250       | 650 Mbps  | Com suporte | Sim |
|**VpnGw2AZ**| Máx. 30*   | Máx. 128  | Máx. 500       | 1 Gbps    | Com suporte | Sim |
|**VpnGw3AZ**| Máx. 30*   | Máx. 128  | Máx. 1000      | 1,25 Gbps | Com suporte | Sim |

(*) Use [WAN Virtual](../articles/virtual-wan/virtual-wan-about.md) se precisar de mais de 30 túneis de VPN S2S.

* Esses limites de conexão são separados. Por exemplo, você pode ter 128 conexões SSTP e 250 conexões IKEv2 em uma SKU VpnGw1.

* Encontre informações sobre preços na página [Preços](https://azure.microsoft.com/pricing/details/vpn-gateway) .

* As informações de SLA (contrato de nível de serviço) podem ser encontradas na página [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/).

* VpnGw1, VpnGw2, VpnGw3, VpnGw1AZ, VpnGw2AZ e VpnGw3AZ são compatíveis para gateways de VPN somente com o modelo de implantação do Resource Manager.

* A SKU Básica é considerada um SKU herdada. A SKU básica tem certas limitações de recurso. Você não pode redimensionar um gateway que usa uma SKU básica para novas SKUs de gateway, em vez disso, você deve alterar para uma nova SKU, que envolve excluir e recriar o gateway VPN. Verifique se o recurso que você precisa é compatível antes de usar a SKU básica.

* O parâmetro de comparação da taxa de transferência total se baseia nas medidas de vários túneis agregados por meio de um único gateway. O Benchmark Agregado de Taxa de Transferência para um Gateway de VPN é uma combinação de S2S + P2S. **Se você tiver muitas conexões P2S, isso poderá afetar negativamente uma conexão S2S devido a limitações de taxa de transferência.** O Parâmetro de Comparação de Taxa de Transferência Agregada não é uma taxa de transferência garantida devido às condições de tráfego de Internet e seus comportamentos de aplicativo.

* Para ajudar nossos clientes a entender o desempenho relativo das SKUs VpnGw, usamos ferramentas iPerf e CTSTraffic disponíveis publicamente para medir o desempenho. A tabela abaixo lista os resultados dos testes de desempenho com algoritmos diferentes. Como você pode ver, o melhor desempenho foi obtido quando usamos o algoritmo GCMAES256 para Criptografia e Integridade de IPsec. Obtivemos o desempenho médio quando usamos AES256 para Criptografia de IPsec e SHA256 para Integridade. Quando usamos DES3 para Criptografia de IPsec e SHA256 para Integridade, o desempenho foi menor.

|**SKU**   | **Algoritmos<br>usados** | **Taxa de transferência<br>observada** | **Pacotes por segundo<br>observados** |
|---       | ---                 | ---            | ---                    |
|**VpnGw1**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 650 Mbps<br>500 Mbps<br>120 Mbps   | 58.000<br>50.000<br>50.000|
|**VpnGw2**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 1 Gbps<br>500 Mbps<br>120 Mbps | 90.000<br>80.000<br>55.000|
|**VpnGw3**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 1,25 Gbps<br>550 Mbps<br>120 Mbps | 105.000<br>90.000<br>60.000|
|**VpnGw1AZ**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 650 Mbps<br>500 Mbps<br>120 Mbps   | 58.000<br>50.000<br>50.000|
|**VpnGw2AZ**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 1 Gbps<br>500 Mbps<br>120 Mbps | 90.000<br>80.000<br>55.000|
|**VpnGw3AZ**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 1,25 Gbps<br>550 Mbps<br>120 Mbps | 105.000<br>90.000<br>60.000|
