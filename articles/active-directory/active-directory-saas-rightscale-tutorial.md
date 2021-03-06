---
title: "Tutorial: integração do Azure Active Directory com o RightScale | Microsoft Docs"
description: "Saiba como configurar o logon único entre o Azure Active Directory e o RightScale."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 3a8d376d-95fb-4dd7-832a-4fdd4dd7c87c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 451369e21e7471180b6cd8c77d62b157d0bcddff
ms.openlocfilehash: d9264e4268bb0418d64d4da680bd6e8b756a9ea6
ms.lasthandoff: 12/14/2016


---
# <a name="tutorial-azure-active-directory-integration-with-rightscale"></a>Tutorial: integração do Azure Active Directory com o RightScale
O objetivo desse tutorial é mostrar como integrar o RightScale ao Azure AD (Azure Active Directory).  
A integração do RightScale ao Azure AD oferece os seguintes benefícios:

* Você pode controlar no Azure AD quem tem acesso ao RightScale
* Você pode permitir que seus usuários façam logon automaticamente no RightScale (logon único) com as contas do Azure AD
* Gerenciar suas contas em um único local: o Portal clássico do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos
Para configurar a integração do Azure AD ao RightScale, você precisará dos seguintes itens:

* Uma assinatura do AD do Azure
* Uma assinatura habilitada para logon único do RightScale

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.
> 
> 

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

* Não use o ambiente de produção, a menos que seja necessário.
* Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
O objetivo deste tutorial é permitir que você teste o logon único do Azure AD em um ambiente de teste.  
O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o RightScale da galeria
2. Configurar e testar o logon único do AD do Azure

## <a name="adding-rightscale-from-the-gallery"></a>Adicionando o RightScale da galeria
Para configurar a integração do RightScale ao Azure AD, você precisa adicioná-lo da galeria à sua lista de aplicativos de SaaS gerenciados.

**Para adicionar o RightScale da galeria, execute as seguintes etapas:**

1. No **Portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**. 
   
    ![Active Directory][1]

2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
    ![Aplicativos][2]

4. Clique em **Adicionar** na parte inferior da página.
   
    ![Aplicativos][3]

5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
    ![Aplicativos][4]

6. Na caixa de pesquisa, digite **RightScale**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_01.png)

7. No painel de resultados, selecione **RightScale**, em seguida, clique em **Concluir** para adicionar o aplicativo.
   

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configurar e testar o logon único do AD do Azure
O objetivo desta seção é mostrar como configurar e testar o logon único do Azure AD com o RightScale, com base em um usuário de teste chamado "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do RightScale é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do RightScale.  

Para configurar e testar o logon único do Azure AD com o RightScale, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** - para testar o logon único do AD do Azure com Brenda Fernandes.
3. **[Criar um usuário de teste do RightScale](#creating-a-rightscale-test-user)** : para ter um equivalente de Brenda Fernandes no RightScale que esteja vinculado à representação dela no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** - para habilitar Britta Simon a usar o logon único do AD do Azure.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD
O objetivo desta seção é habilitar o logon único do Azure AD no Portal Clássico do Azure e configurar o logon único em seu aplicativo RightScale.

**Para configurar o logon único do Azure AD com o RightScale, execute as seguintes etapas:**

1. No portal clássico, na página de integração de aplicativos **RightScale**, clique em **Configurar logon único** para abrir a caixa de diálogo **Configurar Logon Único**.
   
    ![Configurar o logon único][6] 

2. Na página **Como você gostaria que os usuários fizessem logon no RightScale**, selecione **Logon Único do Azure AD**, em seguida, clique em **Próximo**.
   
    ![Configurar Logon Único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_03.png) 

3. Na página de diálogo **Definir Configurações de Aplicativo**, se quiser configurar o aplicativo no **modo iniciado pelo IDP**, execute as seguintes etapas e clique em **Avançar**:
   
    ![Configurar Logon Único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_04.png) 

    a. Na caixa de texto URL de Resposta, digite a URL no seguinte padrão: `https://login.rightscale.com/login/saml2/consume`

    b. Clique em **Próximo**

1. Se quiser configurar o aplicativo no **modo iniciado pelo SP**, na página de diálogo **Definir Configurações do Aplicativo**, clique em **"Mostrar configurações avançadas (opcional)"**, insira a **URL de Logon** e clique em **Avançar**.
   
    ![Configurar o logon único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_06.png) 
   
    a. Na caixa de texto URL de Logon, digite a URL usada pelos usuários para fazer logon em seu aplicativo do RightScale usando o seguinte padrão: `https://login.rightscale.com/`
   
    b. Clique em **Avançar**

2. Na página **Configurar logon único no RightScale**, execute as seguintes etapas e clique em **Próximo**:
   
    ![Configurar Logon Único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_05.png) 
   
    a. Clique em **Baixar certificado**e salve o arquivo de certificado codificado em base&64; no computador.
   
    b. Clique em **Próximo**.

3. Para configurar o SSO para o aplicativo, você precisa entrar no locatário do RightScale como administrador.
   
    a. No menu na parte superior, clique na guia **Configurações** e selecione **Logon Único**.
   
    ![Configurar Logon Único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_001.png) 
   
    b. Clique no botão "**novo**" para adicionar **Seus Provedores de Identidade do SAML**.
   
    ![Configurar Logon Único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_002.png) 
   
    c. Na caixa de texto do **Nome de Exibição**, insira o nome da sua empresa.
   
    ![Configurar Logon Único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_003.png) 
   
    d. Selecione **Permitir o SSO iniciado pelo RightScale usando uma dica de descoberta** e insira seu **nome de domínio** na caixa de texto abaixo.
   
    ![Configurar Logon Único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_004.png)
   
    e. Copie a URL de SSO do SAML do Azure AD para o **Ponto de Extremidade do SSO do SAML** no RightScale.
   
    ![Configurar Logon Único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_005.png)
   
    ![Configurar Logon Único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_006.png)
   
    f. Copie a ID da Entidade do Azure AD para **EntityID do SAML** no RightScale.
   
    ![Configurar Logon Único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_007.png)
   
    ![Configurar Logon Único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_008.png)
   
    g. Clique no botão **Navegador** para carregar o certificado que você baixou na etapa&4;.
   
    ![Configurar Logon Único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_009.png)
   
    h. Clique em **Salvar**.

4. No portal clássico, selecione a confirmação da configuração de logon único e clique em **Avançar**.
   
    ![Logon Único do AD do Azure][10]

5. Na página **Confirmação de logon único**, clique em **Concluir**.  
   
    ![Logon Único do AD do Azure][11]

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal Clássico do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][20]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-rightscale-tutorial/create_aaduser_09.png) 

2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para exibir a lista de usuários, no menu na parte superior, clique em **Usuários**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-rightscale-tutorial/create_aaduser_03.png) 

4. Para abrir a caixa de diálogo **Adicionar Usuário**, na barra de ferramentas na parte inferior, clique em **Adicionar Usuário**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-rightscale-tutorial/create_aaduser_04.png) 

5. Na página do diálogo **Conte-nos sobre este usuário** , realize as seguintes etapas:
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-rightscale-tutorial/create_aaduser_05.png) 
   
    a. Em **Tipo de Usuário**, selecione **Novo usuário na organização**.
   
    b. Na caixa de texto **Nome** do Usuário, digite **BrendaFernandes**.
   
    c. Clique em **Próximo**.

6. Na página do diálogo **Perfil do Usuário** , realize as seguintes etapas:
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-rightscale-tutorial/create_aaduser_06.png) 
   
    a. Na caixa de texto **Nome**, digite **Brenda**.  
   
    b. Na caixa de texto **Sobrenome**, digite **Fernandes**.
   
    c. Na caixa de texto **Nome de Exibição**, digite **Brenda Fernandes**.
   
    d. Na lista **Função**, selecione **Usuário**.
   
    e. Clique em **Próximo**.

7. Na página de diálogo **Obter senha temporária**, clique em **criar**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-rightscale-tutorial/create_aaduser_07.png) 

8. Na página de caixa de diálogo **Obter senha temporária** , execute as seguintes etapas:
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-rightscale-tutorial/create_aaduser_08.png) 
   
    a. Anote o valor da **Nova Senha**.
   
    b. Clique em **Concluído**.   

### <a name="creating-a-rightscale-test-user"></a>Criar um usuário de teste do RightScale
Nesta seção, você criará um usuário chamado Brenda Fernandes no RightScale. Trabalhe com a equipe de suporte do RightScale via support@rightscale.com para adicionar os usuários à plataforma RightScale.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure
O objetivo desta seção é permitir que Brenda Fernandes use o logon único do Azure, concedendo a ela acesso ao RightScale.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao RightScale, execute as seguintes etapas:**

1. No portal clássico, para abrir a exibição de aplicativos, na exibição de diretório, clique em **Aplicativos** no menu superior.
   
    ![Atribuir usuário][201] 

2. Na lista de aplicativos, escolha **RightScale**.
   
    ![Configurar Logon Único](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_50.png) 

3. No menu na parte superior, clique em **Usuários**.
   
    ![Atribuir usuário][203] 

4. Na lista de usuários, selecione **Brenda Fernandes**.

5. Na barra de ferramentas na parte inferior, clique em **Atribuir**.
   
    ![Atribuir usuário][205]

### <a name="testing-single-sign-on"></a>Teste do logon único
O objetivo desta seção é testar sua configuração de logon único do Azure AD usando o Painel de Acesso.  
Ao clicar no bloco do RightScale no Painel de Acesso, você deverá ser conectado automaticamente ao seu aplicativo do RightScale.

## <a name="additional-resources"></a>Recursos adicionais
* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_205.png

