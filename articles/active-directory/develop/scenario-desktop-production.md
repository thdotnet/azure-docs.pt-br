---
title: Aplicativo de desktop que chamadas às APIs da web (mover para a produção) – plataforma de identidade da Microsoft
description: Saiba como criar um aplicativo de Desktop que chamadas de web APIs (mover para a produção)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2343a416bd810792e7267b94395f953aa4f880a1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111194"
---
# <a name="desktop-app-that-calls-web-apis---move-to-production"></a>Aplicativo de área de trabalho que chama o web APIs - passar para a produção

Este artigo fornece detalhes para melhorar ainda mais seu aplicativo e mova-o para produção.

## <a name="handling-errors-in-desktop-applications"></a>Tratamento de erros em aplicativos da área de trabalho

Os fluxos diferentes, você aprendeu como tratar os erros para os fluxos silenciosos (conforme mostrado nos trechos de código). Você também viu que há casos em que a interação é necessária (consentimento incremental e acesso condicional).

## <a name="how-to-have--the-user-consent-upfront-for-several-resources"></a>Como fazer com que o consentimento do usuário com antecedência para obter vários recursos

> [!NOTE]
> Obtendo o consentimento para o works vários recursos para a plataforma de identidade da Microsoft, mas não para o Azure Active Directory (Azure AD) B2C. B2C do AD do Azure dá suporte a apenas consentimento do administrador, não consentimento do usuário.

O ponto de extremidade do Microsoft identity platform (v2.0) não permite que você obtenha um token para vários recursos ao mesmo tempo. Portanto, o `scopes` parâmetro só pode conter os escopos para um único recurso. Você pode garantir que o usuário dá consentimento previamente para vários recursos usando o `extraScopesToConsent` parâmetro.

Por exemplo, se você tiver dois recursos, que tem dois escopos de cada um:

- `https://mytenant.onmicrosoft.com/customerapi` -com 2 escopos `customer.read` e `customer.write`
- `https://mytenant.onmicrosoft.com/vendorapi` -com 2 escopos `vendor.read` e `vendor.write`

Você deve usar o `.WithAdditionalPromptToConsent` modificador que tem o `extraScopesToConsent` parâmetro.

Por exemplo:

```CSharp
string[] scopesForCustomerApi = new string[]
{
  "https://mytenant.onmicrosoft.com/customerapi/customer.read",
  "https://mytenant.onmicrosoft.com/customerapi/customer.write"
};
string[] scopesForVendorApi = new string[]
{
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.read",
 "https://mytenant.onmicrosoft.com/vendorapi/vendor.write"
};

var accounts = await app.GetAccountsAsync();
var result = await app.AcquireTokenInteractive(scopesForCustomerApi)
                     .WithAccount(accounts.FirstOrDefault())
                     .WithExtraScopeToConsent(scopesForVendorApi)
                     .ExecuteAsync();
```

Essa chamada obter um token de acesso para a API da web primeiro.

Quando você precisa chamar a API da web segundo, você pode chamar:

```CSharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```

### <a name="microsoft-personal-account-requires-reconsenting-each-time-the-app-is-run"></a>Conta pessoal da Microsoft requer reconsenting cada vez que o aplicativo é executado

Para usuários de contas pessoais da Microsoft, reprompting consentimento em cada chamada de cliente nativo (aplicativo de área de trabalho/móvel) para autorizar é o comportamento desejado. Identidade do cliente nativo é inerentemente insegura (ao contrário do aplicativo cliente confidencial que um segredo com a plataforma Microsoft Identity para provar sua identidade do exchange). A plataforma de identidade da Microsoft decidiu reduzir esse insegurança para serviços do consumidor ao solicitar ao usuário consentimento, cada vez que o aplicativo está autorizado.

## <a name="next-steps"></a>Próximas etapas

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
