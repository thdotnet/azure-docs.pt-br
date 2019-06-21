---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 05/28/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: da4177fd54c0d8777f15175cea3a74a8b01c0954
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305245"
---
1. Certifique-se de que você entrou em sua conta do Azure e estiver usando a assinatura que você deseja integrar para essa visualização. Use o seguinte exemplo para se registrar:

    ```azurepowershell-interactive
    Register-AzProviderFeature -FeatureName AllowBastionHost -ProviderNamespace Microsoft.Network
    ```
2.  Registrar novamente sua assinatura novamente com o *Network* namespace do provedor.

    ```azurepowershell-interactive
    Register-AzResourceProvider -ProviderNamespace Microsoft.Network
    ````
3. Use o comando a seguir para verificar se o *AllowBastionHost* recurso está registrado com sua assinatura:

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.Network
    ````

    Ele pode levar alguns minutos para concluir o registro.
