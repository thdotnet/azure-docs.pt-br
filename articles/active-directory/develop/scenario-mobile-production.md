---
title: Aplicativo móvel que chamadas às APIs da web (mover para a produção) – plataforma de identidade da Microsoft
description: Saiba como criar um aplicativo móvel que chamadas de web APIs (mover para a produção)
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5cc8b7c86495c2a60b07bef0a825e3872f787520
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550391"
---
# <a name="mobile-app-that-calls-web-apis---move-to-production"></a>Aplicativo móvel que chama o web APIs - passar para a produção

Este artigo fornece detalhes sobre como melhorar a qualidade e a confiabilidade do seu aplicativo antes de movê-lo para produção.

## <a name="handling-errors-in-mobile-applications"></a>Tratamento de erros em aplicativos móveis

Um número de condições de erro pode ocorrer em seu aplicativo no momento. Os cenários principais para lidar com são falhas silenciosas e fallbacks para interação. Outras condições que você deve considerar para a produção incluem situações não-rede, interrupções de serviço, requisitos de consentimento do administrador e outros casos específicos de cenário.

Cada biblioteca MSAL tem conteúdo de wiki e de código de exemplo que descreve como lidar com essas condições:

- [Wiki do Android MSAL](https://github.com/AzureAD/microsoft-authentication-library-for-android)
- [MSAL iOS Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki)
- [MSAL.NET Wiki](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki)

## <a name="mitigating-and-investigating-issues"></a>Redução de e para investigar problemas

Para diagnosticar problemas em seu aplicativo, ele ajuda a coletar dados. Para obter informações sobre os tipos de dados, você pode coletar, consulte os MSAL plataforma wikis.

- Os usuários podem pedir ajuda ao encontrar problemas. É uma prática recomendada capturar e temporariamente armazenar os logs e forneça um local em que os usuários podem carregá-los. A MSAL oferece extensões de registro em log para capturar informações detalhadas sobre a autenticação.
- Se ele estiver disponível, habilite a telemetria por meio da MSAL para reunir dados sobre como os usuários estão entrando seu aplicativo.

## <a name="next-steps"></a>Próximas etapas

[!INCLUDE [Move to production common steps](../../../includes/active-directory-develop-scenarios-production.md)]
