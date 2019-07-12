---
title: Compartilhar imagens da Galeria entre locatários no Azure | Microsoft Docs
description: Aprenda a compartilhar imagens de VM entre locatários do Azure usando Shared galerias de imagens.
services: virtual-machines-windows
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 04/05/2019
ms.author: cynthn
ms.openlocfilehash: c26abe948fa415c780d543c615c34af2091cfbc7
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67709172"
---
# <a name="share-gallery-vm-images-across-azure-tenants"></a>Compartilhar imagens da Galeria de VM entre locatários do Azure

[!INCLUDE [virtual-machines-share-images-across-tenants](../../../includes/virtual-machines-share-images-across-tenants.md)]


> [!IMPORTANT]
> Você não pode usar o portal para implantar uma VM de uma imagem em outro locatário do azure. Para criar uma VM de uma imagem compartilhada entre locatários, você deve usar o [CLI do Azure](../linux/share-images-across-tenants.md) ou o Powershell.

## <a name="create-a-vm-using-powershell"></a>Criar uma VM usando o PowerShell


Faça logon em ambos os locatários usando a ID do aplicativo, ID de locatário e o segredo. 

```azurepowershell-interactive
$applicationId = '<App ID>'
$secret = <Secret> | ConvertTo-SecureString -AsPlainText -Force
$tenant1 = "<Tenant 1 ID>"
$tenant2 = "<Tenant 2 ID>"
$cred = New-Object -TypeName PSCredential -ArgumentList $applicationId, $secret
Clear-AzContext
Connect-AzAccount -ServicePrincipal -Credential $cred  -Tenant "<Tenant 1 ID>"
Connect-AzAccount -ServicePrincipal -Credential $cred -Tenant "<Tenant 2 ID>"
```

Crie a VM no grupo de recursos que tem permissão no registro de aplicativo. Substitua as informações neste exemplo pelos seus próprios.

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$image = "/subscriptions/<Tenant 1 subscription>/resourceGroups/<Resource group>/providers/Microsoft.Compute/galleries/<Gallery>/images/<Image definition>/versions/<version>"
New-AzVm `
   -ResourceGroupName "myResourceGroup" `
   -Name "myVMfromImage" `
   -Image $image `
   -Location "South Central US" `
   -VirtualNetworkName "myImageVnet" `
   -SubnetName "myImageSubnet" `
   -SecurityGroupName "myImageNSG" `
   -PublicIpAddressName "myImagePIP" `
   -OpenPorts 3389
```

## <a name="next-steps"></a>Próximas etapas

Você também pode criar imagem compartilhada usando os recursos da Galeria do [portal do Azure](shared-images-portal.md).