---
title: Definir as configurações de função do Azure AD no PIM-Azure Active Directory | Microsoft Docs
description: Saiba como definir as configurações de função do Azure AD no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 05/31/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: e9252e3bb8ccddb810074b485f6f073f1bda3f05
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804448"
---
# <a name="configure-azure-ad-role-settings-in-pim"></a>Definir as configurações de função do Azure AD no PIM

Um administrador de função com privilégios pode personalizar o Azure Active Directory (Azure AD) Privileged Identity Management (PIM) em sua organização, incluindo a alteração da experiência de um usuário que está ativando uma atribuição de função qualificada.

## <a name="open-role-settings"></a>Abrir configurações de função

Siga estas etapas para abrir as configurações de uma função do Azure AD.

1. Abra o **Azure AD Privileged Identity Management**.

1. Clique em **funções do Microsoft Azure Active Directory**.

1. Clique em **Configurações**.

    ![Funções do Azure AD – configurações](./media/pim-how-to-change-default-settings/pim-directory-roles-settings.png)

1. Clique em **Funções**.

1. Clique na função cujas configurações você deseja configurar.

    ![Funções do Azure AD – funções de configurações](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-role.png)

    Na página de configurações de cada função, há diversas configurações que você pode realizar. Essas configurações afetam apenas os usuários que são atribuições **elegíveis**, mas não atribuições **permanentes**.

## <a name="activations"></a>Ativações

Use o controle deslizante **Ativações** para definir o tempo máximo, em horas, que uma função permanecerá ativa antes de expirar. Isso pode ser entre 1 e 72 horas.

## <a name="notifications"></a>Notificações

Use as **notificações** para especificar se os administradores receberão notificações por email quando as funções estiverem ativadas. Isso pode ser útil para detectar ativações não autorizadas ou ilegítimas.

Quando definido como **Habilitar**, as notificações são enviadas para:

- Administrador da Função com Privilégios
- Administrador de Segurança
- Administrador Global

Para saber mais, confira [Notificações por email no PIM](pim-email-notifications.md).

## <a name="incidentrequest-ticket"></a>Tíquete de Incidente/Solicitação

Use a opção **Tíquete de Incidente/Solicitação** para especificar se os administradores qualificados devem incluir um número de tíquete quando ativarem a função. Isso pode ser útil ao realizar auditorias de acesso à função.

## <a name="multi-factor-authentication"></a>Autenticação Multifator

Use a opção **Autenticação Multifator** para especificar se os usuários deverão confirmar a identidade com MFA antes de poderem ativar as funções. Os usuários só precisam verificar isso uma vez por sessão, não precisam fazê-lo toda vez que ativam uma função. Há duas dicas para ter em mente quando você habilita a MFA:

* Os usuários que têm contas da Microsoft para seus endereços de email (normalmente @outlook.com, mas nem sempre) não podem se registrar para MFA do Azure. Se quiser atribuir funções aos usuários com contas da Microsoft, você deverá torná-los administradores permanentes ou desabilitar o MFA para essa função.
* Você não pode desabilitar o MFA para funções com altos privilégios do Azure AD e do Office365. Esse é um recurso de segurança, porque estas funções devem ser protegidas com cuidado:  
  
  * Administrador da proteção de informações do Azure
  * Administrador de Cobrança
  * Administrador de Aplicativos de Nuvem
  * Administrador de Conformidade
  * Administrador de Acesso Condicional
  * Administrador de Serviços de CRM
  * Aprovador de Acesso do Sistema de Proteção de Dados do Cliente
  * Gravadores de Diretório
  * Administrador do Exchange
  * Administrador Global
  * Administrador de Serviços do Intune
  * Administrador de Serviços do Power BI
  * Administrador da Função com Privilégios
  * Administrador de Segurança
  * Administrador do Serviço SharePoint
  * Administrador do Skype for Business
  * Administrador de Usuários

Para obter mais informações, consulte [MFA (Autenticação Multifator) e PIM](pim-how-to-require-mfa.md).

## <a name="require-approval"></a>Requer aprovação

Se você quiser exigir aprovação para ativar uma função, siga estas etapas.

1. Defina a opção **Requer aprovação** para **Habilitado**. O painel expande com opções para selecionar aprovadores.

    ![Funções do Azure AD – configurações-exigir aprovação](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval.png)

    Se você **NÃO** especificar nenhum aprovador, os Administradores com Função com privilégios se tornam os aprovadores padrão. Os Administradores com Função com privilégios precisarão aprovar **TODAS** as solicitações de ativação dessa função.

1. Para especificar aprovadores, clique em **Selecionar aprovadores**.

    ![Funções do Azure AD – configurações-exigir aprovação](./media/pim-how-to-change-default-settings/pim-directory-roles-settings-require-approval-select-approvers.png)

1. Selecione um ou mais aprovadores e, em seguida, clique em **Selecionar**. Você pode selecionar usuários ou grupos. Recomenda-se pelo menos 2 aprovadores. Não é permitida a aprovação própria.

    Suas seleções serão exibidas na lista de aprovadores selecionados.

1. Depois que você tiver especificado todas as configurações de função, clique em **salvar** para salvar as alterações.


<!--PLACEHOLDER: Need an explanation of what the temporary Global Administrator setting is for.-->

## <a name="next-steps"></a>Próximas etapas

- [Atribuir funções do Azure AD no PIM](pim-how-to-add-role-to-user.md)
- [Configurar alertas de segurança para funções do Azure AD no PIM](pim-how-to-configure-security-alerts.md)
