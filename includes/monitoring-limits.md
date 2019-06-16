---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 050d3314345e64e3d69a07367a0e9acc318fa106
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66271575"
---
| Resource | Limite padrão | Limite máximo |
| --- | --- | --- |
| Configurações de autoescala |100 por região e assinatura. | Mesmo que o padrão. |
| Alertas de métrica (clássico) |regras de alerta ativas 100 por assinatura. | Contate o suporte. |
| Alertas de métricas |1000 regras de alerta ativas por assinatura (em nuvens públicas) e 100 regras de alerta ativas por assinatura no Azure China e Azure governamental. | Contate o suporte. |
| Alertas do log de atividades | regras de alerta ativas 100 por assinatura. | Mesmo que o padrão. |
| Alertas de registro | 512 | Contate o suporte. |
| Grupos de ação |2\.000 grupos de ação por assinatura. | Contate o suporte. |

**Limites da ação específica de um grupo**

| Resource | Limite padrão | Limite máximo |
| --- | --- | --- |
| Push de aplicativo do Azure | 10 ações de aplicativo do azure por grupo de ação. | Contate o suporte. |
| Email | 1\.000 ações de email em um grupo de ação. Consulte também o [informações de limitação de taxa](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Contate o suporte. |
| ITSM | 10 ações de ITSM em um grupo de ação. | Contate o suporte. | 
| Aplicativo lógico | 10 ações do aplicativo lógico em um grupo de ação. | Contate o suporte. |
| Runbook | 10 ações de runbook em um grupo de ação. | Contate o suporte. |
| sms | 10 ações de SMS em um grupo de ação. Consulte também o [informações de limitação de taxa](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Contate o suporte. |
| Voz | 10 ações de voz em um grupo de ação. Consulte também o [informações de limitação de taxa](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Contate o suporte. |
| webhook | 10 ações de webhook em um grupo de ação.  Número máximo de chamadas de webhook é 1500 por minuto para cada assinatura. Outros limites estão disponíveis em [informações específicas da ação](../articles/azure-monitor/platform/action-groups.md#action-specific-information).  | Contate o suporte. |
