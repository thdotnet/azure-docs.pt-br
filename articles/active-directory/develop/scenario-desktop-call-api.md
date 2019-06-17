---
title: Aplicativo de desktop que chamadas de web APIs (chamando uma API da web) - plataforma de identidade da Microsoft
description: Saiba como criar um aplicativo da área de trabalho que chama APIs (chamando uma API da web) da web
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4abaf234d3b216e0f67501e5d2f2f5c3f874c5d7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111239"
---
# <a name="desktop-app-that-calls-web-apis---call-a-web-api"></a>Aplicativo de área de trabalho que chama APIs - web chamar uma API web

Agora que você tem um token, você pode chamar uma API web protegida.

## <a name="calling-a-web-api-from-net"></a>Chamar uma API web do .NET

[!INCLUDE [Call web API in .NET](../../../includes/active-directory-develop-scenarios-call-apis-dotnet.md)]

<!--
More includes will come later for Python and Java
-->

## <a name="calling-several-apis---incremental-consent-and-conditional-access"></a>Chamar várias APIs - consentimento Incremental e o acesso condicional

Se você precisar chamar várias APIs para o mesmo usuário, depois que você obteve um token para a primeira API, você pode simplesmente chamar `AcquireTokenSilent`, e você obterá um token para outras APIs silenciosamente maioria das vezes.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
```

Os casos em que é necessária interação é que quando:

- O usuário com consentimento para a primeira API, mas agora precisa dar consentimento para mais escopos (consentimento incremental)
- A primeira API não exige autenticação multifator, mas o próximo Sim.

```CSharp
var result = await app.AcquireTokenXX("scopeApi1")
                      .ExecuteAsync();

try
{
 result = await app.AcquireTokenSilent("scopeApi2")
                  .ExecuteAsync();
}
catch(MsalUiRequiredException ex)
{
 result = await app.AcquireTokenInteractive("scopeApi2")
                  .WithClaims(ex.Claims)
                  .ExecuteAsync();
}
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Mover para a produção](scenario-desktop-production.md)
