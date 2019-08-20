---
title: Mover os recursos de rede do Azure para uma nova assinatura ou grupo de recursos | Microsoft Docs
description: Use Azure Resource Manager para mover redes virtuais e outros recursos de rede para um novo grupo de recursos ou assinatura.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/19/2019
ms.author: tomfitz
ms.openlocfilehash: c41ab1c3309243fa3a96d907f620ffeff10376a1
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69626256"
---
# <a name="move-guidance-for-networking-resources"></a>Mover diretrizes para recursos de rede

Este artigo descreve como mover redes virtuais e outros recursos de rede para cenários específicos.

## <a name="dependent-resources"></a>Recursos dependentes

Ao mover uma rede virtual, você também deve mover os recursos dependentes. Para Gateways de VPN, você deve mover endereços IP, gateways de rede virtual e todos os recursos de conexão associados. Os gateways de rede local podem estar em um grupo de recursos diferentes.

Para mover uma máquina virtual com uma placa de interface de rede, você deve mover todos os recursos dependentes. Mova a rede virtual para a placa de interface de rede, todas as outras placas de interface de rede para a rede virtual e os gateways de VPN.

## <a name="state-of-dependent-resources"></a>Estado dos recursos dependentes

Se o grupo de recursos de origem ou de destino contiver uma rede virtual, os Estados de todos os recursos dependentes da rede virtual serão verificados durante a movimentação. Se qualquer um desses recursos estiver em um estado de falha, a movimentação será bloqueada. Por exemplo, se uma máquina virtual que usa a rede virtual falhou, a movimentação será bloqueada. A movimentação é bloqueada mesmo quando a máquina virtual não é um dos recursos que estão sendo movidos e não está em um dos grupos de recursos para a movimentação. Para evitar esse problema, mova seus recursos para um grupo de recursos que não tem uma rede virtual.

## <a name="peered-virtual-network"></a>Rede virtual emparelhada

Para mover uma rede virtual emparelhada, primeiro é necessário desabilitar o emparelhamento de rede virtual. Quando desabilitado, você pode mover a rede virtual. Após a movimentação, reabilite o emparelhamento de rede virtual.

## <a name="subnet-links"></a>Links de sub-rede

Você não pode mover uma rede virtual para uma assinatura diferente caso a rede virtual contenha uma sub-rede com links de navegação de recurso. Por exemplo, se um recurso de Cache do Azure para Redis estiver implantado em uma sub-rede, essa sub-rede terá um link de navegação do recurso.

## <a name="next-steps"></a>Próximas etapas

Para ver comandos para mover recursos, confira [Move resources to new resource group or subscription](../resource-group-move-resources.md) (Mover recursos para o novo grupo de recursos ou assinatura).
