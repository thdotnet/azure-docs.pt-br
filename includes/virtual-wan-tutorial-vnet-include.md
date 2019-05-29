---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 04/23/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 40c8cb41ad3bcd46e9973a5f96134ff1bfd02fd2
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66150870"
---
Para criar sua rede virtual rapidamente, clique em "Experimentar" neste artigo para abrir um console do PowerShell. Ajuste os valores e copie e cole os comandos na janela de console. Para saber mais sobre o novo módulo Az e a compatibilidade com o AzureRM, confira [Apresentação do novo módulo Az do Azure PowerShell](/powershell/azure/new-azureps-module-az). Para obter instruções de instalação do módulo Az, confira [Instalar o Azure PowerShell](/powershell/azure/install-az-ps).

Verifique se o espaço de endereço para a VNET criada não corresponde a nenhum dos intervalos de endereço de outras VNETs às quais você deseja se conectar ou a espaços de endereço de rede local.

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Se você ainda não tiver um grupo de recursos que deseja usar, crie um. Ajuste os comandos do PowerShell para refletir o nome do grupo de recursos que você deseja usar e, em seguida, execute o seguinte cmdlet:

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName WANTestRG -Location WestUS
```

### <a name="create-a-vnet"></a>Criar uma VNET

Ajuste os comandos do PowerShell para criar uma rede virtual compatível com seu ambiente.

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name FrontEnd -AddressPrefix "10.1.0.0/24"
$vnet   = New-AzVirtualNetwork `
            -Name WANVNet1 `
            -ResourceGroupName WANTestRG `
            -Location WestUS `
            -AddressPrefix "10.1.0.0/16" `
            -Subnet $fesub1
```