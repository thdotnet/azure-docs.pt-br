---
title: Erros e exceções (MSAL) | Azure
description: Saiba como tratar erros e exceções, acesso condicional e desafio de declarações em aplicativos MSAL.
services: active-directory
documentationcenter: dev-center-name
author: negoe
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/19/2019
ms.author: negoe
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: fe3ad29cfd113deba5824ce25721dc543c6267c0
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70305060"
---
# <a name="handling-exceptions-and-errors-using-msal"></a>Tratar exceções e erros usando MSAL
As exceções na MSAL (Biblioteca de Autenticação da Microsoft) são destinadas aos desenvolvedores de aplicativos para solucionar problemas e não para exibição aos usuários finais. Mensagens de exceção não são localizadas.

Ao processar exceções e erros, é possível usar o próprio tipo de exceção e o código de erro para distinguir entre exceções.  Para obter uma lista de códigos de erro, consulte [Códigos de erro de autenticação e autorização](reference-aadsts-error-codes.md).

## <a name="net-exceptions"></a>Exceções .NET
Ao processar exceções, é possível usar o próprio tipo de exceção e o membro `ErrorCode` para distinguir entre exceções. Os valores de `ErrorCode` são constantes do tipo [MsalError](/dotnet/api/microsoft.identity.client.msalerror?view=azure-dotnet).

Você também pode dar uma olhada nos campos [MsalClientException](/dotnet/api/microsoft.identity.client.msalexception?view=azure-dotnet), [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet), [MsalUIRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet).

Se [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) for gerado, o código de erro poderá conter um código que pode ser localizado em [Códigos de erro de autenticação e autorização](reference-aadsts-error-codes.md).

### <a name="common-exceptions"></a>Exceções comuns
Aqui são apresentadas as exceções comuns que podem ser lançadas e algumas possíveis mitigações.

| Exceção | Código de erro | Atenuação|
| --- | --- | --- |
| [MsalUiRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS65001: O usuário ou administrador não consentiu em usar o aplicativo com a ID '{appId}' nomeada '{appName}'. Envie uma solicitação de autorização interativa para esse usuário e recurso.| É necessário primeiro obter o consentimento do usuário. Se você não estiver usando o .NET Core (que não tem nenhuma IU da Web), chame (apenas uma vez) `AcquireTokeninteractive`. Se você estiver usando o .NET Core ou não quiser fazer um `AcquireTokenInteractive`, o usuário poderá navegar para uma URL para dar consentimento: https://login.microsoftonline.com/common/oauth2/v2.0/authorize?client_id={clientId}&response_type=code&scope=user.read. Para chamar `AcquireTokenInteractive`: `app.AcquireTokenInteractive(scopes).WithAccount(account).WithClaims(ex.Claims).ExecuteAsync();`|
| [MsalUiRequiredException](/dotnet/api/microsoft.identity.client.msaluirequiredexception?view=azure-dotnet) | AADSTS50079: O usuário deverá usar autenticação multifator.| Não há mitigação - se a MFA estiver configurada para o locatário e o AAD decidir impô-lo, será necessário fazer fallback de um fluxo interativo, como `AcquireTokenInteractive` ou `AcquireTokenByDeviceCode`.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) |AADSTS90010: Não há suporte para o tipo de concessão nos pontos de extremidade */common* ou */consumers*. Use o ponto de extremidade */organizations* ou específico do locatário. Você usou */common*.| Conforme explicado na mensagem do Azure AD, a autoridade deve ter um locatário ou, caso contrário, */organizations*.|
| [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) | AADSTS70002: O corpo da solicitação deve conter o seguinte parâmetro: 'client_secret or client_assertion'.| Essa exceção poderá acontecer se o aplicativo não tiver sido registrado como um aplicativo cliente público no Azure AD. No portal do Azure, edite o manifesto do aplicativo e defina `allowPublicClient` para `true`. |
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)| Mensagem unknown_user: Não foi possível identificar o usuário conectado| A biblioteca não pôde consultar o usuário atual conectado ao Windows ou esse usuário não está ingressado no AD ou AAD (não há suporte para usuários ingressados no local de trabalho). Mitigação 1: na Plataforma Universal do Windows, verifique se o aplicativo tem os seguintes recursos: Autenticação Empresarial, Redes Privadas (Cliente e Servidor), Informações sobre a Conta de Usuário. Mitigação 2: Implemente sua própria lógica para buscar o nome de usuário (por exemplo, john@contoso.com) e use o formulário `AcquireTokenByIntegratedWindowsAuth` que recebe o nome de usuário.|
| [MsalClientException](/dotnet/api/microsoft.identity.client.msalclientexception?view=azure-dotnet)|integrated_windows_auth_not_supported_managed_user| Esse método depende de um protocolo exposto pelo AD (Active Directory). Se um usuário foi criado no Azure Active Directory sem suporte do AD (usuário "gerenciado"), esse método falhará. Usuários criados no AD e com suporte do AAD (usuários "federados") poderão se beneficiar desse método de autenticação não interativo. Mitigação: Use autenticação interativa.|

## <a name="javascript-errors"></a>Erros do JavaScript

O MSAL. js fornece objetos de erro que abstraim e classificam os diferentes tipos de erros comuns. Ele também fornece a interface para acessar detalhes específicos dos erros, como mensagens de erro para tratá-los adequadamente.

**Objeto Error**

```javascript                                
export class AuthError extends Error {
    // This is a short code describing the error
    errorCode: string;
    // This is a descriptive string of the error,
    // and may also contain the mitigation strategy
    errorMessage: string;
    // Name of the error class
    this.name = "AuthError";
}
```                
Ao estender a classe do erro, você terá acesso às seguintes propriedades:
* **AuthError.message:**  O mesmo que errorMessage.
* **AuthError.stack:** Rastreamento de pilha para erros gerados. Permite rastrear o ponto de origem do erro.

**Tipos de erro**

Os tipos de erro a seguir estão disponíveis:

* *AuthError:* Classe de erro base para a biblioteca MSAL.js, também usada para erros inesperados.

* *ClientAuthError:* Classe Error, que denota um problema com a autenticação do cliente. A maioria dos erros originados da biblioteca serão ClientAuthErrors. Esses erros são como chamar um método de logon quando o logon está em andamento, os usuários cancelam o logon e assim por diante. 

* *ClientConfigurationError:* Classe de erro que estende o ClientAuthError gerado antes das solicitações serem feitas quando os parâmetros de configuração do usuário especificados estão malformados ou ausentes.

* *ServerError:* Classe de erro para representar as cadeias de caracteres de erro enviadas pelo servidor de autenticação. Esses erros poderão ser: parâmetros ou formatos da solicitação inválidos ou qualquer outro erro que impeça o servidor de autenticar ou autorizar o usuário.

* *InteractionRequiredAuthError:* Classe de erro que estende ServerError para representar erros de servidor, que exigem uma chamada interativa. Esse erro é gerado pelo `acquireTokenSilent` se for necessário que o usuário interaja com o servidor para fornecer credenciais ou consentimento para autenticação/autorização. Os códigos de erro incluem "interaction_required", "login_required", "consent_required".

Para o tratamento de erros nos fluxos de autenticação com`loginRedirect`métodos `acquireTokenRedirect`de redirecionamento (,), você precisará registrar o retorno de chamada, que é chamado com `handleRedirectCallback()` êxito ou falha após o método redirecionar usando o seguinte:

```javascript
function authCallback(error, response) {
    //handle redirect response
}


var myMSALObj = new Msal.UserAgentApplication(msalConfig);

// Register Callbacks for redirect flow
myMSALObj.handleRedirectCallback(authCallback);

myMSALObj.acquireTokenRedirect(request);
```

Os métodos para a experiência de pop-`loginPopup`up `acquireTokenPopup`(,) retornam promessas, para que você possa usar o padrão Promise (. em seguida e. catch) para tratá-los, conforme mostrado:

```javascript
myMSALObj.acquireTokenPopup(request).then(
    function (response) {
        // success response
    }).catch(function (error) {
        console.log(error);
    });
```

### <a name="interaction-required-errors"></a>Erros de interação necessária

Um erro é retornado quando você tenta usar um método não interativo de adquirir um token (por exemplo, `acquireTokenSilent`) e MSAL não pôde fazer isso silenciosamente. 

As possíveis razões são:

* é necessário entrar
* é necessário consentir
* é necessário passar por uma experiência de autenticação multifator.

A correção é chamar um método interativo como `acquireTokenPopup` ou `acquireTokenRedirect`:

```javascript
// Request for Access Token
myMSALObj.acquireTokenSilent(request).then(function (response) {
    // call API
}).catch( function (error) {
    // call acquireTokenPopup in case of acquireTokenSilent failure
    // due to consent or interaction required
    if (error.errorCode === "consent_required"
    || error.errorCode === "interaction_required"
    || error.errorCode === "login_required") {
        myMSALObj.acquireTokenPopup(request).then(
            function (response) {
                // call API
            }).catch(function (error) {
                console.log(error);
            });
    }
});
```

## <a name="conditional-access-and-claims-challenges"></a>Acesso condicional e desafio de declarações
Ao obter tokens silenciosamente, o aplicativo poderá receber erros quando um [desafio de declarações de acesso condicional](conditional-access-dev-guide.md), como a Política de MFA, for exigido por uma API que você está tentando acessar.

O padrão para tratar com esse erro é obter um token de forma interativa usando a MSAL. A obtenção interativa de um token solicita a participação do usuário e oferece a oportunidade para atender à política de acesso condicional exigida.

Em determinados casos, ao chamar uma API que exige acesso condicional, você poderá receber um desafio de declarações no erro da API. Por exemplo, se a política de acesso condicional tiver um dispositivo gerenciado (Intune), o erro será semelhante a [AADSTS53000: O dispositivo deverá ser gerenciado para acessar este recurso](reference-aadsts-error-codes.md) ou algo similar. Nesse caso, é possível passar as declarações na chamada do token de aquisição, de modo que o usuário seja solicitado a atender à política apropriada.

### <a name="net"></a>.NET
Ao chamar uma API que exige acesso condicional do MSAL.NET, o aplicativo deverá tratar as exceções de desafio de declaração. Isso aparecerá como [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) em que a propriedade [Declarações](/dotnet/api/microsoft.identity.client.msalserviceexception.claims?view=azure-dotnet) não estará vazia.

Para tratar o desafio de declaração, será necessário usar o método `.WithClaim()` da classe `PublicClientApplicationBuilder`.

### <a name="javascript"></a>JavaScript
Ao obter tokens silenciosamente (com `acquireTokenSilent`) usando MSAL.js, o aplicativo poderá receber erros quando um [desafio de declarações de acesso condicional](conditional-access-dev-guide.md), como a Política de MFA, for exigido por uma API que você está tentando acessar.

O padrão para tratar esse erro é fazer uma chamada interativa para obter o token em MSAL.js, como `acquireTokenPopup` ou `acquireTokenRedirect`, conforme no exemplo a seguir:

```javascript
myMSALObj.acquireTokenSilent(accessTokenRequest).then(function (accessTokenResponse) {
    // call API
}).catch( function (error) {
    if (error instanceof InteractionRequiredAuthError) {
        // Extract claims from error message
        accessTokenRequest.claimsRequest = extractClaims(error.errorMessage);
        // call acquireTokenPopup in case of InteractionRequiredAuthError failure
        myMSALObj.acquireTokenPopup(accessTokenRequest).then(function (accessTokenResponse) {
            // call API
        }).catch(function (error) {
            console.log(error);
        });
    }
});
```

A obtenção interativa do token solicita a participação do usuário e oferece a oportunidade para atender à política de acesso condicional exigida.

Ao chamar uma API que exige acesso condicional, você poderá receber um desafio de declarações no erro da API. Nesse caso, você pode passar as declarações retornadas no erro para o `claimsRequest` campo `AuthenticationParameters.ts` da classe para atender à política apropriada. 

Consulte [solicitando declarações adicionais](active-directory-optional-claims.md) para obter mais detalhes.

## <a name="retrying-after-errors-and-exceptions"></a>Tentar novamente após erros e exceções

### <a name="http-error-codes-500-600"></a>Códigos de erro HTTP 500-600
O MSAL.NET implementa um mecanismo de nova tentativa única simples para erros com códigos de erro HTTP 500-600.

### <a name="http-429"></a>HTTP 429
Quando o servidor de token de serviço (STS) está sobrecarregado com muitas solicitações, ele retorna um erro HTTP 429 com uma dica sobre quando você pode tentar novamente em termos de tempo. O erro pode ser lido no `Retry-After` campo de resposta.

#### <a name="net"></a>.NET
A exceção [MsalServiceException](/dotnet/api/microsoft.identity.client.msalserviceexception?view=azure-dotnet) mostra `System.Net.Http.Headers.HttpResponseHeaders` como uma propriedade `namedHeaders`. Portanto, é possível aproveitar informações adicionais do código de erro para melhorar a confiabilidade dos aplicativos. No caso que acabamos de descrever, é possível usar o `RetryAfterproperty` (do tipo `RetryConditionHeaderValue`) e calcular quando tentar novamente.

Aqui é apresentado um exemplo de um aplicativo daemon (usando o fluxo de credenciais do cliente), mas é possível adaptá-lo a qualquer um dos métodos para obter um token.

```csharp
do
{
 retry = false;
 TimeSpan? delay;
 try
 {
  result = await publicClientApplication.AcquireTokenForClient(scopes, account)
                                        .ExecuteAsync();
 }
 catch (MsalServiceException serviceException)
 {
  if (ex.ErrorCode == "temporarily_unavailable")
  {
   RetryConditionHeaderValue retryAfter = serviceException.Headers.RetryAfter;
   if (retryAfter.Delta.HasValue)
   {
    delay = retryAfter.Delta;
   }
   else if (retryAfter.Date.HasValue)
   {
    delay = retryAfter.Date.Value.Offset;
   }
  }
 }

    …
 if (delay.HasValue)
 {
  Thread.Sleep((int)delay.Value.TotalMilliseconds); // sleep or other
  retry = true;
 }
} while (retry);
```
