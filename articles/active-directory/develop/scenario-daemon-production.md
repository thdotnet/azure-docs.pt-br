---
title: Chamada de aplicativo de daemon às APIs da web (mover para a produção) – plataforma de identidade da Microsoft
description: Saiba como criar um aplicativo daemon que chamadas de web APIs (mover para a produção)
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
ms.openlocfilehash: 627dab0cb23800664c5fb5b3df9c61f5071d4b87
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545397"
---
# <a name="daemon-app-that-calls-web-apis---move-to-production"></a>Aplicativo daemon que chama web APIs - passar para a produção

Agora que você sabe como adquirir e usar um token para uma chamada de serviços, saiba como mover seu aplicativo para produção.

## <a name="deployment---case-of-multi-tenant-daemon-apps"></a>Implantação – caso de aplicativos do daemon de vários locatários

Se você for um ISV criando um aplicativo daemon que pode ser executados em vários locatários, você precisará ter certeza de que os administradores de locatário:

- Provisiona uma entidade de serviço para o aplicativo
- Concede o consentimento para o aplicativo

Você precisará explicar aos seus clientes como executar essas operações. Para obter mais informações, consulte [solicitar o consentimento para um locatário inteiro](v2-permissions-and-consent.md#requesting-consent-for-an-entire-tenant).

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]

## <a name="next-steps"></a>Próximas etapas

Aqui estão alguns links para saber mais:

### <a name="net"></a>.NET

- Se você ainda não tiver, tente o início rápido [adquirir um token e chamar a API do Microsoft Graph de um aplicativo de console usando a identidade do aplicativo](./quickstart-v2-netcore-daemon.md).
- Documentação de referência para:
  - Criando uma instância de [ConfidentialClientApplication](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.confidentialclientapplicationbuilder)
  - Chamar [AcquireTokenForClient](https://docs.microsoft.com/dotnet/api/microsoft.identity.client.acquiretokenforclientparameterbuilder)
- Outros exemplos/tutoriais:
  - [Microsoft-identity-platform-console-daemon](https://github.com/Azure-Samples/microsoft-identity-platform-console-daemon) apresenta um aplicativo de console de daemon do .NET Core simples que exibe os usuários de um locatário consultando o Microsoft Graph.

    ![topologia](media/scenario-daemon-app/daemon-app-sample.svg)

    O mesmo exemplo também ilustra a variação com certificados.

    ![topologia](media/scenario-daemon-app/daemon-app-sample-with-certificate.svg)

  - [Microsoft-Identity-Platform-ASPNET-WebApp-daemon](https://github.com/Azure-Samples/microsoft-identity-platform-aspnet-webapp-daemon) apresenta um aplicativo web ASP.NET MVC que sincroniza os dados do Microsoft Graph usando a identidade do aplicativo em vez de em nome do usuário. O exemplo também ilustra o processo de consentimento do administrador.

    ![topologia](media/scenario-daemon-app/damon-app-sample-web.svg)

### <a name="python"></a>Python

MSAL Python está atualmente em visualização pública. Para obter mais informações, consulte [exemplo de no repositório de credenciais de cliente de Python MSAL](https://github.com/AzureAD/azure-activedirectory-library-for-python/blob/dev/sample/client_credentials_sample.py).

### <a name="java"></a>Java

MSAL Python está atualmente em visualização pública. Para obter mais informações, consulte [exemplos no repositório de Java MSAL](https://github.com/AzureAD/azure-activedirectory-library-for-java/tree/dev/src/samples).