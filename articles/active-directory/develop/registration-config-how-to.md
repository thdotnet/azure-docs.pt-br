---
title: Como selecionar permissões para uma API | Microsoft Docs
description: Como localizar os pontos de extremidade de autenticação para um aplicativo personalizado que você está desenvolvendo ou registrando com o Azure AD.
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
ms.openlocfilehash: dcfeec7e90f1c582a11889619811330bcf488bbf
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65545909"
---
# <a name="how-to-configure-endpoints"></a>Como configurar pontos de extremidade

Você pode encontrar os pontos de extremidade de autenticação para seu aplicativo no [Portal do Azure](https://portal.azure.com).

-   Navegue até o [Portal do Azure](https://portal.azure.com).

-   No painel de navegação esquerdo, clique em **Azure Active Directory**.

-   Clique em **Registros do Aplicativo** e escolha **Pontos de Extremidade**.

-   Isso abre a página **Pontos de Extremidade**, que lista todos os pontos de extremidade de autenticação de seu locatário.

-   Use o ponto de extremidade específico ao protocolo de autenticação que você está usando, em conjunto com a ID do aplicativo, para criar a solicitação de autenticação específica ao aplicativo.

## <a name="next-steps"></a>Próximas etapas
[Guia do desenvolvedor do Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)
