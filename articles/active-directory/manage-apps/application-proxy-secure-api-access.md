---
title: Acesso APIs com o Proxy de aplicativo do Azure AD no local
description: O Proxy de aplicativo do Active Directory do Azure permite que aplicativos nativos acessar com segurança APIs e lógica de negócios que você hospedar no local ou em VMs na nuvem.
services: active-directory
author: jeevanbisht
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: celested
ms.reviewer: japere
ms.openlocfilehash: c2b99525e3d0a61c02dc502fcd0927ea65993e5b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67108589"
---
# <a name="secure-access-to-on-premises-apis-with-azure-ad-application-proxy"></a>Proteger o acesso a APIs locais com o Proxy de aplicativo do Azure AD

Você pode ter a lógica de negócios APIs em execução no local ou hospedado em máquinas virtuais na nuvem. Aplicativos nativos Android, iOS, Mac ou Windows precisam interagir com os pontos de extremidade de API para usar dados ou fornecer a interação do usuário. O Proxy de aplicativo do Azure AD e o [Azure Active Directory autenticação bibliotecas (ADAL)](/azure/active-directory/develop/active-directory-authentication-libraries) permitem que seus aplicativos nativos acessarem com segurança suas APIs no local. Proxy de aplicativo do Active Directory do Azure é uma solução mais rápida e segura que a abertura de portas de firewall e controlar a autenticação e autorização na camada do aplicativo. 

Este artigo orienta você pela configuração de uma solução de Proxy de aplicativo do Azure AD para hospedar um serviço de API da web que podem acessar a aplicativos nativos. 

## <a name="overview"></a>Visão geral

O diagrama a seguir mostra uma maneira tradicional de publicar APIs locais. Essa abordagem exige a abertura de portas de entrada 80 e 443.

![Acesso de API tradicional](./media/application-proxy-secure-api-access/overview-publish-api-open-ports.png)

O diagrama a seguir mostra como você pode usar o Proxy de aplicativo do Azure AD para publicar com segurança APIs sem abrir portas de entrada:

![Acesso de API de Proxy de aplicativo do AD do Azure](./media/application-proxy-secure-api-access/overview-publish-api-app-proxy.png)

O Proxy de aplicativo do Azure AD constitui o backbone da solução, trabalhando como um ponto de extremidade público para acesso à API e fornecendo autenticação e autorização. Você pode acessar suas APIs de uma grande variedade de plataformas usando o [ADAL](/azure/active-directory/develop/active-directory-authentication-libraries) bibliotecas. 

Uma vez que a autorização e autenticação de Proxy de aplicativo do Azure AD são criados sobre o Azure AD, você pode usar o acesso condicional do Azure AD para garantir que somente os dispositivos confiáveis podem acessar as APIs publicadas por meio do Proxy de aplicativo. Use o ingresso no Azure AD ou do Azure AD híbrido ingressado para desktops e gerenciados pelo Intune para dispositivos. Você também pode tirar proveito dos recursos do Azure Active Directory Premium, como autenticação multifator do Azure e a segurança do machine learning com suporte de [Azure Identity Protection](/azure/active-directory/active-directory-identityprotection).

## <a name="prerequisites"></a>Pré-requisitos

Para seguir este passo a passo, você precisa:

- Acesso de administrador para um diretório do Azure, com uma conta que possa criar e registrar aplicativos
- A API web de exemplo e aplicativos de cliente nativo do [https://github.com/jeevanbisht/API-NativeApp-ADAL-SampleApp](https://github.com/jeevanbisht/API-NativeApp-ADAL-SampleApp) 

## <a name="publish-the-api-through-application-proxy"></a>Publicar a API por meio do Proxy de aplicativo

Para publicar uma API fora de sua intranet por meio do Proxy de aplicativo, você deve seguir o mesmo padrão de publicação de aplicativos web. Para obter mais informações, confira [Tutorial: Adicionar um aplicativo local para acesso remoto por meio do Proxy de aplicativo no Azure Active Directory](application-proxy-add-on-premises-application.md).

Para publicar a API da web SecretAPI por meio do Proxy de aplicativo:

1. Criar e publicar o projeto de exemplo SecretAPI como um aplicativo da web ASP.NET no computador local ou na intranet. Certifique-se de que você pode acessar o aplicativo web localmente. 
   
1. No [portal do Azure](https://portal.azure.com), selecione **Azure Active Directory** no painel de navegação à esquerda. Em seguida, na **visão geral** página, selecione **aplicativos empresariais**.
   
1. Na parte superior do **aplicativos empresariais – todos os aplicativos** página, selecione **novo aplicativo**.
   
1. Sobre o **adicionar um aplicativo** página, em **adicionar seu próprio aplicativo**, selecione **aplicativo local**. 
   
1. Se você não tiver um conector de Proxy de aplicativo instalado, você será solicitado a instalá-lo. Selecione **baixar conector de Proxy de aplicativo** para baixar e instalar o conector. 
   
1. Depois que você instalou o conector de Proxy de aplicativo, o **adicionar seu próprio aplicativo local** página:
   
   1. Insira *SecretAPI* lado **nome**.
      
   1. Insira a URL usada para acessar a API de dentro de sua intranet lado **Url interna**. 
      
   1. Certifique-se **pré-autenticação** é definido como **Azure Active Directory**. 
      
   1. Selecione **adicionar** na parte superior da página e aguarde até que o aplicativo a ser criado.
   
   ![Adicionar o aplicativo de API](./media/application-proxy-secure-api-access/3-add-api-app.png)
   
1. Sobre o **aplicativos empresariais – todos os aplicativos** página, selecione o **SecretAPI** aplicativo. 
   
1. Sobre o **SecretAPI - visão geral** página, selecione **propriedades** no painel de navegação à esquerda.
   
1. Você não quer APIs estejam disponíveis para usuários finais na **MyApps** do painel, portanto, defina **visível para os usuários** para **não** na parte inferior do **propriedades**página e, em seguida, selecione **salvar**.
   
   ![Não é visível para os usuários](./media/application-proxy-secure-api-access/5-not-visible-to-users.png)
   
Você publicou sua API da web por meio do Proxy de aplicativo do Azure AD. Agora, adicione os usuários que podem acessar o aplicativo. 

1. Sobre o **SecretAPI - visão geral** página, selecione **usuários e grupos** no painel de navegação à esquerda.
   
1. Sobre o **usuários e grupos** página, selecione **adicionar usuário**.  
   
1. Sobre o **Adicionar atribuição** página, selecione **usuários e grupos**. 
   
1. Sobre o **usuários e grupos** página, procure e selecione os usuários podem acessar o aplicativo, incluindo pelo menos por conta própria. Depois de selecionar todos os usuários, selecione **selecionar**. 
   
   ![Selecionar e atribuir usuário](./media/application-proxy-secure-api-access/7-select-admin-user.png)
   
1. Volta a **Adicionar atribuição** página, selecione **atribuir**. 

> [!NOTE]
> As APIs que usam autenticação integrada do Windows podem exigir [etapas adicionais](/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd).

## <a name="register-the-native-app-and-grant-access-to-the-api"></a>Registrar o aplicativo nativo e conceder acesso à API

Aplicativos nativos são programas desenvolvidos para usar em um dispositivo ou plataforma específica. Antes de seu aplicativo nativo pode se conectar e acessar uma API, você deve registrá-lo no Azure AD. As etapas a seguir mostram como registrar um aplicativo nativo e dê a ele acesso à API publicado por meio do Proxy de aplicativo da web.

Para registrar o aplicativo nativo de AppProxyNativeAppSample:

1. No Azure Active Directory **visão geral** página, selecione **registros de aplicativo**e na parte superior dos **registros do aplicativo** painel, selecione **novo registro** .
   
1. Sobre o **registrar um aplicativo** página:
   
   1. Sob **nome**, insira *AppProxyNativeAppSample*. 
      
   1. Em **Tipos de conta com suporte**, selecione **Contas em qualquer diretório organizacional e contas pessoais da Microsoft**. 
      
   1. Sob **URL de redirecionamento**, a lista suspensa e selecione **cliente público (móvel e desktop)** e, em seguida, insira *https:\//appproxynativeapp*. 
      
   1. Selecione **registrar**e aguarde até que o aplicativo a ser registrado com êxito. 
      
      ![Novo registro de aplicativo](./media/application-proxy-secure-api-access/8-create-reg-ga.png)
   
Agora, você registrou o aplicativo AppProxyNativeAppSample no Azure Active Directory. Para dar a seu aplicativo nativo acesso à API da web SecretAPI:

1. No Azure Active Directory **visão geral** > **registros do aplicativo** página, selecione o **AppProxyNativeAppSample** aplicativo. 
   
1. Sobre o **AppProxyNativeAppSample** página, selecione **permissões de API** no painel de navegação à esquerda. 
   
1. Sobre o **permissões de API** página, selecione **adicionar uma permissão**.
   
1. No primeiro **permissões de API de solicitação** página, selecione o **APIs minha organização utiliza** e, em seguida, pesquise e selecione **SecretAPI**. 
   
1. Na próxima **permissões de API de solicitação** , marque a caixa de seleção ao lado de **user_impersonation**e, em seguida, selecione **adicionar permissões**. 
   
    ![Selecionar uma API](./media/application-proxy-secure-api-access/10-secretapi-added.png)
   
1. Volta a **permissões de API** página, você pode selecionar **conceder consentimento do administrador da Contoso** para impedir que outros usuários tenham individualmente dar consentimento para o aplicativo. 

## <a name="configure-the-native-app-code"></a>Configure o código do aplicativo nativo

A última etapa é configurar o aplicativo nativo. O trecho a seguir da *Form1.cs* arquivo no aplicativo de exemplo NativeClient faz com que a biblioteca ADAL adquirir o token para solicitar a chamada à API e anexá-lo como portador para o cabeçalho de aplicativo. 
   
   ```csharp
       AuthenticationResult result = null;
       HttpClient httpClient = new HttpClient();
       authContext = new AuthenticationContext(authority);
       result = await authContext.AcquireTokenAsync(todoListResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
       
       // Append the token as bearer in the request header.
       httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);
       
       // Call the API.
       HttpResponseMessage response = await httpClient.GetAsync(todoListBaseAddress + "/api/values/4");
   
       // MessageBox.Show(response.RequestMessage.ToString());
       string s = await response.Content.ReadAsStringAsync();
       MessageBox.Show(s);
   ```
   
Para configurar o aplicativo nativo para se conectar ao Azure Active Directory e chamar o API App Proxy, atualize os valores de espaço reservado na *App. config* arquivo do aplicativo de exemplo NativeClient com os valores do Azure AD: 

- Cole a **ID de diretório (Locatário)** no `<add key="ida:Tenant" value="" />` campo. Você pode localizar e copiar esse valor (um GUID) da **visão geral** página de um dos seus aplicativos. 
  
- Cole o AppProxyNativeAppSample **ID do aplicativo (cliente)** no `<add key="ida:ClientId" value="" />` campo. Você pode localizar e copiar esse valor (um GUID) da AppProxyNativeAppSample **visão geral** página.
  
- Cole o AppProxyNativeAppSample **URI de redirecionamento** no `<add key="ida:RedirectUri" value="" />` campo. Você pode localizar e copiar esse valor (um URI) da AppProxyNativeAppSample **autenticação** página. 
  
- Cole o SecretAPI **URI da ID do aplicativo** no `<add key="todo:TodoListResourceId" value="" />` campo. Você pode localizar e copiar esse valor (um URI) da SecretAPI **expor uma API** página.
  
- Cole o SecretAPI **URL da Home Page** no `<add key="todo:TodoListBaseAddress" value="" />` campo. Você pode localizar e copiar esse valor (uma URL) da SecretAPI **Branding** página.

Depois de configurar os parâmetros, compile e execute o aplicativo nativo. Quando você seleciona os **entrar** botão, o aplicativo permite que você entrar e, em seguida, exibe uma tela de sucesso para confirmar que a TI com êxito conectado para o SecretAPI.

![Êxito](./media/application-proxy-secure-api-access/success.png)

## <a name="next-steps"></a>Próximas etapas

- [Tutorial: Adicionar um aplicativo local para acesso remoto por meio do Proxy de aplicativo no Azure Active Directory](application-proxy-add-on-premises-application.md)
- [Início Rápido: Configurar um aplicativo cliente para acessar APIs web](../develop/quickstart-configure-app-access-web-apis.md)
- [Como habilitar aplicativos cliente nativos interagir com aplicativos de proxy](application-proxy-configure-native-client-application.md)
