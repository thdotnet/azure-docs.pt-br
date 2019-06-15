---
title: API da web - registro de aplicativo protegida | Azure
description: Saiba como criar uma API Web protegida e as informações necessárias registrar o aplicativo.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 22fe71c38678ae789a93ecbc956f24f0b0ebeb01
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111120"
---
# <a name="protected-web-api---app-registration"></a>API - registro do aplicativo web protegida

Este artigo explica as especificidades de registro de aplicativo para uma API web protegida.

Confira [Início Rápido: Registrar um aplicativo com a plataforma de identidade Microsoft](quickstart-register-app.md) para obter as etapas comuns sobre como registrar o aplicativo.

## <a name="accepted-token-version"></a>Versão aceita de token

O ponto de extremidade de plataforma de identidade do Microsoft pode emitir a dois tipos de tokens: tokens da v1.0 e v2.0 tokens. Você pode aprender mais sobre esses tokens em [tokens de acesso](access-tokens.md). A versão de token aceita depende de **suporte para tipos de conta** você escolheu ao criar seu aplicativo:

- Se o valor de **suporte para tipos de conta** é **contas em qualquer diretório organizacional e contas pessoais da Microsoft (por exemplo, o Skype, Xbox, Outlook.com)** , a versão do token aceito será v 2.0.
- Caso contrário, a versão de token aceita será v1.0.

Depois de criar o aplicativo, você pode alterar a versão de token aceita seguindo estas etapas:

1. No portal do Azure, selecione seu aplicativo e, em seguida, selecione a **manifesto** para seu aplicativo.
2. No manifesto, pesquise **"accessTokenAcceptedVersion"** e veja que seu valor é **2**. Essa propriedade permite que o Azure AD sabe que a API da web aceita tokens da v2.0. Se ele estiver **nulo**, a versão do token aceito será v1.0.
3. Clique em **Salvar**.

> [!NOTE]
> Cabe à API para decidir qual versão de token (v 1.0 ou 2.0) aceita da web. Quando os clientes solicitam um token para sua API da web usando o ponto de extremidade do Microsoft identity platform v 2.0, ele obterá um token que indica qual versão é aceito pela API da web.

## <a name="no-redirect-uri"></a>Nenhum URI de redirecionamento

As APIs da Web não precisam registrar um URI de redirecionamento como nenhum usuário está conectado interativamente.

## <a name="expose-an-api"></a>Expor uma API

Outra configuração específica para APIs da web é a API exposta e os escopos expostos.

### <a name="resource-uri-and-scopes"></a>URI de recurso e escopos

Escopos são geralmente no formato `resourceURI/scopeName`. Para o Microsoft Graph, os escopos têm atalhos, como `User.Read`, mas essa cadeia de caracteres é apenas um atalho para `https://graph.microsoft.com/user.read`.

Durante o registro de aplicativo, você precisará definir os seguintes parâmetros:

- O URI de recurso - por padrão, o portal de registro de aplicativo recomenda que você use `api://{clientId}`. Esse URI de recurso é exclusivo, mas não é humana legível. Você pode alterá-lo, mas certifique-se de que ele seja exclusivo.
- Um ou mais **escopos** (para aplicativos cliente, eles aparecerão como **permissões delegadas** para sua API da Web)
- Um ou mais **funções de aplicativo** (para aplicativos cliente, eles aparecerão como **permissões de aplicativo** para sua API da Web)

Os escopos também são exibidos na tela de consentimento é apresentada aos usuários finais que usam seu aplicativo. Portanto, você precisará fornecer as cadeias de caracteres correspondentes que descrevem o escopo:

- Como visto pelo usuário final
- Como visto pelo administrador do locatário, que poderá conceder consentimento do administrador

### <a name="how-to-expose-delegated-permissions-scopes"></a>Como expor permissões (escopos)

1. Selecione o **expor uma API** seção no registro do aplicativo, e:
   1. Selecione **Adicionar um escopo**.
   1. Se solicitado, aceite a proposta URI da ID do aplicativo (api :// {clientId}), selecionando **salvar e continuar**.
   1. Insira os parâmetros s seguir:
      - Para **nome do escopo**, use `access_as_user`.
      - Para **quem pode consentir**, certifique-se a **administradores e usuários** opção está selecionada.
      - Na **nome de exibição de consentimento do administrador**, tipo `Access TodoListService as a user`.
      - Na **descrição de consentimento do administrador**, tipo `Accesses the TodoListService Web API as a user`.
      - Na **nome de exibição de consentimento do usuário**, tipo `Access TodoListService as a user`.
      - Na **descrição do consentimento do usuário**, tipo `Accesses the TodoListService Web API as a user`.
      - Manter **estado** definido como **habilitado**.
      - Selecione **adicionar escopo**.

### <a name="case-where-your-web-api-is-called-by-daemon-application"></a>Caso em que sua API Web é chamada pelo aplicativo de daemon

Neste parágrafo, você aprenderá como registrar sua API Web protegida para que possa ser chamado com segurança por aplicativos daemon:

- Você precisará expor **permissões de aplicativo**. Você só irá declarar permissões de aplicativo como aplicativos daemon não interagem com usuários e, portanto, as permissões delegadas não faria sentido.
- os administradores de locatários podem exigir o Azure AD para emitir tokens para seu aplicativo Web para apenas os aplicativos que foram registrados que desejam acessar uma das permissões de aplicativos de API da Web.

#### <a name="how-to-expose-application-permissions-app-roles"></a>Como expor permissões de aplicativo (funções de aplicativo)

Para expor permissões de aplicativo, você precisará editar o manifesto.

1. No registro do aplicativo para seu aplicativo, clique em **manifesto**.
1. Editar o manifesto, localizando o `appRoles` definindo e adicionando uma ou várias funções de aplicativo. A definição de função é fornecida no bloco JSON de exemplo abaixo.  Deixe o `allowedMemberTypes` como "Application" apenas. Certifique-se de que o **identificação** é um guid exclusivo e **displayName** e **valor** não conter espaços.
1. Salve o manifesto.

O conteúdo do `appRoles` deve ser o seguinte (o `id` pode ser qualquer GUID exclusivo)

```JSon
"appRoles": [
    {
    "allowedMemberTypes": [ "Application" ],
    "description": "Accesses the TodoListService-Cert as an application.",
    "displayName": "access_as_application",
    "id": "ccf784a6-fd0c-45f2-9c08-2f9d162a0628",
    "isEnabled": true,
    "lang": null,
    "origin": "Application",
    "value": "access_as_application"
    }
],
```

#### <a name="how-to-ensure-that-azure-ad-issues-tokens-for-your-web-api-only-to-allowed-clients"></a>Como garantir que o Azure AD emite tokens para a API Web apenas a permissão de clientes

Verifica a API da Web para a função de aplicativo (ou seja, o modo de desenvolvedor de fazê-lo). Mas você pode até mesmo configurar o Azure Active Directory para emitir um token para a API Web somente para aplicativos que foram aprovados pelo administrador do locatário para acessar a API. Para adicionar essa segurança adicional:

1. No aplicativo **visão geral** registro do aplicativo, selecione o hiperlink com o nome do seu aplicativo na **aplicativo gerenciado no diretório local**. O título para esse campo pode ser truncado. Você poderia, por exemplo, leia: `Managed application in ...`

   > [!NOTE]
   >
   > Quando você seleciona esse link, você navegará para o **visão geral do aplicativo Enterprise** página associada com a entidade de serviço para seu aplicativo no locatário em que você criou. Você pode navegar para a página de registro de aplicativo usando o botão Voltar do navegador.

1. Selecione o **propriedades** página na **gerenciar** seção das páginas de aplicativo empresarial
1. Se você quiser AAD para impor o acesso à API Web de apenas determinados clientes, defina **atribuição de usuário necessária?** à **Sim**.

   > [!IMPORTANT]
   >
   > Definindo **atribuição de usuário necessária?** à **Sim**, AAD verificará as atribuições de função de aplicativo dos clientes quando eles solicitam um token de acesso para a API da Web. Se o cliente não era ser atribuído a qualquer AppRoles, AAD retornará apenas o seguinte erro: `invalid_client: AADSTS501051: Application xxxx is not assigned to a role for the xxxx`
   >
   > Se você mantiver **atribuição de usuário necessária?** à **nenhuma**, <span style='background-color:yellow; display:inline'>Azure AD não verificar as atribuições de função de aplicativo quando um cliente solicita um token de acesso para a API Web</span>. Portanto, qualquer cliente daemon (ou seja, qualquer cliente usando o fluxo de credenciais de cliente) ainda seria capaz de obter um token de acesso para a API apenas especificando seu público-alvo. Qualquer aplicativo, seria capaz de acessar a API sem ter que solicitar permissões para ele. Agora, isso não é, em seguida, final, pois sua API Web pode sempre, conforme explicado na próxima seção, verifique se o aplicativo tem a função correta (que foi autorizada pelo administrador do Locatário), validando se o token de acesso tem um `roles` declaração e o valor correto para t sua declaração (no nosso caso, `access_as_application`).

1. Selecione **Salvar**

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Configuração de código do aplicativo](scenario-protected-web-api-app-configuration.md)
