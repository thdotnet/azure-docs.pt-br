---
title: Introdução ao Android – Plataforma de identidade da Microsoft | Azure
description: Como um aplicativo do Android pode obter um token de acesso e chamar a API do Microsoft Graph ou APIs que exigem tokens de acesso por meio da plataforma de identidade da Microsoft.
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
ms.openlocfilehash: 6112facfc0c10d7a0a0495cd778fa6c3cb6130a7
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65962143"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-android-app"></a>Conectar usuários e chamar o Microsoft Graph em um aplicativo Android

Neste tutorial, você aprenderá como integrar um aplicativo Android à plataforma de identidade da Microsoft. Especificamente, seu aplicativo conectará um usuário, obterá um token de acesso para chamar a API do Microsoft Graph e fará uma solicitação para a API do Microsoft Graph.  

Após concluir este guia, seu aplicativo aceitará conexões de contas Microsoft pessoais (incluindo outlook.com, live.com e outras) e contas corporativas ou de estudante de qualquer empresa ou organização que utilize o Azure Active Directory.

## <a name="how-this-tutorial-works"></a>Como este tutorial funciona

![Mostra como funciona o aplicativo de exemplo gerado por este tutorial](../../../includes/media/active-directory-develop-guidedsetup-android-intro/android-intro.svg)

O aplicativo nesse exemplo conectará usuários e obterá dados em seu nome.  Esses dados serão acessados ​​por meio de uma API protegida (API do Microsoft Graph neste caso) que requer autorização.

Mais especificamente:

* Seu aplicativo conectará o usuário por meio de um navegador ou do Microsoft Authenticator e pelo Portal da Empresa do Intune.
* O usuário final aceitará as permissões solicitadas por seu aplicativo. 
* Será emitido um token de acesso para seu aplicativo para a API do Microsoft Graph.
* O token de acesso será incluído na solicitação HTTP para API Web.
* Processe a resposta do Microsoft Graph.

Este exemplo usa a biblioteca de autenticação da Microsoft para Android (MSAL) para implementar a autenticação. A MSAL automaticamente renovará tokens, fornecerá o SSO entre outros aplicativos no dispositivo e ajudará a gerenciar as contas.

## <a name="prerequisites"></a>Pré-requisitos

* Esta configuração guiada usa o Android Studio.
* O Android 16 ou posterior é necessário (recomendável o 19+).

## <a name="library"></a>Biblioteca

Este guia usa a seguinte biblioteca de autenticação:

|Biblioteca|DESCRIÇÃO|
|---|---|
|[com.microsoft.identity.client](https://javadoc.io/doc/com.microsoft.identity.client/msal)|MSAL (Biblioteca de Autenticação da Microsoft)|

## <a name="set-up-your-project"></a>Configurar o seu projeto

Este tutorial irá criar um novo projeto. Se, ao invés disso, você quiser baixar o tutorial concluído, [baixe o código](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip).

### <a name="create-a-new-project"></a>Criar um novo projeto

1. Abra o Android Studio e selecione **Iniciar um novo projeto do Android Studio**.
    - Se o Android Studio já estiver aberto, selecione **Arquivo** > **Novo** > **Novo Projeto**.
2. Deixe **Atividade vazia** como está, selecione **Avançar**.
3. Nomeie seu aplicativo, defina o `Minimum API level` como **API 19 ou mais recente**, clique em **Concluir**.
5. Em `app/build.gradle`, defina `targetedSdkVersion` como 27. 

## <a name="register-your-application"></a>Registre seu aplicativo

Você pode registrar seu aplicativo de duas maneiras, conforme descrito nas duas seções a seguir.

### <a name="register-your-app"></a>Registrar seu aplicativo

1. Acesse o [portal do Azure](https://aka.ms/MobileAppReg) > selecione `New registration`. 
2. Digite um **Nome** para o aplicativo > `Register`. **Não defina um URI de redirecionamento neste estágio**. 
3. Na seção `Manage`, acesse `Authentication` > `Add a platform` > `Android`
    - Insira o nome do pacote do seu projeto. Se você baixou o código, esse valor é `com.azuresamples.msalandroidapp`. 
    - Digite seu hash de assinatura de depuração/desenvolvimento. Use o comando KeyTool no portal para gerar o Hash de Assinatura. 
4. Clique em `Configure` e armazene a ***Configuração de MSAL*** para uso posterior. 

## <a name="build-your-app"></a>Compilar seu aplicativo

### <a name="configure-your-android-app"></a>Configurar seu aplicativo Android

1. Clique com o botão direito do mouse em **res** > **Novo** > **Pasta** > **Pasta de Recursos Brutos**
2. Em **aplicativo** > **res** > **bruto**, crie um novo arquivo JSON chamado `auth_config.json` e cole sua ***Configuração de MSAL***. Confira [Configuração de MSAL para obter mais informações](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki/Configuring-your-app).
   <!-- Workaround for Docs conversion bug -->
3. Em **aplicativo** > **manifestos** > **AndroidManifest.xml**, adicione a atividade `BrowserTabActivity` abaixo. Essa entrada permite que a Microsoft faça uma chamada de retorno ao aplicativo após concluir a autenticação:

    ```xml
    <!--Intent filter to capture System Browser or Authenticator calling back to our app after sign-in-->
    <activity
        android:name="com.microsoft.identity.client.BrowserTabActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="msauth"
                android:host="Enter_the_Package_Name"
                android:path="/Enter_the_Signature_Hash" />
        </intent-filter>
    </activity>
    ```

    Observe que o Hash de Assinatura usado NÃO deve ser a URL codificada no arquivo **AndroidManifest.xml**. 

4. No arquivo **AndroidManifest.xml** e acima da marca `<application>`, adicione as seguintes permissões:

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    ```

5. Em `BrowserTabActivity`, substitua o ***Nome do Pacote*** e ***Hash de Assinatura*** pelos valores registrados no portal do Azure.

### <a name="create-the-apps-ui"></a>Criar a interface do usuário do aplicativo

1. Vá para **res** > **layout**e, em seguida, abra **activity_main.xml**.
2. Altere o layout de atividade de `android.support.constraint.ConstraintLayout` ou outro para `LinearLayout`.
3. Adicionar o propriedade `android:orientation="vertical"` ao nó `LinearLayout`.
4. Cole o seguinte código no nó `LinearLayout`, substituindo o conteúdo atual:

    ```xml
    <TextView
        android:text="Welcome, "
        android:textColor="#3f3f3f"
        android:textSize="50px"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="10dp"
        android:layout_marginTop="15dp"
        android:id="@+id/welcome"
        android:visibility="invisible"/>

    <Button
        android:id="@+id/callGraph"
        android:text="Call Microsoft Graph"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="200dp"
        android:textAllCaps="false" />

    <TextView
        android:text="Getting Graph Data..."
        android:textColor="#3f3f3f"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"
        android:id="@+id/graphData"
        android:visibility="invisible"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dip"
        android:layout_weight="1"
        android:gravity="center|bottom"
        android:orientation="vertical" >

        <Button
            android:text="Sign Out"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="15dp"
            android:textColor="#FFFFFF"
            android:background="#00a1f1"
            android:textAllCaps="false"
            android:id="@+id/clearCache"
            android:visibility="invisible" />
    </LinearLayout>
    ```

### <a name="add-msal-to-your-project"></a>Adicionar MSAL ao seu projeto

1. No Android Studio, selecione **Gradle Scripts** > **build.gradle (Módulo: aplicativo)**.
2. Em **Dependências**, cole o seguinte código:

    ```gradle  
    implementation 'com.android.volley:volley:1.1.1'
    implementation 'com.microsoft.identity.client:msal:0.3.+'
    ```

### <a name="use-msal"></a>Usar a MSAL 

As próximas seções farão alterações dentro de `MainAcitivty.java`. Vamos percorrer cada etapa necessária para adicionar e usar a MSAL no seu aplicativo.

#### <a name="required-imports"></a>Importações necessárias

Adicione as seguintes importações ao seu projeto: 

```java
import android.app.Activity;
import android.content.Intent;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;
import com.android.volley.*;
import com.android.volley.toolbox.JsonObjectRequest;
import com.android.volley.toolbox.Volley;
import org.json.JSONObject;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import com.microsoft.identity.client.*;
import com.microsoft.identity.client.exception.*;
```

#### <a name="instantiating-msal"></a>Criar uma instância da MSAL 

Na classe `MainActivity`, precisaremos criar uma instância da MSAL juntamente com algumas configurações sobre o que o aplicativo fará, incluindo os escopos e a API Web que desejamos acessar. 

Copie as seguintes variáveis ​​dentro de `MainActivity`:

```java
final static String SCOPES [] = {"https://graph.microsoft.com/User.Read"};
final static String MSGRAPH_URL = "https://graph.microsoft.com/v1.0/me";

/* UI & Debugging Variables */
private static final String TAG = MainActivity.class.getSimpleName();
Button callGraphButton;
Button signOutButton;

/* Azure AD Variables */
private PublicClientApplication sampleApp;
private IAuthenticationResult authResult;
```

Agora para criar uma instância da MSAL, copie o seguinte código dentro do método `onCreate(...)`:

```java
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);

callGraphButton = (Button) findViewById(R.id.callGraph);
signOutButton = (Button) findViewById(R.id.clearCache);

callGraphButton.setOnClickListener(new View.OnClickListener() {
    public void onClick(View v) {
        onCallGraphClicked();
    }
});

signOutButton.setOnClickListener(new View.OnClickListener() {
    public void onClick(View v) {
        onSignOutClicked();
    }
});

/* Configure your sample app and save state for this activity */
sampleApp = new PublicClientApplication(
        this.getApplicationContext(),
        R.raw.auth_config);

/* Attempt to get a user and acquireTokenSilent */
sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
    @Override
    public void onAccountsLoaded(final List<IAccount> accounts) {
        if (!accounts.isEmpty()) {
            /* This sample doesn't support multi-account scenarios, use the first account */
            sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
        } else {
            /* No accounts */
        }
    }
});
```

O bloco de código acima tenta conectar silenciosamente os usuários quando eles abrem o aplicativo por meio de `getAccounts(...)` e, se bem-sucedidos, `acquireTokenSilentAsync(...)`.  Nas próximas seções, implementaremos o manipulador de retorno de chamada para o caso de não haver contas conectadas. 

#### <a name="use-msal-to-get-tokens"></a>Usar a MSAL para obter tokens

Agora, podemos implementar a interface de usuário do aplicativo lógica de processamento e a obtenção de tokens interativamente por meio da MSAL. 

A MSAL expõe dois métodos principais para obter tokens: `acquireTokenSilentAsync` e `acquireToken`.  

`acquireTokenSilentAsync` conecta um usuário e obtém tokens sem qualquer interação do usuário se uma conta estiver presente. Se tiver êxito, a MSAL enviará os tokens para seu aplicativo, se falhar, ela irá gerar um `MsalUiRequiredException`.  Se essa exceção for gerada ou se você quiser que seu usuário tenha uma experiência de conexão interativa (credenciais, mfa ou outras políticas de acesso condicional podem ou não ser necessárias), então você pode usar `acquireToken`.  

`acquireToken` sempre mostrará a interface do usuário ao tentar entrar no usuário e obter tokens; no entanto, ela pode usar cookies de sessão no navegador ou uma conta no autenticador da Microsoft para fornecer uma experiência de SSO interativa. 

Para começar, crie os três métodos de interface do usuário a seguir dentro da classe `MainActivity`:

```java
/* Set the UI for successful token acquisition data */
private void updateSuccessUI() {
    callGraphButton.setVisibility(View.INVISIBLE);
    signOutButton.setVisibility(View.VISIBLE);
    findViewById(R.id.welcome).setVisibility(View.VISIBLE);
    ((TextView) findViewById(R.id.welcome)).setText("Welcome, " +
            authResult.getAccount().getUsername());
    findViewById(R.id.graphData).setVisibility(View.VISIBLE);
}

/* Set the UI for signed out account */
private void updateSignedOutUI() {
    callGraphButton.setVisibility(View.VISIBLE);
    signOutButton.setVisibility(View.INVISIBLE);
    findViewById(R.id.welcome).setVisibility(View.INVISIBLE);
    findViewById(R.id.graphData).setVisibility(View.INVISIBLE);
    ((TextView) findViewById(R.id.graphData)).setText("No Data");

    Toast.makeText(getBaseContext(), "Signed Out!", Toast.LENGTH_SHORT)
            .show();
}

/* Use MSAL to acquireToken for the end-user
 * Callback will call Graph api w/ access token & update UI
 */
private void onCallGraphClicked() {
    sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());
}
```

Em seguida, adicione um método para obter a atividade atual e processar retornos de chamada silenciosos e interativos:

```java
public Activity getActivity() {
    return this;
}

/* Callback used in for silent acquireToken calls.
 * Looks if tokens are in the cache (refreshes if necessary and if we don't forceRefresh)
 * else errors that we need to do an interactive request.
 */
private AuthenticationCallback getAuthSilentCallback() {
    return new AuthenticationCallback() {

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            /* Successfully got a token, call graph now */
            Log.d(TAG, "Successfully authenticated");

            /* Store the authResult */
            authResult = authenticationResult;

            /* call graph */
            callGraphAPI();

            /* update the UI to post call graph state */
            updateSuccessUI();
        }

        @Override
        public void onError(MsalException exception) {
            /* Failed to acquireToken */
            Log.d(TAG, "Authentication failed: " + exception.toString());

            if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside the exception */
            } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
            } else if (exception instanceof MsalUiRequiredException) {
                /* Tokens expired or no session, retry with interactive */
            }
        }

        @Override
        public void onCancel() {
            /* User cancelled the authentication */
            Log.d(TAG, "User cancelled login.");
        }
    };
}

/* Callback used for interactive request.  If succeeds we use the access
 * token to call the Microsoft Graph. Does not check cache
 */
private AuthenticationCallback getAuthInteractiveCallback() {
    return new AuthenticationCallback() {

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            /* Successfully got a token, call graph now */
            Log.d(TAG, "Successfully authenticated");
            Log.d(TAG, "ID Token: " + authenticationResult.getIdToken());

            /* Store the auth result */
            authResult = authenticationResult;

            /* call graph */
            callGraphAPI();

            /* update the UI to post call graph state */
            updateSuccessUI();
        }

        @Override
        public void onError(MsalException exception) {
            /* Failed to acquireToken */
            Log.d(TAG, "Authentication failed: " + exception.toString());

            if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside the exception */
            } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
            }
        }

        @Override
        public void onCancel() {
            /* User cancelled the authentication */
            Log.d(TAG, "User cancelled login.");
        }
    };
}
```

#### <a name="use-msal-for-sign-out"></a>Usar a MSAL para saída

Em seguida, adicionaremos suporte sobre a saída do nosso aplicativo. 

É importante observar que a saída com a MSAL remove todas as informações conhecidas sobre um usuário deste aplicativo, mas o usuário ainda terá uma sessão ativa em seu dispositivo. Se o usuário tentar conectar novamente, poderá observar interação, mas talvez não precise inserir novamente suas credenciais, pois a sessão do dispositivo ainda estará ativa. 

Para adicionar a saída, copie o seguinte método no seu aplicativo, ele percorre todas as contas e as remove:

```java
/* Clears an account's tokens from the cache.
 * Logically similar to "sign out" but only signs out of this app.
 * User will get interactive SSO if trying to sign back-in.
 */
private void onSignOutClicked() {
    /* Attempt to get a user and acquireTokenSilent
     * If this fails we do an interactive request
     */
    sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
        @Override
        public void onAccountsLoaded(final List<IAccount> accounts) {

            if (accounts.isEmpty()) {
                /* No accounts to remove */

            } else {
                for (final IAccount account : accounts) {
                    sampleApp.removeAccount(
                            account,
                            new PublicClientApplication.AccountsRemovedCallback() {
                        @Override
                        public void onAccountsRemoved(Boolean isSuccess) {
                            if (isSuccess) {
                                /* successfully removed account */
                            } else {
                                /* failed to remove account */
                            }
                        }
                    });
                }
            }

            updateSignedOutUI();
        }
    });
}
```

#### <a name="call-the-microsoft-graph-api"></a>Chamar a API do Microsoft Graph

Depois de obtermos um token com sucesso, podemos fazer uma solicitação à API do Microsoft Graph. O token de acesso estará no `AuthenticationResult` dentro do método `onSuccess(...)` do retorno de chamada da autenticação. Para criar uma solicitação autorizada, seu aplicativo precisará adicionar o token de acesso ao cabeçalho HTTP:

| chave de cabeçalho    | value                 |
| ------------- | --------------------- |
| Autorização | Bearer <token-de-acesso> |

Para fazer isso no código, adicione os dois métodos a seguir ao seu aplicativo para chamar o gráfico e atualizar a interface do usuário: 

```java
    /* Use Volley to make an HTTP request to the /me endpoint from MS Graph using an access token */
private void callGraphAPI() {
    Log.d(TAG, "Starting volley request to graph");

    /* Make sure we have a token to send to graph */
    if (authResult.getAccessToken() == null) {return;}

    RequestQueue queue = Volley.newRequestQueue(this);
    JSONObject parameters = new JSONObject();

    try {
        parameters.put("key", "value");
    } catch (Exception e) {
        Log.d(TAG, "Failed to put parameters: " + e.toString());
    }
    JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
            parameters,new Response.Listener<JSONObject>() {
        @Override
        public void onResponse(JSONObject response) {
            /* Successfully called graph, process data and send to UI */
            Log.d(TAG, "Response: " + response.toString());

            updateGraphUI(response);
        }
    }, new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            Log.d(TAG, "Error: " + error.toString());
        }
    }) {
        @Override
        public Map<String, String> getHeaders() {
            Map<String, String> headers = new HashMap<>();
            headers.put("Authorization", "Bearer " + authResult.getAccessToken());
            return headers;
        }
    };

    Log.d(TAG, "Adding HTTP GET to Queue, Request: " + request.toString());

    request.setRetryPolicy(new DefaultRetryPolicy(
            3000,
            DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
            DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
    queue.add(request);
}

/* Sets the graph response */
private void updateGraphUI(JSONObject graphResponse) {
    TextView graphText = findViewById(R.id.graphData);
    graphText.setText(graphResponse.toString());
}
```

Saiba mais sobre a [API do Microsoft Graph](https://graph.microsoft.com)!

#### <a name="multi-account-applications"></a>Aplicativos de várias contas

Este aplicativo foi criado para um cenário de conta única. A MSAL também dá suporte a cenários de várias contas, mas requer algum trabalho adicional dos aplicativos. Você precisará criar a interface do usuário para ajudar a selecionar qual conta deseja usar para cada ação que requer tokens do usuário. Como alternativa, seu aplicativo pode implementar uma heurística para selecionar qual conta usar por meio do método `getAccounts(...)`. 

## <a name="test-your-app"></a>Testar seu aplicativo

### <a name="run-locally"></a>Executar localmente

Se você seguiu o código acima, tente criar e implantar o aplicativo em um dispositivo de teste ou emulador. Você deve conseguir conectar e obter tokens para o Azure AD ou contas Microsoft pessoais! Após a conexão do usuário, este aplicativo exibirá os dados retornados do ponto de extremidade `/me` do Microsoft Graph. 

No caso de qualquer problema, fique à vontade para inserir um problema neste documento ou na biblioteca do MSAL e nos avise. 

### <a name="consent-to-your-app"></a>Consentimento para seu aplicativo

Na primeira vez que um usuário entrar no seu aplicativo, ele será solicitado pela identidade da Microsoft a consentir com as permissões solicitadas.  Embora a maioria dos usuários seja capaz de dar seu consentimento, alguns locatários do Azure AD desabilitaram o consentimento do usuário, exigindo que os administradores consintam em nome de todos os usuários.  Para dar suporte a esse cenário, certifique-se de registrar os escopos do seu aplicativo no portal do Azure.

## <a name="help-and-support"></a>Ajuda e Suporte

Teve algum problema com este tutorial ou com a plataforma de identidade da Microsoft? Confira [Ajuda e suporte](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options)
