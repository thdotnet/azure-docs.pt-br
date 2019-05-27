---
title: Políticas de proteção de linha de base de acesso condicional - Azure Active Directory
description: Políticas de acesso condicional de linha de base para proteger as empresas contra ataques comuns
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
ms.openlocfilehash: ca062f4024bb5b0946812e00c8ccc1254b56d333
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66003290"
---
# <a name="what-are-baseline-policies"></a>Quais são as políticas de linha de base?

Diretivas de linha de base são um conjunto de políticas predefinidas que ajudam a proteger as organizações contra vários ataques comuns. Esses ataques comuns podem incluir phishing, reprodução e spray de senha. Diretivas de linha de base estão disponíveis em todas as edições do AD do Azure. Microsoft está disponibilizando essas políticas de proteção de linha de base para todas as pessoas porque ataques baseados em identidade tem sido aumentando ao longo dos últimos anos. A meta dessas políticas quatro é garantir que todas as organizações têm um nível de linha de base de segurança habilitada sem nenhum custo extra.  

Gerenciar políticas de acesso condicional personalizada requer uma licença Azure AD Premium.

## <a name="baseline-policies"></a>Políticas de linha de base

![Políticas de linha de base de acesso condicional no portal do Azure](./media/concept-baseline-protection/conditional-access-baseline-policies.png)

Há quatro diretivas de linha de base que as organizações podem habilitar:

* [Exigir MFA para administradores](howto-baseline-protect-administrators.md)
* [Proteção do usuário final (visualização)](howto-baseline-protect-end-users.md)
* [Autenticação herdada do bloco (visualização)](howto-baseline-protect-legacy-auth.md)
* [Exigir MFA para gerenciamento de serviços (visualização)](howto-baseline-protect-azure.md)

Todos os quatro dessas políticas terá impacto sobre os fluxos de autenticação herdados como POP, IMAP e clientes da área de trabalho mais antigos do Office.

### <a name="require-mfa-for-admins"></a>Exigir MFA para administradores

Devido a potência e o acesso de contas de administrador, você deve tratá-las com cuidado especial. Um método comum para melhorar a proteção de contas privilegiadas é exigir uma forma mais forte de verificação da conta quando eles são usados para entrar. No Azure Active Directory, você pode obter uma verificação de conta mais forte exigindo que os administradores para se registrar e usar a autenticação multifator do Azure.

[Exigir MFA para administradores](howto-baseline-protect-administrators.md) é uma política de linha de base que exige autenticação multifator (MFA) para as seguintes funções de diretório, consideradas as mais privilegiadas funções do Azure AD:

* Administrador global
* Administrador do SharePoint
* Administrador do Exchange
* Administrador de acesso condicional
* Administrador de segurança
* Administrador de assistência técnica / administrador de senha
* Administrador de cobrança
* Administrador de usuários

Se sua organização tiver essas contas em uso no código ou scripts, considere a possibilidade de substituí-los com [identidades gerenciadas](../managed-identities-azure-resources/overview.md). Como solução temporária, você pode excluir contas de usuários específicas da política de linha de base.

### <a name="end-user-protection-preview"></a>Proteção do usuário final (visualização)

Administradores com privilégios altos não são as únicas, direcionadas em ataques. Os atores ruins tendem a direcionar usuários normais. Após obter acesso, esses atores ruins pode solicitar acesso a informações privilegiadas em nome do titular da conta original ou baixar todo o diretório e executar um ataque de phishing em toda a organização. Um método comum para melhorar a proteção para todos os usuários é exigir uma forma mais forte de verificação da conta quando uma entrada arriscada é detectada.

**Proteção do usuário final (versão prévia)** é uma política de linha de base que protege todos os usuários em um diretório. A habilitação dessa política requer que todos os usuários se registrem para autenticação multifator do Azure dentro de 14 dias. Depois de registrado, os usuários serão solicitados para o MFA somente durante as tentativas de entrada arriscadas. Contas de usuário comprometidas serão bloqueadas até que a redefinição de senha e corre o risco de exoneração.

### <a name="block-legacy-authentication-preview"></a>Autenticação herdada do bloco (visualização)

Protocolos de autenticação herdados (ex: IMAP, SMTP, POP3) são protocolos normalmente usados por clientes de email mais antigos para autenticar. Protocolos herdados não suportam a autenticação multifator. Mesmo se você tiver uma diretiva que exija a autenticação multifator para seu diretório, um ator mal-intencionado pode autenticar usando um desses protocolos herdados e ignorar a autenticação multifator.

É a melhor maneira de proteger sua conta das solicitações de autenticação mal-intencionada feitas pelos protocolos herdados para bloqueá-los.

O **autenticação herdada do bloco (visualização)** política de linha de base bloqueia solicitações de autenticação que são feitas usando protocolos herdados. Autenticação moderna deve ser usada para entrar com êxito para todos os usuários. Usado em conjunto com outras políticas de linha de base, as solicitações provenientes de protocolos herdados serão bloqueadas. Além disso, todos os usuários serão necessários para a MFA sempre que for necessário. Essa política não bloqueie o Exchange ActiveSync.

### <a name="require-mfa-for-service-management-preview"></a>Exigir MFA para gerenciamento de serviços (visualização)

As organizações usam uma variedade de serviços do Azure e gerenciá-los no Azure Resource Manager com base em ferramentas como:

* Portal do Azure
* Azure PowerShell
* CLI do Azure

Usando qualquer uma dessas ferramentas para executar o gerenciamento de recursos é uma ação altamente privilegiada. Essas ferramentas podem alterar configurações de toda a assinatura, como configurações de serviço e a cobrança de assinatura.

Para proteger as ações privilegiadas, isso **exigir MFA para o gerenciamento de serviço (versão prévia)** política exigirá a autenticação multifator para qualquer usuário que está acessando o portal do Azure, Azure PowerShell ou CLI do Azure.

## <a name="enable-a-baseline-policy"></a>Habilitar uma política de linha de base

Para habilitar uma política de linha de base:

1. Entrar para o **portal do Azure** como administrador global, administrador de segurança ou administrador de acesso condicional.
1. Navegue até **do Azure Active Directory** > **acesso condicional**.
1. Na lista de políticas, selecione uma política de linha de base que você deseja habilitar.
1. Definir **habilitar política** à **em**.
1. Clique em Salvar.

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte:

* [Cinco etapas para proteger a infraestrutura de identidade](../../security/azure-ad-secure-steps.md)
* [O que é o acesso condicional no Azure Active Directory?](overview.md)
* [Exigir MFA para administradores](howto-baseline-protect-administrators.md)
* [Proteção do usuário final (visualização)](howto-baseline-protect-end-users.md)
* [Autenticação herdada do bloco (visualização)](howto-baseline-protect-legacy-auth.md)
* [Exigir MFA para gerenciamento de serviços (visualização)](howto-baseline-protect-azure.md)