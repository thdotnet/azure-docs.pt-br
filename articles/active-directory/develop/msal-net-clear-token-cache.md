---
title: Limpar o cache de token usando a biblioteca de autenticação da Microsoft para .NET - Azure
description: Saiba como limpar o cache de token usando a biblioteca de autenticação da Microsoft para .NET (MSAL.NET).
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
ms.date: 05/07/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6763c6b2b1f9b4de7d8669a50a4979a7aac00c7
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65544112"
---
# <a name="clear-the-token-cache-using-msalnet"></a>Limpar o cache de token usando MSAL.NET

Quando você [adquirir um token de acesso](msal-acquire-cache-tokens.md) usando a biblioteca de autenticação da Microsoft para .NET (MSAL.NET), o token é armazenado em cache. Quando o aplicativo precisa de um token, ele deve primeiro chamar o `AcquireTokenSilent` método para verificar se um token aceitável está no cache. 

Limpar o cache é obtida, removendo as contas do cache. No entanto, isso não remove o cookie de sessão que está no navegador.  O exemplo a seguir cria uma instância de um aplicativo cliente público, obtém as contas para o aplicativo e remove as contas.

```csharp
private readonly IPublicClientApplication _app;
private static readonly string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
private static readonly string Authority = string.Format(CultureInfo.InvariantCulture, AadInstance, Tenant);

_app = PublicClientApplicationBuilder.Create(ClientId)
                .WithAuthority(Authority)
                .Build();

var accounts = (await _app.GetAccountsAsync()).ToList();

// clear the cache
while (accounts.Any())
{
   await _app.RemoveAsync(accounts.First());
   accounts = (await _app.GetAccountsAsync()).ToList();
}

```

Para obter mais informações sobre como adquirir e cache de tokens, leia [adquirir um token de acesso](msal-acquire-cache-tokens.md).
