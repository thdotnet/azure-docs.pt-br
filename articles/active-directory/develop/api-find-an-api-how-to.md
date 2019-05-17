---
title: Como localizar uma API específica necessária para um aplicativo personalizado | Microsoft Docs
description: Como configurar as permissões que você precisa para acessar uma API em particular em seu aplicativo personalizado do Azure AD
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: ryanwi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4a017c13008288b26ddb11bf58be1966d652bbae
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540549"
---
# <a name="how-to-find-a-specific-api-needed-for-a-custom-developed-application"></a>Como localizar uma API específica necessária para um aplicativo personalizado

O acesso às APIs exigem configuração de escopos de acesso e funções. Se você quiser expor suas APIs web do aplicativo de recursos para aplicativos cliente, precisará configurar escopos de acesso e funções da API. Se desejar que um aplicativo cliente acesse uma API web, você precisará configurar permissões para acessar a API no registro de aplicativo.

## <a name="configuring-a-resource-application-to-expose-web-apis"></a>Configurando um aplicativo de recurso para expor APIs Web

Quando você expõe sua API web, a API é exibida na lista **Selecionar uma API** ao adicionar permissões a um registro de aplicativo. Para adicionar escopos de acesso, siga as etapas descritas em [Adicionando escopos de acesso ao seu aplicativo de recurso](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).

## <a name="configuring-a-client-application-to-access-web-apis"></a>Configurando um aplicativo cliente para acessar APIs Web

Quando você adiciona permissões em seu registro de aplicativo, você pode **Adicionar acesso à API** para APIs web expostas. Para acessar APIs web, siga as etapas descritas em [Adicionar credenciais ou permissões para acessar APIs web](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).

## <a name="next-steps"></a>Próximas etapas

-   [Integrando aplicativos com o Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications)

-   [Noções básicas sobre o manifesto de aplicativo do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-manifest)


