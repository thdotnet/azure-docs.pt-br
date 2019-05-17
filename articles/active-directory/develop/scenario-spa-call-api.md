---
title: Aplicativo de página única (chamada de uma API da web) - plataforma de identidade da Microsoft
description: Saiba como criar um aplicativo de página única (chamar uma API da web)
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/06/2019
ms.author: ryanwi
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 77a4ed01ac55a1153a62c672b33056a543b912ed
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545604"
---
# <a name="single-page-application---call-a-web-api"></a>Aplicativo de página única - chamada de uma API da web

É recomendável que você chame o `acquireTokenSilent` método para adquirir ou renovar um token de acesso antes de chamar uma API da web. Quando você tiver um token, você pode chamar uma API web protegida.

## <a name="call-a-web-api"></a>Chamar uma API da Web

### <a name="javascript"></a>JavaScript

Use o token de acesso adquirido como um portador em uma solicitação HTTP para chamar qualquer API da web, como a API do Microsoft Graph. Por exemplo:

```javascript
    var headers = new Headers();
    var bearer = "Bearer " + access_token;
    headers.append("Authorization", bearer);
    var options = {
         method: "GET",
         headers: headers
    };
    var graphEndpoint = "https://graph.microsoft.com/v1.0/me";

    fetch(graphEndpoint, options)
        .then(function (response) {
             //do something with response
        }
```

### <a name="angular"></a>Angular

Conforme mencionado na [ao adquirir a seção tokens](scenario-spa-acquire-token.md), o wrapper MSAL Angular aproveita o interceptador HTTP para adquirir tokens de acesso silenciosamente e anexá-los às solicitações HTTP para APIs automaticamente.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Mover para a produção](scenario-spa-production.md)
