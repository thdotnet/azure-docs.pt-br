---
title: Exemplos de código de plataforma do Microsoft identity | Microsoft Docs
description: Fornece exemplos de código de plataforma (ponto de extremidade v 2.0), organizados por cenário de um índice de identidade da Microsoft disponível.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: a242a5ff-7300-40c2-ba83-fb6035707433
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/26/2018
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: ceae71766c3e51a84cd09e8ac88740353e783bea
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67482527"
---
# <a name="microsoft-identity-platform-code-samples-v20-endpoint"></a>Exemplos de código de plataforma do Microsoft identity (ponto de extremidade v 2.0)

[!INCLUDE [active-directory-develop-applies-v2-msal](../../../includes/active-directory-develop-applies-v2-msal.md)]

Você pode usar a plataforma de identidade da Microsoft para:

- Adicionar autenticação e autorização para aplicativos Web e APIs da Web.
- Solicitar um token de acesso para acessar uma API da Web protegida.

Este artigo descreve rapidamente e fornece links para exemplos para o ponto de extremidade de plataforma do Microsoft identity. Esses exemplos mostram como ele é feito e também fornecer trechos de código que você pode usar em seus aplicativos. Na página de código de exemplo, você encontrará tópicos Leiame detalhados que ajudam com requisitos, instalação e configuração. Comentários dentro do código de ajudarão-lo a entender as seções críticas.

> [!NOTE]
> Se você estiver interessado nos exemplos de v 1.0, consulte [exemplos de código do Azure AD (ponto de extremidade v1.0)](sample-v1-code.md).

Para entender o cenário básico para cada tipo de amostra, consulte [tipos de aplicativo para o ponto de extremidade de plataforma de identidade do Microsoft](v2-app-types.md).

Você também pode contribuir com os exemplos no GitHub. Para saber como, consulte [Exemplos e documentação do Microsoft Azure Active Directory](https://github.com/Azure-Samples?page=3&query=active-directory).

## <a name="single-page-applications"></a>Aplicativos de página única

Estes exemplos mostram como escrever um aplicativo de página única protegido com a plataforma de identidade da Microsoft. Esses exemplos usam um dos sabores da msal.

| Plataforma | DESCRIÇÃO | Link |
| -------- | --------------------- | -------- |
| ![Esta imagem mostra o logotipo do JavaScript](media/sample-v2-code/logo_js.png) [JavaScript (msal)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | Chama o Microsoft Graph |[javascript-graphapi-web-v2](https://github.com/Azure-Samples/active-directory-javascript-graphapi-web-v2) |
| ![Esta imagem mostra o logotipo do JavaScript](media/sample-v2-code/logo_js.png) [JavaScript (msal)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | Chama o B2C |[b2c-javascript-msal-singlepageapp](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp) |
| ![Esta imagem mostra o logotipo do JavaScript](media/sample-v2-code/logo_js.png) [JavaScript (msal)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-core) | Chamadas própria API da web do |[javascript-singlepageapp-dotnet-webapi-v2](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi-v2) |
| ![Esta imagem mostra o logotipo do Angular JS](media/sample-v2-code/logo_angular.png) [JavaScript (MSAL AngularJS)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angularjs)| Chama o Microsoft Graph  | [MsalAngularjsDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angularjs/samples/MsalAngularjsDemoApp)
| ![Esta imagem mostra o logotipo do Angular](media/sample-v2-code/logo_angular.png) [JavaScript (MSAL Angular)](https://github.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/lib/msal-angular)| Chama o Microsoft Graph  | [MSALAngularDemoApp](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-angular/samples/MSALAngularDemoApp) |

## <a name="web-applications"></a>Aplicativos Web

Os exemplos a seguir ilustram os aplicativos Web que iniciam sessão de usuários. Alguns exemplos também demonstram o aplicativo chamando o Microsoft Graph ou sua própria API Web com a identidade do usuário.

| Plataforma | Conecta apenas usuários | Conecta usuários e chama o Microsoft Graph |
| -------- | ------------------- | --------------------------------- |
| ![Esta imagem mostra o logotipo do ASP.NET Core](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2.2 | [Tutorial do ASP.NET Core WebApp usuários entrar](https://aka.ms/aspnetcore-webapp-sign-in) | Mesmo exemplo na [aplicativo Web ASP.NET Core chama o Microsoft Graph](https://aka.ms/aspnetcore-webapp-call-msgraph) fase |
| ![Esta imagem mostra o logotipo do ASP.NET](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET | [Início rápido do ASP.NET](https://github.com/AzureAdQuickstarts/AppModelv2-WebApp-OpenIDConnect-DotNet) </p> [dotnet-webapp-openidconnect-v2](https://github.com/azure-samples/active-directory-dotnet-webapp-openidconnect-v2)  |  [dotnet-admin-restricted-scopes-v2](https://github.com/azure-samples/active-directory-dotnet-admin-restricted-scopes-v2) </p> |[msgraph-training-aspnetmvcapp](https://github.com/microsoftgraph/msgraph-training-aspnetmvcapp)
| ![Esta imagem mostra o logotipo do Node. js](media/sample-v2-code/logo_nodejs.png)  |                   | [Início rápido do Node.js](https://github.com/azureadquickstarts/appmodelv2-webapp-openidconnect-nodejs) |
| ![Esta imagem mostra o logotipo do Ruby](media/sample-v2-code/logo_ruby.png) |                   | [msgraph-training-rubyrailsapp](https://github.com/microsoftgraph/msgraph-training-rubyrailsapp) |

## <a name="desktop-and-mobile-public-client-apps"></a>Aplicativos cliente públicos de desktop e móveis

Os exemplos a seguir mostram aplicativos (aplicativos da área de trabalho ou móveis) que acessar a API do Microsoft Graph ou sua própria API web no nome de um usuário de cliente público. Todos esses aplicativos de cliente usam a biblioteca de autenticação da Microsoft (MSAL).

| Aplicativo cliente | Plataforma | Fluxo/Concessão | Chama o Microsoft Graph | Chama uma API da web do ASP.NET Core 2.0 |
| ------------------ | -------- |  ----------| ---------- | ------------------------- |
| Desktop (WPF)      | ![Esta imagem mostra do .NET /C# logotipo](media/sample-v2-code/logo_NET.png) | [interativo](msal-authentication-flows.md#interactive)| [dotnet-desktop-msgraph-v2](https://github.com/azure-samples/active-directory-dotnet-desktop-msgraph-v2) | [dotnet-native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi) |
| Área de trabalho (Console)   | ![Esta imagem mostra do .NET /C# logotipo (Desktop)](media/sample-v2-code/logo_NET.png) | [Autenticação Integrada do Windows](msal-authentication-flows.md#integrated-windows-authentication) | [dotnet-iwa-v2](https://github.com/azure-samples/active-directory-dotnet-iwa-v2) |  |
| Área de trabalho (Console)   | ![Esta imagem mostra do .NET /C# logotipo (Desktop)](media/sample-v2-code/logo_NETcore.png) | [Nome de usuário/senha](msal-authentication-flows.md#usernamepassword) |[dotnetcore-up-v2](https://github.com/azure-samples/active-directory-dotnetcore-console-up-v2) |  |
| Móveis (Android, iOS, UWP)   | ![Esta imagem mostra do .NET /C# logotipo (Xamarin)](media/sample-v2-code/logo_xamarin.png) | [interativo](msal-authentication-flows.md#interactive) |[xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) |  |
| Mobile (iOS)       | ![Esta imagem mostra iOS/Objective-C ou Swift.](media/sample-v2-code/logo_iOS.png) | [interativo](msal-authentication-flows.md#interactive) |[ios-swift-native-v2](https://github.com/azure-samples/active-directory-ios-swift-native-v2) </p> [ios-native-nxoauth2-v2](https://github.com/azure-samples/active-directory-ios-native-nxoauth2-v2) |  |
| Mobile (Android)   | ![Esta imagem mostra o logotipo do Android](media/sample-v2-code/logo_Android.png) | [interativo](msal-authentication-flows.md#interactive) |  [android-native-v2](https://github.com/azure-samples/active-directory-android-native-v2 ) |  |

## <a name="daemon-applications"></a>Aplicativos de daemon

Os exemplos a seguir mostram um aplicativo que acessa a API do Microsoft Graph com sua própria identidade (sem usuários).

| Aplicativo cliente | Plataforma | Fluxo/Concessão | Chama o Microsoft Graph |
| ------------------ | -------- | ---------- | -------------------- |
| Console | ![Esta imagem mostra o logotipo do .NET Core](media/sample-v2-code/logo_NETcore.png)</p> ASP.NET  | [Credenciais do cliente](msal-authentication-flows.md#client-credentials) | [dotnetcore-daemon-v2](https://github.com/azure-samples/active-directory-dotnetcore-daemon-v2) |
| Aplicativo Web | ![Esta imagem mostra o logotipo do ASP.NET](media/sample-v2-code/logo_NETframework.png)</p> ASP.NET  | [Credenciais do cliente](msal-authentication-flows.md#client-credentials) | [dotnet-daemon-v2](https://github.com/azure-samples/active-directory-dotnet-daemon-v2) |

## <a name="headless-applications"></a>Aplicativos sem periféricos

O exemplo a seguir mostra um aplicativo cliente público em execução em um dispositivo sem um navegador da Web. O aplicativo pode ser uma ferramenta de linha de comando, um aplicativo em execução no Linux ou Mac ou um aplicativo de IoT. O exemplo apresenta um aplicativo acessando a API do Microsoft Graph, no nome de um usuário que entra no modo interativo em outro dispositivo (como um telefone celular). Esse aplicativo cliente usa a biblioteca de autenticação da Microsoft (MSAL).

| Aplicativo cliente | Plataforma | Fluxo/Concessão | Chama o Microsoft Graph |
| ------------------ | -------- |  ----------| ---------- |
| Área de trabalho (Console)   | ![Esta imagem mostra do .NET /C# logotipo (Desktop)](media/sample-v2-code/logo_NETcore.png) | [Fluxo de código do dispositivo](msal-authentication-flows.md#device-code) |[dotnetcore-devicecodeflow-v2](https://github.com/azure-samples/active-directory-dotnetcore-devicecodeflow-v2) |

## <a name="web-apis"></a>APIs da Web

Os exemplos a seguir mostram como proteger uma API web com o ponto de extremidade de plataforma de identidade do Microsoft e como chamar a API downstream da API web.

| Plataforma | Amostra |
| -------- | ------------------- |
| ![Esta imagem mostra o logotipo do ASP.NET Core](media/sample-v2-code/logo_NETcore.png)</p>ASP.NET Core 2.2 | API (serviço) de web do ASP.NET Core de [dotnet-native-aspnetcore-v2](https://aka.ms/msidentity-aspnetcore-webapi-calls-msgraph)  |
| ![Esta imagem mostra o logotipo do ASP.NET](media/sample-v2-code/logo_NET.png)</p>ASP.NET MVC | Web API (serviço) de [ms-identity-aspnet-webapi-onbehalfof](https://github.com/Azure-Samples/ms-identity-aspnet-webapi-onbehalfof) |

## <a name="other-microsoft-graph-samples"></a>Outros exemplos do Microsoft Graph

Para saber mais sobre [exemplos](https://github.com/microsoftgraph/msgraph-community-samples/tree/master/samples#aspnet) e tutoriais que demonstram os diferentes padrões de uso para a API do Microsoft Graph, incluindo autenticação com o Microsoft Azure AD, consulte [Exemplos e tutoriais da Comunidade do Microsoft Graph](https://github.com/microsoftgraph/msgraph-community-samples).

## <a name="see-also"></a>Consulte também

- [Guia do desenvolvedor do Azure Active Directory (v1.0)](v1-overview.md)
- [Conceitos e referência da API do Graph do Microsoft Azure AD](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Biblioteca auxiliar da API do Graph do Azure AD](https://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient)
