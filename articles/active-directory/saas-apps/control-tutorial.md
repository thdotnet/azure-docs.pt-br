---
title: 'Tutorial: Integração do Azure Active Directory ao Control | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Control.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1cb7f505-0d06-44b0-95b1-65b470e97092
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 569021d79e74bc7a5a2582741109e1094ba90de8
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65874044"
---
# <a name="tutorial-integrate-control-with-azure-active-directory"></a>Tutorial: Integrar o Control ao Azure Active Directory

Neste tutorial, você aprenderá a integrar o Control ao Azure AD (Azure Active Directory). Ao integrar o Control ao Azure AD, você pode:

* Controle no Azure AD quem tem acesso ao Control.
* Permitir que os usuários sejam conectados automaticamente ao Control com suas contas do Azure AD.
* Gerencie suas contas em uma localização central – o portal do Azure.

Para saber mais sobre a integração de aplicativos SaaS ao Azure AD, confira [O que é o acesso de aplicativos e o logon único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Pré-requisitos

Para começar, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Caso você não tenha uma assinatura, obtenha uma avaliação gratuita de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).
* Assinatura habilitada para SSO (logon único) do Control.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o SSO do Azure AD em um ambiente de teste. O Control dá suporte ao SSO iniciado por **SP**.

## <a name="adding-control-from-the-gallery"></a>Como adicionar o Control por meio da galeria

Para configurar a integração do Control ao Azure AD, você precisará adicionar o Control por meio da galeria à lista de aplicativos SaaS gerenciados.

1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante ou uma conta pessoal da Microsoft.
1. No painel de navegação esquerdo, selecione o serviço **Azure Active Directory**.
1. Navegue até **Aplicativos Empresariais** e, em seguida, selecione **Todos os Aplicativos**.
1. Para adicionar um novo aplicativo, selecione **Novo aplicativo**.
1. Na seção **Adicionar da por meio da galeria**, digite **Control** na caixa de pesquisa.
1. Selecione **Control** no painel de resultados e, em seguida, adicione o aplicativo. Aguarde alguns segundos enquanto o aplicativo é adicionado ao seu locatário.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Configure e teste o SSO do Azure AD com o Control usando um usuário de teste chamado **Brenda Fernandes**. Para que o SSO funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Control.

Para configurar e testar o SSO do Azure AD com o Control, conclua os seguintes blocos de construção:

1. **[Configurar o SSO do Azure AD](#configure-azure-ad-sso)** – para permitir que os usuários usem esse recurso.
2. **[Configurar o SSO do Control](#configure-control-sso)** – para definir as configurações de logon único no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
5. **[Criar um usuário de teste do Control](#create-control-test-user)** – para ter um equivalente de Brenda Fernandes no Control que esteja vinculado à representação de usuário do Azure AD.
6. **[Testar o SSO](#test-sso)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estas etapas para habilitar o SSO do Azure AD no portal do Azure.

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **Control**, localize a seção **Gerenciar** e selecione **Logon único**.
1. Na página **Selecionar um método de logon único**, selecione **SAML**.
1. Na página **Configurar o Logon Único com o SAML**, clique no ícone Editar/de caneta de **Configuração Básica do SAML** para editar as configurações.

   ![Editar a Configuração Básica de SAML](common/edit-urls.png)

1. Na página **Configuração Básica do SAML**, insira os valores para o seguinte campo:

    Na caixa de texto **URL de logon**, digite um URL usando o seguinte padrão: `https://<SUBDOMAIN>.continuity.net/auth/saml`

    > [!Note]
    > O valor não é real. Atualize o valor com a URL de Logon real. Contate a [equipe de suporte ao cliente do Control](mailto:help@continuity.net) para obter o valor. Você também pode consultar os padrões exibidos na seção **Configuração Básica de SAML** no portal do Azure.

1. Na seção **Certificado de Autenticação SAML**, clique no botão **Editar** para abrir a caixa de diálogo **Certificado de Autenticação SAML**.

    ![Editar o Certificado de Autenticação SAML](common/edit-certificate.png)

1. Na seção **Certificado de Autenticação SAML**, copie a **Impressão Digital** e salve-a no computador.

    ![Copiar o valor da Impressão Digital](common/copy-thumbprint.png)

1. Na seção **Configurar o Control**, copie as URLs apropriadas de acordo com suas necessidades.

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

    a. URL de logon

    b. Identificador do Azure AD

    c. URL de logoff

### <a name="configure-control-sso"></a>Configurar o SSO do Control

Para configurar o logon único no lado do **Control**, é necessário enviar o **valor da Impressão Digital** e as URLs apropriadas copiadas do portal do Azure para a [equipe de suporte do Control](mailto:help@continuity.net). Eles definem essa configuração para ter a conexão de SSO de SAML definida corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

Nesta seção, você criará um usuário de teste no portal do Azure chamado Brenda Fernandes.

1. No painel esquerdo do portal do Azure, selecione **Azure Active Directory**, **Usuários** e, em seguida, **Todos os usuários**.
1. Selecione **Novo usuário** na parte superior da tela.
1. Nas propriedades de **Usuário**, siga estas etapas:
   1. No campo **Nome**, insira `Britta Simon`.  
   1. No campo **Nome de usuário**, insira username@companydomain.extension. Por exemplo, `BrittaSimon@contoso.com`.
   1. Marque a caixa de seleção **Mostrar senha** e, em seguida, anote o valor exibido na caixa **Senha**.
   1. Clique em **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo a ela acesso ao Control.

1. No portal do Azure, selecione **Aplicativos empresariais** e, em seguida, selecione **Todos os aplicativos**.
1. Na lista de aplicativos, selecione **Control**.
1. Na página de visão geral do aplicativo, localize a seção **Gerenciar** e selecione **Usuários e grupos**.

   ![O link “Usuários e grupos”](common/users-groups-blade.png)

1. Selecione **Adicionar usuário** e, em seguida, **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![O link Adicionar Usuário](common/add-assign-user.png)

1. Na caixa de diálogo **Usuários e grupos**, selecione **Brenda Fernandes** na lista de Usuários e, em seguida, clique no botão **Selecionar** na parte inferior da tela.
1. Se você estiver esperando um valor de função na declaração SAML, na caixa de diálogo **Selecionar Função**, selecione a função apropriada para o usuário na lista e, em seguida, clique no botão **Selecionar** na parte inferior da tela.
1. Na caixa de diálogo **Adicionar atribuição**, clique no botão **Atribuir**.

### <a name="create-control-test-user"></a>Criar um usuário de teste do Control

Nesta seção, você criará um usuário chamado Brenda Fernandes no Control. Trabalhe com a [equipe de suporte do Control](mailto:help@continuity.net) para adicionar os usuários à plataforma Control. Os usuários devem ser criados e ativados antes de usar o logon único.

### <a name="test-sso"></a>Testar o SSO

Ao selecionar o bloco do Control no Painel de Acesso, você deverá ser conectado automaticamente ao Control, para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [ Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
