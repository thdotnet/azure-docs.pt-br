---
title: Exibir e baixar sua fatura do Microsoft Azure
description: Descreve como exibir e baixar sua fatura do Microsoft Azure.
keywords: fatura de cobrança, download de fatura, fatura do Azure, uso do Azure
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: 862ec629504da4d8ee7eadd8f9b925984d96614c
ms.sourcegitcommit: a19f4b35a0123256e76f2789cd5083921ac73daf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71718884"
---
# <a name="view-and-download-your-microsoft-azure-invoice"></a>Exibir e baixar sua fatura do Microsoft Azure

Para a maioria das assinaturas, você pode fazer o download da sua fatura no [portal do Microsoft Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ou enviá-la por e-mail. Se você for um cliente do Azure com um Contrato Enterprise (cliente da EA), não poderá fazer o download das faturas da sua organização. As faturas são enviadas para quem está configurado para receber as faturas da inscrição.

Apenas determinadas funções têm permissão para exibir faturas. Por exemplo, o administrador da conta ou o administrador corporativo. Para obter mais informações sobre como obter acesso às informações de cobrança, confira [Gerenciar acesso à cobrança do Azure usando funções](billing-manage-access.md).

Se você tiver um [Contrato de Cliente da Microsoft](#check-your-access-to-a-microsoft-customer-agreement), deverá ter uma das seguintes funções para receber suas faturas:

- Proprietário do perfil de cobrança
- Colaborador
- Leitor
- Gerenciador de faturas

Para saber mais sobre funções de cobrança de Contratos de Cliente da Microsoft, confira [Funções e tarefas do perfil de cobrança](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

## <a name="noinvoice"></a> Por que você não pode receber uma fatura

Pode haver vários motivos pelos quais você não vê uma fatura:

- Faz menos de 30 dias que você se inscreveu no Azure.

- O Azure cobra você no final do período de faturamento. Portanto, uma fatura ainda pode não ter sido gerada. Aguarde até o final do período de cobrança.

- Você não tem permissão para exibir faturas. Se tiver um Contrato de Cliente da Microsoft, você deverá ter um perfil de cobrança Proprietário, Colaborador, Leitor ou Gerenciador de faturas. Para outras assinaturas, talvez você não veja faturas antigas se não for o Administrador da Conta. Para saber mais sobre como obter acesso a informações de cobrança, consulte [Gerenciar o acesso à cobrança do Azure usando funções](billing-manage-access.md).

- Se você tiver uma Avaliação Gratuita ou uma quantidade de crédito mensal com sua assinatura, você só receberá uma fatura quando exceder a quantidade de crédito mensal. Se você tiver um Contrato de Cliente da Microsoft, sempre receberá uma fatura.

## <a name="download-your-azure-invoices-pdf"></a>Baixe suas faturas do Azure (.pdf)

Para a maioria das assinaturas, você pode baixar sua fatura do portal do Azure. Se você tiver um Contrato de Cliente da Microsoft, confira [Baixar faturas para um Contrato de Cliente da Microsoft](#download-invoices-for-a-microsoft-customer-agreement).

### <a name="download-invoices-for-an-individual-subscription"></a>Baixar faturas para uma assinatura individual

1. Selecione sua assinatura na [página Assinaturas](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) no portal do Azure como [um usuário com acesso a faturas](billing-manage-access.md).

2. Selecione **Faturas**.

    ![Captura de tela que mostra a opção de Cobrança e Uso](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png)

3. Clique em **Baixar Fatura** para exibir uma cópia da sua fatura em PDF. Se o resultado for **Não disponível**, veja [Por que não encontro uma fatura do último período de cobrança?](#noinvoice)

    ![Captura de tela que mostra os períodos de cobrança, a opção de download e as cobranças totais para cada período de cobrança](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Você também pode exibir seu uso diário clicando no período de cobrança.

Para saber mais sobre a fatura, confira [Entenda sua fatura do Microsoft Azure](billing-understand-your-bill.md). Para obter ajuda com o gerenciamento de custos, confira [Evitar custos inesperados com o gerenciamento de custos e a cobrança do Azure](billing-getting-started.md).

### <a name="download-invoices-for-a-microsoft-customer-agreement"></a>Baixar faturas para um Contrato de Cliente da Microsoft

As faturas são geradas para cada [perfil de cobrança](billing-mca-overview.md#billing-profiles) no Contrato de Cliente da Microsoft. É necessário ter um perfil de cobrança Proprietário, Colaborador, Leitor ou Gerenciador de faturas para baixar faturas do portal do Azure.

1. Entre no [Portal do Azure](https://portal.azure.com).
1. Pesquise *Gerenciamento de Custos + Cobrança*.
1. Selecione um perfil de cobrança. Dependendo de seu acesso, talvez você precise selecionar uma conta de cobrança primeiro.
1. Selecione **Faturas**.
1. Na grade da fatura, encontre a linha da fatura que você deseja baixar.
1. Clique nas reticências (`...`) no final da linha.
    ![Captura de tela que mostra as reticências no fim da linha](./media/billing-download-azure-invoice/billingprofile-invoicegrid.png)
1. No menu de contexto do download, selecione **Fatura**.

    ![Captura de tela que mostra o menu de contexto](./media/billing-download-azure-invoice/contextmenu.png)

Se você não vir uma fatura para o último período de cobrança, confira [Por que eu não vejo uma fatura para o último período de cobrança?](#noinvoice)

## <a name="get-your-invoice-in-email-pdf"></a>Obter sua fatura por email (.pdf)

Você pode aceitar e configurar destinatários adicionais para receber sua fatura do Azure em um email. Esse recurso pode não estar disponível para determinadas assinaturas, como ofertas de suporte, Enterprise Agreements ou Azure via Open. Se você tiver um Contrato de Cliente da Microsoft, confira a próxima seção, [Receber suas faturas de perfil de cobrança no email](#get-your-subscriptions-invoices-in-email).

### <a name="get-your-subscriptions-invoices-in-email"></a>Receber as faturas de sua assinatura no email

1. Selecione sua assinatura na [página Assinaturas](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Aceitar para cada assinatura que você possui. Clique em **Faturas** e em **Enviar minha fatura por email**.

    ![Captura de tela que mostra o fluxo de aceitação](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)

2. Clique em **Aceitar** e aceite os termos.

    ![Captura de tela que mostra a etapa 2 do fluxo de aceitação](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)

3. Depois de aceitar o contrato, você poderá configurar destinatários adicionais. Quando um destinatário é removido, o endereço de email não é armazenado. Se você mudar de ideia, precisará adicioná-los novamente.

    ![Captura de tela que mostra a etapa 3 do fluxo de aceitação](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)

Se você não receber um email depois de seguir as etapas, verifique se seu endereço de email está correto nas [opções de comunicação de seu perfil](https://account.windowsazure.com/profile).

### <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>Cancelar o recebimento das faturas de sua assinatura no email

Para cancelar o recebimento de sua fatura por email, siga as etapas anteriores e clique em **Recusar faturas enviadas por email**. Essa opção remove todos os endereços de e-mail configurados para receber faturas por e-mail. Você poderá reconfigurar os destinatários se aceitar receber novamente.

 ![Captura de tela que mostra o fluxo para recusar](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Receber as faturas do Contrato de Cliente da Microsoft no email

Se você tiver um Contrato de Cliente da Microsoft, poderá aceitar receber sua fatura em um email. Todos os perfis de cobrança Proprietários, Colaboradores, Leitores e Gerenciadores de faturas receberão a fatura por email. Os leitores não podem atualizar a preferência de fatura por email.

1. Entre no [Portal do Azure](https://portal.azure.com).
1. Pesquise **Gerenciamento de Custos + Cobrança**.
1. Selecione um perfil de cobrança. Dependendo de seu acesso, talvez você precise selecionar uma conta de cobrança primeiro.
1. Em **Configurações**, selecione **Propriedades**.
1. Em **Fatura por Email**, selecione **Atualizar preferência de fatura por email**.

    ![Captura de tela que mostra propriedades da fatura por email](./media/billing-download-azure-invoice/billingprofile-email.png)

1. Selecione **Aceitar**.
1. Clique em **Atualizar**.

### <a name="opt-out-of-getting-your-microsoft-customer-agreement-invoices-in-email"></a>Cancelar o recebimento de suas faturas de Contrato de Cliente da Microsoft no email

Para cancelar o recebimento de sua fatura por email, siga as etapas anteriores e clique em **Recusar**. Todos os Proprietários, Colaboradores, Leitores e Gerenciadores de faturas também não recebem mais a fatura por email. Se você for Leitor, não poderá alterar a preferência de fatura por email.



## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Verificar acesso ao Contrato de Cliente da Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Precisa de ajuda? Entre em contato conosco.

Se você tiver dúvidas ou precisar de ajuda, [crie uma solicitação de suporte](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre a fatura e os encargos, confira:

- [Exibir e baixar o uso e os encargos do Microsoft Azure](billing-download-azure-daily-usage.md)
- [Entenda sua fatura do Microsoft Azure](billing-understand-your-bill.md)
- [Noções básicas sobre os termos em sua fatura do Azure](billing-understand-your-invoice.md)
- [Compreender os termos em uso detalhado do Microsoft Azure](billing-understand-your-usage.md)
- [Exibir os preços do Azure da sua organização](billing-ea-pricing.md)

Se você tiver um Contrato de Cliente da Microsoft, confira:

- [Noções básicas sobre encargos na fatura do seu perfil de cobrança](billing-mca-understand-your-bill.md)
- [Noções básicas sobre termos na fatura do seu perfil de cobrança](billing-mca-understand-your-invoice.md)
- [Noções básicas sobre o arquivo de uso e encargos do Azure do seu perfil de cobrança](billing-mca-understand-your-usage.md)
- [Exibir e baixar documentos de imposto do seu perfil de cobrança](billing-mca-download-tax-document.md)
- [Exibir os preços do Azure da sua organização](billing-ea-pricing.md)
