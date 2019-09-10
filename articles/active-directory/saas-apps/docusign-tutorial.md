---
title: 'Tutorial: Integração do SSO (logon único) do Azure Active Directory ao DocuSign | Microsoft Docs'
description: Saiba como configurar o logon único entre o Active Directory do Azure e o DocuSign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe1f018612e889f49993895d88cdaf9ad732b393
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70306305"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-docusign"></a>Tutorial: Integração do SSO (logon único) do Azure Active Directory com o DocuSign

Neste tutorial, você aprenderá a integrar o DocuSign ao Azure AD (Azure Active Directory). Quando integrar o DocuSign ao Azure AD, você poderá:

* Controlar no Azure AD quem tem acesso ao DocuSign.
* Permitir que os usuários sejam conectados automaticamente ao DocuSign com suas contas do Azure AD.
* Gerenciar suas contas em um local central: o portal do Azure.

Para saber mais sobre a integração de aplicativos SaaS ao Azure AD, confira [O que é o acesso de aplicativos e o logon único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Pré-requisitos

Para começar, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Caso você não tenha uma assinatura, obtenha uma [conta gratuita](https://azure.microsoft.com/free/).
* Assinatura do DocuSign habilitada para SSO (logon único).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o SSO do Azure AD em um ambiente de teste.

* O DocuSign dá suporte ao SSO iniciado por **SP**

* O DocuSign dá suporte ao provisionamento de usuário **Just-In-Time**

* O DocuSign dá suporte a [provisionamento automático de usuários](https://docs.microsoft.com/azure/active-directory/saas-apps/docusign-provisioning-tutorial)

## <a name="adding-docusign-from-the-gallery"></a>Adicionando o DocuSign por meio da galeria

Para configurar a integração do DocuSign ao Azure AD, é necessário adicionar o DocuSign à lista de aplicativos SaaS gerenciados por meio da galeria.

1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
1. No painel de navegação esquerdo, escolha o serviço **Azure Active Directory**.
1. Navegue até **Aplicativos Empresariais** e, em seguida, escolha **Todos os Aplicativos**.
1. Para adicionar um novo aplicativo, escolha **Novo aplicativo**.
1. Na seção **Adicionar da galeria**, digite **DocuSign** na caixa de pesquisa.
1. Selecione **DocuSign** no painel de resultados e, depois, adicione o aplicativo. Aguarde alguns segundos enquanto o aplicativo é adicionado ao seu locatário.


## <a name="configure-and-test-azure-ad-single-sign-on-for-docusign"></a>Configurar e testar o logon único do Azure AD para o DocuSign

Configure e teste o SSO do Azure AD com o DocuSign usando um usuário de teste chamado **B.Fernandes**. Para que o SSO funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do DocuSign.

Para configurar e testar o SSO do Azure AD com o DocuSign, conclua os seguintes blocos de construção:

1. **[Configurar o SSO do Azure AD](#configure-azure-ad-sso)** – para permitir que os usuários usem esse recurso.
    1. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** para testar o logon único do Azure AD com B.Fernandes.
    1. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que B.Fernandes use o logon único do Azure AD.
1. **[Configurar o SSO do DocuSign](#configure-docusign-sso)** – para definir as configurações de logon único no lado do aplicativo.
    1. **[Criar um usuário de teste do DocuSign](#create-docusign-test-user)** – para ter um equivalente de B. Fernandes no DocuSign que esteja vinculado à representação de usuário do Azure AD.
1. **[Testar o SSO](#test-sso)** – para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estas etapas para habilitar o SSO do Azure AD no portal do Azure.

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **DocuSign**, localize a seção **Gerenciar** e selecione **Logon único**.
1. Na página **Selecionar um método de logon único**, escolha **SAML**.
1. Na página **Configurar o logon único com o SAML**, clique no ícone de edição/caneta da **Configuração Básica do SAML** para editar as configurações.

   ![Editar a Configuração Básica de SAML](common/edit-urls.png)

1. Na seção **Configuração Básica do SAML**, insira os valores para os seguintes campos:

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/sp/<IDPID>`

    b. Na caixa de texto **Identificador (ID da Entidade)** , digite uma URL usando o seguinte padrão: `https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2`

    > [!NOTE]
    > Esses valores não são reais. Atualize esses valores com a URL de Logon e o Identificador reais, que são explicados em seguida na seção **Exibir pontos de extremidade do SAML 2.0** do tutorial.

1. Na página **Configurar o logon único com o SAML**, na seção **Certificado de Autenticação SAML**, localize **Certificado (Base64)** e selecione **Baixar** para baixar o certificado e salvá-lo no computador.

    ![O link de download do Certificado](common/certificatebase64.png)

1. Na seção **Configurar o DocuSign**, copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

Nesta seção, você criará um usuário de teste no portal do Azure chamado B.Fernandes.

1. No painel esquerdo do portal do Azure, escolha **Azure Active Directory**, **Usuários** e, em seguida, **Todos os usuários**.
1. Selecione **Novo usuário** na parte superior da tela.
1. Nas propriedades do **Usuário**, siga estas etapas:
   1. No campo **Nome**, insira `B.Simon`.  
   1. No campo **Nome de usuário**, insira username@companydomain.extension. Por exemplo, `B.Simon@contoso.com`.
   1. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa **Senha**.
   1. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que B.Fernandes use o logon único do Azure concedendo-lhe acesso ao DocuSign.

1. No portal do Azure, selecione **Aplicativos empresariais** e, em seguida, selecione **Todos os aplicativos**.
1. Na lista de aplicativos, selecione **DocuSign**.
1. Na página de visão geral do aplicativo, localize a seção **Gerenciar** e escolha **Usuários e grupos**.

   ![O link “Usuários e grupos”](common/users-groups-blade.png)

1. Escolha **Adicionar usuário** e, em seguida, **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O link Adicionar Usuário](common/add-assign-user.png)

1. Na caixa de diálogo **Usuários e grupos**, selecione **B.Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.
1. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar Função**, escolha a função apropriada para o usuário da lista e, em seguida, clique no botão **Escolher** na parte inferior da tela.
1. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

## <a name="configure-docusign-sso"></a>Configurar o SSO do DocuSign

1. Para automatizar a configuração no DocuSign, é necessário instalar a **Extensão do navegador de Entrada Segura dos Meus Aplicativos**, clicando em **Instalar a extensão**.

    ![Extensão Meus Aplicativos](common/install-myappssecure-extension.png)

2. Depois de adicionar a extensão ao navegador, clique em **Instalação do DocuSign**. Você será direcionado ao aplicativo DocuSign. Em seguida, forneça as credenciais de administrador para entrar no DocuSign. A extensão do navegador configurará automaticamente o aplicativo e automatizará as etapas de 3 a 5.

    ![Configuração da instalação](common/setup-sso.png)

3. Se desejar configurar o DocuSign manualmente, abra uma nova janela do navegador da Web, entre no site da empresa do DocuSign como administrador e execute as etapas a seguir:

4. No canto superior direito da página, clique no **logotipo** do perfil e, em seguida, em **Ir para Administrador**.
  
    ![Configurando o logon único][51]

5. Na página de soluções do domínio, clique em **Domínios**

    ![Configurando o logon único][50]

6. Na seção **Domínios**, clique em **DECLARAR DOMÍNIO**.

    ![Configurando o logon único][52]

7. Na caixa de diálogo **Declarar um domínio**, na caixa de texto **Nome do Domínio**, digite o domínio de sua empresa e clique em **DECLARAR**. Verifique o domínio e confira se o status está ativo.

    ![Configurando o logon único][53]

8. Na página de soluções do domínio, clique em **Provedores de Identidade**.
  
    ![Configurando o logon único][54]

9. Na seção **Provedores de Identidade**, clique em **ADICIONAR PROVEDOR DE IDENTIDADE**. 

    ![Configurando o logon único][55]

10. Na página **Configurações do Provedor de Identidade** , execute as seguintes etapas:

    ![Configurando o logon único][56]

    a. Na caixa de texto **Nome** , digite um nome exclusivo para sua configuração. Não use espaços.

    b. Na caixa de texto **Emissor de provedor de identidade**, cole o valor de **Identificador do Azure AD**, copiado do portal do Azure.

    c. Na caixa de texto **URL de Logon do Provedor de Identidade**, cole o valor de **URL de Logon** que você copiou do portal do Azure.

    d. Na caixa de texto **URL de Logout do Provedor de Identidade**, cole o valor de **URL de Logout** que você copiou do portal do Azure.

    e. Selecione **Assinar solicitação AuthN**.

    f. Para **Enviar solicitação AuthN por**, selecione **POST**.

    g. Em **Enviar solicitação de logoff por**, selecione **GET**.

    h. Na seção **Mapeamento de atributo personalizado**, clique em **ADICIONAR NOVO MAPEAMENTO**.

    ![Configurando o logon único][62]

    i. Escolha o campo a mapear com a declaração do Azure AD. Neste exemplo, a declaração **emailaddress** é mapeada com o valor de **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** . Esse é o nome de declaração padrão do Azure AD para a declaração de email; depois clique em **SALVAR**.

    ![Configurando o logon único][57]

    > [!NOTE]
    > Use o **Identificador de usuário** apropriado para mapear o usuário do Azure AD para o mapeamento de usuário do DocuSign. Selecione o campo adequado e insira o valor apropriado com base nas configurações de sua organização.

    j. Na seção **Certificado do Provedor de Identidade**, clique em **ADICIONAR CERTIFICADO** e, depois, carregue o certificado baixado do portal do Azure AD e clique em **SALVAR**.

    ![Configurando o logon único][58]

    k. Na seção **Provedores de Identidade**, clique em **AÇÕES** e em **Pontos de Extremidade**.

    ![Configurando o logon único][59]

    l. Na seção **Exibir Pontos de Extremidade do SAML 2.0** do **portal de administração do DocuSign**, realize as seguintes etapas:

    ![Configurando o logon único][60]

    * Copie a **URL do Emissor de Provedor de Serviços** e, em seguida, cole-a na caixa de texto **Identificador** da seção **Configuração Básica do SAML** no portal do Azure.

    * Copie a **URL de Logon do Provedor de Serviços** e, em seguida, cole-a na caixa de texto **URL de Logon** da seção **Configuração Básica do SAML** no portal do Azure.

    * Clique em **Fechar**

### <a name="create-docusign-test-user"></a>Criar um usuário de teste do DocuSign

Nesta seção, uma usuária chamada B.Fernandes será criada no DocuSign. O DocuSign dá suporte ao provisionamento de usuário just-in-time, que está habilitado por padrão. Não há itens de ação para você nesta seção. Se um usuário ainda não existir no DocuSign, um novo será criado após a autenticação.

>[!Note]
>Caso precise criar um usuário manualmente, contate a [equipe de suporte do DocuSign](https://support.docusign.com/).

## <a name="test-sso"></a>Testar o SSO 

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do DocuSign no Painel de Acesso, você deverá ser conectado automaticamente ao DocuSign, para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [ Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Experimentar o DocuSign com o Azure AD](https://aad.portal.azure.com/)

<!--Image references-->

[50]: ./media/docusign-tutorial/tutorial_docusign_18.png
[51]: ./media/docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/docusign-tutorial/tutorial_docusign_29.png
[62]: ./media/docusign-tutorial/tutorial_docusign_30.png