---
title: Evite recargas de página (biblioteca de autenticação do Microsoft para JavaScript) | Azure
description: Saiba como evitar recargas de página ao adquirir e renovação de tokens silenciosamente usando a biblioteca de autenticação da Microsoft para JavaScript (msal).
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
ms.date: 05/29/2019
ms.author: nacanuma
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 162811221e6dde89ad11f358b2ec8f32f3c82522
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66420462"
---
# <a name="avoid-page-reloads-when-acquiring-and-renewing-tokens-silently-using-msaljs"></a>Evite recargas de página ao adquirir e renovação de tokens silenciosamente usando msal
Biblioteca de autenticação da Microsoft para JavaScript (msal) Use ocultada `iframe` elementos para adquirir e renovar tokens silenciosamente em segundo plano. Azure AD retorna o token de volta para o redirect_uri registrados especificado na solicitação de token (por padrão essa é a página do aplicativo raiz). Uma vez que a resposta é uma 302, isso resulta em HTML correspondente para o `redirect_uri` sendo carregados no `iframe`. Do normalmente, o aplicativo `redirect_uri` é a página raiz e isso faz com que a recarregar.

Em outros casos, se navegar até a página do aplicativo raiz requer autenticação, isso pode levar para aninhados `iframe` elementos ou `X-Frame-Options: deny` erro.

Como o msal não é possível ignorar o 302 emitido pelo Azure AD e é necessário para processar o token retornado, ele não pode impedir que o `redirect_uri` obtendo carregadas no `iframe`.

Para evitar o aplicativo inteiro recarregando novamente ou outros erros causados devido a isso, siga essas soluções alternativas.

## <a name="specify-different-html-for-the-iframe"></a>Especificar diferente HTML para o iframe

Defina o `redirect_uri` propriedade na configuração para uma página simple, que não requer autenticação. Você precisa certificar-se de que ele corresponda com a `redirect_uri` registrado no portal do Azure. Isso não afetará experiência de logon do usuário, como MSAL salva a página inicial quando o usuário inicia o processo de logon e redireciona de volta para o local exato após a conclusão do logon.

## <a name="initialization-in-your-main-app-file"></a>Inicialização em seu arquivo de aplicativo principal

Se seu aplicativo é estruturado de maneira que há um arquivo Javascript central que define a inicialização do aplicativo, roteamento e outras coisas, você pode carregar condicionalmente seus módulos de aplicativo com base no aplicativo que está sendo carregado em um `iframe` ou não. Por exemplo:

In AngularJS: app.js

```javascript
// Check that the window is an iframe and not popup
if (window !== window.parent && !window.opener) {
angular.module('todoApp', ['ui.router', 'MsalAngular'])
    .config(['$httpProvider', 'msalAuthenticationServiceProvider','$locationProvider', function ($httpProvider, msalProvider,$locationProvider) {
        msalProvider.init(
            // msal configuration
        );

        $locationProvider.html5Mode(false).hashPrefix('');
    }]);
}
else {
    angular.module('todoApp', ['ui.router', 'MsalAngular'])
        .config(['$stateProvider', '$httpProvider', 'msalAuthenticationServiceProvider', '$locationProvider', function ($stateProvider, $httpProvider, msalProvider, $locationProvider) {
            $stateProvider.state("Home", {
                url: '/Home',
                controller: "homeCtrl",
                templateUrl: "/App/Views/Home.html",
            }).state("TodoList", {
                url: '/TodoList',
                controller: "todoListCtrl",
                templateUrl: "/App/Views/TodoList.html",
                requireLogin: true
            })

            $locationProvider.html5Mode(false).hashPrefix('');

            msalProvider.init(
                // msal configuration
            );
        }]);
}
```

No Angular: TS

```javascript
// Imports...
@NgModule({
  declarations: [
    AppComponent,
    MsalComponent,
    MainMenuComponent,
    AccountMenuComponent,
    OsNavComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    HttpClientModule,
    ServiceWorkerModule.register('ngsw-worker.js', { enabled: environment.production }),
    MsalModule.forRoot(environment.MsalConfig),
    SuiModule,
    PagesModule
  ],
  providers: [
    HttpServiceHelper,
    {provide: HTTP_INTERCEPTORS, useClass: MsalInterceptor, multi: true},
    AuthService
  ],
  entryComponents: [
    AppComponent,
    MsalComponent
  ]
})
export class AppModule {
  constructor() {
    console.log('APP Module Constructor!');
  }

  ngDoBootstrap(ref: ApplicationRef) {
    if (window !== window.parent && !window.opener)
    {
      console.log("Bootstrap: MSAL");
      ref.bootstrap(MsalComponent);
    }
    else
    {
    //this.router.resetConfig(RouterModule);
      console.log("Bootstrap: App");
      ref.bootstrap(AppComponent);
    }
  }
}
```

MsalComponent:

```javascript
import { Component} from '@angular/core';
import { MsalService } from '@azure/msal-angular';

// This component is used only to avoid Angular reload
// when doing acquireTokenSilent()

@Component({
  selector: 'app-root',
  template: '',
})
export class MsalComponent {
  constructor(private Msal: MsalService) {
  }
}
```

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre [criando um aplicativo de página única (SPA)](scenario-spa-overview.md) usando msal.