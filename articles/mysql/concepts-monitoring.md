---
title: Monitoramento no Banco de Dados do Azure para MySQL
description: Este artigo descreve as métricas de monitoramento e alertas para o Banco de Dados do Azure para MySQL, incluindo CPU, armazenamento e estatísticas de conexão.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/05/2019
ms.openlocfilehash: 0122f952e586d0535fc2e482c7b78266f8809272
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67062444"
---
# <a name="monitoring-in-azure-database-for-mysql"></a>Monitoramento no Banco de Dados do Azure para MySQL
Monitorar os dados dos seus servidores ajuda a solucionar problemas e otimizar sua carga de trabalho. O Banco de Dados do Azure para MySQL oferece várias métricas que fornecem insights sobre o comportamento do seu servidor.

## <a name="metrics"></a>metrics
Todas as métricas do Azure têm uma frequência de um minuto e cada uma delas fornece 30 dias de histórico. É possível configurar alertas nas métricas. Para obter diretrizes passo a passo, consulte [How to set up alerts](howto-alert-on-metric.md) (Como configurar alertas). Outras tarefas incluem a configuração de ações automatizadas, execução de análises avançadas e arquivamento de histórico. Para obter mais informações, consulte a [Visão geral das métricas no Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Lista de métricas
Essas métricas estão disponíveis para o Banco de Dados do Azure para MySQL:

|Métrica|Nome de exibição da métrica|Unidade|DESCRIÇÃO|
|---|---|---|---|
|cpu_percent|Porcentagem de CPU|Porcentagem|O percentual de CPU em uso.|
|memory_percent|Porcentagem de memória|Porcentagem|O percentual de memória em uso.|
|io_consumption_percent|Porcentagem de E/S|Porcentagem|O percentual de E/S em uso.|
|storage_percent|Porcentagem de armazenamento|Porcentagem|O percentual de armazenamento usado fora do máximo do servidor.|
|storage_used|Armazenamento usado|Bytes|A quantidade de armazenamento em uso. O armazenamento usado pelo serviço pode incluir os arquivos de banco de dados, os logs de transação e os logs do servidor.|
|serverlog_storage_percent|Porcentagem de armazenamento do Log do Servidor|Porcentagem|A porcentagem de armazenamento de log do servidor usada fora do armazenamento de log máximo do servidor.|
|serverlog_storage_usage|Armazenamento do Log do Servidor usado|Bytes|A quantidade de armazenamento de log do servidor em uso.|
|serverlog_storage_limit|Limite de armazenamento do Log do Servidor|Bytes|O armazenamento de log do servidor de máximo para esse servidor.|
|storage_limit|Limite de armazenamento|Bytes|O armazenamento máximo para esse servidor.|
|active_connections|Conexões ativas|Contagem|O número de conexões ativas com o servidor.|
|connections_failed|Conexões com falha|Count|O número de conexões com falha com o servidor.|
|seconds_behind_master|Retardo de replicação em segundos|Contagem|O número de segundos que o servidor de réplica está atrasando contra o servidor mestre.|
|network_bytes_egress|Saída da rede|Bytes|Rede-Out em conexões ativas.|
|network_bytes_ingress|Entrada na rede|Bytes|Entrada de rede em conexões ativas.|
|backup_storage_used|Backup do Microsoft Azure|Bytes|A quantidade de armazenamento de backup usado.|

## <a name="server-logs"></a>Logs do servidor
Você pode habilitar consultas lentas e registro em log no servidor de auditoria. Esses logs também estão disponíveis por meio dos Logs de diagnóstico do Azure na conta de armazenamento, Hubs de eventos e logs do Azure Monitor. Para saber mais sobre o registro em log, visite o [logs de auditoria](concepts-audit-logs.md) e [lenta dos logs de consulta](concepts-server-logs.md) artigos.

## <a name="query-store"></a>Repositório de Consultas
[Repositório de Consultas](concepts-query-store.md) é uma versão prévia pública do recurso que mantém o controle do desempenho da consulta ao longo do tempo incluindo eventos de espera e estatísticas de tempo de execução de consulta. O recurso de persiste informações de desempenho de tempo de execução de consulta na **mysql** esquema. Você pode controlar a coleta e o armazenamento de dados por meio de vários botões de configuração.

## <a name="query-performance-insight"></a>Análise de Desempenho de Consultas
[Análise de Desempenho de Consultas](concepts-query-performance-insight.md) funciona em conjunto com o Repositório de Consultas para fornecer visualizações acessíveis do portal do Azure. Esses gráficos permitem que você identifique as principais consultas que afetam o desempenho. Análise de desempenho de consulta está em visualização pública e está acessível a **desempenho inteligente** seção do seu banco de dados do Azure para a página do portal do servidor MySQL.

## <a name="performance-recommendations"></a>Recomendações de desempenho
O recurso [Recomendações de Desempenho](concepts-performance-recommendations.md) identifica as oportunidades de melhorar o desempenho da carga de trabalho. A versão prévia pública das Recomendações de Desempenho fornece recomendações para a criação de novos índices que têm o potencial de melhorar o desempenho de suas cargas de trabalho. Para produzir recomendações de índice, o recurso leva em consideração várias características do banco de dados, inclusive seu esquema e a carga de trabalho, conforme relatado pelo Repositório de Consultas. Depois de implementar qualquer recomendação de desempenho, os clientes devem testar o desempenho para avaliar o impacto dessas alterações.

## <a name="next-steps"></a>Próximas etapas
- Consulte [Como configurar alertas](howto-alert-on-metric.md) para obter orientação sobre como criar um alerta em uma métrica.
- Para obter mais informações sobre como acessar e exportar métricas usando o Portal do Azure, a API REST ou a CLI, consulte a [Visão geral das métricas no Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
- Leia nosso blog sobre [práticas recomendadas para monitorar seu servidor](https://azure.microsoft.com/blog/best-practices-for-alerting-on-metrics-with-azure-database-for-mysql-monitoring/).
