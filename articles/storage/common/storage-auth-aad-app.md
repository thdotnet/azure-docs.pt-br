---
title: Autenticar com o Azure Active Directory para acessar dados de blob e fila do aplicativo cliente
description: Use o Azure Active Directory para autenticar a partir de um aplicativo cliente, adquirir um token OAuth 2.0 e autorizar solicitações para o armazenamento de BLOBs do Azure e o armazenamento de filas.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/05/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: e45fe20e93d81c1cfd1f868b40f76743558758bb
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754985"
---
# <a name="authenticate-with-azure-active-directory-from-an-application-for-access-to-blobs-and-queues"></a>Autenticar com o Azure Active Directory de um aplicativo para acesso a blobs e filas

Uma vantagem importante do uso do Azure Active Directory (Azure AD) com o armazenamento de BLOBs do Azure ou armazenamento de filas é que suas credenciais não precisam ser armazenados em seu código. Em vez disso, você pode solicitar um token de acesso OAuth 2.0 de plataforma de identidade da Microsoft (anteriormente conhecido como Azure AD). O Azure AD autentica a entidade de segurança (um usuário, grupo ou entidade de serviço) que executa o aplicativo. Se a autenticação for bem-sucedida, Azure AD retorna o token de acesso para o aplicativo e o aplicativo pode usar o token de acesso para autorizar solicitações para o armazenamento de BLOBs do Azure ou armazenamento de filas.

Este artigo mostra como configurar seu aplicativo nativo ou aplicativo web para autenticação com o Azure AD. O exemplo de código apresenta o .NET, mas outras linguagens usam uma abordagem semelhante.

Para obter uma visão geral do fluxo de concessão do código do OAuth 2.0, consulte [Autorizar acesso a aplicativos Web do Azure Active Directory usando o fluxo de concessão do código do OAuth 2.0](../../active-directory/develop/v2-oauth2-auth-code-flow.md).

## <a name="assign-an-rbac-role-to-an-azure-ad-security-principal"></a>Atribuir uma função RBAC a uma entidade de segurança do Azure AD

Para autenticar uma entidade de segurança do seu aplicativo de armazenamento do Azure, primeiro configure as configurações de RBAC (controle) de acesso baseado em função para essa entidade de segurança. O armazenamento do Azure define as funções RBAC internas que abrangem as permissões para contêineres e filas. Quando a função RBAC é atribuída a uma entidade de segurança, essa entidade de segurança recebe acesso para esse recurso. Para obter mais informações, consulte [gerenciar direitos de acesso a dados do Azure Blob e fila com RBAC](storage-auth-aad-rbac.md).

## <a name="register-your-application-with-an-azure-ad-tenant"></a>Registre o aplicativo com um locatário do Azure AD

A primeira etapa no uso do Azure AD para autorizar o acesso aos recursos de armazenamento está registrando seu aplicativo cliente com um locatário do AD do Azure da [portal do Azure](https://portal.azure.com). Quando você registra seu aplicativo cliente, você pode fornecer informações sobre o aplicativo para o Azure AD. O Azure AD, em seguida, fornece uma ID do cliente (também chamado de *ID do aplicativo*) que você usa para associar o aplicativo ao Azure AD no tempo de execução. Para saber mais sobre a ID do cliente, consulte [Objetos de entidade de serviço e aplicativo no Azure Active Directory](../../active-directory/develop/app-objects-and-service-principals.md).

Para registrar seu aplicativo de armazenamento do Azure, siga as etapas mostradas no [guia de início rápido: Registrar um aplicativo com a plataforma de identidade Microsoft](../../active-directory/develop/quickstart-configure-app-access-web-apis.md). A imagem a seguir mostra as configurações comuns para registrar um aplicativo web:

![Captura de tela mostrando como registrar seu aplicativo de armazenamento com o Azure AD](./media/storage-auth-aad-app/app-registration.png)

> [!NOTE]
> Se você registrar o aplicativo como um aplicativo nativo, poderá especificar qualquer URI válido para **URI de Redirecionamento**. Para aplicativos nativos, esse valor não precisa ser uma URL real. Para aplicativos web, o URI de redirecionamento deve ser um URI válido, pois ele especifica a URL para o qual os tokens são fornecidos.

Depois de registrar o aplicativo, você verá a ID do aplicativo (ou ID do cliente) em **Configurações** :

![Captura de tela mostrando a ID do cliente](./media/storage-auth-aad-app/app-registration-client-id.png)

Para obter mais informações sobre como registrar um aplicativo no Azure AD, consulte [Integrando aplicativos com o Azure Active Directory](../../active-directory/develop/quickstart-v2-register-an-app.md).

## <a name="grant-your-registered-app-permissions-to-azure-storage"></a>Conceder as permissões de aplicativo registradas ao Armazenamento do Microsoft Azure

Em seguida, conceda as permissões do aplicativo para chamar as APIs de armazenamento do Azure. Esta etapa permite que seu aplicativo autorizar solicitações para o armazenamento do Azure com o Azure AD.

1. Sobre o **visão geral** página do seu aplicativo registrado, selecione **exibir permissões de API**.
1. No **permissões de API** seção, selecione **adicionar uma permissão** e escolha **APIs minha organização usa**.
1. Sob o **APIs minha organização utiliza** seção, pesquise por "Armazenamento do Azure" e selecione **armazenamento do Azure** na lista de resultados para exibir o **permissões de API de solicitação** painel.

    ![Captura de tela mostrando permissões para o armazenamento](media/storage-auth-aad-app/registered-app-permissions-1.png)

1. Sob **que tipo de permissões de seu aplicativo exigir?** , observe que é o tipo de permissão disponíveis **permissões delegadas**. Por padrão, essa opção é selecionada para você.
1. No **selecionar permissões** seção o **permissões de API de solicitação** painel, marque a caixa de seleção ao lado **user_impersonation**, em seguida, clique em **adicionar permissões**.
1. O **permissões de API** painel agora mostra que o aplicativo Azure AD tem acesso ao Microsoft Graph e o armazenamento do Azure. Permissões são concedidas automaticamente para o Microsoft Graph quando você primeiro registra seu aplicativo com o Azure AD.

    ![Captura de tela mostrando registrar as permissões de aplicativo](media/storage-auth-aad-app/registered-app-permissions-2.png)

## <a name="libraries-for-token-acquisition"></a>Bibliotecas para aquisição de token

Depois que você tiver registrado seu aplicativo e concedeu permissões para acessar dados no armazenamento de BLOBs do Azure ou armazenamento de filas, você pode adicionar código ao seu aplicativo para autenticar uma entidade de segurança e adquirir um token OAuth 2.0. Para autenticar e adquirir o token, você pode usar qualquer uma da [bibliotecas de autenticação do Microsoft identity platform](../../active-directory/develop/reference-v2-libraries.md) ou em outra biblioteca de código-fonte aberto que dá suporte a OpenID Connect 1.0. Seu aplicativo, em seguida, pode usar o token de acesso para autorizar uma solicitação no armazenamento de BLOBs do Azure ou armazenamento de filas.

Para obter uma lista de cenários para o qual a adquirir tokens de suporte, consulte o [cenários](https://aka.ms/msal-net-scenarios) seção o [biblioteca de autenticação da Microsoft (MSAL) para .NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) repositório do GitHub.

## <a name="net-code-example-create-a-block-blob"></a>Exemplo de código do .NET: criar um blob de blocos

O exemplo de código mostra como obter um token de acesso do Azure AD. O token de acesso é usado para autenticar o usuário especificado e, em seguida, autoriza uma solicitação para criar um blob de blocos. Para que esse exemplo funcione, primeiro siga as etapas descritas nas seções anteriores.

> [!NOTE]
> Como proprietário da conta do Armazenamento do Microsoft Azure, você não recebe permissões automaticamente para acessar dados. Você deve atribuir explicitamente a si mesmo uma função RBAC para o Armazenamento do Microsoft Azure. Você pode atribuí-la no nível de assinatura, grupo de recursos, conta de armazenamento ou contêiner ou fila.
>
> Por exemplo, para executar o código de exemplo em uma conta de armazenamento em que você é um proprietário e sob sua própria identidade de usuário, você deverá atribuir a função RBAC para Colaborador de Dados do Blob a si mesmo. Caso contrário, a chamada para criar o blob falhará com o código de status HTTP 403 (Proibido). Para obter mais informações, consulte [gerenciar direitos de acesso aos dados do armazenamento com RBAC](storage-auth-aad-rbac.md).

### <a name="well-known-values-for-authentication-with-azure-ad"></a>Valores conhecidos para autenticação com Azure AD

Para autenticar uma entidade de segurança com Azure AD, você precisa incluir alguns valores conhecidos no código.

#### <a name="azure-ad-authority"></a>Autoridade do Azure AD

Para a nuvem pública da Microsoft, a autoridade do Azure AD básica é a seguinte, em que *tenant-id* é a ID do locatário do Active Directory (ou ID de diretório):

`https://login.microsoftonline.com/<tenant-id>/`

A ID do locatário identifica o locatário do Azure AD a ser usado para autenticação. Para recuperar a ID de locatário, siga as etapas descritas na seção intitulada **obter a ID de locatário para o Azure Active Directory**.

#### <a name="storage-resource-id"></a>ID do recurso de armazenamento

Use a ID de recurso de armazenamento do Azure para adquirir um token de autorização de solicitações no armazenamento do Azure:

`https://storage.azure.com/`

### <a name="get-the-tenant-id-for-your-azure-active-directory"></a>Obter a ID do locatário para o Azure Active Directory

Para obter a ID de locatário, siga estas etapas:

1. No portal do Azure, selecione seu Active Directory.
2. Clique em **Propriedades**.
3. Copie o valor do GUID fornecido para o **ID de Diretório**. Esse valor também é chamado de ID do locatário.

![Captura de tela mostrando como copiar a ID do locatário](./media/storage-auth-aad-app/aad-tenant-id.png)

## <a name="set-up-a-basic-web-app-to-authenticate-to-azure-ad"></a>Configurar um aplicativo web básico para autenticar para o Azure AD

Quando o aplicativo acessa o armazenamento do Azure, ele assim por diante nome do usuário. Para testar este exemplo de código, você precisa de uma web aplicativo que solicita que o usuário pode entrar usando uma identidade do AD do Azure. Você pode baixá-lo [exemplo de código](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2) para testar um aplicativo web básico que autentica com sua conta do AD do Azure.

### <a name="completed-sample"></a>Exemplo concluído

Uma versão de trabalho completa do código de exemplo mostrado neste artigo pode ser baixada do [GitHub](http://aka.ms/aadstorage). Examinar e executar o exemplo completo podem ser útil para entender os exemplos de código.

### <a name="add-references-and-using-statements"></a>Adicionar referências e usar instruções  

No Visual Studio, instale a biblioteca de cliente de armazenamento do Azure. No menu **Ferramentas**, selecione **Gerenciador de Pacotes do NuGet** e, em seguida, **Console do Gerenciador de Pacotes**. Digite os comandos a seguir na janela do console para instalar os pacotes necessários da biblioteca de cliente de armazenamento do Azure para .NET:

```console
Install-Package Microsoft.Azure.Storage.Blob
Install-Package Microsoft.Azure.Storage.Common
```

Em seguida, adicione as seguintes instruções using ao arquivo HomeController.cs:

```csharp
using System;
using Microsoft.Identity.Client; //MSAL library for getting the access token
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
```

### <a name="create-a-block-blob"></a>criar um blob de blocos

Adicione o seguinte trecho de código para criar um blob de blocos:

```csharp
private static async Task<string> CreateBlob(string accessToken)
{
    // Create a blob on behalf of the user
    TokenCredential tokenCredential = new TokenCredential(accessToken);
    StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

    // Replace the URL below with your storage account URL
    CloudBlockBlob blob =
        new CloudBlockBlob(
            new Uri("https://<storage-account>.blob.core.windows.net/sample-container/Blob1.txt"),
            storageCredentials);
    await blob.UploadTextAsync("Blob created by Azure AD authenticated user.");
    return "Blob successfully created";
}
```

> [!NOTE]
> Para autorizar operações de blob e fila com um token OAuth 2.0, você deve usar HTTPS.

No exemplo acima, a biblioteca de cliente .NET lida com a autorização da solicitação para criar o blob de blocos. Bibliotecas de cliente de armazenamento do Azure para outros idiomas também lidar com a autorização da solicitação para você. No entanto, se você estiver chamando uma operação de armazenamento do Azure com um token de OAuth, usando a API REST, você precisará autorizar a solicitação usando o token OAuth.

Para chamar as operações de serviço Blob e Queue usando tokens de acesso OAuth, passe o token de acesso no cabeçalho **Authorization** usando o esquema **Bearer** e especifique uma versão de serviço de 2017-11-09 ou superior, conforme mostrado no exemplo a seguir:

```https
GET /container/file.txt HTTP/1.1
Host: mystorageaccount.blob.core.windows.net
x-ms-version: 2017-11-09
Authorization: Bearer eyJ0eXAiOnJKV1...Xd6j
```

### <a name="get-an-oauth-token-from-azure-ad"></a>Obter um token OAuth do Azure AD

Em seguida, adicione um método que solicite um token do Azure AD. O token que você solicitar será em nome do usuário, e usaremos o método GetTokenOnBehalfOfUser.

Para solicitar o token, os seguintes valores no registro do seu aplicativo, será necessário

- ID de locatário (ou diretório)
- ID do cliente (ou aplicativo)
- URI de redirecionamento de cliente

Lembre-se de que se você tiver conectado apenas e você está solicitando um token para o `storage.azure.com` recurso, será preciso apresentar ao usuário uma interface do usuário em que o usuário pode concordar com essa ação em seu nome. Para facilitar a que você precisa capturar o `MsalUiRequiredException` e adicionar a funcionalidade para solicitar o consentimento do usuário, conforme mostrado no exemplo a seguir:

```csharp
public async Task<IActionResult> Blob()
{
    var scopes = new string[] { "https://storage.azure.com/user_impersonation" };
    try
    {
        var accessToken =
            await _tokenAcquisition.GetAccessTokenOnBehalfOfUser(HttpContext, scopes);
        ViewData["Message"] = await CreateBlob(accessToken);
        return View();
    }
    catch (MsalUiRequiredException ex)
    {
        AuthenticationProperties properties = BuildAuthenticationPropertiesForIncrementalConsent(scopes, ex);
        return Challenge(properties);
    }
}
```

Consentimento é o processo de um usuário conceder autorização a um aplicativo para acessar recursos protegidos em seu nome. A plataforma de identidade da Microsoft 2.0 dá suporte a consentimento incremental, o que significa que uma entidade de segurança pode solicitar um conjunto mínimo de permissões inicialmente e adicionar permissões ao longo do tempo conforme necessário. Quando seu código solicita um token de acesso, especifique o escopo das permissões que seu aplicativo precisa em qualquer momento determinado no `scope` parâmetro. O método a seguir constrói as propriedades de autenticação para solicitar o consentimento incremental:

```csharp
private AuthenticationProperties BuildAuthenticationPropertiesForIncrementalConsent(string[] scopes, MsalUiRequiredException ex)
{
    AuthenticationProperties properties = new AuthenticationProperties();

    // Set the scopes, including the scopes that ADAL.NET / MSAL.NET need for the Token cache.
    string[] additionalBuildInScopes = new string[] { "openid", "offline_access", "profile" };
    properties.SetParameter<ICollection<string>>(OpenIdConnectParameterNames.Scope, scopes.Union(additionalBuildInScopes).ToList());

    // Attempt to set the login_hint so that the logged-in user is not presented with an account selection dialog.
    string loginHint = HttpContext.User.GetLoginHint();
    if (!string.IsNullOrWhiteSpace(loginHint))
    {
        properties.SetParameter<string>(OpenIdConnectParameterNames.LoginHint, loginHint);

        string domainHint = HttpContext.User.GetDomainHint();
        properties.SetParameter<string>(OpenIdConnectParameterNames.DomainHint, domainHint);
    }

    // Specify any additional claims that are required (for instance, MFA).
    if (!string.IsNullOrEmpty(ex.Claims))
    {
        properties.Items.Add("claims", ex.Claims);
    }

    return properties;
}
```

## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre a plataforma de identidade da Microsoft, consulte [plataforma de identidade Microsoft](https://docs.microsoft.com/azure/active-directory/develop/).
- Para saber mais sobre as funções RBAC para o armazenamento do Azure, consulte [gerenciar direitos de acesso aos dados do armazenamento com RBAC](storage-auth-aad-rbac.md).
- Para saber mais sobre como usar identidades gerenciadas de recursos do Azure com o armazenamento do Azure, consulte [autenticar o acesso a blobs e filas com o Azure Active Directory e identidades gerenciadas para recursos do Azure](storage-auth-aad-msi.md).
