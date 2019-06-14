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
ms.openlocfilehash: c57feb33967732481d78e0ddaba5e90f4f82f327
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65544434"
---
# <a name="xamarin-android-specific-considerations-with-msalnet"></a>Considerações sobre o Xamarin Android específicos com MSAL.NET
Este artigo discute considerações específicas ao usar o navegador do sistema no Xamarin Android com a biblioteca de autenticação da Microsoft para .NET (MSAL.NET).

Começando com o MSAL.NET 2.4.0-preview, MSAL.NET dá suporte a navegadores diferentes do Chrome e não requer mais Chrome ser instalado no dispositivo Android para autenticação.

É recomendável que você use navegadores que oferecem suporte a guias personalizadas, como estes:

| Navegadores com suporte de guias personalizadas | Nome do Pacote |
|------| ------- |
|Chrome | com.android.chrome|
|Microsoft Edge | com.microsoft.emmx|
|Firefox | org.mozilla.firefox|
|Ecosia | com.ecosia.android|
|Kiwi | com.kiwibrowser.browser|
|Corajoso | com.brave.browser|

Além de navegadores com suporte de guias personalizadas, com base em nossos testes, alguns navegadores que não dão suporte a guias personalizadas também funcionará para autenticação: Opera, Opera Mini InBrowser e Maxthon. Para obter mais informações, leia [tabela de resultados do teste](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/Android-system-browser#devices-and-browsers-tested).

## <a name="known-issues"></a>Problemas conhecidos

- Se o usuário não tiver nenhum navegador habilitado no dispositivo, MSAL.NET lançará um `AndroidActivityNotFound` exceção. 
  - **Atenuação**: Informar ao usuário que eles devem habilitar um navegador (preferencialmente um com o suporte de guias personalizadas) em seu dispositivo.

- Se a autenticação falhar (por exemplo autenticação é iniciado com DuckDuckGo), MSAL.NET retornará um `AuthenticationCanceled MsalClientException`. 
  - **Raiz do problema**: Um navegador com suporte de guias personalizadas não foi habilitado no dispositivo. Autenticação é iniciado com um navegador alternativo, que não conseguiu concluir a autenticação. 
  - **Atenuação**: Informar ao usuário que eles devem instalar um navegador (preferencialmente um com o suporte da guia personalizada) em seu dispositivo.

## <a name="devices-and-browsers-tested"></a>Dispositivos e navegadores testados
A tabela a seguir lista os dispositivos e navegadores que foram testados.

| | Navegador&ast;     |  Result  | 
| ------------- |:-------------:|:-----:|
| Huawei um + | Chrome&ast; | Aprovado|
| Huawei um + | Edge&ast; | Aprovado|
| Huawei um + | Firefox&ast; | Aprovado|
| Huawei um + | Brave&ast; | Aprovado|
| Um + | Ecosia&ast; | Aprovado|
| Um + | Kiwi&ast; | Aprovado|
| Huawei um + | Opera | Aprovado|
| Huawei | OperaMini | Aprovado|
| Huawei um + | InBrowser | Aprovado|
| Um + | Maxthon | Aprovado|
| Huawei um + | DuckDuckGo | Autenticação de usuário cancelada|
| Huawei um + | Navegador UC | Autenticação de usuário cancelada|
| Um + | Dolphin | Autenticação de usuário cancelada|
| Um + | Navegador de CM | Autenticação de usuário cancelada|
| Huawei um + | Nenhuma instalada | AndroidActivityNotFound ex|

&ast; Dá suporte a guias personalizadas

## <a name="next-steps"></a>Próximas etapas
Para o código de trechos de código e informações adicionais sobre como usar o navegador de sistema com o Xamarin Android, leia esta [guia](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/MSAL.NET-uses-web-browser#choosing-between-embedded-web-browser-or-system-browser-on-xamarinandroid).  