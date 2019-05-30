---
title: Escopos para um aplicativo v1.0 (Biblioteca de Autenticação da Microsoft) | Azure
description: Saiba mais sobre os escopos para um aplicativo v1.0 usando a Biblioteca de Autenticação da Microsoft (MSAL).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/23/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 44aad6b2fab7e0ab2ff11d8469782b001b1f4d18
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545891"
---
# <a name="scopes-for-a-web-api-accepting-v10-tokens"></a>Escopos para uma API Web que aceita os tokens v1.0

As permissões OAuth2 são escopos de permissão que um aplicativo de API Web (recurso) do Azure AD para desenvolvedores (v1.0) expõe aos aplicativos cliente. Os escopos de permissões podem ser concedidos a aplicativos cliente durante o consentimento. Confira a seção sobre `oauth2Permissions` na [referência do manifesto do aplicativo Azure Active Directory](reference-app-manifest.md#manifest-reference).

## <a name="scopes-to-request-access-to-specific-oauth2-permissions-of-a-v10-application"></a>Os escopos solicitam acesso a permissões específicas de OAuth2 de um aplicativo v1.0
Se você deseja adquirir tokens para escopos específicos de um aplicativo de v1.0 (por exemplo, o grafo do Azure AD, que é https://graph.windows.net), você precisaria criar escopos concatenando um identificador de recurso desejado com uma permissão OAuth2 desejado para esse recurso.

Por exemplo, para acessar em nome do usuário uma API Web v 1.0 em que o URI da ID do aplicativo é `ResourceId`:

```csharp
var scopes = new [] {  ResourceId+"/user_impersonation"};
```

```javascript
var scopes = [ ResourceId + "/user_impersonation"];
```

Se você quiser ler e escrever com o Azure Active Directory da MSAL.NET usando a API do grafo do Azure AD (https://graph.windows.net/), você criaria uma lista de escopos, como a seguir:

```csharp
string ResourceId = "https://graph.windows.net/";
var scopes = new [] { ResourceId + "Directory.Read", ResourceID + "Directory.Write"}
```

```javascript
var ResourceId = "https://graph.windows.net/";
var scopes = [ ResourceId + "Directory.Read", ResourceID + "Directory.Write"];
```

Se você quiser escrever o escopo correspondente à API do Azure Resource Manager (https://management.core.windows.net/), você precisará solicitar o escopo a seguir (observe as duas barras):

```csharp
var scopes = new[] {"https://management.core.windows.net//user_impersonation"};
var result = await app.AcquireTokenInteractive(scopes).ExecuteAsync();

// then call the API: https://management.azure.com/subscriptions?api-version=2016-09-01
```

> [!NOTE]
> Você precisa usar duas barras, pois a API do Azure Resource Manager espera uma barra em sua declaração de público-alvo (aud), e, em seguida, há uma barra para separar o nome da API do escopo.

A lógica usada pelo Azure AD é a seguinte:

- Ponto de extremidade da ADAL (v1.0) com um token de acesso v 1.0 (o único possível), aud = recurso
- Para a MSAL (ponto de extremidade [v2.0] da plataforma de identidade da Microsoft) solicitando um token de acesso para um recurso que aceita tokens v2.0, aud=resource.AppId
- Para a MSAL (ponto de extremidade v2.0) solicitando um token de acesso para um recurso que aceita um token de acesso v 1.0 (que é o caso acima), o Azure Active Directory analisa o público-alvo desejado do escopo solicitado considerando tudo antes da última barra e usa como o identificador de recurso. Portanto, se https://database.windows.net espera um público de "https://database.windows.net/", você precisará solicitar um escopo de "https://database.windows.net//.default". Consulte também problema do GitHub [#747: Uma barra à direita do recurso da url foi omitida, o que causou a falha de autenticação do sql](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/747).

## <a name="scopes-to-request-access-to-all-the-permissions-of-a-v10-application"></a>Os escopos solicitam acesso a permissões específicas de v1.0
Se você quiser adquirir um token para todos os escopos estáticos de um aplicativo v1.0, anexe ".default" ao URI da ID do aplicativo da API:

```csharp
ResourceId = "someAppIDURI";
var scopes = new [] {  ResourceId+"/.default"};
```

```javascript
var ResourceId = "someAppIDURI";
var scopes = [ ResourceId + "/.default"];
```

## <a name="scopes-to-request-for-client-credential-flow--daemon-app"></a>Escopos para solicitar no caso de fluxo de credencial de cliente/aplicativo daemon
No caso do fluxo de credenciais do cliente, o escopo para passar também seria `/.default`. Isso informa ao Azure Active Directory: "todas as permissões de nível de aplicativo que o administrador consentiu no registro de aplicativo.