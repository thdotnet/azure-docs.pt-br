---
title: Lista Negar as atribuições de recursos do Azure usando o Azure PowerShell | Microsoft Docs
description: Saiba como listar os usuários, grupos, entidades de serviço e identidades gerenciadas negou acesso a ações específicas de recursos do Azure em escopos específicos usando o Azure PowerShell.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/12/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: c1ea26fdb4d60262f89ea6ab0f87220a08c01e68
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67110479"
---
# <a name="list-deny-assignments-for-azure-resources-using-azure-powershell"></a>Lista Negar as atribuições de recursos do Azure usando o Azure PowerShell

As [atribuições de negação](deny-assignments.md) impedem que os usuários executem ações específicas, mesmo que uma atribuição de função conceda o acesso a elas. Este artigo descreve como listar negar atribuições usando o Azure PowerShell.

> [!NOTE]
> Você não pode criar diretamente seus próprios negar atribuições. Para obter informações sobre como negar as atribuições são criadas, consulte [negar atribuições](deny-assignments.md).

## <a name="prerequisites"></a>Pré-requisitos

Para obter informações sobre uma atribuição de deny, você deve ter:

- `Microsoft.Authorization/denyAssignments/read` permissão, que está incluído na maioria dos [funções internas para recursos do Azure](built-in-roles.md)
- [PowerShell no Azure Cloud Shell](/azure/cloud-shell/overview) ou [PowerShell do Azure](/powershell/azure/install-az-ps)

## <a name="list-deny-assignments"></a>Lista as atribuições de negação

### <a name="list-all-deny-assignments"></a>Lista todas as atribuições de negar

Para listar todas as atribuições da assinatura atual, use de negar [Get-AzDenyAssignment](/powershell/module/az.resources/get-azdenyassignment).

```azurepowershell
Get-AzDenyAssignment
```

```Example
PS C:\> Get-AzDenyAssignment

Id                      : 22222222-2222-2222-2222-222222222222
DenyAssignmentName      : Deny assignment '22222222-2222-2222-2222-222222222222' created by Blueprint Assignment
                          '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Description             : Created by Blueprint Assignment '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Actions                 : {*}
NotActions              : {*/read}
DataActions             : {}
NotDataActions          : {}
Scope                   : /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/TestingBPLocks
DoNotApplyToChildScopes : True
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
ExcludePrincipals       : {
                          ObjectType:   ServicePrincipal
                          }
IsSystemProtected       : True

Id                      : 33333333-3333-3333-3333-333333333333
DenyAssignmentName      : Deny assignment '33333333-3333-3333-3333-333333333333' created by Blueprint Assignment
                          '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Description             : Created by Blueprint Assignment '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Actions                 : {*}
NotActions              : {*/read}
DataActions             : {}
NotDataActions          : {}
Scope                   : /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/TestingBPLocks/providers/Microsoft.Storage/storageAccounts/storep6vkuxmu4m4pq
DoNotApplyToChildScopes : True
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
ExcludePrincipals       : {
                          DisplayName:  assignment-locked-storageaccount-TestingBPLocks
                          ObjectType:   ServicePrincipal
                          ObjectId:     2311a0b7-657a-4ca2-af6f-d1c33f6d2fff
                          }
IsSystemProtected       : True
```

### <a name="list-deny-assignments-at-a-resource-group-scope"></a>Lista Negar atribuições em um escopo de grupo de recursos

Para listar todos os negar atribuições em um escopo de grupo de recursos, use [Get-AzDenyAssignment](/powershell/module/az.resources/get-azdenyassignment).

```azurepowershell
Get-AzDenyAssignment -ResourceGroupName <resource_group_name>
```

```Example
PS C:\> Get-AzDenyAssignment -ResourceGroupName TestingBPLocks | FL DenyAssignmentName, Scope

DenyAssignmentName : Deny assignment '22222222-2222-2222-2222-222222222222' created by Blueprint Assignment
                     '/subscriptions/11111111-1111-1111-1111-111111111111/providers/Microsoft.Blueprint/blueprintAssignments/assignment-locked-storageaccount-TestingBPLocks'.
Scope              : /subscriptions/11111111-1111-1111-1111-111111111111/resourceGroups/TestingBPLocks
Principals         : {
                     DisplayName:       All Principals
                     ObjectType:        SystemDefined
                     ObjectId:  00000000-0000-0000-0000-000000000000
                     }
```

### <a name="list-deny-assignments-at-a-subscription-scope"></a>Lista Negar atribuições em um escopo de assinatura

Para listar todos os negar atribuições em um escopo de assinatura, use [Get-AzDenyAssignment](/powershell/module/az.resources/get-azdenyassignment). Para obter a ID da assinatura, você pode encontrá-lo sobre a **assinaturas** folha no portal do Azure, ou você pode usar [Get-AzSubscription](/powershell/module/Az.Accounts/Get-AzSubscription).

```azurepowershell
Get-AzDenyAssignment -Scope /subscriptions/<subscription_id>
```

```Example
PS C:\> Get-AzDenyAssignment -Scope /subscriptions/11111111-1111-1111-1111-111111111111
```

## <a name="next-steps"></a>Próximas etapas

- [Compreender atribuições de negação dos recursos do Azure](deny-assignments.md)
- [Lista Negar as atribuições de recursos do Azure usando o portal do Azure](deny-assignments-portal.md)
- [Listar atribuições de negação para recursos do Azure usando a API REST](deny-assignments-rest.md)