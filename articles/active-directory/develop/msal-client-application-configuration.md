---
title: Configuração de aplicativo do cliente (biblioteca de autenticação da Microsoft) | Azure
description: Saiba mais sobre as opções de configuração de cliente público e cliente confidencial aplicativos na autenticação biblioteca MSAL (Microsoft).
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
ms.date: 04/12/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b5e175a8cdd1622add90bd80df63303fe914ab9c
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66430817"
---
# <a name="application-configuration-options"></a>Opções de configuração de aplicativo

Em seu código, você inicializa um novo cliente público ou confidencial (ou o agente do usuário para a msal) o aplicativo para autenticar e obter tokens. Você pode definir várias opções de configuração quando você inicializa o aplicativo cliente na biblioteca de autenticação da Microsoft (MSAL). Essas opções se enquadram em dois grupos:

- Opções de registro, incluindo:
    - [Autoridade](#authority) (composta de provedor de identidade [instância](#cloud-instance) e entre [público-alvo](#application-audience) para o aplicativo e, possivelmente, a ID do Locatário).
    - [ID do cliente](#client-id).
    - [URI de redirecionamento](#redirect-uri).
    - [segredo do cliente](#client-secret) (para aplicativos de cliente confidencial).
- [Opções de log](#logging), incluindo o nível de log, controle de dados pessoais e o nome do componente usando a biblioteca.

## <a name="authority"></a>Authority
A autoridade é uma URL que indique um diretório que o MSAL pode solicitar tokens do. Autoridades comuns são:

- https://login.microsoftonline.com/&lt; locatário&gt;/, onde &lt;locatário&gt; é a ID de locatário do locatário do Azure Active Directory (Azure AD) ou um domínio associado a este locatário do Azure AD. Usado somente para entrada de usuários de uma organização específica.
- https://login.microsoftonline.com/common/. Usado para conectar os usuários com contas pessoais da Microsoft ou o trabalho e contas de estudante.
- https://login.microsoftonline.com/organizations/. Usado para conectar os usuários com contas corporativas e de estudante.
- https://login.microsoftonline.com/consumers/. Usado para conectar os usuários com apenas contas pessoais da Microsoft (anteriormente conhecidas como contas do Windows Live ID).

A configuração da autoridade precisa ser consistente com o que é declarado no portal de registro de aplicativo.

A URL de autoridade é composta de instância e o público-alvo.

A autoridade pode ser:
- Uma autoridade de nuvem do Azure AD.
- uma autoridade do Azure AD B2C. Ver [especificidades do B2C](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics).
- Uma autoridade de serviços de Federação do Active Directory (ADFS). Ver [dar suporte a ADFS](https://aka.ms/msal-net-adfs-support).

Autoridades de nuvem do Azure AD tem duas partes:
- o provedor de identidade *instância*
- Na entrada *público* para o aplicativo

A instância e o público-alvo podem ser concatenados e fornecidos como a URL de autoridade. Nas versões anteriores à MSAL 3 de MSAL.NET. *x*, você tinha que compõem a autoridade por conta própria, com base em nuvem, você quisesse de destino e o público entrar.  Este diagrama mostra como a URL de autoridade é composto por:

![Como a URL de autoridade é composta](media/msal-client-application-configuration/authority.png)

## <a name="cloud-instance"></a>Instância de nuvem
O *instância* é usado para especificar se seu aplicativo está se conectando a usuários de nuvem pública do Azure ou de nuvens nacionais. Usando a MSAL em seu código, você pode definir a instância de nuvem do Azure usando uma enumeração ou passando a URL para o [instância de nuvem nacional](authentication-national-cloud.md#azure-ad-authentication-endpoints) como o `Instance` membro (se souber).

MSAL.NET lançará uma exceção explícita se os dois `Instance` e `AzureCloudInstance` são especificados.

Se você não especificar uma instância, seu aplicativo se destinará a instância de nuvem pública do Azure (a instância do URL `https://login.onmicrosoftonline.com`).

## <a name="application-audience"></a>Público-alvo do aplicativo

O público entrar depende das necessidades de negócios para seu aplicativo:
- Se você for uma linha de desenvolvedor de negócios (LOB), você provavelmente produzirá um aplicativo de locatário único que será usado apenas em sua organização. Nesse caso, você precisa especificar a organização, por sua ID de locatário (ID da sua instância do AD do Azure) ou por um nome de domínio associado com a instância do AD do Azure.
- Se você for um ISV, você talvez queira conectar usuários com suas contas corporativas e de estudantes em qualquer organização ou em algumas organizações (multilocatário app). Mas você também poderá ter usuários entrar com suas contas pessoais da Microsoft.

### <a name="how-to-specify-the-audience-in-your-codeconfiguration"></a>Como especificar o público em seu código/configuração
Usando o MSAL em seu código, você especificar o público-alvo, usando um dos seguintes valores:
- A enumeração de público-alvo de autoridade do AD do Azure
- A ID do locatário, que pode ser:
  - Um GUID (ID da sua instância do AD do Azure), para aplicativos de locatário único
  - Um nome de domínio associado com sua instância do AD do Azure (e também para aplicativos de locatário único)
- Um desses espaços reservados como uma ID de locatário no lugar da enumeração de público-alvo de autoridade do Azure AD:
    - `organizations` para um aplicativo multilocatário
    - `consumers` para conectar usuários apenas com as respectivas contas pessoais
    - `common` para conectar os usuários com seu trabalho e contas de estudante ou suas contas pessoais da Microsoft

MSAL lançará uma exceção significativa se você especificar o público-alvo de autoridade do Azure AD e a ID de locatário.

Se você não especificar um público-alvo, seu aplicativo se destinará do Azure AD e contas pessoais da Microsoft como um público-alvo. (Ou seja, ele se comportará como se `common` foram especificadas.)

### <a name="effective-audience"></a>Público-alvo efetiva
O público-alvo em vigor para o seu aplicativo será o mínimo (se houver uma interseção) público definido em seu aplicativo e o público-alvo é especificado no registro de aplicativo. Na verdade, o [registros de aplicativo](https://aka.ms/appregistrations) experiência lhe permite especificar o público-alvo (os tipos de conta com suporte) para o aplicativo. Para saber mais, confira [Início Rápido: Registrar um aplicativo com a plataforma de identidade Microsoft](quickstart-register-app.md).

Atualmente, a única maneira de obter um aplicativo para conectar os usuários com apenas contas pessoais da Microsoft é configurar essas duas configurações:
- Defina o público de registro de aplicativo como `Work and school accounts and personal accounts`.
- Defina o público em seu código/configuração para `AadAuthorityAudience.PersonalMicrosoftAccount` (ou `TenantID` = "clientes").

## <a name="client-id"></a>ID do cliente
A ID do cliente é a ID de aplicativo exclusiva ao aplicativo (cliente) atribuída ao seu aplicativo pelo AD do Azure quando o aplicativo foi registrado.

## <a name="redirect-uri"></a>URI de redirecionamento
O redirecionamento de URI é o provedor de identidade enviará de volta para os tokens de segurança.

### <a name="redirect-uri-for-public-client-apps"></a>URI de redirecionamento para aplicativos de cliente público
Se você for um desenvolvedor de aplicativos de cliente público que está usando a MSAL:
- Você não precisa passar `RedirectUri` porque ele é calculado automaticamente pelo MSAL. Esse redirecionamento URI é definido como um destes valores, dependendo da plataforma:
   - `urn:ietf:wg:oauth:2.0:oob` para todas as plataformas Windows
   - `msal{ClientId}://auth` para Xamarin Android e iOS

- Você precisa configurar o redirecionamento de URI na [registros de aplicativo](https://aka.ms/appregistrations):

   ![URI de redirecionamento em registros de aplicativo](media/msal-client-application-configuration/redirect-uri.png)

Você pode substituir o URI de redirecionamento usando o `RedirectUri` propriedade (por exemplo, se você usar agentes). Aqui estão alguns exemplos de URIs de redirecionamento para esse cenário:

- `RedirectUriOnAndroid` = "msauth-5a434691-ccb2-4fd1-b97b-b64bcfbc03fc://com.microsoft.identity.client.sample";
- `RedirectUriOnIos` = $"msauth.{Bundle.ID}://auth";

Para obter detalhes, consulte o [documentação do Android e iOS](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Leveraging-the-broker-on-iOS).

### <a name="redirect-uri-for-confidential-client-apps"></a>URI de redirecionamento para aplicativos de cliente confidencial
Para aplicativos web, o URI de redirecionamento (ou URI de resposta) é o URI que o Azure AD usará para enviar o token de volta para o aplicativo. Isso pode ser a URL da web aplicativo/API Web se o aplicativo confidencial é um deles. O redirecionamento de URI deve ser registrado no registro do aplicativo. Esse registro é especialmente importante quando você implanta um aplicativo que inicialmente testou localmente. Em seguida, você precisa adicionar a URL de resposta do aplicativo implantado no portal de registro do aplicativo.

Para aplicativos de daemon, você não precisa especificar um URI de redirecionamento.

## <a name="client-secret"></a>Segredo do cliente
Esta opção especifica o segredo do cliente para o aplicativo cliente confidencial. Esse segredo (senha de aplicativo) é fornecido pelo portal de registro do aplicativo ou fornecido para o Azure AD durante o registro de aplicativo com PowerShell AzureAD, PowerShell AzureRM ou CLI do Azure.

## <a name="logging"></a>Registrando em log
Opções de configuração Habilitar registro em log e solução de problemas. Consulte a [registro em log](msal-logging.md) para obter detalhes sobre como usá-los.

## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre [criando uma instância de aplicativos cliente usando MSAL.NET](msal-net-initializing-client-applications.md).

Saiba mais sobre [criando uma instância de aplicativos cliente usando a msal](msal-js-initializing-client-applications.md).
