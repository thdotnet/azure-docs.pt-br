---
title: Problema ao entrar um aplicativo de galeria do Azure AD configurado para SSO de senha | Microsoft Docs
description: Como solucionar problemas com um aplicativo de galeria do Azure AD que está configurado para logon único senha.
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
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 742df882fb64e09ff63ef2eceb5514ca070dc227
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190318"
---
# <a name="sign-in-problems-with-an-azure-ad-gallery-app-configured-for-sso"></a>Problemas ao entrar com um aplicativo de galeria do Azure AD configurado para SSO

Painel de acesso é um portal baseado na web. Ele permite que os usuários que tem o trabalho do Azure Active Directory (Azure AD) ou de estudante contas para acessar aplicativos baseados em nuvem que eles têm permissões para. Os usuários que têm as edições do Azure AD também podem usar grupos de autoatendimento e recursos de gerenciamento de aplicativo por meio do painel de acesso.

Painel de acesso é separado do portal do Azure. Os usuários não precisam de uma assinatura do Azure para usar o painel de acesso.

Para usar com base em senha de logon único (SSO) no painel de acesso, a extensão do painel de acesso deve ser instalada em seu navegador. A extensão baixa automaticamente quando você seleciona um aplicativo que está configurado para SSO baseado em senha.

## <a name="browser-requirements-for-access-panel"></a>Requisitos de navegador para o painel de acesso

Painel de acesso exige um navegador com suporte para JavaScript e CSS habilitado.

Os seguintes navegadores dão suporte a SSO baseado em senha:

- Internet Explorer 8, 9, 10 e 11 no Windows 7 ou posterior

- Chrome no Windows 7 ou posterior ou no MacOS X ou posterior

- Firefox 26.0 ou posterior no Windows XP SP2 ou posterior ou no Mac OS X 10.6 ou posterior

>[!NOTE]
>A extensão SSO baseada em senha será disponibilizada para o Microsoft Edge no Windows 10 quando o suporte para extensões do navegador foi adicionado ao Microsoft Edge.

## <a name="install-the-access-panel-browser-extension"></a>Instalar a extensão de navegador do painel de acesso

Siga estas etapas:

1. Abra [painel de acesso](https://myapps.microsoft.com) em um navegador com suporte e entre como um usuário do Azure AD.

2. Selecione um aplicativo habilitado por SSO de senha no painel de acesso.

3. Quando solicitado, selecione **instalar agora**.

4. Você será direcionado para um link de download com base no seu navegador. Selecione **adicionar** para instalar a extensão do navegador.

5. Se você for solicitado, selecione **habilitar** ou **permitir**.

6. Após a instalação, reinicie o navegador.

7.  Entrar no painel de acesso e veja se você pode iniciar seus aplicativos habilitados por SSO de senha.

Também diretamente, você pode baixar as extensões para Chrome e Firefox por meio destes links:

-   [Extensão do painel de acesso do Chrome](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Extensão do painel de acesso do Firefox](https://addons.mozilla.org/firefox/addon/access-panel-extension/)

## <a name="set-up-a-group-policy-for-internet-explorer"></a>Configurar uma política de grupo para o Internet Explorer

Você pode configurar uma política de grupo que permite instalar remotamente a extensão do painel de acesso para o Internet Explorer nos computadores dos usuários.

Estes são os pré-requisitos:

-   [Serviços de domínio do Active Directory](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx) deve ser configurado, e os computadores dos usuários devem ser associados a seu domínio.

-   Você tem permissão "Editar configurações" para editar o objeto de diretiva de grupo (GPO). Por padrão, os membros dos grupos de segurança a seguir têm essa permissão: Administradores de Domínio, Administradores da Empresa e Proprietários criadores de políticas de grupo. [Saiba mais](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx).

Para configurar a política de grupo e implantá-lo aos usuários, consulte [como implantar a extensão do painel de acesso do Internet Explorer usando a diretiva de grupo](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-group-policy).

## <a name="troubleshoot-access-panel-in-internet-explorer"></a>Solucionar problemas de painel de acesso no Internet Explorer

Para acessar uma ferramenta de diagnóstico e as instruções para configurar a extensão, consulte [solucionar problemas da extensão do painel de acesso para o Internet Explorer](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ie-troubleshooting).

## <a name="configure-password-sso-for-an-azure-ad-gallery-app"></a>Configurar o SSO de senha para um aplicativo de galeria do Azure AD

Para configurar um aplicativo da Galeria do Azure AD, você precisa fazer estas coisas:

-   Adicionar o aplicativo da Galeria do Azure AD
-   [Configurar o aplicativo para logon único senha](#configure-the-app-for-password-sso)
-   [Atribuir usuários ao aplicativo](#assign-users-to-the-app)

### <a name="add-the-app-from-the-azure-ad-gallery"></a>Adicionar o aplicativo da Galeria do Azure AD

Siga estas etapas:

1. Abra o [portal do Azure](https://portal.azure.com) e entre como um administrador global ou um coadministrador.

2. Selecione **todos os serviços** na parte superior do painel de navegação à esquerda para abrir a extensão do AD do Azure.

3. Tipo de **Azure Active Directory** na caixa de pesquisa do filtro e selecione **Azure Active Directory**.

4. Selecione **aplicativos empresariais** no painel de navegação do Azure AD.

5. Selecione **Add** no canto superior direito das **aplicativos empresariais** painel.

6. No **adicionar da galeria** seção, digite o nome do aplicativo na **Insira um nome** caixa.

7. Selecione o aplicativo que você deseja configurar para que o SSO.

8. *Opcional:* Antes de adicionar o aplicativo, você pode alterar seu nome na **nome** caixa.

9. Clique em **adicionar** para adicionar o aplicativo.

   Após um breve intervalo, você poderá ver o painel de configuração do aplicativo.

### <a name="configure-the-app-for-password-sso"></a>Configurar o aplicativo de SSO de senha

Siga estas etapas:

1. Abra o [portal do Azure](https://portal.azure.com/) e entre como um administrador global ou um coadministrador.

2. Selecione **todos os serviços** na parte superior do painel de navegação à esquerda para abrir a extensão do AD do Azure.

3. Tipo de **Azure Active Directory** na caixa de pesquisa do filtro e selecione **Azure Active Directory**.

4. Selecione **aplicativos empresariais** no painel de navegação do Azure AD.

5. Selecione **todos os aplicativos** para exibir uma lista dos seus aplicativos.

   > [!NOTE]
   > Se você não vir o aplicativo que você deseja, use o **filtro** controle na parte superior das **lista de todos os aplicativos**. Defina as **Mostrar** opção para "Todos os aplicativos".

6. Selecione o aplicativo que você deseja configurar para que o SSO.

7. Depois que o aplicativo for carregado, selecione **logon único** no painel à esquerda do aplicativo.

8. Selecione **baseado em senha logon** modo.

9. Atribua usuários ao aplicativo.

10. Você também pode fornecer credenciais para os usuários. (Caso contrário, os usuários deverão inserir as credenciais na inicialização do aplicativo.) Para fazer isso, selecione as linhas dos usuários. Em seguida, selecione **atualizar credenciais** e insira seus nomes de usuário e senhas.

### <a name="assign-users-to-the-app"></a>Atribuir usuários ao aplicativo

Para atribuir usuários a um aplicativo diretamente, siga estas etapas:

1. Abra o [portal do Azure](https://portal.azure.com/) e entre como um administrador global.

2. Selecione **todos os serviços** na dificuldade de navegação à esquerda para abrir a extensão do AD do Azure.

3. Tipo de **Azure Active Directory** na caixa de pesquisa do filtro e selecione **Azure Active Directory**.

4. Selecione **aplicativos empresariais** no painel de navegação do Azure AD.

5. Selecione **todos os aplicativos** para exibir uma lista dos seus aplicativos.

   > [!NOTE]
   > Se você não vir o aplicativo que você deseja, use o **filtro** controle na parte superior das **lista de todos os aplicativos**. Defina as **Mostrar** opção para "Todos os aplicativos".

6. Na lista, selecione o aplicativo que você deseja atribuir a um usuário.

7. Depois que o aplicativo é carregado, selecione **usuários e grupos** do aplicativo painel de navegação à esquerda.

8. Selecione **Add** na parte superior das **usuários e grupos** lista para abrir o **Adicionar atribuição** painel.

9. Selecione **usuários e grupos** na **Adicionar atribuição** painel.

10. No **pesquisar por nome ou endereço de email** caixa, digite o nome completo ou endereço de email do usuário que você deseja atribuir.

11. Passe o mouse sobre o usuário na lista. Marque a caixa de seleção ao lado de foto de perfil do usuário ou o logotipo para adicionar esse usuário para o **selecionados** lista.

12. *Opcional:* Para adicionar outro usuário, digite outro nome ou endereço de email na **pesquisar por nome ou endereço de email** caixa e, em seguida, selecione a caixa de seleção para adicionar esse usuário para o **selecionados** lista.

13. Quando você terminar de selecionar usuários, clique em **selecionar** para adicioná-los à lista de usuários e grupos que são atribuídos ao aplicativo.

14. *Opcional:* Clique em **Selecionar função** na **Adicionar atribuição** painel para selecionar uma função para atribuir aos usuários que você selecionou.

15. Selecione **atribuir** para atribuir o aplicativo aos usuários selecionados.

    Após um breve intervalo, os usuários poderão acessar esses aplicativos de painel de acesso.

## <a name="request-support"></a>Solicitar suporte 
Se você receber uma mensagem de erro quando você configurar o SSO e atribuir usuários, abra um tíquete de suporte. Inclua o máximo das seguintes informações quanto possível:

-   ID de erro de correlação
-   UPN (endereço de email de usuário)
-   TenantID
-   Tipo de navegador
-   Fuso horário e o quadro de hora/hora quando o erro ocorreu
-   Rastreamentos do Fiddler

## <a name="next-steps"></a>Próximas etapas
[Fornecer logon único para seus aplicativos com Proxy de Aplicativo](application-proxy-configure-single-sign-on-with-kcd.md)
