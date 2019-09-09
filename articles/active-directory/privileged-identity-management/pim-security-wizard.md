---
title: Assistente de segurança de funções do Azure AD no PIM-Azure Active Directory | Microsoft Docs
description: Descreve o assistente de segurança que você pode usar para converter atribuição de função privilegiadas do Azure Active Directory para qualificados usando o Azure Active Directory Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: curtand
ms.custom: pim ; H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d6d94df29ba16ecee06d70f5edac15a96a4299d
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804020"
---
# <a name="azure-ad-roles-security-wizard-in-pim"></a>Assistente de segurança de funções no Azure Active Directory no PIM

Se você for a primeira pessoa a executar Azure Active Directory (Azure AD) Privileged Identity Management (PIM) para sua organização, você verá um assistente. O assistente ajuda você a entender os riscos à segurança de identidades com privilégios e como usar o PIM para reduzir esses riscos. Você não precisará fazer nenhuma alteração nas atribuições de função existentes no assistente, se preferir fazer isso posteriormente.

## <a name="wizard-overview"></a>Visão geral do assistente

Antes de sua organização começar a usar o PIM, todas as atribuições de função são permanentes: os usuários sempre estarão sempre nessas funções, mesmo se não precisarem dos privilégios no momento. A primeira etapa do assistente mostra uma lista de funções com privilégios altos e quantos usuários atualmente estão nessas funções. Você poderá analisar uma função específica para saber mais sobre os usuários se um ou mais não forem familiares.

A segunda etapa do assistente lhe fornece a oportunidade de alterar as atribuições de função do administrador.  

> [!WARNING]
> É importante que você tenha pelo menos um administrador global e mais de um administrador de função com privilégios com uma conta organizacional (não uma conta da Microsoft). Se houver apenas um administrador de função com privilégios, a organização não poderá gerenciar o PIM se essa conta for excluída.
> Além disso, mantenha as atribuições de função permanentes se um usuário tiver uma conta da Microsoft (uma conta que ele usa para entrar nos serviços Microsoft como o Outlook.com e o Skype). Se você planejar exigir o MFA para ativação para essa função, esse usuário será bloqueado.

## <a name="run-the-wizard"></a>Executar o assistente

1. Entre no [Portal do Azure](https://portal.azure.com/).

1. Abra o **Azure AD Privileged Identity Management**.

1. Clique em **funções do Microsoft Azure Active Directory** e, em seguida, clique em **Assistente**.

    ![Funções do Azure AD – página do assistente mostrando as 3 etapas para executar o assistente](./media/pim-security-wizard/wizard-start.png)

1. Clique em **1 Descobrir funções com privilégios**.

1. Examine a lista de funções com privilégios para ver quais usuários são permanentes ou qualificados.

    ![Descobrir funções privilegiadas – painel de funções mostrando membros permanentes e qualificados](./media/pim-security-wizard/discover-privileged-roles-users.png)

1. Clique em **Avançar** para selecionar os membros que você quer tornar qualificados.

    ![Converter Membros em uma página qualificada com opções para selecionar membros que você deseja tornar qualificados para funções](./media/pim-security-wizard/convert-members-eligible.png)

1. Depois de selecionar os membros, clique em **OK**.

    ![Página examinar alterações mostrando Membros com atribuições de função permanentes que serão convertidas](./media/pim-security-wizard/review-changes.png)

1. Clique em **OK** para converter as atribuições de permanentes para qualificadas.

    Quando a conversão for concluída, você verá uma notificação.

    ![Notificação mostrando o status de uma conversão](./media/pim-security-wizard/notification-completion.png)

Se você precisar converter outras atribuições de função com privilégios para qualificados, você pode executar o assistente novamente. Se você quiser usar a interface PIM em vez do assistente, consulte [atribuir funções do Azure AD no PIM](pim-how-to-add-role-to-user.md).

## <a name="next-steps"></a>Próximas etapas

- [Atribuir funções do Azure AD no PIM](pim-how-to-add-role-to-user.md)
- [Permitir acesso a outros administradores para gerenciar o PIM](pim-how-to-give-access-to-pim.md)
