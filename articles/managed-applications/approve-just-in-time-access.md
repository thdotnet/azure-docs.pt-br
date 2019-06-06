---
title: Configurar e aprovar o acesso just-in-time para aplicativos gerenciados do Azure
description: Descreve como os consumidores de aplicativos gerenciados do Azure aprovem as solicitações de acesso just-in-time para um aplicativo gerenciado.
author: MSEvanhi
ms.service: managed-applications
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: evanhi
ms.openlocfilehash: 55fbf7292ab894cad3de3de9e96ddc96fe0b79b3
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481708"
---
# <a name="configure-and-approve-just-in-time-access-for-azure-managed-applications"></a>Configurar e aprovar o acesso just-in-time para aplicativos gerenciados do Azure

Como um consumidor de um aplicativo gerenciado, talvez não se sinta confortável dando o publicador acesso permanente ao grupo de recursos gerenciado. Para dar maior controle sobre como conceder acesso aos recursos gerenciados, os aplicativos gerenciados do Azure fornece um recurso chamado acesso de (JIT) just-in-time, que está atualmente em versão prévia. Ele permite que você aprove quando e quanto tempo o publicador tem acesso ao grupo de recursos. O publicador pode tornar as atualizações necessárias durante esse tempo, mas quando esse tempo terminar, o acesso do fornecedor expira.

O fluxo de trabalho de concessão de acesso é:

1. O Editor adiciona um aplicativo gerenciado no Marketplace e especifica que o acesso JIT está disponível.

1. Durante a implantação, você habilita o acesso JIT para sua instância do aplicativo gerenciado.

1. Após a implantação, você pode alterar as configurações para o acesso JIT.

1. O publicador envia uma solicitação de acesso.

1. Aprove a solicitação.

Este artigo aborda as ações que os consumidores executar para habilitar o acesso JIT e aprovar solicitações. Para saber mais sobre como publicar um aplicativo gerenciado com acesso JIT, consulte [solicitar o acesso just-in-time em aplicativos gerenciados do Azure](request-just-in-time-access.md).

> [!NOTE]
> Para usar o acesso just-in-time, você deve ter uma [licença do Active Directory do Azure P2](../active-directory/privileged-identity-management/subscription-requirements.md).

## <a name="enable-during-deployment"></a>Habilitar durante a implantação

1. Entre no [Portal do Azure](https://portal.azure.com).

1. Localize uma entrada de marketplace de um aplicativo gerenciado com o JIT habilitado. Selecione **Criar**.

1. Ao fornecer valores para o novo aplicativo gerenciado, o **configuração de JIT** etapa permite habilitar ou desabilitar o acesso JIT para o aplicativo gerenciado. Selecione **Yes** para **habilitar o acesso JIT**. Essa opção é selecionada por padrão para aplicativos gerenciados que são definidos com o JIT habilitado no marketplace.

   ![Configurar o acesso](./media/approve-just-in-time-access/configure-jit-access.png)

   Só é possível habilitar o acesso JIT durante a implantação. Se você selecionar **não**, o Editor obtém acesso permanente ao grupo de recursos gerenciados. Não é possível habilitar o acesso JIT mais tarde.

1. Para alterar as configurações de aprovação padrão, selecione **personalizar a configuração de JIT**.

   ![Personalizar acesso](./media/approve-just-in-time-access/customize-jit-access.png)

   Por padrão, um aplicativo gerenciado com o JIT habilitado tem as seguintes configurações:

   * Modo de aprovação – automática
   * Duração máxima de acesso – 8 horas
   * Aprovadores – none

   Quando o modo de aprovação é definido como **automática**, os aprovadores recebem uma notificação para cada solicitação, mas a solicitação for aprovada automaticamente. Quando definido como **manual**, os aprovadores recebem uma notificação para cada solicitação e um deles deve aprová-lo.

   A duração máxima da ativação Especifica a quantidade máxima de tempo que um publicador pode solicitar para acesso ao grupo de recursos gerenciado.

   A lista de aprovadores é que os usuários do Active Directory do Azure que podem aprovar JIT solicitações de acesso. Para adicionar um aprovador, selecione **adicionar aprovador** e pesquise o usuário.

   Depois de atualizar a configuração, selecione **salvar**.

## <a name="update-after-deployment"></a>Após a implantação de atualização

Você pode alterar os valores para como as solicitações são aprovadas. No entanto, se você não habilitou o acesso JIT durante a implantação, você não pode habilitá-lo mais tarde.

Para alterar as configurações para um aplicativo gerenciado implantado:

1. No portal, selecione o aplicativo de gerenciamento.

1. Selecione **configuração de JIT** e alterar as configurações conforme necessário.

   ![Alterar configurações de acesso](./media/approve-just-in-time-access/change-settings.png)

1. Ao terminar, escolha **Salvar**.

## <a name="approve-requests"></a>Aprovar solicitações

Quando o publicador solicita acesso, você será notificado da solicitação. Você pode aprovar solicitações de acesso JIT diretamente por meio do aplicativo gerenciado ou em todos os aplicativos gerenciados por meio do serviço do Azure AD Privileged Identity Management. Para usar o acesso just-in-time, você deve ter uma [licença do Active Directory do Azure P2](../active-directory/privileged-identity-management/subscription-requirements.md).

Para aprovar solicitações por meio do aplicativo gerenciado:

1. Selecione **acesso JIT** para o aplicativo gerenciado e selecione **aprovar solicitações**.

   ![Aprovar solicitações](./media/approve-just-in-time-access/approve-requests.png)
 
1. Selecione a solicitação de aprovação.

   ![Selecione a solicitação](./media/approve-just-in-time-access/select-request.png)

1. No formulário, forneça o motivo para a aprovação e selecione **aprovar**.

Para aprovar solicitações por meio do Azure AD Privileged Identity Management:

1. Selecione **todos os serviços** e comece a procurar **Azure AD Privileged Identity Management**. Selecione-o entre as opções disponíveis.

   ![Pesquise por serviço](./media/approve-just-in-time-access/search.png)

1. Selecione **aprovar solicitações**.

   ![Selecione aprovar solicitações](./media/approve-just-in-time-access/select-approve-requests.png)

1. Selecione **aplicativos gerenciados do Azure**e selecione a solicitação de aprovação.

   ![Selecione solicitações](./media/approve-just-in-time-access/view-requests.png)

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre como publicar um aplicativo gerenciado com acesso JIT, consulte [solicitar o acesso just-in-time em aplicativos gerenciados do Azure](request-just-in-time-access.md).
