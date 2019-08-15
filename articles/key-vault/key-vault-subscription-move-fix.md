---
title: Alterar a ID de locatário do cofre de chaves de depois de mover uma assinatura - Azure Key Vault| Microsoft Docs
description: Saiba como mudar a ID de locatário para um cofre de chaves depois que uma assinatura é movida para um locatário diferente
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 2159b5b515e22458edf3ba0eb5b6f23f3f37ce95
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68990116"
---
# <a name="change-a-key-vault-tenant-id-after-a-subscription-move"></a>Alterar a ID de locatário do cofre de chaves depois de mover uma assinatura

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="q-my-subscription-was-moved-from-tenant-a-to-tenant-b-how-do-i-change-the-tenant-id-for-my-existing-key-vault-and-set-correct-acls-for-principals-in-tenant-b"></a>P: Minha assinatura foi movida do locatário A para o locatário B. Como posso alterar a ID do locatário para o cofre de chaves existente e definir as ACLs corretas para as entidades de segurança no locatário B?

Quando você cria um novo cofre de chaves em uma assinatura, ele é vinculado automaticamente à ID de locatário do Azure Active Directory padrão para essa assinatura. Todas as entradas de política de acesso também são vinculadas a essa ID de locatário. Quando você move a assinatura do Azure do locatário A para o locatário B, os cofres de chaves existentes ficam inacessíveis para as entidades (usuários e aplicativos) no locatário B. Para corrigir esse problema, você precisa:

* Alterar a ID do locatário associada a todos os cofres de chaves existentes nessa assinatura para o locatário B.
* Remover todas as entradas de política de acesso existentes.
* Adicionar novas entradas de política de acesso que estão associadas ao locatário B.

Por exemplo, se você tiver o cofre de chaves 'myvault' em uma assinatura que foi movida do locatário A para o B, veja como mudar a ID de locatário para o cofre de chaves e remover as antigas políticas de acesso.

<pre>
Select-AzSubscription -SubscriptionId YourSubscriptionID                   # Select your Azure Subscription
$vaultResourceId = (Get-AzKeyVault -VaultName myvault).ResourceId          # Get your Keyvault's Resource ID 
$vault = Get-AzResource –ResourceId $vaultResourceId -ExpandProperties     # Get the properties for your Keyvault
$vault.Properties.TenantId = (Get-AzContext).Tenant.TenantId               # Change the Tenant that your Keyvault resides in
$vault.Properties.AccessPolicies = @()                                     # Accesspolicies can be updated with real
                                                                           # applications/users/rights so that it does not need to be                                                                              # done after this whole activity. Here we are not setting 
                                                                           # any access policies. 
Set-AzResource -ResourceId $vaultResourceId -Properties $vault.Properties  # Modifies the kevault's properties.
</pre>

Já que o cofre estava no locatário A antes da mudança, o valor original de **$vault.Properties.TenantId** é o locatário A e **(Get-AzContext).Tenant.TenantId** é o locatário B.

Agora que o cofre está associado à ID de locatário correta e as antigas entradas da política de acesso foram removidas, defina novas entradas da política de acesso com [Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy).

## <a name="next-steps"></a>Próximas etapas

Se você tiver dúvidas sobre o Cofre de Chaves do Azure, visite os [Fóruns do Cofre de Chaves do Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
