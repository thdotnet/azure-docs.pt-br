---
title: API da web - configuração de código de aplicativo protegida | O Azure Active Directory
description: Saiba como compilar uma API Web protegida e configurar o código do seu aplicativo.
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
ms.openlocfilehash: b700825be9a7fe23fe4b50a2d69d4de71f7dc038
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67116448"
---
# <a name="protected-web-api---adding-authorization-to-your-api"></a>API - adicionar autorização a sua API web protegida

Este artigo descreve como você pode adicionar autorização para sua API Web. Essa proteção garante que ele é chamado apenas:

- aplicativos em nome dos usuários com os escopos certos 
- ou por aplicativos daemon com as funções de aplicativo correto.

Para um ASP.NET / ASP.NET Core Web API a ser protegido, você precisará adicionar o `[Authorize]` atributo:

- o controlador em si se você deseja que todas as ações do controlador a serem protegidos
- ou a ação do controlador individual para sua API.

```CSharp
    [Authorize]
    public class TodoListController : Controller
    {
     ...
    }
```

Mas essa proteção não é suficiente. Ele apenas garante que o ASP.NET / ASP.NET Core validará o token. Sua API precisa verificar se o token usado para chamar a API da Web foi solicitado com as declarações que ele espera, em particular:

- o **escopos** se a API é chamada em nome do usuário
- o **funções de aplicativo** se a API pode ser chamada de um aplicativo daemon.

## <a name="verifying-scopes-in-apis-called-on-behalf-of-users"></a>Verificando os escopos em APIs chamadas em nome dos usuários

Se sua API é chamada por um aplicativo cliente em nome do usuário, em seguida, ele precisa solicitar um token de portador com escopos específicos para a API (consulte [configuração de código | Token de portador](scenario-protected-web-api-app-configuration.md#bearer-token))

```CSharp
[Authorize]
public class TodoListController : Controller
{
    /// <summary>
    /// The Web API will only accept tokens 1) for users, 2) having the `access_as_user` scope for
    /// this API
    /// </summary>
    const string scopeRequiredByAPI = "access_as_user";

    // GET: api/values
    [HttpGet]
    public IEnumerable<TodoItem> Get()
    {
        VerifyUserHasAnyAcceptedScope(scopeRequiredByAPI);
        // Do the work and return the result
        ...
    }
...
}
```

O `VerifyUserHasAnyAcceptedScope` método faria algo semelhante ao seguinte:

- Verifique se há uma chamada de declarações `http://schemas.microsoft.com/identity/claims/scope` ou `scp`
- Verifique se a declaração tem um valor que contém o escopo esperado pela API.

```CSharp
    /// <summary>
    /// When applied to an <see cref="HttpContext"/>, verifies that the user authenticated in the 
    /// Web API has any of the accepted scopes.
    /// If the authenticated user does not have any of these <paramref name="acceptedScopes"/>, the
    /// method throws an HTTP Unauthorized with the message telling which scopes are expected in the token
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

Esse código de exemplo é para o ASP.NET Core. Para substituir apenas do ASP.NET `HttpContext.User` pela `ClaimsPrincipal.Current`e o tipo de declaração `"http://schemas.microsoft.com/identity/claims/scope"` por `"scp"` (Consulte também o trecho de código abaixo)

## <a name="verifying-app-roles-in-apis-called-by-daemon-apps"></a>Verificando a funções de aplicativo no APIs chamadas por aplicativos daemon

Se sua API Web é chamada por um [aplicativo Daemon](scenario-daemon-overview.md), em seguida, o aplicativo deve exigir uma permissão de aplicativo para sua API Web. Vimos em [scenario-protected-web-api-app-registration.md#how-to-expose-application-permissions--app-roles-] que sua API expõe tais permissões (por exemplo, como o `access_as_application` função de aplicativo).
Agora, você precisa ter suas APIs Verifique se o token recebido contém o `roles` declarações e que essa declaração tem o valor que se espera. O código de fazer essa verificação é semelhante ao código que verifica permissões delegadas, exceto que, em vez de testar para `scopes`, a ação de controlador será testado para `roles`:

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
    // In this case we look for a `role` value of `access_as_application`
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

Esse código de exemplo é para o ASP.NET. Para o ASP.NET Core, basta substituir `ClaimsPrincipal.Current` pela `HttpContext.User` e o `"roles"` de declaração de nome por `"http://schemas.microsoft.com/identity/claims/roles"` (Consulte também o trecho de código acima)

### <a name="accepting-app-only-tokens-if-the-web-api-should-only-be-called-by-daemon-apps"></a>Aceitando tokens de aplicativo somente se a API da Web só deve ser chamada por aplicativos daemon

O `roles` declaração também é usada para usuários em padrões de atribuição do usuário (consulte [como: Adicione funções de aplicativo em seu aplicativo e recebê-las no token](howto-add-app-roles-in-azure-ad-apps.md)). Portanto, apenas verificando as funções permitirá aplicativos entrar como usuários e o caminho inverso, se as funções podem ser atribuídas a ambos. É recomendável ter declaradas para usuários e aplicativos evitar essa confusão de diferentes funções.

Se você quiser permitir que apenas aplicativos daemon chamar a API da Web, você desejará adicionar uma condição, quando você validar a função de aplicativo, que o token é um token somente de aplicativo:

```CSharp
string oid = ClaimsPrincipal.Current.FindFirst("oid");
string sub = ClaimsPrincipal.Current.FindFirst("sub");
bool isAppOnlyToken = oid == sub;
```

Verificação da condição inversa permitirá que apenas os aplicativos que a entrada do usuário, para chamar a API.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Mover para a produção](scenario-protected-web-api-production.md)
