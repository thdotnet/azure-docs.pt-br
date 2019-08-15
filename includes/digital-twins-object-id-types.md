---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 08/12/2019
ms.author: v-adgera
ms.custom: include file
ms.openlocfilehash: 94baeb3d459b700cc95d88fb82f995957640aab6
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2019
ms.locfileid: "69012035"
---
O `objectIdType` (ou **tipo de identificador de objeto**) refere-se ao tipo de identidade que é dada a uma função. Além dos tipos `DeviceId` e `UserDefinedFunctionId`, os tipos de identificador de objeto correspondem às propriedades dos objetos do Azure Active Directory.

A tabela a seguir contém os tipos de identificador de objeto com suporte no Gêmeos Digitais do Azure:

| Tipo | Descrição |
| --- | --- |
| UserId | Atribui uma função a um usuário. |
| DeviceID | Atribui uma função a um dispositivo. |
| DomainName | Atribui uma função a um nome de domínio. Cada usuário com o nome de domínio especificado possui os direitos de acesso da função correspondente. |
| TenantId | Atribui uma função a um locatário. Cada usuário que pertence ao ID de locatário do Microsoft Azure Active Directory especificado tem os direitos de acesso da função correspondente. |
| ServicePrincipalId | Atribui uma função a uma ID de objeto de entidade de serviço. |
| UserDefinedFunctionId | Atribui uma função a uma UDF (função definida pelo usuário). |