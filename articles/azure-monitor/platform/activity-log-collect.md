---
title: Coletar e analisar logs de atividades do Azure no espaço de trabalho do Log Analytics | Microsoft Docs
description: Coletar o Log de atividades do Azure nos Logs do Azure Monitor e usar a solução de monitoramento para analisar e pesquisar o log de atividades do Azure em todas as suas assinaturas do Azure.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/19/2019
ms.author: bwren
ms.openlocfilehash: 5839fd40a128097e400f13acbe4fb6ef90c656b7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66248123"
---
# <a name="collect-and-analyze-azure-activity-logs-in-log-analytics-workspace-in-azure-monitor"></a>Coletar e analisar logs de atividades do Azure no espaço de trabalho do Log Analytics no Azure Monitor
O [Log de atividades do Azure](activity-logs-overview.md) fornece ideias sobre eventos de nível de assinatura que ocorreram na sua assinatura do Azure. Este artigo descreve como coletar o Log de atividades em um espaço de trabalho do Log Analytics e como usar o Log Analytics de atividade [solução de monitoramento](../insights/solutions.md), que fornece o log de consultas e exibições para analisar esses dados. 

Conectar-se o Log de atividades para um espaço de trabalho do Log Analytics fornece os seguintes benefícios:

- Consolide o Log de atividades de várias assinaturas do Azure em um único local para análise.
- Store entradas de Log de atividades por mais de 90 dias.
- Correlacione dados de Log de atividades com outros dados de monitoramento coletados pelo Azure Monitor.
- Use [registrar consultas](../log-query/log-query-overview.md) para realizar uma análise complexa e obter informações detalhadas sobre entradas de Log de atividades.

## <a name="connect-to-log-analytics-workspace"></a>Conectar-se ao espaço de trabalho do Log Analytics
Um Log de atividades pode ser conectado a apenas um espaço de trabalho, mas um único espaço de trabalho pode ser conectado ao Log de atividades para várias assinaturas no mesmo locatário do Azure. Para a coleção de vários locatários, consulte [coletar Logs de atividades do Azure em um espaço de trabalho do Log Analytics em assinaturas no Azure Active Directory diferentes locatários](activity-log-collect-tenants.md).

Use o procedimento a seguir para conectar o Log de atividades para seu espaço de trabalho do Log Analytics:

1. Dos **espaços de trabalho do Log Analytics** menu no portal do Azure, selecione o espaço de trabalho para coletar o Log de atividades.
1. No **fontes de dados de espaço de trabalho** seção do menu do espaço de trabalho, selecione **log de atividades do Azure**.
1. Clique na assinatura que você deseja se conectar.

    ![Workspaces](media/activity-log-export/workspaces.png)

1. Clique em **Connect** para conectar-se o log de atividades na assinatura para o espaço de trabalho selecionado. Se a assinatura já está conectada a outro espaço de trabalho, clique em **desconectar** primeiro para desconectá-lo.

    ![Conectar-se os espaços de trabalho](media/activity-log-export/connect-workspace.png)

## <a name="analyze-in-log-analytics-workspace"></a>Analisar no espaço de trabalho do Log Analytics
Quando você se conecta a um Log de atividades para um espaço de trabalho do Log Analytics, entradas serão gravadas no espaço de trabalho em uma tabela chamada **AzureActivity** que você pode recuperar com um [consulta de log](../log-query/log-query-overview.md). A estrutura dessa tabela varia dependendo de [categoria de entrada de log](activity-logs-overview.md#categories-in-the-activity-log). Ver [esquema de evento do Log de atividades do Azure](activity-log-schema.md) para obter uma descrição de cada categoria.

## <a name="activity-logs-analytics-monitoring-solution"></a>Solução de monitoramento de análise de Logs de atividade
A solução de monitoramento do Azure Log Analytics inclui várias consultas de log e exibições para analisar os registros de Log de atividades em seu espaço de trabalho do Log Analytics.

### <a name="install-the-solution"></a>Instalar a solução
Use o procedimento em [instalar uma solução de monitoramento](../insights/solutions.md#install-a-monitoring-solution) para instalar o **Log Analytics de atividade** solução. Não há nenhuma configuração adicional necessária.

### <a name="use-the-solution"></a>Usar a solução
Soluções de monitoramento são acessadas a partir de **Monitor** menu no portal do Azure. Selecione **mais** na **Insights** seção para abrir o **visão geral** página com os blocos de solução. O **Logs de atividades do Azure** bloco exibe uma contagem do número de **AzureActivity** registros em seu espaço de trabalho.

![Bloco de logs de atividade do Azure](media/collect-activity-logs/azure-activity-logs-tile.png)


Clique o **Logs de atividades do Azure** bloco para abrir o **Logs de atividades do Azure** modo de exibição. O modo de exibição inclui as partes de visualização na tabela a seguir. Cada parte mostra até 10 itens que correspondem aos critérios do que partes para o intervalo de tempo especificado. Você pode executar uma consulta de log que retorna todos os registros correspondentes clicando **ver tudo** na parte inferior da parte.

![Painel Logs de Atividade do Azure](media/collect-activity-logs/activity-log-dash.png)

| Parte de visualização | DESCRIÇÃO |
| --- | --- |
| Entradas de log de atividades do Azure | Mostra um gráfico de barras da entrada de Log de atividades do Azure superior dos totais de registro para o intervalo de datas selecionado e mostra uma lista dos principais 10 chamadores da atividade. Clique no gráfico de barras para executar uma pesquisa de logs para `AzureActivity`. Clique em um item do chamador para executar uma pesquisa de log retornando todas as entradas de Log de atividades para aquele item. |
| Logs de atividade por status | Mostra um gráfico de rosca para status do Log de atividades do Azure para o intervalo de datas selecionado e uma lista dos registros de status de dez principais. Clique no gráfico para executar uma consulta de log para `AzureActivity | summarize AggregatedValue = count() by ActivityStatus`. Clique em um item de status para executar uma pesquisa de logs que retorna todas as entradas de Log de atividades para esse registro de status. |
| Logs de atividade por recurso | Mostra o número total de recursos com os Logs de atividade e lista as principais contagens de dez recursos com o registro de cada recurso. Clique na área total para executar uma pesquisa de logs de `AzureActivity | summarize AggregatedValue = count() by Resource`, que mostra todos os recursos do Azure disponíveis para a solução. Clique em um recurso para executar uma consulta de log retornando todos os registros de atividade para esse recurso. |
| Logs de atividade por provedor de recursos | Mostra o número total de provedores de recursos que produzem os Logs de atividade e lista os dez principais. Clique na área total para executar uma consulta de log para `AzureActivity | summarize AggregatedValue = count() by ResourceProvider`, que mostra todos os provedores de recursos do Azure. Clique em um provedor de recursos para executar uma consulta de log retornando todos os registros de atividade para o provedor. |

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre o [Log de atividades](activity-logs-overview.md).
- Saiba mais sobre o [plataforma de dados do Azure Monitor](data-platform.md).
- Use [registrar consultas](../log-query/log-query-overview.md) para exibir informações detalhadas de seu Log de atividades.
