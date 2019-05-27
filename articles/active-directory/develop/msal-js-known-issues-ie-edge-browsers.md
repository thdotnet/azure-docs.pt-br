---
title: Alguns problemas conhecidos no navegadores (biblioteca de autenticação do Microsoft para JavaScript) | Azure
description: Saiba mais sobre problemas conhecidos ao usar a biblioteca de autenticação da Microsoft para JavaScript (msal) com os navegadores Internet Explorer e Microsoft Edge.
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
ms.openlocfilehash: c57ed956ec50c8bac26720a27894c07353928336
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873894"
---
# <a name="known-issues-on-internet-explorer-and-microsoft-edge-browsers-with-msaljs"></a>Problemas conhecidos nos navegadores Internet Explorer e Microsoft Edge com msal

## <a name="issues-due-to-security-zones"></a>Problemas devido a zonas de segurança
Tivemos vários relatórios de problemas com a autenticação no IE e Microsoft Edge (desde a atualização do *versão do navegador Microsoft Edge para 40.15063.0.0*). Estamos acompanhando esses e ter informado a equipe do Microsoft Edge. Enquanto o Microsoft Edge funciona em uma resolução, aqui está uma descrição dos problemas que ocorrem com frequência e as possíveis soluções alternativas que podem ser implementadas.

### <a name="cause"></a>Causa
A causa para a maioria desses problemas é da seguinte maneira. O armazenamento de sessão e o armazenamento local são particionados por zonas de segurança no navegador Microsoft Edge. Nesta versão específica do Microsoft Edge, quando o aplicativo é redirecionado em zonas, o armazenamento de sessão e o armazenamento local são desmarcadas. Especificamente, o armazenamento de sessão está desmarcado no painel de navegação normal do navegador e a sessão e o armazenamento local são limpas no modo do navegador InPrivate. Msal salva determinado estado no armazenamento de sessão e se baseia na verificação nesse estado durante os fluxos de autenticação. Quando o armazenamento de sessão é desmarcado, esse estado é perdido e, portanto, resulta em experiências quebradas.

### <a name="issues"></a>Questões

- **Recarrega a página e loops de redirecionamento infinitos durante a autenticação**. Quando os usuários entram aplicativo no Microsoft Edge, eles são redirecionados de volta na página de logon do AAD e são preso em um loop de redirecionamento infinito, resultando em recargas de página repetidas. Isso geralmente é acompanhado por um `invalid_state` erro no armazenamento de sessão.

- **Infinito adquirir token loops e erro AADSTS50058**. Quando um aplicativo em execução no Microsoft Edge tenta adquirir um token para um recurso, o aplicativo poderá ficar preso em um loop infinito da aquisição token chamada juntamente com o seguinte erro do AAD no seu rastreamento de rede:

    `Error :login_required; Error description:AADSTS50058: A silent sign-in request was sent but no user is signed in. The cookies used to represent the user's session were not sent in the request to Azure AD. This can happen if the user is using Internet Explorer or Edge, and the web app sending the silent sign-in request is in different IE security zone than the Azure AD endpoint (login.microsoftonline.com)`

- **Janela pop-up não fecha ou está preso ao usar o logon por meio do pop-up para autenticar**. Ao fazer a autenticação por meio da janela pop-up no Microsoft Edge ou IE(InPrivate), depois de inserir as credenciais e entrar, se vários domínios em zonas de segurança estão envolvidos no painel de navegação, a janela pop-up não fecha porque perde o identificador para a msal a janela pop-up.  

    Aqui estão links para esses problemas em que o rastreador de problemas do Microsoft Edge:  
    - [Bug 13861050](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/13861050/)
    - [Bug 13861663](https://developer.microsoft.com/en-us/microsoft-edge/platform/issues/13861663/)

### <a name="update-fix-available-in-msaljs-023"></a>Atualização: Correção disponível na msal 0.2.3
Correções para os problemas de loop de redirecionamento de autenticação que foram liberados nos [msal 0.2.3](https://github.com/AzureAD/microsoft-authentication-library-for-js/releases). Habilitar o sinalizador `storeAuthStateInCookie` na configuração msal para tirar proveito dessa correção. Por padrão, esse sinalizador é definido como false.

Quando o `storeAuthStateInCookie` sinalizador estiver habilitado, a msal usará os cookies do navegador para armazenar o estado de solicitação necessário para a validação dos fluxos de autenticação.

> [!NOTE]
> Essa correção ainda não está disponível para os wrappers msal angular e angularjs msal. Essa correção não resolver o problema com janelas pop-up.

Use soluções alternativas abaixo.

#### <a name="other-workarounds"></a>Outras soluções alternativas
Certifique-se de que o problema está ocorrendo apenas na versão específica do navegador Microsoft Edge e funciona em outros navegadores antes de adotar essas soluções alternativas de teste.  
1. Como uma primeira etapa para resolver esses problemas, certifique-se de que o domínio de aplicativo, e quaisquer outros sites envolvidos nos redirecionamentos do fluxo de autenticação são adicionados como sites confiáveis nas configurações de segurança do navegador, para que eles pertencem à mesma zona de segurança.
Para fazer isso, siga estas etapas:
    - Abra **Internet Explorer** e clique no **configurações** (ícone de engrenagem) no canto superior direito
    - Selecione **opções da Internet**
    - Selecione o **segurança** guia
    - Sob o **Sites confiáveis** opção, clique no **sites** botão e adicione as URLs na caixa de diálogo que é aberta.

2. Como mencionado antes, desde apenas a sessão de armazenamento é removido durante a navegação regular, você pode configurar o msal para usar o armazenamento local em vez disso. Isso pode ser definido como o `cacheLocation` parâmetro config ao inicializar a MSAL.

Observe que isso não resolverá o problema para a navegação InPrivate, pois a sessão e o armazenamento local são desmarcadas.

## <a name="issues-due-to-popup-blockers"></a>Problemas devido à bloqueadores de pop-up

Há casos em que pop-ups estão bloqueados no IE ou Microsoft Edge, por exemplo, quando um pop-up segundo ocorre durante a autenticação multifator. Você receberá um alerta no navegador para permitir pop-up uma vez ou sempre. Se você optar por permitir, o navegador abre a janela pop-up automaticamente e retorna um `null` manipular para ele. Como resultado, a biblioteca não tem um identificador para a janela e não há nenhuma maneira para fechar a janela pop-up. O mesmo problema não ocorre no Chrome quando ele solicita que você permitir pop-ups, porque ele não abre automaticamente uma janela pop-up.

Como uma **solução alternativa**, os desenvolvedores precisarão permitir pop-ups no IE e Microsoft Edge, antes de começar a usar seu aplicativo para evitar esse problema.

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre [usando a msal no Internet Explorer](msal-js-use-ie-browser.md).
