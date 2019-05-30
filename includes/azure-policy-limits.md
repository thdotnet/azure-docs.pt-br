---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: azure-policy
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 09/18/2018
ms.author: dacoulte
ms.custom: include file
ms.openlocfilehash: 57cec39bde460c6079091490acf541761c61e003
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66119294"
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
| Atribuições de iniciativa ou política | Exclusões (notScopes) | 250 |
| Regra de política | Condicionais aninhadas | 512 |
