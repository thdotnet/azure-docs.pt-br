---
title: Considerações sobre o Xamarin Android (biblioteca de autenticação do Microsoft para .NET) | Azure
description: Saiba mais sobre as considerações específicas ao usar o Xamarin Android com a biblioteca de autenticação da Microsoft para .NET (MSAL.NET).
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
ms.date: 04/24/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: cb0cfb06e95cadbb549f669e5d59bdb0d795c896
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65545874"
---
# <a name="xamarin-android-specific-considerations-with-msalnet"></a>Considerações sobre o Xamarin Android específicos com MSAL.NET
Este artigo discute considerações específicas ao usar o Xamarin Android com a biblioteca de autenticação da Microsoft para .NET (MSAL.NET).

Este artigo serve para MSAL.NET 3.x. Se você estiver interessado em MSAL.NET 2.x, consulte [especificidades do Xamarin Android no MSAL.NET 2.x](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Xamarin-Android-specifics-2x).

## <a name="set-the-parent-activity"></a>Defina a atividade pai

No xamarin. Android, você precisa definir a atividade pai para que o token obtém novamente quando a interação aconteceu.

```csharp
var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .ExecuteAsync();
```

## <a name="ensuring-control-goes-back-to-msal-once-the-interactive-portion-of-the-authentication-flow-ends"></a>Garantir que o controle de volta para a MSAL uma vez a parte interativa das extremidades de fluxo de autenticação
No Android, você precisa substituir os `OnActivityResult` método da `Activity` e chame o método SetAuthenticationContinuationEventArgs da classe AuthenticationContinuationHelper MSAL.

```csharp
protected override void OnActivityResult(int requestCode, 
                                         Result resultCode, Intent data)
{
 base.OnActivityResult(requestCode, resultCode, data);
 AuthenticationContinuationHelper.SetAuthenticationContinuationEventArgs(requestCode,
                                                                         resultCode,
                                                                         data);
}

```
Essa linha garante que o controle de volta para a MSAL depois que a parte interativa do fluxo de autenticação terminou.

## <a name="update-the-android-manifest"></a>Atualize o manifesto do Android
O `AndroidManifest.xml` deve conter os seguintes valores:
```csharp
<activity android:name="microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="msal{client_id}" android:host="auth" />
         </intent-filter>
</activity>
```

## <a name="use-the-embedded-web-view-optional"></a>Use a exibição da web incorporado (opcional)

Por padrão, MSAL.NET usa o navegador da web do sistema, que permite que você obtenha o SSO com aplicativos Web e outros aplicativos. Em alguns casos raros, você talvez queira especificar que você deseja usar o modo de exibição da web incorporadas. Para obter mais informações, consulte [MSAL.NET usa um navegador da Web](msal-net-web-browsers.md) e [navegador de sistema Android](msal-net-system-browser-android-considerations.md).

```csharp
bool useEmbeddedWebView = !app.IsSystemWebViewAvailable;

var authResult = AcquireTokenInteractive(scopes)
 .WithParentActivityOrWindow(parentActivity)
 .WithEmbeddedWebView(useEmbeddedWebView)
 .ExecuteAsync();
```

## <a name="troubleshooting"></a>solução de problemas
Se você cria um novo aplicativo xamarin. Forms e adicione uma referência ao pacote MSAL.Net NuGet, isso apenas funcionará.
No entanto, se você quiser atualizar um aplicativo xamarin. Forms existente para MSAL.NET visualizar 1.1.2 ou mais tarde você poderá ter problemas de compilação.

Para solucionar esses problemas, você deve:
- Atualizar o pacote de NuGet MSAL.NET existente para visualização MSAL.NET 1.1.2 ou posterior
- Verifique que o xamarin. Forms atualizada automaticamente para a versão 2.5.0.122203 (caso contrário, a atualização para esta versão)
- Verifique que Xamarin.Android.Support.v4 automaticamente atualizado para a versão 25.4.0.2 (caso contrário, a atualização para esta versão)
- Todos os pacotes Support devem direcionar a versão do 25.4.0.2
- Limpar/recriação
- Tente configurar o projeto paralelo max compilações para 1 no Visual Studio (Opções -> projetos e soluções -> Build e executar -> o número máximo de compilações paralelas de projetos)
- Como alternativa, se você estiver compilando na linha de comando, tente remover /m do comando, se ele estiver em uso.


### <a name="error-the-name-authenticationcontinuationhelper-does-not-exist-in-the-current-context"></a>Erro: O nome 'AuthenticationContinuationHelper' não existe no contexto atual

Isso provavelmente ocorre porque o Visual Studio não atualizou corretamente o arquivo Android.csproj*. Às vezes, o **<HintPath>** filepath incorretamente contém netstandard13 em vez de **monoandroid90**.

```xml
<Reference Include="Microsoft.Identity.Client, Version=3.0.4.0, Culture=neutral, PublicKeyToken=0a613f4dd989e8ae,
           processorArchitecture=MSIL">
  <HintPath>..\..\packages\Microsoft.Identity.Client.3.0.4-preview\lib\monoandroid90\Microsoft.Identity.Client.dll</HintPath>
</Reference>
```

## <a name="next-steps"></a>Próximas etapas

Mais detalhes e exemplos são fornecidos na [Android considerações específicas](https://github.com/azure-samples/active-directory-xamarin-native-v2#android-specific-considerations) parágrafo do arquivo readme.md do exemplo a seguir:

| Amostra | Plataforma | DESCRIÇÃO |
| ------ | -------- | ----------- |
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/azure-samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | Um aplicativo Xamarin Forms simples, mostrando como usar MSAL para autenticar a MSA e o Azure AD por meio do ponto de extremidade v2.0 adicione e acessar o Microsoft Graph com o token resultante. <br>![Topologia](media/msal-net-xamarin-android-considerations/topology.png) |