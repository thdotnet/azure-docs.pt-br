---
title: Aplicativo de página única (mover para a produção) – plataforma de identidade da Microsoft
description: Saiba como criar um aplicativo de página única (mover para a produção)
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6a04f5d62ec750cfbe44765e833552bd694654d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65075004"
---
# <a name="single-page-application---move-to-production"></a>Aplicativo de página única - mover para a produção

Agora que você sabe como adquirir um token para chamar APIs da Web, saiba como mover para a produção.

## <a name="improve-your-app"></a>Melhorar seu aplicativo

Siga as etapas necessárias para preparar sua produção de aplicativo.

- [Habilitar registro em log](msal-logging.md) em seu aplicativo.

## <a name="test-your-integration"></a>Testar sua integração

- Teste sua integração seguindo a [lista de verificação de integração da plataforma de identidade da Microsoft](identity-platform-integration-checklist.md).

## <a name="next-steps"></a>Próximas etapas

Aqui estão alguns outros exemplos/tutoriais:

- Para um mergulho profundo do início rápido de amostra que explica o código para saber como conectar usuários e obter um token de acesso para chamar a API do MS Graph usando msal

    > [!div class="nextstepaction"]
    > [Tutorial de JavaScript SPA](./tutorial-v2-javascript-spa.md)

- Exemplo que demonstra como obter tokens para sua própria API da web de back-end usando msal

     > [!div class="nextstepaction"]
     > [SPA com um back-end do ASP.NET](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2)

- Exemplo para mostrar como usar msal para conectar usuários em um aplicativo registrado com o Azure AD B2C

    > [!div class="nextstepaction"]
    > [SPA com B2C do AD do Azure](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)
