---
title: Aplicativos de cliente (biblioteca de autenticação da Microsoft) | Azure
description: Saiba mais sobre o cliente público e o cliente confidencial aplicativos na autenticação biblioteca MSAL (Microsoft).
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/25/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9d09436b9a2ac38e7b07a51f01d65769ed19d08e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66430831"
---
# <a name="public-client-and-confidential-client-applications"></a>Cliente público e aplicativos de cliente confidencial
Biblioteca de autenticação da Microsoft (MSAL) define dois tipos de clientes: clientes públicos e clientes confidenciais. Os tipos de cliente de dois são diferenciados por sua capacidade de autenticar com segurança com o servidor de autorização e manter a confidencialidade das suas credenciais de cliente. Em contraste, a biblioteca de autenticação do Azure AD (ADAL) usa o que é chamado *contexto de autenticação* (que é uma conexão com o Azure AD).

- **Aplicativos de cliente confidencial** são aplicativos que são executados em servidores (aplicativos web, aplicativos de API da Web ou até mesmo aplicativos de serviço/daemon). Eles são considerados difícil acesso e por esse motivo capazes de manter um segredo do aplicativo. Clientes confidenciais podem armazenar segredos de tempo de configuração. Cada instância do cliente tem uma configuração distinta (incluindo a ID do cliente e segredo do cliente). Esses valores são difíceis para os usuários finais extrair. Um aplicativo web é o cliente confidencial mais comuns. A ID do cliente é exposta por meio do navegador da web, mas o segredo é transmitido apenas no canal de retorno e nunca exposto diretamente.

    Aplicativos de cliente confidencial: <BR>
    ![Aplicativo Web](media/msal-client-applications/web-app.png) ![API Web](media/msal-client-applications/web-api.png) ![Daemon/serviço](media/msal-client-applications/daemon-service.png)

- **Aplicativos de cliente público** são aplicativos que são executados em dispositivos ou computadores desktop ou em um navegador da web. Eles não são confiáveis para manter com segurança segredos do aplicativo, para que eles só acessar APIs da Web em nome do usuário. (Eles suportam apenas fluxos de cliente público). Clientes públicos não podem armazenar segredos de tempo de configuração, que eles não precisem segredos de cliente.

    Aplicativos de cliente público: <BR>
    ![Aplicativo de desktop](media/msal-client-applications/desktop-app.png) ![API Browserless](media/msal-client-applications/browserless-app.png) ![aplicativo móvel](media/msal-client-applications/mobile-app.png)

> [!NOTE]
> No msal, não há nenhuma separação de aplicativos clientes públicos e confidenciais.  Msal representando aplicativos cliente como aplicativos baseados em agente usuário, no qual o código do cliente é executado em um agente do usuário, como um navegador da web de clientes públicos. Esses clientes não armazenarem segredos, como o contexto do navegador é acessível abertamente.

## <a name="comparing-the-client-types"></a>Comparando os tipos de cliente
Aqui estão algumas semelhanças e diferenças entre o cliente público e o cliente confidencial aplicativos:

- Os dois tipos de aplicativo mantêm um cache de token de usuário e podem adquirir um token silenciosamente (quando o token já está no cache de token). Aplicativos de cliente confidencial também têm um cache de token de aplicativo para tokens que são para o aplicativo em si.
- Ambos os tipos de aplicativo gerenciar contas de usuário e podem obter uma conta do cache de token de usuário, obter uma conta de seu identificador ou remover uma conta.
- Aplicativos de cliente público têm quatro maneiras para adquirir um token (quatro fluxos de autenticação). Aplicativos de cliente confidencial têm três maneiras de adquirir um token (e autorizar o ponto de extremidade de uma maneira para calcular a URL do provedor de identidade). Para obter mais informações, consulte [adquirir tokens](msal-acquire-cache-tokens.md).

Se você já usou a ADAL, você pode observar que, ao contrário do contexto de autenticação da ADAL, na MSAL a ID do cliente (também chamado de *ID do aplicativo* ou *app ID*) é passada uma vez na construção do aplicativo. Ele não precisa ser passado novamente quando o aplicativo adquire um token. Isso é verdadeiro para ambos para aplicativos clientes públicos e confidenciais. Construtores de aplicativos de cliente confidencial também são passados para as credenciais do cliente: o segredo que compartilham com o provedor de identidade.

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre:
- [Opções de configuração de aplicativo do cliente](msal-client-application-configuration.md)
- [Criando uma instância de aplicativos cliente usando MSAL.NET](msal-net-initializing-client-applications.md)
- [Criando uma instância de aplicativos cliente usando msal](msal-js-initializing-client-applications.md)
