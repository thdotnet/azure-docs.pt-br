---
title: O daemon aplicativo chamar APIs da web (configuração do aplicativo) – plataforma de identidade da Microsoft
description: Saiba como criar um aplicativo daemon que chamadas de web APIs (configuração de aplicativo)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: fd2da6baecdce3ab85a45347f27f573bf814445d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055757"
---
# <a name="daemon-app-that-calls-web-apis---code-configuration"></a>Aplicativo daemon que chamadas às APIs - configuração de código da web

Saiba como configurar o código para seu aplicativo de daemon que chamadas às APIs da web.

## <a name="msal-libraries-supporting-daemon-apps"></a>Aplicativos daemon suporte de bibliotecas MSAL

As bibliotecas da Microsoft que dão suporte a aplicativos daemon são:

  Biblioteca MSAL | DESCRIÇÃO
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Plataformas com suporte para criar um aplicativo daemon são plataformas do .NET Framework e .NET Core (não UWP, xamarin. IOS e xamarin. Android como essas plataformas são usados para criar aplicativos de cliente público)
  ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL.Python | Desenvolvimento em andamento - em visualização pública
  ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL.Java | Desenvolvimento em andamento - em visualização pública

## <a name="configuration-of-the-authority"></a>Configuração da autoridade

Considerando que os aplicativos daemon não usam permissões delegadas, mas as permissões de aplicativo, seus *suporte para o tipo de conta* não pode ser *(de contas de contas em qualquer diretório organizacional e o pessoal da Microsoft Por exemplo, o Skype, Xbox, Outlook.com)* . Na verdade, não há nenhum administrador de locatário para dar consentimento para o aplicativo de daemon para contas pessoais da Microsoft. Você precisará escolher *contas em minha organização* ou *contas de qualquer organização*.

Portanto, a autoridade especificada na configuração do aplicativo deve ser ed de locatário (especificando uma ID de locatário ou um nome de domínio associado com sua organização).

Se você for um ISV e quiser fornecer uma ferramenta de multilocatário, você pode usar `organizations`. Mas tenha em mente que você também precisará explicar aos seus clientes como conceder consentimento do administrador. Ver [solicitar o consentimento para um locatário inteiro](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant) para obter detalhes. Também no momento, há uma limitação na MSAL que `organizations` só é permitido quando as credenciais do cliente são um segredo do aplicativo (não um certificado). Consulte [MSAL.NET bug #891](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues/891)

## <a name="application-configuration-and-instantiation"></a>Instanciação e a configuração de aplicativo

Em bibliotecas MSAL, as credenciais do cliente (segredo ou certificado) são passadas como um parâmetro de construção de aplicativo cliente confidencial.

> [!IMPORTANT]
> Mesmo que seu aplicativo é um aplicativo de console em execução como um serviço, se ele for um aplicativo daemon ele precisa ser um aplicativo cliente confidencial.

### <a name="msalnet"></a>MSAL.NET

Adicione a [Microsoft.IdentityClient](https://www.nuget.org/packages/Microsoft.Identity.Client) pacote NuGet ao seu aplicativo.

Usar o namespace MSAL.NET

```CSharp
using Microsoft.Identity.Client;
```

O aplicativo de daemon será apresentado por um `IConfidentialClientApplication`

```CSharp
IConfidentialClientApplication app;
```

Aqui está o código para criar um aplicativo com um segredo do aplicativo:

```CSharp
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
           .WithClientSecret(config.ClientSecret)
           .WithAuthority(new Uri(config.Authority))
           .Build();
```

Aqui está o código para criar um aplicativo com um certificado:

```CSharp
X509Certificate2 certificate = ReadCertificate(config.CertificateName);
app = ConfidentialClientApplicationBuilder.Create(config.ClientId)
    .WithCertificate(certificate)
    .WithAuthority(new Uri(config.Authority))
    .Build();
```

### <a name="msalpython"></a>MSAL.Python

```Python
# Create a preferably long-lived app instance which maintains a token cache.

app = msal.ConfidentialClientApplication(
    config["client_id"], authority=config["authority"],
    client_credential=config["secret"],
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache

    )
```

### <a name="msaljava"></a>MSAL.Java

```Java
PrivateKey key = getPrivateKey();
X509Certificate publicCertificate = getPublicCertificate();

// create clientCredential with public and private key
IClientCredential credential = ClientCredentialFactory.create(key, publicCertificate);

ConfidentialClientApplication cca = ConfidentialClientApplication
  .builder(CLIENT_ID, credential)
  .authority(AUTHORITY_MICROSOFT)
  .build();
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Aplicativo daemon - aquisição de tokens para o aplicativo](./scenario-daemon-acquire-token.md)