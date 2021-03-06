---
title: "Tutorial: Integração do Azure Active Directory com o Zoho Mail | Microsoft Docs"
description: "Saiba como usar o Zoho Mail com o Active Directory do Azure para habilitar logon único, provisionamento automatizado e muito mais."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 9874e1f3-ade5-42e7-a700-e08b3731236a
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: fd79688a66c2b3919b11c0b06d268b60e1d93a8f
ms.openlocfilehash: 60165d0cd7fea3cd0861f36d9a4a245cedabe07a
ms.lasthandoff: 02/24/2017


---
# <a name="tutorial-azure-active-directory-integration-with-zoho-mail"></a>Tutorial: Integração do Active Directory do Azure com o Zoho Mail
O objetivo deste tutorial é mostrar a integração do Azure com o Zoho Mail.  
O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

* Uma assinatura válida do Azure
* Um locatário do Zoho Mail

Depois de concluir este tutorial, os usuários do Azure AD atribuídos ao Zoho Mail poderão fazer logon único no aplicativo em seu site de empresa do Zoho Mail (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1. Habilitando a integração do aplicativo com o Zoho Mail
2. Configurando o logon único
3. Configurando o provisionamento de usuários
4. Atribuindo usuários

![Cenário](./media/active-directory-saas-zoho-mail-tutorial/IC789600.png "Cenário")

## <a name="enabling-the-application-integration-for-zoho-mail"></a>Habilitando a integração do aplicativo com o Zoho Mail
O objetivo desta seção é descrever como habilitar a integração de aplicativos com o Zoho Mail.

### <a name="to-enable-the-application-integration-for-zoho-mail-perform-the-following-steps"></a>Para habilitar a integração de aplicativos com o Zoho Mail, execute as seguintes etapas:
1. No Portal clássico do Azure, no painel de navegação à esquerda, clique em **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-zoho-mail-tutorial/IC700993.png "Active Directory")

2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
    ![Aplicativos](./media/active-directory-saas-zoho-mail-tutorial/IC700994.png "Aplicativos")

4. Clique em **Adicionar** na parte inferior da página.
   
    ![Adicionar aplicativo](./media/active-directory-saas-zoho-mail-tutorial/IC749321.png "Adicionar aplicativo")

5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
    ![Adicionar um aplicativo da galeria](./media/active-directory-saas-zoho-mail-tutorial/IC749322.png "Adicionar um aplicativo da galeria")

6. Na **caixa de pesquisa**, digite **Zoho Mail**.
   
    ![Galeria de Aplicativos](./media/active-directory-saas-zoho-mail-tutorial/IC789601.png "Galeria de Aplicativos")

7. No painel de resultados, selecione **Zoho Mail** e clique em **Concluir** para adicionar o aplicativo.
   
    ![Zoho Mail](./media/active-directory-saas-zoho-mail-tutorial/IC789602.png "Zoho Mail")

## <a name="configuring-single-sign-on"></a>Configurando o logon único
O objetivo desta seção é descrever como permitir que os usuários se autentiquem no Zoho Mail com sua conta do AD do Azure usando federação baseada em protocolo SAML.  
Como parte desse procedimento, será necessário criar um arquivo de certificado codificado em base&64;.  
Se você não estiver familiarizado com este procedimento, consulte [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o).

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Para configurar o logon único, execute as seguintes etapas:
1. No portal clássico do Azure, na página de integração do aplicativo **Zoho Mail**, clique em **Configurar logon único** para abrir a caixa de diálogo **Configurar Logon Único**.
   
    ![Configurar Logon Único](./media/active-directory-saas-zoho-mail-tutorial/IC789603.png "Configurar Logon Único")

2. Na página **Como você deseja que os usuários façam logon no Zoho Mail**, selecione **Logon Único do Microsoft Azure AD** e clique em **Avançar**.
   
    ![Configurar Logon Único](./media/active-directory-saas-zoho-mail-tutorial/IC789604.png "Configurar Logon Único")

3. Na página **Configurar URL do Aplicativo** , execute as seguintes etapas:
   
    ![Configurar URL do Aplicativo](./media/active-directory-saas-zoho-mail-tutorial/IC789605.png "Configurar URL do Aplicativo")
   
    a. Na caixa de texto **URL de Entrada no Zoho Mail**, digite a URL usando o seguinte padrão: `http://<company name>.ZohoMail.com`
   
    b. Clique em **Próximo**.

4. Na página **Configurar logon único no Zoho Mail**, clique em **Baixar certificado** e salve o arquivo de certificado no computador.
   
    ![Configurar Logon Único](./media/active-directory-saas-zoho-mail-tutorial/IC789606.png "Configurar Logon Único")

5. Em uma janela diferente do navegador da Web, faça logon no site da sua empresa Zoho Mail como administrador.

6. Vá para o **Painel de Controle**.
   
    ![Painel de controle](./media/active-directory-saas-zoho-mail-tutorial/IC789607.png "Painel de controle")

7. Clique na guia **Autenticação SAML** .
   
    ![Autenticação SAML](./media/active-directory-saas-zoho-mail-tutorial/IC789608.png "Autenticação SAML")

8. Na seção **Detalhes da Autenticação SAML** , realize as seguintes etapas:
   
    ![Detalhes de Autenticação SAML](./media/active-directory-saas-zoho-mail-tutorial/IC789609.png "Detalhes de Autenticação SAML")
   
    a. No portal clássico do Azure, na página do diálogo **Configurar logon único no Zoho Mail**, copie o valor da **URL de Logon Remoto** e cole-o na caixa de texto **URL de Logon**.
   
    b. No portal clássico do Azure, na página do diálogo **Configurar logon único no Zoho Mail**, copie o valor da **URL de Logoff Remoto** e cole-o na caixa de texto **URL de Logoff**.
   
    c. No portal clássico do Azure, na página do diálogo **Configurar logon único no Zoho Mail**, copie o valor de **Alterar a URL da Senha** e cole-o na caixa de texto **Alterar a URL da Senha**.
   
    d. Crie um arquivo **codificado em base&64;** usando o certificado baixado.  
      
    > [!TIP]
    > Para obter mais detalhes, confira [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o)
    > 
    > 
   
    e. Abra seu certificado codificado em Base&64; no bloco de notas, copie o conteúdo dele na área de transferência e cole-o na caixa de texto **PublicKey** .
   
    f. Para **Algoritmo**, selecione **RSA**.
   
    g. Clique em **OK**.

9. No portal clássico do Azure, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar logon único**.
   
    ![Configurar Logon Único](./media/active-directory-saas-zoho-mail-tutorial/IC789610.png "Configurar Logon Único")

## <a name="configuring-user-provisioning"></a>Configurando o provisionamento de usuários
Para permitir que os usuários do AD do Azure façam logon no Zoho Mail, eles devem ser provisionados no Zoho Mail.  
No caso do Zoho Mail, o provisionamento é uma tarefa manual.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Para provisionar contas de usuário, execute as seguintes etapas:
1. Faça logon em seu site de empresa do **Zoho Mail** como administrador.

2. Vá para **Painel de Controle \> Emails e Documentos**.

3. Vá para **Detalhes do Usuário \> Adicionar Usuário**.
   
    ![Adicionar Usuário](./media/active-directory-saas-zoho-mail-tutorial/IC789611.png "Adicionar Usuário")

4. No diálogo **Adicionar usuários** , realize as seguintes etapas:
   
    ![Adicionar Usuário](./media/active-directory-saas-zoho-mail-tutorial/IC789612.png "Adicionar Usuário")
   
    a. Preencha os campos **Nome**, **Sobrenome**, **ID do Email** e **Senha** de uma conta válida do Azure Active Directory que você deseja provisionar nas caixas de texto relacionadas.
   
    b. Clique em **OK**.  
      
    > [!NOTE]
    > O titular da conta do Active Directory do Azure receberá um email com um link para confirmar a conta antes que ela se torne ativa.
    > 
    > 

> [!NOTE]
> É possível usar qualquer outra ferramenta de criação da conta de usuário do Zoho Mail ou APIs fornecidas pelo Zoho Mail para provisionar as contas de usuário do AAD.
> 
> 

## <a name="assigning-users"></a>Atribuindo usuários
Para testar sua configuração, é necessário conceder acesso ao aplicativo aos usuários do Azure AD que você deseja que usem seu aplicativo.

### <a name="to-assign-users-to-zoho-mail-perform-the-following-steps"></a>Para atribuir usuários ao Zoho Mail, execute as seguintes etapas:
1. No Portal clássico do Azure, crie uma conta de teste.

2. Na página de integração do aplicativo **Zoho Mail**, cique em **Atribuir usuários**.
   
    ![Atribuir Usuários](./media/active-directory-saas-zoho-mail-tutorial/IC789613.png "Atribuir Usuários")

3. Selecione seu usuário de teste, clique em **Atribuir** e, em seguida, clique em **Sim** para confirmar a atribuição.
   
    ![Sim](./media/active-directory-saas-zoho-mail-tutorial/IC767830.png "Sim")

Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).


