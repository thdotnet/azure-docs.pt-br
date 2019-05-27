---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: include file
ms.openlocfilehash: a4b0c2e2d7732c6d2a4f57411731f7145cf3e73e
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65967664"
---
## <a name="register-your-application"></a>Registrar seu aplicativo

Você pode registrar seu aplicativo de duas maneiras, conforme descrito nas duas seções a seguir.

### <a name="option-1-express-mode"></a>Opção 1: Modo Expresso

Agora você precisa registrar seu aplicativo no *Portal de Registro de Aplicativos da Microsoft*:

1. Registre seu aplicativo através do [Portal de Registro de Aplicativos da Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure).
2. Insira um nome para o aplicativo e seu email.
3. Verifique se que a opção Instalação guiada está marcada.
4. Siga as instruções para obter a ID do aplicativo e colá-la no código.

### <a name="option-2-advanced-mode"></a>Opção 2: Modo Avançado

1. Vá para [Portal de registro de aplicativos da Microsoft](https://apps.dev.microsoft.com/portal/register-app).
2. Insira um nome para seu aplicativo.
3. Certifique-se de que a opção de Configuração Orientada esteja desmarcada.
4. Selecione `Add Platform`e, em seguida, selecione`Native Application`.
5. Selecione `Save`.
6. Volte para o Xcode. Em `ViewController.swift`, substitua a linha que começa com “`let kClientID`” pela ID do aplicativo que você acabou de registrar:

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
Pressione CTRL e clique em <code>Info.plist</code> para abrir o menu contextual e, em seguida, clique em: <code>Open As</code> > <code>Source Code</code>
</li>
<li>
No nó raiz <code>dict</code>, adicione o seguinte:
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Your_Application_Id_Here]</string>
        </array>
    </dict>
</array>
```
<ol start="8">
<li>
Substitua <i><code>[Your_Application_Id_Here]</code></i> pela ID do Aplicativo que você acabou de registrar
</li>
</ol>
