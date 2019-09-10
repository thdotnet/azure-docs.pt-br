---
title: 'Tutorial: Integração do SSO (logon único) do Azure Active Directory ao Jamf Pro | Microsoft Docs'
description: Saiba como configurar um logon único entre o Azure Active Directory e o Jamf Pro.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 35e86d08-c29e-49ca-8545-b0ff559c5faf
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/28/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 24f0d067351ab2e24b103624c8534ac8d6f2e757
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70305418"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-jamf-pro"></a>Tutorial: Integração do SSO (logon único) do Azure Active Directory com o Jamf Pro

Neste tutorial, você aprenderá a integrar o Jamf Pro ao Azure AD (Azure Active Directory). Ao integrar o Jamf Pro ao Azure AD, você pode:

* Controlar no Azure AD quem terá acesso ao Jamf Pro.
* Permitir que os usuários sejam conectados automaticamente ao Jamf Pro com suas contas do Azure AD.
* Gerenciar suas contas em um local central: o portal do Azure.

Para saber mais sobre a integração de aplicativos SaaS ao Azure AD, confira [O que é o acesso de aplicativos e o logon único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Pré-requisitos

Para começar, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Caso você não tenha uma assinatura, obtenha uma [conta gratuita](https://azure.microsoft.com/free/).
* Uma assinatura do Jamf Pro habilitada para SSO (logon único).

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o SSO do Azure AD em um ambiente de teste.

* O Jamf Pro é compatível com SSO iniciado por **SP e IDP**

## <a name="adding-jamf-pro-from-the-gallery"></a>Como adicionar o Jamf Pro da galeria

Para configurar a integração do Jamf Pro ao AD do Azure, você precisará adicionar o Jamf Pro da galeria à sua lista de aplicativos SaaS gerenciados.

1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
1. No painel de navegação esquerdo, escolha o serviço **Azure Active Directory**.
1. Navegue até **Aplicativos Empresariais** e, em seguida, escolha **Todos os Aplicativos**.
1. Para adicionar um novo aplicativo, escolha **Novo aplicativo**.
1. Na seção **Adicionar da galeria**, digite **Jamf Pro** na caixa de pesquisa.
1. Selecione **Jamf Pro** no painel de resultados e, em seguida, adicione o aplicativo. Aguarde alguns segundos enquanto o aplicativo é adicionado ao seu locatário.

## <a name="configure-and-test-azure-ad-single-sign-on-for-jamf-pro"></a>Configurar e testar o logon único do Azure AD para o Jamf Pro

Configure e teste o SSO do Azure AD com o Jamf Pro usando um usuário de teste chamado **B.Fernandes**. Para que o SSO funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Jamf Pro.

Para configurar e testar o SSO do Azure AD com o Jamf Pro, conclua os seguintes blocos de construção:

1. **[Configurar o SSO do Azure AD](#configure-azure-ad-sso)** – para permitir que os usuários usem esse recurso.
    1. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** para testar o logon único do Azure AD com B.Fernandes.
    1. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que B.Fernandes use o logon único do Azure AD.
1. **[Configurar o SSO do Jamf Pro](#configure-jamf-pro-sso)** – para definir as configurações de logon único no lado do aplicativo.
    1. **[Criar um usuário de teste do Jamf Pro](#create-jamf-pro-test-user)** – para ter um equivalente de B. Fernandes no Jamf Pro que esteja vinculado à representação de usuário do Azure AD.
1. **[Testar o SSO](#test-sso)** – para verificar se a configuração funciona.

## <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estas etapas para habilitar o SSO do Azure AD no portal do Azure.

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **Jamf Pro**, localize a seção **Gerenciar** e selecione **Logon único**.
1. Na página **Selecionar um método de logon único**, escolha **SAML**.
1. Na página **Configurar o logon único com o SAML**, clique no ícone de edição/caneta da **Configuração Básica do SAML** para editar as configurações.

   ![Editar a Configuração Básica de SAML](common/edit-urls.png)

1. Na seção **Configuração Básica do SAML**, caso deseje configurar o aplicativo no modo iniciado por **IDP**, digite os valores dos seguintes campos:

    a. No **identificador** caixa de texto, digite uma URL usando o seguinte padrão: `https://<subdomain>.jamfcloud.com/saml/metadata`

    b. No **URL de resposta** caixa de texto, digite uma URL usando o seguinte padrão: `https://<subdomain>.jamfcloud.com/saml/SSO`

1. Clique em **Definir URLs adicionais** e execute o passo seguinte se quiser configurar a aplicação no modo **SP** iniciado:

    Na caixa de texto **URL de logon**, digite um URL usando o seguinte padrão: `https://<subdomain>.jamfcloud.com`

    > [!NOTE]
    > Esses valores não são reais. Atualize esses valores com o Identificador real, a URL de Resposta e a URL de Entrada. Você obterá o valor real do Identificador de **Logon único** seção no portal de Jamf Pro, que é explicado no tutorial posteriormente. Você pode extrair o valor real **subdomínio** o valor do identificador de valor e usar as informações do **subdomínio** na URL de entrada e da URL de resposta. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

1. Na página **Configurar o logon único com o SAML**, na seção **Certificado de Autenticação SAML**, clique no botão Copiar para copiar a **URL de Metadados de Federação do Aplicativo** e salve-a no computador.

    ![O link de download do Certificado](common/copy-metadataurl.png)

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

Nesta seção, você permitirá que B.Fernandes use o logon único do Azure concedendo a ela acesso ao Jamf Pro.

1. No portal do Azure, selecione **Aplicativos empresariais** e, em seguida, selecione **Todos os aplicativos**.
1. Na lista de aplicativos, escolha **Jamf Pro**.
1. Na página de visão geral do aplicativo, localize a seção **Gerenciar** e escolha **Usuários e grupos**.

   ![O link “Usuários e grupos”](common/users-groups-blade.png)

1. Escolha **Adicionar usuário** e, em seguida, **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O link Adicionar Usuário](common/add-assign-user.png)

1. Na caixa de diálogo **Usuários e grupos**, selecione **B.Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.
1. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar Função**, escolha a função apropriada para o usuário da lista e, em seguida, clique no botão **Escolher** na parte inferior da tela.
1. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

## <a name="configure-jamf-pro-sso"></a>Configurar o SSO do JAMF Pro

1. Para automatizar a configuração no Jamf Pro é necessário instalar a **Extensão do navegador de Entrada Segura dos Meus Aplicativos**, clicando em **Instalar a extensão**.

    ![Extensão Meus Aplicativos](common/install-myappssecure-extension.png)

2. Após adicionar a extensão ao navegador, clicar em **configurar Jamf Pro** irá direcioná-lo ao aplicativo Jamf Pro. Em seguida, forneça as credenciais de administrador para entrar no Jamf Pro. A extensão do navegador configurará automaticamente o aplicativo e automatizará as etapas de 3 a 7.

    ![Configuração da instalação](common/setup-sso.png)

3. Se quiser configurar o Jamf Pro manualmente, abra uma nova janela do navegador da Web, entre no site da empresa do Jamf Pro como administrador e execute as seguintes etapas:

4. Clique no **ícone de Configurações** no canto superior direito da página.

    ![Configuração do Jamf Pro](./media/jamfprosamlconnector-tutorial/configure1.png)

5. Clique no **Logon Único**.

    ![Configuração do Jamf Pro](./media/jamfprosamlconnector-tutorial/configure2.png)

6. Na página **Single Sign-On**, execute as seguintes etapas:

    ![Configuração do Jamf Pro](./media/jamfprosamlconnector-tutorial/configure3.png)

    a. Marque **Habilitar Autenticação de Logon Único**.

    b. Selecione **Outro** como opção no menu suspenso **PROVEDOR DE IDENTIDADE**.

    c. Na caixa de texto **OTHER PROVIDER**, digite **Azure AD**.

    d. Copie o valor da **ID DA ENTIDADE** e cole-o na caixa de texto **Identificador (ID da Entidade)** , que está na seção **Configuração SAML Básica** no portal do Azure.

    > [!NOTE]
    > Na parte `<SUBDOMAIN>`, você precisa usar esse valor para completar a URL de Entrada e a URL de Resposta na seção **Configuração SAML Básica** no portal do Azure.

    e. Selecione **URL de metadados** como uma opção do menu suspenso **FONTE DE METADADOS DO PROVEDOR DE IDENTIDADE** e, na caixa de texto a seguir, cole o valor da **URL de metadados de federação do aplicativo** que você copiou do portal do Azure.

7. Na mesma página, role para baixo até a seção **Mapeamento de Usuário** e execute as seguintes etapas: 

    ![O Jamf Pro único](./media/jamfprosamlconnector-tutorial/tutorial_jamfprosamlconnector_single.png)

    a. Selecione a opção **NameID** para **MAPEAMENTO DE USUÁRIO DO PROVEDOR DE IDENTIDADE**. Por padrão, essa configuração é definida como **NameID**, mas você pode definir um atributo personalizado.

    b. Selecione **Email** para **MAPEAMENTO DE USUÁRIO DO JAMF PRO**. O Jamf Pro mapeia atributos SAML enviados pelo IdP das seguintes maneiras: por usuários e por grupos. Quando um usuário tenta acessar o Jamf Pro, por padrão, o Jamf Pro obtém informações sobre o usuário do Provedor de Identidade e o associa às contas de usuário do Jamf Pro. Se a conta de usuário recebida não existir no Jamf Pro, ocorrerá a correspondência do nome do grupo.

    c. Cole o valor `http://schemas.microsoft.com/ws/2008/06/identity/claims/groups` no na caixa de texto **NOME DO ATRIBUTO DE GRUPO DO PROVEDOR DE IDENTIDADE**.

    d. Com a seleção de **Permitir que os usuários ignorem a autenticação de Logon Único**, os usuários não serão redirecionados para a página de entrada do Provedor de Identidade para autenticação, mas poderão entrar no Jamf Pro diretamente. Quando um usuário tenta acessar o Jamf Pro por meio do Provedor de Identidade, ocorre a autenticação e autorização de SSO iniciada pelo IdP.

    e. Clique em **Save** (Salvar).

### <a name="create-jamf-pro-test-user"></a>Criar um usuário de teste do Jamf Pro

Para permitir que os usuários do Azure AD entrem no Jamf Pro, eles devem ser provisionados no Jamf Pro. No caso do Jamf Pro, o provisionamento é uma tarefa manual.

**Para provisionar uma conta de usuário, execute as seguintes etapas:**

1. Entre em seu site corporativo do Jamf Pro como administrador.

2. Clique no **ícone de Configurações** no canto superior direito da página.

    ![Adicionar Funcionário](./media/jamfprosamlconnector-tutorial/configure1.png)

3. Clique em **Contas de usuário e grupos do Pro Jamf**.

    ![Adicionar Funcionário](./media/jamfprosamlconnector-tutorial/user1.png)

4. Clique em **Novo**.

    ![Adicionar Funcionário](./media/jamfprosamlconnector-tutorial/user2.png)

5. Selecione **Criar Conta Padrão**.

    ![Adicionar Funcionário](./media/jamfprosamlconnector-tutorial/user3.png)

6. Na caixa de diálogo **Novo Usuário**, realize as seguintes etapas:

    ![Adicionar Funcionário](./media/jamfprosamlconnector-tutorial/user4.png)

    a. Na caixa de texto **USERNAME**, digite o nome completo de BrendaFernandes.

    b. Selecione as opções apropriadas de acordo com a sua organização para o **NÍVEL DE ACESSO**, **CONJUNTO DE PRIVILÉGIOS** e para o **STATUS DE ACESSO**.

    c. Na caixa de texto **NOME COMPLETO**, digite o nome completo de Brenda Fernandes.

    d. Na caixa de texto **ENDEREÇO DE EMAIL**, digite o endereço de email da conta de Brenda Fernandes.

    e. Na caixa de texto **SENHA**, digite a senha do usuário.

    f. Na caixa de texto **VERIFICAR SENHA**, digite a senha do usuário.

    g. Clique em **Save** (Salvar).

## <a name="test-sso"></a>Testar o SSO 

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco do Jamf Pro no Painel de Acesso, você deverá ser conectado automaticamente ao Jamf Pro no qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [ Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Experimentar o Jamf Pro com o Azure AD](https://aad.portal.azure.com/)

