---
title: Deixar uma organização como um usuário convidado - Azure Active Directory | Microsoft Docs
description: Mostra como um usuário convidado do Azure AD B2B pode deixar uma organização usando o Painel de Acesso.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/13/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 26d9eb883cc014c1bea092a12e22b6d144a37994
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67112982"
---
# <a name="leave-an-organization-as-a-guest-user"></a>Deixar uma organização como um usuário convidado

Um usuário convidado do Azure Active Directory B2B (Azure AD) pode optar por deixar uma organização a qualquer momento se ele não precisar mais usar aplicativos da organização ou manter qualquer associação. Um usuário pode deixar uma organização por conta própria, sem a necessidade de contatar um administrador.

> [!NOTE]
> Um usuário convidado não pode deixar uma organização, se sua conta está desabilitada no locatário inicial ou Locatário do recurso. Se a conta está desativada, o usuário convidado será necessário entrar em contato com o administrador de locatário, que pode excluir a conta de convidado ou habilitar a conta de convidado para que o usuário pode deixar a organização.

## <a name="leave-an-organization"></a>Sair da organização

Para deixar uma organização, siga estas etapas.

1. Acesse a página de perfil do painel de acesso de uma das seguintes etapas:
   
   - No [portal do Azure](https://portal.azure.com), clique em seu nome no canto superior direito e selecione **exibir conta**.
   - Abra seu [painel de acesso](https://myapps.microsoft.com), clique no nome no canto superior direito e próximo a **organizações**, selecione o ícone de configurações (engrenagem).
 
   ![Captura de tela mostrando as configurações do usuário no Painel de Acesso](media/leave-the-organization/UserSettings.png) 

   > [!NOTE]
   > Se você ainda não entrou na organização deseja sair, sob **organizações**, clique no **entre para sair da organização** link ao lado do nome da organização. Depois que você está conectado, clique em seu nome novamente no canto superior direito e lado **organizações**, selecione o ícone de configurações (engrenagem).

3. Em **Organizações**, localizar a organização que deseja sair e selecione **Deixar a organização**.

   ![Captura de tela mostrando a opção Deixar organização na interface do usuário](media/leave-the-organization/LeaveOrg.png)

4. Quando solicitado a confirmar, selecione **Deixar**. 

## <a name="account-removal"></a>Remoção de conta

Quando um usuário deixar uma organização, a conta de usuário é "excluída de forma reversível" no diretório. Por padrão, o objeto de usuário será movido para o **usuários excluídos** área no Azure AD, mas não é excluído permanentemente por 30 dias. Essa exclusão reversível permite ao administrador restaurar a conta de usuário (incluindo grupos e permissões), se o usuário faz uma solicitação para restaurar a conta dentro do período de 30 dias.

Se desejar, um administrador de locatários pode excluir permanentemente a conta a qualquer momento durante o período de 30 dias. Para fazer isso:

1. No [portal do Azure](https://portal.azure.com), selecione **Azure Active Directory**.
2. Em **Gerenciar**, selecione **Usuários**.
3. Selecione **Usuários Excluídos**.
4. Selecione a caixa de seleção próxima a um usuário deletado e então selecione **Deletar permanentemente**.

Se você permanentemente deletar um usuário, esta ação será irrevogável.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Próximas etapas

- Para obter uma visão geral do Azure AD B2B, consulte [O que é a colaboração do Azure AD B2B?](what-is-b2b.md)



