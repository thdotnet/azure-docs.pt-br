---
title: Usar o Internet Explorer (biblioteca de autenticação da Microsoft para JavaScript) | Azure
description: Saiba mais sobre como usar a biblioteca de autenticação da Microsoft para JavaScript (msal) com o navegador Internet Explorer.
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8cf8c84120f4c90d3943cfc31ffbf9aafcec0ba3
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873909"
---
# <a name="known-issues-on-internet-explorer-and-microsoft-edge-browsers-with-msaljs"></a>Problemas conhecidos nos navegadores Internet Explorer e Microsoft Edge com msal

Biblioteca de autenticação da Microsoft para JavaScript (msal) é gerada para [JavaScript ES5](https://fr.wikipedia.org/wiki/ECMAScript#ECMAScript_Edition_5_.28ES5.29) para que ele pode ser executado no Internet Explorer. No entanto, há algumas coisas que saber.

## <a name="run-an-app-in-internet-explorer"></a>Executar um aplicativo no Internet Explorer
Se você pretende usar msal em aplicativos que podem ser executados no Internet Explorer, você precisará adicionar uma referência a um suporte retroativo de promessa antes de fazer referência o script msal.

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/bluebird/3.3.4/bluebird.min.js" class="pre"></script>
```

Isso ocorre porque o Internet Explorer não oferece suporte a JavaScript promessas nativamente.

## <a name="debugging-an-application-running-in-internet-explorer"></a>Depurando um aplicativo em execução no Internet Explorer

### <a name="running-in-production"></a>Em execução na produção
Implantando seu aplicativo para produção (por exemplo, em aplicativos Web do Azure) normalmente funciona bem, fornecido que o usuário final aceitou pop-ups. Testamos-lo com o Internet Explorer 11.

### <a name="running-locally"></a>Em execução localmente
Se você deseja executar e depurar localmente de seu aplicativo em execução no Internet Explorer, você precisará estar ciente das considerações a seguir (suponha que você deseja executar seu aplicativo como *http://localhost:1234*):

- Internet Explorer tem um mecanismo de segurança chamado "modo protegido", que impede que o msal funcione corretamente. Entre os sintomas, depois de entrar, a página pode ser redirecionada para http://localhost:1234/null.

- Para executar e depurar seu aplicativo localmente, você precisará desativar esse "modo protegido". Para isso:

    1. Clique em Internet Explorer **ferramentas** (o ícone de engrenagem).
    1. Selecione **opções da Internet** e, em seguida, o **segurança** guia.
    1. Clique no **Internet** de zona e desmarque **habilitar modo protegido (requer reinicialização do Internet Explorer)**. Internet Explorer avisa que o computador não está mais protegido. Clique em **OK**.
    1. Reinicie o Internet Explorer.
    1. Executar e depurar seu aplicativo.

Quando você terminar, restaure as configurações de segurança do Internet Explorer.  Selecione **as configurações** -> **opções da Internet** -> **segurança** -> **redefinir todas as regiões para o nível padrão**.

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre [problemas conhecidos ao usar a msal no Internet Explorer](msal-js-use-ie-browser.md).