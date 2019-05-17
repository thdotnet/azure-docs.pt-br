---
title: Aplicativo móvel que chama o web APIs – visão geral | Plataforma de identidade da Microsoft
description: Saiba como criar um aplicativo móvel que chamadas de web APIs (visão geral)
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
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d9978905f8ae0719f6a77ca2351f2c43bbf94ee0
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550357"
---
# <a name="scenario-mobile-application-that-calls-web-apis"></a>Cenário: Aplicativo móvel que chamadas às APIs da web

Aprenda tudo o que você precisa saber para compilar um aplicativo móvel que chama as APIs da web.

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [Prerequisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Introdução

Crie seu primeiro aplicativo móvel e experimente um início rápido!

> [!div class="nextstepaction"]
> [Início Rápido: Adquirir um token e chamar a API do Microsoft Graph de um aplicativo Android](./quickstart-v2-android.md)
>
> [Início Rápido: Adquirir um token e chamar a API do Microsoft Graph de um aplicativo iOS](./quickstart-v2-ios.md)
>
> [Início Rápido: Adquirir um token e chamar a API do Microsoft Graph de um aplicativo Android e o Xamarin iOS](https://github.com/Azure-Samples/active-directory-xamarin-native-v2)

## <a name="overview"></a>Visão geral

Uma experiência de usuário personalizado, contínuo, é essencial para aplicativos móveis.  Plataforma de identidade da Microsoft permite que os desenvolvedores de aplicativos móveis criar essa experiência para os usuários do Android e iOS. Seu aplicativo pode entrar em usuários do Active Directory do Azure (AD Azure), usuários de conta pessoas da Microsoft e os usuários do Azure AD B2C e adquirir tokens para chamar uma API da web em seu nome. Para implementar esses fluxos, usaremos a biblioteca de autenticação de Microsoft (MSAL), que implementa o padrão do setor [fluxo de código de autorização do OAuth 2.0](v2-oauth2-auth-code-flow.md).

![Aplicativos Daemon](./media/scenarios/mobile-app.svg)

Considerações para aplicativos móveis:

- **Experiência do usuário é a chave**: Permitir que os usuários ver o valor do seu aplicativo antes de solicitar para entrar e solicitar apenas as permissões necessárias.
- **Suporte a todas as configurações de usuário**: Muitos usuários de negócios móveis estão sob o acesso condicional e políticas de conformidade do dispositivo. Certifique-se de dar suporte a esses cenários-chave.
- **Implementar o logon único (SSO)**: MSAL e plataforma de identidade Microsoft simplificam habilitar logon único por meio do navegador do dispositivo ou o Microsoft Authenticator (e Portal da empresa do Intune no Android).

## <a name="specifics"></a>Informações específicas

Tenha em mente essas considerações ao compilar um aplicativo móvel na plataforma de identidade da Microsoft:

- Dependendo da plataforma, alguma interação do usuário pode ser necessária na primeira vez em que os usuários entram. Por exemplo, iOS requer que os aplicativos para mostrar a interação do usuário ao usar SSO na primeira vez por meio do Microsoft Authenticator (e o Portal da empresa do Intune no Android).
- No iOS e Android, a MSAL pode usar um navegador externo (que pode aparecer na parte superior de seu aplicativo) para conectar os usuários. Você pode personalizar a configuração para usar no aplicativo WebViews em vez disso.
- Nunca use um segredo em um aplicativo móvel. Ele será acessível a todos os usuários.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Registro do Aplicativo](scenario-mobile-app-registration.md)
