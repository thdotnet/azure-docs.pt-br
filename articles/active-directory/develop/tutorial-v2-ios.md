---
title: Introdução ao iOS – Plataforma de identidade da Microsoft | Azure
description: Como aplicativos iOS (Swift) podem chamar uma API que exige tokens de acesso usando a plataforma de identidade da Microsoft
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/26/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 780da9359aaf645abc9b685fa9d90bbea9199759
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65962169"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-ios-app"></a>Conectar usuários e chamar o Microsoft Graph de um aplicativo iOS

Neste tutorial, você aprenderá como criar um aplicativo iOS e integrá-lo à plataforma de identidade da Microsoft. Especificamente, este aplicativo conectará um usuário, obterá um token de acesso para chamar a API do Microsoft Graph e fará uma solicitação básica para a API do Microsoft Graph.  

Após concluir este guia, seu aplicativo aceitará conexões de contas Microsoft pessoais (incluindo outlook.com, live.com e outras) e contas corporativas ou de estudante de qualquer empresa ou organização que utilize o Azure Active Directory.

## <a name="how-this-guide-works"></a>Como funciona este guia

![Mostra como funciona o aplicativo de exemplo gerado por este tutorial](../../../includes/media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

O aplicativo nesse exemplo conectará usuários e obterá dados em seu nome.  Esses dados serão acessados por meio de uma API protegida (API do Microsoft Graph neste caso) que requer autorização e também é protegida pela plataforma de identidade da Microsoft.

Mais especificamente:

* Seu aplicativo conectará o usuário por meio de um navegador ou do Microsoft Authenticator.
* O usuário final aceitará as permissões solicitadas por seu aplicativo. 
* Será emitido um token de acesso para seu aplicativo para a API do Microsoft Graph.
* O token de acesso será incluído na solicitação HTTP para API Web.
* Processe a resposta do Microsoft Graph.

Este exemplo usa a biblioteca de autenticação da Microsoft (MSAL) para implementar a autenticação. A MSAL automaticamente renovará tokens, fornecerá o SSO entre outros aplicativos no dispositivo e ajudará a gerenciar as contas.

## <a name="prerequisites"></a>Pré-requisitos

- É necessário XCode versão 10.x para o exemplo criado neste guia. Você pode fazer o download do XCode na [URL de download do XCode](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "do site iTunes").
- Biblioteca de Autenticação da Microsoft ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)). Você pode usar o gerenciador de dependências ou adicionar manualmente. Há uma seção abaixo com [mais informações](#add-msal). 

## <a name="set-up-your-project"></a>Configurar o seu projeto

Este tutorial irá criar um novo projeto. Se, ao invés disso, você quiser baixar o tutorial concluído, [baixe o código](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip).

### <a name="create-a-new-project"></a>Criar um novo projeto

1. Abra o Xcode e selecione **Crie um novo projeto Xcode**.
2. Selecione **iOS > aplicativo de exibição única** e selecione **próxima**.
3. Dê um nome de produto e selecione **próxima**.
4. Selecione uma pasta para criar seu aplicativo e clique em *Criar*.

## <a name="register-your-application"></a>Registre seu aplicativo 

Você pode registrar seu aplicativo de duas maneiras, conforme descrito nas duas seções a seguir.

### <a name="register-your-app"></a>Registrar seu aplicativo

1. Acesse o [portal do Azure](https://aka.ms/MobileAppReg) > selecione `New registration`. 
2. Digite um **Nome** para o aplicativo > `Register`. **Não defina um URI de redirecionamento neste estágio**. 
3. Na seção `Manage`, acesse `Authentication` > `Add a platform` > `iOS`
    - Insira a ID do Pacote do seu projeto. Se você baixou o código, ele é `com.microsoft.identitysample.MSALiOS`.
4. Clique em `Configure` e armazene o `MSAL Configuration` para uso posterior. 

## <a name="add-msal"></a>Adicionar MSAL

### <a name="get-msal"></a>Obter MSAL

#### <a name="cocoapods"></a>CocoaPods

Você pode usar o [CocoaPods](https://cocoapods.org/) para instalar a `MSAL` adicionando-a ao seu `Podfile` abaixo do destino:

```
use_frameworks!

target '<your-target-here>' do
   pod 'MSAL', '~> 0.4.0'
end
```

#### <a name="carthage"></a>Carthage

Você pode usar o [Carthage](https://github.com/Carthage/Carthage) para instalar a `MSAL` adicionando-a ao seu `Cartfile`: 

```
github "AzureAD/microsoft-authentication-library-for-objc" "master"
```

#### <a name="manually"></a>Manualmente

Você também pode usar o Submódulo Git ou verificar a versão mais recente e usá-la como estrutura em seu aplicativo.

### <a name="add-your-app-registration"></a>Adicionar o registro do seu aplicativo

Em seguida, adicione o registro do aplicativo ao código. Adicione ***ID do cliente/aplicativo*** ao `ViewController.swift`:

```swift
let kClientID = "Your_Application_Id_Here"

// Additional variables for Auth and Graph API
let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
let kAuthority = "https://login.microsoftonline.com/common"
var accessToken = String()
var applicationContext : MSALPublicClientApplication?
```

### <a name="configure-url-schemes"></a>Configurar esquemas de URL

Registre `CFBundleURLSchemes` para permitir um retorno de chamada para que o usuário possa ser redirecionado para o aplicativo depois de conectar.

`LSApplicationQueriesSchemes` permite usar seu aplicativo para usar o Microsoft Authenticator, se disponível. 

Para fazer isso, abra `Info.plist` como código-fonte e adicione o seguinte, substituindo ***BUNDLE_ID*** pelo que você configurou no portal do Azure.

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msauth.[BUNDLE_ID]</string>
        </array>
    </dict>
</array>
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>msauth</string>
    <string>msauthv2</string>
</array>
```

### <a name="import-msal"></a>Importar MSAL

Importe a estrutura da MSAL nos arquivos `ViewController.swift` e `AppDelegate.swift`:

```swift
import MSAL
```

## <a name="create-your-apps-ui"></a>Criar a interface do usuário do seu aplicativo

Neste tutorial, é preciso criar:

- Botão Chamar API do Graph
- Botão Sair
- Textview log

Em `ViewController.swift`, defina propriedades e `initUI()` da seguinte forma:

```swift

var loggingText: UITextView!
var signOutButton: UIButton!
var callGraphButton: UIButton!

func initUI() {
        // Add call Graph button
        callGraphButton  = UIButton()
        callGraphButton.translatesAutoresizingMaskIntoConstraints = false
        callGraphButton.setTitle("Call Microsoft Graph API", for: .normal)
        callGraphButton.setTitleColor(.blue, for: .normal)
        callGraphButton.addTarget(self, action: #selector(callGraphAPI(_:)), for: .touchUpInside)
        self.view.addSubview(callGraphButton)
        
        callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 50.0).isActive = true
        callGraphButton.widthAnchor.constraint(equalToConstant: 300.0).isActive = true
        callGraphButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
        // Add sign out button
        signOutButton = UIButton()
        signOutButton.translatesAutoresizingMaskIntoConstraints = false
        signOutButton.setTitle("Sign Out", for: .normal)
        signOutButton.setTitleColor(.blue, for: .normal)
        signOutButton.setTitleColor(.gray, for: .disabled)
        signOutButton.addTarget(self, action: #selector(signOut(_:)), for: .touchUpInside)
        self.view.addSubview(signOutButton)
        
        signOutButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        signOutButton.topAnchor.constraint(equalTo: callGraphButton.bottomAnchor, constant: 10.0).isActive = true
        signOutButton.widthAnchor.constraint(equalToConstant: 150.0).isActive = true
        signOutButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        signOutButton.isEnabled = false
        
        // Add logging textfield
        loggingText = UITextView()
        loggingText.isUserInteractionEnabled = false
        loggingText.translatesAutoresizingMaskIntoConstraints = false
        
        self.view.addSubview(loggingText)
        
        loggingText.topAnchor.constraint(equalTo: signOutButton.bottomAnchor, constant: 10.0).isActive = true
        loggingText.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 10.0).isActive = true
        loggingText.rightAnchor.constraint(equalTo: self.view.rightAnchor, constant: 10.0).isActive = true
        loggingText.bottomAnchor.constraint(equalTo: self.view.bottomAnchor, constant: 10.0).isActive = true
    }
```

Em seguida, adicione `viewDidLoad()` do ViewController.swift:

```swift
    override func viewDidLoad() {
        super.viewDidLoad()
        initUI()
        do {
            try self.initMSAL()
        } catch let error {
            self.loggingText.text = "Unable to create Application Context \(error)"
        }
    }
```

## <a name="use-msal"></a>Usar a MSAL

### <a name="initialize-msal"></a>Inicializar a MSAL

Primeiro, você precisa criar um `MSALPublicClientApplication` com uma instância de `MSALPublicClientConfiguration` para seu aplicativo:

```swift
    func initMSAL() throws {
        
        guard let authorityURL = URL(string: kAuthority) else {
            self.loggingText.text = "Unable to create authority URL"
            return
        }
        
        let authority = try MSALAADAuthority(url: authorityURL)
        
        let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
        self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
    }
```

### <a name="handle-the-callback"></a>Lidar com o retorno de chamada

Para lidar com o retorno de chamada após conectar, adicione `MSALPublicClientApplication.handleMSALResponse` em `appDelegate`:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
    }
```

#### <a name="acquire-tokens"></a>Adquirir tokens

Agora, podemos implementar a lógica de processamento de IU do aplicativo e obter tokens interativamente por meio da MSAL. 

A MSAL expõe dois métodos principais para obter tokens: `acquireTokenSilently` e `acquireTokenInteractively`.  

`acquireTokenSilently()` tenta conectar um usuário e obtém tokens sem qualquer interação do usuário se uma conta estiver presente.

`acquireTokenInteractively` sempre mostrará a interface do usuário ao tentar entrar no usuário e obter tokens; no entanto, ela pode usar cookies de sessão no navegador ou uma conta no autenticador da Microsoft para fornecer uma experiência de SSO interativa. 

```swift
    @objc func callGraphAPI(_ sender: UIButton) {
        
        guard let currentAccount = self.currentAccount() else {
            // We check to see if we have a current logged in account.
            // If we don't, then we need to sign someone in.
            acquireTokenInteractively()
            return
        }
        
        acquireTokenSilently(currentAccount)
    }

    func currentAccount() -> MSALAccount? {
        
        guard let applicationContext = self.applicationContext else { return nil }
        
        // We retrieve our current account by getting the first account from cache
        // In multi-account applications, account should be retrieved by home account identifier or username instead
        
        do {
            let cachedAccounts = try applicationContext.allAccounts()
            if !cachedAccounts.isEmpty {
                return cachedAccounts.first
            }
        } catch let error as NSError {
            self.updateLogging(text: "Didn't find any accounts in cache: \(error)")
        }
        
        return nil
    }
```

#### <a name="get-a-token-interactively"></a>Obter um token interativamente

Para adquirir um token pela primeira vez, você precisa criar um `MSALInteractiveTokenParameters` e chamar `acquireToken`.

1. Crie `MSALInteractiveTokenParameters` com escopos.
2. Chame `acquireToken` com os parâmetros criados.
3. Lide com o erro adequadamente. Para obter mais detalhes, confira o [guia de tratamento de erros do iOS](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki/Error-Handling).
4. Lidar com o caso de sucesso. 

```swift
    func acquireTokenInteractively() {
   
        guard let applicationContext = self.applicationContext else { return }
     // #1    
        let parameters = MSALInteractiveTokenParameters(scopes: kScopes)
     // #2        
        applicationContext.acquireToken(with: parameters) { (result, error) in
     // #3            
            if let error = error {
                self.updateLogging(text: "Could not acquire token: \(error)")
                return
            }
            guard let result = result else {   
                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }
     // #4            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```



#### <a name="get-a-token-silently"></a>Obter um token no modo silencioso

Para adquirir um token atualizado silenciosamente, você precisará criar um `MSALSilentTokenParameters` e chamar `acquireTokenSilent`:

```swift
    
    func acquireTokenSilently(_ account : MSALAccount!) {
        guard let applicationContext = self.applicationContext else { return }
        let parameters = MSALSilentTokenParameters(scopes: kScopes, account: account)
        
        applicationContext.acquireTokenSilent(with: parameters) { (result, error) in    
            if let error = error {
                let nsError = error as NSError
                if (nsError.domain == MSALErrorDomain) {
                    if (nsError.code == MSALError.interactionRequired.rawValue) {
                        DispatchQueue.main.async {
                            self.acquireTokenInteractively()
                        }
                        return
                    }
                }
                self.updateLogging(text: "Could not acquire token silently: \(error)")
                return
            }
            
            guard let result = result else {
                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }
            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Refreshed Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```

### <a name="call-the-microsoft-graph-api"></a>Chamar a API do Microsoft Graph 

Após obter um token pelo `self.accessToken`, seu aplicativo pode usar esse valor no cabeçalho HTTP para fazer uma solicitação autorizada ao Microsoft Graph:

| chave de cabeçalho    | value                 |
| ------------- | --------------------- |
| Autorização | Bearer <token-de-acesso> |

Adicione o seguinte a `ViewController.swift`:

```swift
    func getContentWithToken() {        
        // Specify the Graph API endpoint
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)
        
        // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")
               
        URLSession.shared.dataTask(with: request) { data, response, error in
               
        if let error = error {
            self.updateLogging(text: "Couldn't get graph result: \(error)")
            return
        }
               
        guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {
               
        self.updateLogging(text: "Couldn't deserialize result JSON")
            return
        }
               
        self.updateLogging(text: "Result from Graph: \(result))")
        
        }.resume()
    }
```

Saiba mais sobre a [API do Microsoft Graph](https://graph.microsoft.com)

### <a name="use-msal-for-sign-out"></a>Usar a MSAL para saída

Em seguida, adicionaremos suporte sobre a saída do nosso aplicativo. 

É importante observar que a saída com a MSAL remove todas as informações conhecidas sobre um usuário deste aplicativo, mas o usuário ainda terá uma sessão ativa em seu dispositivo. Se o usuário tentar conectar novamente, poderá observar interação, mas talvez não precise inserir novamente suas credenciais, pois a sessão do dispositivo ainda estará ativa. 

Para adicionar a saída, copie o seguinte método em `ViewController.swift`:

```swift 
    @objc func signOut(_ sender: UIButton) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        guard let account = self.currentAccount() else { return }
        
        do {
            
            /**
             Removes all tokens from the cache for this application for the provided account
             
             - account:    The account to remove from the cache
             */
            
            try applicationContext.remove(account)
            self.loggingText.text = ""
            self.signOutButton.isEnabled = false
            
        } catch let error as NSError {
            
            self.updateLogging(text: "Received error signing account out: \(error)")
        }
    }
```

### <a name="enable-token-caching"></a>Ativar cache de token

Por padrão, a MSAL armazena em cache os tokens do seu aplicativo nas chaves do iOS. 

Para habilitar o cache de token, acesse Configurações de Projeto do Xcode > `Capabilities tab` > `Enable Keychain Sharing` > clique em `Plus` > Enter **com.microsoft.adalcache**.

### <a name="add-helper-methods"></a>Adicionar métodos auxiliares

Adicione esses métodos auxiliares para concluir o exemplo:

``` swift
    
    func updateLogging(text : String) {
        
        if Thread.isMainThread {
            self.loggingText.text = text
        } else {
            DispatchQueue.main.async {
                self.loggingText.text = text
            }
        }
    }
    
    func updateSignOutButton(enabled : Bool) {
        if Thread.isMainThread {
            self.signOutButton.isEnabled = enabled
        } else {
            DispatchQueue.main.async {
                self.signOutButton.isEnabled = enabled
            }
        }
    }
```

### <a name="multi-account-applications"></a>Aplicativos de várias contas

Este aplicativo foi criado para um cenário de conta única. A MSAL também dá suporte a cenários de várias contas, mas requer algum trabalho adicional dos aplicativos. Você precisará criar a interface do usuário para ajudar a selecionar qual conta deseja usar para cada ação que requer tokens do usuário. Como alternativa, seu aplicativo pode implementar uma heurística para selecionar qual conta usar por meio do método `getAllAccounts(...)`.

## <a name="test-your-app"></a>Testar seu aplicativo

### <a name="run-locally"></a>Executar localmente

Se você seguiu o código acima, tente criar e implantar o aplicativo em um dispositivo de teste ou emulador. Você deve conseguir conectar e obter tokens para o Azure AD ou contas Microsoft pessoais! Após a conexão do usuário, este aplicativo exibirá os dados retornados do ponto de extremidade `/me` do Microsoft Graph. 

No caso de qualquer problema, fique à vontade para inserir um problema neste documento ou na biblioteca do MSAL e nos avise. 

### <a name="consent-to-your-app"></a>Consentimento para seu aplicativo

Na primeira vez que um usuário entrar no seu aplicativo, ele será solicitado pela identidade da Microsoft a consentir com as permissões solicitadas.  Embora a maioria dos usuários seja capaz de dar seu consentimento, alguns locatários do Azure AD desabilitaram o consentimento do usuário, exigindo que os administradores consintam em nome de todos os usuários.  Para dar suporte a esse cenário, certifique-se de registrar os escopos do seu aplicativo no portal do Azure.

## <a name="help-and-support"></a>Ajuda e Suporte

Teve algum problema com este tutorial ou com a plataforma de identidade da Microsoft? Confira [Ajuda e suporte](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options)

