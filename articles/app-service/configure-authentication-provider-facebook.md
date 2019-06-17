---
title: Configurar a autenticação do Facebook – Serviço de Aplicativo do Azure
description: Saiba como configurar a autenticação do Facebook para seu aplicativo de Serviços de Aplicativos.
services: app-service
documentationcenter: ''
author: mattchenderson
manager: syntaxc4
editor: ''
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 06/06/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: e91d55c29d325301b8ac70ddc63fb408961fbb2c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66742973"
---
# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Como configurar seu aplicativo do Serviço do Aplicativo para usar o logon do Facebook
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Este tópico mostra como configurar o Serviço de Aplicativo do Azure para usar o Facebook como um provedor de autenticação.

Para concluir o procedimento neste tópico, você deve ter uma conta do Facebook com um endereço de email verificado e um número de telefone celular. Para criar uma nova conta do Facebook, vá para [facebook.com].

## <a name="register"> </a>Registrar seu aplicativo com o Facebook
1. Navegue até o site de [Desenvolvedores do Facebook] e entre com suas credenciais de conta do Facebook.
3. (Opcional) Se você não tiver um Facebook para conta de desenvolvedores, clique em **começar** e siga as etapas de registro.
4. Clique em **meus aplicativos** > **adicionar novo aplicativo**.
5. Em **Nome de Exibição**, digite um nome exclusivo para seu aplicativo. Forneça também seu **Email de Contato** e clique em **Criar ID do Aplicativo** e conclua a verificação de segurança. Isso o levará ao painel do desenvolvedor de seu novo aplicativo do Facebook.
6. Clique em **Dashboard** > **logon do Facebook** > **configurar** > **Web**.
1. No painel de navegação à esquerda sob **logon do Facebook**, clique em **configurações**.
1. Na **URIs de redirecionamento OAuth válidos**, digite `https://<app-name>.azurewebsites.net/.auth/login/facebook/callback` e substituir  *\<nome do aplicativo >* com o nome do seu aplicativo de serviço de aplicativo do Azure. Clique em **salvar alterações**.
8. No painel de navegação esquerdo, clique em **Configurações** > **Básico**. Sobre o **segredo do aplicativo** , clique em **Mostrar**. Copie os valores de **ID do aplicativo** e **segredo do aplicativo**. Você usá-los mais tarde para configurar seu aplicativo de serviço de aplicativo no Azure.
   
   > [!IMPORTANT]
   > O segredo do aplicativo é uma credencial de segurança importante. Não compartilhe essa senha com ninguém nem distribua-a em um aplicativo cliente.
   > 
   > 
9. A conta do Facebook usada para registrar o aplicativo é um administrador do aplicativo. Neste ponto, apenas os administradores podem entrar neste aplicativo. Para autenticar outras contas do Facebook, clique em **revisão do aplicativo** e habilite **tornar \<-nome do seu aplicativo > público** para habilitar o acesso ao público geral usando a autenticação do Facebook.

## <a name="secrets"> </a>Adicionar informações do Facebook ao seu aplicativo
1. Entrar para o [portal do Azure] e navegue até seu aplicativo de serviço de aplicativo. Clique em **Configurações** > **Autenticação/Autorização** e verifique se a **Autenticação do Serviço de Aplicativo** está **Ativada**.
2. Clique em **Facebook**, cole os valores do ID do Aplicativo e do Segredo do Aplicativo obtidos anteriormente e, como opção, habilite os escopos necessários para seu aplicativo, então, clique em **OK**.
   
    ![][0]
   
    Por padrão, o Serviço de Aplicativo fornece autenticação, mas não restringe o acesso autorizado ao conteúdo do site e às APIs. Você deve autorizar os usuários no código do aplicativo.
3. (Opcional) Para restringir o acesso ao seu site para apenas os usuários autenticados pelo Facebook, defina a **Ação a tomar quando a solicitação não está autenticada** para **Facebook**. Isso exige que todas as solicitações sejam autenticadas e todas as solicitações não autenticadas sejam redirecionadas ao Facebook para autenticação.
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
