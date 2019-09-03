---
title: Configurar a autenticação do Facebook – Serviço de Aplicativo do Azure
description: Saiba como configurar a autenticação do Facebook para seu aplicativo do serviço de aplicativo.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/06/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: 410d769d0d9abe3a0a0f9c45e3cf67bb94ec9f4d
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70232064"
---
# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Como configurar seu aplicativo do Serviço do Aplicativo para usar o logon do Facebook
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Este tópico mostra como configurar o Serviço de Aplicativo do Azure para usar o Facebook como um provedor de autenticação.

Para concluir o procedimento neste tópico, você deve ter uma conta do Facebook com um endereço de email verificado e um número de telefone celular. Para criar uma nova conta do Facebook, vá para [facebook.com].

## <a name="register"> </a>Registrar seu aplicativo com o Facebook
1. Navegue até o site de [Desenvolvedores do Facebook] e entre com suas credenciais de conta do Facebook.
3. Adicional Se você não tiver uma conta do Facebook para desenvolvedores,clique em introdução e siga as etapas de registro.
4. Clique em **meus aplicativos** > **Adicionar novo aplicativo**.
5. Em **Nome de Exibição**, digite um nome exclusivo para seu aplicativo. Forneça também seu **Email de Contato** e clique em **Criar ID do Aplicativo** e conclua a verificação de segurança. O painel do desenvolvedor para seu novo aplicativo do Facebook é aberto.
6. Clique **em painel** > **de** > logon do Facebook configurar Web. > 
1. No painel de navegação à esquerda, em **logon do Facebook**, clique em **configurações**.
1. Em **URIs**de redirecionamento OAuth `https://<app-name>.azurewebsites.net/.auth/login/facebook/callback` válidos, digite e substitua  *\<o nome de aplicativo >* pelo nome do seu aplicativo de serviço de Azure app. Clique em **salvar alterações**.
8. No painel de navegação esquerdo, clique em **Configurações** > **Básico**. No campo **segredo do aplicativo** , clique em **Mostrar**. Copie os valores da **ID do aplicativo** e do **segredo do aplicativo**. Você os usará posteriormente para configurar seu aplicativo do serviço de aplicativo no Azure.
   
   > [!IMPORTANT]
   > O segredo do aplicativo é uma credencial de segurança importante. Não compartilhe essa senha com ninguém nem distribua-a em um aplicativo cliente.
   > 
   > 
9. A conta do Facebook que você usou para registrar o aplicativo é um administrador do aplicativo. Neste ponto, apenas os administradores podem entrar neste aplicativo. Para autenticar outras contas do Facebook, clique em **revisão do aplicativo** e habilite **tornar \<seu nome de aplicativo > público** para habilitar o acesso público geral usando a autenticação do Facebook.

## <a name="secrets"> </a>Adicionar informações do Facebook ao seu aplicativo
1. Entre no [portal do Azure] e navegue até o aplicativo do serviço de aplicativo. Clique em **Configurações** > **Autenticação/Autorização** e verifique se a **Autenticação do Serviço de Aplicativo** está **Ativada**.
2. Clique em **Facebook**, Cole os valores de ID do aplicativo e segredo do aplicativo que você obteve anteriormente, opcionalmente, habilite todos os escopos necessários para seu aplicativo e clique em **OK**.
   
    ![][0]
   
    Por padrão, o Serviço de Aplicativo fornece autenticação, mas não restringe o acesso autorizado ao conteúdo do site e às APIs. Você deve autorizar os usuários no código do aplicativo.
3. (Opcional) Para restringir o acesso ao seu site para apenas os usuários autenticados pelo Facebook, defina a **Ação a tomar quando a solicitação não está autenticada** para **Facebook**. Isso exige que todas as solicitações sejam autenticadas e todas as solicitações não autenticadas sejam redirecionadas ao Facebook para autenticação.
 
> [!CAUTION]
> Restringir o acesso dessa maneira se aplica a todas as chamadas para seu aplicativo, o que pode não ser desejável para aplicativos que querem um home page publicamente disponível, como em muitos aplicativos de página única. Para tais aplicativos, **Permitir solicitações anônimas (nenhuma ação)** pode ser preferível, com o aplicativo iniciando o logon manualmente, conforme descrito [aqui](overview-authentication-authorization.md#authentication-flow).

4. Ao concluir a configuração de autenticação, clique em **Salvar**.

Agora você está pronto para usar o Facebook para autenticação em seu aplicativo.

## <a name="related-content"> </a>Conteúdo relacionado
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Desenvolvedores do Facebook]: https://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: https://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Portal do Azure]: https://portal.azure.com/
