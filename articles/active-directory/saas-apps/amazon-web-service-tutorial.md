---
title: 'Tutorial: Integração do Azure Active Directory à AWS (Amazon Web Services) | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o AWS (Amazon Web Services).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/30/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f346c995cbc8be6e609020db799959d873ce89b3
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68944948"
---
# <a name="tutorial-integrate-amazon-web-services-aws-with-azure-active-directory"></a>Tutorial: integração da AWS (Amazon Web Services) ao Azure Active Directory

Neste tutorial, você aprenderá a integrar a AWS (Amazon Web Services) ao Azure AD (Azure Active Directory). Quando integrar o AWS ao Azure AD, você poderá:

* Controlar no Azure AD quem tem acesso ao AWS.
* Permitir que os usuários entrem automaticamente no AWS com suas contas do Azure AD.
* gerenciar suas contas em um local central, o portal do Azure.

Para saber mais sobre a integração de aplicativos SaaS ao Azure AD, confira [O que é o acesso de aplicativos e o logon único com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

![Diagrama do relacionamento do Azure AD e do AWS](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

Você pode configurar vários identificadores para várias instâncias. Por exemplo:

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Com esses valores, o Azure AD remove o valor de **#** e envia o valor correto `https://signin.aws.amazon.com/saml` como a URL de público no token SAML.

É recomendável essa abordagem pelos seguintes motivos:

- Cada aplicativo fornece a você um certificado X509 exclusivo. Cada instância de uma instância de aplicativo da AWS pode ter uma data de expiração do certificado diferente, que pode ser gerenciada individualmente por conta da AWS. A rolagem geral do certificado é mais fácil nesse caso.

- Você pode ativar o aprovisionamento de usuário com um aplicativo da AWS no Azure AD e, em seguida, nosso serviço busca todas as funções dessa conta da AWS. Você não precisa adicionar ou atualizar manualmente as funções da AWS no aplicativo.

- Você pode atribuir o proprietário do aplicativo individualmente para o aplicativo. Essa pessoa pode gerenciar o aplicativo diretamente no Azure AD.

> [!Note]
> Use apenas o aplicativo da galeria.

## <a name="prerequisites"></a>Pré-requisitos

Para começar, você precisará dos seguintes itens:

* Uma assinatura do Azure AD. Caso você não tenha uma assinatura, obtenha uma [conta gratuita](https://azure.microsoft.com/free/).
* Assinatura habilitada para SSO (logon único) do AWS.

## <a name="scenario-description"></a>Descrição do cenário

Neste tutorial, você configurará e testará o SSO do Azure AD em um ambiente de teste. O AWS dá suporte ao SSO iniciado por **SP e IDP**.

## <a name="add-aws-from-the-gallery"></a>Adicionar o AWS da galeria

Para configurar a integração do AWS ao Azure AD, você precisa adicionar o AWS por meio da galeria à sua lista de aplicativos SaaS gerenciados.

1. Entre no [portal do Azure](https://portal.azure.com) usando uma conta corporativa ou de estudante, ou uma conta pessoal da Microsoft.
1. No painel esquerdo, selecione o serviço **Azure Active Directory**.
1. Vá para **Aplicativos da empresa**, em seguida, selecione **Todos os Aplicativos**.
1. Para adicionar um novo aplicativo, escolha **Novo aplicativo**.
1. Na seção **Adicionar por meio da galeria**, digite **Amazon Web Services (AWS)** na caixa de pesquisa.
1. Selecione **AWS (Amazon Web Services)** no painel de resultados e adicione o aplicativo. Aguarde alguns segundos enquanto o aplicativo é adicionado ao seu locatário.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Configure e teste o SSO do Azure AD com o AWS usando um usuário de teste chamado **B.Fernandes**. Para que o SSO funcione, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do AWS.

Para configurar e testar o SSO do Azure AD com o AWS, conclua os seguintes blocos de construção:

1. **Configurar o SSO do Azure AD** – para permitir que os usuários usem esse recurso.
2. **Configure o AWS** para definir as configurações de SSO no lado do aplicativo.
3. **Crie um usuário de teste do Azure AD** para testar o SSO do Azure AD com B.Fernandes.
4. **Atribua o usuário de teste do Azure AD** para permitir que B.Fernandes use o SSO do Azure AD.
5. **Crie um usuário de teste do AWS** para ter um equivalente de B. Fernandes no AWS que esteja vinculado à representação de usuário do Azure AD.
6. **Teste o SSO** para verificar se a configuração funciona.

### <a name="configure-azure-ad-sso"></a>Configurar o SSO do Azure AD

Siga estas etapas para habilitar o SSO do Azure AD no portal do Azure.

1. No [portal do Azure](https://portal.azure.com/), na página de integração de aplicativos do **AWS (Amazon Web Services)** , localize a seção **Gerenciar** e selecione **Logon único**.
1. Na página **Escolher um método de logon único**, escolha **SAML**.
1. Na página **Configurar o Logon Único com SAML**, selecione o ícone de caneta da **Configuração Básica de SAML** para editar as configurações.

   ![Captura de tela da página Configurar logon único com SAML, com o ícone de caneta realçado](common/edit-urls.png)

4. Na seção **Configuração Básica de SAML**, o aplicativo é pré-configurado e as URLs necessárias já são preenchidas previamente com o Azure. O usuário precisa salvar a configuração selecionando **Salvar**.

5. Quando você estiver configurando mais de uma instância, forneça um valor de identificador. Da segunda instância em diante, use o seguinte formato, incluindo um sinal **#** para especificar um valor SPN exclusivo.

    `https://signin.aws.amazon.com/saml#2`

6. O aplicativo AWS espera as declarações do SAML em um formato específico, o que exige que você adicione mapeamentos de atributo personalizados de acordo com a sua configuração de atributos do token SAML. A captura de tela a seguir mostra a lista de atributos padrão. Selecione o ícone de caneta para abrir a caixa de diálogo Atributos do Usuário.

    ![Captura de tela da caixa de diálogo Atributos do Usuário, com o ícone de caneta realçado](common/edit-attribute.png)

7. Além dos atributos anteriores, o aplicativo AWS espera que mais alguns atributos sejam passados novamente na resposta SAML. Na seção **Declarações do Usuário** na caixa de diálogo **Atributos do Usuário**, execute as seguintes etapas para adicionar o atributo do token SAML.

    | NOME  | Atributo de origem  | Namespace |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Função            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    | SessionDuration             | "fornecer um valor entre 900 segundos (15 minutos) para segundos 43200 (12 horas)" |  https://aws.amazon.com/SAML/Attributes |

    a. Selecione **Adicionar nova declaração** para abrir a caixa de diálogo **Gerenciar declarações de usuários**.

    ![Captura de tela da seção Declarações do usuário, com Adicionar nova declaração e Salvar realçados](common/new-save-attribute.png)

    ![Captura de tela da caixa de diálogo Gerenciar declarações do usuário](common/new-attribute-details.png)

    b. Em **Nome**, digite o nome do atributo mostrado para essa linha.

    c. Em **Namespace**, digite o valor do namespace mostrado para essa linha.

    d. Em **Origem**, selecione **Atributo**.

    e. Na lista **Atributo de origem**, digite o valor do atributo mostrado para essa linha.

    f. Selecione **Ok**.

    g. Clique em **Salvar**.

1. Na página **Configurar logon único com SAML** na seção **Certificado de Autenticação SAML**, localize **XML de Metadados de Federação**. Selecione **Baixar** para baixar o certificado e salvá-lo em seu computador.

   ![Captura de tela da seção Certificado de Autenticação SAML, com Download realçado](common/metadataxml.png)

1. Na seção **Configurar o AWS (Amazon Web Services)** , copie a URL apropriada, com base em suas necessidades.

   ![Captura de tela da seção configurar AWS (Amazon Web Services), com URLs de configuração realçadas](common/copy-configuration-urls.png)

### <a name="configure-aws"></a>Configurar AWS

1. Em uma janela diferente do navegador, entre no site da empresa do AWS como administrador.

2. Selecione **Página inicial do AWS**.

    ![Captura de tela do site da empresa AWS, com o ícone Página inicial do AWS realçado][11]

3. Selecione **Gerenciamento de acesso e identidade**.

    ![Captura de tela da página de serviços do AWS, com IAM realçado][12]

4. Selecione **Provedores de Identidade** > **Criar Provedor**.

    ![Captura de tela da página IAM, com Provedores de Identidade e Criar Provedor realçados][13]

5. Na página **Configurar Provedor**, execute as seguintes etapas:

    ![Captura de tela de Configurar Provedor][14]

    a. Em **Tipo de provedor**, selecione **SAML**.

    b. Em **Nome do Provedor**, digite um nome para o provedor (por exemplo: *WAAD*).

    c. Para carregar seu **arquivo de metadados** baixado do Portal do Azure, selecione **Escolher Arquivo**.

    d. Selecione **Próxima etapa**.

6. Na página **Verificar Informações do Provedor**, selecione **Criar**.

    ![Captura de tela de Verificar Informações do Provedor, com Criar realçado][15]

7. Selecione **Funções** > **Criar função**.

    ![Captura de tela da página Funções][16]

8. Na página **Criar função**, realize as seguintes etapas:  

    ![Captura de tela da página Criar função][19]

    a. Em **Selecionar tipo de entidade confiável**, selecione **Federação do SAML 2.0**.

    b. Em **Escolher um provedor do SAML 2.0**, selecione o **Provedor do SAML** criado anteriormente (por exemplo: *WAAD*).

    c. Selecione **Permitir acesso do Console de Gerenciamento do AWS e programação**.
  
    d. Selecione **Avançar: Permissões.**

9. Na caixa de diálogo **Anexar políticas de permissão**, anexe a política adequada conforme sua organização. Em seguida, selecione **Avançar: Análise**.  

    ![Captura de tela da caixa de diálogo Anexar política de permissões][33]

10. Na caixa de diálogo **Examinar**, execute as seguintes etapas:

    ![Captura de tela da caixa de diálogo Examinar][34]

    a. Em **Nome da função**, insira o nome da função.

    b. Em **Descrição da função**, insira a descrição.

    c. Selecione **Criar função**.

    d. Crie quantas funções forem necessárias e mapeie-as para o provedor de identidade.

11. Use as credenciais de conta de serviço do AWS para buscar as funções da conta AWS no provisionamento de usuário do Azure AD. Para isso, abra a página inicial do console AWS.

12. Selecione **Serviços**. Em **Segurança, Identidade e Conformidade**, selecione **IAM**.

    ![Captura de tela da página inicial do console do AWS, com Serviços e IAM realçados](./media/amazon-web-service-tutorial/fetchingrole1.png)

13. Na seção IAM, selecione **Políticas**.

    ![Captura de tela da seção IAM, com Políticas realçado](./media/amazon-web-service-tutorial/fetchingrole2.png)

14. Crie uma política selecionando **Criar política** para buscar as funções da conta AWS no provisionamento de usuário do Azure AD.

    ![Captura de tela da página Criar função, com Criar política realçado](./media/amazon-web-service-tutorial/fetchingrole3.png)

15. Crie sua própria política para buscar todas as funções de contas AWS.

    ![Captura de tela da página Criar política, com JSON realçado](./media/amazon-web-service-tutorial/policy1.png)

    a. Na **Criar a política**, selecione a guia **JSON**.

    b. No documento de política, adicione o seguinte JSON:

    ```json
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                "iam:ListRoles"
                ],
                "Resource": "*"
            }
        ]
    }
    ```

    c. Selecione **Examinar política** para validar a política.

    ![Captura de tela da página Criar política](./media/amazon-web-service-tutorial/policy5.png)

16. Defina a nova política.

    ![Captura de tela da página Criar política, com os campos Nome e Descrição realçados](./media/amazon-web-service-tutorial/policy2.png)

    a. Para **Nome**, insira **AzureAD_SSOUserRole_Policy**.

    b. Para **Descrição**, insira **Esta política permitirá buscar as funções de contas do AWS**.

    c. Selecione **Criar política**.

17. Crie uma conta de usuário no serviço de IAM do AWS.

    a. No console IAM do AWS, selecione **Usuários**.

    ![Captura de tela do console IAM do AWS, com Usuários realçado](./media/amazon-web-service-tutorial/policy3.png)

    b. Para criar um usuário, selecione **Adicionar usuário**.

    ![Captura de tela do botão Adicionar usuário](./media/amazon-web-service-tutorial/policy4.png)

    c. Na seção **Adicionar usuário**:

    ![Captura de tela da página Adicionar usuário, com Nome de usuário e Tipo de acesso realçados](./media/amazon-web-service-tutorial/adduser1.png)

    * Digite o nome de usuário como **AzureADRoleManager**.

    * Para o tipo de acesso, selecione **Acesso programático**. Dessa forma, o usuário pode invocar as APIs e buscar as funções da conta AWS.

    * Selecione **Próximas Permissões**.

18. Crie uma política para este usuário.

    ![Captura de tela de Adicionar usuário](./media/amazon-web-service-tutorial/adduser2.png)

    a. Selecione **Anexar políticas existentes diretamente**.

    b. Pesquise a política recém-criada na seção filtro **AzureAD_SSOUserRole_Policy**.

    c. Selecione a política e, em seguida, **Próximo: Análise**.

19. Examine a política para o usuário anexado.

    ![Captura de tela da página Adicionar usuário, com Criar usuário realçado](./media/amazon-web-service-tutorial/adduser3.png)

    a. Examine o nome de usuário, tipo de acesso e política mapeados para o usuário.

    b. Selecione **Criar usuário**.

20. Baixe as credenciais do usuário de um usuário.

    ![Captura de tela de Adicionar usuário](./media/amazon-web-service-tutorial/adduser4.png)

    a. Copie a **ID de chave de acesso** do usuário e a **Chave de acesso secreta**.

    b. Insira essas credenciais na seção de provisionamento de usuário do Azure AD para buscar as funções do console AWS.

    c. Selecione **Fechar**.

21. No portal de gerenciamento do Azure AD, no aplicativo AWS, acesse **Provisionamento**.

    ![Captura de tela do aplicativo AWS, com Provisionamento realçado](./media/amazon-web-service-tutorial/provisioning.png)

22. Insira a chave de acesso e o segredo nos campos **clientsecret** e **Token do Segredo**, respectivamente.

    ![Captura de tela da caixa de diálogo Credenciais de Administrador](./media/amazon-web-service-tutorial/provisioning1.png)

    a. Insira a chave de acesso do usuário do AWS no campo **Segredo do cliente**.

    b. Insira o segredo do usuário do AWS no campo **Segredo do Token**.

    c. Selecione **Testar Conexão**.

    d. Salve a configuração selecionando **Salvar**.

23. Na seção **Configurações**, para **Status de Provisionamento**, selecione **Ativado**. Em seguida, selecione **Salvar**.

    ![Captura de tela da seção Configurações, com Ativado realçado](./media/amazon-web-service-tutorial/provisioning2.png)

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

Nesta seção, você criará uma usuária de teste, B. Fernandes, no portal do Azure.

1. No painel esquerdo do portal do Azure, selecione **Azure Active Directory** > **Usuários** > **Todos os usuários**.
1. Selecione **Novo usuário** na parte superior da tela.
1. Nas propriedades do **Usuário**, siga estas etapas:
   
   a. No campo **Nome**, insira `B.Simon`.  
   b. No campo **Nome de usuário**, insira username@companydomain.extension. Por exemplo, `B.Simon@contoso.com`.   
   c. Selecione **Mostrar Senha** e anote-a. Em seguida, selecione **Criar**.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que B.Fernandes use o SSO do Azure permitindo a ela acesso ao AWS.

1. No portal do Azure, selecione **Aplicativos empresariais** e, em seguida, selecione **Todos os aplicativos**.
1. Na lista de aplicativos, selecione **AWS (Amazon Web Services)** .
1. Na página de visão geral do aplicativo, localize a seção **Gerenciar** e selecione **Usuários e grupos**.

   ![Captura de tela da seção Gerenciar, com Usuários e grupos realçados](common/users-groups-blade.png)

1. Selecione **Adicionar usuário**. Na caixa de diálogo **Adicionar Atribuição**, selecione **Usuários e grupos**.

    ![Captura de tela de Adicionar usuário](common/add-assign-user.png)

1. Na caixa de diálogo **Usuários e grupos**, selecione **B.Fernandes**. Em seguida, escolha **Selecionar**.
1. Se você esperar um valor de função na declaração SAML, na caixa de diálogo **Selecionar Função**, selecione a função apropriada para o usuário na lista. Em seguida, escolha **Selecionar**.
1. Na caixa de diálogo **Adicionar Atribuição**, selecione **Atribuir**.

### <a name="test-single-sign-on"></a>Testar logon único

Quando selecionar o bloco do AWS no Painel de Acesso, você deverá ser conectado automaticamente ao AWS para o qual você configurou o SSO. Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="known-issues"></a>Problemas conhecidos

 * Na seção **Provisionamento**, a subseção **Mapeamentos** mostra uma mensagem "Carregando..." e nunca exibe os mapeamentos de atributo. O único fluxo de trabalho de provisionamento compatível hoje é a importação de funções do AWS no Azure AD para seleção durante a atribuição de um usuário ou grupo. Os mapeamentos de atributo para isso são predeterminados e não são configuráveis.
 
 * A seção **Provisionamento** só dá suporte a um conjunto de credenciais para um locatário do AWS por vez. Todas as funções importadas são escritas na propriedade `appRoles` do [`servicePrincipal` objeto](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) do Azure AD para o locatário do AWS. 
 
   Vários locatários AWS (representados por `servicePrincipals`) podem ser adicionados ao Azure AD da galeria para provisionamento. Há um problema conhecido, no entanto, sem ser possível gravar automaticamente todas as funções importadas de vários `servicePrincipals` do AWS usados para provisionamento em um único `servicePrincipal` usado para SSO. 
   
   Como alternativa, você pode usar a [API do Microsoft Graph](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) para extrair todo o `appRoles` importado em cada `servicePrincipal` do AWS em que o provisionamento é configurado. Posteriormente, você pode adicionar essas cadeias de caracteres de função ao `servicePrincipal` do AWS em que o SSO está configurado.

## <a name="additional-resources"></a>Recursos adicionais

- [Tutoriais para a integração de aplicativos SaaS ao Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [O que é o acesso condicional no Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

<!--Image references-->

[11]: ./media/amazon-web-service-tutorial/ic795031.png
[12]: ./media/amazon-web-service-tutorial/ic795032.png
[13]: ./media/amazon-web-service-tutorial/ic795033.png
[14]: ./media/amazon-web-service-tutorial/ic795034.png
[15]: ./media/amazon-web-service-tutorial/ic795035.png
[16]: ./media/amazon-web-service-tutorial/ic795022.png
[17]: ./media/amazon-web-service-tutorial/ic795023.png
[18]: ./media/amazon-web-service-tutorial/ic795024.png
[19]: ./media/amazon-web-service-tutorial/ic795025.png
[32]: ./media/amazon-web-service-tutorial/ic7950251.png
[33]: ./media/amazon-web-service-tutorial/ic7950252.png
[35]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/amazon-web-service-tutorial/ic7950253.png
[36]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
