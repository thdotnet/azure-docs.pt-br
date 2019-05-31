---
title: Passar o estado personalizado nas solicitações de autenticação (biblioteca de autenticação do Microsoft para JavaScript) | Azure
description: Saiba como passar um valor de parâmetro de estado personalizado na solicitação de autenticação usando a biblioteca de autenticação da Microsoft para JavaScript (msal).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/29/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f29d84838ddb11ac359d7a04dbce8e39dd05ac01
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420492"
---
# <a name="pass-custom-state-in-authentication-requests-using-msaljs"></a>Passar o estado personalizado em solicitações de autenticação usando msal
O *estado* parâmetro, conforme definido pelo OAuth 2.0 está incluído em uma solicitação de autenticação e também é retornado na resposta de token para impedir ataques de falsificação de solicitação entre sites. Por padrão, a biblioteca de autenticação da Microsoft para JavaScript (msal) passa gerado aleatoriamente exclusivo *estado* valor de parâmetro nas solicitações de autenticação.

O parâmetro de estado também pode ser usado para codificar as informações de estado do aplicativo antes de redirecionamento. Você pode passar o estado do usuário no aplicativo, como a página ou exibição que ele estava, como entrada para esse parâmetro. A biblioteca msal permite que você passe seu estado personalizado como parâmetro de estado no `Request` objeto:

```javascript
// Request type
export type AuthenticationParameters = {
    scopes?: Array<string>;
    extraScopesToConsent?: Array<string>;
    prompt?: string;
    extraQueryParameters?: QPDict;
    claimsRequest?: string;
    authority?: string;
    state?: string;
    correlationId?: string;
    account?: Account;
    sid?: string;
    loginHint?: string;
};
```

Por exemplo:

```javascript
let loginRequest = {
    scopes: ["user.read", "user.write"],
    state: “page_url”
}

myMSALObj.loginPopup(loginRequest);
```

O estado passado é acrescentado ao GUID exclusivo definido pelo msal ao enviar a solicitação. Quando a resposta é retornada, msal verifica se há uma correspondência de estado e, em seguida, retorna o personalizado passado no estado na `Response` objeto como `accountState`.

```javascript
export type AuthResponse = {
    uniqueId: string;
    tenantId: string;
    tokenType: string;
    idToken: IdToken;
    accessToken: string;
    scopes: Array<string>;
    expiresOn: Date;
    account: Account;
    accountState: string;
};
```

Para saber mais, leia sobre [criando um aplicativo de página única (SPA)](scenario-spa-overview.md) usando msal.