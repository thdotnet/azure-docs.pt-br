---
title: Instâncias de conjunto de proteção de instância para a escala de máquina virtual do Azure | Microsoft Docs
description: Saiba como proteger instâncias de conjunto de dimensionamento de máquina virtual do Azure de operações de redução horizontal e conjunto de dimensionamento.
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: drewm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2019
ms.author: manayar
ms.openlocfilehash: 61430f5a43a04fa0e5b2f0c79ff03419c73aaf28
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66416557"
---
# <a name="instance-protection-for-azure-virtual-machine-scale-set-instances-preview"></a>Proteção de instância para a escala de máquina virtual do Azure definida instâncias (versão prévia)
Habilitam a elasticidade melhor para suas cargas de trabalho por meio de conjuntos de dimensionamento de máquina virtual do Azure [AutoEscala](virtual-machine-scale-sets-autoscale-overview.md), portanto, você pode configurar quando sua infraestrutura de escala horizontalmente e quando ele reduz horizontalmente. Conjuntos de dimensionamento também permitem que você gerencie centralmente, configurar e atualizar um grande número de VMs por meio de diferentes [política de atualização](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model) configurações. Você pode configurar uma atualização no modelo de conjunto de escala e a nova configuração é aplicada automaticamente em cada instância do conjunto de dimensionamento se você tiver definido a política de atualização como automático ou sem interrupção.

Conforme seu aplicativo processa o tráfego, pode haver situações em que você quer instâncias específicas para ser tratados de modo diferente do restante da escala de instância do conjunto. Por exemplo, algumas instâncias no conjunto de dimensionamento podem executar operações de longa execução, e você não deseja que essas instâncias para ser dimensionado-in até que as operações forem concluídas. Você também pode ter especializadas algumas instâncias no conjunto de dimensionamento para executar tarefas adicionais ou diferentes que os outros membros do conjunto de dimensionamento. Você requer essas VMs 'especiais' não deve ser modificada com as outras instâncias no conjunto de dimensionamento. Proteção de instância fornece os controles adicionais para habilitar esses e outros cenários para seu aplicativo.

Este artigo descreve como você pode aplicar e usar os recursos de proteção de instância diferente com instâncias de conjunto de dimensionamento.

> [!NOTE]
>Proteção de instância está atualmente em visualização pública. Nenhum procedimento de aceitação é necessária para usar a funcionalidade de visualização pública, descrita abaixo. Visualização da proteção de instância só tem suporte com a API versão 2019-03-01 e em conjuntos de dimensionamento usando discos gerenciados.

## <a name="types-of-instance-protection"></a>Tipos de proteção de instância
Conjuntos de dimensionamento fornecem dois tipos de recursos de proteção da instância:

-   **Proteger contra horizontalmente**
    - Habilitado por meio **protectFromScaleIn** definida na escala de instância
    - Protege a instância de AutoEscala iniciada horizontalmente
    - Operações de instância iniciada pelo usuário (incluindo a exclusão de instância) são **não bloqueado**
    - Operações iniciadas no conjunto de dimensionamento (atualizar, refazer a imagem, desalocar, etc.) são **não bloqueado**

-   **Proteger contra ações de conjunto de escala**
    - Habilitado por meio **protectFromScaleSetActions** definida na escala de instância
    - Protege a instância de AutoEscala iniciada horizontalmente
    - Protege a instância de operações iniciadas no conjunto de dimensionamento (como atualização, refazer a imagem, desalocar, etc.)
    - Operações de instância iniciada pelo usuário (incluindo a exclusão de instância) são **não bloqueado**
    - A exclusão do conjunto de dimensionamento completo está **não bloqueado**

## <a name="protect-from-scale-in"></a>Proteger contra horizontalmente
Proteção de instância pode ser aplicada a instâncias de conjunto de dimensionamento depois que as instâncias são criadas. Proteção é aplicada e modificada apenas na [modelo de instância](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-vm-model-view) e não na [modelo do conjunto de dimensionamento](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-model).

Há várias maneiras de aplicar a proteção de escala em suas instâncias de conjunto de dimensionamento conforme detalhado nos exemplos a seguir.

### <a name="rest-api"></a>API REST

O exemplo a seguir se aplica a proteção de escala para uma instância no conjunto de dimensionamento.

```
PUT on `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualMachines/{instance-id}?api-version=2019-03-01`
```

```json
{
  "properties": {
    "protectionPolicy": {
      "protectFromScaleIn": true
    }
  }        
}

```

> [!NOTE]
>Proteção de instância só tem suporte com a API versão 2019-03-01 e acima

### <a name="azure-powershell"></a>Azure PowerShell

Use o [AzVmssVM atualização](/powershell/module/az.compute/update-azvmssvm) instância do conjunto de cmdlet para aplicar proteção de escala à sua escala.

O exemplo a seguir se aplica a proteção de escala para uma instância no conjunto de dimensionamento com a ID de instância 0.

```azurepowershell-interactive
Update-AzVmssVM `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myVMScaleSet" `
  -InstanceId 0 `
  -ProtectFromScaleIn $true
```

### <a name="azure-cli-20"></a>CLI do Azure 2.0

Use [az vmss atualização](/cli/azure/vmss#az-vmss-update) para aplicar a proteção em escala para a instância do conjunto de dimensionamento.

O exemplo a seguir se aplica a proteção de escala para uma instância no conjunto de dimensionamento com a ID de instância 0.

```azurecli-interactive
az vmss update \  
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --instance-id 0 \
  --protect-from-scale-in true
```

## <a name="protect-from-scale-set-actions"></a>Proteger contra ações de conjunto de escala
Proteção de instância pode ser aplicada a instâncias de conjunto de dimensionamento depois que as instâncias são criadas. Proteção é aplicada e modificada apenas na [modelo de instância](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-vm-model-view) e não na [modelo do conjunto de dimensionamento](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-model).

Também protegendo uma instância de ações de conjunto de escala protege a instância de dimensionamento automático iniciado horizontalmente.

Há várias maneiras de aplicar a escala de instâncias de conjunto de proteção de ações em seu dimensionamento conjunto conforme detalhado nos exemplos a seguir.

### <a name="rest-api"></a>API REST

O exemplo a seguir se aplica a proteção contra ações de conjunto de escala para uma instância no conjunto de dimensionamento.

```
PUT on `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vMScaleSetName}/virtualMachines/{instance-id}?api-version=2019-03-01`
```

```json
{
  "properties": {
    "protectionPolicy": {
      "protectFromScaleIn": true,
      "protectFromScaleSetActions": true
    }
  }        
}

```

> [!NOTE]
>Proteção de instância só tem suporte com a API versão 2019-03-01 e acima.</br>
Também protegendo uma instância de ações de conjunto de escala protege a instância de dimensionamento automático iniciado horizontalmente. Não é possível especificar "protectFromScaleIn": falso quando a configuração "protectFromScaleSetActions": true

### <a name="azure-powershell"></a>Azure PowerShell

Use o [AzVmssVM atualização](/powershell/module/az.compute/update-azvmssvm) cmdlet para aplicar a proteção de escala de conjunto de ações para sua instância do conjunto de dimensionamento.

O exemplo a seguir se aplica a proteção contra ações de conjunto de escala para uma instância no conjunto de dimensionamento com a ID de instância 0.

```azurepowershell-interactive
Update-AzVmssVM `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myVMScaleSet" `
  -InstanceId 0 `
  -ProtectFromScaleIn $true `
  -ProtectFromScaleSetAction $true
```

### <a name="azure-cli-20"></a>CLI do Azure 2.0

Use [az vmss atualização](/cli/azure/vmss#az-vmss-update) para aplicar a proteção de ações de conjunto de escala para a instância do conjunto de dimensionamento.

O exemplo a seguir se aplica a proteção contra ações de conjunto de escala para uma instância no conjunto de dimensionamento com a ID de instância 0.

```azurecli-interactive
az vmss update \  
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --instance-id 0 \
  --protect-from-scale-in true \
  --protect-from-scale-set-actions true
```

## <a name="troubleshoot"></a>Solução de problemas
### <a name="no-protectionpolicy-on-scale-set-model"></a>Não há protectionPolicy no modelo do conjunto de dimensionamento
Proteção de instância só é aplicável em instâncias de conjunto de dimensionamento e não no modelo de conjunto de dimensionamento.

### <a name="no-protectionpolicy-on-scale-set-instance-model"></a>Não há protectionPolicy no modelo de instância do conjunto de dimensionamento
Por padrão, política de proteção não é aplicada a uma instância quando ele é criado.

Você pode aplicar proteção de instância para instâncias de conjunto de dimensionamento depois que as instâncias são criadas.

### <a name="not-able-to-apply-instance-protection"></a>Não é possível aplicar a proteção de instância
Proteção de instância só tem suporte com a API versão 2019-03-01 e acima. Verifique a versão da API que está sendo usada e atualize conforme o necessário. Você também talvez seja necessário atualizar o PowerShell ou CLI para a versão mais recente.

## <a name="next-steps"></a>Próximas etapas
Aprenda como [Implantar o aplicativo](virtual-machine-scale-sets-deploy-app.md) em conjuntos de dimensionamento de máquinas virtuais
