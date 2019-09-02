---
title: Como configurar o logon único federado para um aplicativo na Galeria do Azure AD | Microsoft Docs
description: Como configurar o logon único federado para um aplicativo existente da galeria do Azure AD e como usar os tutoriais para começar rapidamente
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.collection: M365-identity-device-management
ROBOTS: NOINDEX
ms.openlocfilehash: 8e50a495e1b0406e0c935ac31111dc6b5d0c0821
ms.sourcegitcommit: 5f67772dac6a402bbaa8eb261f653a34b8672c3a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/01/2019
ms.locfileid: "70207123"
---
# <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Como configurar o logon único federado para um aplicativo na Galeria do Azure AD

Há um tutorial passo a passo disponível para todos os aplicativos na galeria do Azure Active Directory (Azure AD) que têm o recurso de logon único corporativo. É possível acessar a [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) para uma orientação passo a passo detalhada.

## <a name="overview-of-steps-required"></a>Visão geral das etapas necessárias
Para configurar um aplicativo da galeria do Azure AD, será necessário:

-   [Adicionar um aplicativo da galeria do Azure AD](#add-an-application-from-the-azure-ad-gallery)

-   [Configurar os valores de metadados do aplicativo no Azure AD (URL de logon, identificador, URL de resposta)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Selecionar o Identificador de Usuário e adicionar os atributos de usuário a serem enviados ao aplicativo](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Recuperar os metadados e o certificado do Azure AD](#download-the-azure-ad-metadata-or-certificate)

-   [Configurar os valores de metadados do Azure AD no aplicativo (URL de logon, emissor, URL de logout e certificado)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Atribuir usuários ao aplicativo](#assign-users-to-the-application)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Adicionar um aplicativo da galeria do Azure AD

Para adicionar um aplicativo da Galeria do Azure Active Directory, siga estas etapas:

1.  Abra o [Portal do Azure](https://portal.azure.com) e entre como um **Administrador Global** ou **Coadministrador**.

2.  Abra a **extensão Azure Active Directory** selecionando **todos os serviços** na parte superior do menu de navegação do lado esquerdo principal.

3.  Digite "Azure Active Directory" na caixa de pesquisa e selecione **Azure Active Directory**.

4.  Selecione **aplicativos empresariais** no menu de navegação do lado esquerdo do Azure AD.

5.  Selecione **Adicionar** no canto superior direito no painel **aplicativos empresariais** .

6.  Na caixa **Inserir um nome** na seção **Adicionar da Galeria** , digite o nome do aplicativo.

7.  Selecione o aplicativo para o qual você deseja configurar o logon único.

8.  Antes de adicionar o aplicativo, você pode alterar seu nome na caixa **nome** .

9.  Selecione **Adicionar** para adicionar o aplicativo.

Após um curto período, você deve ser capaz de ver o painel de configuração do aplicativo.

## <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Configurar o logon único para um aplicativo da galeria do Azure AD

Para configurar o logon único para um aplicativo, siga estas etapas:

1. Abra o [Portal do Azure](https://portal.azure.com/) e entre como um **Administrador Global** ou **Coadministrador**.

2. Abra a **extensão Azure Active Directory** selecionando **todos os serviços** na parte superior do menu de navegação do lado esquerdo principal.

3. Digite "Azure Active Directory" na caixa de pesquisa e selecione **Azure Active Directory**.

4. Selecione **aplicativos empresariais** no Azure Active Directory menu de navegação do lado esquerdo.

5. Marque **Todos os aplicativos** para exibir uma lista com todos os seus aplicativos.

   * Se você não vir o aplicativo desejado aqui, use o controle de **filtro** na parte superior da **lista todos os aplicativos** e defina a opção **Mostrar** como **todos os aplicativos.**

6. Selecione o aplicativo para o qual você deseja configurar o logon único.

7. Depois que o aplicativo for carregado, selecione o **logon único** no menu de navegação do lado esquerdo do aplicativo.

8. Selecione **Logon com base em SAML** da lista suspensa **Modo**.

9. Insira os valores necessários em **Domínio e URLs.** É possível obter esses valores do fornecedor do aplicativo.

   1. Para configurar o aplicativo como SSO iniciado por SP, a URL de Logon é um valor obrigatório. Para alguns aplicativos, o Identificador também é um valor obrigatório.

   2. Para configurar o aplicativo como SSO iniciado por IdP, a URL de Resposta é um valor obrigatório. Para alguns aplicativos, o Identificador também é um valor obrigatório.

10. **Opcional**: Selecione **Mostrar configurações de URL avançadas** se desejar ver os valores não obrigatórios.

11. Nos **Atributos de usuário**, selecione o identificador exclusivo para os usuários na lista suspensa **Identificador de usuário**.

12. **Opcional**: Selecione **Exibir e editar todos os outros atributos de usuário** para editar os atributos a serem enviados ao aplicativo no token SAML quando os usuários entrarem.

    Para adicionar um atributo:
   
    1. Selecione **Adicionar Atributo**. Insira o **Nome** e selecione o **Valor** da lista suspensa.

    1. Selecione **Salvar**. Você verá o novo atributo na tabela.

13. Selecione **Configurar &lt;nome&gt; do aplicativo** para acessar a documentação sobre como configurar o logon único no aplicativo. Além disso, você tem as URLs de metadados necessárias e o certificado para configurar o SSO com o aplicativo.

14. Selecione **Salvar** para salvar a configuração.

15. Atribuir usuários a um aplicativo.

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Selecionar o Identificador de Usuário e adicionar os atributos de usuário a serem enviados ao aplicativo

Para selecionar o identificador de usuário ou adicionar atributos de usuário, siga estas etapas:

1. Abra o [portal do Azure](https://portal.azure.com/) e entre como um **administrador global** ou coadministrador **.**

2. Abra a **extensão Azure Active Directory** selecionando **todos os serviços** na parte superior do menu de navegação do lado esquerdo principal.

3. Digite "Azure Active Directory" na caixa de pesquisa e selecione **Azure Active Directory**.

4. Selecione **aplicativos empresariais** no Azure Active Directory menu de navegação do lado esquerdo.

5. Marque **Todos os aplicativos** para exibir uma lista com todos os seus aplicativos.

   * Se você não vir o aplicativo desejado aqui, use o controle de **filtro** na parte superior da **lista todos os aplicativos** e defina a opção **Mostrar** como **todos os aplicativos.**

6. Selecione o aplicativo que você configurou com logon único.

7. Depois que o aplicativo for carregado, selecione o **logon único** no menu de navegação do lado esquerdo do aplicativo.

8. Na seção **Atributos de usuário**, selecione o identificador exclusivo para os usuários na lista suspensa **Identificador de usuário**. A opção selecionada precisa corresponder ao valor esperado no aplicativo para autenticar o usuário.

   >[!NOTE] 
   >O Azure AD seleciona o formato para o atributo NameID (identificador de usuário) com base no valor selecionado ou no formato solicitado pelo aplicativo no AuthRequest do SAML. Para obter mais informações, consulte [protocolo SAML de logon único](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) na seção NameIDPolicy.
   >
   >

9. Para adicionar atributos de usuário, selecione **Exibir e editar todos os outros atributos de usuário** para editar os atributos a serem enviados ao aplicativo no token SAML quando os usuários entrarem.

   Para adicionar um atributo:
  
   1. Selecione **Adicionar Atributo**. Insira o **Nome** e selecione o **Valor** da lista suspensa.

   2. Clique em **Salvar**. Você verá o novo atributo na tabela.

## <a name="download-the-azure-ad-metadata-or-certificate"></a>Baixar o certificado ou metadados do Azure AD

Para baixar o certificado ou metadados do aplicativo Azure Active Directory, siga estas etapas:

1. Abra o [portal do Azure](https://portal.azure.com/) e entre como um **administrador global** ou coadministrador **.**

2. Abra a **extensão Azure Active Directory** selecionando **todos os serviços** na parte superior do menu de navegação do lado esquerdo principal.

3. Digite "Azure Active Directory" na caixa de pesquisa e selecione **Azure Active Directory**.

4. Selecione **aplicativos empresariais** no Azure Active Directory menu de navegação do lado esquerdo.

5. Marque **Todos os aplicativos** para exibir uma lista com todos os seus aplicativos.

   *  Se você não vir o aplicativo desejado aqui, use o controle de **filtro** na parte superior da **lista todos os aplicativos** e defina a opção **Mostrar** como **todos os aplicativos**.

6. Selecione o aplicativo que você configurou com logon único.

7. Depois que o aplicativo for carregado, selecione o **logon único** no menu de navegação do lado esquerdo do aplicativo.

8. Acesse a seção **certificado de autenticação SAML** e selecione **baixar** valor da coluna. Dependendo do aplicativo que exigir a configuração de logon único, você verá a opção para baixar o Certificado ou o XML de Metadados.

O AD do Azure também fornece uma URL para acessar os metadados. Use o modelo a seguir para obter a URL de metadados específica para o aplicativo:`https://login.microsoftonline.com/<Directory ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<Application ID>`

## <a name="assign-users-to-the-application"></a>Atribuir usuários ao aplicativo

Para atribuir um ou mais usuários a um aplicativo diretamente, siga estas etapas:

1. Abra o [portal do Azure](https://portal.azure.com/) e entre como um **administrador global.**

2. Abra a **extensão Azure Active Directory** selecionando **todos os serviços** na parte superior do menu de navegação do lado esquerdo principal.

3. Digite "Azure Active Directory" na caixa de pesquisa e selecione **Azure Active Directory**.

4. Selecione **aplicativos empresariais** no Azure Active Directory menu de navegação do lado esquerdo.

5. Marque **Todos os aplicativos** para exibir uma lista com todos os seus aplicativos.

   * Se você não vir o aplicativo desejado aqui, use o controle de **filtro** na parte superior da **lista todos os aplicativos** e defina a opção **Mostrar** como **todos os aplicativos.**

6. Na lista, selecione o aplicativo ao qual deseja atribuir um usuário.

7. Depois que o aplicativo for carregado, selecione **usuários e grupos** no menu de navegação do lado esquerdo do aplicativo.

8. Selecione o botão **Adicionar** na parte superior da lista **usuários e grupos** para abrir o painel **Adicionar atribuição** .

9. Clique no seletor **Usuários e grupos** do painel **Adicionar Atribuição**.

10. Digite o **nome completo** ou o **endereço de email** do usuário que você deseja atribuir à caixa de pesquisa **Pesquisar por nome ou endereço de email** .

11. Passe o mouse sobre o **usuário** na lista para revelar uma **caixa de seleção**. Marque a caixa de seleção ao lado da foto ou do logotipo do perfil do usuário para adicionar o usuário à lista **selecionada** .

12. **Opcional**: Se você quiser **adicionar mais de um usuário**, digite outro **nome completo** ou **endereço de email** na caixa de pesquisa **Pesquisar por nome ou endereço de email** e marque a caixa de seleção para adicionar esse usuário à lista **selecionada** .

13. Quando tiver terminado de selecionar os usuários, selecione o botão **selecionar** para adicioná-los à lista de usuários e grupos a serem atribuídos ao aplicativo.

14. **Opcional**: Selecione o seletor **selecionar função** no painel **Adicionar atribuição** para selecionar uma função a ser atribuída aos usuários selecionados.

15. Selecione o botão **atribuir** para atribuir o aplicativo aos usuários selecionados.

Após um breve período, os usuários que você selecionou poderão iniciar esses aplicativos usando os métodos descritos na seção de descrição da solução.

## <a name="customizing-the-saml-claims-sent-to-an-application"></a>Personalizando as declarações SAML enviadas para um aplicativo

Para saber como personalizar as declarações de atributo SAML enviadas ao seu aplicativo, consulte [mapeamento de declarações em Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping).

## <a name="next-steps"></a>Próximas etapas
[Fornecer logon único para seus aplicativos com Proxy de Aplicativo](application-proxy-configure-single-sign-on-with-kcd.md)



