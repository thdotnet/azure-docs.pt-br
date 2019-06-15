---
title: Tutorial – criar fluxos de usuário - Azure Active Directory B2C
description: Saiba como criar fluxos de usuário no portal do Azure para habilitar o logon, entrar e edição de perfil de usuário em seus aplicativos no Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 06/07/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 111196388d0e17ecde8a2055959f2f573e43ade8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056374"
---
# <a name="tutorial-create-user-flows-in-azure-active-directory-b2c"></a>Tutorial: Criar fluxos dos usuários no Azure Active Directory B2C

Em seus aplicativos, você pode ter [fluxos de usuário](active-directory-b2c-reference-policies.md) que permitem que os usuários se inscrever, entrar ou gerenciar seu perfil. É possível criar vários fluxos dos usuários de diferentes tipos no locatário do Azure AD (Azure Active Directory) B2C e usá-los nos aplicativos, conforme necessário. Fluxos dos usuários podem ser reutilizados nos aplicativos.

Neste artigo, você aprenderá a:

> [!div class="checklist"]
> * Criar um fluxo de usuário de inscrição e de entrada
> * Criar um fluxo de usuário de edição de perfil
> * Criar um fluxo de usuário de redefinição de senha

Este tutorial mostra como criar alguns fluxos dos usuários recomendados usando o portal do Azure. Se você estiver procurando informações sobre como configurar credenciais de senha de proprietário do recurso fluxo (ROPC) em seu aplicativo, consulte [configurar fluem de credenciais de senha de proprietário do recurso no Azure AD B2C](configure-ropc.md).

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

[Registre seus aplicativos](tutorial-register-applications.md) que fazem parte dos fluxos dos usuários que deseja criar.

## <a name="create-a-sign-up-and-sign-in-user-flow"></a>Criar um fluxo de usuário de inscrição e de entrada

O fluxo do usuário de inscrição e credenciais controla as experiências de inscrição e credenciais do consumidor com uma única configuração. Os usuários do aplicativo são conduzidos pelo caminho certo, de acordo com o contexto.

1. Entre no [Portal do Azure](https://portal.azure.com).
1. Verifique se você está usando o diretório que contém o locatário do Azure AD B2C clicando no **filtro Diretório e assinatura** no menu superior e escolhendo o diretório que contém seu locatário.

    ![Alternar para o diretório de assinatura](./media/tutorial-create-user-flows/switch-directories.PNG)

1. Escolha **Todos os serviços** no canto superior esquerdo do Portal do Azure, pesquise **Azure AD B2C** e selecione-o.
1. No menu à esquerda, sob **diretivas**, selecione **(diretivas) de fluxos de usuário**e, em seguida, selecione **novo fluxo de usuário**.

    ![Selecionar novo fluxo de usuário](./media/tutorial-create-user-flows/signup-signin-user-flow.png)

1. Sobre o **recomendado** guia, selecione o **assinar e inscrição de** fluxo de usuário.

    ![Selecionar o fluxo do usuário de inscrição e conexão](./media/tutorial-create-user-flows/signup-signin-type.png)

1. Insira um **Nome** para o fluxo de usuário. Por exemplo, *signupsignin1*.
1. Para **Provedores de identidade**, selecione **Inscrição por email**.

    ![Definir as propriedades do fluxo](./media/tutorial-create-user-flows/signup-signin-properties.png)

1. Para **Atributos e declarações do usuário**, escolha as declarações e atributos que deseja coletar e enviar do usuário durante a inscrição. Por exemplo, selecione **Mostrar mais**e, em seguida, escolha atributos e declarações para **país/região**, **nome de exibição**, e **CEP**. Clique em **OK**.

    ![Selecionar atributos e declarações](./media/tutorial-create-user-flows/signup-signin-attributes.png)

1. Clique em **Criar** para adicionar o fluxo de usuário. Um prefixo *B2C_1* é acrescentado automaticamente ao nome.

### <a name="test-the-user-flow"></a>Testar o fluxo de usuário

1. Selecione o fluxo de usuário que você criou para abrir sua página de visão geral e, em seguida, selecione **executar o fluxo de usuário**.
1. Para **Aplicativo**, selecione o aplicativo Web denominado *webapp1* que você registrou anteriormente. A **URL de resposta** deve mostrar `https://jwt.ms`.
1. Clique em **Executar o fluxo do usuário**e, em seguida, selecione **Inscreva-se agora**.

    ![Executar o fluxo do usuário](./media/tutorial-create-user-flows/signup-signin-run-now.PNG)

1. Insira um endereço de email válido, clique em **enviar código de verificação**, insira o código de verificação que você recebe, em seguida, selecione **verificar código**.
1. Insira uma nova senha e confirme-a.
1. Selecione seu país e região, insira o nome a ser exibido, insira um código postal e, em seguida, clique em **criar**. O token é retornado para `https://jwt.ms` e deve ser exibido para você.
1. Agora, você pode executar o fluxo do usuário novamente e poderá se conectar com a conta que você criou. O token retornado inclui as declarações que você selecionou de país/região, o nome e o código postal.

## <a name="create-a-profile-editing-user-flow"></a>Criar um fluxo de usuário de edição de perfil

Se quiser permitir que os usuários editem seu perfil no aplicativo, use um fluxo do usuário de edição de perfil.

1. No menu à esquerda da página de visão geral de locatário do Azure AD B2C, selecione **(políticas) de fluxos de usuário**e, em seguida, selecione **novo fluxo de usuário**.
1. Selecione o fluxo do usuário **Edição de perfil** na guia Recomendado.
1. Insira um **Nome** para o fluxo de usuário. Por exemplo, *profileediting1*.
1. Para **Provedores de identidade**, selecione **Conexão na Conta Local**.
1. Para **Atributos do usuário**, escolha os atributos que deseja que o cliente seja capaz de editar no perfil dele. Por exemplo, selecione **Mostrar mais**e, em seguida, escolha os atributos e declarações para **nome de exibição** e **cargo**. Clique em **OK**.
1. Clique em **Criar** para adicionar o fluxo de usuário. Um prefixo *B2C_1* é acrescentado automaticamente ao nome.

### <a name="test-the-user-flow"></a>Testar o fluxo de usuário

1. Selecione o fluxo de usuário que você criou para abrir sua página de visão geral e, em seguida, selecione **executar o fluxo de usuário**.
1. Para **Aplicativo**, selecione o aplicativo Web denominado *webapp1* que você registrou anteriormente. A **URL de resposta** deve mostrar `https://jwt.ms`.
1. Clique em **Executar o fluxo do usuário** e, em seguida, entre com a conta que criou anteriormente.
1. Agora, você tem a oportunidade de alterar o nome de exibição e o cargo do usuário. Clique em **Continuar**. O token é retornado para `https://jwt.ms` e deve ser exibido para você.

## <a name="create-a-password-reset-user-flow"></a>Criar um fluxo de usuário de redefinição de senha

Para permitir que os usuários do seu aplicativo para redefinir sua senha, você deve usar um fluxo de usuário de redefinição de senha.

1. No menu à esquerda, selecione **(políticas) de fluxos de usuário**e, em seguida, selecione **novo fluxo de usuário**.
1. Selecione o fluxo do usuário **Redefinição de senha** na guia Recomendado.
1. Insira um **Nome** para o fluxo de usuário. Por exemplo, *passwordreset1*.
1. Em **Provedores de identidade**, habilite **Redefinir senha usando endereço de email**.
1. Em Declarações do aplicativo, clique em **Mostrar mais** e escolha as declarações que você quer retornar nos tokens de autorização enviados de volta ao seu aplicativo. Por exemplo, selecione **ID de Objeto do Usuário**.
1. Clique em **OK**.
1. Clique em **Criar** para adicionar o fluxo de usuário. Um prefixo *B2C_1* é acrescentado automaticamente ao nome.

### <a name="test-the-user-flow"></a>Testar o fluxo de usuário

1. Selecione o fluxo de usuário que você criou para abrir sua página de visão geral e, em seguida, selecione **executar o fluxo de usuário**.
1. Para **Aplicativo**, selecione o aplicativo Web denominado *webapp1* que você registrou anteriormente. A **URL de resposta** deve mostrar `https://jwt.ms`.
1. Clique em **executar o fluxo de usuário**, verifique se o endereço de email da conta que você anteriormente criou e selecione **continuar**.
1. Agora, você pode alterar a senha para o usuário. Alterar a senha e selecione **continuar**. O token é retornado para `https://jwt.ms` e deve ser exibido para você.

## <a name="next-steps"></a>Próximas etapas

Neste artigo, você aprendeu a:

> [!div class="checklist"]
> * Criar um fluxo de usuário de inscrição e de entrada
> * Criar um fluxo de usuário de edição de perfil
> * Criar um fluxo de usuário de redefinição de senha

Em seguida, saiba como adicionar provedores de identidade para seus aplicativos para habilitar a entrada do usuário com provedores, como o AD do Azure, Facebook, Amazon, LinkedIn, GitHub, Microsoft ou Twitter.

> [!div class="nextstepaction"]
> [Adicionar provedores de identidade para seus aplicativos >](tutorial-add-identity-providers.md)