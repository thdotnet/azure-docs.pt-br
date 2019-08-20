---
title: Solucionar erros ao mover recursos do Azure para uma nova assinatura ou grupo de recursos
description: Use o Azure Resource Manager para mover recursos para um novo grupo de recursos ou uma nova assinatura.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 08/19/2019
ms.author: tomfitz
ms.openlocfilehash: 445ee2784a74a366089a49a0e2f2f17d51ef93bf
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69624293"
---
# <a name="troubleshoot-moving-azure-resources-to-new-resource-group-or-subscription"></a>Solucionar problemas de movimentação de recursos do Azure para novo grupo de recursos ou assinatura

Este artigo fornece sugestões para ajudar a resolver problemas ao mover recursos.

## <a name="upgrade-a-subscription"></a>Atualizar uma assinatura

Se você realmente quiser atualizar sua assinatura do Azure (por exemplo, mudando de gratuita para pré-paga), você precisará converter sua assinatura.

* Para atualizar uma avaliação gratuita, consulte [Faça o upgrade da sua avaliação gratuita ou da assinatura do Microsoft Imagine Azure para o Pague conforme o uso](../billing/billing-upgrade-azure-subscription.md).
* Para alterar uma conta de pagamento conforme o uso, consulte [Alterar sua assinatura do Azure Pay-As-You-Go para uma oferta diferente](../billing/billing-how-to-switch-azure-offer.md).

Se você não conseguir converter a assinatura, [crie uma solicitação de suporte do Azure](../azure-supportability/how-to-create-azure-support-request.md). Selecione **Subscription Management** para o tipo de problema.

## <a name="service-limitations"></a>Limitações de serviço

Alguns serviços exigem considerações adicionais ao mover recursos. Se você estiver movendo os seguintes serviços, certifique-se de verificar as diretrizes e as limitações.

* [Serviços de Aplicativos](./move-limitations/app-service-move-limitations.md)
* [Azure DevOps Services](/azure/devops/organizations/billing/change-azure-subscription?toc=/azure/azure-resource-manager/toc.json)
* [Modelo de implantação clássica](./move-limitations/classic-model-move-limitations.md)
* [Rede](./move-limitations/networking-move-limitations.md)
* [Serviços de Recuperação](../backup/backup-azure-move-recovery-services-vault.md?toc=/azure/azure-resource-manager/toc.json)
* [Máquinas Virtuais](./move-limitations/virtual-machines-move-limitations.md)

## <a name="large-requests"></a>Solicitações grandes

Quando possível, quebre grandes movimentações em operações de movimentação separadas. O Resource Manager imediatamente retornará um erro quando houver mais de 800 recursos em uma única operação. No entanto, mover menos de 800 recursos também pode falhar por tempo limite.

## <a name="resource-not-in-succeeded-state"></a>Recurso que não está no estado com êxito

Se você receber uma mensagem de erro que indica que um recurso não pode ser movido porque não está em um estado de êxito, ele poderá, na verdade, ser um recurso dependente que está bloqueando a movimentação. Consulte [estado dos recursos dependentes](./move-limitations/networking-move-limitations.md#state-of-dependent-resources).

## <a name="next-steps"></a>Próximas etapas

Para ver comandos para mover recursos, confira [Move resources to new resource group or subscription](resource-group-move-resources.md) (Mover recursos para o novo grupo de recursos ou assinatura).
