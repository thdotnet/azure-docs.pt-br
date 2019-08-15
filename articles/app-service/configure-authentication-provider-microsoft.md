---
title: Configurar a autenticação de Conta Microsoft – Serviço de Aplicativo do Azure
description: Saiba como configurar a autenticação de conta da Microsoft para o seu aplicativo de Serviços de Aplicativos.
author: mattchenderson
services: app-service
documentationcenter: ''
manager: syntaxc4
editor: ''
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/08/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 729c235ee51a904714358221389703632fd8296c
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69033642"
---
# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Como configurar seu aplicativo do Serviço de Aplicativo para usar o logon da Conta da Microsoft
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Este tópico mostra como configurar o Serviço de Aplicativo do Azure para usar a conta da Microsoft como um provedor de autenticação. 

## <a name="register-microsoft-account"> </a>Registrar seu aplicativo na conta da Microsoft
1. Entre no [portal do Azure]e navegue até seu aplicativo. 

<!-- Copy your **URL**, which you will use later to configure your app with Microsoft Account. -->
1. Navegue até [**registros de aplicativo**](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/ApplicationsListBlade)e entre com sua conta Microsoft, se solicitado.

1. Clique em **novo registro**e digite um nome de aplicativo.

1. Em **URIs**de redirecionamento, selecione **Web**e `https://<app-domain-name>/.auth/login/microsoftaccount/callback supply the endpoint for your application`, em seguida, digite. *Substitua\<app-Domain-Name >* pelo nome de domínio do seu aplicativo.  Por exemplo, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`. 

   > [!NOTE]
   > Use o esquema HTTPS na URL.

1. Selecione **registrar**. 

1. Copie a **ID do aplicativo (cliente)** . Isso será necessário mais tarde. 
   
7. No painel de navegação à esquerda do novo registro de aplicativo, selecione **certificados & segredos** > **novo segredo do cliente**. Forneça uma descrição, selecione a duração da validade e selecione **Adicionar**.

1. Copie o valor que aparece na página **certificados & segredos** . Depois que você sair da página, ela não será exibida novamente.

    > [!IMPORTANT]
    > A senha é uma credencial de segurança importante. Não compartilhe a senha com ninguém nem distribua-a em um aplicativo cliente.

## <a name="secrets"> </a>Adicionar informações da conta da Microsoft ao seu aplicativo de Serviço de Aplicativo
1. No [Portal do Azure], navegue até o seu aplicativo. No painel de navegação esquerdo, clique em **autenticação/autorização**.

2. Se o recurso autenticação/autorização não estiver habilitado, selecione **ativado**.

3. Em **provedores de autenticação**, selecione **conta da Microsoft**. Cole a ID do aplicativo (cliente) e o segredo do cliente obtidos anteriormente e, opcionalmente, habilite todos os escopos exigidos pelo aplicativo. Clique em **OK**.

    Por padrão, o Serviço de Aplicativo fornece autenticação, mas não restringe o acesso autorizado ao conteúdo do site e às APIs. Você deve autorizar os usuários no código do aplicativo.

4. Adicional Para restringir o acesso a conta Microsoft usuários, defina **a ação a ser tomada quando a solicitação não for autenticada** para **fazer logon com a conta da Microsoft**. Isso exige que todas as solicitações sejam autenticadas e todas as solicitações não autenticadas sejam redirecionadas à conta da Microsoft para autenticação.

> [!CAUTION]
> Restringir o acesso dessa maneira se aplica a todas as chamadas para seu aplicativo, o que pode não ser desejável para aplicativos que querem um home page publicamente disponível, como em muitos aplicativos de página única. Para tais aplicativos, **Permitir solicitações anônimas (nenhuma ação)** pode ser preferível, com o aplicativo iniciando o logon manualmente, conforme descrito [aqui](overview-authentication-authorization.md#authentication-flow).

5. Clique em **Salvar**.

Agora você está pronto para usar uma conta da Microsoft para autenticação em seu aplicativo.

## <a name="related-content"> </a>Conteúdo relacionado
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- URLs. -->

[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Portal do Azure]: https://portal.azure.com/
