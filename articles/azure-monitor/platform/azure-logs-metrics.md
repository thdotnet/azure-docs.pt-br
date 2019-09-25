---
title: Logs e métricas no Azure | Microsoft Docs
description: Visão geral dos logs de diagnóstico no Azure que fornecem dados avançados e frequentes sobre a operação de um recurso do Azure.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 3fe220506e074f881a16c1805d25fb4b39927488
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71262356"
---
# <a name="logs-and-metrics-in-azure"></a>Logs e métricas no Azure
Há diferentes [logs](data-platform-logs.md) e [métricas](data-platform-metrics.md)

O monitoramento de aplicativos e serviços no Azure pode ser separado no que está armazenado na [plataforma de dados do Azure](data-platform.md). 

Os logs e as métricas podem ser separados em duas categorias

- Plataforma-logs e métricas que são gerados automaticamente sem nenhuma configuração necessária além de simples 



| Camada | Logs da plataforma | Métricas de plataforma | Logs personalizados | Métricas personalizadas |
|:---|:---|:---|:---|:---|
| Aplicativo  | | | | |
| SO convidado     | Pulsação |  | Extensão de diagnóstico<br>Agente do Log Analytics | Extensão de diagnóstico |
| Resource     | [Logs de recursos](resource-logs-overview.md)<br>(específico para cada serviço) | [Métricas de recurso](metrics-supported.md)<br>(específico para cada serviço) | | [Métricas personalizadas](metrics-custom-overview.md) |
| Assinatura | [Log de atividade](activity-logs-overview.md) | | | |
| Locatário       | 

## <a name="next-steps"></a>Próximas etapas

* [Transmitir logs de diagnóstico de recurso os **Hubs de Eventos**](resource-logs-stream-event-hubs.md)
* [Alterar as configurações de diagnóstico do recurso usando a API REST do Azure Monitor](https://docs.microsoft.com/rest/api/monitor/)
* [Analisar logs do Armazenamento do Azure com o Azure Monitor](collect-azure-metrics-logs.md)
