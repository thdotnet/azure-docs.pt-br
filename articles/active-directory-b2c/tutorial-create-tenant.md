---
title: Tutorial – criar um locatário do Azure Active Directory B2C
description: Saiba como preparar para registrar os aplicativos criando um locatário do Azure Active Directory B2C usando o portal do Azure.
services: B2C
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/07/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 711b9152f9f3fa1b3573e39d1950f18b628c268a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056326"
---
# <a name="tutorial-create-an-azure-active-directory-b2c-tenant"></a>Tutorial: Criar um locatário do Azure Active Directory B2C

Antes que os aplicativos possam interagir com o Azure AD (Azure Active Directory B2C), eles deverão ser registrados em um locatário que você gerencia.

Neste artigo, você aprenderá a:

> [!div class="checklist"]
> * Criar um locatário do Azure AD B2C
> * Vincular seu locatário à sua assinatura

Saiba como registrar um aplicativo no próximo tutorial.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="create-an-azure-ad-b2c-tenant"></a>Criar um locatário do Azure AD B2C

1. Entre no [Portal do Azure](https://portal.azure.com/).
2. Verifique se que você estiver usando o diretório que contém sua assinatura. Clique o **filtro de diretório e assinatura** no menu superior, selecione o diretório que contém sua assinatura. Esse diretório é diferente daquele que conterá o seu locatário do Azure AD B2C.

    ![Alternar para o diretório de assinatura](./media/tutorial-create-tenant/switch-directory-subscription.PNG)

3. Escolha **Criar um recurso** no canto superior esquerdo do portal do Azure.
4. Pesquise e selecione **Active Directory B2C**; depois, clique em **Criar**.
5. Escolher **criar um novo locatário do Azure AD B2C** e insira um nome de organização e o nome de domínio inicial. Selecione o país/região (ele não pode ser alterado posteriormente) e, em seguida, clique em **criar**.

    O nome de domínio inicial é usado como parte do nome do locatário. Neste exemplo, é o nome do locatário *contoso0926Tenant.onmicrosoft.com*:

    ![Criar um locatário](./media/tutorial-create-tenant/create-tenant.PNG)

6. Sobre o **criar novo locatário do B2C ou vincular a locatário existente** , escolha **vincular um existente do AD B2C da Azure locatário à minha assinatura do Azure**.

    Selecione o locatário que você criou e selecione sua assinatura.

    Para o grupo de recursos, selecione **criar novo**. Insira um nome para o grupo de recursos que conterá o locatário, selecione a localização e, em seguida, clique em **Criar**.
1. Para começar a usar o novo locatário, verifique se você estiver usando o diretório que contém o seu locatário do Azure AD B2C clicando o **filtro de diretório e assinatura** no menu superior e escolher o diretório que o contém.

    Se a princípio, você não vir o novo locatário B2C do Azure, na lista, atualize a janela do navegador, selecione a **filtro de diretório e assinatura** novamente no menu superior.

    ![Alternar para o diretório do locatário](./media/tutorial-create-tenant/switch-directories.PNG)

## <a name="next-steps"></a>Próximas etapas

Neste artigo, você aprendeu a:

> [!div class="checklist"]
> * Criar um locatário do Azure AD B2C
> * Vincular seu locatário à sua assinatura

Em seguida, saiba como registrar um aplicativo web no seu novo locatário.

> [!div class="nextstepaction"]
> [Registrar seus aplicativos >](tutorial-register-applications.md)
