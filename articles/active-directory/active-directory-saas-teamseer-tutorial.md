---
title: "Tutorial: Integração do Azure Active Directory com o TeamSeer | Microsoft Docs"
description: "Saiba como usar o TeamSeer com o Active Directory do Azure para habilitar o logon único, provisionamento automatizado e muito mais!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 6ec4806f-fe0f-4ed7-8cfa-32d1c840433f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 49352a5e8255468bbc54c02e0cd9242d49002dbd
ms.openlocfilehash: 75bd875a4ca7bbfd4d5a1cd9ac6bde302430136a
ms.lasthandoff: 12/08/2016


---
# <a name="tutorial-azure-active-directory-integration-with-teamseer"></a>Tutorial: Integração do Active Directory do Azure ao TeamSeer
O objetivo deste tutorial é mostrar a integração do Azure ao TeamSeer.  
O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

* Uma assinatura válida do Azure
* Um locatário do TeamSeer

Depois de concluir este tutorial, os usuários do Azure AD atribuídos ao TeamSeer poderão fazer logon único no aplicativo em seu site de empresa do TeamSeer (logon iniciado pelo provedor de serviços) ou usando a [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).

O cenário descrito neste tutorial consiste nos seguintes blocos de construção:

1. Habilitando a integração de aplicativos para o TeamSeer
2. Configurando o logon único
3. Configurando o provisionamento de usuários
4. Atribuindo usuários

![Cenário](./media/active-directory-saas-teamseer-tutorial/IC789618.png "Cenário")

## <a name="enabling-the-application-integration-for-teamseer"></a>Habilitando a integração de aplicativos para o TeamSeer
O objetivo desta seção é descrever como habilitar a integração de aplicativos para o TeamSeer.

### <a name="to-enable-the-application-integration-for-teamseer-perform-the-following-steps"></a>Para habilitar a integração de aplicativos com o TeamSeer, execute as seguintes etapas:
1. No Portal clássico do Azure, no painel de navegação à esquerda, clique em **Active Directory**.
   
    ![Active Directory](./media/active-directory-saas-teamseer-tutorial/IC700993.png "Active Directory")

2. Na lista **Diretório** , selecione o diretório para o qual você deseja habilitar a integração de diretórios.

3. Para abrir a visualização dos aplicativos, na exibição do diretório, clique em **Aplicativos** no menu principal.
   
    ![Aplicativos](./media/active-directory-saas-teamseer-tutorial/IC700994.png "Aplicativos")

4. Clique em **Adicionar** na parte inferior da página.
   
    ![Adicionar aplicativo](./media/active-directory-saas-teamseer-tutorial/IC749321.png "Adicionar aplicativo")

5. Na caixa de diálogo **O que você deseja fazer**, clique em **Adicionar um aplicativo da galeria**.
   
    ![Adicionar um aplicativo da galeria](./media/active-directory-saas-teamseer-tutorial/IC749322.png "Adicionar um aplicativo da galeria")

6. Na **caixa de pesquisa**, digite **TeamSeer**.
   
    ![Galeria de Aplicativos](./media/active-directory-saas-teamseer-tutorial/IC789619.png "Galeria de Aplicativos")

7. No painel de resultados, selecione **TeamSeer** e clique em **Concluir** para adicionar o aplicativo.
   
    ![TeamSeer](./media/active-directory-saas-teamseer-tutorial/IC789620.png "TeamSeer")

## <a name="configuring-single-sign-on"></a>Configurando o logon único
O objetivo desta seção é descrever como permitir que os usuários se autentiquem no TeamSeer com sua conta do AD do Azure usando federação baseada em protocolo SAML.  
Como parte desse procedimento, será necessário criar um arquivo de certificado codificado em base&64;.  
Se você não estiver familiarizado com este procedimento, consulte [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o).

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Para configurar o logon único, execute as seguintes etapas:
1. No portal clássico do Azure, na página de integração de aplicativos do **TeamSeer**, clique em **Configurar logon único** para abrir a caixa de diálogo **Configurar Logon Único**.
   
    ![Configurar Logon Único](./media/active-directory-saas-teamseer-tutorial/IC789621.png "Configurar Logon Único")

2. Na página **Como você deseja que os usuários façam logon no TeamSeer**, selecione **Logon Único do Microsoft Azure AD** e clique em **Avançar**.
   
    ![Configurar Logon Único](./media/active-directory-saas-teamseer-tutorial/IC789628.png "Configurar Logon Único")

3. Na página **Configurar a URL do Aplicativo**, na caixa de texto **URL de Entrada do TeamSeer**, digite a URL usando o padrão “*http://www.teamseer.com/companyid*” e clique em **Avançar**.
   
    ![Configurar URL do Aplicativo](./media/active-directory-saas-teamseer-tutorial/IC789629.png "Configurar URL do Aplicativo")

4. Na página **Configurar logon único no TeamSeer**, para baixar o certificado, clique em **Baixar certificado** e salve o arquivo de certificado no computador.
   
    ![Configurar Logon Único](./media/active-directory-saas-teamseer-tutorial/IC789630.png "Configurar Logon Único")

5. Em outra janela do navegador da Web, faça logon em seu site de empresa TeamSeer como um administrador.

6. Vá para **Administrador de RH**.
   
    ![Administrador de RH](./media/active-directory-saas-teamseer-tutorial/IC789634.png "Administrador de RH")

7. Clique em **Instalação**.
   
    ![Configuração](./media/active-directory-saas-teamseer-tutorial/IC789635.png "Configuração")

8. Clique em **Configurar detalhes do provedor de SAML**.
   
    ![Configurações do SAML](./media/active-directory-saas-teamseer-tutorial/IC789636.png "Configurações do SAML")

9. Na seção de detalhes do provedor de SAML, execute as seguintes etapas:
   
    ![Configurações do SAML](./media/active-directory-saas-teamseer-tutorial/IC789637.png "Configurações do SAML")
   
    a. No portal clássico do Azure, na página da caixa de diálogo **Configurar logon único no TeamSeer**, copie o valor de **URL de Serviço de Logon Único** e cole-o na caixa de texto **URL**.
   
    b. Crie um arquivo **codificado em base&64;** usando o certificado baixado.  
      
    > [!TIP]
    > Para obter mais detalhes, consulte [Como converter um certificado binário em um arquivo de texto](http://youtu.be/PlgrzUZ-Y1o)
    > 
    > 
   
    c. Abra seu certificado codificado em Base&64; no bloco de notas, copie o conteúdo dele na área de transferência e cole-o na caixa de texto **Certificado Público do IdP** .

10. Para concluir a configuração do provedor de SAML, execute as seguintes etapas:
    
    ![Configurações do SAML](./media/active-directory-saas-teamseer-tutorial/IC789638.png "Configurações do SAML")
    
    a. Nos **Endereços de Email de Teste**, digite o endereço de email do usuário de teste.
    
    b. Na caixa de texto **Emissor** , digite a URL do Emissor do provedor de serviços.
    
    c. Clique em **Salvar**.

11. No portal clássico do Azure, selecione a confirmação da configuração de logon único e clique em **Concluir** para fechar a caixa de diálogo **Configurar logon único**.
    
    ![Configurar Logon Único](./media/active-directory-saas-teamseer-tutorial/IC789639.png "Configurar Logon Único")

## <a name="configuring-user-provisioning"></a>Configurando o provisionamento de usuários
Para permitir que os usuários do AD do Azure façam logon no TeamSeer, eles deverão ser provisionados no ShiftPlanning.  
No caso do TeamSeer, o provisionamento é uma tarefa manual.

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a>Para provisionar contas de usuário, execute as seguintes etapas:
1. Faça logon em seu site de empresa do **TeamSeer** como administrador.

2. Execute as seguintes etapas:
   
    ![Administrador de RH](./media/active-directory-saas-teamseer-tutorial/IC789640.png "Administrador de RH")
   
    a. Vá para **Administrador de RH \> Usuários**.
   
    b. Clique em **Executar o assistente de Novo Usuário**.

3. Na seção **Detalhes do Usuário** , realize as seguintes etapas:
   
   ![Detalhes do Usuário](./media/active-directory-saas-teamseer-tutorial/IC789641.png "Detalhes do Usuário")
   
    a. Digite o **Nome**, **Sobrenome** e **Nome de usuário (Endereço de email)** de uma conta válida do AAD que você deseja provisionar nas caixas de texto relacionadas.
  
    b. Clique em **Próximo**.

4. Siga as instruções na tela para adicionar um novo usuário e clique em **Concluir**.

> [!NOTE]
> Você pode usar qualquer outra ferramenta de criação da conta de usuário do TeamSeer ou APIs fornecidas pelo TeamSeer para provisionar as contas de usuário do AD do Azure.
> 
> 

## <a name="assigning-users"></a>Atribuindo usuários
Para testar sua configuração, é necessário conceder acesso ao aplicativo aos usuários do Azure AD que você deseja que usem seu aplicativo.

### <a name="to-assign-users-to-teamseer-perform-the-following-steps"></a>Para atribuir usuários ao TeamSeer, execute as seguintes etapas:
1. No Portal clássico do Azure, crie uma conta de teste.

2. Na página de integração de aplicativos do **TeamSeer**, clique em **Atribuir usuários**.
   
    ![Atribuir Usuários](./media/active-directory-saas-teamseer-tutorial/IC789642.png "Atribuir Usuários")

3. Selecione seu usuário de teste, clique em **Atribuir** e, em seguida, clique em **Sim** para confirmar a atribuição.
   
    ![Sim](./media/active-directory-saas-teamseer-tutorial/IC767830.png "Sim")

Se você quiser testar suas configurações de logon único, abra o Painel de Acesso. Para obter mais detalhes sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](active-directory-saas-access-panel-introduction.md).


