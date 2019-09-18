---
title: Baixar a fatura de cobrança e os dados de uso diário do Azure | Microsoft Docs
description: Descreve como baixar ou exibir a fatura de cobrança e os dados de uso diário do Azure.
keywords: fatura de cobrança, download de fatura, fatura do Azure, uso do Azure
services: billing
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: 16b2eaef74a7aa0e3e28bfcbb6dbd9da568db6cf
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2019
ms.locfileid: "67491399"
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>Baixar ou exibir a fatura de cobrança e os dados de uso diário do Azure

Para a maioria das assinaturas, você pode fazer o download da sua fatura no [portal do Microsoft Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) ou enviá-la por e-mail. Se você for um cliente do Azure com um Contrato Enterprise (cliente da EA), não poderá fazer o download das faturas da sua organização. As faturas são enviadas para quem está configurado para receber as faturas da inscrição.

Se você for um cliente EA ou tiver um [Contrato de Cliente da Microsoft](#check-access-to-a-microsoft-customer-agreement), poderá baixar o uso no [portal do Azure](https://portal.azure.com/). Para outras assinaturas, acesse o [Centro de Contas do Azure](https://account.azure.com/Subscriptions) para baixar o uso.

Apenas determinadas funções têm permissão para receber informações de fatura e uso de faturamento, como o administrador da conta ou o administrador corporativo. Para saber mais sobre como obter acesso a informações de cobrança, consulte [Gerenciar o acesso à cobrança do Azure usando funções](billing-manage-access.md).

Se tiver um Contrato de Cliente da Microsoft, será necessário ter um perfil de cobrança Proprietário, Colaborador, Leitor ou Gerenciador de faturas para exibir informações e uso de cobrança. Para obter mais informações sobre funções de cobrança de Contratos de Cliente da Microsoft, confira [Funções e tarefas do perfil de cobrança](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="download-your-azure-invoices-pdf"></a>Baixe suas faturas do Azure (.pdf)

Para a maioria das assinaturas, você pode baixar sua fatura do portal do Azure. Se você tiver um Contrato de Cliente da Microsoft, confira Baixar faturas para um perfil de cobrança.

### <a name="download-invoices-for-an-individual-subscription"></a>Baixar faturas para uma assinatura individual

1. Selecione sua assinatura da [página Assinaturas](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) no portal do Azure como [um usuário com acesso a faturas](billing-manage-access.md).

2. Selecione **Faturas**.

    ![Captura de tela que mostra a opção de Cobrança e Uso](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png)

3. Clique em **Baixar Fatura** para exibir uma cópia da sua fatura em PDF. Se o resultado for **Não disponível**, veja [Por que não encontro uma fatura do último período de cobrança?](#noinvoice)

    ![Captura de tela que mostra os períodos de cobrança, a opção de download e as cobranças totais para cada período de cobrança](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Você também pode exibir seu uso diário clicando no período de cobrança.

Para saber mais sobre a fatura, confira [Entenda sua fatura do Microsoft Azure](billing-understand-your-bill.md). Para obter ajuda sobre como gerenciar os custos, confira [Evitar custos inesperados com o gerenciamento de custos e a cobrança do Azure](billing-getting-started.md).

### <a name="download-invoices-for-a-microsoft-customer-agreement"></a>Baixar faturas para um Contrato de Cliente da Microsoft

As faturas são geradas para cada [perfil de cobrança](billing-mca-overview.md#billing-profiles) no Contrato de Cliente da Microsoft. É necessário ter um perfil de cobrança Proprietário, Colaborador, Leitor ou Gerenciador de faturas para baixar faturas do portal do Azure.

1. Pesquise **Gerenciamento de Custos + Cobrança**.
2. Selecione um perfil de cobrança.
3. Selecione **Faturas**.
4. Na grade da fatura, encontre a linha da fatura que você deseja baixar.
5. Clique nas reticências (`...`) no fim da linha.
6. No menu de contexto do download, selecione **Fatura**.

Se você não vir uma fatura no último período de cobrança, confira **Informações adicionais**. <!-- Fix this -->
### <a name="noinvoice"></a> Por que não encontro uma fatura do último período de cobrança?

Pode haver vários motivos pelos quais você não vê uma fatura:

- Faz menos de 30 dias que você se inscreveu no Azure.

- A fatura não foi gerada ainda. Aguarde até o final do período de cobrança.

- Você não tem permissão para exibir faturas. Se tiver um Contrato de Cliente da Microsoft, você deverá ter um perfil de cobrança Proprietário, Colaborador, Leitor ou Gerenciador de faturas. Para outras assinaturas, talvez você não veja faturas antigas se não for o Administrador da Conta. Para saber mais sobre como obter acesso a informações de cobrança, consulte [Gerenciar o acesso à cobrança do Azure usando funções](billing-manage-access.md).

- Se você tiver uma Avaliação Gratuita ou uma quantidade de crédito mensal com sua assinatura que você não excedeu, você não receberá uma fatura, a menos que tenha um Contrato de Cliente da Microsoft.

## <a name="get-your-invoice-in-email-pdf"></a>Obter sua fatura por email (.pdf)

Você pode aceitar e configurar destinatários adicionais para receber sua fatura do Azure em um email. Esse recurso pode não estar disponível para determinadas assinaturas, como ofertas de suporte, Enterprise Agreements ou Azure via Open. Se você tiver um Contrato de Cliente da Microsoft, confira Receber suas faturas de perfil de cobrança no email.

### <a name="get-your-subscriptions-invoices-in-email"></a>Receber as faturas de sua assinatura no email

1. Selecione sua assinatura na [página Assinaturas](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Aceitar para cada assinatura que você possui. Clique em **Faturas** e em **Enviar minha fatura por email**.

    ![Captura de tela que mostra o fluxo de aceitação](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)

2. Clique em **Aceitar** e aceite os termos.

    ![Captura de tela que mostra a etapa 2 do fluxo de aceitação](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)

3. Depois de aceitar o contrato, você poderá configurar destinatários adicionais. Quando um destinatário é removido, o endereço de email não é armazenado. Se você mudar de ideia, você precisa adicioná-los novamente.

    ![Captura de tela que mostra a etapa 3 do fluxo de aceitação](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)

Se você não receber um email depois de seguir as etapas, verifique se seu endereço de email está correto nas [opções de comunicação de seu perfil](https://account.windowsazure.com/profile).

### <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>Cancelar o recebimento das faturas de sua assinatura no email

É possível cancelar o recebimento de sua fatura por email seguindo as etapas abaixo e clicando em **Recusar faturas enviadas por email**. Essa opção remove todos os endereços de e-mail configurados para receber faturas por e-mail. Você poderá reconfigurar os destinatários se aceitar receber novamente.

 ![Captura de tela que mostra o fluxo para recusar](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Receber as faturas do Contrato de Cliente da Microsoft no email

Se você tiver um Contrato de Cliente da Microsoft, poderá aceitar receber sua fatura em um email. Todos os perfis de cobrança Proprietários, Colaboradores, Leitores e Gerenciadores de faturas receberão a fatura por email. Os leitores não podem atualizar a preferência de fatura por email.

1. Pesquise **Gerenciamento de Custos + Cobrança**.
1. Selecione um perfil de cobrança.
1. Em **Configurações**, selecione **Propriedades**.
1. Em **Fatura por Email**, selecione **Atualizar preferência de fatura por email**.
1. Selecione **Aceitar**.
1. Clique em **Atualizar**.

### <a name="opt-out-of-getting-your-billing-profile-invoices-in-email"></a>Cancelar o recebimento de faturas de perfil de cobrança no email

É possível cancelar o recebimento de sua fatura por email seguindo as etapas acima e clicando em **Recusar**. Todos os Proprietários, Colaboradores, Leitores e Gerenciadores de faturas também não receberão mais a fatura por email. Se você for Leitor, não poderá alterar a preferência de fatura por email.

## <a name="download-usage"></a>Fazer o download de uso

 Para a maioria das assinaturas, encontre seu arquivo de uso diário no [ Centro de Contas do Azure ](https://account.azure.com/Subscriptions). Se você for cliente EA ou tiver um Contrato de Cliente da Microsoft, poderá baixar o uso no [portal do Azure](https://portal.azure.com/). <!-- TO DO: update PayG experience to Ibiza once it ships-->

### <a name="download-usage-from-the-account-center-csv"></a>Baixar uso do Centro de Contas (.csv)

1. Entre no [Centro de Contas do Azure](https://account.windowsazure.com/subscriptions) como o Administrador da Conta.

2. Selecione a assinatura da qual deseja obter informações de fatura e uso.

3. Clique em **HISTÓRICO DE COBRANÇA**.

    ![Captura de tela que mostra a opção de histórico de cobrança](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. Você pode ver os demonstrativos dos últimos seis períodos de cobrança e o período atual não faturado.

    ![Captura de tela que mostra os períodos de cobrança, as opções para baixar a fatura e o uso diário e as cobranças totais para cada período de cobrança](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. Selecione **Exibir Demonstrativo Atual** para ver uma estimativa dos seus encargos no momento em que a estimativa foi gerada. Essas informações são atualizadas diariamente e talvez não incluam todo o seu uso. Sua fatura mensal pode ser diferente dessa estimativa.

    ![Captura de tela que mostra a opção de Exibir Demonstrativo Atual](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Captura de tela que mostra a estimativa das cobranças atuais](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. Selecione **Baixar Uso** para baixar os dados de uso diário como um arquivo CSV. Se vir duas versões disponíveis, baixe a versão 2.

    ![Captura de tela que mostra a opção de Baixar Uso](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

Somente o Administrador da Conta pode acessar o Centro de Contas do Azure. Outros administradores de cobrança, por exemplo um proprietário, podem obter informações de uso usando as [APIs de cobrança](billing-usage-rate-card-overview.md).

Para saber mais sobre o uso diário, confira [Entenda sua fatura do Microsoft Azure](billing-understand-your-bill.md). Para obter ajuda sobre como gerenciar os custos, confira [Evitar custos inesperados com o gerenciamento de custos e a cobrança do Azure](billing-getting-started.md).

### <a name="download-usage-for-ea-customers"></a>Faça o download uso para clientes do EA

Para exibir e baixar os dados de uso como cliente do EA, você deverá ser administrador corporativo, proprietário da conta ou administrador do departamento com a política de cobranças de exibição habilitada.

1. Entre no [Portal do Azure](https://portal.azure.com).
1. Pesquise *Gerenciamento de Custos + Cobrança*.

    ![Captura de tela que mostra a pesquisa do portal do Azure](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. Selecione **uso + encargos**.
1. No mês que você deseja fazer o download, selecione **fazer o download**.

### <a name="download-usage-for-your-microsoft-customer-agreement"></a>Baixar uso para seu Contrato de Cliente da Microsoft

Para exibir e baixar os dados de uso para um perfil de cobrança, você deverá ter o perfil de cobrança Proprietário, Colaborador, Leitor ou Gerenciador de faturas.

#### <a name="download-usage-for-billed-charges"></a>Baixar uso para encargos cobrados

1. Pesquise **Gerenciamento de Custos + Cobrança**.
2. Selecione um perfil de cobrança.
3. Selecione **Faturas**.
4. Na grade da fatura, encontre a linha da fatura correspondente ao uso que você deseja baixar.
5. Clique nas reticências (`...`) no fim da linha.
6. No menu de contexto do download, selecione **Uso e encargos do Azure**.

#### <a name="download-usage-for-open-charges"></a>Baixar uso para encargos abertos

Também é possível baixar o uso do mês atual para o período de cobrança atual, o que significa que os encargos ainda não foram cobrados.

1. Pesquise **Gerenciamento de Custos + Cobrança**.
2. Selecione um perfil de cobrança.
3. Na folha **Visão geral**, clique em **Baixar uso e encargos do Azure**.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Verificar o acesso ao Contrato de Cliente da Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Precisa de ajuda? Fale conosco

Caso tenha dúvidas ou precise de ajuda, [crie uma solicitação de suporte](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre a fatura e os encargos, confira:

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
