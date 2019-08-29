---
title: Como desbloquear usuários com o Azure Active Directory Identity Protection | Microsoft Docs
description: Saiba como desbloquear usuários que foram bloqueados por uma política do Azure Active Directory Identity Protection.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4e3756435703c4e8c887a4e7b9d4f75a6701d840
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70126260"
---
# <a name="how-to-unblock-users"></a>Como: Desbloquear usuários

Com o Azure Active Directory Identity Protection, você pode configurar políticas para bloquear os usuários se as condições configuradas forem satisfeitas. Normalmente, um usuário bloqueado entra em contato com o suporte técnico para ser desbloqueado. Este artigo explica as etapas que você pode executar para desbloquear um usuário bloqueado.

## <a name="determine-the-reason-for-blocking"></a>Determinar o motivo do bloqueio

Como uma primeira etapa para desbloquear um usuário, você precisa determinar o tipo de política que bloqueou o usuário porque as próximas etapas dependerão dela.
Com o Azure Active Directory Identity Protection, um usuário pode ser bloqueado ou por uma política de risco de entrada ou por uma política de risco de usuário.

Você pode obter o tipo de política que bloqueou um usuário do título na caixa de diálogo apresentado ao usuário durante uma tentativa de entrada:

| Política | Diálogo do usuário |
| --- | --- |
| Risco de entrada |![Entrada bloqueada](./media/howto-unblock-user/02.png) |
| Risco do usuário |![Conta bloqueada](./media/howto-unblock-user/104.png) |

Um usuário bloqueado por:

* Uma política de risco de entrada também é conhecida como entrada suspeita
* Uma política de risco de usuário também é conhecida como uma conta em risco

## <a name="unblocking-suspicious-sign-ins"></a>Desbloqueio de entradas suspeitas

Para desbloquear uma entrada suspeita, você tem as seguintes opções:

1. **Entrada de um local ou dispositivo familiar** – uma razão comum para entradas suspeitas bloqueadas são as tentativas de entrada de locais ou de dispositivos desconhecidos. Os usuários podem determinar rapidamente se esse é o motivo do bloqueio, tentando entrar de um dispositivo ou local familiar.
2. **Exclusão da política** - se você acha que a configuração atual de sua política de entrada está causando problemas para usuários específicos, pode excluir os usuários dela. Para obter mais informações, consulte [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).
3. **Desabilitar política** - se você achar que sua configuração de política está causando problemas para todos os usuários, poderá desabilitar a política. Para obter mais informações, consulte [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).

## <a name="unblocking-accounts-at-risk"></a>Desbloqueio de contas em risco

Para desbloquear uma conta em risco, você tem as seguintes opções:

1. **Redefinir senha** - você pode redefinir a senha do usuário. 
2. **Ignorar todas as detecções de risco** – a política de risco do usuário bloqueia um usuário se o nível de risco do usuário configurado para bloquear o acesso tiver sido atingido. Você pode reduzir o nível de risco de um usuário fechando manualmente as detecções de risco relatadas. 
3. **Exclusão da política** - se você acha que a configuração atual de sua política de entrada está causando problemas para usuários específicos, pode excluir os usuários dela. Para obter mais informações, consulte [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).
4. **Desabilitar política** - se você achar que sua configuração de política está causando problemas para todos os usuários, poderá desabilitar a política. Para obter mais informações, consulte [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).

## <a name="next-steps"></a>Próximas etapas
 
Você deseja saber mais sobre o Azure AD Identity Protection? Confira [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).
