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
ms.openlocfilehash: 21f1fca1e8c5cf2b060cee41d1e474996f306f8e
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65787586"
---
| Resource | Limite padrão | Limite máximo |
| --- | --- | --- |
| Configurações de autoescala |100 por região e assinatura. | Mesmo que o padrão. |
| Alertas de métrica (clássico) |regras de alerta ativas 100 por assinatura. | Contate o suporte. |
| Alertas de métricas |regras de alerta ativas 100 por assinatura. | Contate o suporte. |
| Alertas do log de atividades | regras de alerta ativas 100 por assinatura. | Mesmo que o padrão. |
| Alertas de registro | 512 | Contate o suporte. |
| Grupos de ações |2.000 grupos de ação por assinatura. | Contate o suporte. |

**Limites da ação específica de um grupo**

| Resource | Limite padrão | Limite máximo |
| --- | --- | --- |
| Push do aplicativo do Azure | 10 ações de aplicativo do azure por grupo de ação. | Contate o suporte. |
| Email | 1.000 ações de email em um grupo de ação. Consulte também o [informações de limitação de taxa](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Contate o suporte. |
| ITSM | 10 ações de ITSM em um grupo de ação. | Contate o suporte. | 
| Aplicativo lógico | 10 ações do aplicativo lógico em um grupo de ação. | Contate o suporte. |
| Runbook | 10 ações de runbook em um grupo de ação. | Contate o suporte. |
| SMS | 10 ações de SMS em um grupo de ação. Consulte também o [informações de limitação de taxa](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Contate o suporte. |
| Voz | 10 ações de voz em um grupo de ação. Consulte também o [informações de limitação de taxa](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Contate o suporte. |
| Webhook | 10 ações de webhook em um grupo de ação.  Número máximo de chamadas de webhook é 1500 por minuto para cada assinatura. Outros limites estão disponíveis em [informações específicas da ação](../articles/azure-monitor/platform/action-groups.md#action-specific-information).  | Contate o suporte. |
