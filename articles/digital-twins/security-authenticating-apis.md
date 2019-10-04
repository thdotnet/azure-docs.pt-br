---
title: Entender a autenticação de API usando o gêmeos digital do Azure | Microsoft Docs
description: Saiba como se conectar e autenticar com APIs usando o gêmeos digital do Azure.
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 09/30/2019
ms.openlocfilehash: c75db8d1885c8680dd316952a5f67e11dc26edb1
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949792"
---
# <a name="connect-to-and-authenticate-with-apis"></a>Conectar e autenticar com APIs

O Gêmeos Digitais do Azure usa o Active Directory do Azure (Azure AD) para autenticar usuários e proteger aplicativos. O Microsoft Azure Active Directory oferece suporte à autenticação para diversas arquiteturas modernas. Todos eles são baseados nos protocolos padrão da indústria OAuth 2.0 ou OpenID Connect. Além disso, os desenvolvedores podem usar o Microsoft Azure Active Directory para criar aplicativos de locatário único e de linha de negócios (LOB). Os desenvolvedores também podem usar o  Microsoft Azure Active Directory para desenvolver aplicativos multilocatários.

Para obter uma visão geral do Microsoft Azure Active Directory, visite a [página de fundamentos](https://docs.microsoft.com/azure/active-directory/fundamentals/) para obter guias passo a passo, conceitos e iniciações rápidas.

> [!TIP]
> Siga o [tutorial](tutorial-facilities-setup.md) para configurar e executar um aplicativo de exemplo gêmeos digital do Azure.

Para integrar um aplicativo ou serviço ao Azure AD, um desenvolvedor deve primeiro registrar o aplicativo no Azure AD. Para instruções detalhadas e screenshots, veja [este início rápido](../active-directory/develop/quickstart-register-app.md).

[Cinco cenários de aplicativos principais](../active-directory/develop/v2-app-types.md) são suportados pelo Microsoft Azure Active Directory:

* Aplicativo de página única (SPA): Um usuário precisa entrar em um aplicativo de página única protegido pelo Azure AD.
* Navegador da Web para aplicativo Web: Um usuário precisa entrar em um aplicativo Web protegido pelo Azure AD.
* Aplicativo nativo para API da Web: Um aplicativo nativo que é executado em um telefone, Tablet ou PC precisa autenticar um usuário para obter recursos de uma API Web protegida pelo Azure AD.
* Aplicativo Web para API da Web: um aplicativo Web precisa obter recursos de uma API Web protegida pelo Azure AD.
* Aplicativo de daemon ou de servidor para API da Web: Um aplicativo daemon ou um aplicativo de servidor sem interface do usuário da Web precisa obter recursos de uma API Web protegida pelo Azure AD.

> [!IMPORTANT]
> O Azure digital gêmeos dá suporte a ambas as bibliotecas de autenticação a seguir:
> * A biblioteca de [autenticação da Microsoft mais recente (MSAL)](https://docs.microsoft.com/azure/active-directory/develop/msal-overview)
> * A [Adal (biblioteca de autenticação de Azure Active Directory)](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries)

## <a name="call-digital-twins-from-a-middle-tier-web-api"></a>Chame os Gêmeos Digitais do Azure de uma API da web de camada intermediária

Quando os desenvolvedores arquitetam os Gêmeos Digitais do Azure, eles geralmente criam um aplicativo ou API de camada intermediária. O aplicativo ou API chama o downstream dos Gêmeos Digitais do Azure. Para dar suporte a essa arquitetura da solução da web padrão, certifique-se de que os usuários primeiro:

1. Autentiquem com o aplicativo de camada intermediária

1. Um token em nome do OAuth 2.0 é adquirido durante a autenticação

1. O token adquirido é então usado para autenticar com ou chamar as APIs que são downstream ainda mais usando o fluxo Em nome de

Para obter instruções sobre como orquestrar o fluxo [Em nome de OAuth 2.0 On-Behalf-Of](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). Você também pode exibir exemplos de código em [chamar uma API da web downstream](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/).

## <a name="next-steps"></a>Próximas etapas

Para configurar e testar os Gêmeos Digitais do Azure usando o fluxo de concessão implícita do OAuth 2.0, leia [Configurar o Postman](./how-to-configure-postman.md).

Para saber mais sobre a segurança dos Gêmeos Digitais do Azure, leia [Criar e gerenciar atribuições de função](./security-create-manage-role-assignments.md).