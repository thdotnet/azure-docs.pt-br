---
title: Autenticação serviço a serviço no Azure Key Vault usando o .NET
description: Use a biblioteca Microsoft.Azure.Services.AppAuthentication para se autenticar no Azure Key Vault usando o .NET.
keywords: credenciais de local de autenticação do Azure Key Vault
author: msmbaldwin
manager: barbkess
services: key-vault
ms.author: mbaldwin
ms.date: 07/06/2019
ms.topic: conceptual
ms.service: key-vault
ms.openlocfilehash: d34c94ccca47d29afc4f3d83bec58db737be270c
ms.sourcegitcommit: bc3a153d79b7e398581d3bcfadbb7403551aa536
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68840419"
---
# <a name="service-to-service-authentication-to-azure-key-vault-using-net"></a>Autenticação serviço a serviço no Azure Key Vault usando o .NET

Para autenticar no Azure Key Vault você precisa de uma credencial do Azure Active Directory (AD), seja um segredo compartilhado ou um certificado. 

O gerenciamento de credenciais como essas pode ser difícil e é tentador agrupar as credenciais em um aplicativo incluindo-os nos arquivos de configuração ou de origem.  O `Microsoft.Azure.Services.AppAuthentication` para a biblioteca .NET simplifica esse problema. Ele usa as credenciais do desenvolvedor para autenticação durante o desenvolvimento local. Quando a solução é implantada posteriormente no Azure, a biblioteca alterna automaticamente para as credenciais do aplicativo.    O uso das credenciais do desenvolvedor durante o desenvolvimento local é mais seguro, porque você não precisa criar credenciais do Azure AD nem compartilhar credenciais entre desenvolvedores.

A biblioteca `Microsoft.Azure.Services.AppAuthentication` gerencia a autenticação automaticamente, que, por sua vez, permite que você se concentre em sua solução, em vez de nas credenciais.  Ele dá suporte ao desenvolvimento local com Microsoft Visual Studio, CLI do Azure ou autenticação integrada do Azure AD. Quando implantada em um recurso do Azure que oferece suporte a uma identidade gerenciada, a biblioteca usa automaticamente as [identidades gerenciadas para recursos do Azure](../active-directory/msi-overview.md). Nenhuma alteração de configuração ou de código é necessária. A biblioteca também dá suporte ao uso direto das [credenciais do cliente](../azure-resource-manager/resource-group-authenticate-service-principal.md) do Azure AD quando uma identidade gerenciada não está disponível ou quando o contexto de segurança do desenvolvedor não pode ser determinado durante o desenvolvimento local.

## <a name="using-the-library"></a>Usando a biblioteca

Para aplicativos .NET, a maneira mais simples de trabalhar com uma identidade gerenciada é por meio do pacote `Microsoft.Azure.Services.AppAuthentication`. Estas são algumas dicas para começar:

1. Adicione uma referência aos pacotes NuGet [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) e [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) no aplicativo. 

2. Adicione o seguinte código:

    ``` csharp
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Azure.KeyVault;

    // Instantiate a new KeyVaultClient object, with an access token to Key Vault
    var azureServiceTokenProvider1 = new AzureServiceTokenProvider();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider1.KeyVaultTokenCallback));

    // Optional: Request an access token to other Azure services
    var azureServiceTokenProvider2 = new AzureServiceTokenProvider();
    string accessToken = await azureServiceTokenProvider2.GetAccessTokenAsync("https://management.azure.com/").ConfigureAwait(false);
    ```

A classe `AzureServiceTokenProvider` armazena em cache o token na memória e recupera-o do Azure AD logo antes da expiração. Consequentemente, você não precisa mais verificar a expiração antes de chamar o método `GetAccessTokenAsync`. Basta chamar o método quando desejar usar o token. 

O método `GetAccessTokenAsync` exige um identificador de recurso. Para saber mais, veja [quais serviços do Azure dão suporte a identidades gerenciadas para recursos do Azure](../active-directory/msi-overview.md).

## <a name="local-development-authentication"></a>Autenticação de desenvolvimento local

Para o desenvolvimento local, há dois cenários de autenticação principais: [autenticação nos serviços do Azure](#authenticating-to-azure-services)e [autenticação para serviços personalizados](#authenticating-to-custom-services).

### <a name="authenticating-to-azure-services"></a>Autenticação nos serviços do Azure

Máquinas virtuais não dão suporte a identidades gerenciadas para recursos do Azure.  Como resultado, a biblioteca `Microsoft.Azure.Services.AppAuthentication` usa suas credenciais de desenvolvedor para ser executada no ambiente de desenvolvimento local. Quando a solução é implantada no Azure, a biblioteca usa uma identidade gerenciada para mudar para um fluxo de concessão de credenciais do cliente do OAuth 2.0.  Isso significa que você pode testar o mesmo código local e remotamente sem se preocupar.

Para o desenvolvimento local, o `AzureServiceTokenProvider` busca tokens usando o **Visual Studio**, a **CLI (interface de linha de comando) do Azure** ou a **Autenticação Integrada do Azure AD**. Cada opção é tentada na sequência e a biblioteca usa a primeira opção bem-sucedida. Se nenhuma opção funcionar, uma exceção `AzureServiceTokenProviderException` será gerada com informações detalhadas.

### <a name="authenticating-with-visual-studio"></a>Autenticando com o Visual Studio

A autenticação com o Visual Studio tem os seguintes pré-requisitos:

1. [Visual Studio 2017 v 15.5](https://blogs.msdn.microsoft.com/visualstudio/2017/10/11/visual-studio-2017-version-15-5-preview/) ou posterior.

2. A [extensão de autenticação de aplicativo para o Visual Studio](https://go.microsoft.com/fwlink/?linkid=862354), disponível como uma extensão separada para a atualização 5 do visual Studio 2017 e agrupada com o produto na atualização 6 e posterior. Com a atualização 6 ou posterior, você pode verificar a instalação da extensão de autenticação do aplicativo selecionando Ferramentas de desenvolvimento do Azure no instalador do Visual Studio.
 
Entre no Visual Studio e use **ferramentas**&nbsp;&nbsp;>**Opções**&nbsp;autenticaçãode**serviço do Azure** para selecionar uma conta para o desenvolvimento local.>&nbsp; 

Caso tenha problemas com o Visual Studio, como erros referentes ao arquivo do provedor de token, leia estas etapas com atenção. 

Também pode ser necessário autenticar novamente o token de desenvolvedor. Para fazer isso, acesse **Ferramentas**&nbsp;>&nbsp;**Opções**>**Azure&nbsp;Serviço&nbsp;Autenticação** e procure um link **Autenticar novamente** na conta selecionada.  Selecione-o para autenticar. 

### <a name="authenticating-with-azure-cli"></a>Autenticando com a CLI do Azure

Para usar a CLI do Azure para o desenvolvimento local:

1. Instale a [CLI v2.0.12 do Azure](/cli/azure/install-azure-cli) ou posterior. Atualize as versões anteriores. 

2. Use **az login** para entrar no Azure.

Use `az account get-access-token` para verificar o acesso.  Caso receba um erro, verifique se a Etapa 1 foi concluída com êxito. 

Se a CLI do Azure não estiver instalada no diretório padrão, você poderá receber um erro relatando que o `AzureServiceTokenProvider` não pode localizar o caminho para a CLI do Azure.  Use a variável de ambiente **AzureCLIPath** para definir a pasta de instalação do CLI do Azure. `AzureServiceTokenProvider` adiciona o diretório especificado na variável de ambiente **AzureCLIPath** à variável de ambiente **Path**, quando necessário.

Caso esteja conectado à CLI do Azure com várias contas ou caso sua conta tenha acesso a várias assinaturas, você precisará especificar a assinatura específica a ser usada.  Para fazer isso, use:

```
az account set --subscription <subscription-id>
```

Esse comando gera a saída apenas em caso de falha.  Para verificar as configurações de conta atuais, use:

```
az account list
```

### <a name="authenticating-with-azure-ad-authentication"></a>Autenticando com a autenticação do Azure AD

Para usar a autenticação do Azure AD, verifique se:

- O Active Directory local [é sincronizado com o Azure AD](../active-directory/connect/active-directory-aadconnect.md).

- O código está sendo executado em um computador ingressado no domínio.


### <a name="authenticating-to-custom-services"></a>Autenticação em serviços personalizados

Quando um serviço chama os serviços do Azure, as etapas anteriores funcionam porque os serviços do Azure permitem o acesso a usuários e aplicativos.  

Ao criar um serviço que chama um serviço personalizado, use as credenciais do cliente do Azure AD para autenticação de desenvolvimento local.  Há duas opções: 

1.  Use uma entidade de serviço para entrar no Azure:

    1.  [Crie uma entidade de serviço](/cli/azure/create-an-azure-service-principal-azure-cli).

    2.  Use a CLI do Azure para entrar:

        ```azurecli
        az login --service-principal -u <principal-id> --password <password> --tenant <tenant-id> --allow-no-subscriptions
        ```

        Como a entidade de serviço pode não ter acesso a uma assinatura, use o argumento `--allow-no-subscriptions`.

2.  Use variáveis de ambiente para especificar detalhes da entidade de serviço.  Para obter detalhes, consulte [Executando o aplicativo usando uma entidade de serviço](#running-the-application-using-a-service-principal).

Depois de entrar no Azure, o `AzureServiceTokenProvider` usa a entidade de serviço para recuperar um token para o desenvolvimento local.

Isso se aplica apenas ao desenvolvimento local. Quando a solução é implantada no Azure, a biblioteca muda uma identidade gerenciada com propósito de autenticação.

## <a name="running-the-application-using-managed-identity-or-user-assigned-identity"></a>Executando o aplicativo usando identidade gerenciada ou identidade atribuída pelo usuário 

Quando você executa o código em um Serviço de Aplicativo do Azure ou em uma VM do Azure com uma identidade gerenciada habilitada, a biblioteca usa automaticamente a identidade gerenciada. 

Como alternativa, você pode autenticar com uma identidade atribuída pelo usuário. Para obter mais informações sobre identidades atribuídas ao usuário, consulte [sobre identidades gerenciadas para recursos do Azure](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work). Para autenticar com uma identidade atribuída pelo usuário, você precisa especificar a ID do cliente da identidade atribuída pelo usuário na cadeia de conexão. A cadeia de conexão é especificada na seção [suporte à cadeia de conexão](#connection-string-support) abaixo.

## <a name="running-the-application-using-a-service-principal"></a>Executando o aplicativo usando uma Entidade de Serviço 

Pode ser necessário criar uma credencial do Cliente do Azure AD para autenticar. Exemplos comuns incluem:

- O código é executado em um ambiente de desenvolvimento local, mas não na identidade do desenvolvedor.  O Service Fabric, por exemplo, usa a [conta NetworkService](../service-fabric/service-fabric-application-secret-management.md) para o desenvolvimento local.
 
- O código é executado em um ambiente de desenvolvimento local e você se autentica em um serviço personalizado e, portanto, não pode usar sua identidade de desenvolvedor. 
 
- O código é executado em um recurso de computação do Azure que ainda não dá suporte a identidades gerenciadas para recursos do Azure, tais como o Lote do Azure.

Há três métodos principais de usar uma entidade de serviço para executar seu aplicativo. Para usar qualquer um deles, você deve primeiro [criar uma entidade de serviço](/cli/azure/create-an-azure-service-principal-azure-cli).

### <a name="use-a-certificate-in-local-keystore-to-sign-into-azure-ad"></a>Usar um certificado no repositório de chaves local para entrar no Azure AD

1. Crie um certificado de entidade de serviço usando o comando CLI do Azure [AZ ad SP Create-for-RBAC](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) . 

    ```azurecli
    az ad sp create-for-rbac --create-cert
    ```

    Isso criará um arquivo. PEM (chave privada) que será armazenado em seu diretório base. Implante esse certificado no repositório *LocalMachine* ou *CurrentUser* . 

    > [!Important]
    > O comando da CLI gera um arquivo. PEM, mas o Windows só fornece suporte nativo para certificados PFX. Para gerar um certificado PFX, use os comandos do PowerShell mostrados aqui: [Crie uma entidade de serviço com um certificado autoassinado](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate). Esses comandos também implantam o certificado automaticamente.

1. Defina uma variável de ambiente chamada **AzureServicesAuthConnectionString** como:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};
          CertificateStoreLocation={CertificateStore}
    ```
 
    Substitua *{AppId}* , *{TenantId}* e *{Thumbprint}* pelos valores gerados na Etapa 1. Substitua *{CertificateStore}* com um `LocalMachine` ou `CurrentUser`, com base no seu plano de implantação.

1. Execute o aplicativo. 

### <a name="use-a-shared-secret-credential-to-sign-into-azure-ad"></a>Usar uma credencial de segredo compartilhado para entrar no Azure AD

1. Crie um certificado de entidade de serviço com uma senha usando [AZ ad SP Create-for-RBAC--password](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac). 

2. Defina uma variável de ambiente chamada **AzureServicesAuthConnectionString** como:

    ```
    RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret} 
    ```

    Substitua _{AppId}_ , _{TenantId}_ e _{ClientSecret}_ pelos valores gerados na Etapa 1.

3. Execute o aplicativo. 

Depois que tudo estiver configurado corretamente, nenhuma alteração de código adicional será necessária.  O `AzureServiceTokenProvider` usa a variável de ambiente e o certificado para se autenticar no Azure AD. 

### <a name="use-a-certificate-in-key-vault-to-sign-into-azure-ad"></a>Usar um certificado no Key Vault para entrar no Azure AD

Essa opção permite que você armazene o certificado de cliente de uma entidade de serviço em Key Vault e use-o para autenticação de entidade de serviço. Você pode usar isso para os seguintes cenários:

* Autenticação local, onde você deseja autenticar usando uma entidade de serviço explícita e deseja manter as credenciais da entidade de serviço com segurança em um cofre de chaves. A conta de desenvolvedor deve ter acesso ao cofre de chaves. 
* Autenticação do Azure em que você deseja usar credencial explícita (por exemplo, para cenários de locatário cruzado) e deseja manter a credencial da entidade de serviço com segurança em um cofre de chaves. A identidade gerenciada deve ter acesso ao key Vault. 

A identidade gerenciada ou a identidade do desenvolvedor deve ter permissão para recuperar o certificado do cliente do Key Vault. A biblioteca AppAuthentication usa o certificado recuperado como a credencial do cliente da entidade de serviço.

Para usar um certificado de cliente para autenticação de entidade de serviço

1. Crie um certificado de entidade de serviço e armazene-o automaticamente no seu keyvault usando o CLI do Azure [AZ ad SP Create-for-RBAC <keyvaultname> --keyvault--CERT <certificatename> --Create-CERT--ignore-Assignment](/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac) comando:

    ```azurecli
    az ad sp create-for-rbac --keyvault <keyvaultname> --cert <certificatename> --create-cert --skip-assignment
    ```
    
    O identificador de certificado será uma URL no formato`https://<keyvaultname>.vault.azure.net/secrets/<certificatename>`

1. Substituir `{KeyVaultCertificateSecretIdentifier}` nesta cadeia de conexão pelo identificador de certificado:

    ```
    RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier={KeyVaultCertificateSecretIdentifier}
    ```

    Se, por exemplo, o cofre de chaves foi chamado "myKeyVault" e você criou um certificado chamado "mycert", o identificador de certificado seria:

    ```
    RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier=https://myKeyVault.vault.azure.net/secrets/myCert
    ```


## <a name="connection-string-support"></a>Suporte à cadeia de conexão

Por padrão, o `AzureServiceTokenProvider` usa vários métodos para recuperar um token. 

Para controlar o processo, use uma cadeia de conexão passada para o construtor `AzureServiceTokenProvider` ou especificada na variável de ambiente *AzureServicesAuthConnectionString*. 

Há suporte para as seguintes opções:

| Opção de cadeia de conexão | Cenário | Comentários|
|:--------------------------------|:------------------------|:----------------------------|
| `RunAs=Developer; DeveloperTool=AzureCli` | Desenvolvimento local | O AzureServiceTokenProvider usa a AzureCli para obter um token. |
| `RunAs=Developer; DeveloperTool=VisualStudio` | Desenvolvimento local | O AzureServiceTokenProvider usa o Visual Studio para obter um token. |
| `RunAs=CurrentUser` | Desenvolvimento local | O AzureServiceTokenProvider usa a Autenticação Integrada do Azure AD para obter um token. |
| `RunAs=App` | [Identidades gerenciadas para os recursos do Azure](../active-directory/managed-identities-azure-resources/index.yml) | O AzureServiceTokenProvider usa uma identidade gerenciada para obter um token. |
| `RunAs=App;AppId={ClientId of user-assigned identity}` | [Identidade atribuída pelo usuário para recursos do Azure](../active-directory/managed-identities-azure-resources/overview.md#how-does-the-managed-identities-for-azure-resources-work) | O azureservicetokenprovider usa uma identidade atribuída pelo usuário para obter o token. |
| `RunAs=App;AppId={TestAppId};KeyVaultCertificateSecretIdentifier={KeyVaultCertificateSecretIdentifier}` | Autenticação de serviços personalizados | KeyVaultCertificateSecretIdentifier = o identificador secreto do certificado. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateThumbprint={Thumbprint};CertificateStoreLocation={LocalMachine or CurrentUser}`| Entidade de serviço | O `AzureServiceTokenProvider` usa um certificado para obter um token do Azure AD. |
| `RunAs=App;AppId={AppId};TenantId={TenantId};CertificateSubjectName={Subject};CertificateStoreLocation={LocalMachine or CurrentUser}` | Entidade de serviço | O `AzureServiceTokenProvider` usa um certificado para obter um token do Azure AD|
| `RunAs=App;AppId={AppId};TenantId={TenantId};AppKey={ClientSecret}` | Entidade de serviço |O `AzureServiceTokenProvider` usa um segredo para obter um token do Azure AD. |

## <a name="samples"></a>Exemplos

Para ver a `Microsoft.Azure.Services.AppAuthentication` biblioteca em ação, consulte os exemplos de código a seguir.

1. [Usar uma identidade gerenciada para recuperar um segredo do Azure Key Vault em tempo de execução](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet)

2. [Implante de forma programática um modelo do Azure Resource Manager de uma VM do Azure com uma identidade gerenciada](https://github.com/Azure-Samples/windowsvm-msi-arm-dotnet).

3. [Use uma amostra do .NET Core e uma identidade gerenciada para chamar os serviços do Azure de uma VM Linux do Azure](https://github.com/Azure-Samples/linuxvm-msi-keyvault-arm-dotnet/).

## <a name="appauthentication-troubleshooting"></a>Solução de problemas do AppAuthentication

### <a name="common-issues-during-local-development"></a>Problemas comuns durante o desenvolvimento local

#### <a name="azure-cli-is-not-installed-you-are-not-logged-in-or-you-do-not-have-the-latest-version"></a>O CLI do Azure não está instalado, você não está conectado ou não tem a versão mais recente

Execute **AZ Account Get-Access-token** para ver se CLI do Azure mostra um token para você. Se esse programa não for encontrado, instale a [versão mais recente do CLI do Azure](/cli/azure/install-azure-cli?view=azure-cli-latest). Se você o tiver instalado, poderá ser solicitado a fazer logon. 
 
#### <a name="azureservicetokenprovider-cannot-find-the-path-for-azure-cli"></a>O azureservicetokenprovider não pode localizar o caminho para CLI do Azure

O azureservicetokenprovider procura CLI do Azure em seus locais de instalação padrão. Se não for possível localizar CLI do Azure, defina a variável de ambiente **AzureCLIPath** para a pasta de instalação CLI do Azure. O azureservicetokenprovider adicionará a variável de ambiente à variável de ambiente Path.
 
#### <a name="you-are-logged-into-azure-cli-using-multiple-accounts-the-same-account-has-access-to-subscriptions-in-multiple-tenants-or-you-get-an-access-denied-error-when-trying-to-make-calls-during-local-development"></a>Você está conectado em CLI do Azure usando várias contas, a mesma conta tem acesso a assinaturas em vários locatários ou você recebe um erro de acesso negado ao tentar fazer chamadas durante o desenvolvimento local

Usando CLI do Azure, defina a assinatura padrão para uma que tenha a conta que você deseja usar e que esteja no mesmo locatário que o recurso que você deseja acessar: **AZ Account Set--Subscription [Subscription-ID]** . Se nenhuma saída for vista, ela será bem-sucedida. Verifique se a conta certa agora é a padrão usando a **lista de contas AZ**.

### <a name="common-issues-across-environments"></a>Problemas comuns entre ambientes

#### <a name="unauthorized-access-access-denied-forbidden-etc-error"></a>Acesso não autorizado, acesso negado, proibido, etc. erro
 
A entidade de segurança usada não tem acesso ao recurso que está tentando acessar. Conceda à sua conta de usuário ou ao MSI "colaborador" do serviço de aplicativo o acesso ao recurso desejado, dependendo se você estiver executando o exemplo em seu computador de desenvolvimento local ou implantado no Azure em seu serviço de aplicativo. Alguns recursos, como cofres de chaves, também têm suas próprias [políticas de acesso](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-secure-your-key-vault#data-plane-and-access-policies) que você usa para conceder acesso a entidades de segurança (usuários, aplicativos, grupos, etc.).

### <a name="common-issues-when-deployed-to-azure-app-service"></a>Problemas comuns quando implantados no serviço Azure App

#### <a name="managed-identity-is-not-setup-on-the-app-service"></a>A identidade gerenciada não está configurada no serviço de aplicativo
 
Verifique as variáveis de ambiente MSI_ENDPOINT e MSI_SECRET existam usando o [console de depuração do kudu](https://azure.microsoft.com/en-us/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/). Se essas variáveis de ambiente não existirem, a identidade gerenciada não será habilitada no serviço de aplicativo. 
 
### <a name="common-issues-when-deployed-locally-with-iis"></a>Problemas comuns quando implantados localmente com o IIS

#### <a name="cant-retrieve-tokens-when-debugging-app-in-iis"></a>Não é possível recuperar Tokens ao depurar o aplicativo no IIS

Por padrão, o AppAuth é executado em um contexto de usuário diferente no IIS e, portanto, não tem acesso para usar sua identidade de desenvolvedor para recuperar Tokens de acesso. Você pode configurar o IIS para ser executado com o seu contexto de usuário com as duas etapas a seguir:
- Configure o pool de aplicativos para que o aplicativo Web seja executado como sua conta de usuário atual. Confira mais informações [aqui](https://docs.microsoft.com/en-us/iis/manage/configuring-security/application-pool-identities#configuring-iis-application-pool-identities)
- Configure "setProfileEnvironment" como "true". Veja mais informações [aqui](https://docs.microsoft.com/en-us/iis/configuration/system.applicationhost/applicationpools/add/processmodel#configuration). 

    - Vá para%windir%\System32\inetsrv\config\applicationHost.config
    - Pesquise por "setProfileEnvironment". Se estiver definido como "false", altere-o para "true". Se não estiver presente, adicione-o como um atributo ao elemento processModel (/configuration/system.applicationHost/applicationPools/applicationPoolDefaults/processModel/@setProfileEnvironment) e defina-o como "true".

- Aprenda sobre [identidades gerenciadas dos recursos do Azure](../active-directory/managed-identities-azure-resources/index.yml).
- Saiba mais sobre os [cenários de autenticação do Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).
