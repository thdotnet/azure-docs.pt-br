---
title: 'Tutorial: Integração do Azure Active Directory ao Workgrid | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o monday.com.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9e8ad807-0664-4e31-91de-731097c768e2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/15/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fc142bf02a44ea85861f4cc648fd7ee8602c7520
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65896888"
---
# <a name="tutorial-azure-active-directory-integration-with-mondaycom"></a>Tutorial: Integração do Azure Active Directory com o monday.com

Neste tutorial, você aprende a integrar o monday.com ao Azure Active Directory (Azure AD).

A integração do monday.com ao Azure AD oferece os seguintes benefícios:

* Você pode usar o Azure AD para controlar quem tem acesso ao monday.com.
* Os usuários podem ser conectados automaticamente ao monday.com com suas contas do Azure AD (logon único).
* Você pode gerenciar suas contas em um único local, o portal clássico do Azure.

Para obter mais informações sobre a integração de aplicativos SaaS (software como serviço) ao Azure AD, confira [Logon único em aplicativos no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD com o monday.com, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Caso você não tenha uma assinatura do Azure AD, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.
* Uma assinatura do monday.com com logon único habilitado.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o logon único do Azure AD em um ambiente de teste e integrará o monday.com ao Azure AD.

O monday.com dá suporte aos seguintes recursos:

* **Logon único iniciado por SP**
* **Logon único iniciado por IDP**
* **Provisionamento Just-In-Time do usuário**

## <a name="add-mondaycom-in-the-azure-portal"></a>Adicionar o monday.com no portal do Azure

Para integrar o monday.com ao Azure AD, é necessário adicionar o monday.com à lista de aplicativos SaaS gerenciados.

1. Entre no [Portal do Azure](https://portal.azure.com).

1. No menu esquerdo, selecione **Azure Active Directory**.

    ![A opção Azure Active Directory](common/select-azuread.png)

1. Selecione **Aplicativos empresariais** > **Todos os aplicativos**.

    ![O painel Aplicativos Empresariais](common/enterprise-applications.png)

1. Para adicionar um aplicativo, selecione **Novo aplicativo**.

    ![A opção Novo aplicativo](common/add-new-app.png)

1. Na caixa de pesquisa, insira **monday.com**. Nos resultados da pesquisa, selecione **monday.com** e, em seguida, **Adicionar**.

    ![monday.com na lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o monday.com, com base em um usuário de teste chamado **Brenda Fernandes**. Para que o logon único funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do monday.com.

Para configurar e testar o logon único do Azure AD com o monday.com, você precisará concluir os seguintes blocos de construção:

| Tarefa | DESCRIÇÃO |
| --- | --- |
| **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** | Permite que os usuários usem esse recurso. |
| **[Configurar o logon único do monday.com](#configure-mondaycom-single-sign-on)** | Define as configurações de logon único no aplicativo. |
| **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** | Testa o logon único do Azure AD para um usuário chamado Brenda Fernandes. |
| **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** | Habilita Brenda Fernandes a usar o logon único do Azure AD. |
| **[Criar um usuário de teste do monday.com](#create-a-mondaycom-test-user)** | Cria um equivalente de Brenda Fernandes no monday.com que é vinculado à representação do usuário do Azure AD. |
| **[Testar o logon único](#test-single-sign-on)** | Verifica se a configuração funciona. |

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você configurará o logon único do Azure AD com o monday.com no portal do Azure.

1. No [portal do Azure](https://portal.azure.com/), no painel de integração de aplicativos do **monday.com**, selecione **Logon único**.

    ![Opção Configurar logon único](common/select-sso.png)

1. No painel **Selecionar um método de logon único**, selecione o modo **SAML** ou **SAML/WS-Fed** para habilitar o logon único.

    ![Modo de seleção de logon único](common/select-saml-option.png)

1. No painel **Configurar logon único com SAML**, selecione **Editar** (o ícone de lápis) para abrir o painel **Configuração Básica de SAML**.

    ![Editar a Configuração Básica de SAML](common/edit-urls.png)

1. No painel **Configuração Básica do SAML**, se você tiver um arquivo de metadados do provedor de serviços e desejar configurar o *modo iniciado por IDP*, conclua as seguintes etapas:

    1. Selecione **Carregar o arquivo de metadados**.

       ![A opção Carregar o arquivo de metadados](common/upload-metadata.png)

    1. Para selecionar o arquivo de metadados, selecione o ícone de pasta e, em seguida, selecione **Carregar**.

       ![Selecione o arquivo de metadados e, em seguida, o botão Carregar](common/browse-upload-metadata.png)

    1. Depois que o arquivo de metadados for carregado com êxito, os valores de **Identificador** e **URL de Resposta** serão populados automaticamente no painel **Configuração Básica do SAML**:

       ![Os valores do IdP no painel Configuração Básica do SAML](common/idp-intiated.png)

       > [!Note]
       > Se os valores de **Identificador** e **URL de Resposta** não forem populados automaticamente, insira os valores manualmente.

1. Para configurar o aplicativo no *modo iniciado por SP*:

    1. Selecione **Definir URLs adicionais**.
    
    1. Na caixa **URL de Logon**, insira uma URL que tenha o seguinte padrão: https:\//\<domínio>.monday.com. Contate a [equipe de suporte ao cliente do monday.com](mailto:support@monday.com) para obter a URL de logon.

        ![A opção Definir URLs adicionais](common/metadata-upload-additional-signon.png)

1. O aplicativo monday.com espera que as declarações SAML estejam em um formato específico. Configure as declarações a seguir para este aplicativo. Para gerenciar esses valores de atributo, no painel **Configurar o Logon Único com o SAML**, selecione **Editar** para abrir o painel **Atributos de Usuário**.

    ![O painel Atributos de usuário](common/edit-attribute.png)

1. Em **Declarações de usuário**, selecione **Editar** para editar as declarações. Para adicionar uma declaração, selecione **Adicionar nova declaração** e, em seguida, configure o atributo do token SAML conforme mostrado na imagem anterior. Em seguida, conclua as seguintes etapas: 

    1. Selecione **Adicionar nova declaração**.

        ![A opção Adicionar nova declaração no painel Declarações de usuário](common/new-save-attribute.png)

    1. No painel **Gerenciar declarações de usuário**, defina os seguintes valores:
        
       1. Na caixa **Nome**, insira o nome do atributo mostrado para a linha de declaração de usuário.

       1. Deixe **Namespace** em branco.

       1. Para **Origem**, selecione **Atributo**.

       1. Na lista **Atributo de origem**, selecione o valor do atributo mostrado para a linha de declaração de usuário.

       1. Selecione **OK** e, em seguida, **Salvar**.

       ![O painel Gerenciar declarações de usuário](common/new-attribute-details.png)

1. No painel **Configurar o Logon Único com o SAML**, em **Certificado de Autenticação SAML**, selecione **Baixar** ao lado de **Certificado (Base64)** . Selecione uma opção de download com base em seus requisitos. Salve o certificado no computador.

    ![A opção de download do Certificado (Base64)](common/certificatebase64.png)

1. Na seção **Configurar o monday.com**, copie as seguintes URLs de acordo com suas necessidades:

    * URL de logon
    * Identificador do Azure AD
    * URL de logoff

    ![Copiar URLs de configuração](common/copy-configuration-urls.png)

### <a name="configure-mondaycom-single-sign-on"></a>Configurar o logon único do monday.com

Para configurar o logon único no lado do monday.com, envie o arquivo de Certificado (Base64) baixado e as URLs relevantes copiadas do portal do Azure para a [equipe de suporte do monday.com](mailto:support@monday.com). A equipe de suporte do monday.com usa as informações enviadas por você para garantir que a conexão de logon único do SAML seja definida corretamente em ambos os lados.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

Nesta seção, você criará uma usuária de teste no portal do Azure chamada Brenda Fernandes.

1. No portal do Azure, selecione **Azure Active Directory** > **Usuários** > **Todos os usuários**.

    ![As opções Usuários e Todos os usuários](common/users.png)

1. Selecione **Novo usuário**.

    ![A opção Novo usuário](common/new-user.png)

1. No painel **Usuário**, conclua as etapas a seguir:

    1. Na caixa **Nome**, insira **BrendaFernandes**.
  
    1. Na caixa **Nome de usuário**, insira **brendafernandes\@\<domíniodaempresa>.\<extensão>** . Por exemplo, **brendafernandes\@contoso.com**.

    1. Selecione a caixa de seleção **Mostrar Senha**. Anote o valor exibido na caixa **Senha**.

    1. Selecione **Criar**.

    ![O painel Usuário](common/user-properties.png)

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá a Brenda Fernandes o acesso ao monday.com para que ela possa usar o logon único do Azure.

1. No portal do Azure, selecione **Aplicativos empresariais** > **Todos os aplicativos** > **monday.com**.

    ![O painel Aplicativos Empresariais](common/enterprise-applications.png)

1. Na lista de aplicativos, escolha **monday.com**.

    ![monday.com na lista de aplicativos](common/all-applications.png)

1. No menu, selecione **Usuários e grupos**.

    ![A opção Usuários e grupos](common/users-groups-blade.png)

1. Selecione **Adicionar usuário**. Em seguida, no painel **Adicionar atribuição**, selecione **Usuários e grupos**.

    ![O painel Adicionar atribuição](common/add-assign-user.png)

1. No painel **Usuários e grupos**, selecione **Brenda Fernandes** na lista de usuários. Escolha **Selecionar**.

1. Se você estiver esperando um valor de função na declaração SAML, no painel **Selecionar função**, selecione a função relevante ao usuário na lista. Escolha **Selecionar**.

1. No painel **Adicionar atribuição**, selecione **Atribuir**.

### <a name="create-a-mondaycom-test-user"></a>Criar um usuário de teste do monday.com

Nesta seção, um usuário chamado Brenda Fernandes será criado no aplicativo monday.com. O monday.com é compatível com o provisionamento de usuário Just-In-Time, que é habilitado por padrão. Não há itens de ação para você nesta seção. Se um usuário ainda não existir no monday.com, um novo será criado após a autenticação.

### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará a configuração de logon único do Azure AD usando o portal Meus Aplicativos.

Depois de configurar o logon único, ao selecionar **monday.com** no portal Meus Aplicativos, você será automaticamente conectado ao monday.com. Para obter mais informações sobre o portal Meus Aplicativos, confira [Acessar e usar aplicativos no portal Meus Aplicativos](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Próximas etapas

Para saber mais, examine estes artigos:

- [Lista de tutoriais de integração de aplicativos SaaS ao Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [Logon único em aplicativos no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
