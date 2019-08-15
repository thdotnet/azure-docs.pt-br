---
title: Exemplo de Script do Azure PowerShell – Abrir porta de aplicativo no balanceador de carga | Microsoft Docs
description: Exemplo de Script do Azure PowerShell – Abrir uma porta no Azure Load Balancer para um aplicativo do Service Fabric.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 05/18/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 8e6a0b7f273df96cff5cc9ce1081ddc59eb49382
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69035547"
---
# <a name="open-an-application-port-in-the-azure-load-balancer"></a>Abrir uma porta de aplicativo no Azure Load Balancer

Um aplicativo do Service Fabric em execução no Azure fica por trás do Azure Load Balancer. Este exemplo de script abre uma porta em um Azure Load Balancer para que um aplicativo do Service Fabric possa se comunicar com clientes externos. Personalize os parâmetros conforme necessário. Se o cluster estiver em um grupo de segurança de rede, também [adicione uma regra de grupo de segurança de rede de entrada](service-fabric-powershell-add-nsg-rule.md) para permitir o tráfego de entrada.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Se necessário, instale o módulo Service Fabric do PowerShell com o [SDK do Service Fabric](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Script de exemplo

[!code-powershell[main](../../../powershell_scripts/service-fabric/open-port-in-load-balancer/open-port-in-load-balancer.ps1 "Open a port in the load balancer")]

## <a name="script-explanation"></a>Explicação sobre o script

Este script usa os comandos a seguir. Cada comando na tabela redireciona para a documentação específica do comando.

| Comando | Observações |
|---|---|
| [Get-AzResource](/powershell/module/az.resources/get-azresource) | Obtém um recurso do Azure.  |
| [Get-AzLoadBalancer](/powershell/module/az.network/get-azloadbalancer) | Obtém o Azure Load Balancer. |
| [Add-AzLoadBalancerProbeConfig](/powershell/module/az.network/add-azloadbalancerprobeconfig) | Adiciona uma configuração de investigação a um balanceador de carga.|
| [Get-AzLoadBalancerProbeConfig](/powershell/module/az.network/get-azloadbalancerprobeconfig) | Obtém uma configuração de investigação para um balanceador de carga. |
| [Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/add-azloadbalancerruleconfig) | Adiciona uma configuração de regra a um balanceador de carga. |
| [Set-AzLoadBalancer](/powershell/module/az.network/set-azloadbalancer) | Define a meta de estado para um balanceador de carga. |

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre o módulo do Azure PowerShell, confira [Documentação do Azure PowerShell](/powershell/azure/overview).

Mais exemplos do PowerShell para o Azure Service Fabric podem ser encontrados nos [exemplos do Azure PowerShell](../service-fabric-powershell-samples.md).
