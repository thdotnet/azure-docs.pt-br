---
title: Criar uma assinatura adicional do Azure para sua conta de cobrança
description: Saiba como adicionar uma nova assinatura do Azure no portal do Azure.
author: amberbhargava
manager: amberb
editor: banders
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: e1fafafe5f67d1775ca80a1f7c2aff4dec9e0bc4
ms.sourcegitcommit: d4c9821b31f5a12ab4cc60036fde00e7d8dc4421
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/01/2019
ms.locfileid: "71709584"
---
# <a name="create-an-additional-azure-subscription-for-microsoft-customer-agreement"></a>Criar uma assinatura adicional do Azure para o Contrato de Cliente da Microsoft

Crie assinaturas adicionais para sua conta de cobrança a fim de configurar ambientes separados para desenvolvimento e teste, segurança ou para isolar dados por motivos de conformidade.

Este artigo aplica-se a uma conta de cobrança para um Contrato de Cliente da Microsoft. [Verifique se você tem acesso a um Contrato de Cliente da Microsoft](#check-access). Caso deseje criar assinaturas para outros tipos de contas de cobrança, confira [Criar uma assinatura adicional no portal do Azure](billing-create-subscription.md).

Para criar uma assinatura, você precisa ser um **proprietário da seção da fatura**, um **colaborador da seção da fatura** ou um **criador de assinatura do Azure**. Para obter mais informações, confira [Funções e tarefas da cobrança de assinatura](billing-understand-mca-roles.md#subscription-billing-roles-and-tasks). Para fornecer a outros usuários as permissões para criar assinaturas do Azure para sua conta de cobrança, confira [Conceder a outros usuários a permissão para criar assinaturas do Azure](#give-others-permission).

## <a name="create-a-subscription"></a>Criar uma assinatura

1. Entre no [Portal do Azure](https://portal.azure.com).

2. Pesquise **Assinaturas**.

   ![Captura de tela que mostra a pesquisa no portal para assinaturas](./media/billing-mca-create-subscription/billing-search-subscriptions.png)

3. Selecione **Adicionar**

4. Caso você tenha acesso a várias contas de cobrança, selecione a conta de cobrança de seu Contrato de Cliente da Microsoft.

   ![Captura de tela que mostra a página Criar assinatura](./media/billing-mca-create-subscription/billing-mca-create-azure-subscription.png)

5. Selecione um perfil de cobrança. Os encargos de sua assinatura serão faturados no perfil de cobrança selecionado. Caso você tenha acesso a apenas um perfil de cobrança, a seleção ficará esmaecida.

6. Selecione uma seção da fatura. Os encargos de sua assinatura serão faturados nesta seção da fatura do perfil de cobrança. Caso você tenha acesso a apenas uma seção da fatura, a seleção ficará esmaecida.

7. Selecione um plano para a assinatura. Selecione **Plano do Microsoft Azure para DevTest** se pretender usar essa assinatura para cargas de trabalho de desenvolvimento ou teste, caso contrário, use **Plano do Microsoft Azure**. Caso você tenha acesso a apenas um plano, a seleção ficará esmaecida.

8. Insira um nome para a assinatura. O nome ajuda você a identificar com facilidade a assinatura no portal do Azure.

9. Selecione **Criar**.

## <a name="give-others-permission"></a>Conceder permissão a outros usuários

Adicione usuários como criadores de assinatura do Azure em uma seção da fatura para conceder a eles permissão para criar assinaturas do Azure.

1. Entre no [Portal do Azure](https://portal.azure.com).

2. Pesquise **Gerenciamento de Custos + Cobrança**.

   ![Captura de tela que mostra a pesquisa no portal para assinaturas](./media/billing-mca-create-subscription/billing-search-cost-management-billing.png)

3. Acesse a seção de fatura. Dependendo de seu acesso, talvez você precise selecionar uma conta de cobrança ou um perfil de cobrança. Na conta ou no perfil de cobrança, selecione **Seções da fatura** e, em seguida, uma seção da fatura na lista. Todas as assinaturas que serão criadas pelos usuários serão faturadas nesta seção da fatura.
   
   ![Captura de tela que mostra a seleção das seções da fatura](./media/billing-mca-create-subscription/mca-select-invoice-sections.png)        

4. Selecione **Gerenciamento de Acesso (IAM)** no lado superior esquerdo.

5. Na parte superior da página, selecione **Adicionar**.

6. Selecione **Criador de assinatura do Azure** para a função.

7. Insira o endereço de email do usuário a quem você deseja permitir acesso.

8. Clique em **Salvar**.

## <a name="check-access"></a>Verificar acesso
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-support"></a>Precisa de ajuda? Contate o suporte

Caso tenha dúvidas ou precise de ajuda, [crie uma solicitação de suporte](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Próximas etapas

- [Conceder a outros usuários a permissão para criar recursos do Azure usando funções internas](../role-based-access-control/built-in-roles.md#built-in-role-descriptions)
- [Criar uma máquina virtual do Windows](../virtual-machines/windows/quick-create-portal.md)
- [Criar uma máquina virtual do Linux](../virtual-machines/linux/quick-create-portal.md)
- [Crie grupos de gerenciamento para organização e gerenciamento de recursos](../governance/management-groups/create.md?toc=/azure/billing/TOC.json)
