---
title: Implantar o Firewall do Azure com vários endereços IP públicos usando o Azure PowerShell
description: Neste artigo, você aprenderá como implantar um Firewall do Azure com vários endereços IP públicos usando o Azure PowerShell.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 06/15/2019
ms.author: victorh
ms.openlocfilehash: a7dd35212a573fc3e94dadea4365f150122e1b5a
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67276737"
---
# <a name="deploy-an-azure-firewall-with-multiple-public-ip-addresses-using-azure-powershell"></a>Implantar um Firewall do Azure com vários endereços IP públicos usando o Azure PowerShell

> [!IMPORTANT]
> Firewall do Azure com vários endereços IP públicos está atualmente em visualização pública.
> Essa versão prévia é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Você pode implantar um Firewall do Azure com os endereços IP públicos até 600.

Esse recurso permite os seguintes cenários:

- **DNAT** -várias instâncias de porta padrão pode ser convertido em seus servidores de back-end. Por exemplo, se você tiver dois endereços IP públicos, você pode traduzir a porta TCP 3389 (RDP) para os dois endereços IP.
- **SNAT** -portas adicionais estão disponíveis para conexões SNAT de saída, reduz a possibilidade de esgotamento da porta SNAT. Neste momento, o Firewall do Azure seleciona aleatoriamente o endereço IP público do código-fonte a ser usado para uma conexão. Se você tiver qualquer filtragem downstream em sua rede, você precisa permitir todos os endereços IP públicos associados com seu firewall.

Os exemplos do Azure PowerShell a seguir mostram como você pode adicionar, remover e configurar endereços IP públicos para o Firewall do Azure.

> [!NOTE]
> Se você adicionar ou remove um endereço IP público a um firewall em execução, a conectividade de entrada existente usando as regras de DNAT pode não funcionar por 120 40 segundos. Essa é uma limitação da visualização pública para esse recurso.

## <a name="add-a-public-ip-address-to-an-existing-firewall"></a>Adicionar um endereço IP público a um firewall existente

Neste exemplo, o endereço IP público *azFwPublicIp1* como anexado ao firewall.

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

Neste exemplo, o endereço IP público *azFwPublicIp1* como desanexadas do firewall.

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

## <a name="next-steps"></a>Próximas etapas

* [Tutorial: Monitorar os logs do Firewall do Azure](./tutorial-diagnostics.md)
