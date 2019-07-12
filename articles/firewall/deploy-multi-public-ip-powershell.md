---
title: Implantar o Firewall do Azure com vários endereços IP públicos usando o Azure PowerShell
description: Neste artigo, você aprenderá como implantar um Firewall do Azure com vários endereços IP públicos usando o Azure PowerShell.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 7/10/2019
ms.author: victorh
ms.openlocfilehash: ce47612f18ee64caa3a053001deb5448f7c27bfd
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67703978"
---
# <a name="deploy-an-azure-firewall-with-multiple-public-ip-addresses-using-azure-powershell"></a>Implantar um Firewall do Azure com vários endereços IP públicos usando o Azure PowerShell

> [!IMPORTANT]
> Firewall do Azure com vários endereços IP públicos está disponível por meio do Azure PowerShell, CLI do Azure, REST e modelos. A interface do usuário do portal está sendo adicionada a regiões de forma incremental e estarão disponível em todas as regiões quando a distribuição é concluída.

Você pode implantar um Firewall do Azure com até 100 endereços IP públicos.

Esse recurso permite os seguintes cenários:

- **DNAT** -várias instâncias de porta padrão pode ser convertido em seus servidores de back-end. Por exemplo, se você tiver dois endereços IP públicos, você pode traduzir a porta TCP 3389 (RDP) para os dois endereços IP.
- **SNAT** -portas adicionais estão disponíveis para conexões SNAT de saída, reduz a possibilidade de esgotamento da porta SNAT. Neste momento, o Firewall do Azure seleciona aleatoriamente o endereço IP público do código-fonte a ser usado para uma conexão. Se você tiver qualquer filtragem downstream em sua rede, você precisa permitir todos os endereços IP públicos associados com seu firewall.

Os exemplos do Azure PowerShell a seguir mostram como você pode configurar, adicionar e remover endereços IP públicos para o Firewall do Azure.

> [!NOTE]
> Você não pode remover a ipConfiguration do primeiro a página de configuração endereços IP pública Firewall do Azure. Se você quiser modificar o endereço IP, você pode usar o Azure PowerShell.

## <a name="create-a-firewall-with-two-or-more-public-ip-addresses"></a>Criar um firewall com dois ou mais endereços IP públicos

Este exemplo cria um firewall conectado à rede virtual *vnet* com dois endereços IP públicos.

```azurepowershell
$rgName = "resourceGroupName"

$vnet = Get-AzVirtualNetwork `
  -Name "vnet" `
  -ResourceGroupName $rgName

$pip1 = New-AzPublicIpAddress `
  -Name "AzFwPublicIp1" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

$pip2 = New-AzPublicIpAddress `
  -Name "AzFwPublicIp2" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

New-AzFirewall `
  -Name "azFw" `
  -ResourceGroupName $rgName `
  -Location centralus `
  -VirtualNetwork $vnet `
  -PublicIpAddress @($pip1, $pip2)
```

## <a name="add-a-public-ip-address-to-an-existing-firewall"></a>Adicionar um endereço IP público a um firewall existente

Neste exemplo, o endereço IP público *azFwPublicIp1* está anexado ao firewall.

```azurepowershell
$pip = New-AzPublicIpAddress `
  -Name "azFwPublicIp1" `
  -ResourceGroupName "rg" `
  -Sku "Standard" `
  -Location "centralus" `
  -AllocationMethod Static

$azFw = Get-AzFirewall `
  -Name "AzureFirewall" `
  -ResourceGroupName "rg"

$azFw.AddPublicIpAddress($pip)

$azFw | Set-AzFirewall
```

## <a name="remove-a-public-ip-address-from-an-existing-firewall"></a>Remover um endereço IP público de um firewall existente

Neste exemplo, o endereço IP público *azFwPublicIp1* é desanexado do firewall.

```azurepowershell
$pip = Get-AzPublicIpAddress `
  -Name "azFwPublicIp1" `
  -ResourceGroupName "rg"

$azFw = Get-AzFirewall `
  -Name "AzureFirewall" `
  -ResourceGroupName "rg"

$azFw.RemovePublicIpAddress($pip)

$azFw | Set-AzFirewall
```

## <a name="next-steps"></a>Próximas etapas

* [Tutorial: Monitorar os logs do Firewall do Azure](./tutorial-diagnostics.md)
