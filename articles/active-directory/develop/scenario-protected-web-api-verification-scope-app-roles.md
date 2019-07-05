---
title: API da web - configuração de código de aplicativo protegida | O Azure Active Directory
description: Saiba como compilar uma API web protegida e configurar o código do seu aplicativo.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
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
ms.openlocfilehash: 23ff03316a1f9409d4d6e4b7ddf52d0c8cc7a909
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551534"
---
# <a name="protected-web-api-adding-authorization-to-your-api"></a>API web protegida: Adicionar autorização a sua API

Este artigo descreve como você pode adicionar autorização para sua API da web. Essa proteção garante que a API é chamada somente por:

- Aplicativos em nome dos usuários que têm os escopos certos.
- Aplicativos daemon com as funções de aplicativo correto.

Para proteger uma API web ASP.NET/ASP.NET Core, você precisará adicionar o `[Authorize]` atributo em uma destas opções:

- O próprio controlador, se desejar que todas as ações do controlador a serem protegidos
- A ação do controlador individual para sua API

```CSharp
    [Authorize]
    public class TodoListController : Controller
    {
     ...
    }
```

Mas essa proteção não é suficiente. Ele apenas garante que ASP.NET/ASP.NET Core validará o token. Sua API precisa verificar que o token usado para chamar o web de que API foi solicitada com as declarações espera, em particular:

- O *escopos*, se a API é chamada em nome do usuário.
- O *funções de aplicativo*, se a API pode ser chamada de um aplicativo daemon.

## <a name="verifying-scopes-in-apis-called-on-behalf-of-users"></a>Verificando os escopos em APIs chamadas em nome dos usuários

Se sua API for chamada por um aplicativo cliente em nome do usuário, ele precisa solicitar um token de portador com escopos específicos para a API. (Consulte [configuração de código | Token de portador](scenario-protected-web-api-app-configuration.md#bearer-token).)

```CSharp
[Authorize]
public class TodoListController : Controller
{
    /// <summary>
    /// The web API will accept only tokens 1) for users, 2) that have the `access_as_user` scope for
    /// this API.
    /// </summary>
    const string scopeRequiredByAPI = "access_as_user";

    // GET: api/values
    [HttpGet]
    public IEnumerable<TodoItem> Get()
    {
        VerifyUserHasAnyAcceptedScope(scopeRequiredByAPI);
        // Do the work and return the result.
        ...
    }
...
}
```

O `VerifyUserHasAnyAcceptedScope` método faria algo semelhante ao seguinte:

- Verifique se há uma declaração denominada `http://schemas.microsoft.com/identity/claims/scope` ou `scp`.
- Verifique se a declaração tem um valor que contém o escopo esperado pela API.

```CSharp
    /// <summary>
    /// When applied to a <see cref="HttpContext"/>, verifies that the user authenticated in the 
    /// web API has any of the accepted scopes.
    /// If the authenticated user doesn't have any of these <paramref name="acceptedScopes"/>, the
    /// method throws an HTTP Unauthorized error with a message noting which scopes are expected in the token.
    /// </summary>
    /// <param name="acceptedScopes">Scopes accepted by this API</param>
    /// <exception cref="HttpRequestException"/> with a <see cref="HttpResponse.StatusCode"/> set to 
    /// <see cref="HttpStatusCode.Unauthorized"/>
    public static void VerifyUserHasAnyAcceptedScope(this HttpContext context,
                                                     params string[] acceptedScopes)
    {
        if (acceptedScopes == null)
        {
            throw new ArgumentNullException(nameof(acceptedScopes));
        }
        Claim scopeClaim = HttpContext?.User
                                      ?.FindFirst("http://schemas.microsoft.com/identity/claims/scope");
        if (scopeClaim == null || !scopeClaim.Value.Split(' ').Intersect(acceptedScopes).Any())
        {
            context.Response.StatusCode = (int)HttpStatusCode.Unauthorized;
            string message = $"The 'scope' claim does not contain scopes '{string.Join(",", acceptedScopes)}' or was not found";
            throw new HttpRequestException(message);
        }
    }
```

Esse código de exemplo é para o ASP.NET Core. Para o ASP.NET, basta substituir `HttpContext.User` com `ClaimsPrincipal.Current`e substitua o tipo de declaração `"http://schemas.microsoft.com/identity/claims/scope"` com `"scp"`. (Consulte também o trecho de código neste artigo.)

## <a name="verifying-app-roles-in-apis-called-by-daemon-apps"></a>Verificando a funções de aplicativo no APIs chamadas por aplicativos daemon

Se sua API da web é chamado por um [aplicativo daemon](scenario-daemon-overview.md), que o aplicativo deve exigir uma permissão de aplicativo para sua API da web. Já vimos no [expondo permissões de aplicativo (funções de aplicativo)](https://docs.microsoft.com/azure/active-directory/develop/scenario-protected-web-api-app-registration#exposing-application-permissions-app-roles) que sua API expõe tais permissões (por exemplo, o `access_as_application` função de aplicativo).
Agora, você precisa ter suas APIs Verifique se o token recebido contém o `roles` declaração e que essa declaração tem o valor que se espera. O código de fazer essa verificação é semelhante ao código que verifica permissões delegadas, exceto que, em vez de testar para `scopes`, a ação de controlador será testado para `roles`:

```CSharp
[Authorize]
public class TodoListController : ApiController
{
    public IEnumerable<TodoItem> Get()
    {
        ValidateAppRole("access_as_application");
        ...
    }
```

O `ValidateAppRole()` método pode ser algo parecido com isto:

```CSharp
private void ValidateAppRole(string appRole)
{
    //
    // The `role` claim tells you what permissions the client application has in the service.
    // In this case, we look for a `role` value of `access_as_application`.
    //
    Claim roleClaim = ClaimsPrincipal.Current.FindFirst("roles");
    if (roleClaim == null || !roleClaim.Value.Split(' ').Contains(appRole))
    {
        throw new HttpResponseException(new HttpResponseMessage 
        { StatusCode = HttpStatusCode.Unauthorized, 
            ReasonPhrase = $"The 'roles' claim does not contain '{appRole}' or was not found" 
        });
    }
}
}
```

Esse código de exemplo é para o ASP.NET. Para o ASP.NET Core, basta substituir `ClaimsPrincipal.Current` com `HttpContext.User`e substitua o `"roles"` de declaração de nome com `"http://schemas.microsoft.com/identity/claims/roles"`. (Consulte também o trecho de código neste artigo.)

### <a name="accepting-app-only-tokens-if-the-web-api-should-be-called-only-by-daemon-apps"></a>Aceitação de tokens de aplicativo somente se a API da web deve ser chamado apenas por aplicativos daemon

O `roles` declaração também é usada para usuários em padrões de atribuição do usuário. (Consulte [como: Adicione funções de aplicativo em seu aplicativo e recebê-las no token](howto-add-app-roles-in-azure-ad-apps.md).) Portanto, apenas verificando as funções permitirá aplicativos entrar como usuários e o caminho inverso, se as funções podem ser atribuídas a ambos. É recomendável que você declare funções diferentes para usuários e aplicativos evitar essa confusão.

Se você quiser permitir que apenas os aplicativos daemon chamar sua API da web, adicione uma condição, quando você validar a função de aplicativo, que o token é um token somente de aplicativo:

```CSharp
string oid = ClaimsPrincipal.Current.FindFirst("oid");
string sub = ClaimsPrincipal.Current.FindFirst("sub");
bool isAppOnlyToken = oid == sub;
```

Verificação da condição inversa permitirá que apenas os aplicativos que conectar um usuário para chamar a API.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Mover para a produção](scenario-protected-web-api-production.md)
