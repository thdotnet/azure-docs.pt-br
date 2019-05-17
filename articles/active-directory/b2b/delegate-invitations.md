---
title: Habilitar configurações de colaboração externa do B2B – Azure Active Directory | Microsoft Docs
description: Saiba como habilitar a colaboração externa do B2B do Active Directory e gerenciar quem pode convidar usuários convidados. Use a função de emissor do convite convidado delegar convites.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 11dda7fc3760f468c094fb4cf4484a27895f83b9
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65812682"
---
# <a name="enable-b2b-external-collaboration-and-manage-who-can-invite-guests"></a>Habilitar a colaboração de B2B externa e gerenciar quem pode convidar pessoas

Este artigo descreve como habilitar a colaboração B2B do Azure Active Directory (Azure AD) e determinar quem pode convidar convidados. Por padrão, todos os usuários e convidados em seu diretório podem convidar pessoas, mesmo se eles não tiver sido atribuídos a uma função de administrador. Configurações de colaboração externa permitem que você ative convites ativada ou desativada para diferentes tipos de usuários em sua organização. Você também pode delegar convites para usuários individuais, atribuindo funções que permitem a eles convidar pessoas.

## <a name="configure-b2b-external-collaboration-settings"></a>Definir as configurações de colaboração externa do B2B

Com a colaboração B2B do Azure AD, um administrador de locatário pode definir as seguintes políticas de convite:

- Desativar convites
- Somente administradores e usuários na função Emissor de convite para convidado podem convidar
- Administradores, a função Emissor de convite para convidado e membros podem convidar
- Todos os usuários, incluindo convidados, podem convidar

Por padrão, todos os usuários, incluindo convidados, podem convidar usuários convidados.

### <a name="to-configure-external-collaboration-settings"></a>Para definir configurações de colaboração externa:

1. Entrar para o [portal do Azure](https://portal.azure.com) como um administrador de locatários.
2. Selecione **Azure Active Directory** > **Usuários** > **Configurações de usuário**.
3. Em **Usuários externos**, selecione **Gerenciar configurações de colaboração externa**.
   > [!NOTE]
   > As **configurações de colaboração externa** também estão disponíveis na página **Relações organizacionais**. No Azure Active Directory, em **Gerenciar**, acesse **Relações organizacionais** > **Configurações**.
4. Sobre o **configurações de colaboração externa** , escolha as políticas que você deseja habilitar.

   ![Configurações de colaboração externa](./media/delegate-invitations/control-who-to-invite.png)

  - **Permissões de usuários convidados são limitadas**: Essa política determina as permissões para convidados em seu diretório. Selecione **Sim** para convidados do bloco de determinadas tarefas de diretório, como enumerar usuários, grupos ou outros recursos de diretório. Selecione **não** para fornecer convidados com o mesmo acesso aos dados do diretório como usuários regulares em seu diretório.
   - **Administradores e usuários na função de convidador podem convidar**: Para permitir que administradores e usuários na função "Emissor do convite convidado" convidar pessoas, definir essa política como **Sim**.
   - **Membros podem convidar**: Para permitir que os membros não-administrador do seu diretório convidar pessoas, definir essa política como **Sim**.
   - **Convidados podem convidar**: Para permitir que os convidados convidarem outras pessoas, definir essa política como **Sim**.
   - **Permitir senha de uso único de Email para convidados (visualização)**: Para obter mais informações sobre o recurso de senha de uso único, consulte [autenticação de senha de uso único de Email (visualização)](one-time-passcode.md).
   - **Restrições de colaboração**: Para obter mais informações sobre como permitir ou bloquear convites para domínios específicos, consulte [permitir ou bloquear convites para usuários B2B de organizações específicas](allow-deny-list.md).

## <a name="assign-the-guest-inviter-role-to-a-user"></a>Atribua a função de emissor do convite convidado a um usuário

Com a função de emissor do convite convidado, você pode conceder a usuários individuais a capacidade de convidar pessoas sem atribuir um administrador global ou outra função de administrador. Atribua a função de emissor do convite convidado a indivíduos. Em seguida, verifique se você definiu **administradores e usuários na função de convidador podem convidar** à **Sim**.

Veja um exemplo que mostra como usar o PowerShell para adicionar um usuário à função Emissor de convite para convidado:

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="next-steps"></a>Próximas etapas

Consulte os seguintes artigos na colaboração B2B do Azure AD:

- [O que é a colaboração B2B do AD do Azure?](what-is-b2b.md)
- [Adicionar usuários convidados da colaboração B2B sem um convite](add-user-without-invite.md)
- [Adicionar um usuário de colaboração B2B a uma função](add-guest-to-role.md)


