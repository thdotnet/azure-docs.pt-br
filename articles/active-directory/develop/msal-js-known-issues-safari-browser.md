---
title: Alguns problemas conhecidos no navegadores (biblioteca de autenticação do Microsoft para JavaScript) | Azure
description: Saiba mais sobre problemas conhecidos ao usar a biblioteca de autenticação da Microsoft para JavaScript (msal) com o navegador Safari.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: cb89b1ef4dbbef234fba3152d7f85bbadfbdc64a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65873879"
---
# <a name="known-issues-on-safari-browser-with-msaljs"></a>Problemas conhecidos no navegador Safari com msal 

## <a name="silent-token-renewal-on-safari-12-and-itp-20"></a>Renovação de token silenciosa no Safari 12 e ITP 2.0

Apple iOS 12 e sistemas de operacionais MacOS 10.14 incluída uma versão dos [navegador Safari 12](https://developer.apple.com/safari/whats-new/). Para fins de segurança e privacidade, Safari 12 inclui o [inteligente 2.0 de prevenção de acompanhamento](https://webkit.org/blog/8311/intelligent-tracking-prevention-2-0/). Isso essencialmente faz com que o navegador descartar os cookies de terceiros que está sendo definidos. ITP 2.0 também trata os cookies definidos por provedores de identidade como cookies de terceiros.

### <a name="impact-on-msaljs"></a>Impacto na msal

Msal usa um Iframe oculto para executar a aquisição de token silenciosa e renovação como parte do `acquireTokenSilent` chamadas. As solicitações de token silenciosas contam com o Iframe que têm acesso à sessão do usuário autenticado representado pelos cookies definidos pelo AD do Azure. Com o 2.0 ITP impedindo o acesso a esses cookies, msal falhará silenciosamente adquirir e renovar tokens e isso resulta em `acquireTokenSilent` falhas.

Não há solução para esse problema no momento e estamos avaliando as opções com a comunidade de padrões.

### <a name="work-around"></a>Contornar

Por padrão, a configuração de ITP está habilitada no navegador Safari. Você pode desabilitar essa configuração, navegando até **preferências** -> **privacidade** e desmarcando o **impedir que o rastreamento de sites** opção.

![configuração do Safari](./media/msal-js-known-issue-safari-browser/safari.png)

Você precisará lidar com o `acquireTokenSilent` falhas com interativo adquirem token chamada, que solicita ao usuário para entrar.
Para evitar entradas repetidas, uma abordagem que você pode implementar é manipular o `acquireTokenSilent` falha e fornecer ao usuário uma opção para desabilitar a configuração de ITP no Safari antes de prosseguir com a chamada interativa. Depois que a configuração estiver desabilitada, renovações de token silenciosas subsequentes devem ser bem-sucedidos.
