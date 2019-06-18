---
title: Use a biblioteca de autenticação da Microsoft (MSAL) nas nuvens nacionais - plataforma de identidade da Microsoft
description: Biblioteca de autenticação da Microsoft (MSAL) permite que os desenvolvedores de aplicativos adquirir tokens para chamar APIs da web protegida. Essas APIs da web pode ser o Microsoft Graph, outras APIs da Microsoft, APIs da web de parceiro ou sua própria API web. A MSAL dá suporte a várias arquiteturas de aplicativos e plataformas.
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
ms.date: 05/07/2019
ms.author: negoe
ms.reviewer: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: f9958356cae3c486ecf68e280f33d63c6a537b14
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66235259"
---
# <a name="use-msal-in-a-national-cloud-environment"></a>Usar a MSAL em um ambiente de nuvem nacionais

[Nuvens nacionais](authentication-national-cloud.md) são instâncias isoladas fisicamente do Azure. Essas regiões do Azure ajudam a garantir que os requisitos de conformidade, Soberania e residência de dados são respeitados dentro dos limites geográficos.

Além de nuvem da Microsoft em todo o mundo, a biblioteca de autenticação Microsoft (MSAL) permite que os desenvolvedores de aplicativos em nuvens nacionais adquirir tokens para autenticar e chamar APIs da web protegida. Essas APIs da web pode ser o Microsoft Graph ou outras APIs de Microsoft.

Incluindo a nuvem global, o Azure Active Directory (Azure AD) é implantado nas nuvens nacionais do seguintes:  

- Azure Government
- Azure China 21Vianet
- Azure Alemanha

Este guia demonstra como entrar trabalho e contas corporativas, obter um acesso de token e chamar a API do Microsoft Graph na [nuvem do Azure governamental](https://azure.microsoft.com/global-infrastructure/government/) ambiente.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que estes pré-requisitos são atendidos.

### <a name="choose-the-appropriate-identities"></a>Escolha as identidades apropriadas

[O Azure governamental](https://docs.microsoft.com/azure/azure-government/) aplicativos podem usar identidades de governo do Azure AD e identidades pública do Azure AD para autenticar usuários. Como você pode usar qualquer uma dessas identidades, você precisará decidir qual ponto de extremidade de autoridade que você deve escolher para seu cenário:

- Público do AD do Azure: Normalmente usado se sua organização já tiver um locatário pública do Azure AD para o suporte do Office 365 (público ou GCC) ou outro aplicativo.
- Azure AD Government: Normalmente usado se a sua organização já tiver um locatário do governo do Azure AD para o suporte do Office 365 (alta GCC ou DoD) ou é criar um novo locatário no Azure governamental de AD.

Depois de decidir, uma consideração especial é onde você pode executar o registro do aplicativo. Se você escolher identidades pública do Azure AD para seu aplicativo do Azure governamental, você deve registrar o aplicativo em seu locatário do AD do Azure público.

### <a name="get-an-azure-government-subscription"></a>Obter uma assinatura do Azure governamental

Para obter uma assinatura do Azure governamental, veja [Gerenciando e conectando-se a sua assinatura no Azure governamental](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-subscriptions).

Se você não tiver uma assinatura do Azure governamental, crie uma [conta gratuita](https://azure.microsoft.com/global-infrastructure/government/request/) antes de começar.

## <a name="javascript"></a>JavaScript

### <a name="step-1-register-your-application"></a>Etapa 1: Registre seu aplicativo

1. Entre no [Portal do Azure](https://portal.azure.us/).
    
   Para localizar pontos de extremidade do portal do Azure para outras nuvens nacionais, consulte [pontos de extremidade de registro de aplicativo](authentication-national-cloud.md#app-registration-endpoints).

1. Se sua conta der acesso a mais de um locatário, selecione sua conta no canto superior direito e defina a sessão do portal com o Azure AD desejado de locatário.
1. Vá para o [registros de aplicativo](https://aka.ms/ra/ff) página na plataforma de identidade da Microsoft para desenvolvedores.
1. Quando a página **Registrar um aplicativo** aparecer, insira um nome para o seu aplicativo.
1. Sob **suporte para tipos de conta**, selecione **contas em qualquer diretório organizacional**.
1. No **URI de redirecionamento** seção, selecione a **Web** plataforma e defina o valor para a URL do aplicativo com base em seu servidor web. Consulte as seções a seguir para obter instruções sobre como definir e obter a URL de redirecionamento no Visual Studio e o nó.
1. Selecione **Registrar**.
1. Na página **Visão geral** do aplicativo, anote o valor de **ID do aplicativo (cliente)** .
1. Este tutorial requer que você habilite o [fluxo de concessão implícita](v2-oauth2-implicit-grant-flow.md). No painel esquerdo do aplicativo registrado, selecione **autenticação**.
1. Na **configurações avançadas**, em **concessão implícita**, selecione o **tokens de ID** e **tokens de acesso** caixas de seleção. Tokens de ID e tokens de acesso são necessários porque esse aplicativo precisa conectar usuários e chamar uma API.
1. Clique em **Salvar**.

### <a name="step-2--set-up-your-web-server-or-project"></a>Etapa 2:  Configurar seu servidor web ou um projeto

- [Baixar os arquivos de projeto](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip) para um servidor web local, como o nó.

  ou o

- [Baixe o projeto do Visual Studio](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip).

Em seguida, vá para [configurar seu JavaScript SPA](#step-4-configure-your-javascript-spa) para configurar o exemplo de código antes de executá-lo.

### <a name="step-3-use-the-microsoft-authentication-library-to-sign-in-the-user"></a>Etapa 3: Use a biblioteca de autenticação da Microsoft para a entrada do usuário

Siga as etapas na [JavaScript tutorial](tutorial-v2-javascript-spa.md#create-your-project) para criar seu projeto e integrar com a MSAL para a entrada do usuário.

### <a name="step-4-configure-your-javascript-spa"></a>Etapa 4: Configurar o JavaScript SPA

No arquivo `index.html` criado durante a configuração do projeto, inclua as informações de registro do aplicativo. Adicione o seguinte código na parte superior dentro das marcas `<script></script>` no corpo do arquivo `index.html`:

```javascript
const msalConfig = {
    auth:{
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.us/Enter_the_Tenant_Info_Here",
        }
}

const graphConfig = {
        graphEndpoint: "https://graph.microsoft.us",
        graphScopes: ["user.read"],
}

// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(msalConfig);
```

Nesse código:

- `Enter_the_Application_Id_here` é o **ID do aplicativo (cliente)** valor para o aplicativo que você registrou.
- `Enter_the_Tenant_Info_Here` é definido como uma das seguintes opções:
    - Se seu aplicativo suporta **contas neste diretório organizacional**, substitua esse valor com a ID de locatário ou nome (por exemplo, contoso.microsoft.com) do locatário.
    - Se seu aplicativo suporta **contas em qualquer diretório organizacional**, substitua esse valor com `organizations`.
    
    Para localizar pontos de extremidade de autenticação para todas as nuvens nacionais, consulte [pontos de extremidade de autenticação do Azure AD](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud#azure-ad-authentication-endpoints).

    > [!NOTE]
    > Não há suporte para contas pessoais da Microsoft em nuvens nacionais.
  
- `graphEndpoint` é o ponto de extremidade do Microsoft Graph para a nuvem da Microsoft para o governo dos EUA.

   Para localizar pontos de extremidade do Microsoft Graph para todas as nuvens nacionais, consulte [pontos de extremidade do Microsoft Graph em nuvens nacionais](https://docs.microsoft.com/graph/deployments#microsoft-graph-and-graph-explorer-service-root-endpoints).

## <a name="net"></a>.NET

Você pode usar MSAL.NET para conectar usuários, adquira tokens e chamar a API do Microsoft Graph em nuvens nacionais.

Os tutoriais a seguir demonstram como criar um aplicativo Web MVC do .NET Core 2.2. O aplicativo usa o OpenID Connect para conectar os usuários com uma conta corporativas e de estudantes em uma organização que pertence a uma nuvem nacional.

- Para conectar usuários e adquirir tokens, siga [este tutorial](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-4-Sovereign#build-an-aspnet-core-web-app-signing-in-users-in-sovereign-clouds-with-the-microsoft-identity-platform).
- Para chamar a API do Microsoft Graph, execute [este tutorial](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-4-Sovereign-Call-MSGraph#using-the-microsoft-identity-platform-to-call-the-microsoft-graph-api-from-an-an-aspnet-core-2x-web-app-on-behalf-of-a-user-signing-in-using-their-work-and-school-account-in-microsoft-national-cloud).

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre:

- [Azure Governamental](https://docs.microsoft.com/azure/azure-government/)
- [Azure China 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Alemanha](https://docs.microsoft.com/azure/germany/)