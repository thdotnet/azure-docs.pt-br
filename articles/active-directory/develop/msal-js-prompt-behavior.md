---
title: Solicitar o comportamento em solicitações interativas (biblioteca de autenticação do Microsoft para JavaScript) | Azure
description: Saiba mais sobre como personalizar o comportamento do prompt em chamadas interativas usando a biblioteca de autenticação da Microsoft para JavaScript (msal).
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: dd0d736345f312f1a1d6f8f029b41429a3e5f0a7
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544262"
---
# <a name="prompt-behavior-in-msaljs-interactive-requests"></a>Comportamento do prompt em solicitações interativas de msal

Quando um usuário tiver estabelecido um Azure Active Directory sessão AD com várias contas de usuário, na página de entrada do AD do Azure por padrão solicitará que o usuário selecione uma conta antes de entrar. Os usuários não verão uma seleção de conta experiência se houver apenas uma sessão autenticada com o Azure AD.

A biblioteca msal (a partir v0.2.4) não deve enviar um parâmetro de prompt durante as solicitações interativas (`loginRedirect`, `loginPopup`, `acquireTokenRedirect` e `acquireTokenPopup`) e, portanto, não impõe qualquer comportamento de solicitação. Para solicitações de token silenciosas usando o `acquireTokenSilent` método, a msal passa um parâmetro de prompt definido como `none`.

Com base em seu cenário de aplicativo, você pode controlar o comportamento do prompt para solicitações interativas definindo o parâmetro de prompt em parâmetros de solicitação são passados para os métodos. Por exemplo, se você deseja invocar a experiência de seleção de conta:

```javascript
var request = {
    scopes: ["user.read"],
    prompt: 'select_account',
}

userAgentApplication.loginRedirect(request);
```


Os seguintes valores de aviso podem ser passados ao autenticar com o Azure AD:

**login:** Esse valor forçará o usuário insira credenciais na solicitação de autenticação.

**select_account:** Esse valor será fornecer ao usuário uma experiência de seleção de conta listando todas as contas na sessão.

**consent:** Esse valor será invocar a caixa de diálogo de consentimento de OAuth que permite aos usuários conceder permissões para o aplicativo.

**Nenhum:** Esse valor garante que o usuário não verá nenhum prompt interativo. É recomendável não passar esse valor para os métodos interativos em msal, como ele pode ter comportamentos inesperados. Em vez disso, use o `acquireTokenSilent` método de atender a chamadas silenciosas.

## <a name="next-steps"></a>Próximas etapas

Leia mais sobre o `prompt` parâmetro na [concessão implícita do OAuth 2.0](v2-oauth2-implicit-grant-flow.md) protocolo usa qual biblioteca msal.
