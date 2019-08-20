---
title: Configure a inscrição e a entrada com uma conta do GitHub-Azure Active Directory B2C
description: Forneça a inscrição e entrada aos consumidores com contas do GitHub em seus aplicativos usando o Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 056570db89fbe1a3db55c138b46e5b73acc282f8
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69622434"
---
# <a name="set-up-sign-up-and-sign-in-with-a-github-account-using-azure-active-directory-b2c"></a>Configurar a inscrição e entrada com a conta do GitHub usando o Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="create-a-github-oauth-application"></a>Crie um aplicativo GitHub OAuth

Para usar uma conta do GitHub como um [provedor de identidade](active-directory-b2c-reference-oauth-code.md) no Azure AD (Azure Active Directory) B2C, você precisará criar um aplicativo no locatário que o representa. Se você ainda não tiver uma conta do GitHub, poderá se inscrever [https://www.github.com/](https://www.github.com/)em.

1. Entrar no site de [Desenvolvedor do GitHub](https://github.com/settings/developers) com suas credenciais do GitHub.
1. Selecione **Aplicativos do OAuth** e, em seguida, selecione **Novo Aplicativo do OAuth**.
1. Insira um **nome do Aplicativo** e **URL da home page**.
1. Insira `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` em **URL de retorno de chamada de autorização**. Substitua `your-tenant-name` pelo nome de seu locatário do Azure AD B2C. Use todas as letras minúsculas, ao inserir o nome do locatário, mesmo se o locatário estiver definido com letras maiúsculas no Azure AD B2C.
1. Clique em **Registrar aplicativo**.
1. Copie os valores de **ID do cliente** e **Segredo do cliente**. Você precisará delas para adicionar o provedor de identidade ao locatário.

## <a name="configure-a-github-account-as-an-identity-provider"></a>Configurar a conta do GitHub como um provedor de identidade

1. Entre no [portal do Azure](https://portal.azure.com/) como administrador global do locatário Azure AD B2C.
1. Verifique se você está usando o diretório que contém seu locatário de Azure AD B2C selecionando o **diretório +** filtro de assinatura no menu superior e escolhendo o diretório que contém seu locatário.
1. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
1. Selecione **provedores de identidade**e selecione **GitHub (versão prévia)** .
1. Insira um **Nome**. Por exemplo, *GitHub*.
1. Para a **ID do cliente**, insira a ID do cliente do aplicativo GitHub que você criou anteriormente.
1. Para o **segredo do cliente**, insira o segredo do cliente que você registrou.
1. Clique em **Salvar**.
