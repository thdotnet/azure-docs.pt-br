---
title: Configurar a inscrição e a entrada com uma conta do LinkedIn-Azure Active Directory B2C
description: Forneça a inscrição e entrada aos consumidores com contas do LinkedIn em seus aplicativos usando o Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/08/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: be347fe33e03b4c2a7ecc1015a407c5a58062326
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71065141"
---
# <a name="set-up-sign-up-and-sign-in-with-a-linkedin-account-using-azure-active-directory-b2c"></a>Configurar a inscrição e entrada com a conta do LinkedIn usando o Azure Active Directory B2C

## <a name="create-a-linkedin-application"></a>Criar um aplicativo do LinkedIn

Para usar uma conta do LinkedIn como um [provedor de identidade](active-directory-b2c-reference-oauth-code.md) no Azure Active Directory B2C (Azure ad B2C), você precisa criar um aplicativo em seu locatário que o represente. Se você ainda não tiver uma conta do LinkedIn, poderá se inscrever [https://www.linkedin.com/](https://www.linkedin.com/)em.

1. Entre no [site de Desenvolvedores do LinkedIn](https://www.developer.linkedin.com/) com suas credencias de conta do LinkedIn.
1. Escolha **Meus Aplicativos** e clique em **Criar Aplicativo**.
1. Insira **Nome da Empresa**, **Nome do Aplicativo**, **Descrição do Aplicativo**, **Logotipo do Aplicativo**, **Uso do Aplicativo**, **URL do Site**, **Email Comercial** e **Telefone Comercial**.
1. Aceite os **Termos de Uso da API do LinkedIn** e clique em **Enviar**.
1. Copie os valores de **ID do cliente** e **Segredo do cliente**. Você pode encontrá-los em **Chaves de Autenticação**. Você precisará de ambos para configurar o LinkedIn como um provedor de identidade no seu locatário. **Segredo do Cliente** é uma credencial de segurança importante.
1. Insira `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp` em **URLs de Redirecionamento Autorizadas**. Substitua `your-tenant-name` pelo nome do seu locatário. Todas as letras que você usar ao inserir o nome do locatário precisarão ser minúsculas, mesmo se o locatário estiver definido com letras maiúsculas no Azure AD B2C. Selecione **Adicionar** e depois clique em **Atualizar**.

## <a name="configure-a-linkedin-account-as-an-identity-provider"></a>Configurar uma conta do LinkedIn como um provedor de identidade

1. Entre no [portal do Azure](https://portal.azure.com/) como administrador global do locatário Azure AD B2C.
1. Verifique se você está usando o diretório que contém seu locatário de Azure AD B2C selecionando o **diretório +** filtro de assinatura no menu superior e escolhendo o diretório que contém seu locatário.
1. Escolha **Todos os serviços** no canto superior esquerdo do portal do Azure, procure e selecione **Azure AD B2C**.
1. Selecione **provedores de identidade**e, em seguida, selecione **LinkedIn**.
1. Insira um **Nome**. Por exemplo, *LinkedIn*.
1. Para a **ID do cliente**, insira a ID do cliente do aplicativo do LinkedIn que você criou anteriormente.
1. Para o **segredo do cliente**, insira o segredo do cliente que você registrou.
1. Clique em **Salvar**.

## <a name="migration-from-v10-to-v20"></a>Migração de v 1.0 para v 2.0

O LinkedIn [atualizou recentemente suas APIs de v 1.0 para v 2.0](https://engineering.linkedin.com/blog/2018/12/developer-program-updates). Como parte da migração, Azure AD B2C só é capaz de obter o nome completo do usuário do LinkedIn durante a inscrição. Se um endereço de email for um dos atributos coletados durante a inscrição, o usuário deverá inserir manualmente o endereço de email e validá-lo.
