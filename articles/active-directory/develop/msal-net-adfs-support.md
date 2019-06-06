---
title: Suporte do AD FS na biblioteca de autenticação da Microsoft para .NET | Azure
description: Saiba mais sobre o suporte a serviços de Federação do Active Directory (AD FS) na biblioteca de autenticação da Microsoft para .NET (MSAL.NET).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/03/2019
ms.author: jmprieur
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5963c5e83b5af3848edd934328caa1f095bd184
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676722"
---
# <a name="active-directory-federation-services-support-in-msalnet"></a>Suporte a serviços de Federação do Active Directory no MSAL.NET
Serviços de Federação do Active Directory (AD FS) no Windows Server permite que você adicione OpenID Connect e OAuth 2.0 com base em autenticação e autorização para aplicativos que você está desenvolvendo e tiverem esses aplicativos autenticar usuários diretamente no AD FS. Para obter mais informações, leia [cenários do AD FS para desenvolvedores](/windows-server/identity/ad-fs/overview/ad-fs-scenarios-for-developers).

Biblioteca de autenticação da Microsoft para .NET (MSAL.NET) dá suporte a dois cenários para se autenticar no AD FS:

- Fala MSAL.NET ao Azure Active Directory, que por si só é *federado* com o AD FS.
- Fala MSAL.NET *diretamente* a uma autoridade do AD FS, em que a versão do AD FS é OpenID Connect compatível (a partir do AD FS 2019). Conexão direta com o AD FS permite MSAL.NET autenticar com os aplicativos em execução no [do Azure Stack](https://azure.microsoft.com/overview/azure-stack/).

## <a name="msal-connects-to-azure-ad-which-is-federated-with-ad-fs"></a>MSAL se conecta ao Azure AD, que é federado com o AD FS
MSAL.NET dá suporte à conexão ao Azure AD, que conecta gerenciado-os usuários (usuários gerenciados no Azure AD) ou federado (usuários gerenciados pelo outro provedor de identidade como o AD FS). MSAL.NET não conhece o fato de que os usuários são federados. Como diz respeito-lo, ele fala para o Azure AD.

O [autoridade](msal-client-application-configuration.md#authority) você uso nesse caso é a autoridade usual (nome de host de autoridade + locatário, comum ou organizações).

### <a name="acquiring-a-token-interactively"></a>Adquirir um token interativamente
Quando você chama o `AcquireTokenInteractive` método, a experiência do usuário é normalmente:

1. O usuário insere seu ID de conta.
2. Resumidamente, o Azure AD exibirá a mensagem "Levando você à página da sua organização".
3. O usuário é redirecionado para a página de logon do provedor de identidade. Página de entrada geralmente é personalizada com o logotipo da organização.

Versões com suporte do AD FS neste cenário federado são AD FS v2, v3 do AD FS (Windows Server 2012 R2) e v4 do AD FS (AD FS 2016).

### <a name="acquiring-a-token-using-acquiretokenbyintegratedauthentication-or-acquiretokenbyusernamepassword"></a>Adquirindo um token usando AcquireTokenByIntegratedAuthentication ou AcquireTokenByUsernamePassword
Ao adquirir um token usando o `AcquireTokenByIntegratedAuthentication` ou `AcquireTokenByUsernamePassword` métodos, MSAL.NET obtém entrar em contato com o provedor de identidade com base no nome de usuário.  MSAL.NET recebe uma [token SAML 1.1](reference-saml-tokens.md) depois de entrar em contato com o provedor de identidade.  MSAL.NET, em seguida, fornece o token SAML para o Azure AD como uma asserção de usuário (semelhantes para o [fluxo on-behalf-of](msal-authentication-flows.md#on-behalf-of)) para retornar um JWT.

## <a name="msal-connects-directly-to-ad-fs"></a>MSAL se conecta diretamente ao AD FS
MSAL.NET dá suporte à conexão para o AD FS 2019, que é o Open ID Connect em conformidade. Ao se conectar diretamente ao AD FS, a autoridade que você vai querer usar para compilar seu aplicativo é semelhante ao `https://mysite.contoso.com/adfs/`.

Atualmente, não há nenhum plano para oferecer suporte a uma conexão direta para o AD FS 2016 ou do AD FS v2 (que não é OpenID Connect compatíveis). Se você precisar dar suporte a cenários que exigem uma conexão direta com AD FS 2016, use a versão mais recente do [Azure Active Directory Authentication Library](active-directory-authentication-libraries.md#microsoft-supported-client-libraries). Quando você atualizou seu sistema local para o AD FS 2019, você poderá usar MSAL.NET.
