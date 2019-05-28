---
title: Notificações por push para aplicativos Kindle usando Hubs de Notificação do Azure | Microsoft Docs
description: Neste tutorial, você aprende a usar os Hubs de Notificação do Azure para enviar notificações por push a um aplicativo Kindle.
services: notification-hubs
documentationcenter: ''
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 346fc8e5-294b-4e4f-9f27-7a82d9626e93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-kindle
ms.devlang: Java
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/29/2019
ms.author: jowargo
ms.openlocfilehash: edd0e12460e07cfd2990cc43a9056ed06b84fb1d
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64926677"
---
# <a name="get-started-with-notification-hubs-for-kindle-apps"></a>Introdução aos Hubs de Notificação para aplicativos do Kindle

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

Este tutorial mostra como usar os Hubs de Notificação do Azure para enviar notificações por push para um aplicativo Kindle. Você deve criar um aplicativo Kindle em branco que recebe notificações por push usando Amazon Device Messaging (ADM).

Neste tutorial, você cria/atualiza o código para executar as seguintes tarefas:

> [!div class="checklist"]
> * Adicionar um novo aplicativo ao portal do desenvolvedor
> * Criar uma chave para a API
> * Criar e configurar um hub de notificação
> * Configurar o aplicativo
> * Crie o manipulador de mensagens do ADM:
> * Adicione a chave de API ao seu aplicativo
> * Execute o aplicativo
> * Enviar uma notificação de teste

## <a name="prerequisites"></a>Pré-requisitos

- [Android Studio](https://developer.android.com/studio/?gclid=CjwKCAjwwZrmBRA7EiwA4iMzBPZ9YYmR0pbb5LtjnWhWCxe8PWrmjmeaR6ad5ksCr_j2mmkVj_-o6hoCAqwQAvD_BwE).
- Siga as etapas em [Configurar seu ambiente de desenvolvimento](https://developer.amazon.com/docs/fire-tablets/ft-set-up-your-development-environment.html) para configurar o ambiente de desenvolvimento para Kindle.

## <a name="add-a-new-app-to-the-developer-portal"></a>Adicionar um novo aplicativo ao portal do desenvolvedor

1. Entre no [Portal do Desenvolvedor da Amazon](https://developer.amazon.com/apps-and-games/console/apps/list.html).
2. Selecione **Adicionar Novo Aplicativo** e, em seguida, selecione **Android**.  

    ![Adicionar novo botão do aplicativo](./media/notification-hubs-kindle-get-started/add-new-app-button.png)
1. Na página **Novo Envio de Aplicativo**, siga estas etapas para obter a **chave do aplicativo**:
    1. Insira um nome para o **Nome do aplicativo**.
    2. Selecione qualquer **categoria** (por exemplo: educação)
    4. Insira um endereço de email para o campo **Endereço de email de suporte ao cliente**. 
    5. Clique em **Salvar**.

        ![Página do Novo Envio do Aplicativo](./media/notification-hubs-kindle-get-started/new-app-submission-page.png) 
2.  Na parte superior, mude para a guia **Mobile Ads** e siga estas etapas: 
    1. Especifique se seu aplicativo é direcionado principalmente a crianças com menos de 13 anos. Para este tutorial, selecione **Não**.
    2. Selecione **Enviar**. 

        ![Página Mobile Ads](./media/notification-hubs-kindle-get-started/mobile-ads-page.png)
    3. Copie a **chave do aplicativo** da página **Mobile Ads**. 

        ![Chave do aplicativo](./media/notification-hubs-kindle-get-started/application-key.png)
3.  Selecione o menu **Aplicativos e Serviços** na parte superior e selecione seu aplicativo na lista. 

    ![Selecione seu aplicativo na lista](./media/notification-hubs-kindle-get-started/all-apps-select.png)
4. Mude para a guia **Device Messaging** e, em seguida, siga estas etapas: 
    1. Selecione **Criar um Perfil de Segurança**.
    2. Insira um **nome** para seu perfil de segurança. 
    3. Insira uma **descrição** para seu perfil de segurança. 
    4. Clique em **Salvar**. 
    5. Selecione **Exibir Perfil de Segurança** na página de resultados. 
5. Agora, na página **Perfil de Segurança**, siga estas etapas: 
    1. Mude para a guia **Configurações da Web** e copie o valor **ID do Cliente** e **Segredo do Cliente** para uso posterior. 

        ![Obter a ID e o segredo do cliente](./media/notification-hubs-kindle-get-started/client-id-secret.png) 
    2. Mude para a página **Configurações do Android/Kindle** e mantenha a página aberta. Você inserirá esses valores na próxima seção. 

## <a name="create-an-api-key"></a>Criar uma chave para a API
1. Abra uma prompt de comando com privilégios de administrador.
2. Navegue até a pasta Android SDK.
3. Digite o seguinte comando:

    ```shell
    keytool -list -v -alias androiddebugkey -keystore ./debug.keystore
    ```
4. Para a senha do **keystore**, digite **android**.
5. Copie as impressões digitais **MD5** e **SHA256**. 
6. De volta no portal do desenvolvedor, na guia **Configurações do Android/Kindle**, siga estas etapas: 
    1. Insira um **nome para a chave de API**. 
    2. insira o **nome do pacote** para seu aplicativo (por exemplo, **com.fabrikam.mykindleapp**) e o valor **MD5**.
        
        >[!IMPORTANT]
        > Quando você cria um aplicativo no Android Studio, use o mesmo nome do pacote especificado aqui. 
    1. Cole a **Assinatura do MD5** que você copiou anteriormente. 
    2. Cole a **Assinatura do SHA256** que você copiou anteriormente.  
    3. Selecione **Gerar nova chave**.

        ![Configurações do Android/Kindle – gerar chave](./media/notification-hubs-kindle-get-started/android-kindle-settings.png)
    4. Agora, selecione **Mostrar** na lista para ver a chave de API. 

        ![Configurações do Android/Kindle – Mostrar chave de API](./media/notification-hubs-kindle-get-started/show-api-key-button.png) 
    5. Na janela **Detalhes da Chave de API**, copie a Chave de API e salve-a em outro lugar. Em seguida, selecione **X** no canto superior direito para fechar a janela. 


## <a name="create-and-configure-a-notification-hub"></a>Criar e configurar um hub de notificação

1. Siga as etapas no artigo [Criar um hub de notificação do Azure no portal do Azure](create-notification-hub-portal.md) para criar um hub de notificação. 
2. Selecione **Amazon (ADM)** no menu **Configurações**.
3. Cole a **ID do Cliente** e o **Segredo do Cliente** que você salvou anteriormente. 
4. Selecione **Salvar** na barra de ferramentas. 

    ![Configurar o ADM para um hub de notificação](./media/notification-hubs-kindle-get-started/configure-notification-hub.png)
5. Selecione **Políticas de Acesso** no menu esquerdo e selecione o botão de **copiar** da cadeia de conexão para a política **DefaultListenSharedAccessSignature**. Salve-a em algum lugar. Você o usará posteriormente no código-fonte. 

    ![Hub de Notificação – escutar cadeia de conexão](./media/notification-hubs-kindle-get-started/event-hub-listen-connection-string.png)    

## <a name="set-up-your-application"></a>Configurar o aplicativo

1. Inicie o Android Studio. 
2. Selecione **Arquivo**, aponte para **Novo** e selecione **Novo Projeto**. 
3. Na janela **Escolher o projeto**, na guia **Telefone e Tablet**, selecione **Atividade Vazia** e **Avançar**. 
4. Na janela **Configurar o projeto**, execute as seguintes etapas:
    1. Insira um **nome para seu aplicativo**. Talvez você queira fazê-lo corresponder ao nome do aplicativo que você criou no Portal do Desenvolvedor da Amazon. 
    2. Insira um **nome para o pacote**. 
        
        >[!IMPORTANT]
        >O nome do pacote deve corresponder ao nome do pacote especificado no Portal do Desenvolvedor da Amazon.
    3. Examine e atualize os valores restantes conforme apropriado. 
    4. Selecione **Concluir**. 

        ![Configurar projeto do Android](./media/notification-hubs-kindle-get-started/new-android-studio-project.png)
5. Baixe a biblioteca [SDK do Desenvolvedor da Amazon para Android](https://developer.amazon.com/sdk-download) para seu disco rígido. Extraia o arquivo zip do SDK.
6. No Android Studio, altere a estrutura de pastas de **Android** para **Projeto** se ela ainda não estiver definida como **Projeto**. 

    ![Android Studio – Mudar para estrutura de projeto](./media/notification-hubs-kindle-get-started/android-studio-project-view.png)
7. Expanda o **aplicativo** para ver a pasta **libs** no modo de exibição de árvore.     
8. Em uma janela do Explorador de Arquivos, navegue até a pasta em que você baixou os SDKs do Android da Amazon.
9. Pressione **CTRL** e arraste e solte o arquivo **amazon-device-messaging-1.0.1.jar** no nó **lib** no modo de exibição de árvore. 

    ![Android Studio – Adicionar JAR do Amazon Device Messaging](./media/notification-hubs-kindle-get-started/drag-drop-amazon-device-messaging-jar.png)
9. Na janela **Copiar**, selecione **OK**. Se você vir a janela **Mover**, em vez da janela **Copiar**, feche-a e tente executar a operação de arrastar e soltar com o botão **CTRL** pressionado. 

    ![Android Studio – Copiar JAR](./media/notification-hubs-kindle-get-started/copy-jar-window.png)
10. Adicione a seguinte instrução ao arquivo **build.gradle do aplicativo** na seção **dependências**: `implementation files('libs/amazon-device-messaging-1.0.1.jar')`. 

    ![Android Studio – adicionar ADM a build.gradle do aplicativo](./media/notification-hubs-kindle-get-started/adm-build-gradle.png)
11. No arquivo `Build.Gradle` do **aplicativo**, adicione as seguintes linhas na seção de **dependências**: 

    ```gradle
    implementation 'com.microsoft.azure:notification-hubs-android-sdk:0.6@aar'
    implementation 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```
12. Adicione o seguinte repositório **após** a seção de **dependências**:

    ```gradle
    repositories {
        maven {
            url "https://dl.bintray.com/microsoftazuremobile/SDK"
        }
    }
    ```
13. No editor do arquivo **build.gradle** para o **aplicativo**, selecione **Sincronizar agora** na barra de ferramentas. 

    ![Android Studio – sincronizar build.gradle do aplicativo](./media/notification-hubs-kindle-get-started/gradle-sync-now.png)
14. Volte para a estrutura do Android no modo de exibição de árvore.  Adicione o namespace Amazon no elemento raiz do manifesto:

    ```xml
    xmlns:amazon="http://schemas.amazon.com/apk/res/android"
    ```
   
    ![Namespace Amazon no manifesto](./media/notification-hubs-kindle-get-started/amazon-namespace-manifest.png)
2. Adicione permissões como o primeiro elemento sob o elemento manifest. Substitua **[NOME DO SEU PACOTE]** pelo pacote que você usou para criar o aplicativo.

    ```xml
    <permission
        android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE"
        android:protectionLevel="signature" />

    <uses-permission android:name="android.permission.INTERNET"/>

    <uses-permission android:name="[YOUR PACKAGE NAME].permission.RECEIVE_ADM_MESSAGE" />

    <!-- This permission allows your app access to receive push notifications
    from ADM. -->
    <uses-permission android:name="com.amazon.device.messaging.permission.RECEIVE" />

    <!-- ADM uses WAKE_LOCK to keep the processor from sleeping when a message is received. -->
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    ```
3. Insira o seguinte elemento como o primeiro filho do elemento application. Substitua **[NOME DO SEU PACOTE]** pelo nome do pacote com o qual você criou seu aplicativo. Você criará a classe MyADMMessageHandler na próxima etapa. 

    ```xml
        <amazon:enable-feature
            android:name="com.amazon.device.messaging"
            android:required="true"/>
        <service
            android:name="[YOUR PACKAGE NAME].MyADMMessageHandler"
            android:exported="false" />
        <receiver
            android:name="[YOUR PACKAGE NAME].MyADMMessageHandler$Receiver"
            android:permission="com.amazon.device.messaging.permission.SEND" >

            <!-- To interact with ADM, your app must listen for the following intents. -->
            <intent-filter>
                <action android:name="com.amazon.device.messaging.intent.REGISTRATION" />
                <action android:name="com.amazon.device.messaging.intent.RECEIVE" />
                <!-- Replace the name in the category tag with your app's package name. -->
                <category android:name="[YOUR PACKAGE NAME]" />
            </intent-filter>
        </receiver>
    ```

## <a name="create-your-adm-message-handler"></a>Crie o manipulador de mensagens do ADM:

1. Adicione uma nova classe ao pacote `com.fabrikam.mykindleapp` no projeto herdado de `com.amazon.device.messaging.ADMMessageHandlerBase` e nomeie-o `MyADMMessageHandler`, como mostrado na imagem a seguir:

    ![Criar classe MyADMMessageHandler](./media/notification-hubs-kindle-get-started/create-adm-message-handler.png)

2. Adicione as seguintes instruções `import` à classe `MyADMMessageHandler`:

    ```java
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;
    import android.support.v4.app.NotificationCompat;
    import android.util.Log;
    import com.amazon.device.messaging.ADMMessageReceiver;
    import com.microsoft.windowsazure.messaging.NotificationHub;
    ```

3. Adicione o seguinte código à classe criada. Lembre-se do `[HUB NAME]` e do `[LISTEN CONNECTION STRING]` com o nome do seu hub de notificação e a cadeia de conexão de escuta: 

    ```java
    public static final int NOTIFICATION_ID = 1;
    private NotificationManager mNotificationManager;
    NotificationCompat.Builder builder;
    private static NotificationHub hub;

    public static NotificationHub getNotificationHub(Context context) {
        Log.v("com.wa.hellokindlefire", "getNotificationHub");
        if (hub == null) {
            hub = new NotificationHub("[HUB NAME]", "[HUB NAMESPACE CONNECTION STRING]", context);
        }
        return hub;
    }

    public MyADMMessageHandler() {
        super("MyADMMessageHandler");
    }

    @Override
    protected void onMessage(Intent intent) {
        String nhMessage = intent.getExtras().getString("msg");
        sendNotification(nhMessage);
    }

    @Override
    protected void onRegistrationError(String s) {

    }

    @Override
    protected void onRegistered(String s) {
        try {
            getNotificationHub(getApplicationContext()).register(s);
        } catch (Exception e) {
            Log.e("[your package name]", "Fail onRegister: " + e.getMessage(), e);
        }
    }

    @Override
    protected void onUnregistered(String s) {
        try {
            getNotificationHub(getApplicationContext()).unregister();
        } catch (Exception e) {
            Log.e("[your package name]", "Fail onUnregister: " + e.getMessage(), e);
        }
    }

    public static class Receiver extends ADMMessageReceiver
    {
        public Receiver()
        {
            super(MyADMMessageHandler.class);
        }
    }

    private void sendNotification(String msg) {
        Context ctx = getApplicationContext();

        mNotificationManager = (NotificationManager)
                ctx.getSystemService(Context.NOTIFICATION_SERVICE);

        PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                new Intent(ctx, MainActivity.class), 0);


        NotificationCompat.Builder mBuilder =
                new NotificationCompat.Builder(ctx)
                        .setSmallIcon(R.mipmap.ic_launcher)
                        .setContentTitle("Notification Hub Demo")
                        .setStyle(new NotificationCompat.BigTextStyle()
                                .bigText(msg))
                        .setContentText(msg);

        mBuilder.setContentIntent(contentIntent);
        mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
    }
    ```

## <a name="add-your-api-key-to-your-app"></a>Adicione a chave de API ao seu aplicativo
1. Siga estas etapas para adicionar a pasta de ativos ao projeto. 
    1. Mude para a exibição **Projeto**. 
    2. Clique com o botão direito do mouse em **aplicativo**.
    3. Selecione **Novo**.
    4. Selecione **Pasta**. 
    5. Em seguida, selecione **Pasta de Ativos**. 

        ![Menu Adicionar pasta de ativos](./media/notification-hubs-kindle-get-started/add-assets-folder-menu.png)    
    6. Na página **Configurar Componente**, execute as seguintes etapas:
        1. Selecione **Alterar local da pasta**
        2. Confirme se a pasta foi definida como: `src/main/assets`.
        3. Selecione **Concluir**. 
        
            ![Configurar pasta de ativos](./media/notification-hubs-kindle-get-started/configure-asset-folder.png)
2. Adicione um arquivo chamado **api_key.txt** à pasta de **ativos**. No modo de exibição de árvore, expanda **app**, **src**, **main** e clique com o botão direito do mouse em **assets**, aponte para **Novo** e, em seguida, selecione **Arquivo**. Insira **api_key.txt** para o nome do arquivo. 3. 
5. Copie a chave de API gerada no portal do desenvolvedor da Amazon para o arquivo api_key.txt. 
6. Compile o projeto. 

## <a name="run-the-app"></a>Execute o aplicativo
1. No dispositivo Kindle, arraste de cima para baixo e clique em **Configurações**; em seguida, clique em **Minha conta** e registre-se com uma conta da Amazon válida.
2. Execute o aplicativo em um dispositivo Kindle do Android Studio. 

> [!NOTE]
> Se ocorrer um problema, verifique a hora do emulador (ou dispositivo). O valor de hora deve ser preciso. Para alterar a hora do emulador do Kindle, você pode executar o seguinte comando no diretório de ferramentas da plataforma do SDK do Android:

```shell
adb shell  date -s "yyyymmdd.hhmmss"
```

## <a name="send-a-notification-message"></a>Enviar uma mensagem de notificação

Para enviar uma mensagem usando o .NET:

```csharp
static void Main(string[] args)
{
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("[conn string]", "[hub name]");

    hub.SendAdmNativeNotificationAsync("{\"data\":{\"msg\" : \"Hello from .NET!\"}}").Wait();
}
```

Para obter um exemplo de código, confira [este exemplo no GitHub](https://github.com/Azure/azure-notificationhubs-dotnet/blob/master/Samples/SendPushSample/SendPushSample/Program.cs).

![][7]

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você envia notificações de transmissão para todos os seus dispositivos Kindle registrados com o back-end. Para saber como enviar notificações por push a dispositivos Kindle específicos, vá para o tutorial a seguir:  o tutorial a seguir mostra como enviar notificações por push para dispositivos Android específicos, mas você pode usar a mesma lógica para enviar notificações para dispositivos Kindle específicos.

> [!div class="nextstepaction"]
>[Notificações por push para especificar dispositivos](notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md)

<!-- URLs. -->
[Amazon developer portal]: https://developer.amazon.com/home.html
[download the SDK]: https://developer.amazon.com/public/resources/development-tools/sdk
[0]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal1.png
[1]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal2.png
[2]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal3.png
[3]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal4.png
[4]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-portal5.png
[5]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-cmd-window.png
[6]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-new-java-class.png
[7]: ./media/notification-hubs-kindle-get-started/notification-hub-kindle-notification.png
