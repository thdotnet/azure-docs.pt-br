---
title: Monitoramento de locais de dados no Azure Monitor | Microsoft Docs
description: Descreve os diferentes locais em que os dados de monitoramento é armazenado no Azure, incluindo a plataforma de dados do Azure Monitor.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/21/2019
ms.author: bwren
ms.openlocfilehash: 61f34277a03deb312e93920e3bd76ce5297f020f
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357539"
---
# <a name="monitoring-data-locations-in-azure-monitor"></a>Monitoramento de locais de dados no Azure Monitor

O Azure Monitor se baseia em uma [plataforma de dados](data-platform.md) de [Logs](data-platform-logs.md) e [métricas](data-platform-metrics.md) conforme descrito na [plataforma de dados do Azure Monitor](data-platform.md). Dados de monitoramento de recursos do Azure podem ser gravados para outros locais no entanto, qualquer um, antes de serem copiados para o Azure Monitor ou para dar suporte a cenários adicionais. 

## <a name="monitoring-data-locations"></a>Locais de dados de monitoramento

A tabela a seguir identifica os diferentes locais em que dados de monitoramento no Azure é enviado e os diferentes métodos para acessá-lo.

| Local padrão | DESCRIÇÃO | Métodos de acesso |
|:---|:---|:---|:--|
| Métricas do Azure Monitor | Banco de dados de série temporal que é otimizado para análise de dados de carimbo de data / hora. | [Metrics Explorer](metrics-getting-started.md)<br>[API de métricas do Azure Monitor](/rest/api/monitor/metrics) |
| Logs do Azure Monitor    | Espaço de trabalho de análise que se baseia no Data Explorer do Azure que fornece uma linguagem de consulta poderosa análise avançada e de mecanismo de log. | [Log Analytics](../log-query/portals.md)<br>[API de análise de log](https://dev.loganalytics.io/)<br>[API do Application Insights](https://dev.applicationinsights.io/reference/get-query) |
| Log de atividades | Dados do log de atividades são mais útil quando enviado para Logs do Azure Monitor para analisá-los com outros dados, mas também são coletado por conta própria para que ela pode ser exibida diretamente no portal do Azure. | [Portal do Azure](activity-log-view.md#azure-portal)<br>[API de Eventos do Azure Monitor](/rest/api/monitor/eventcategories) |
| Armazenamento do Azure | Algumas fontes de dados gravará diretamente no armazenamento do Azure e requer configuração para mover dados para Logs. Você também pode enviar dados para o armazenamento do Azure para arquivar e para a integração com sistemas externos.  | [Análise de Armazenamento](/rest/api/storageservices/storage-analytics)<br>[Gerenciador de Servidores](/visualstudio/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage)<br>[Gerenciador de Armazenamento](/visualstudio/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) |
| Hubs de Eventos | Envie dados para Hubs de eventos do Azure para transmiti-lo para outros locais. | [Captura para o armazenamento](../../event-hubs/event-hubs-capture-overview.md)  |
| Azure Monitor para VMs | O Azure Monitor para VMs armazena dados de integridade da carga de trabalho em um local personalizado que é usado pela sua experiência de monitoramento no portal do Azure. | [Portal do Azure](../insights/vminsights-overview.md)<br>[API REST do monitor de carga de trabalho](https://docs.microsoft.com/rest/api/monitor/microsoft.workloadmonitor/components)<br>[O Azure Resource health API REST](https://docs.microsoft.com/rest/api/resourcehealth/)  |
| Alertas | Alertas criados pelo Azure Monitor. | [Portal do Azure](alerts-managing-alert-instances.md)<br>[API REST de gerenciamento de alertas](https://docs.microsoft.com/rest/api/monitor/alertsmanagement/alerts) |



## <a name="next-steps"></a>Próximas etapas

- Consulte as fontes diferentes de [dados de monitoramento coletados pelo Azure Monitor](data-sources.md).
- Saiba mais sobre o [tipos de dados de monitoramento coletados pelo Azure Monitor](data-platform.md) e como exibir e analisar esses dados.
