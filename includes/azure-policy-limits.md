---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 05/30/2019
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 7907504401f4b47aafe6032ea895d9647e6c303c
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420765"
---
Há uma contagem máxima para cada tipo de objeto do Azure Policy. Uma entrada do _Escopo_ significa a assinatura ou o [grupo de gerenciamento](../articles/governance/management-groups/overview.md).

| Where | O que | Contagem máxima |
|---|---|---|
| Escopo | Definições de política | 250 |
| Escopo | Definições de iniciativa | 100 |
| Locatário | Definições de iniciativa | 1.000 |
| Escopo | Atribuições de iniciativa ou política | 100 |
| Definição de política | parâmetros | 20 |
| Definição de iniciativa | Políticas | 100 |
| Definição de iniciativa | parâmetros | 100 |
| Atribuições de iniciativa ou política | Exclusões (notScopes) | 400 |
| Regra de política | Condicionais aninhadas | 512 |
