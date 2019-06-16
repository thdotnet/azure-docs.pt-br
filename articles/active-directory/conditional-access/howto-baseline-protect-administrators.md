---
title: Política de linha de base exigir MFA para administradores - Azure Active Directory
description: Política de acesso condicional para exigir a autenticação multifator para administradores
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4f8b7f281ad5ed8424110696544ffdb49e50ce59
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67112436"
---
# <a name="baseline-policy-require-mfa-for-admins"></a>Política de linha de base: Exigir MFA para administradores

Usuários com acesso a contas privilegiadas têm acesso irrestrito a seu ambiente. Devido à capacidade que essas contas têm, devem ser tratadas com cuidado especial. Um método comum para melhorar a proteção de contas privilegiadas é exigir uma forma mais forte de verificação de conta quando elas são usadas para entrar. No Azure Active Directory, você pode obter uma verificação de conta mais forte exigindo MFA (autenticação multifator).

**Exigir MFA para administradores** é um [política de linha de base](concept-baseline-protection.md) que requer a MFA sempre que uma das seguintes funções de administrador com privilégios entra em:

* Administrador global
* Administrador do SharePoint
* Administrador do Exchange
* Administrador de acesso condicional
* Administrador de segurança
* Administrador de assistência técnica / administrador de senha
* Administrador de cobrança
* Administrador de usuários

Após a habilitação da exigir MFA para os administradores política, as funções de nove administrador acima precisará se registrar para MFA usando o aplicativo autenticador. Depois que o registro de MFA for concluído, os administradores precisarão executar a MFA toda vez que entrar.

![Exigir MFA para a política de linha de base de administradores](./media/howto-baseline-protect-administrators/baseline-policy-require-mfa-for-admins.png)

## <a name="deployment-considerations"></a>Considerações de implantação

Porque o **exigir MFA para que os administradores** política se aplica a todos os administradores críticos, várias considerações precisam ser feitas para assegurar uma implantação tranquila. Essas considerações incluem a identificação de usuários e entidades de serviço no Azure AD que não é possível ou não deve realizar a MFA, bem como aplicativos e clientes usados pela sua organização que não dão suporte a autenticação moderna.

### <a name="legacy-protocols"></a>Protocolos herdados

Protocolos de autenticação herdados (IMAP, SMTP, POP3, etc.) são usados por clientes de email para fazer solicitações de autenticação. Esses protocolos não oferecem suporte a MFA. Com muita frequência os comprometimentos de conta vistos pela Microsoft é causadas por atores ruins executar ataques contra protocolos herdados, a tentativa de ignorar o MFA. Para garantir que o MFA é exigido ao fazer logon em uma conta administrativa e atores ruins não é possível ignorar o MFA, esta política bloqueia todas as solicitações de autenticação feitas em contas de administrador de protocolos herdados.

> [!WARNING]
> Antes de habilitar essa política, verifique se que os administradores não estão usando protocolos de autenticação herdados. Consulte o artigo [como: Autenticação herdados do bloco para o Azure AD com acesso condicional](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) para obter mais informações.

### <a name="user-exclusions"></a>Exclusões de usuário

Essa política de linha de base fornece a opção de excluir usuários. Antes de habilitar a política para o seu locatário, é recomendável, exceto as seguintes contas:

* **Acesso de emergência** ou **vigilância** contas para impedir que o bloqueio de conta de todo o locatário. No cenário improvável que todos os administradores estão bloqueados de seu locatário, sua conta administrativa de acesso de emergência pode ser usada para fazer logon em etapas locatário take para recuperar o acesso.
   * Mais informações podem ser encontradas no artigo [gerenciar contas de acesso de emergência no Azure AD](../users-groups-roles/directory-emergency-access.md).
* **Contas de serviço** e **entidades de serviço**, como a conta do Azure AD Connect Sync. Contas de serviço são contas não-interativas que não estão vinculadas a nenhum usuário específico. Eles normalmente são usados pelos serviços de back-end e permitam o acesso programático aos aplicativos. Contas de serviço devem ser excluídas, pois o MFA não pode ser concluída por meio de programação.
   * Se sua organização tiver essas contas em uso no código ou scripts, considere a possibilidade de substituí-los com [identidades gerenciadas](../managed-identities-azure-resources/overview.md). Como solução temporária, você pode excluir essas contas específicas da política de linha de base.
* Usuários que não têm ou não será capaz de usar um Smartphone.
   * Essa política requer que os administradores para se registrar para MFA usando o aplicativo Microsoft Authenticator.

## <a name="enable-the-baseline-policy"></a>Habilitar a política de linha de base

A política **política de linha de base: Exigir MFA para os administradores** vem pré-configurada e aparecerá na parte superior quando você navega até a folha de acesso condicional no portal do Azure.

Para habilitar essa política e proteger seus administradores:

1. Entrar para o **portal do Azure** como administrador global, administrador de segurança ou administrador de acesso condicional.
1. Navegue até **do Azure Active Directory** > **acesso condicional**.
1. Na lista de políticas, selecione **política de linha de base: Exigir MFA para os administradores**.
1. Definir **habilitar política** à **usar a política imediatamente**.
1. Adicionar exclusões usuário clicando em **os usuários** > **selecionar usuários excluídos** e escolhendo os usuários que precisam ser excluídas. Clique em **selecionar** , em seguida, **feito**.
1. Clique em **salvar**.

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte:

* [Políticas de proteção de linha de base de acesso condicional](concept-baseline-protection.md)
* [Cinco etapas para proteger a infraestrutura de identidade](../../security/azure-ad-secure-steps.md)
* [O que é acesso condicional no Azure Active Directory?](overview.md)
