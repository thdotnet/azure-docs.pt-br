---
title: 'Tutorial: integração do Azure Active Directory com o Projectplace | Microsoft Docs'
description: Neste tutorial você aprenderá a configurar o logon único entre o Azure Active Directory e o Projectplace.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 298059ca-b652-4577-916a-c31393d53d7a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/26/2019
ms.author: jeedes
ms.openlocfilehash: 17fbc6bc4f022a15c34c5ca7b9465be392cdc639
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65560622"
---
# <a name="tutorial-azure-active-directory-integration-with-projectplace"></a>Tutorial: integração do Azure Active Directory com o Projectplace

Neste tutorial, você aprenderá a integrar o Projectplace ao Azure AD (Azure Active Directory).

Essa integração oferece estes benefícios:

* Você pode usar o Azure AD para controlar quem tem acesso ao Projectplace.
* Você pode permitir que seus usuários entrem automaticamente no Projectplace (logon único) com suas contas do Azure AD.
* Gerencie suas contas em um único local: o portal do Azure.

Para saber mais sobre a integração de aplicativos SaaS ao Azure AD, confira [Logon único em aplicativos no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Caso você não tenha uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Projectplace, você precisa de:

* Uma assinatura do Azure AD. Caso você não tenha um ambiente do Azure AD, inscreva-se em uma assinatura de [avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).
* Uma assinatura do Projectplace que tenha o logon único habilitado.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste.

* O Projectplace é compatível com SSO iniciado por SP.

## <a name="add-projectplace-from-the-gallery"></a>Adicionar o Projectplace por meio da galeria

Para configurar a integração do Projectplace ao Azure AD, você precisa adicionar o Projectplace à sua lista de aplicativos SaaS gerenciados na galeria.

1. No [portal do Azure](https://portal.azure.com), no painel esquerdo, selecione **Azure Active Directory**:

    ![Selecione Azure Active Directory](common/select-azuread.png)

2. Acesse **Aplicativos empresariais** > **Todos os aplicativos**:

    ![A folha Aplicativos empresariais](common/enterprise-applications.png)

3. Para adicionar um aplicativo, selecione **Novo aplicativo** na parte superior da janela:

    ![Selecionar Novo aplicativo](common/add-new-app.png)

4. Na caixa de pesquisa, insira **Projectplace**. Selecione **Projectplace** nos resultados da pesquisa e, em seguida, selecione **Adicionar**.

     ![Resultados da Pesquisa](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Projectplace usando um usuário de teste chamado Brenda Fernandes.
Para habilitar o logon único, você precisará estabelecer uma relação entre um usuário do Azure AD e o usuário correspondente no Projectplace.

Para configurar e testar o logon único do Azure AD com o Projectplace, você precisará concluir estas etapas:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** para habilitar o recurso para os usuários.
2. **[Configurar o logon único do Projectplace](#configure-projectplace-single-sign-on)** no lado do aplicativo.
3. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** para testar o logon único do Azure AD.
4. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** para habilitar o logon único do Azure AD para o usuário.
5. **[Criar um usuário de teste do Projectplace](#create-a-projectplace-test-user)** que esteja vinculado à representação do Azure AD do usuário.
6. **[Testar o logon único](#test-single-sign-on)** para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no portal do Azure.

Para configurar o logon único do Azure AD com o Projectplace, siga estas etapas:

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **Projectplace**, selecione **Logon único**:

    ![Selecione Logon único](common/select-sso.png)

2. Na caixa de diálogo **Selecionar um método de logon único**, selecione o modo **SAML/WS-Fed** para habilitar o logon único:

    ![Selecionar um método de logon único](common/select-saml-option.png)

3. Na página **Configurar logon único com SAML**, selecione o ícone **Editar** para abrir a caixa de diálogo **Configuração Básica de SAML**:

    ![Ícone Editar](common/edit-urls.png)

4. Na caixa de diálogo **Configuração básica de SAML**, na caixa **URL de logon**, digite uma URL neste padrão:

    `https://<company>.projectplace.com`

   ![Caixa de diálogo Configuração Básica de SAML](common/sp-signonurl.png)
    > [!NOTE]
    > Esse valor é um espaço reservado. Você precisa usar a URL de logon real. Entre em contato com a [equipe de suporte do Projectplace](https://success.planview.com/Projectplace/Support) para obter o valor. Você também pode ver os padrões exibidos na caixa de diálogo **Configuração Básica de SAML** no portal do Azure.

5. Na página **Configurar logon único com SAML**, na seção **Certificado de Autenticação SAML**, selecione o link **Baixar** ao lado do **XML de Metadados de Federação**, de acordo com suas necessidades e salve o certificado no computador:

    ![Link de download do certificado](common/metadataxml.png)

6. Na seção **Configurar Projectplace**, copie as URLs adequadas, de acordo com suas necessidades.

    ![Copiar as URLs de configuração](common/copy-configuration-urls.png)

    1. **URL de Logon**.

    1. **Identificador do Azure AD**.

    1. **URL de Logoff**.

### <a name="configure-projectplace-single-sign-on"></a>Configurar o logon único do Projectplace

Para configurar o logon único no lado do **Projectplace**, é necessário enviar o certificado de **XML de metadados de federação** baixado e as URLs que você copiou do portal do Azure para a [equipe de suporte do Projectplace](https://success.planview.com/Projectplace/Support). Essa equipe garante que a conexão de SSO do SAML seja definida corretamente em ambos os lados.

>[!NOTE]
>A configuração de logon único deve ser executada pela [equipe de suporte do Projectplace](https://success.planview.com/Projectplace/Support). Assim que a configuração for concluída, você receberá uma notificação.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

Nesta seção, você criará um usuário de teste chamado Brenda Fernandes no portal do Azure.

1. No portal do Azure, selecione **Azure Active Directory** no painel esquerdo, **Usuários** e, em seguida, **Todos os usuários**:

    ![Selecionar Todos os usuários](common/users.png)

2. Selecione **Novo usuário** na parte superior da tela:

    ![Selecionar Novo usuário](common/new-user.png)

3. Na caixa de diálogo **Usuário**, execute as etapas a seguir.

    ![Caixa de diálogo Usuário](common/user-properties.png)

    1. Na caixa **Nome**, insira **BrendaFernandes**.
  
    1. Na caixa **Nome de usuário**, insira **BrendaFernandes@\<domíniodaempresa>.\<extensão>** . (Por exemplo, BrittaSimon@contoso.com.)

    1. Selecione **Mostrar Senha** e, em seguida, anote o valor mostrado na caixa **Senha**.

    1. Selecione **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure AD concedendo a ela o acesso ao Projectplace.

1. No portal do Azure, selecione **Aplicativos empresariais**, **Todos os aplicativos** e, em seguida, selecione **Projectplace**.

    ![Folha de aplicativos empresariais](common/enterprise-applications.png)

2. Na lista de aplicativos, selecione **Projectplace**.

    ![Lista de aplicativos](common/all-applications.png)

3. No painel esquerdo, selecione **Usuários e grupos**:

    ![Selecionar Usuários e grupos](common/users-groups-blade.png)

4. Selecione **Adicionar usuário** e, em seguida, selecione **Usuários e grupos** na caixa de diálogo **Adicionar Atribuição**.

    ![Selecione Adicionar usuário](common/add-assign-user.png)

5. Na caixa de diálogo **Usuários e grupos**, selecione **Brenda Fernandes** na lista de usuários e, em seguida, clique no botão **Selecionar** na parte inferior da tela.

6. Se você esperar um valor de função na declaração SAML, na caixa de diálogo **Selecionar Função**, selecione a função apropriada para o usuário na lista. Clique no botão **Selecionar** na parte inferior da tela.

7. Na caixa de diálogo **Adicionar Atribuição**, selecione **Atribuir**.

### <a name="create-a-projectplace-test-user"></a>Criar um usuário de teste do Projectplace

Para permitir que os usuários do Azure AD entrem no Projectplace, você precisará adicioná-los ao Projectplace. Você precisa adicioná-los manualmente.

Para criar uma conta de usuário, siga estas etapas:

1. Entre no site do **Projectplace** da sua empresa como administrador.

2. Acesse **Pessoas** e selecione **Membros**:
   
    ![Acesse Pessoas e, em seguida, selecione Membros](./media/projectplace-tutorial/ic790228.png "Pessoas")

3. Selecione **Adicionar membro**:
   
    ![Selecionar Adicionar membro](./media/projectplace-tutorial/ic790232.png "Adicionar membros")

4. Na seção **Adicionar Membro**, realize as etapas a seguir.
   
    ![Seção Adicionar Membro](./media/projectplace-tutorial/ic790233.png "Novos Membros")
   
    1. Na caixa **Novos Membros**, digite o endereço de email de uma conta válida do Azure AD que você deseja adicionar.
   
    1. Selecione **Enviar**.

   Um email contendo um link para confirmar a conta antes que ela se torne ativa é enviado ao titular da conta do Azure AD.

>[!NOTE]
>Você também pode usar qualquer outra API ou ferramenta de criação de conta de usuário fornecida pelo Projectplace para adicionar contas de usuário do Azure AD.

### <a name="test-single-sign-on"></a>Testar logon único

Agora você precisará testar a configuração de logon único do Azure AD usando o Painel de Acesso.

Ao selecionar o bloco do Projectplace no Painel de Acesso, você deverá ser conectado automaticamente à instância do Projectplace na qual você configurou o SSO. Para obter mais informações, confira [Acessar e usar aplicativos no portal Meus Aplicativos](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionais

- [Tutoriais para a integração de aplicativos SaaS ao Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
