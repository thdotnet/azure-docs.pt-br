---
title: Autenticação do Azure Active Directory com base em certificado com o Azure Cosmos DB
description: Saiba como configurar uma identidade do Azure AD para autenticação baseada em certificado para chaves de acesso do Azure Cosmos DB.
author: voellm
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: tvoellm
ms.reviewer: sngun
ms.openlocfilehash: eb8c98df0f015244adf06a9b57f2223509f1f081
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082957"
---
# <a name="certificate-based-authentication-for-an-azure-ad-identity-to-access-keys-from-an-azure-cosmos-account"></a>Autenticação baseada em certificado para uma identidade do AD do Azure para chaves de acesso de uma conta do Azure Cosmos

Autenticação baseada em certificado permite que seu aplicativo cliente seja autenticado usando o Azure Active Directory (Azure AD) com um certificado de cliente. Você pode executar a autenticação baseada em certificado em um computador em que você precisa de uma identidade, como um computador local ou máquina virtual no Azure. Seu aplicativo pode ler as chaves do Azure Cosmos DB sem a necessidade das chaves diretamente no aplicativo. Este artigo descreve como criar um aplicativo de exemplo do Azure AD, configurá-lo para autenticação baseada em certificado, entrar no Azure usando a nova identidade do aplicativo e, em seguida, ele recupera as chaves de sua conta do Cosmos do Azure. Este artigo usa o Azure PowerShell para configurar as identidades e fornece um C# aplicativo de exemplo que autentica e acessa chaves de sua conta do Cosmos do Azure.  

## <a name="prerequisites"></a>Pré-requisitos

* Instalar o [versão mais recente](/powershell/azure/install-az-ps) do PowerShell do Azure.

* Se você não tiver uma [assinatura do Azure](https://docs.microsoft.com/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing), crie uma [conta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de começar.

## <a name="register-an-app-in-azure-ad"></a>Registrar um aplicativo no Azure AD

Nesta etapa, você registrará o aplicativo web de exemplo em sua conta do Azure AD. Este aplicativo mais tarde é usado para ler as chaves da sua conta do Cosmos do Azure. Use as etapas a seguir para registrar um aplicativo: 

1. Faça logon no [Portal do Azure](https://portal.azure.com/).

1. Abra o Azure **do Active Directory** painel, vá para painel de registros de aplicativo e selecione **novo registro**. 

   ![Novo registro de aplicativo no Active Directory](./media/certificate-based-authentication/new-app-registration.png)

1. Preencher a **registrar um aplicativo** formulário com os seguintes detalhes:  

   * **Nome** – forneça um nome para seu aplicativo, ele pode ser qualquer nome como "sampleApp".
   * **Suporte para tipos de conta** – escolha **contas na organização somente o diretório (diretório padrão)** para permitir que os recursos em seu diretório atual para acessar este aplicativo. 
   * **URL de redirecionamento** – escolha o aplicativo do tipo **Web** e forneça uma URL em que seu aplicativo está hospedado, ele pode ser qualquer URL. Neste exemplo, você pode fornecer uma URL de teste, como `https://sampleApp.com` é okey mesmo se o aplicativo não existe.

   ![Registrar um aplicativo web de exemplo](./media/certificate-based-authentication/register-sample-web-app.png)

1. Selecione **registrar** depois de preencher o formulário.

1. Depois que o aplicativo é registrado, anote o **Application(client) ID** e **ID de objeto**, você usará esses detalhes nas próximas etapas. 

   ![Obter as IDs de aplicativo e objeto](./media/certificate-based-authentication/get-app-object-ids.png)

## <a name="install-the-azuread-module"></a>Instalar o módulo do Azure AD

Nesta etapa, você instalará o módulo do PowerShell do Azure AD. Esse módulo é necessária para obter a ID do aplicativo que você registrou na etapa anterior e associar um certificado autoassinado para o aplicativo. 

1. Abra o ISE do Windows PowerShell com direitos de administrador. Se você ainda não o fez, instale o módulo PowerShell do AZ e conecte-se à sua assinatura. Se você tiver várias assinaturas, você pode definir o contexto da assinatura atual, conforme mostrado nos comandos a seguir:

   ```powershell

   Install-Module -Name Az -AllowClobber
   Connect-AzAccount

   Get-AzSubscription
   $context = Get-AzSubscription -SubscriptionId <Your_Subscription_ID>
   Set-AzContext $context 
   ```

1. Instalar e importar os [AzureAD](/powershell/module/azuread/?view=azureadps-2.0) módulo

   ```powershell
   Install-Module AzureAD
   Import-Module AzureAD 
   ```

## <a name="sign-into-your-azure-ad"></a>Logon no AD do Azure

Entrar no AD do Azure em que você registrou o aplicativo. Use o comando Connect-AzureAD para entrar na sua conta, insira suas credenciais de conta do Azure na janela pop-up. 

```powershell
Connect-AzureAD 
```

## <a name="create-a-self-signed-certificate"></a>Crie um certificado autoassinado

Abra outra instância do ISE do Windows PowerShell e execute os seguintes comandos para criar um certificado autoassinado e ler a chave associada ao certificado:

```powershell
$cert = New-SelfSignedCertificate -CertStoreLocation "Cert:\CurrentUser\My" -Subject "CN=sampleAppCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData()) 
```

## <a name="create-the-certificate-based-credential"></a>Criar a credencial baseada em certificado 

Em seguida, execute os comandos a seguir para obter a ID de objeto do seu aplicativo e criar a credencial baseada em certificado. Neste exemplo, definimos o certificado expire após um ano, você pode defini-lo para qualquer data de término necessária.

```powershell
$application = Get-AzureADApplication -ObjectId <Object_ID_of_Your_Application>

New-AzureADApplicationKeyCredential -ObjectId $application.ObjectId -CustomKeyIdentifier "Key1" -Type AsymmetricX509Cert -Usage Verify -Value $keyValue -EndDate "2020-01-01"
```

O comando acima resulta em uma saída semelhante à captura de tela abaixo:

![Saída de criação de credenciais baseadas em certificado](./media/certificate-based-authentication/certificate-based-credential-output.png)

## <a name="configure-your-azure-cosmos-account-to-use-the-new-identity"></a>Configurar sua conta do Cosmos do Azure para usar a nova identidade

1. Faça logon no [Portal do Azure](https://portal.azure.com/).

1. Navegue até sua conta do Cosmos do Azure, abra o **controle de acesso (IAM)** folha.

1. Selecione **Add** e **Adicionar atribuição de função**. Adicionar o sampleApp que você criou na etapa anterior com **Colaborador** função, conforme mostrado na seguinte captura de tela:

   ![Configurar conta de Cosmos do Azure para usar a nova identidade](./media/certificate-based-authentication/configure-cosmos-account-with-identify.png)

1. Selecione **salvar** depois de preencher o formulário


## <a name="access-the-keys-from-powershell"></a>As chaves de acesso do PowerShell

Nesta etapa, você irá entrar no Azure usando o aplicativo e o certificado que você criou e acessar chaves da sua conta do Azure Cosmos. 

1. Inicialmente, desmarque as credenciais da conta do Azure que você usou para entrar em sua conta. Você pode limpar as credenciais usando o seguinte comando:

   ```powershell
   Disconnect-AzAccount -Username <Your_Azure_account_email_id> 
   ```

1. Em seguida, valide que você pode entrar no portal do Azure usando as credenciais do aplicativo e acessar chaves do Azure Cosmos DB:

   ```powershell
   Login-AzAccount -ApplicationId <Your_Application_ID> -CertificateThumbprint $cert.Thumbprint -ServicePrincipal -Tenant <Tenant_ID_of_your_application>

   Invoke-AzResourceAction -Action listKeys -ResourceType "Microsoft.DocumentDB/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName <Resource_Group_Name_of_your_Azure_Cosmos_account> -ResourceName <Your_Azure_Cosmos_Account_Name> 
   ```

O comando anterior irá exibir as chaves mestras primárias e secundárias da sua conta do Cosmos do Azure. Você pode exibir o log de atividades da sua conta do Cosmos do Azure para validar que a solicitação de chaves get foi bem-sucedida e o evento é iniciado pelo aplicativo de "sampleApp". 
 
![Validar a chamada get chaves no Azure AD](./media/certificate-based-authentication/activity-log-validate-results.png)


## <a name="access-the-keys-from-a-c-application"></a>Acessar as chaves de um C# aplicativo 

Também é possível validar esse cenário, acessando chaves de um C# aplicativo. O seguinte C# aplicativo do console, que pode acessar as chaves do Azure Cosmos DB usando o aplicativo registrado no Active Directory. Certifique-se de atualizar a tenantId, o clientID, o certName, o nome do grupo de recursos, ID de assinatura, os detalhes de nome da conta do Azure Cosmos antes de executar o código. 

```csharp
using System;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System.Linq;
using System.Net.Http;
using System.Security.Cryptography.X509Certificates;
using System.Threading;
using System.Threading.Tasks;
 
namespace TodoListDaemonWithCert
{
    class Program
    {
        private static string aadInstance = "https://login.windows.net/";
        private static string tenantId = "<Your_Tenant_ID>";
        private static string clientId = "<Your_Client_ID>";
        private static string certName = "<Your_Certificate_Name>";
 
        private static int errorCode = 0;
        static int Main(string[] args)
        {
            MainAync().Wait();
            Console.ReadKey();
 
            return 0;
        } 
 
        static async Task MainAync()
        {
            string authContextURL = aadInstance + tenantId;
            AuthenticationContext authContext = new AuthenticationContext(authContextURL);
            X509Certificate2 cert = ReadCertificateFromStore(certName);
 
            ClientAssertionCertificate credential = new ClientAssertionCertificate(clientId, cert);
            AuthenticationResult result = await authContext.AcquireTokenAsync("https://management.azure.com/", credential);
            if (result == null)
            {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }
 
            string token = result.AccessToken;
            string subscriptionId = "<Your_Subscription_ID>";
            string rgName = "<ResourceGroup_of_your_Cosmos_account>";
            string accountName = "<Your_Cosmos_account_name>";
            string cosmosDBRestCall = $"https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{rgName}/providers/Microsoft.DocumentDB/databaseAccounts/{accountName}/listKeys?api-version=2015-04-08";
 
            Uri restCall = new Uri(cosmosDBRestCall);
            HttpClient httpClient = new HttpClient();
            httpClient.DefaultRequestHeaders.Remove("Authorization");
            httpClient.DefaultRequestHeaders.Add("Authorization", "Bearer " + token);
            HttpResponseMessage response = await httpClient.PostAsync(restCall, null);
 
            Console.WriteLine("Got result {0} and keys {1}", response.StatusCode.ToString(), response.Content.ReadAsStringAsync().Result);
        }
 
 
        /// <summary>
        /// Reads the certificate
        /// </summary>
        private static X509Certificate2 ReadCertificateFromStore(string certName)
        {
            X509Certificate2 cert = null;
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            store.Open(OpenFlags.ReadOnly);
            X509Certificate2Collection certCollection = store.Certificates;
 
            // Find unexpired certificates.
            X509Certificate2Collection currentCerts = certCollection.Find(X509FindType.FindByTimeValid, DateTime.Now, false);
 
            // From the collection of unexpired certificates, find the ones with the correct name.
            X509Certificate2Collection signingCert = currentCerts.Find(X509FindType.FindBySubjectName, certName, false);
 
            // Return the first certificate in the collection, has the right name and is current.
            cert = signingCert.OfType<X509Certificate2>().OrderByDescending(c => c.NotBefore).FirstOrDefault();
            store.Close();
            return cert;
        }
 
 
        /// <summary>
        /// Get an access token from Azure AD using client credentials.
        /// If the attempt to get a token fails because the server is unavailable, retry twice after 3 seconds each
        /// </summary>
        private static async Task<AuthenticationResult> GetAccessToken(AuthenticationContext authContext, string resourceUri, ClientAssertionCertificate cert)
        {
            //
            // Get an access token from Azure AD using client credentials.
            // If the attempt to get a token fails because the server is unavailable, retry twice after 3 seconds each.
            //
            AuthenticationResult result = null;
            int retryCount = 0;
            bool retry = false;
 
            do
            {
                retry = false;
                errorCode = 0;
 
                try
                {
                    result = await authContext.AcquireTokenAsync(resourceUri, cert);
                }
                catch (AdalException ex)
                {
                    if (ex.ErrorCode == "temporarily_unavailable")
                    {
                        retry = true;
                        retryCount++;
                        Thread.Sleep(3000);
                    }
 
                    Console.WriteLine(
                        String.Format("An error occurred while acquiring a token\nTime: {0}\nError: {1}\nRetry: {2}\n",
                        DateTime.Now.ToString(),
                        ex.ToString(),
                        retry.ToString()));
 
                    errorCode = -1;
                }
 
            } while ((retry == true) && (retryCount < 3));
            return result;
        }
    }
}
```

Esse script gera as chaves mestras primárias e secundárias, conforme mostrado na seguinte captura de tela:

![saída do aplicativo CSharp](./media/certificate-based-authentication/csharp-application-output.png)

Semelhante à seção anterior, você pode exibir o log de atividades da sua conta do Cosmos do Azure para validar que o evento de solicitação de chaves get é iniciado pelo aplicativo "sampleApp". 


## <a name="next-steps"></a>Próximas etapas

* [Chaves do Azure Cosmos seguras usando o Azure Key Vault](access-secrets-from-keyvault.md)

* [Atributos de segurança para o Azure Cosmos DB](cosmos-db-security-attributes.md)