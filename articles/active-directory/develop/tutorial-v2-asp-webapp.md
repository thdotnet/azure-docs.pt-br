---
title: Introdução ao servidor Web ASP.NET no Azure AD v2.0 | Microsoft Docs
description: Implementando a opção Entrar com uma Conta da Microsoft em uma solução ASP.NET com um aplicativo tradicional baseado em navegador da Web usando o padrão OpenID Connect
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.collection: M365-identity-device-management
ms.openlocfilehash: fce6ab2c5068d1628860356a9df0dd33c73948b3
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69511965"
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Adicionar a opção Entrar com uma Conta da Microsoft a um aplicativo Web ASP.NET

Este guia demonstra como implementar a opção Entrar com uma Conta da Microsoft usando uma solução ASP.NET MVC com um aplicativo tradicional baseado em navegador da Web usando o OpenID Connect.

No fim deste guia, seu aplicativo poderá aceitar entradas de contas pessoais, por exemplo outlook.com, live.com e outros. Essas contas também incluem contas corporativas e de estudante de qualquer empresa ou organização que se integrou ao Azure Active Directory.

> Este guia requer o Visual Studio 2019.  Ainda não tem?  [Baixe gratuitamente o Visual Studio 2019](https://www.visualstudio.com/downloads/)

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Como o aplicativo de exemplo gerado por este guia funciona

![Mostra como funciona o aplicativo de exemplo gerado por este tutorial](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral.svg)

Este aplicativo de exemplo que você cria é baseado no cenário em que você usa o navegador para acessar um site ASP.NET que solicita que um usuário faça sua autenticação por meio de um botão de entrada. Nesse cenário, a maior parte do trabalho de renderização da página da Web ocorre no lado do servidor.

## <a name="libraries"></a>Bibliotecas

Este guia usa as seguintes bibliotecas:

|Biblioteca|DESCRIÇÃO|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Middleware que permite que um aplicativo use OpenIdConnect para autenticação|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Middleware que permite que um aplicativo mantenha a sessão de usuário usando cookies|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|Permite que os aplicativos baseado em OWIN sejam executados no IIS usando o pipeline de solicitação do ASP.NET|

## <a name="set-up-your-project"></a>Configurar o seu projeto

Esta seção mostra as etapas para instalar e configurar o pipeline de autenticação por meio do middleware OWIN em um projeto do ASP.NET usando o OpenID Connect.

> Prefere baixar este projeto do Visual Studio de exemplo? [Baixe um projeto](https://github.com/AzureADQuickStarts/AppModelv2-WebApp-OpenIDConnect-DotNet/archive/master.zip) e vá para a [Etapa de configuração](#register-your-application) para configurar o exemplo de código antes de executá-lo.

### <a name="create-your-aspnet-project"></a>Criar seu projeto do ASP.NET

1. No Visual Studio: `File` > `New` > `Project`
2. Em *Visual C#\Web*, selecione `ASP.NET Web Application (.NET Framework)`.
3. Nomeie o aplicativo e clique em *OK*
4. Selecione `Empty` e marque a caixa de seleção para adicionar referências `MVC`

## <a name="add-authentication-components"></a>Adicionar componentes de autenticação

1. No Visual Studio: `Tools` > `Nuget Package Manager` > `Package Manager Console`
2. Adicione *Pacotes NuGet do middleware OWIN* digitando o seguinte na janela do Console do Gerenciador de Pacotes:

    ```powershell
    Install-Package Microsoft.Owin.Security.OpenIdConnect
    Install-Package Microsoft.Owin.Security.Cookies
    Install-Package Microsoft.Owin.Host.SystemWeb
    ```

<!--start-collapse-->
> ### <a name="about-these-libraries"></a>Sobre estas bibliotecas
> As bibliotecas acima possibilitam o SSO (logon único) usando o OpenID Connect por meio da autenticação baseada em cookie. Depois que a autenticação for concluída e o token que representa o usuário for enviado ao aplicativo, o middleware OWIN criará um cookie de sessão. Em seguida, o navegador usará esse cookie nas próximas solicitações, de forma que o usuário não precise digitar sua senha novamente, e nenhuma verificação adicional será necessária.
<!--end-collapse-->

## <a name="configure-the-authentication-pipeline"></a>Configurar o pipeline de autenticação

As etapas abaixo são usadas para criar uma Classe de Inicialização do middleware OWIN para configurar a autenticação do OpenID Connect. Essa classe será executada automaticamente quando o processo do IIS for iniciado.

> [!TIP]
> Se o projeto não tiver um arquivo `Startup.cs` na pasta raiz:
> 1. Clique com o botão direito do mouse na pasta raiz do projeto: > `Add` > `New Item...` > `OWIN Startup class`<br/>
> 2. Nomeie-o `Startup.cs`
>
>> Verifique se a classe selecionada é uma Classe de Inicialização OWIN e não uma classe C# padrão. Confirme isso verificando se você consegue ver `[assembly: OwinStartup(typeof({NameSpace}.Startup))]` acima do namespace.

1. Adicione as referências *OWIN* e *Microsoft.IdentityModel* a `Startup.cs`:

    ```csharp
    using Microsoft.Owin;
    using Owin;
    using Microsoft.IdentityModel.Protocols.OpenIdConnect;
    using Microsoft.IdentityModel.Tokens;
    using Microsoft.Owin.Security;
    using Microsoft.Owin.Security.Cookies;
    using Microsoft.Owin.Security.OpenIdConnect;
    using Microsoft.Owin.Security.Notifications;
    ```

2. Substitua a Classe de inicialização pelo seguinte código:

    ```csharp
    public class Startup
    {
        // The Client ID is used by the application to uniquely identify itself to Azure AD.
        string clientId = System.Configuration.ConfigurationManager.AppSettings["ClientId"];

        // RedirectUri is the URL where the user will be redirected to after they sign in.
        string redirectUri = System.Configuration.ConfigurationManager.AppSettings["RedirectUri"];

        // Tenant is the tenant ID (e.g. contoso.onmicrosoft.com, or 'common' for multi-tenant)
        static string tenant = System.Configuration.ConfigurationManager.AppSettings["Tenant"];

        // Authority is the URL for authority, composed by Azure Active Directory v2.0 endpoint and the tenant name (e.g. https://login.microsoftonline.com/contoso.onmicrosoft.com/v2.0)
        string authority = String.Format(System.Globalization.CultureInfo.InvariantCulture, System.Configuration.ConfigurationManager.AppSettings["Authority"], tenant);

        /// <summary>
        /// Configure OWIN to use OpenIdConnect 
        /// </summary>
        /// <param name="app"></param>
        public void Configuration(IAppBuilder app)
        {
            app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

            app.UseCookieAuthentication(new CookieAuthenticationOptions());
            app.UseOpenIdConnectAuthentication(
                new OpenIdConnectAuthenticationOptions
                {
                    // Sets the ClientId, authority, RedirectUri as obtained from web.config
                    ClientId = clientId,
                    Authority = authority,
                    RedirectUri = redirectUri,
                    // PostLogoutRedirectUri is the page that users will be redirected to after sign-out. In this case, it is using the home page
                    PostLogoutRedirectUri = redirectUri,
                    Scope = OpenIdConnectScope.OpenIdProfile,
                    // ResponseType is set to request the id_token - which contains basic information about the signed-in user
                    ResponseType = OpenIdConnectResponseType.IdToken,
                    // ValidateIssuer set to false to allow personal and work accounts from any organization to sign in to your application
                    // To only allow users from a single organizations, set ValidateIssuer to true and 'tenant' setting in web.config to the tenant name
                    // To allow users from only a list of specific organizations, set ValidateIssuer to true and use ValidIssuers parameter
                    TokenValidationParameters = new TokenValidationParameters()
                    {
                        ValidateIssuer = false // This is a simplification
                    },
                    // OpenIdConnectAuthenticationNotifications configures OWIN to send notification of failed authentications to OnAuthenticationFailed method
                    Notifications = new OpenIdConnectAuthenticationNotifications
                    {
                        AuthenticationFailed = OnAuthenticationFailed
                    }
                }
            );
        }

        /// <summary>
        /// Handle failed authentication requests by redirecting the user to the home page with an error in the query string
        /// </summary>
        /// <param name="context"></param>
        /// <returns></returns>
        private Task OnAuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> context)
        {
            context.HandleResponse();
            context.Response.Redirect("/?errormessage=" + context.Exception.Message);
            return Task.FromResult(0);
        }
    }
    ```

> [!NOTE]
> A configuração de `ValidateIssuer = false` é uma simplificação deste início rápido. Em aplicativos reais você precisa validar o emissor; consulte os exemplos para entender como fazer isso.

<!--start-collapse-->
> ### <a name="more-information"></a>Mais informações
> Os parâmetros fornecidos em *OpenIDConnectAuthenticationOptions* servem como coordenadas para seu aplicativo se comunicar com o Azure AD. Como o middleware OpenID Connect usa cookies em segundo plano, você também precisa configurar a autenticação de cookie, conforme o código acima mostra. O valor *ValidateIssuer* informa OpenIdConnect para não restringir o acesso a uma organização específica.
<!--end-collapse-->

## <a name="add-a-controller-to-handle-sign-in-and-sign-out-requests"></a>Adicionar um controlador para manipular solicitações de entrada e saída

Esta etapa mostra como criar um novo controlador para expor métodos de entrada e saída.

1.  Clique com o botão direito do mouse na pasta `Controllers` e selecione `Add` > `Controller`
2.  Selecione `MVC (.NET version) Controller – Empty`.
3.  Clique em *Adicionar*
4.  Nomeie-o `HomeController` e clique em *Adicionar*
5.  Adicione referências *OWIN* à classe:

    ```csharp
    using Microsoft.Owin.Security;
    using Microsoft.Owin.Security.Cookies;
    using Microsoft.Owin.Security.OpenIdConnect;
    ```
    
6. Adicione os dois métodos abaixo para manipular a entrada e saída do controlador iniciando um desafio de autenticação por meio de código:
    
    ```csharp
    /// <summary>
    /// Send an OpenID Connect sign-in request.
    /// Alternatively, you can just decorate the SignIn method with the [Authorize] attribute
    /// </summary>
    public void SignIn()
    {
        if (!Request.IsAuthenticated)
        {
            HttpContext.GetOwinContext().Authentication.Challenge(
                new AuthenticationProperties{ RedirectUri = "/" },
                OpenIdConnectAuthenticationDefaults.AuthenticationType);
        }
    }
    
    /// <summary>
    /// Send an OpenID Connect sign-out request.
    /// </summary>
    public void SignOut()
    {
        HttpContext.GetOwinContext().Authentication.SignOut(
                OpenIdConnectAuthenticationDefaults.AuthenticationType,
                CookieAuthenticationDefaults.AuthenticationType);
    }
    ```

## <a name="create-the-apps-home-page-to-sign-in-users-via-a-sign-in-button"></a>Criar a home page do aplicativo para conectar os usuários por meio de um botão de conexão

No Visual Studio, crie uma nova exibição para adicionar o botão de conexão e exibir informações de usuário após a autenticação:

1.  Clique com o botão direito do mouse na pasta `Views\Home` e selecione `Add View`
2.  Nomeie-o `Index`.
3.  Adicione o seguinte HTML, que inclui o botão de conexão, ao arquivo:

    ```html
    <html>
    <head>
        <meta name="viewport" content="width=device-width" />
        <title>Sign in with Microsoft Guide</title>
    </head>
    <body>
    @if (!Request.IsAuthenticated)
    {
        <!-- If the user is not authenticated, display the sign-in button -->
        <a href="@Url.Action("SignIn", "Home")" style="text-decoration: none;">
            <svg xmlns="http://www.w3.org/2000/svg" xml:space="preserve" width="300px" height="50px" viewBox="0 0 3278 522" class="SignInButton">
            <style type="text/css">.fil0:hover {fill: #4B4B4B;} .fnt0 {font-size: 260px;font-family: 'Segoe UI Semibold', 'Segoe UI'; text-decoration: none;}</style>
            <rect class="fil0" x="2" y="2" width="3174" height="517" fill="black" />
            <rect x="150" y="129" width="122" height="122" fill="#F35325" />
            <rect x="284" y="129" width="122" height="122" fill="#81BC06" />
            <rect x="150" y="263" width="122" height="122" fill="#05A6F0" />
            <rect x="284" y="263" width="122" height="122" fill="#FFBA08" />
            <text x="470" y="357" fill="white" class="fnt0">Sign in with Microsoft</text>
            </svg>
        </a>
    }
    else
    {
        <span><br/>Hello @System.Security.Claims.ClaimsPrincipal.Current.FindFirst("name").Value;</span>
        <br /><br />
        @Html.ActionLink("See Your Claims", "Index", "Claims")
        <br /><br />
        @Html.ActionLink("Sign out", "SignOut", "Home")
    }
    @if (!string.IsNullOrWhiteSpace(Request.QueryString["errormessage"]))
    {
        <div style="background-color:red;color:white;font-weight: bold;">Error: @Request.QueryString["errormessage"]</div>
    }
    </body>
    </html>
    ```

<!--start-collapse-->
> ### <a name="more-information"></a>Mais informações
> Esta página adiciona um botão de conexão no formato SVG com uma tela de fundo preta:<br/>![Entrar com a Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-use/aspnetsigninbuttonsample.png)<br/> Para obter mais botões de conexão, acesse [esta página](https://docs.microsoft.com/azure/active-directory/develop/active-directory-branding-guidelines "Diretrizes de identidade visual").
<!--end-collapse-->

## <a name="add-a-controller-to-display-users-claims"></a>Adicionar um controlador para exibir as declarações do usuário
Esse controlador demonstra os usos do atributo `[Authorize]` para proteger um controlador. Esse atributo restringe o acesso ao controlador permitindo apenas usuários autenticados. O código abaixo usa o atributo para exibir as declarações de usuário que foram recuperadas como parte da conexão.

1.  Clique com o botão direito do mouse na pasta `Controllers`: `Add` > `Controller`
2.  Selecione `MVC {version} Controller – Empty`.
3.  Clique em *Adicionar*
4.  Nomeie-o `ClaimsController`
5.  Substitua o código da classe de controlador pelo código abaixo – isso adiciona o atributo `[Authorize]` à classe:

    ```csharp
    [Authorize]
    public class ClaimsController : Controller
    {
        /// <summary>
        /// Add user's claims to viewbag
        /// </summary>
        /// <returns></returns>
        public ActionResult Index()
        {
            var userClaims = User.Identity as System.Security.Claims.ClaimsIdentity;
    
            //You get the user’s first and last name below:
            ViewBag.Name = userClaims?.FindFirst("name")?.Value;
    
            // The 'preferred_username' claim can be used for showing the username
            ViewBag.Username = userClaims?.FindFirst("preferred_username")?.Value;
    
            // The subject/ NameIdentifier claim can be used to uniquely identify the user across the web
            ViewBag.Subject = userClaims?.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier)?.Value;
    
            // TenantId is the unique Tenant Id - which represents an organization in Azure AD
            ViewBag.TenantId = userClaims?.FindFirst("http://schemas.microsoft.com/identity/claims/tenantid")?.Value;
    
            return View();
        }
    }
    ```

<!--start-collapse-->
> ### <a name="more-information"></a>Mais informações
> Devido ao uso do atributo `[Authorize]`, todos os métodos desse controlador podem ser executados apenas se o usuário está autenticado. Se o usuário não estiver autenticado e tentar acessar o controlador, o OWIN iniciará um desafio de autenticação e forçará o usuário a se autenticar. O código acima examina a lista de declarações em busca de atributos de usuário específicos incluídos no token do usuário. Esses atributos incluem o nome completo do usuário e o nome de usuário, bem como a entidade de identificador de usuário global. Também contém a *ID de Locatário*, que representa a ID da organização do usuário. 
<!--end-collapse-->

## <a name="create-a-view-to-display-the-users-claims"></a>Criar uma exibição para exibir as declarações do usuário

No Visual Studio, crie uma nova exibição para exibir as declarações do usuário em uma página da Web:

1.  Clique com o botão direito do mouse na pasta `Views\Claims` e: `Add View`
2.  Nomeie-o `Index`.
3.  Adicione o seguinte HTML ao arquivo:

    ```html
    <html>
    <head>
        <meta name="viewport" content="width=device-width" />
        <title>Sign in with Microsoft Sample</title>
        <link href="@Url.Content("~/Content/bootstrap.min.css")" rel="stylesheet" type="text/css" />
    </head>
    <body style="padding:50px">
        <h3>Main Claims:</h3>
        <table class="table table-striped table-bordered table-hover">
            <tr><td>Name</td><td>@ViewBag.Name</td></tr>
            <tr><td>Username</td><td>@ViewBag.Username</td></tr>
            <tr><td>Subject</td><td>@ViewBag.Subject</td></tr>
            <tr><td>TenantId</td><td>@ViewBag.TenantId</td></tr>
        </table>
        <br />
        <h3>All Claims:</h3>
        <table class="table table-striped table-bordered table-hover table-condensed">
        @foreach (var claim in System.Security.Claims.ClaimsPrincipal.Current.Claims)
        {
            <tr><td>@claim.Type</td><td>@claim.Value</td></tr>
        }
        </table>
        <br />
        <br />
        @Html.ActionLink("Sign out", "SignOut", "Home", null, new { @class = "btn btn-primary" })
    </body>
    </html>
    ```

## <a name="register-your-application"></a>Registre seu aplicativo

Para registrar seu aplicativo e adicionar suas informações de registro de aplicativo à sua solução, você tem duas opções:

### <a name="option-1-express-mode"></a>Opção 1: Modo Expresso

Registre rapidamente seu aplicativo fazendo o seguinte:

1. Acesse o novo painel do [portal do Azure – Registros de aplicativo](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs).
1. Insira um nome para o aplicativo e clique em **Registrar**.
1. Siga as instruções para baixar e configurar automaticamente o novo aplicativo com apenas um clique.

### <a name="option-2-advanced-mode"></a>Opção 2: Modo Avançado

Para registrar seu aplicativo e adicionar as informações de registro do aplicativo à solução manualmente, siga estas etapas:

1. Acesse o Visual Studio e:
   1. no Gerenciador de Soluções, selecione o projeto e examine a janela Propriedades (se uma janela Propriedades não for exibida, pressione F4).
   1. Altere habilitado para SSL `True`.
   1. Clique com botão direito no projeto no Visual Studio e, em seguida, escolha **propriedades**e o **Web** guia. No *servidores* seção alteração a *Url do projeto* para ser a URL de SSL.
   1. Copie a URL do SSL. Você adicionará essa URL à lista de URLs de Redirecionamento na lista de URLs de Redirecionamento do Portal de Registro na próxima etapa:<br/><br/>![Propriedades do projeto](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
1. Se sua conta fornecer acesso a mais de um locatário, selecione sua conta no canto superior direito e defina sua sessão do portal para o locatário desejado do Azure AD.
1. Navegue até a página [Registros de aplicativo](https://go.microsoft.com/fwlink/?linkid=2083908) da plataforma de identidade da Microsoft para desenvolvedores.
1. Selecione **Novo registro**.
1. Quando a página **Registrar um aplicativo** for exibida, insira as informações de registro do aplicativo:
   1. Na seção **Nome**, insira um nome de aplicativo relevante que será exibido aos usuários do aplicativo, por exemplo, `ASPNET-Tutorial`.
   1. Adicione a URL do SSL copiada do Visual Studio na etapa 1 (por exemplo, `https://localhost:44368/`) à **URL de Resposta** e clique em **Registrar**.
1. Selecione o menu **Autenticação** menu, defina **Tokens de ID** em **Concessão Implícita**e selecione **Salvar**.
1. Adicione o seguinte em `web.config` localizado na pasta raiz, na seção `configuration\appSettings`:

    ```xml
    <add key="ClientId" value="Enter_the_Application_Id_here" />
    <add key="redirectUri" value="Enter_the_Redirect_URL_here" />
    <add key="Tenant" value="common" />
    <add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" />
    ```

1. Substitua `ClientId` pelo ID do aplicativo que você acabou de registrar.
1. Substitua `redirectUri` com a URL do SSL de seu projeto.

## <a name="test-your-code"></a>Testar seu código

Para testar o aplicativo no Visual Studio, pressione **F5** para executar seu projeto. O navegador abre para o local http://<span></span>localhost:{port} e você visualiza o botão **Entrar com a conta da Microsoft**. Selecione o botão para iniciar o processo de entrada.

Quando estiver pronto para executar o teste, use uma conta do Microsoft Azure AD (Microsoft Azure Active Directory) (conta corporativa ou de estudante) ou uma conta pessoal da Microsoft (<span>live.</span>com ou <span>outlook.</span>com) para entrar.

![Entrar com a Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Entre na sua conta da Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

#### <a name="view-application-results"></a>Veja os resultados de aplicativo

Depois de entrar, o usuário será redirecionado para a home page do seu site. A home page é a URL HTTPS especificada nas informações de registro do aplicativo no Portal de Registro de Aplicativos da Microsoft. A página inicial inclui uma mensagem de boas-vindas *"Olá \<Usuário>,"* um link para sair e um link para exibir as declarações de usuário. O link para as declarações de usuário navega até o controlador *Declarações* criado anteriormente.

### <a name="browse-to-see-the-users-claims"></a>Navegue para ver as declarações de usuário

Para ver as declarações de usuário, selecione o link para navegar até a exibição do controlador disponível somente para usuários autenticados.

#### <a name="view-the-claims-results"></a>Exibir os resultados de declarações

Depois de navegar para a exibição do controlador, você deverá visualizar uma tabela que contém as propriedades básicas para o usuário:

|Propriedade |Valor |DESCRIÇÃO |
|---|---|---|
|**Nome** |Nome completo do usuário | O nome e sobrenome do usuário.
|**Nome de Usuário** |usuário<span>@domain.com</span> | O nome de usuário que é usado para identificar o usuário.
|**Assunto** |Assunto |Uma cadeia de caracteres que identifica de forma exclusiva o usuário na Web.|
|**ID do locatário** |Guid | Um **guid** que representa exclusivamente a organização do Microsoft Azure Active Directory do usuário.|

Além disso, você deve ver uma tabela de todas as declarações que estão na solicitação de autenticação. Para obter mais informações, consulte a [lista de declarações que estão em um Token de ID do Microsoft Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).

### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>Testar o acesso a um método que tenha um atributo Autorizar (opcional)

Para testar o acesso como um usuário anônimo a um controlador protegido com o atributo `Authorize`, siga estas etapas:

1. Selecione o link para desconectar o usuário e concluir o processo de saída.
2. No navegador, digite http://<span></span>localhost:{port}/claims para acessar o controlador que está protegido com o atributo `Authorize`.

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Resultados esperados após o acesso a um controlador protegido

Você será solicitado a autenticar para usar a exibição do controlador protegido.

## <a name="advanced-options"></a>Opções Avançadas

<!--start-collapse-->
### <a name="protect-your-entire-website"></a>Proteja todo o seu site
Para proteger todo o site, no arquivo **Global.asax**, adicione o atributo `AuthorizeAttribute` ao filtro `GlobalFilters` no método `Application_Start`:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

### <a name="restrict-who-can-sign-in-to-your-application"></a>Restringir quem pode se conectar ao seu aplicativo

Por padrão, quando você compila o aplicativo criado por este guia, o aplicativo aceitará conexões de contas pessoais (incluindo outlook.com, live.com e outras), bem como contas corporativas ou de estudante de qualquer empresa ou organização que foi integrada ao Azure Active Directory. Essa é uma opção recomendada para aplicativos SaaS.

Para restringir acesso de entrada de usuário para seu aplicativo, há várias opções disponíveis:

#### <a name="option-1-restrict-users-from-only-one-organizations-active-directory-instance-to-sign-in-to-your-application-single-tenant"></a>Opção 1: Restrinja usuários da instância do Active Directory de uma única organização entre no seu aplicativo (único locatário)

Essa opção é um cenário comum para *aplicativos de linha de negócios*: Se desejar que seu aplicativo aceite entradas apenas de contas pertencentes a uma instância específica do Azure Active Directory (incluindo *contas convidado* dessa instância), siga estas etapas:

1. No arquivo **web. config**, altere o valor para o parâmetro `Tenant` de `Common` para o nome do locatário da organização, como `contoso.onmicrosoft.com`.
2. Na [classe de inicialização OWIN](#configure-the-authentication-pipeline), defina o argumento `ValidateIssuer` como `true`.

#### <a name="option-2-restrict-access-to-your-application-to-users-in-a-specific-list-of-organizations"></a>Opção 2: restringir o acesso ao seu aplicativo aos usuários em uma lista específica de organizações

É possível restringir acesso de entrada apenas a contas de usuários que estão em uma organização do Microsoft Azure Active Directory que esteja na lista de organizações permitidas:
1. Na [classe de inicialização OWIN](#configure-the-authentication-pipeline), defina o argumento `ValidateIssuer` como `true`.
2. Defina o valor do parâmetro `ValidIssuers` para a lista de organizações permitidas.

#### <a name="option-3-use-a-custom-method-to-validate-issuers"></a>Opção 3: Usar um método personalizado para validar emissores

É possível implementar um método personalizado para validar emissores usando o parâmetro **IssuerValidator**. Para obter mais informações sobre como usar esse parâmetro, leia sobre a [classe TokenValidationParameters](/previous-versions/visualstudio/dn464192(v=vs.114)).

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre aplicativos Web chamando APIs Web:.

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Conheça as etapas para criar o aplicativo usado neste início rápido

> [!div class="nextstepaction"]
> [Aplicativos Web chamando APIs Web]( https://aka.ms/msal-net-authorization-code)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]

Ajude-nos a melhorar a plataforma de identidade da Microsoft. Deixe sua opinião respondendo a uma breve pesquisa de duas perguntas.

> [!div class="nextstepaction"]
> [Pesquisa da plataforma de identidade da Microsoft](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)