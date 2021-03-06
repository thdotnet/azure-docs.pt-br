---
title: "Tutorial: Integração do Azure Active Directory com o SpringCM | Microsoft Docs"
description: "Saiba como usar o Spring CM com o Active Directory do Azure para habilitar o logon único, provisionamento automatizado e muito mais!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 4a42f797-ac58-4aca-a8e6-53bfe5529083
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/08/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 451369e21e7471180b6cd8c77d62b157d0bcddff
ms.openlocfilehash: 95fbe26a9bb886c6edbb862c9e15885ffc5eeed6
ms.lasthandoff: 12/14/2016


---
# <a name="tutorial-azure-active-directory-integration-with-springcm"></a>Tutorial: Integração do Azure Active Directory com o SpringCM
O objetivo deste tutorial é mostrar como configurar o logon único entre o Active Directory do Azure e o SpringCM.

O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

* Uma assinatura válida do Azure
* Uma assinatura habilitada para logon único do SpringCM

Depois de concluir este tutorial, os usuários do Active Directory do Azure que você atribuiu ao SpringCM poderão fazer um logon único no aplicativo usando o Painel de Acesso do AAD.

1. Habilitando a integração de aplicativos para o SpringCM
2. Configurando o logon único
3. Configurando o provisionamento de usuários
4. Atribuindo usuários

![Cenário](./media/active-directory-saas-spring-cm-tutorial/IC797044.png "Cenário")

## <a name="enabling-the-application-integration-for-springcm"></a>Habilitando a integração de aplicativos para o SpringCM
O objetivo desta seção é descrever como habilitar a integração de aplicativos para o SpringCM.

### <a name="to-enable-the-application-integration-for-springcm-perform-the-following-steps"></a>Para habilitar a integração de aplicativos com o SpringCM, execute as seguintes etapas:
1. No Portal clássico do Azure, no painel de navegação à esquerda, clique em **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-spring-cm-tutorial/IC700993.png "Active Directory")

2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
    ![Aplicativos](./media/active-directory-saas-spring-cm-tutorial/IC700994.png "Aplicativos")

4. Clique em **Adicionar** na parte inferior da página.
   
    ![Adicionar aplicativo](./media/active-directory-saas-spring-cm-tutorial/IC749321.png "Adicionar aplicativo")

5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
    ![Adicionar um aplicativo da galeria](./media/active-directory-saas-spring-cm-tutorial/IC749322.png "Adicionar um aplicativo da galeria")

6. Na **caixa de pesquisa**, digite **SpringCM**.
   
    ![Galeria de Aplicativos](./media/active-directory-saas-spring-cm-tutorial/IC797045.png "Galeria de Aplicativos")

7. No painel de resultados, selecione **SpringCM** e clique em **Concluir** para adicionar o aplicativo.
   
    ![SpringCM](./media/active-directory-saas-spring-cm-tutorial/IC797046.png "SpringCM")

## <a name="configuring-single-sign-on"></a>Configurando o logon único
Esta descreve como permitir que os usuários se autentiquem no SpringCM com a respectiva conta do Active Directory do Azure usando federação baseada no protocolo SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Para configurar o logon único, execute as seguintes etapas:
1. No portal clássico do Azure, na página de integração do aplicativo **SpringCM**, clique em **Configurar logon único** para abrir a caixa de diálogo **Configurar Logon Único**.
   
    ![Configurar Logon Único](./media/active-directory-saas-spring-cm-tutorial/IC797047.png "Configurar Logon Único")

2. Na página **Como você deseja que os usuários façam logon no SpringCM**, selecione **Logon Único do Microsoft Azure AD** e clique em **Avançar**.
   
    ![Configurar Logon Único](./media/active-directory-saas-spring-cm-tutorial/IC797048.png "Configurar Logon Único")

3. Na página **Configurar a URL do Aplicativo**, na caixa de texto **URL de Logon do SpringCM**, digite a URL usada pelos usuários para fazer logon no seu aplicativo SpringCM e clique em **Avançar**. 
   
    A URL do aplicativo é a URL de locatário do SpringCM (por exemplo: *https://na11.springcm.com/atlas/SSO/SSOEndpoint.ashx?aid=16826*):
   
    ![Configurar URL do Aplicativo](./media/active-directory-saas-spring-cm-tutorial/IC797049.png "Configurar URL do Aplicativo")

4. Na página **Configurar logon único no SpringCM**, para baixar seu certificado, clique em **Baixar certificado** e salve o arquivo de certificado localmente no computador.
   
    ![Configurar Logon Único](./media/active-directory-saas-spring-cm-tutorial/IC797050.png "Configurar Logon Único")

5. Em outra janela do navegador da Web, entre em seu site de empresa do **SpringCM** como administrador.

6. No menu na parte superior, clique em **IR PARA**, clique em **Preferências** e, na seção **Preferências da Conta**, clique em **SSO do SAML**.
   
    ![SSO do SAML](./media/active-directory-saas-spring-cm-tutorial/IC797051.png "SSO do SAML")

7. Na seção Configuração do Provedor de Identidade, execute as seguintes etapas:
   
    ![Configuração do Provedor de Identidade](./media/active-directory-saas-spring-cm-tutorial/IC797052.png "Configuração do Provedor de Identidade")
   
    a. Para carregar seu certificado baixado do Azure Active Directory, clique em **Selecionar Certificado do Emissor** ou **Alterar Certificado do Emissor**.
   
    b. No portal clássico do Azure, na página **Configurar logon único no SpringCM**, copie o valor da **URL do Emissor** e cole-o na caixa de texto **Emissor**.
   
    c. No portal clássico do Azure, na página **Configurar logon único no SpringCM**, copie o valor da **URL do Serviço de Logon Único** e cole-o na caixa de texto **Ponto de Extremidade Iniciado pelo SP (Provedor de Serviços)**.
   
    d. Para **SAML Habilitado**, selecione **Habilitar**.
   
    e. Clique em **Salvar**.

8. No portal clássico do Azure, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar logon único**.
   
   ![Configurar Logon Único](./media/active-directory-saas-spring-cm-tutorial/IC797053.png "Configurar Logon Único")

## <a name="configuring-user-provisioning"></a>Configurando o provisionamento de usuários
Para permitir que os usuários do AD do Azure façam logon no SpringCM, eles deverão ser provisionados no SpringCM.  
No caso do SpringCM, o provisionamento será uma tarefa manual.

> [!NOTE]
> Para obter mais detalhes, veja [Criar e editar um usuário do SpringCM](http://knowledge.springcm.com/create-and-edit-a-springcm-user)
> 
> 

### <a name="to-provision-a-user-account-to-springcm-perform-the-following-steps"></a>Para provisionar uma conta de usuário no SpringCM, execute as seguintes etapas:
1. Faça logon em seu site de empresa do **SpringCM** como administrador.

2. Clique em **IR PARA** e clique em **Catálogo de Endereços**.
   
    ![Criar Usuário](./media/active-directory-saas-spring-cm-tutorial/IC797054.png "Criar Usuário")

3. Clique em **Criar Usuário**.

4. Selecione uma **Função de Usuário**.

5. Selecione **Enviar Email de Ativação**.

6. Digite o nome, o sobrenome e o endereço de email de uma conta de usuário válida do Active Directory do Azure que deseja provisionar nas caixas de texto relacionadas.

7. Adicione o usuário a um **Grupo de segurança**.

8. Clique em **Salvar**.

> [!NOTE]
> É possível usar qualquer outra ferramenta de criação da conta de usuário do SpringCM ou as APIs fornecidas pelo SpringCM para provisionar as contas de usuário do AAD.
> 
> 

## <a name="assigning-users"></a>Atribuindo usuários
Para testar sua configuração, é necessário conceder acesso ao aplicativo aos usuários do Azure AD que você deseja que usem seu aplicativo.

### <a name="to-assign-users-to-springcm-perform-the-following-steps"></a>Para atribuir usuários ao SpringCM, execute as seguintes etapas:
1. No Portal clássico do Azure, crie uma conta de teste.

2. Na página de integração do aplicativo **SpringCM**, clique em **Atribuir usuários**.
   
    ![Atribuir Usuários](./media/active-directory-saas-spring-cm-tutorial/IC797055.png "Atribuir Usuários")

3. Selecione seu usuário de teste, clique em **Atribuir** e, em seguida, clique em **Sim** para confirmar a atribuição.
   
    ![Sim](./media/active-directory-saas-spring-cm-tutorial/IC767830.png "Sim")

Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).


