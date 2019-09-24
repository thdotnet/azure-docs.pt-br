---
title: Alterar seu cartão de crédito para o Azure
description: Descreve como alterar o cartão de crédito usado para pagar uma assinatura do Azure.
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: banders
ms.openlocfilehash: 05b1fe3d061e735322e6eb6c65e60f63d8adb4d6
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70933861"
---
# <a name="add-update-or-remove-a-credit-card-for-azure"></a>Adicionar, atualizar ou remover um cartão de crédito para o Azure

Este documento aplica-se a clientes que se inscreveram no Azure online com um cartão de crédito.

No portal do Azure, é possível adicionar um novo cartão de crédito, atualizar um cartão de crédito existente ou excluir um cartão de crédito que você não usa. Você deve ser um [administrador da conta](billing-subscription-transfer.md#whoisaa) para fazer essas alterações.

Se você tiver um [Contrato de Cliente da Microsoft](#check-access-to-a-microsoft-customer-agreement), as formas de pagamento serão associadas aos perfis de cobrança. Saiba como [alterar a forma de pagamento padrão de um perfil de cobrança](#change-payment-method-for-a-billing-profile). Somente o usuário que se inscreveu no Azure pode atualizar a forma de pagamento.

**Deseja mudar para pagar por fatura (cheque/transferência eletrônica)?** Consulte [Pagar assinaturas do Azure por fatura](billing-how-to-pay-by-invoice.md).

<a id="addcard"></a>

## <a name="add-a-new-credit-card-to-an-azure-subscription"></a>Adicionar um novo cartão de crédito a uma assinatura do Azure

1. Entre no [Portal do Azure](https://portal.azure.com) como Administrador da Conta.
1. Pesquise **Gerenciamento de Custos + Cobrança**.

    ![Captura de tela que mostra a pesquisa](./media/billing-how-to-change-credit-card/search.png)

1. Selecione a assinatura à qual você deseja adicionar o cartão de crédito.
1. Selecione **Formas de pagamento**.

    ![Captura de tela que mostra a opção Gerenciar formas de pagamento selecionada.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. No canto superior esquerdo, selecione "+" para adicionar um cartão. Um formulário de cartão de crédito será exibido à direita.
1. Insira os detalhes do cartão de crédito.

    ![Captura de tela que mostra como adicionar um novo cartão.](./media/billing-how-to-change-credit-card/sub-add-new-x.png)

1. Para tornar este cartão sua forma de pagamento ativa, marque a caixa ao lado de **Tornar esta minha forma de pagamento ativa** acima do formulário. Esse cartão se tornará o meio de pagamento ativo para todas as assinaturas usando o mesmo cartão, como a assinatura selecionada.

1. Selecione **Avançar**.

Se você receber um erro depois de adicionar o cartão de crédito, consulte [Cartão de crédito recusado na inscrição do Azure](billing-credit-card-fails-during-azure-sign-up.md).

## <a name="update-existing-credit-card"></a>Atualizar cartão de crédito existente

Caso seu cartão de crédito seja renovado e o número permaneça o mesmo, atualize os detalhes do cartão de crédito existente, como a data de vencimento. Se o número de seu cartão de crédito for alterado devido a perda, roubo ou vencimento do cartão, siga as etapas da seção [Adicionar um cartão de crédito como uma forma de pagamento](#addcard). Você não precisa atualizar o CVV.

1. Entre no [Portal do Azure](https://portal.azure.com) como Administrador da Conta.
1. Pesquise **Gerenciamento de Custos + Cobrança**.

    ![Captura de tela que mostra a pesquisa](./media/billing-how-to-change-credit-card/search.png)

1. Selecione **Formas de pagamento**.

    ![Captura de tela que mostra a opção Gerenciar formas de pagamento selecionada.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Clique no cartão de crédito que você deseja editar. Um formulário de cartão de crédito será exibido à direita.

    ![Captura de tela que mostra o cartão de crédito selecionado.](./media/billing-how-to-change-credit-card/edit-card-x.png)

1. Atualize os detalhes do cartão de crédito.
1. Clique em **Salvar**.

## <a name="use-a-different-credit-card"></a>Usar um cartão de crédito diferente

Se mais de uma de suas assinaturas tiverem a mesma forma de pagamento ativa, a alteração dessa forma de pagamento ativa em qualquer uma dessas assinaturas também atualizará a das demais.

1. Entre no [Portal do Azure](https://portal.azure.com) como Administrador da Conta.
1. Pesquise **Gerenciamento de Custos + Cobrança**.

    ![Captura de tela que mostra a pesquisa](./media/billing-how-to-change-credit-card/search.png)

1. Selecione a assinatura à qual você deseja adicionar o cartão de crédito.
1. Selecione **Formas de pagamento**.

    ![Captura de tela que mostra a opção Gerenciar formas de pagamento selecionada.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Marque a caixa ao lado do cartão que você gostaria de tornar a forma de pagamento ativa.
1. Clique em **Definir como ativo**.
    ![Captura de tela que mostra o cartão de crédito selecionado e definido como ativo.](./media/billing-how-to-change-credit-card/sub-change-active-x.png)

## <a name="remove-a-credit-card-from-the-account"></a>Remover um cartão de crédito da conta

1. Entre no [Portal do Azure](https://portal.azure.com) como Administrador da Conta.
1. Selecione **Gerenciamento de Custos + Cobrança** do lado esquerdo da página.

    ![Captura de tela que mostra a pesquisa](./media/billing-how-to-change-credit-card/search.png)

1. Em **Cobrança**, selecione **Formas de pagamento**.

    ![Captura de tela que mostra a opção Gerenciar formas de pagamento selecionada.](./media/billing-how-to-change-credit-card/payment-methods-blade-x.png)

1. Selecione a caixa ao lado do cartão que você deseja remover.
1. Clique em **Excluir**.

Se o seu cartão de crédito for a forma de pagamento ativa para as assinaturas da Microsoft, não será possível removê-lo de sua conta do Azure. Altere a forma de pagamento ativa para todas as assinaturas vinculadas a esse cartão de crédito e tente novamente
<!-- # Add, update, or remove a credit card for Azure

In the Account Center, you can add a new credit card, update an existing credit card, or delete a credit card that you don't use. You must be an [Account Administrator](billing-subscription-transfer.md#whoisaa) to make these changes.

**Want to switch to pay by invoice?** See [Pay for Azure subscriptions by invoice](billing-how-to-pay-by-invoice.md).

<a id="addcard"></a>

## Add a new credit card

1. Sign in to the [Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select a subscription.
1. On the right side of the page, select **Manage payment methods**.

    ![Screenshot that shows Manage payment methods option selected.](./media/billing-how-to-change-credit-card/changesub_new.png)
1. Select “+” to add a card.

    ![Screenshot that shows the edit option next to the payment method.](./media/billing-how-to-change-credit-card/editcard_new.png)
1. Enter credit card details.
1. Select **Save**.

If you get an error after you add the credit card, see [Credit card declined at Azure sign-up](billing-credit-card-fails-during-azure-sign-up.md).

## Update existing credit card

If your credit card gets renewed and the number remains the same, update the existing credit card details like the expiration date. If your credit card number changes because the card is lost, stolen, or expired, follow the steps in the [Add a credit card as a payment method](#addcard) section. You don't need to update the CVV.

1. Sign in to the [Azure Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select the subscription that's linked to the card.
1. Select **Manage payment methods**.
1. Select **Edit** next to the card you want to update.
1. Update the credit card details.
1. Select **Save**.

## Use a different credit card for the Azure subscription

1. Sign in to the [Azure Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select the subscription that's linked to the card.
1. On the right side of the page, select **Manage payment methods**.
1. Click **Use Instead** next to the card that you want to use. This also updates any other subscriptions currently associated with this card.

## Remove a credit card from the account

1. Sign in to the [Azure Account Center](https://account.windowsazure.com/Subscriptions) as the [Account Administrator](billing-subscription-transfer.md#whoisaa).
1. Select the subscription that's linked to the card.
3. On the right side of the page, select **Manage payment methods**.
4. Click **Delete** for the credit card that you want to delete.

If your credit card is associated with other active Microsoft subscriptions, you can't remove it from your Azure account. Remove the credit card from all active subscriptions that you have with Microsoft and try again. -->

## <a name="change-payment-method-for-a-billing-profile"></a>Alterar a forma de pagamento de um perfil de cobrança

Para alterar a forma de pagamento de um perfil de cobrança, é necessário ser a pessoa que se inscreveu no Azure.

Se desejar mudar a forma de pagamento padrão para cheque/transferência eletrônica, saiba como [mudar um perfil de cobrança para cheque/transferência eletrônica](billing-how-to-pay-by-invoice.md).

1. Entre no [Portal do Azure](https://portal.azure.com).
1. Pesquise **Gerenciamento de Custos do Azure + Cobrança**.
1. No menu à esquerda, clique em **Perfis de cobrança**.

    ![captura de tela que mostra o perfil de cobrança no menu](./media/billing-how-to-change-credit-card/billing-profile.png)

1. Selecione um perfil de cobrança.
1. No menu à esquerda, selecione **Formas de pagamento**.

   ![Captura de tela que mostra as formas de pagamento no menu](./media/billing-how-to-change-credit-card/billing-profile-payment-methods.png)

1. Em cima da forma de pagamento padrão, clique em **Alterar**.

    ![Captura de tela que mostra o botão alterar](./media/billing-how-to-change-credit-card/customer-led-switch-credit-card.png)

1. Selecione um cartão existente ou adicione um novo.

## <a name="frequently-asked-questions"></a>Perguntas frequentes
As seções a seguir respondem às perguntas mais frequentes sobre como alterar as informações do seu cartão de crédito.

### <a name="my-subscription-is-disabled-why-cant-i-remove-my-credit-card-now"></a>Minha assinatura está desabilitada. Por que não é possível remover o cartão de crédito agora?

Depois que sua assinatura estiver desabilitada ou cancelada, poderemos esperar 90 dias antes de excluir permanentemente a sua assinatura. Mantemos sua forma de pagamento no arquivo durante o período de retenção caso você deseje reativar a assinatura. Após isso, a assinatura será excluída permanentemente.

Se precisar remover seu cartão de crédito antes do período de retenção de 90 dias terminar, [reative sua assinatura](billing-subscription-become-disable.md). Se não conseguir reativar, [entre em contato com o suporte do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="why-do-i-keep-getting-your-login-session-has-expired-please-click-here-to-log-back-in"></a>Por que continuo recebendo "Sua sessão de logon expirou. Clique aqui para fazer logon novamente"?

Se você continuar recebendo essa mensagem de erro mesmo que já tenha feito o logout e voltado, tente novamente com uma sessão de navegação privada.

### <a name="how-do-i-use-a-different-card-for-each-subscription-i-have"></a>Como usar um cartão diferente para cada assinatura que tenho?

Infelizmente, se suas assinaturas já estiverem usando o mesmo cartão, não será possível usar cartões diferentes. No entanto, ao se inscrever para uma nova assinatura, você pode escolher usar uma nova forma de pagamento para essa assinatura.

### <a name="how-do-i-make-payments"></a>Como faço pagamentos?

Se você tiver configurado um cartão de crédito como forma de pagamento, faremos a cobrança no cartão automaticamente após o período de cobrança. Você não precisa fazer nada.

Se estiver [pagando por fatura](billing-how-to-pay-by-invoice.md), envie seu pagamento para o local listado na parte inferior da fatura.

### <a name="how-do-i-change-the-tax-id"></a>Como alterar a ID de imposto?

Para adicionar ou atualizar o TID, atualize seu perfil no [Centro de Contas do Azure](https://account.azure.com/Profile), em seguida, selecione **Registro fiscal**. Essa ID de imposto é usada para cálculos de isenção de impostos e aparece em sua fatura.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Verificar o acesso ao Contrato de Cliente da Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Precisa de ajuda? Entre em contato conosco.

Caso tenha dúvidas ou precise de ajuda, [crie uma solicitação de suporte](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Próximas etapas
- Saiba mais sobre as [reservas do Azure](billing-save-compute-costs-reservations.md) para ver se elas podem fazer você economizar dinheiro.
