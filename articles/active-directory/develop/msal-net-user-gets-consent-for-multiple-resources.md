---
title: Obter o consentimento de vários recursos (biblioteca de autenticação do Microsoft para .NET) | Azure
description: Saiba como um usuário pode obter o consentimento de pré-lançamento para vários recursos usando a biblioteca de autenticação da Microsoft para .NET (MSAL.NET).
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
ms.date: 04/30/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f7d24a1e14cfbb1163ab78b94dd36ec288dce50
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544041"
---
# <a name="user-gets-consent-for-several-resources-using-msalnet"></a>Usuário recebe consentimento para vários recursos usando MSAL.NET
O ponto de extremidade de plataforma do Microsoft identity não permite que você obtenha um token para vários recursos ao mesmo tempo. Ao usar a biblioteca de autenticação da Microsoft para .NET (MSAL.NET), o parâmetro no método de token de aquisição escopos deve conter apenas escopos para um único recurso. No entanto, você pode previamente autoriza antecipadamente vários recursos especificando escopos adicionais usando o `.WithExtraScopeToConsent` método de construtor.

> [!NOTE]
> Obtendo o consentimento para o works vários recursos para a plataforma de identidade da Microsoft, mas não para o Azure AD B2C. B2C do AD do Azure dá suporte a apenas consentimento do administrador, não consentimento do usuário.

Por exemplo, se você tiver dois recursos que têm 2 escopos cada:

- https://mytenant.onmicrosoft.com/customerapi (com 2 escopos `customer.read` e `customer.write`)
- https://mytenant.onmicrosoft.com/vendorapi (com 2 escopos `vendor.read` e `vendor.write`)

Você deve usar o `.WithExtraScopeToConsent` modificador que tem o *extraScopesToConsent* parâmetro, conforme mostrado no exemplo a seguir:

```csharp
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

Isso irá proporcionar a você um token de acesso para a API da web primeiro. Em seguida, quando você precisa para acessar a API da web segundo silenciosamente pode adquirir o token do cache de token:

```csharp
AcquireTokenSilent(scopesForVendorApi, accounts.FirstOrDefault()).ExecuteAsync();
```
