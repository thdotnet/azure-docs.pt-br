---
title: Limite de gastos do Azure | Microsoft Docs
description: Este artigo descreve como funciona o limite de gastos do Azure e como removê-lo.
author: bandersmsft
manager: amberb
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/28/2019
ms.author: banders
ms.openlocfilehash: 1324b60de05805cd409975358ff78e5b2b27eef1
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70114446"
---
# <a name="azure-spending-limit"></a>Limite de gastos do Azure

O limite de gastos do Azure impede gastos sobre a sua quantidade de créditos. Todos os novos clientes que se inscreverem para uma avaliação do Azure ou para ofertas que incluam créditos durante vários meses, terão o limite de gastos ativado por padrão. O limite de gastos é de US$ 0 e não pode ser alterado. Por exemplo, você não pode alterar o limite de gastos para US$ 100. No entanto, você pode remover o limite de gastos. Portanto, ou você não tem limite ou tem um limite igual a zero, que impede a maioria dos tipos de gastos. O limite de gastos não está disponível para assinaturas de alguns planos, como os de compromisso ou aqueles com preço pago conforme o uso. Consulte a [lista completa de ofertas do Azure e a disponibilidade do limite de gastos](https://azure.microsoft.com/support/legal/offer-details/).

## <a name="reaching-a-spending-limit"></a>Alcance de um limite de gastos

Quando seu uso resultar em encargos que consomem todas as quantias mensais incluídas em sua assinatura do Azure, os serviços implantados serão desabilitados durante o restante do período de cobrança.

Por exemplo, quando você gastar todo o crédito incluso na sua assinatura, os recursos do Azure implantados serão removidos da produção e suas máquinas virtuais do Azure serão interrompidas e desalocadas. Os dados de suas contas de armazenamento estão disponíveis como somente leitura.

No início do próximo período de cobrança, se a oferta de assinatura incluir créditos durante vários meses, a assinatura será habilitada novamente de maneira automática. Depois, você pode reimplantar os recursos do Azure e ter acesso completo às contas de armazenamento e aos bancos de dados.

O Azure enviará notificações por email quando você atingir o limite de gastos da sua assinatura. Entre no [Centro de Contas](https://account.windowsazure.com/Subscriptions) para ver notificações sobre assinaturas que atingiram o limite de gastos.

Se você tiver uma assinatura de avaliação gratuita e alcançar o limite de gastos, será possível atualizar para um plano com preço [pago conforme o uso](billing-upgrade-azure-subscription.md) a fim de remover o limite de gastos e habilitar automaticamente a assinatura.

<a id="remove"></a>

## <a name="remove-the-spending-limit-in-account-center"></a>Remover o limite de gastos no Centro de Contas

Você poderá remover o limite de gastos a qualquer momento, contanto que haja uma forma de pagamento válida associada à sua assinatura do Azure. Para ofertas que têm crédito ao longo de vários meses, também é possível habilitar o limite de gastos no começo do próximo período de cobrança.

Para remover seu limite de gastos, execute estas etapas:

1. Entre no [Centro de Contas](https://account.windowsazure.com/Subscriptions).
1. Selecione uma assinatura. Se a assinatura for desabilitada porque o limite de gastos foi alcançado, clique na notificação: **A assinatura alcançou o Limite de Gastos e foi desabilitada para evitar encargos.** Caso contrário, clique em **Remover limite de gastos** na área **STATUS DA ASSINATURA**.
1. Selecione uma opção que é adequada para você.

![Selecionando uma opção para remover o limite de gastos](./media/billing-spending-limit/remove-spending-limit.PNG)

| Opção | Efeito |
| --- | --- |
| Remover o limite de gastos indefinidamente | Remove o limite de gastos sem ativá-lo automaticamente no início do próximo período de cobrança. |
| Remover limite de gastos para o período de cobrança atual | Remove o limite de gastos para que ele seja ativado automaticamente no início do próximo período de cobrança. |

## <a name="why-you-might-want-to-remove-the-spending-limit"></a>Por que remover o limite de gastos?

O limite de gastos pode evitar que você implante ou use determinados serviços da Microsoft e de terceiros. Veja algumas situações em que você poderia remover o limite de gastos da sua assinatura.

-  Você planeja implantar imagens primárias como o Oracle e serviços como o Azure DevOps Services. Essa situação fará com que você exceda seu limite de gastos quase imediatamente e que sua assinatura seja desabilitada.
- Há alguns serviços que não podem ser interrompidos.
- Você tem serviços e recursos com configurações como endereços IP virtuais e que não deseja perder. Essas configurações são perdidas quando os serviços e os recursos são desalocados.

## <a name="turn-on-the-spending-limit-after-removing"></a>Ativar o limite de gastos após removê-lo

Esse recurso está disponível somente quando o limite de gastos foi removido indefinidamente. Altere-o para ativá-lo automaticamente no início do próximo período de cobrança.

1. Entre no [Centro de Contas](https://account.windowsazure.com/Subscriptions).
1. Clique na faixa amarela para alterar a opção de limite de gastos.
1. Escolha **Ativar o limite de gastos no próximo período de cobrança \<start date of billing period\>**

## <a name="custom-spending-limit"></a>Limite de gastos personalizado

Limites de gastos personalizados não estão disponíveis.

## <a name="a-spending-limit-doesnt-prevent-all-charges"></a>Um limite de gastos não impede todos os encargos

[Alguns serviços externos publicados no Azure Marketplace](billing-understand-your-azure-marketplace-charges.md) não podem ser usados com seus créditos de assinatura, podendo gerar cobranças independentes, mesmo quando o limite de gastos estiver definido. Exemplos incluem licenças do Visual Studio, o Azure Active Directory Premium, planos de suporte e a maior parte dos serviços de terceiros. Quando você provisiona um novo serviço externo, é mostrado um aviso informando que os serviços são cobrados separadamente:

![Aviso de compra no Marketplace](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

## <a name="need-help-contact-us"></a>Precisa de ajuda? Entre em contato conosco.

Caso tenha dúvidas ou precise de ajuda, [crie uma solicitação de suporte](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Próximas etapas
- Atualize para um plano com o preço [pago conforme o uso](billing-upgrade-azure-subscription.md).
