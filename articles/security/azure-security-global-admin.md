---
title: Habilitar a MFA para todos os administradores do Azure
description: Diretrizes para habilitar o administrador global
ms.service: security
author: barclayn
manager: barbkess
editor: TomSh
ms.topic: article
ms.date: 03/20/2018
ms.author: barclayn
ms.openlocfilehash: 7d40b8f0ca05000a51e70d7a124e9cb143aa2dcf
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67127249"
---
# <a name="enforce-multi-factor-authentication-mfa-for-subscription-administrators"></a>Impor a MFA (autenticação multifator) para administradores de assinaturas

Quando você cria seus administradores, incluindo sua conta de administrador global, é essencial usar métodos de autenticação muito fortes.

Você pode executar a administração cotidiana atribuindo funções específicas de administrador – como administrador do Exchange ou administrador de senhas – para contas de usuário da equipe de TI conforme necessário.
Além disso, habilitar a [Autenticação Multifator (MFA) do Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication) para seus administradores adiciona uma segunda camada de segurança para logons de usuário e transações. A MFA do Azure também ajuda o departamento de TI a reduzir a probabilidade de que uma credencial comprometida tenha acesso aos dados da organização.

Por exemplo:  Impor o MFA do Azure para seus usuários e configurá-lo para usar uma chamada telefônica ou mensagem de texto como verificação. Se as credenciais do usuário estiverem comprometidas, o invasor não será capaz de acessar qualquer recurso, pois ele não terá acesso ao telefone do usuário. As organizações que não adicionam camadas adicionais de proteção de identidade são mais suscetíveis a ataques de roubo de credenciais, que podem levar ao comprometimento dos dados.

Uma alternativa para as organizações que desejam manter todo o controle da autenticação localmente é usar o [Servidor de Autenticação Multifator do Azure](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server), também chamado de "MFA local". Usando esse método você ainda poderá impor a autenticação multifator, mantendo o servidor MFA local.

Você pode verificar quem em sua organização tem privilégios administrativos usando o seguinte comando de PowerShell V2 do Microsoft Azure AD:

```azurepowershell-interactive
Get-AzureADDirectoryRole | Where { $_.DisplayName -eq "Company Administrator" } | Get-AzureADDirectoryRoleMember | Ft DisplayName
```

## <a name="enabling-mfa"></a>Enabling MFA

Revise como a [MFA](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next) opera antes de continuar.

Se os usuários tiverem licenças que incluam a Autenticação Multifator do Microsoft Azure, nada precisa ser feito para ativar o Azure MFA. Você pode começar solicitando uma verificação em duas etapas em base de usuário individual. As licenças que habilitam o Azure MFA são:

- Autenticação Multifator do Azure
- Azure Active Directory Premium
- Enterprise Mobility + Security

## <a name="turn-on-two-step-verification-for-users"></a>Ativar a verificação em duas etapas para usuários

Use um dos procedimentos listados em [Como exigir uma verificação em duas etapas](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-user-states) para um usuário ou grupo para começar a usar o Azure MFA. Você pode optar por impor a verificação em duas etapas para todas as entradas, ou você pode criar políticas de acesso condicional para exigir verificação em duas etapas somente quando for importante para você.

