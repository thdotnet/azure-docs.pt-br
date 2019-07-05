---
title: Configurar a federação direta com um provedor de identidade para B2B – Azure Active Directory | Microsoft Docs
description: Federar diretamente com um provedor de identidade SAML ou WS-Fed, para que convidados possam se conectar aos seus aplicativos do Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: a4dadc68e78fbaa979751d5bcd04ef481c3ab886
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67544342"
---
# <a name="direct-federation-with-ad-fs-and-third-party-providers-for-guest-users-preview"></a>Federação direta com AD FS e provedores de terceiros para usuários convidados (visualização)
|     |
| --- |
| Federação direta é um recurso de visualização pública do Azure Active Directory. Para saber mais sobre versões prévias, consulte os [Termos de Uso Complementares para Visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Este artigo descreve como configurar a federação direta com outra organização para colaboração B2B. Você pode configurar a federação direta com qualquer empresa cujo provedor de identidade (IdP) suporta o protocolo SAML 2.0 ou WS-Fed.
Quando você configurar a federação direta com IdP um parceiro, novos usuários convidados do domínio podem usar sua própria conta organizacional gerenciados pelo IdP para entrar no seu locatário do AD do Azure e começar a colaborar com você. Não é necessário para o usuário convidado crie uma conta do Azure AD.
> [!NOTE]
> Direcionar os usuários convidados de Federação devem entrar usando um link que inclui o contexto do locatário (por exemplo, `https://myapps.microsoft.com/?tenantid=<tenant id>` ou `https://portal.azure.com/<tenant id>`, ou, no caso de um domínio verificado, `https://myapps.microsoft.com/\<verified domain>.onmicrosoft.com`). Links diretos para aplicativos e recursos também funcionam desde que incluam o contexto do locatário. Os usuários de Federação direta não conseguem entrar usando pontos de extremidade comuns que não têm nenhum contexto de locatário no momento. Por exemplo, usando `https://myapps.microsoft.com`, `https://portal.azure.com`, ou `https://teams.microsoft.com` resultará em erro.
 
## <a name="when-is-a-guest-user-authenticated-with-direct-federation"></a>Quando um usuário convidado é autenticado com federação direta?
Depois de configurar a federação direta com uma organização, os novos usuários convidados que você convidar serão autenticados usando a federação direta. É importante observar que configurar a federação direta não altera o método de autenticação para usuários convidados que já resgataram o convite de você. Estes são alguns exemplos:
 - Se os usuários convidados já resgatou convites de e você subsequentemente configurar a federação direta com a sua organização, esses usuários convidados continuará a usar o mesmo método de autenticação usados antes de você configurar a federação direta.
 - Se você configurar a federação direta com uma organização parceira e convida usuários convidados, e, em seguida, a organização do parceiro se move para o Azure AD, os usuários convidados que já resgataram convites continuarão a usar a federação direta, contanto que o direct política de Federação em seu locatário existe.
 - Se você excluir a federação direta com uma organização parceira, quaisquer usuários convidados no momento usando a federação direta será possível entrar.

Em qualquer um desses cenários, você pode atualizar o método de autenticação de um usuário convidado ao excluir a conta de usuário convidado do seu diretório e reinviting-los.

Federação direta está vinculada ao domínio namespaces, como contoso.com e fabrikam.com. Durante o estabelecimento de uma configuração de Federação direta com AD FS ou um IdP de terceiros, as organizações associar um ou mais namespaces de domínio desses IdPs. 

## <a name="end-user-experience"></a>Experiência do usuário final 
Com a federação direta, os usuários convidados entram no locatário do AD do Azure usando sua própria conta organizacional. Quando eles estão acessando recursos compartilhados e for solicitados para entrar, os usuários a federação direta serão redirecionados para seu IdP. Depois de entrar com êxito, eles são retornados para o Azure AD para acessar os recursos. Direcionar os tokens de atualização dos usuários da federação são válidos por 12 horas, o [padrão de comprimento para o token de atualização de passagem](../develop/active-directory-configurable-token-lifetimes.md#exceptions) no Azure AD. Se o IdP federado tiver SSO habilitado, o usuário será experiência de SSO e não verão qualquer prompt de entrada após a autenticação inicial.

## <a name="limitations"></a>Limitações

### <a name="dns-verified-domains-in-azure-ad"></a>Domínios DNS verificado no Azure AD
Direcionar a federação só é permitida para domínios que estão ***não*** verificado de DNS no Azure AD. Federação direta é permitida para locatários de não gerenciado (verificado por email ou "viral") do Azure AD porque eles não são verificadas pelo DNS.
### <a name="authentication-url"></a>URL de autenticação
Federação direta é permitida somente para as políticas em que o domínio da URL de autenticação corresponde ao domínio de destino, ou onde a URL de autenticação é um desses provedores de identidade permitidos (essa lista está sujeita a alterações):
-   accounts.google.com
-   pingidentity.com
-   login.pingone.com
-   okta.com
-   my.salesforce.com
-   federation.exostar.com
-   federation.exostartest.com

Por exemplo, ao configurar a federação direta para **fabrikam.com**, a URL de autenticação `https://fabrikam.com/adfs` irá passar a validação. Um host no mesmo domínio também passará, por exemplo `https://sts.fabrikam.com/adfs`. No entanto, a URL de autenticação `https://fabrikamconglomerate.com/adfs` ou `https://fabrikam.com.uk/adfs` para o mesmo domínio não passa.

### <a name="signing-certificate-renewal"></a>Renovação de certificado de assinatura
Se você especificar a URL de metadados nas configurações do provedor de identidade, o AD do Azure será renovada automaticamente o certificado de assinatura quando ela expirar. No entanto, se o certificado é girado por qualquer motivo, antes da hora de expiração, ou se você não fornecer uma URL de metadados, do Azure AD será possível renová-la. Nesse caso, você precisará atualizar manualmente o certificado de autenticação.
## <a name="frequently-asked-questions"></a>Perguntas frequentes
### <a name="can-i-set-up-direct-federation-with-an-unmanaged-email-verified-tenant"></a>Pode configurar a federação direta com um locatário não gerenciado (verificado por email)? 
Sim. Se o domínio não verificado e locatário ainda não passou por uma [controle de administrador](../users-groups-roles/domains-admin-takeover.md), você pode configurar a federação direta. Os locatários não gerenciados ou verificado por email, são criados quando um usuário resgata um convite de B2B ou executa uma inscrição de autoatendimento do Azure AD usando um domínio que não existe no momento. Você pode configurar a federação direta com esses domínios. Se você tentar configurar a federação direta com um domínio verificado de DNS, no portal do Azure ou por meio do PowerShell, você verá um erro.
### <a name="if-direct-federation-and-email-one-time-passcode-authentication-are-both-enabled-which-method-takes-precedence"></a>Se a federação direta e autenticação de senha de uso único email estiverem habilitadas, o qual método terá precedência?
Quando a federação direta é estabelecida com uma organização parceira, ela terá precedência sobre a autenticação de senha de uso único de email para novos usuários convidados da organização. Se um usuário convidado resgatou seu convite usando a autenticação de senha única antes de configurar a federação direta, eles continuarão a usar a autenticação de senha de uso único. 
### <a name="does-direct-federation-address-sign-in-issues-due-to-a-partially-synced-tenancy"></a>Direcionar federação endereço entrar problemas devido a uma locação parcialmente sincronizada?
Não, o [senha de uso único de email](one-time-passcode.md) recurso deve ser usado neste cenário. Um "parcialmente sincronizado locação" refere-se a um locatário do parceiro do Azure AD em que as identidades de usuário local não são totalmente sincronizadas na nuvem. Um convidado cuja identidade ainda não existe na nuvem, mas quem tenta resgatar seu convite de B2B não conseguirá entrar. O recurso de senha de uso único permitiria esse convidado entrar. O recurso de Federação direta aborda cenários em que o convidado tem sua própria conta organizacional gerenciados pelo IdP, mas a organização não tem o AD do Azure presença em todos os.

## <a name="step-1-configure-the-partner-organizations-identity-provider"></a>Etapa 1: Configurar provedor de identidade da organização do parceiro
Primeiro, sua organização parceira precisa configurar seu provedor de identidade com as declarações necessárias e a terceira parte confiável. 

> [!NOTE]
> Para ilustrar como configurar um provedor de identidade para a federação direta, vamos usar os serviços de Federação do Active Directory (AD FS) como um exemplo. Consulte o artigo [configurar a federação direta com o AD FS](direct-federation-adfs.md), que oferece exemplos de como configurar o AD FS como um provedor de identidade SAML 2.0 ou WS-Fed em preparação para a federação direta.

### <a name="saml-20-configuration"></a>Configuração do SAML 2.0

B2B do AD do Azure pode ser configurado para federar com provedores de identidade que usam o protocolo SAML com requisitos específicos listados abaixo. Para obter mais informações sobre como configurar uma relação de confiança entre seu provedor de identidade SAML e o Azure AD, consulte [usar um provedor de identidade SAML 2.0 (IdP) para logon único](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-fed-saml-idp).  

> [!NOTE]
> Observe o domínio de destino para a federação direta não deve ser verificada de DNS no Azure AD. O domínio da URL de autenticação deve corresponder ao domínio de destino ou deve ser o domínio de um provedor de identidade permitidos. Consulte a [limitações](#limitations) seção para obter detalhes. 

#### <a name="required-saml-20-attributes-and-claims"></a>Necessário atributos de SAML 2.0 e declarações
As tabelas a seguir mostram os requisitos para atributos específicos e declarações que devem ser configuradas no provedor de identidade de terceiros. Para configurar a federação direta, os seguintes atributos devem ser recebidos na resposta do provedor de identidade SAML 2.0. Esses atributos podem ser configurados por meio da vinculação para o serviço de token de segurança online arquivo XML ou inserindo-os manualmente.

Atributos necessários para a resposta de SAML 2.0 do IdP:

|Atributo  |Value  |
|---------|---------|
|AssertionConsumerService     |`https://login.microsoftonline.com/login.srf`         |
|Público-alvo     |`urn:federation:MicrosoftOnline`         |
|Emissor     |O emissor do URI do parceiro IdP, por exemplo `http://www.example.com/exk10l6w90DHM0yi...`         |


Declarações necessárias para o token de SAML 2.0 emitido pelo IdP:

|Atributo  |Value  |
|---------|---------|
|Formato de NameID     |`urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

### <a name="ws-fed-configuration"></a>Configuração de WS-Fed 
B2B do AD do Azure pode ser configurado para federar com provedores de identidade que usam o protocolo WS-Fed com alguns requisitos específicos, conforme listado abaixo. Atualmente, os dois provedores de WS-Fed foram testados para compatibilidade com o Azure AD incluem o AD FS e o Shibboleth. Para obter mais informações sobre como estabelecer uma terceira parte confiável entre um provedor de WS-Fed em conformidade com o Azure AD, consulte o "Paper STS Integration usando protocolos WS" disponível na [documentos de compatibilidade de provedor do Azure AD Identity](https://www.microsoft.com/download/details.aspx?id=56843).

> [!NOTE]
> O domínio de destino para a federação direta não deve ser verificada de DNS no Azure AD. O domínio da URL de autenticação deve corresponder ao domínio de destino ou o domínio de um provedor de identidade permitidos. Consulte a [limitações](#limitations) seção para obter detalhes. 

#### <a name="required-ws-fed-attributes-and-claims"></a>Necessário WS-Fed atributos e declarações

As tabelas a seguir mostram os requisitos para atributos específicos e declarações que devem ser configuradas no provedor de identidade de WS-Fed de terceiros. Para configurar a federação direta, os seguintes atributos devem ser recebidos na mensagem do provedor de identidade WS-Fed. Esses atributos podem ser configurados por meio da vinculação para o serviço de token de segurança online arquivo XML ou inserindo-os manualmente.

Atributos necessários na mensagem de WS-Fed do IdP:
 
|Atributo  |Value  |
|---------|---------|
|PassiveRequestorEndpoint     |`https://login.microsoftonline.com/login.srf`         |
|Público-alvo     |`urn:federation:MicrosoftOnline`         |
|Emissor     |O emissor do URI do parceiro IdP, por exemplo `http://www.example.com/exk10l6w90DHM0yi...`         |

Declarações necessárias para o token de WS-Fed emitido pelo IdP:

|Atributo  |Value  |
|---------|---------|
|ImmutableID     |`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`         |
|emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

## <a name="step-2-configure-direct-federation-in-azure-ad"></a>Etapa 2: Configurar a federação direta no Azure AD 
Em seguida, você vai configurar a federação com o provedor de identidade configurado na etapa 1 no Azure AD. Você pode usar o portal do Azure AD ou o PowerShell. Pode levar de 5 a 10 minutos antes que a política de Federação direta entra em vigor. Durante esse tempo, não tente resgatar um convite para o domínio de Federação direta. Os atributos a seguir são obrigatórios:
- URI do parceiro de IdP do emissor
- Ponto de extremidade de autenticação passiva do parceiro IdP (há suporte para apenas https)
- Certificado

### <a name="to-configure-direct-federation-in-the-azure-ad-portal"></a>Para configurar a federação direta no portal do Azure AD

1. Vá para o [Portal do Azure](https://portal.azure.com/). No painel esquerdo, selecione **Azure Active Directory**. 
2. Selecione **Relações organizacionais**.
3. Selecione **provedores de identidade**e, em seguida, selecione **IdP novo SAML/WS-Fed**.

    ![Captura de tela mostrando o botão para adicionar um novo SAML ou WS-Fed IdP](media/direct-federation/new-saml-wsfed-idp.png)

4. Sobre o **IdP novo SAML/WS-Fed** página, em **protocolo do provedor de identidade**, selecione **SAML** ou **WS-FED**.

    ![Captura de tela mostrando o botão de análise na página de IdP de WS-Fed ou SAML](media/direct-federation/new-saml-wsfed-idp-parse.png)

5. Insira o nome de domínio da sua organização de parceiro, que será o nome de domínio de destino para a federação direta
6. Você pode carregar um arquivo de metadados para preencher os detalhes dos metadados. Se você escolher manualmente os metadados de entrada, insira as seguintes informações:
   - Nome de domínio do parceiro IdP
   - ID da entidade do IdP do parceiro
   - Ponto de extremidade de solicitante passivo do IdP do parceiro
   - Certificado
   > [!NOTE]
   > URL de metadados é opcional, mas é altamente recomendável. Se você fornecer a URL de metadados, o Azure AD pode renovar o certificado de autenticação automaticamente quando ela expirar. Se o certificado é girado por qualquer motivo, antes da hora de expiração ou se você não fornecer uma URL de metadados, o Azure AD será possível renová-la. Nesse caso, você precisará atualizar manualmente o certificado de autenticação.

7. Clique em **Salvar**. 

### <a name="to-configure-direct-federation-in-azure-ad-using-powershell"></a>Para configurar a federação direta no Azure AD usando o PowerShell

1. Instale a versão mais recente do módulo PowerShell for Graph ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)) do Azure AD. (Se você precisar obter as etapas detalhadas, o guia de início rápido para adicionar um usuário convidado inclui a seção [instalar o módulo mais recente do AzureADPreview](b2b-quickstart-invite-powershell.md#install-the-latest-azureadpreview-module).) 
2. Execute o comando a seguir: 
   ```powershell
   Connect-AzureAD
   ```
1. No prompt de entrada, entre com a conta de Administrador Global gerenciada. 
2. Execute os seguintes comandos, substituindo os valores do arquivo de metadados de Federação. Para o servidor do AD FS e o Okta, o arquivo de Federação está federationmetadata, por exemplo: `https://sts.totheclouddemo.com/federationmetadata/2007-06/federationmetadata.xml`. 

   ```powershell
   $federationSettings = New-Object Microsoft.Open.AzureAD.Model.DomainFederationSettings
   $federationSettings.PassiveLogOnUri ="https://sts.totheclouddemo.com/adfs/ls/"
   $federationSettings.LogOffUri = $federationSettings.PassiveLogOnUri
   $federationSettings.IssuerUri = "http://sts.totheclouddemo.com/adfs/services/trust"
   $federationSettings.MetadataExchangeUri="https://sts.totheclouddemo.com/adfs/services/trust/mex"
   $federationSettings.SigningCertificate= <Replace with X509 signing cert’s public key>
   $federationSettings.PreferredAuthenticationProtocol="WsFed" OR "Samlp"
   $domainName = <Replace with domain name>
   New-AzureADExternalDomainFederation -ExternalDomainName $domainName  -FederationSettings $federationSettings
   ```

## <a name="step-3-test-direct-federation-in-azure-ad"></a>Etapa 3: Teste a federação direta no Azure AD
Agora, teste sua configuração de Federação direta pelo convidar um novo usuário convidado B2B. Para obter detalhes, consulte [usuários de colaboração de adicionar o Azure AD B2B no portal do Azure](add-users-administrator.md).
 
## <a name="how-do-i-edit-a-direct-federation-relationship"></a>Como faço para editar um relacionamento de Federação direta

1. Vá para o [Portal do Azure](https://portal.azure.com/). No painel esquerdo, selecione **Azure Active Directory**. 
2. Selecione **Relações organizacionais**.
3. Selecione **provedores de identidade**
4. Sob **provedores de identidade SAML/WS-Fed**, selecione o provedor.
5. No painel de detalhes do provedor de identidade, atualize os valores.
6. Clique em **Salvar**.


## <a name="how-do-i-remove-direct-federation"></a>Como posso remover federação direta?
Você pode remover a configuração de Federação direta. Se você fizer isso, os usuários convidados de Federação direta que já tiverem resgatado seus convites não conseguirá entrar. Mas você pode ter acesso a seus recursos novamente, excluindo-os do diretório e reinviting-los. Para remover a federação direta com um provedor de identidade no portal do Azure AD:

1. Vá para o [Portal do Azure](https://portal.azure.com/). No painel esquerdo, selecione **Azure Active Directory**. 
2. Selecione **Relações organizacionais**.
3. Selecione **provedores de identidade**.
4. Selecione o provedor de identidade e, em seguida, selecione **excluir**. 
5. Clique em **Sim** para confirmar a exclusão. 

Para remover uma federação direta com um provedor de identidade por meio do PowerShell:
1. Instale a versão mais recente do módulo PowerShell for Graph ([AzureADPreview](https://www.powershellgallery.com/packages/AzureADPreview)) do Azure AD.
2. Execute o comando a seguir: 
   ```powershell
   Connect-AzureAD
   ```
3. No prompt de entrada, entre com a conta de Administrador Global gerenciada. 
4. Digite o seguinte comando:
   ```powershell
   Remove-AzureADExternalDomainFederation -ExternalDomainName  $domainName
   ```
