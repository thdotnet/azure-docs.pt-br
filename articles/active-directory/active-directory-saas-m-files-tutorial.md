---
title: "Tutorial: integração do Azure Active Directory com o M-Files | Microsoft Docs"
description: "Saiba como configurar o logon único entre o Azure Active Directory e o M-Files."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 4536fd49-3a65-4cff-9620-860904f726d0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/24/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 4a07f946d396a9263d5b00202cd5229ddc86d1be
ms.openlocfilehash: 78f0a065b675326a9a507e0cf480bc779fd458a9
ms.lasthandoff: 03/01/2017


---
# <a name="tutorial-azure-active-directory-integration-with-m-files"></a>Tutorial: integração do Azure Active Directory com o M-Files
Neste tutorial, você aprenderá a integrar o M-Files ao Azure AD (Azure Active Directory).

A integração do M-Files ao Azure AD oferece os seguintes benefícios:

* No Azure AD, você pode controlar quem tem acesso ao M-Files
* Você pode permitir que os usuários entrem automaticamente no M-Files usando SSO (logon único) com suas contas do Azure AD
* Gerenciar suas contas em um único local: o Portal clássico do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Pré-requisitos
Para configurar a integração do Azure AD com o M-Files, você precisará dos seguintes itens:

* Uma assinatura do AD do Azure
* Uma assinatura do **M-Files** com SSO habilitado

>[!NOTE]
>Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção. 
> 

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

* Não use o ambiente de produção, a menos que seja necessário.
* Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o M-Files da galeria
2. Configurar e testar o SSO do Azure AD

## <a name="adding-m-files-from-the-gallery"></a>Adicionando o M-Files da galeria
Para configurar a integração do M-Files ao Azure AD, você precisará adicionar o M-Files da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o M-Files da galeria, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**. 
   
    ![Active Directory][1]
2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.
3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
    ![Aplicativos][2]
4. Clique em **Adicionar** na parte inferior da página.
   
    ![Aplicativos][3]
5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
    ![Aplicativos][4]
6. Na caixa de pesquisa, digite **M-Files**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_01.png)
7. No painel de resultados, selecione **M-Files** e clique em **Concluir** para adicionar o aplicativo.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_02.png)

## <a name="configure-and-test-azure-ad-sso"></a>Configurar e testar SSO do Azure AD
Nesta seção, você configurará e testará o logon único (SSO) do Azure AD com o M-Files com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o SSO funcione, o Azure AD precisa saber qual usuário do M-Files é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do M-Files.

Essa relação de vínculo é estabelecida atribuindo o valor de **nome de usuário** ao Azure AD como sendo o valor de **Nome de usuário** no M-Files. Para configurar e testar o logon único do Azure AD com o M-Files, você precisa concluir os seguintes blocos de construção:

1. **[Configurar logon único do Azure AD](#configuring-azure-ad-single-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
2. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** - para testar o logon único do AD do Azure com Brenda Fernandes.
3. **[Criando um usuário de teste do M-Files](#creating-a-m-file-test-user)**: para ter um equivalente de Brenda Fernandes no M-Files que esteja vinculado à representação dela no Azure AD.
4. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** - para habilitar Britta Simon a usar o logon único do Azure AD.
5. **[Teste do logon único](#testing-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configuring-azure-ad-sso"></a>Configurar o SSO do Azure AD
O objetivo desta seção é habilitar o SSO do Azure AD no Portal Clássico do Azure e configurar o SSO em seu aplicativo M-Files.

**Para configurar o SSO do Azure AD com o M-Files, execute as seguintes etapas:**

1. No portal clássico do Azure, na página de integração do aplicativo **M-Files**, clique em **Configurar logon único** para abrir o diálogo **Configurar Logon Único**.
   
    ![Configurar Logon Único][7] 
2. Na página **Como você deseja que os usuários entrem no M-Files**, selecione **Logon Único do Azure AD** e clique em **Avançar**.
   
    ![Configurar Logon Único](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_06.png)
3. Na página de diálogo **Definir Configurações de Aplicativo** , execute as seguintes etapas:
   
    ![Configurar Logon Único](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_07.png)
  1. Na caixa de texto URL de Entrada, digite uma URL usando o seguinte padrão: `https://<tenant-name>.cloudvault.m-files.com/authentication/MFiles.AuthenticationProviders.Core/sso`
  2. Clique em **Próximo**.
4. Na página **Configurar o logon único no M-Files**, clique em **Baixar metadados** e salve o arquivo de metadados no computador.
   
    ![Configurar Logon Único](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_09.png)
5. Para que o SSO seja configurado para o aplicativo, entre em contato com sua equipe de suporte do M-Files em <mailto:support@m-files.com> e forneça os metadados baixados.
   
   >[!NOTE]
   >Siga as próximas etapas se quiser configurar o SSO para o aplicativo da área de trabalho do M-Files. Nenhuma etapa adicional é necessária se você quer apenas configurar o SSO para versão Web do M-Files.  
   > 
6. Siga as próximas etapas para configurar o aplicativo da área de trabalho do M-Files a fim de habilitar o SSO com o Azure AD. Para baixar o M-Files, vá para a página [Baixar M-Files](https://www.m-files.com/en/download-latest-version).
7. Abra a janela **Configurações da área de trabalho do M-Files**. Em seguida, clique em **Adicionar**.
   
    ![Configurar Logon Único](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_10.png)
8. Na janela **Propriedades de Conexão do Cofre de Documentos**, execute as seguintes etapas:
   
  ![Configurar Logon Único](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_11.png)  
    Na seção **Servidor**, digite os valores abaixo:  
 1. Para **Nome**, digite `<tenant-name>.cloudvault.m-files.com`.  
 2. Para **Número de Porta**, digite **4466**. 
 3. Para **Protocolo**, selecione **HTTPS**. 
 4. No campo **Autenticação**, selecione **Usuário específico do Windows**. Em seguida, você verá uma página de entrada. Insira suas credenciais do Azure AD. 
 5. Para o **Cofre no Servidor**, selecione o cofre correspondente no servidor. 
 6. Clique em **OK**.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD
Nesta seção, você criará uma usuária de teste no portal clássico chamada Brenda Fernandes.

![Criar um usuário do AD do Azure][20]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **portal clássico do Azure**, no painel de navegação à esquerda, clique em **Active Directory**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-m-files-tutorial/create_aaduser_09.png) 
2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.
3. Para exibir a lista de usuários, no menu na parte superior, clique em **Usuários**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-m-files-tutorial/create_aaduser_03.png) 
4. Para abrir a caixa de diálogo **Adicionar Usuário**, na barra de ferramentas na parte inferior, clique em **Adicionar Usuário**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-m-files-tutorial/create_aaduser_04.png) 
5. Na página do diálogo **Conte-nos sobre este usuário** , realize as seguintes etapas:
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-m-files-tutorial/create_aaduser_05.png) 
 1. Em Tipo de Usuário, selecione Novo usuário na organização.  
 2. Na **caixa de texto** Nome do Usuário, digite **BrendaFernandes**. 
 3. Clique em **Próximo**.
6. Na página do diálogo **Perfil do Usuário** , realize as seguintes etapas:
   
   ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-m-files-tutorial/create_aaduser_06.png)  
 1. Na caixa de texto **Nome**, digite **Brenda**.   
 2. Na caixa de texto **Sobrenome**, digite **Fernandes**. 
 3. Na caixa de texto **Nome de Exibição**, digite **Brenda Fernandes**. 
 4. Na lista **Função**, selecione **Usuário**. 
 5. Clique em **Próximo**.
7. Na página de diálogo **Obter senha temporária**, clique em **criar**.
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-m-files-tutorial/create_aaduser_07.png) 
8. Na página de caixa de diálogo **Obter senha temporária** , execute as seguintes etapas:
   
    ![Criação de um usuário de teste do AD do Azure](./media/active-directory-saas-m-files-tutorial/create_aaduser_08.png)  
 1. Anote o valor da **Nova Senha**. 
 2. Clique em **Concluído**.   

### <a name="create-a-m-files-test-user"></a>Criar um usuário de teste do M-Files
Nesta seção, você criará uma usuária chamada Brenda Fernandes no M-Files. Se você não sabe como criar um usuário no M-Files, entre em contato com o suporte do M-Files em <mailto:support@m-files.com>.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD
Nesta seção, você permitirá que Brenda Fernandes use o logon único (SSO) do Azure concedendo-lhe acesso ao M-Files.

![Atribuir usuário][200]

**Para atribuir Brenda Fernandes ao M-Files, execute as seguintes etapas:**

1. No portal clássico, para abrir o modo de exibição de aplicativos, no modo de exibição de diretório, clique em **Aplicativos** no menu superior.
   
    ![Atribuir usuário][201] 
2. Na lista de aplicativos, selecione **M-Files**.
   
    ![Configurar Logon Único](./media/active-directory-saas-m-files-tutorial/tutorial_m_files_12.png)
3. No menu na parte superior, clique em **Usuários**.
   
    ![Atribuir usuário][203] 
4. Na lista Todos os Usuários, escolha **Brenda Fernandes**.
5. Na barra de ferramentas na parte inferior, clique em **Atribuir**.
   
    ![Atribuir usuário][205]

### <a name="test-single-sign-on"></a>Testar logon único
O objetivo desta seção é testar sua configuração de SSO do Azure AD usando o Painel de Acesso.

Ao clicar no bloco M-Files no Painel de Acesso, você deverá ser conectado automaticamente ao aplicativo M-Files.

## <a name="additional-resources"></a>Recursos adicionais
* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](active-directory-saas-tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-m-files-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-m-files-tutorial/tutorial_general_205.png

