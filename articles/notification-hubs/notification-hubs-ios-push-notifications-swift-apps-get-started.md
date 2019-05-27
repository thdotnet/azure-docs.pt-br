---
title: Notificações por push para aplicativos iOS Swift usando os Hubs de notificação do Azure | Microsoft Docs
description: Saiba como notificações por push para aplicativos iOS Swift usando os Hubs de notificação do Azure.
services: notification-hubs
documentationcenter: ios
author: mikeparker104
manager: patniko
editor: spelluru
ms.assetid: 4e3772cf-20db-4b9f-bb74-886adfaaa65d
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: ios
ms.devlang: objective-c
ms.topic: article
ms.date: 05/21/2019
ms.author: miparker
ms.openlocfilehash: 64a05c6cbd412953ae10b31efc1d141c9cefd062
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65994760"
---
# <a name="tutorial-push-notifications-to-swift-ios-apps-using-the-notification-hubs-rest-api"></a>Tutorial: Notificações por push para aplicativos iOS Swift usando a API de REST de Hubs de notificação

> [!div class="op_single_selector"]
> * [Objective-C](notification-hubs-ios-apple-push-notification-apns-get-started.md)
> * [Swift](notification-hubs-ios-push-notifications-swift-apps-get-started.md)

Neste tutorial, você deve usar os Hubs de notificação do Azure para enviar notificações por push para um aplicativo iOS baseado em Swift usando o [API REST](/rest/api/notificationhubs/). Você cria um aplicativo do iOS em branco que recebe notificações por push usando o [APNs (Apple Push Notification Service)](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1).

Neste tutorial, você deve executar as seguintes etapas:

> [!div class="checklist"]
> * Gerar o arquivo de solicitação de assinatura de certificado
> * Solicitar seu aplicativo para notificações por push
> * Criar um perfil de provisionamento para o aplicativo
> * Criar um hub de notificação
> * Configurar o hub de notificação com informações de APNS
> * Conectar seu aplicativo do iOS aos hubs de notificação
> * Testar a solução

## <a name="prerequisites"></a>Pré-requisitos
Para acompanhar, você precisa:

- Percorra [visão geral dos Hubs de notificação do Azure](notification-hubs-push-notification-overview.md) se você não estiver familiarizado com o serviço. 
- Saiba mais sobre registros e a instalação: [Gerenciamento de registro](notification-hubs-push-notification-registration-management.md)
- Um ativo [conta de desenvolvedor da Apple](https://developer.apple.com) 
- Um Mac com Xcode junto com um certificado de desenvolvedor válida instalado em seu conjunto de chaves
- Um dispositivo físico iPhone que você pode executar e depurar com (não é possível testar notificações por push com o simulador)
- Seu dispositivo físico iPhone registrado na [Portal da Apple](https://developer.apple.com) e associado ao certificado
- Uma [assinatura do Azure](https://portal.azure.com) onde você pode criar e gerenciar recursos

Ele deve ser possível acompanhá-lo para criar este exemplo primeiro princípios sem nenhuma experiência anterior. No entanto, será útil estar familiarizado com os seguintes conceitos:

- Criar aplicativos iOS com Xcode e Swift
- Configurando uma [Hub de notificação do Azure](notification-hubs-ios-apple-push-notification-apns-get-started.md) para iOS
- Familiaridade com o [Portal do desenvolvedor da Apple](https://developer.apple.com) e o [portal do Azure](https://portal.azure.com)

> [!NOTE]
> O hub de notificação será configurado para usar o *área restrita* somente o modo de autenticação. Você não deve usar esse modo de autenticação para cargas de trabalho de produção.

[!INCLUDE [Notification Hubs Enable Apple Push Notifications](../../includes/notification-hubs-enable-apple-push-notifications.md)]

## <a name="connect-your-ios-app-to-notification-hubs"></a>Conectar seu aplicativo do iOS aos Hubs de Notificação
Nesta seção, você compilará o aplicativo do iOS que se conecta ao hub de notificação.  

### <a name="create-an-ios-project"></a>Criar um projeto do iOS
1. Na **Xcode**, crie um novo projeto do iOS e selecione o **aplicativo de exibição única** modelo.
2. Ao definir as opções para o novo projeto, siga estas etapas:
    1. Use o mesmo **nome do produto** (ou seja, **PushDemo**) e **identificador de organização** (ou seja, `com.<organization>`) usado quando o **pacote Identificador** foi definido em de **Portal do desenvolvedor Apple**. 
    2. Escolha o **Team** que o **ID do aplicativo** foi configurada para.
    3. Defina as **linguagem** à **Swift**.
    4. Clique em **Avançar**
3. Criar uma nova pasta denominada **SupportingFiles**.
4. Criar um novo **plist** arquivo chamado **devsettings.plist** sob o **SupportingFiles** pasta. Certifique-se de adicionar essa pasta ao seu **gitignore** arquivo para que não é confirmada quando estiver trabalhando com um **repositório git**. Em um aplicativo de produção, você seria provavelmente condicionalmente definindo esses segredos como parte de um processo de compilação automatizado. No entanto, não é abordada como parte deste passo a passo.
5. Atualização **devsettings.plist** para incluir as seguintes entradas de configuração (usando seus próprios valores da **Hub de notificação** você provisionado):

   | Chave                            | Type                     | Value                     |               
   |--------------------------------| -------------------------| --------------------------|
   | notificationHubKey             | String                   | \<hubKey\>                |
   | notificationHubKeyName         | String                   | \<hubKeyName\>            |
   | notificationHubName            | String                   | \<hubName\>               |
   | notificationHubNamespace       | String                   | \<hubNamespace\>          |
    
   Você pode encontrar os valores necessários, navegando até a **Hub de notificação** recurso nas **portal do Azure**. Você pode encontrar o **notificationHubName** e o **notificationHubNamespace** valores no canto superior direito da **Essentials** resumo dentro de  **Visão geral** página.

   ![Resumo de princípios de Hubs de notificação](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials.png)

   Você pode encontrar o **notificationHubKeyName** e **notificationHubKey** valores, navegando até **políticas de acesso**, clicando no respectivo **acesso Diretiva**. Por exemplo: `DefaultFullSharedAccessSignature`. Em seguida, copie do **cadeia de Conexão primária** o valor é prefixado com `SharedAccessKeyName=` para `notificationHubKeyName` e o valor é prefixado com `SharedAccessKey=` para o `notificationHubKey`. A cadeia de caracteres de conexão deve estar no seguinte formato:
    
   ```xml
   Endpoint=sb://<namespace>.servicebus.windows.net/;SharedAccessKeyName=<notificationHubKeyName>;SharedAccessKey=<notificationHubKey>
   ```

   Para manter a simplicidade, você usará *DefaultFullSharedAccessSignature* para que você pode usar o token para enviar notificações e também. No entanto, na prática a `DefaultListenSharedAccessSignature` seria uma opção melhor para situações em que você apenas deseja receber notificações.       
6. Sob **navegador de projeto**, clique no **nome do projeto**, em seguida, clique o **geral** guia
7. Encontrar **identidade**, em seguida, defina o **identificador de pacote** valor para que coincida com aquele usado para o **ID do aplicativo** (da etapa anterior), ou seja, `'com.<organization>.PushDemo'`
8. Encontrar **Signing**, verifique se você selecionar as devidas **equipe** para sua **conta de desenvolvedor da Apple** (aquele em que você tivesse criado seus certificados e perfis anteriormente).  **Xcode** deverá automaticamente efetuar pull apropriado **perfil de provisionamento** com base no **identificador de pacote**. Se você não vir o novo **perfil de provisionamento**, tente atualizar os perfis para o **identidade de assinatura** (*Xcode > Preferências > conta > Exibir detalhes*). Clicando na **identidade de assinatura**, clicando no **atualizar** botão no canto inferior direito deve baixar os perfis.
9. Selecione o **capacidades** guia e certifique-se de que **notificações por push** estão habilitados.
10. Abra seu **Appdelegate** arquivo para implementar o *UNUserNotificationCenterDelegate* de protocolo e adicione o seguinte código à parte superior da classe:
    
    ```swift
    @UIApplicationMain
    class AppDelegate: UIResponder, UIApplicationDelegate, UNUserNotificationCenterDelegate {
        
        ...

        var configValues: NSDictionary?
        var notificationHubNamespace : String?
        var notificationHubName : String?
        var notificationHubKeyName : String?
        var notificationHubKey : String?
        let tags = ["12345"]
        let genericTemplate = PushTemplate(withBody: "{\"aps\":{\"alert\":\"$(message)\"}}")
        
        ...
    }
    ```

    Você usará esses membros mais tarde. O *marcas* e *genericTemplate* será usado como parte do registro. Para obter mais informações sobre marcas, consulte [marcas de registros](notification-hubs-tags-segment-push-message.md) e [registros de modelo](notification-hubs-templates-cross-platform-push-messages.md).
 
11. No mesmo arquivo, adicione o seguinte código na *didFinishLaunchingWithOptions* função:

    ```swift
    if let path = Bundle.main.path(forResource: "devsettings", ofType: "plist") {
        if let configValues = NSDictionary(contentsOfFile: path) {
            self.notificationHubNamespace = configValues["notificationHubNamespace"] as? String
            self.notificationHubName = configValues["notificationHubName"] as? String
            self.notificationHubKeyName = configValues["notificationHubKeyName"] as? String
            self.notificationHubKey = configValues["notificationHubKey"] as? String
        }
    }
        
    if #available(iOS 10.0, *){
        UNUserNotificationCenter.current().delegate = self
        UNUserNotificationCenter.current().requestAuthorization(options: [.alert, .sound, .badge]) {
            (granted, error) in
                
            if (granted)
            {
                DispatchQueue.main.async {
                    application.registerForRemoteNotifications()
                }
            }
        }
    }

    return true
    ```

    Esse código recupera os valores de configuração de **devsettings.plist**, define o **AppDelegate** da classe como o *UNUserNotificationCenter* delegar autorização de solicitações para notificações por push e, em seguida, chama *registerForRemoteNotifications*.
    
    Para simplificar, o código suporta **iOS 10 e superiores apenas**. Você pode adicionar o suporte para versões mais antigas do sistema operacional usando condicionalmente os respectivos APIs e as abordagens como você faria normalmente.

12. No mesmo arquivo, adicione as seguintes funções:

    ```swift
    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let installationId = (UIDevice.current.identifierForVendor?.description)!
        let pushChannel = deviceToken.reduce("", {$0 + String(format: "%02X", $1)})
    }

    func showAlert(withText text : String) {
        let alertController = UIAlertController(title: "PushDemo", message: text, preferredStyle: UIAlertControllerStyle.alert)
        alertController.addAction(UIAlertAction(title: "OK", style: UIAlertActionStyle.default,handler: nil))
        self.window?.rootViewController?.present(alertController, animated: true, completion: nil)
    }
    ```

    O código usa o *installationId* e *pushChannel* valores para registrar com o **Hub de notificação**. Nesse caso, você está usando *UIDevice.current.identifierForVendor* para nos fornecer um valor exclusivo para identificar o dispositivo e, em seguida, formatar os *deviceToken* para nos fornecer o desejado *pushChannel* valor. O *showAlert* função é simplesmente exibir um texto de mensagem para fins de demonstração.

13. Ainda na **Appdelegate**, adicione o *willPresent* e *didReceive* **UNUserNotificationCenterDelegate** funções para Exibir um alerta quando a notificação foi recebido quando o aplicativo está em execução em primeiro e segundo plano, respectivamente
    
    ```swift
    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, 
        willPresent notification: UNNotification, 
        withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
        showAlert(withText: notification.request.content.body)
    }
    
    @available(iOS 10.0, *)
    func userNotificationCenter(_ center: UNUserNotificationCenter, 
        didReceive response: UNNotificationResponse, 
        withCompletionHandler completionHandler: @escaping () -> Void) {
        showAlert(withText: response.notification.request.content.body)
    }
    ```    

14. Adicionar instruções de impressão na parte inferior do *didRegisterForRemoteNotificationsWithDeviceToken* função para verificar se *installationId* e *pushChannel* estão sendo valores atribuídos
15. Criar pastas (**modelos**, **Services**, e **utilitários**) para os componentes fundamentais que você o adicionará ao projeto mais tarde
16. Verifique que o projeto compilado e executado em um dispositivo físico (push notificações não podem ser testadas usando o simulador)

### <a name="create-models"></a>Criar modelos
Nesta etapa, você criará um conjunto de modelos para representar o [API de REST de Hubs de notificação](/rest/api/notificationhubs/) cargas e para armazenar o necessária **token SAS** dados.


1.  Adicione um novo arquivo swift para o **modelos** chamado **PushTemplate.swift**. Esse modelo fornece uma estrutura que representa o **corpo** de um modelo individual como parte do **DeviceInstallation** carga:
    
    ```swift
    import Foundation

    struct PushTemplate : Codable {
        let body : String
    
        init(withBody body : String) {
            self.body = body
        }
    }
    ```

2. Adicione um novo arquivo swift para o **modelos** pasta denominada **DeviceInstallation.swift**. Esse arquivo define uma estrutura que representa o conteúdo para criar ou atualizar uma **instalação de dispositivo**. Adicione o seguinte código ao arquivo:
    
    ```swift
    import Foundation

    struct DeviceInstallation : Codable {
        let installationId : String
        let pushChannel : String
        let platform : String = "apns"
        var tags : [String]
        var templates : Dictionary<String, PushTemplate>
    
        init(withInstallationId installationId : String, andPushChannel pushChannel : String) {
            self.installationId = installationId
            self.pushChannel = pushChannel
            self.tags = [String]()
            self.templates = Dictionary<String, PushTemplate>()
        }
    }
    ```

3.  Adicione um novo arquivo swift sob **modelos** chamado **TokenData.swift**. Esse modelo será usado para armazenar um **token SAS** juntamente com sua expiração

    ```swift
    import Foundation

    struct TokenData {
    
        let token : String
        let expiration : Int

        init(withToken token : String, andTokenExpiration expiration : Int) {
            self.token = token
            self.expiration = expiration
        }
    }
    ```

### <a name="generate-a-sas-token"></a>Gerar um token SAS
**Os Hubs de notificação** usam a mesma infraestrutura de segurança como **barramento de serviço do Azure**. Para chamar a API REST, você precisará [programaticamente, gerar um token SAS](/rest/api/eventhub/generate-sas-token) que pode ser usado na **autorização** cabeçalho da solicitação.  

O token resultante será no seguinte formato: 

```xml
SharedAccessSignature sig=<UrlEncodedSignature>&se=<ExpiryEpoch>&skn=<KeyName>&sr=<UrlEncodedResourceUri>
```

O próprio processo envolve as mesmas seis etapas principais:  

1.  Computação de expiração na [época do UNIX](https://en.wikipedia.org/wiki/Unix_time) formato (segundos transcorridos desde 00:00:00 UTC 1 de janeiro de 1970)
2.  Formatação de **ResourceUrl** (que representa o recurso que você está tentando acessar, ou seja, `'https://\<namespace\>.servicebus.windows.net/\<hubName\>'`) portanto, é codificado por percentual e em minúsculas
3.  Preparando-se a **StringToSign**, que é composto de `'\<**UrlEncodedResourceUrl**\>\n\<**ExpiryEpoch**\>'`
4.  Computação (e codificação base 64) a **assinatura** usando o **HMAC-SHA256** da **StringToSign** valor com o **chave** faz parte do **Cadeia de caracteres de Conexão** (para os respectivos **regra de autorização**)
5.  Formatação de codificados na base 64 **assinatura** para que ela seja codificada por cento
6.  Construindo a **token** no formato esperado usando o **UrlEncodedSignature**, **ExpiryEpoch**, **KeyName**e **UrlEncodedResourceUrl** valores

Consulte a [documentação do barramento de serviço do Azure](../service-bus-messaging/service-bus-sas.md) para obter uma visão mais completa da **assinatura de acesso compartilhado** e como ele é usado pelo **do barramento de serviço do Azure** e  **Os Hubs de notificação**.

Para os fins deste exemplo Swift, você vai usar o software livre da Apple **CommonCrypto** biblioteca para ajudar com o hash da assinatura. Como é uma biblioteca C, ele não está acessível no Swift out-of-the-box. No entanto, você pode disponibilizar isso usando um cabeçalho ponte. Para adicionar e configurar o cabeçalho ponte:

1. Na **Xcode**, vá para **arquivo**, em seguida, **New**, em seguida, **arquivo** e selecione **arquivo de cabeçalho** nomeando- *'BridgingHeader.h'*
2. Edite o arquivo para importar **CommonHMAC**

    ```swift
    #import <CommonCrypto/CommonHMAC.h>

    #ifndef BridgingHeader_h
    #define BridgingHeader_h


    #endif /* BridgingHeader_h */
    ```

3. Atualizar o destino **configurações de Build** para referenciar o cabeçalho ponte. Abra o **configurações de construção** guia e role para baixo até a **compilador Swift** seção. Certifique-se de que o **instalar o cabeçalho de compatibilidade de Objective-C** opção definida como **Sim** e digite o caminho do arquivo para nosso cabeçalho ponte para o **ponte cabeçalho Objective-C**   que é a opção `'\<ProjectName\>/BridgingHeader.h'`. Se você não encontrar essas opções, verifique se você tem o **todos os** modo de exibição selecionado (em vez de **básica** ou **personalizado**).
    
   Há muitas bibliotecas de terceiros wrapper de código-fonte aberto disponíveis, que podem tornar usando **CommonCrypto** um pouco mais fácil. Ele está além do escopo deste artigo.

4. Adicione um novo arquivo Swift sob o **utilitários** pasta chamada **TokenUtility.swift** e adicione o seguinte código:

   ```swift
   import Foundation

   struct TokenUtility {    
        typealias Context = UnsafeMutablePointer<CCHmacContext>
    
        static func getSasToken(forResourceUrl resourceUrl : String, withKeyName keyName : String, andKey key : String, andExpiryInSeconds expiryInSeconds : Int = 3600) -> TokenData {
            let expiry = (Int(NSDate().timeIntervalSince1970) + expiryInSeconds).description
            let encodedUrl = urlEncodedString(withString: resourceUrl)
            let stringToSign = "\(encodedUrl)\n\(expiry)"
            let hashValue = sha256HMac(withData: stringToSign.data(using: .utf8)!, andKey: key.data(using: .utf8)!)
            let signature = hashValue.base64EncodedString(options: .init(rawValue: 0))
            let encodedSignature = urlEncodedString(withString: signature)
            let sasToken = "SharedAccessSignature sr=\(encodedUrl)&sig=\(encodedSignature)&se=\(expiry)&skn=\(keyName)"
            let tokenData = TokenData(withToken: sasToken, andTokenExpiration: expiryInSeconds)
        
            return tokenData
        }
    
        private static func sha256HMac(withData data : Data, andKey key : Data) -> Data {
            let context = Context.allocate(capacity: 1)
            CCHmacInit(context, CCHmacAlgorithm(kCCHmacAlgSHA256), (key as NSData).bytes, size_t((key as NSData).length))
            CCHmacUpdate(context, (data as NSData).bytes, (data as NSData).length)
            var hmac = Array<UInt8>(repeating: 0, count: Int(CC_SHA256_DIGEST_LENGTH))
            CCHmacFinal(context, &hmac)
        
            let result = NSData(bytes: hmac, length: hmac.count)
            context.deallocate()
        
            return result as Data
        }
    
        private static func urlEncodedString(withString stringToConvert : String) -> String {
            var encodedString = ""
            let sourceUtf8 = (stringToConvert as NSString).utf8String
            let length = strlen(sourceUtf8)
        
            let charArray: [Character] = [ ".", "-", "_", "~", "a", "z", "A", "Z", "0", "9"]
            let asUInt8Array = String(charArray).utf8.map{ Int8($0) }
        
            for i in 0..<length {
                let currentChar = sourceUtf8![i]
            
                if (currentChar == asUInt8Array[0] || currentChar == asUInt8Array[1] || currentChar == asUInt8Array[2] || currentChar == asUInt8Array[3] ||
                    (currentChar >= asUInt8Array[4] && currentChar <= asUInt8Array[5]) ||
                    (currentChar >= asUInt8Array[6] && currentChar <= asUInt8Array[7]) ||
                    (currentChar >= asUInt8Array[8] && currentChar <= asUInt8Array[9])) {
                    encodedString += String(format:"%c", currentChar)
                }
                else {
                    encodedString += String(format:"%%%02x", currentChar)
                }
            }
        
            return encodedString
        }
    }
   ```
    
    Esse utilitário encapsula a lógica responsável por gerar o **token SAS**. O *getSasToken* função orquestra as etapas de alto nível necessárias para preparar o token, conforme descrito anteriormente e será chamada pelo serviço de instalação nas etapas posteriormente no tutorial. As duas outras funções são chamadas pelo *getSasToken função*; *sha256HMac* para a assinatura de computação e *urlEncodedString* para codificar a respectiva cadeia de caracteres de Url. O *urlEncodedString* função era necessária, pois não foi possível obter a saída necessária usando internos *addingPercentEncoding* função. O [SDK do iOS de armazenamento do Azure](https://github.com/Azure/azure-storage-ios/blob/master/Lib/Azure%20Storage%20Client%20Library/Azure%20Storage%20Client%20Library/AZSUtil.m) atuou como um excelente exemplo de como abordar essas operações, embora em Objective-C. Obter mais informações sobre **tokens de SAS do barramento de serviço do Azure** podem ser encontradas na [documentação do barramento de serviço do Azure](../service-bus-messaging/service-bus-sas.md). 

### <a name="verify-the-sas-token"></a>Verifique se o token SAS
Antes de implementar o serviço de instalação no cliente, você pode verificar que nosso aplicativo está gerando corretamente a **token SAS** usando o utilitário de http de escolha. Para os fins desta postagem, nossa ferramenta de escolha estará **Postman**.

Anote o *installationId* e o *token* valores que está sendo gerados pelo aplicativo usando um posicionados adequadamente imprimir instrução ou um ponto de interrupção. 

Siga estas etapas para chamar o *instalações* API:

1. Na **Postman**, abra uma nova guia
2. Definir a solicitação **obter** e o endereço a seguir:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/installations/<installationId>?api-version=2015-01
    ```

3. Configure os cabeçalhos de solicitação da seguinte maneira:
    
   | Chave           | Value            |
   | ------------- | ---------------- |
   | Tipo de Conteúdo  | aplicativo/json |
   | Autorização | \<sasToken\>     |
   | x-ms-version  | 2015-01          |

4. Clique no **código** botão (canto superior direito sob a **salvar** botão). A solicitação deve ser semelhante ao exemplo a seguir:

    ```html
    GET /<hubName>/installations/<installationId>?api-version=2015-01 HTTP/1.1
    Host: <namespace>.servicebus.windows.net
    Content-Type: application/json
    Authorization: <sasToken>
    x-ms-version: 2015-01
    Cache-Control: no-cache
    Postman-Token: <postmanToken>
    ```

5. Clique o **enviar** botão

Nenhum registro existe para especificado *installationId* neste ponto, no entanto ele deve resultar em um **404 não encontrado** resposta em vez de **401 não autorizado**. Esse resultado deve confirmar que o **token SAS** foi aceita.

### <a name="implement-the-installation-service-class"></a>Implementar a classe de serviço de instalação
Em seguida, você implementará básica wrapper em torno de [API REST de instalações](/rest/api/notificationhubs/create-overwrite-installation).  

Adicione um novo arquivo Swift sob o **serviços** pasta chamada **NotificationRegistrationService.swift**, em seguida, adicione o seguinte código para esse arquivo:

```swift
import Foundation

class NotificationRegistrationService {
    private let tokenizedBaseAddress: String = "https://%@.servicebus.windows.net/%@"
    private let tokenizedCreateOrUpdateInstallationRequest = "/installations/%@?api-version=%@"
    private let session = URLSession(configuration: URLSessionConfiguration.default)
    private let apiVersion = "2015-01"
    private let jsonEncoder = JSONEncoder()
    private let defaultHeaders: [String : String]
    private let installationId : String
    private let pushChannel : String
    private let hubNamespace : String
    private let hubName : String
    private let keyName : String
    private let key : String
    private var tokenData : TokenData? = nil
    
    init(withInstallationId installationId : String,
            andPushChannel pushChannel : String,
            andHubNamespace hubNamespace : String,
            andHubName hubName : String,
            andKeyName keyName : String,
            andKey key: String) {
        self.installationId = installationId
        self.pushChannel = pushChannel
        self.hubNamespace = hubNamespace
        self.hubName = hubName
        self.keyName = keyName
        self.key = key
        self.defaultHeaders = ["Content-Type": "application/json", "x-ms-version": apiVersion]
    }
    
    func register(
        withTags tags : [String]? = nil,
        andTemplates templates : Dictionary<String, PushTemplate>? = nil,
        completeWith completion: ((_ result: Bool) -> ())? = nil) {
        
        var deviceInstallation = DeviceInstallation(withInstallationId: installationId, andPushChannel: pushChannel)
        
        if let tags = tags {
            deviceInstallation.tags = tags
        }
        
        if let templates = templates {
            deviceInstallation.templates = templates
        }
        
        if let deviceInstallationJson = encodeToJson(deviceInstallation) {
            let sasToken = getSasToken()
            let requestUrl = String.init(format: tokenizedCreateOrUpdateInstallationRequest, installationId, apiVersion)
            let apiEndpoint = "\(getBaseAddress())\(requestUrl)"
            
            var request = URLRequest(url: URL(string: apiEndpoint)!)
            request.httpMethod = "PUT"
            
            for (key,value) in self.defaultHeaders {
                request.addValue(value, forHTTPHeaderField: key)
            }
            
            request.addValue(sasToken, forHTTPHeaderField: "Authorization")
            request.httpBody = Data(deviceInstallationJson.utf8)
            
            (self.session.dataTask(with: request) { dat, res, err in
                if let completion = completion {
                        completion(err == nil && (res as! HTTPURLResponse).statusCode == 200)
                }
            }).resume()
        }
    }
    
    private func getBaseAddress() -> String {
        return String.init(format: tokenizedBaseAddress, hubNamespace, hubName)
    }
    
    private func getSasToken() -> String {
        if (tokenData == nil ||
            Date(timeIntervalSince1970: Double((tokenData?.expiration)!)) < Date(timeIntervalSinceNow: -(5 * 60))) {
            self.tokenData = TokenUtility.getSasToken(forResourceUrl: getBaseAddress(), withKeyName: self.keyName, andKey: self.key)
        }

        return (tokenData?.token)!
    }
    
    private func encodeToJson<T : Encodable>(_ object: T) -> String? {
        do {
            let jsonData = try jsonEncoder.encode(object)
            if let jsonString = String(data: jsonData, encoding: .utf8) {
                return jsonString
            } else {
                return nil
            }
        }
        catch {
            return nil
        }
    }
}
```
 
Os requisito detalhes são fornecidos como parte da inicialização. Marcas e modelos, opcionalmente, são passados para o *registre* função como parte da **instalação do dispositivo** carga JSON.  

O *registrar* função chama funções privadas para preparar a solicitação. Depois que uma resposta for recebida, a conclusão é chamada que indica se o registro foi bem-sucedido ou não.  

O ponto de extremidade de solicitação é construído pela *getBaseAddress* funcionam usando a **Hub de notificação** parâmetros **namespace** e **nome**fornecidas durante a inicialização.  

O *getSasToken* função verificará se o token armazenado no momento é válido, caso contrário, ele chamará-out para o **TokenUtility** para gerar um novo e armazená-lo antes de retornar um valor. 

Por fim o *encodeToJson* irá converter os objetos do respectivo modelo em JSON para uso como parte do corpo da solicitação.

### <a name="invoke-the-notification-hubs-rest-api"></a>Invocar os API REST de Hubs de notificação
A última etapa é atualizar **AppDelegate** usar o **NotifiationRegistrationService** registrar com nosso **NotificationHub**. 

1. Abra **Appdelegate** e adicione uma variável de nível de classe para armazenar uma referência para o **NoficiationRegistrationService**:

    ```swift
    var registrationService : NotificationRegistrationService?
    ```

2. No mesmo arquivo, atualize o *didRegisterForRemoteNotificationsWithDeviceToken* função para inicializar o **NotificationRegistrationService** com os parâmetros necessários, em seguida, chame o *registrar* função.

    ```swift
    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
        let installationId = (UIDevice.current.identifierForVendor?.description)!
        let pushChannel = deviceToken.reduce("", {$0 + String(format: "%02X", $1)})

        // Initialize the Notification Registration Service
        self.registrationService = NotificationRegistrationService(
            withInstallationId: installationId,
            andPushChannel: pushChannel,
            andHubNamespace: notificationHubNamespace!,
            andHubName: notificationHubName!,
            andKeyName: notificationHubKeyName!,
            andKey: notificationHubKey!)
    
        // Call register passing in the tags and template parameters
        self.registrationService!.register(withTags: tags, andTemplates: ["genericTemplate" : self.genericTemplate]) { (result) in
            if !result {
                print("Registration issue")
            } else {
                print("Registered")
            }
        }
    }
    ```

    Para manter a simplicidade, você vai usar algumas instruções de impressão para atualizar a janela de saída com o resultado do *registrar* operação. 

3. Agora, compile e execute o aplicativo (em um dispositivo físico). Você deve ver **"Registrado"** na janela de saída.

## <a name="test-the-solution"></a>Testar a solução
Nesse estágio, nosso aplicativo está registrado com **NotificationHub** e ele pode receber notificações por push. Na **Xcode**, pare o depurador e feche o aplicativo (se ele estiver em execução). Em seguida, você irá verificar que o **instalação de dispositivo** detalhes estão conforme o esperado e que nosso aplicativo agora, de fato, pode receber notificações por push.  

### <a name="verify-the-device-installation"></a>Verifique se a instalação do dispositivo
Agora você pode fazer a mesma solicitação, como fez anteriormente usando **Postman** para [verificando o token SAS](#verify-the-sas-token). Supondo que o **token SAS** não tiver expirado, a resposta agora deve incluir os detalhes de instalação fornecidos, como as marcas e modelos.  

```json
{
    "installationId": "<installationId>",
    "pushChannel": "<pushChannel>",
    "pushChannelExpired": false,
    "platform": "apns",
    "expirationTime": "9999-12-31T23:59:59.9999999Z",
    "tags": [
        "12345"
    ],
    "templates": {
        "genericTemplate": {
            "body": "{\"aps\":{\"alert\":\"$(message)\"}}",
            "tags": [
                "genericTemplate"
            ]
        }
    }
}
```

### <a name="send-a-test-notification-azure-portal"></a>Enviar uma notificação de teste (portal do Azure)
A maneira mais rápida que agora você pode receber notificações de teste é navegar para o **Hub de notificação** na **portal do Azure**.

1. No **portal do Azure**, navegue até a **visão geral** guia seu **Hub de notificação**
2. Clique em **envio de teste** (esquerda) acima a **Essentials** resumo

    ![Conceitos básicos de Hubs de notificação botão de envio de teste de resumo](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-essentials-test-send.png)
3. Escolher **modelo personalizado** na lista de **plataformas**
4. Insira **12345** para o **enviar a expressão de marca** (especificadas essa marca na nossa instalação)
5. Se preferir, edite o **mensagem** na carga JSON
    
    ![Envio de teste de Hubs de notificação](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send.png)
6. Clique em **enviar** e o portal deve indicar se a notificação foi enviada com êxito para o dispositivo

    ![Resultados de envio de teste de Hubs de notificação](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/hub-test-send-result.png)

    Você também verá uma notificação na **Centro de notificações** em seu dispositivo (supondo que o aplicativo não está em execução em primeiro plano). Tocar na notificação deve abrir o aplicativo e mostrar o alerta.

    ![Exemplo de notificação recebida](./media/notification-hubs-ios-push-notifications-swift-apps-get-started/test-send-notification-received.png)

### <a name="send-a-test-notification-postman"></a>Enviar uma notificação de teste (Postman)
Você pode enviar notificações via os respectivos [API REST](/rest/api/notificationhubs/) via **Postman** as bem e ele podem ser uma maneira mais conveniente para testar. 

1. Na **Postman**, abra uma nova guia
2. Definir a solicitação **POST** e insira o endereço a seguir:

    ```xml
    https://<namespace>.servicebus.windows.net/<hubName>/messages/?api-version=2015-01
    ```

3. Configure os cabeçalhos de solicitação da seguinte maneira:
    
   | Chave                            | Value                          |
   | ------------------------------ | ------------------------------ |
   | Tipo de Conteúdo                   | application/json;charset=utf-8 |
   | Autorização                  | \<sasToken\>                   |
   | ServiceBusNotification-Format  | modelo                       |
   | tags                           | “12345”                        |

4. Configure a solicitação **corpo** usar **RAW - JSON (application.json)** com a seguinte carga JSON:

    ```json
    {
       "message" : "Hello from Postman!"
    }
    ```

5. Clique no **código** botão (canto superior direito sob a **salvar** botão). A solicitação deve ser semelhante ao exemplo a seguir:

    ```html
    POST /<hubName>/messages/?api-version=2015-01 HTTP/1.1
    Host: <namespace>.servicebus.windows.net
    Content-Type: application/json;charset=utf-8.
    ServiceBusNotification-Format: template
    Tags: "12345"
    Authorization: <sasToken>
    Cache-Control: no-cache
    Postman-Token: <postmanToken>

    {
        "message" : "Hello from Postman!"
    }
    ```

5. Clique o **enviar** botão

Você deve obter um código de status de êxito e receber a notificação no dispositivo cliente.

## <a name="next-steps"></a>Próximas etapas
Agora você tem um aplicativo Swift do iOS básico conectado a um **Hub de notificação** por meio de [API REST](/rest/api/notificationhubs/) e pode enviar e receber notificações. Para obter mais informações, consulte os seguintes artigos: 

- [Visão geral de Hubs de notificação do Azure](notification-hubs-push-notification-overview.md)
- [APIs REST dos Hubs de Notificação](/rest/api/notificationhubs/)
- [SDK de Hubs de notificação para operações de back-end](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [Hubs de notificação do SDK no GitHub](https://github.com/Azure/azure-notificationhubs)
- [Registrar com o back-end do aplicativo](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [Gerenciamento de registro](notification-hubs-push-notification-registration-management.md)
- [Trabalhando com marcas](notification-hubs-tags-segment-push-message.md) 
- [Trabalhando com modelos personalizados](notification-hubs-templates-cross-platform-push-messages.md)
- [Controle de acesso do barramento de serviço com assinaturas de acesso compartilhado](../service-bus-messaging/service-bus-sas.md)
- [Por meio de programação gerar tokens SAS](/rest/api/eventhub/generate-sas-token)
- [Segurança da Apple: criptografia comum](https://developer.apple.com/security/)
- [Hora da época do UNIX](https://en.wikipedia.org/wiki/Unix_time)
- [HMAC](https://en.wikipedia.org/wiki/HMAC)
