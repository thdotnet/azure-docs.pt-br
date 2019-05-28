---
title: 'Tutorial: Integração do Azure Active Directory ao Replicon | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Replicon.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 02a62f15-917c-417c-8d80-fe685e3fd601
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: be006cd49041a33c13b5cc5c459a1d54c46c3b47
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64727610"
---
# <a name="tutorial-azure-active-directory-integration-with-replicon"></a>Tutorial: Integração do Azure Active Directory ao Replicon

Neste tutorial, você aprenderá a integrar o Replicon ao Microsoft Azure AD (Azure Active Directory).
A integração do Replicon ao Microsoft Azure AD oferece os seguintes benefícios:

* Você pode controlar no Microsoft Azure AD quem terá acesso ao Replicon.
* Você pode permitir que os usuários sejam conectados automaticamente ao Replicon (logon único) com suas contas do Microsoft Azure Active Directory.
* Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Microsoft Azure AD ao Replicon, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Se não tiver um ambiente do Azure AD, poderá obter uma [conta gratuita](https://azure.microsoft.com/free/)
* Uma assinatura habilitada para logon único do Replicon

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O Replicon dá suporte a SSO iniciado por **SP**

## <a name="adding-replicon-from-the-gallery"></a>Adicionando Replicon a partir da galeria

Para configurar a integração do Replicon ao Microsoft Azure AD, você precisará adicionar o Replicon a partir da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Replicon a partir da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)** , no painel navegação à esquerda, clique no ícone **Azure Active Directory**.

    ![O botão Azure Active Directory](common/select-azuread.png)

2. Navegue até **Aplicativos Empresariais** e, em seguida, selecione a opção **Todos os Aplicativos**.

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, digite **Replicon**, selecione **Replicon** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Replicon na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Microsoft Azure Active Directory com o Replicon, com base em uma usuária de teste chamada **Brenda Fernandes**.
Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Microsoft Azure Active Directory e o usuário relacionado do Replicon.

Para configurar e testar o logon único do Microsoft Azure AD com o Replicon, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Configurar o logon único do Replicon](#configure-replicon-single-sign-on)** : para definir as configurações de Logon Único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar um usuário de teste do Replicon](#create-replicon-test-user)** : para ter um equivalente de Brenda Fernandes no Replicon que esteja vinculado à representação de usuário no Microsoft Azure Active Directory.
6. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Microsoft Azure Active Directory com o Replicon, execute as seguintes etapas:

1. No [portal do Azure](https://portal.azure.com/), na página de integração do aplicativo **Replicon**, clique em **Logon Único**.

    ![Link Configurar logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único**, selecione o modo **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

3. Na página **Definir logon único com SAML**, clique no ícone **Editar** para abrir a caixa de diálogo **Configuração básica do SAML**.

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

4. Na seção **Configuração básica de SAML**, realize as seguintes etapas:

    ![Informações de logon único de Domínio e URLs do Replicon](common/sp-identifier-reply.png)

     a. Na caixa de texto **URL de Entrada** digite uma URL usando o seguinte padrão: `https://na2.replicon.com/<companyname>/saml2/sp-sso/post`

    b. Na caixa **Identificador**, digite uma URL usando o seguinte padrão: `https://global.replicon.com/<companyname>`

    c. No **URL de resposta** caixa de texto, digite uma URL usando o seguinte padrão: `https://global.replicon.com/!/saml2/<companyname>/sso/post`

    > [!NOTE]
    > Esses valores não são reais. Você precisa atualizar esses valores com a URL de Logon, o Identificador e a URL de Resposta reais. Entre em contato com a [equipe de suporte ao cliente do Replicon](https://www.replicon.com/customerzone/contact-support) para obter esses valores. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

5. Na página **Configurar Logon Único com SAML**, na seção **Certificado de Autenticação SAML**, clique em **Baixar** para baixar o **XML de Metadados de Federação** usando as opções fornecidas de acordo com seus requisitos e salve-o no computador.

    ![O link de download do Certificado](common/metadataxml.png)

6. Na seção **Configurar o Replicon**, copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

    a. URL de logon

    b. Identificador do Azure AD

    c. URL de logoff

### <a name="configure-replicon-single-sign-on"></a>Configurar o logon único do Replicon

1. Em uma janela de navegador da Web diferente, faça logon no site de sua empresa no Replicon como administrador.

2. Para configurar o SAML 2.0, execute as seguintes etapas:

    ![Habilitar autenticação de SAML](./media/replicon-tutorial/ic777805.png "Habilitar autenticação de SAML")

     a. Para exibir a caixa de diálogo **EnableSAML Authentication2**, acrescente o seguinte à URL, após a chave da empresa: `/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

    * A seguir, é mostrado o esquema do URL completo: `https://na2.replicon.com/\<YourCompanyKey\>/services/SecurityService1.svc/help/test/EnableSAMLAuthentication2`

   b. Clique em **+** para expandir a seção **v20Configuration**.

   c. Clique em **+** para expandir a seção **metaDataConfiguration**.

   d. Clique em **Escolher Arquivo** para selecionar o arquivo XML de metadados de provedor de identidade e clique em **Enviar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

1. No Portal do Azure, no painel esquerdo, selecione **Azure Active Directory**, selecione **Usuários** e, em seguida, **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](common/users.png)

2. Selecione **Novo usuário** na parte superior da tela.

    ![Botão Novo usuário](common/new-user.png)

3. Nas Propriedades do usuário, execute as etapas a seguir.

    ![A caixa de diálogo Usuário](common/user-properties.png)

    a. No campo **Nome**, insira **BrendaFernandes**.
  
    b. No campo **Nome de usuário**, digite `brittasimon@yourcompanydomain.extension`. Por exemplo, BrittaSimon@contoso.com

    c. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa Senha.

    d. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo-lhe acesso ao Replicon.

1. No portal do Azure, selecione **Aplicativos Empresariais**, **Todos os Aplicativos** e, em seguida, **Replicon**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, escolha **Replicon**.

    ![O link do Replicon na lista de Aplicativos](common/all-applications.png)

3. No menu à esquerda, selecione **Usuários e grupos**.

    ![O link “Usuários e grupos”](common/users-groups-blade.png)

4. Escolha o botão **Adicionar usuário** e, em seguida, escolha **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O painel Adicionar Atribuição](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos**, escolha **Brenda Fernandes** na lista Usuários e clique no botão **Selecionar** na parte inferior da tela.

6. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar função**, escolha a função de usuário apropriada na lista e clique no botão **Selecionar** na parte inferior da tela.

7. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

### <a name="create-replicon-test-user"></a>Criar um usuário de teste do Replicon

O objetivo desta seção é criar um usuário chamado Brenda Fernandes no Mixpanel.

**Se você precisar criar o usuário manualmente, execute as seguintes etapas:**

1. Em uma janela de navegador da Web, faça logon no site de sua empresa do Replicon como administrador.

2. Vá para **Administração\>Usuários**.

    ![Usuários](./media/replicon-tutorial/ic777806.png "Usuários")

3. Clique em **+Adicionar Usuário**.

    ![Adicionar Usuário](./media/replicon-tutorial/ic777807.png "Adicionar Usuário")

4. Na seção **Perfil de Usuário** , realize as seguintes etapas:

    ![Perfil de usuário](./media/replicon-tutorial/ic777808.png "Perfil de usuário")

     a. Na caixa de texto **Nome de Logon**, digite o endereço de email do Microsoft Azure Active Directory para o usuário do que você deseja provisionar, como `BrittaSimon\@contoso.com`.

    b. Para **Tipo de Autenticação**, selecione **SSO**.

    c. Na caixa de texto **Departamento** , digite o departamento do usuário.

    d. Para **Tipo de Funcionário**, selecione **Administrador**.

    e. Clique em **Salvar Perfil do Usuário**.

> [!NOTE]
> É possível usar qualquer outra ferramenta de criação da conta de usuário do Replicon ou as APIs fornecidas pelo Replicon para provisionar as contas de usuário do Microsoft Azure AD.

### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco Replicon no Painel de Acesso, você deve ser conectado automaticamente ao Replicon no qual configurou o logon único. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)