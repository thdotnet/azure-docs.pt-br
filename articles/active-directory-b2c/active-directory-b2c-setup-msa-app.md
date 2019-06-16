---
title: Configurar inscrição e entrar com uma conta da Microsoft - Azure Active Directory B2C
description: Forneça inscrição e entrada aos consumidores com contas da Microsoft em seus aplicativos usando o Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/11/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 63aa93e72d76d430fc7e026478d91e496f27eae6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055107"
---
# <a name="set-up-sign-up-and-sign-in-with-a-microsoft-account-using-azure-active-directory-b2c"></a>Configurar a inscrição e entrada com a conta da Microsoft usando o Azure Active Directory B2C

## <a name="create-a-microsoft-account-application"></a>Criar um aplicativo de conta da Microsoft

Para usar uma conta da Microsoft como um [provedor de identidade](active-directory-b2c-reference-oidc.md) no Azure AD (Azure Active Directory) B2C, é preciso criar um aplicativo no locatário que o representa. Se ainda não tiver uma conta da Microsoft, consiga uma conta em [https://www.live.com/](https://www.live.com/).

1. Entre no [Portal do Azure](https://portal.azure.com/).
1. Selecione **todos os serviços** no canto superior esquerdo do portal do Azure e, em seguida, procure e selecione **registros de aplicativo**.
1. Selecione **novo registro**
1. Insira um **Nome** para seu aplicativo. Por exemplo, *MSAapp1*.
1. Sob **suporte para tipos de conta**, selecione **contas em qualquer diretório organizacional e contas pessoais da Microsoft (por exemplo, o Skype, Xbox, Outlook.com)** . Essa opção destina-se o conjunto mais amplo de identidades da Microsoft.

   Para obter mais informações sobre as seleções de tipo de conta diferente, consulte [guia de início rápido: Registrar um aplicativo com a plataforma de identidade Microsoft](../active-directory/develop/quickstart-register-app.md).
1. Sob **URI de redirecionamento (opcional)** , selecione **Web** e digite `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` na caixa de texto. Substitua `your-tenant-name` pelo nome do locatário do Azure AD B2C.
1. Selecione **registrar**
1. Registro a **ID do aplicativo (cliente)** mostrada na página de visão geral do aplicativo. Você precisará dela quando você configura o provedor de identidade na próxima seção.
1. Selecione **certificados e segredos**
1. Clique em **novo segredo do cliente**
1. Insira um **descrição** para o segredo, por exemplo *senha do aplicativo 1*e, em seguida, clique em **adicionar**.
1. Registre a senha de aplicativo, mostrada na **valor** coluna. Você precisará dela quando você configura o provedor de identidade na próxima seção.

## <a name="configure-a-microsoft-account-as-an-identity-provider"></a>Configurar uma conta da Microsoft como um provedor de identidade

1. Entre no [portal do Azure](https://portal.azure.com/) como administrador global do locatário Azure AD B2C.
1. Verifique se você está usando o diretório que contém o locatário do Azure AD B2C clicando no **filtro Diretório e assinatura** no menu superior e escolhendo o diretório que contém seu locatário.
1. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
1. Escolha **Provedores de identidade** e escolha **Adicionar**.
1. Forneça um **Nome**. Por exemplo, insira *MSA*.
1. Escolha **Tipo do provedor de identidade**, marque **Conta da Microsoft** e clique em **OK**.
1. Selecione **configurar este provedor de identidade** e insira a ID do aplicativo (cliente) que você registrou anteriormente na **ID do cliente** texto caixa e insira o segredo do cliente que você registrou no **cliente segredo** caixa de texto.
1. Clique em **OK** e em **Criar** para salvar a configuração da conta da Microsoft.
