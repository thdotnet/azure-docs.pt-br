---
title: Aplicativo móvel que chamadas de web APIs - chamar uma API web | Plataforma de identidade da Microsoft
description: Saiba como criar um aplicativo móvel que chama APIs (chamando uma API da web) da web
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7fc8c21db0f42bbb6804c00e27e82f840d7038c2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111181"
---
# <a name="mobile-app-that-calls-web-apis---call-a-web-api"></a>Aplicativo móvel que chama APIs - web chamar uma API web

Depois que seu aplicativo é um usuário conectado e recebido tokens, MSAL expõe várias partes de informações sobre o usuário, o ambiente do usuário e os tokens emitidos. Seu aplicativo pode usar esses valores para chamar uma API da web ou exibir uma mensagem de boas-vinda ao usuário.

Primeiro, vamos examinar o resultado da MSAL. Em seguida, vamos examinar como usar um token de acesso do `AuthenticationResult` ou `result` para chamar uma API web protegida.

## <a name="msal-result"></a>Resultado da MSAL
A MSAL oferece os seguintes valores: 

- `AccessToken`: Usado para chamar APIs web protegida em uma solicitação de portador de HTTP.
- `IdToken`: Contém informações úteis sobre o usuário conectado, como o nome do usuário, o locatário inicial e um identificador exclusivo para o armazenamento.
- `ExpiresOn`: A hora de expiração do token. A MSAL manipula a atualização automática para aplicativos.
- `TenantId`: O identificador do locatário que o usuário conectado. Para usuários convidados (Azure Active Directory B2B), esse valor identificará o locatário que o usuário conectado com não locatário inicial do usuário.  
- `Scopes`: Os escopos que foram concedidos com seu token. Os escopos concedidos podem ser um subconjunto dos escopos que você solicitou.

A MSAL também fornece uma abstração para um `Account`. Um `Account` representa a conta de logon do usuário atual.

- `HomeAccountIdentifier`: O identificador do locatário inicial do usuário.
- `UserName`: O nome do usuário preferencial do usuário. Isso pode ser vazio para os usuários do Azure Active Directory B2C.
- `AccountIdentifier`: O identificador do usuário conectado. Esse valor será o mesmo que o `HomeAccountIdentifier` valor na maioria dos casos, a menos que o usuário é um convidado em outro locatário.

## <a name="call-an-api"></a>Chamar uma API

Depois de ter o token de acesso, é fácil chamar uma API da web. Seu aplicativo usará o token para construir uma solicitação HTTP e, em seguida, execute a solicitação.

### <a name="android"></a>Android

```Java
        RequestQueue queue = Volley.newRequestQueue(this);
        JSONObject parameters = new JSONObject();

        try {
            parameters.put("key", "value");
        } catch (Exception e) {
            // Error when constructing.
        }
        JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
                parameters,new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                // Successfully called Graph. Process data and send to UI.
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                // Error.
            }
        }) {
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> headers = new HashMap<>();
                
                // Put access token in HTTP request.
                headers.put("Authorization", "Bearer " + authResult.getAccessToken());
                return headers;
            }
        };

        request.setRetryPolicy(new DefaultRetryPolicy(
                3000,
                DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
                DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
        queue.add(request);
```

### <a name="ios"></a>iOS

```swift
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)

        // Put access token in HTTP request.
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")

        URLSession.shared.dataTask(with: request) { data, response, error in
            if let error = error {
                self.updateLogging(text: "Couldn't get graph result: \(error)")
                return
            }
            guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {
                self.updateLogging(text: "Couldn't deserialize result JSON")
                return
            }

            // Successfully got data from Graph.
            self.updateLogging(text: "Result from Graph: \(result))")
        }.resume()
```

### <a name="xamarin"></a>Xamarin

```CSharp
httpClient = new HttpClient();

// Put access token in HTTP request.
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call Graph.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```

## <a name="making-several-api-requests"></a>Fazer várias solicitações de API

Se você precisar chamar a mesma API várias vezes, ou se você precisar chamar várias APIs, leve em consideração o seguinte quando você compila seu aplicativo:

- **Consentimento incremental**: Plataforma de identidade da Microsoft permite aos aplicativos obter consentimento do usuário, como as permissões são necessárias, em vez de tudo no início. Cada vez que seu aplicativo está pronto para chamar uma API, ele deve solicitar apenas os escopos que ele precisa usar.
- **Acesso condicional**: Em determinados cenários, você poderá obter os requisitos de acesso condicional adicionais ao fazer várias solicitações de API. Isso pode acontecer se a primeira solicitação não tem nenhuma política de acesso condicional aplicada e o aplicativo tenta acessar silenciosamente uma nova API que exige o acesso condicional. Para manipular esse cenário, certifique-se de capturar erros de solicitações silenciosas e estar preparado para fazer uma solicitação interativa.  Para obter mais informações, consulte [diretrizes para acesso condicional](conditional-access-dev-guide.md).

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Mover para a produção](scenario-mobile-production.md)
