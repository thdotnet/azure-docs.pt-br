---
title: Métricas compatíveis por tipo de recurso do Azure Monitor
description: Lista de métricas disponíveis para cada tipo de recurso com o Azure Monitor.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 05/20/2019
ms.author: ancav
ms.subservice: metrics
ms.openlocfilehash: dbbe5a667b8d467b416e4a4a571d8d3599ec45b6
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051811"
---
# <a name="supported-metrics-with-azure-monitor"></a>Métricas compatíveis com o Azure Monitor

O Azure Monitor fornece várias maneiras de interagir com as métricas, incluindo a criação de gráficos para os mesmos no portal, acessá-las por meio da API REST ou consultá-las usando o PowerShell ou a CLI. Abaixo está uma lista completa de todas as métricas atualmente disponíveis do pipeline de métrica do Azure Monitor. Outras métricas podem estar disponíveis no portal ou usando as APIs herdadas. Essa lista abaixo inclui apenas as métricas disponíveis usando o pipeline de métrica consolidado do Azure Monitor. Para consultar e acessar essas métricas, use [2018-01-01 api-version](https://docs.microsoft.com/rest/api/monitor/metricdefinitions)

> [!NOTE]
> Atualmente, não há suporte para o envio da métrica multidimensional por meio das configurações de diagnóstico. As métricas com dimensões são exportadas como métricas dimensionais simples, agregadas nos valores da dimensão.
>
> *Por exemplo*: A métrica “Mensagens de Entrada” em um Hub de Eventos pode ser explorada e mapeada por nível da fila. No entanto, quando exportada por meio das configurações de diagnóstico, a métrica será representada como todas as mensagens de entrada em todas as filas no Hub de Eventos.
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|qpu_metric|QPU|Count|Average|QPU. Intervalo de 0 a 100 para S1, 0 a 200 para S2 e 0 a 400 para S4|ServerResourceType|
|memory_metric|Memória|Bytes|Average|Memória. Intervalo de 0 a 25 GB para S1, 0 a 50 GB para S2 e 0 a 100 GB para S4|ServerResourceType|
|TotalConnectionRequests|Solicitações de conexão totais|Contagem|Average|Solicitações de conexão totais. Estas são chegadas.|ServerResourceType|
|SuccessfullConnectionsPerSec|Conexões bem-sucedidas por segundo|CountPerSecond|Average|Taxa de conclusões de conexão bem-sucedidas.|ServerResourceType|
|TotalConnectionFailures|Falhas de conexão totais|Count|Average|Total de falhas em tentativas de conexão.|ServerResourceType|
|CurrentUserSessions|Sessões de usuário atuais|Count|Average|Número atual de sessões de usuário estabelecidas.|ServerResourceType|
|QueryPoolBusyThreads|Threads ocupados do pool de consulta|Count|Average|Número de threads ocupados no pool de threads de consulta.|ServerResourceType|
|CommandPoolJobQueueLength|Comprimento da fila de trabalho do pool de comando|Contagem|Average|Número de trabalhos na fila do pool de threads de comando.|ServerResourceType|
|ProcessingPoolJobQueueLength|Comprimento da fila de trabalho do pool de processamento|Contagem|Average|Número de trabalhos não de E/S na fila do pool de threads de processamento.|ServerResourceType|
|CurrentConnections|Conexão: Conexões atuais|Count|Average|Número atual de conexões de cliente estabelecidas.|ServerResourceType|
|CleanerCurrentPrice|Memória: Preço atual do limpador|Contagem|Average|Preço atual da memória, $/byte/tempo, normalizado em 1000.|ServerResourceType|
|CleanerMemoryShrinkable|Memória: Memória do limpador reduzível|Bytes|Average|Quantidade de memória, em bytes, sujeita a eliminação pelo limpador na tela de fundo.|ServerResourceType|
|CleanerMemoryNonshrinkable|Memória: Memória do limpador não reduzível|Bytes|Average|Quantidade de memória, em bytes, não sujeita a eliminação pelo limpador na tela de fundo.|ServerResourceType|
|MemoryUsage|Memória: Uso de Memória|Bytes|Average|Uso de memória do processo do servidor, como usado no cálculo de preço de memória do limpador. Igual ao contador Processo\PrivateBytes mais o tamanho dos dados mapeados em memória, ignorando qualquer memória mapeada ou alocada pelo mecanismo de análise de memória xVelocity (VertiPaq), além do limite de memória do mecanismo xVelocity.|ServerResourceType|
|MemoryLimitHard|Memória: Limite de memória física|Bytes|Average|Limite de memória física, do arquivo de configuração.|ServerResourceType|
|MemoryLimitHigh|Memória: Limite de memória superior|Bytes|Average|Limite de memória superior, do arquivo de configuração.|ServerResourceType|
|MemoryLimitLow|Memória: Limite de memória inferior|Bytes|Average|Limite de memória inferior, do arquivo de configuração.|ServerResourceType|
|MemoryLimitVertiPaq|Memória: Limite de memória VertiPaq|Bytes|Average|Limite na memória, do arquivo de configuração.|ServerResourceType|
|Cota|Memória: Cota|Bytes|Average|Cota de memória atual, em bytes. A cota de memória também é conhecida como uma reserva de memória ou concessão de memória.|ServerResourceType|
|QuotaBlocked|Memória: Cota bloqueada|Contagem|Average|Número atual de solicitações de cota bloqueadas até que outras cotas de memória sejam liberadas.|ServerResourceType|
|VertiPaqNonpaged|Memória: VertiPaq não paginado|Bytes|Average|Bytes de memória bloqueada no conjunto de trabalho para uso pelo mecanismo na memória.|ServerResourceType|
|VertiPaqPaged|Memória: VertiPaq paginado|Bytes|Average|Bytes de memória paginada em uso para dados na memória.|ServerResourceType|
|RowsReadPerSec|Processamento: Linhas lidas por segundo|CountPerSecond|Average|Taxa de linhas lidas de todos os bancos de dados relacionais.|ServerResourceType|
|RowsConvertedPerSec|Processamento: Linhas convertidas por segundo|CountPerSecond|Average|Taxa de linhas convertidas durante o processamento.|ServerResourceType|
|RowsWrittenPerSec|Processamento: Linhas gravadas por segundo|CountPerSecond|Average|Taxa de linhas gravadas durante o processamento.|ServerResourceType|
|CommandPoolBusyThreads|Threads: Threads ocupados do pool comando|Contagem|Average|Número de threads ocupados no pool de threads de comando.|ServerResourceType|
|CommandPoolIdleThreads|Threads: Threads ociosos do pool comando|Contagem|Average|Número de threads ociosos no pool de threads de comando.|ServerResourceType|
|LongParsingBusyThreads|Threads: Threads ocupados de análise longa|Contagem|Average|Número de threads ocupados no pool de threads de análise longa.|ServerResourceType|
|LongParsingIdleThreads|Threads: Threads ociosos de análise longa|Contagem|Average|Número de threads ociosos no pool de threads de análise longa.|ServerResourceType|
|LongParsingJobQueueLength|Threads: Tamanho da fila de trabalhos de análise longa|Contagem|Average|Número de trabalhos na fila do pool de threads de análise longa.|ServerResourceType|
|ProcessingPoolBusyIOJobThreads|Threads: Threads de trabalho de E/S ocupados do pool de processamento|Count|Average|Número de threads que executam trabalhos de E/S no pool de threads de processamento.|ServerResourceType|
|ProcessingPoolBusyNonIOThreads|Threads: Threads de trabalho não E/S ocupados do pool de processamento|Count|Average|Número de threads que executam trabalhos não E/S no pool de threads de processamento.|ServerResourceType|
|ProcessingPoolIOJobQueueLength|Threads: Comprimento da fila de trabalho de E/S do pool de processamento|Contagem|Average|Número de trabalhos de E/S na fila do pool de threads de processamento.|ServerResourceType|
|ProcessingPoolIdleIOJobThreads|Threads: Threads de trabalho de E/S ociosos do pool de processamento|Contagem|Average|Número de threads ociosos para trabalhos de E/S no pool de threads de processamento.|ServerResourceType|
|ProcessingPoolIdleNonIOThreads|Threads: Threads de trabalho não E/S ociosos do pool de processamento|Contagem|Average|Número de threads ociosos no pool de threads de processamento dedicado a trabalhos não E/S.|ServerResourceType|
|QueryPoolIdleThreads|Threads: Threads ociosos do pool de consultas|Count|Average|Número de threads ociosos para trabalhos de E/S no pool de threads de processamento.|ServerResourceType|
|QueryPoolJobQueueLength|Threads: Tamanho da fila de trabalhos do pool consultas|Count|Average|Número de trabalhos na fila do pool de threads de consulta.|ServerResourceType|
|ShortParsingBusyThreads|Threads: Threads ocupados de análise resumida|Contagem|Average|Número de threads ocupados no pool de threads de análise resumida.|ServerResourceType|
|ShortParsingIdleThreads|Threads: Threads ociosos de análise resumida|Count|Average|Número de threads ociosos no pool de threads de análise resumida.|ServerResourceType|
|ShortParsingJobQueueLength|Threads: Tamanho da fila de trabalhos de análise resumida|Count|Average|Número de trabalhos na fila do pool de threads de análise resumida.|ServerResourceType|
|memory_thrashing_metric|Sobrecarga de memória|Percent|Average|Sobrecarga de memória média.|ServerResourceType|
|mashup_engine_qpu_metric|QPU do mecanismo M|Count|Average|Uso de QPU por processos de mecanismo de mashup|ServerResourceType|
|mashup_engine_memory_metric|Memória do mecanismo M|Bytes|Average|Uso de memória pelos processos do mecanismo de mashup|ServerResourceType|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|Solicitações|Solicitações|Count|Total|O número total de solicitações de gateway em um determinado período. Ele pode ser dividido por várias dimensões para ajudá-lo a diagnosticar problemas. |Local, BackendResponseCode, LastErrorReason, GatewayResponseCode|
|TotalRequests|Total de Solicitações de Gateway|Count|Total|O número total de solicitações de gateway em um determinado período. Essa métrica foi preterida, é recomendável usar a `Requests` nova métrica. |Local, Nome do host|
|SuccessfulRequests|Solicitações de Gateway com Êxito|Contagem|Total|O número total de solicitações de gateway bem-sucedidas em um determinado período. Essa métrica foi preterida, é recomendável usar a `Requests` nova métrica.|Local, Nome do host|
|UnauthorizedRequests|Solicitações de Gateway não autorizado|Contagem|Total| O número total de solicitações de gateway não autorizadas em um determinado período. Essa métrica foi preterida, é recomendável usar a `Requests` nova métrica.|Local, Nome do host|
|FailedRequests|Solicitações de Gateway com Falha|Contagem|Total|O número total de solicitações de gateway com falha em um determinado período. Essa métrica foi preterida, é recomendável usar a `Requests` nova métrica.|Local, Nome do host|
|OtherRequests|Outras solicitações de gateway|Count|Total|O número total de solicitações de gateway em um determinado período que não se enquadram nas categorias bem-sucedidas, não autorizadas ou com falha. Essa métrica foi preterida, é recomendável usar a `Requests` nova métrica. |Local, Nome do host|
|Duração|Duração total de solicitações de gateway|Milissegundos|Average|O tempo entre o gerenciamento de API recebe uma solicitação de um cliente e quando ele retorna uma resposta ao cliente.|Local, Nome do host|
|Capacity|Capacity|Percent|Average|Indicador de carga em uma instância de gerenciamento de API para tomar decisões informadas sobre a possibilidade de dimensionar a instância para acomodar mais carga.|Location|
|EventHubTotalEvents|Total de eventos do EventHub|Count|Total|O número total de eventos enviados ao EventHub do gerenciamento de API em um determinado período.|Location|
|EventHubSuccessfulEvents|Eventos de EventHub bem-sucedidos|Contagem|Total|O número total de eventos de EventHub bem-sucedidos em um determinado período.|Location|
|EventHubTotalFailedEvents|Eventos de EventHub com falha|Count|Total|O número total de eventos de EventHub com falha em um determinado período.|Location|
|EventHubRejectedEvents|Eventos de EventHub rejeitados|Count|Total|O número total de eventos do EventHub rejeitados (configuração incorreta ou não autorizado) em um determinado período.|Location|
|EventHubThrottledEvents|Eventos de EventHub limitados|Count|Total|O número total de eventos de EventHub limitado em um determinado período.|Location|
|EventHubTimedoutEvents|Eventos do EventHub com tempo limite excedido|Contagem|Total|O número total de eventos de EventHub expirados em um determinado período.|Location|
|EventHubDroppedEvents|Eventos do EventHub ignorados|Count|Total|O número total de eventos ignorados devido ao limite de tamanho de fila atingido em um determinado período.|Location|
|EventHubTotalBytesSent|Tamanho dos eventos do EventHub|Bytes|Total|O tamanho total dos eventos do EventHub em bytes em um determinado período.|Location|


## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|TotalJob|Total de trabalhos|Count|Total|O número total de trabalhos|Runbook, Status|
|TotalUpdateDeploymentRuns|Total de execuções de implantação de atualização|Contagem|Total|Total de execuções de implantação de atualização de software|SoftwareUpdateConfigurationName, Status|
|TotalUpdateDeploymentMachineRuns|Total de execuções do computador de implantação de atualização|Contagem|Total|O computador de implantação de atualização de software total é executado em uma execução de implantação de atualização de software|SoftwareUpdateConfigurationName, Status, TargetComputer, SoftwareUpdateConfigurationRunId|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|CoreCount|Contagem de núcleos dedicados|Contagem|Total|Número total de núcleos dedicados na conta do lote|Sem dimensões|
|TotalNodeCount|Contagem de nós dedicados|Contagem|Total|Número total de nós dedicados na conta do lote|Sem dimensões|
|LowPriorityCoreCount|Contagem de núcleos LowPriority|Contagem|Total|Número total de núcleos de baixa prioridade na conta do lote|Sem dimensões|
|TotalLowPriorityNodeCount|Contagem de nós de baixa prioridade|Count|Total|Número total de nós de baixa prioridade na conta do lote|Sem dimensões|
|CreatingNodeCount|Criação de contagem de nós|Count|Total|Número de nós sendo criados|Sem dimensões|
|StartingNodeCount|Contagem inicial dos nós|Count|Total|Número de nós iniciais|Sem dimensões|
|WaitingForStartTaskNodeCount|Contagem de nós para tarefa de inicialização em espera|Contagem|Total|Número de nós aguardando a conclusão da Tarefa de Inicialização|Sem dimensões|
|StartTaskFailedNodeCount|Falha na contagem de nós para a tarefa de inicialização|Count|Total|Número de nós com falha na tarefa de inicialização|Sem dimensões|
|IdleNodeCount|Contagem de nós ociosos|Count|Total|Número de nós ociosos|Sem dimensões|
|OfflineNodeCount|Contagem de nós off-line|Count|Total|Número de nós off-line|Sem dimensões|
|RebootingNodeCount|Contagem de nós de reinicialização|Count|Total|Número de nós de reinicialização|Sem dimensões|
|ReimagingNodeCount|Contagem de nós refazendo a imagem|Count|Total|Número de nós refazendo a imagem|Sem dimensões|
|RunningNodeCount|Contagem de nós em execução|Count|Total|Número de nós em execução|Sem dimensões|
|LeavingPoolNodeCount|Contagem de nós saindo do pool|Count|Total|Número de nós saindo do pool|Sem dimensões|
|UnusableNodeCount|Contagem de nós inutilizáveis|Count|Total|Número de nós inutilizáveis|Sem dimensões|
|PreemptedNodeCount|Contagem de nós com preempção|Count|Total|Número de nós com preempção|Sem dimensões|
|TaskStartEvent|Eventos da tarefa de inicialização|Count|Total|Número total de tarefas que iniciaram|Sem dimensões|
|TaskCompleteEvent|Eventos de conclusão de tarefa|Count|Total|Número total de tarefas concluídas|Sem dimensões|
|TaskFailEvent|Eventos de falha de tarefa|Count|Total|Número total de tarefas que foram concluídas em um estado com falha|Sem dimensões|
|PoolCreateEvent|Eventos de criação de pool|Count|Total|Número total de pools criados|Sem dimensões|
|PoolResizeStartEvent|Eventos de início de redimensionamento de pool|Count|Total|Número total de tarefas de redimensionamento de pool que iniciaram|Sem dimensões|
|PoolResizeCompleteEvent|Eventos de conclusão de redimensionamento de pool|Count|Total|Número total de redimensionamentos de pool concluídos|Sem dimensões|
|PoolDeleteStartEvent|Eventos de início de exclusão de pool|Contagem|Total|Número total de exclusões de pool iniciados|Sem dimensões|
|PoolDeleteCompleteEvent|Eventos de conclusão de exclusão do pool|Contagem|Total|Número total de exclusões de pool concluídas|Sem dimensões|
|JobDeleteCompleteEvent|Eventos de conclusão de exclusão do trabalho|Count|Total|Número total de trabalhos que foram excluídos com êxito.|Sem dimensões|
|JobDeleteStartEvent|Eventos de início de exclusão do trabalho|Count|Total|Número total de trabalhos que foram solicitados a serem excluídos.|Sem dimensões|
|JobDisableCompleteEvent|Eventos de conclusão de desabilitação do trabalho|Contagem|Total|Número total de trabalhos que foram desabilitados com êxito.|Sem dimensões|
|JobDisableStartEvent|Eventos de início de desabilitação do trabalho|Count|Total|Número total de trabalhos que foram solicitados a serem desabilitados.|Sem dimensões|
|JobStartEvent|Eventos de início do trabalho|Count|Total|Número total de trabalhos que foram iniciados com êxito.|Sem dimensões|
|JobTerminateCompleteEvent|Eventos de conclusão de encerramento do trabalho|Count|Total|Número total de trabalhos que foram terminados com êxito.|Sem dimensões|
|JobTerminateStartEvent|Eventos de início de encerramento do trabalho|Count|Total|Número total de trabalhos que foram solicitados a serem encerrados.|Sem dimensões|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|connectedclients|Clientes Conectados|Contagem|Máximo||ShardId|
|totalcommandsprocessed|Total de Operações|Contagem|Total||ShardId|
|cachehits|Acertos de Cache|Contagem|Total||ShardId|
|cachemisses|Perdas de Cache|Contagem|Total||ShardId|
|getcommands|Obtenções|Count|Total||ShardId|
|setcommands|Conjuntos|Count|Total||ShardId|
|operationsPerSecond|Operations por segundo|Count|Máximo||ShardId|
|evictedkeys|Chaves Removidas|Count|Total||ShardId|
|totalkeys|Total de Chaves|Count|Máximo||ShardId|
|expiredkeys|Chaves Expiradas|Count|Total||ShardId|
|usedmemory|Memória Usada|Bytes|Máximo||ShardId|
|usedmemorypercentage|Porcentagem de memória utilizada|Percent|Máximo||ShardId|
|usedmemoryRss|Memória RSS Usada|Bytes|Máximo||ShardId|
|serverLoad|Carga do Servidor|Percent|Máximo||ShardId|
|cacheWrite|Gravação de Cache|BytesPerSecond|Máximo||ShardId|
|cacheRead|Leitura de Cache|BytesPerSecond|Máximo||ShardId|
|percentProcessorTime|CPU|Percent|Máximo||ShardId|
|cacheLatency|Microssegundos de latência de cache (versão prévia)|Count|Average||ShardId, SampleType|
|Erros|Erros|Count|Máximo||ShardId, ErrorType|
|connectedclients0|Clientes conectados (Fragmento 0)|Count|Máximo||Sem dimensões|
|totalcommandsprocessed0|Total de operações (Fragmento 0)|Count|Total||Sem dimensões|
|cachehits0|Ocorrências no cache (Fragmento 0)|Count|Total||Sem dimensões|
|cachemisses0|Perdas no cache (Fragmento 0)|Count|Total||Sem dimensões|
|getcommands0|Gets (Fragmento 0)|Count|Total||Sem dimensões|
|setcommands0|Sets (Fragmento 0)|Count|Total||Sem dimensões|
|operationsPerSecond0|Operações por segundo (fragmento 0)|Count|Máximo||Sem dimensões|
|evictedkeys0|Chaves removidas (Fragmento 0)|Count|Total||Sem dimensões|
|totalkeys0|Total de Chaves (Fragmento 0)|Count|Máximo||Sem dimensões|
|expiredkeys0|Chaves expiradas (Fragmento 0)|Count|Total||Sem dimensões|
|usedmemory0|Memória usada (Fragmento 0)|Bytes|Máximo||Sem dimensões|
|usedmemoryRss0|Memória RSS usada (Fragmento 0)|Bytes|Máximo||Sem dimensões|
|serverLoad0|Carga do servidor (Fragmento 0)|Percent|Máximo||Sem dimensões|
|cacheWrite0|Gravação no cache (Fragmento 0)|BytesPerSecond|Máximo||Sem dimensões|
|cacheRead0|Leitura no cache (Fragmento 0)|BytesPerSecond|Máximo||Sem dimensões|
|percentProcessorTime0|CPU (Fragmento 0)|Percent|Máximo||Sem dimensões|
|connectedclients1|Clientes conectados (Fragmento 1)|Count|Máximo||Sem dimensões|
|totalcommandsprocessed1|Total de operações (Fragmento 1)|Count|Total||Sem dimensões|
|cachehits1|Ocorrências no cache (Fragmento 1)|Count|Total||Sem dimensões|
|cachemisses1|Perdas no cache (Fragmento 1)|Count|Total||Sem dimensões|
|getcommands1|Gets (Fragmento 1)|Contagem|Total||Sem dimensões|
|setcommands1|Sets (Fragmento 1)|Count|Total||Sem dimensões|
|operationsPerSecond1|Operações por segundo (fragmento 1)|Count|Máximo||Sem dimensões|
|evictedkeys1|Chaves removidas (Fragmento 1)|Count|Total||Sem dimensões|
|totalkeys1|Total de Chaves (Fragmento 1)|Contagem|Máximo||Sem dimensões|
|expiredkeys1|Chaves expiradas (Fragmento 1)|Contagem|Total||Sem dimensões|
|usedmemory1|Memória usada (Fragmento 1)|Bytes|Máximo||Sem dimensões|
|usedmemoryRss1|Memória RSS usada (Fragmento 1)|Bytes|Máximo||Sem dimensões|
|serverLoad1|Carga do servidor (Fragmento 1)|Percent|Máximo||Sem dimensões|
|cacheWrite1|Gravação no cache (Fragmento 1)|BytesPerSecond|Máximo||Sem dimensões|
|cacheRead1|Leitura no cache (Fragmento 1)|BytesPerSecond|Máximo||Sem dimensões|
|percentProcessorTime1|CPU (Fragmento 1)|Percent|Máximo||Sem dimensões|
|connectedclients2|Clientes conectados (Fragmento 2)|Contagem|Máximo||Sem dimensões|
|totalcommandsprocessed2|Total de operações (Fragmento 2)|Count|Total||Sem dimensões|
|cachehits2|Ocorrências no cache (Fragmento 2)|Count|Total||Sem dimensões|
|cachemisses2|Perdas no cache (Fragmento 2)|Count|Total||Sem dimensões|
|getcommands2|Gets (Fragmento 2)|Count|Total||Sem dimensões|
|setcommands2|Sets (Fragmento 2)|Contagem|Total||Sem dimensões|
|operationsPerSecond2|Operações por segundo (fragmento 2)|Count|Máximo||Sem dimensões|
|evictedkeys2|Chaves removidas (Fragmento 2)|Count|Total||Sem dimensões|
|totalkeys2|Total de Chaves (Fragmento 2)|Contagem|Máximo||Sem dimensões|
|expiredkeys2|Chaves expiradas (Fragmento 2)|Count|Total||Sem dimensões|
|usedmemory2|Memória usada (Fragmento 2)|Bytes|Máximo||Sem dimensões|
|usedmemoryRss2|Memória RSS usada (Fragmento 2)|Bytes|Máximo||Sem dimensões|
|serverLoad2|Carga do servidor (Fragmento 2)|Percent|Máximo||Sem dimensões|
|cacheWrite2|Gravação no cache (Fragmento 2)|BytesPerSecond|Máximo||Sem dimensões|
|cacheRead2|Leitura no cache (Fragmento 2)|BytesPerSecond|Máximo||Sem dimensões|
|percentProcessorTime2|CPU (Fragmento 2)|Percent|Máximo||Sem dimensões|
|connectedclients3|Clientes conectados (Fragmento 3)|Count|Máximo||Sem dimensões|
|totalcommandsprocessed3|Total de operações (Fragmento 3)|Count|Total||Sem dimensões|
|cachehits3|Ocorrências no cache (Fragmento 3)|Count|Total||Sem dimensões|
|cachemisses3|Perdas no cache (Fragmento 3)|Count|Total||Sem dimensões|
|getcommands3|Gets (Fragmento 3)|Count|Total||Sem dimensões|
|setcommands3|Sets (Fragmento 3)|Count|Total||Sem dimensões|
|operationsPerSecond3|Operações por segundo (fragmento 3)|Contagem|Máximo||Sem dimensões|
|evictedkeys3|Chaves removidas (Fragmento 3)|Count|Total||Sem dimensões|
|totalkeys3|Total de Chaves (Fragmento 3)|Count|Máximo||Sem dimensões|
|expiredkeys3|Chaves expiradas (Fragmento 3)|Count|Total||Sem dimensões|
|usedmemory3|Memória usada (Fragmento 3)|Bytes|Máximo||Sem dimensões|
|usedmemoryRss3|Memória RSS usada (Fragmento 3)|Bytes|Máximo||Sem dimensões|
|serverLoad3|Carga do servidor (Fragmento 3)|Percent|Máximo||Sem dimensões|
|cacheWrite3|Gravação no cache (Fragmento 3)|BytesPerSecond|Máximo||Sem dimensões|
|cacheRead3|Leitura no cache (Fragmento 3)|BytesPerSecond|Máximo||Sem dimensões|
|percentProcessorTime3|CPU (Fragmento 3)|Percent|Máximo||Sem dimensões|
|connectedclients4|Clientes conectados (Fragmento 4)|Count|Máximo||Sem dimensões|
|totalcommandsprocessed4|Total de operações (Fragmento 4)|Contagem|Total||Sem dimensões|
|cachehits4|Ocorrências no cache (Fragmento 4)|Count|Total||Sem dimensões|
|cachemisses4|Perdas no cache (Fragmento 4)|Count|Total||Sem dimensões|
|getcommands4|Gets (Fragmento 4)|Contagem|Total||Sem dimensões|
|setcommands4|Sets (Fragmento 4)|Count|Total||Sem dimensões|
|operationsPerSecond4|Operações por segundo (fragmento 4)|Count|Máximo||Sem dimensões|
|evictedkeys4|Chaves removidas (Fragmento 4)|Count|Total||Sem dimensões|
|totalkeys4|Total de Chaves (Fragmento 4)|Count|Máximo||Sem dimensões|
|expiredkeys4|Chaves expiradas (Fragmento 4)|Count|Total||Sem dimensões|
|usedmemory4|Memória usada (Fragmento 4)|Bytes|Máximo||Sem dimensões|
|usedmemoryRss4|Memória RSS usada (Fragmento 4)|Bytes|Máximo||Sem dimensões|
|serverLoad4|Carga do servidor (Fragmento 4)|Percent|Máximo||Sem dimensões|
|cacheWrite4|Gravação no cache (Fragmento 4)|BytesPerSecond|Máximo||Sem dimensões|
|cacheRead4|Leitura no cache (Fragmento 4)|BytesPerSecond|Máximo||Sem dimensões|
|percentProcessorTime4|CPU (Fragmento 4)|Percent|Máximo||Sem dimensões|
|connectedclients5|Clientes conectados (Fragmento 5)|Count|Máximo||Sem dimensões|
|totalcommandsprocessed5|Total de operações (Fragmento 5)|Count|Total||Sem dimensões|
|cachehits5|Ocorrências no cache (Fragmento 5)|Count|Total||Sem dimensões|
|cachemisses5|Perdas no cache (Fragmento 5)|Count|Total||Sem dimensões|
|getcommands5|Gets (Fragmento 5)|Count|Total||Sem dimensões|
|setcommands5|Sets (Fragmento 5)|Count|Total||Sem dimensões|
|operationsPerSecond5|Operações por segundo (fragmento 5)|Count|Máximo||Sem dimensões|
|evictedkeys5|Chaves removidas (Fragmento 5)|Count|Total||Sem dimensões|
|totalkeys5|Total de Chaves (Fragmento 5)|Count|Máximo||Sem dimensões|
|expiredkeys5|Chaves expiradas (Fragmento 5)|Count|Total||Sem dimensões|
|usedmemory5|Memória usada (Fragmento 5)|Bytes|Máximo||Sem dimensões|
|usedmemoryRss5|Memória RSS usada (Fragmento 5)|Bytes|Máximo||Sem dimensões|
|serverLoad5|Carga do servidor (Fragmento 5)|Percent|Máximo||Sem dimensões|
|cacheWrite5|Gravação no cache (Fragmento 5)|BytesPerSecond|Máximo||Sem dimensões|
|cacheRead5|Leitura no cache (Fragmento 5)|BytesPerSecond|Máximo||Sem dimensões|
|percentProcessorTime5|CPU (Fragmento 5)|Percent|Máximo||Sem dimensões|
|connectedclients6|Clientes conectados (Fragmento 6)|Contagem|Máximo||Sem dimensões|
|totalcommandsprocessed6|Total de operações (Fragmento 6)|Count|Total||Sem dimensões|
|cachehits6|Ocorrências no cache (Fragmento 6)|Count|Total||Sem dimensões|
|cachemisses6|Perdas no cache (Fragmento 6)|Contagem|Total||Sem dimensões|
|getcommands6|Gets (Fragmento 6)|Contagem|Total||Sem dimensões|
|setcommands6|Sets (Fragmento 6)|Count|Total||Sem dimensões|
|operationsPerSecond6|Operações por segundo (fragmento 6)|Contagem|Máximo||Sem dimensões|
|evictedkeys6|Chaves removidas (Fragmento 6)|Count|Total||Sem dimensões|
|totalkeys6|Total de Chaves (Fragmento 6)|Count|Máximo||Sem dimensões|
|expiredkeys6|Chaves expiradas (Fragmento 6)|Count|Total||Sem dimensões|
|usedmemory6|Memória usada (Fragmento 6)|Bytes|Máximo||Sem dimensões|
|usedmemoryRss6|Memória RSS usada (Fragmento 6)|Bytes|Máximo||Sem dimensões|
|serverLoad6|Carga do servidor (Fragmento 6)|Percent|Máximo||Sem dimensões|
|cacheWrite6|Gravação no cache (Fragmento 6)|BytesPerSecond|Máximo||Sem dimensões|
|cacheRead6|Leitura no cache (Fragmento 6)|BytesPerSecond|Máximo||Sem dimensões|
|percentProcessorTime6|CPU (Fragmento 6)|Percent|Máximo||Sem dimensões|
|connectedclients7|Clientes conectados (Fragmento 7)|Count|Máximo||Sem dimensões|
|totalcommandsprocessed7|Total de operações (Fragmento 7)|Count|Total||Sem dimensões|
|cachehits7|Ocorrências no cache (Fragmento 7)|Contagem|Total||Sem dimensões|
|cachemisses7|Perdas no cache (Fragmento 7)|Count|Total||Sem dimensões|
|getcommands7|Gets (Fragmento 7)|Contagem|Total||Sem dimensões|
|setcommands7|Sets (Fragmento 7)|Count|Total||Sem dimensões|
|operationsPerSecond7|Operações por segundo (fragmento 7)|Count|Máximo||Sem dimensões|
|evictedkeys7|Chaves removidas (Fragmento 7)|Count|Total||Sem dimensões|
|totalkeys7|Total de Chaves (Fragmento 7)|Count|Máximo||Sem dimensões|
|expiredkeys7|Chaves expiradas (Fragmento 7)|Contagem|Total||Sem dimensões|
|usedmemory7|Memória usada (Fragmento 7)|Bytes|Máximo||Sem dimensões|
|usedmemoryRss7|Memória RSS usada (Fragmento 7)|Bytes|Máximo||Sem dimensões|
|serverLoad7|Carga do servidor (Fragmento 7)|Percent|Máximo||Sem dimensões|
|cacheWrite7|Gravação no cache (Fragmento 7)|BytesPerSecond|Máximo||Sem dimensões|
|cacheRead7|Leitura no cache (Fragmento 7)|BytesPerSecond|Máximo||Sem dimensões|
|percentProcessorTime7|CPU (Fragmento 7)|Percent|Máximo||Sem dimensões|
|connectedclients8|Clientes conectados (Fragmento 8)|Count|Máximo||Sem dimensões|
|totalcommandsprocessed8|Total de operações (Fragmento 8)|Contagem|Total||Sem dimensões|
|cachehits8|Ocorrências no cache (Fragmento 8)|Count|Total||Sem dimensões|
|cachemisses8|Perdas no cache (Fragmento 8)|Contagem|Total||Sem dimensões|
|getcommands8|Gets (Fragmento 8)|Contagem|Total||Sem dimensões|
|setcommands8|Sets (Fragmento 8)|Contagem|Total||Sem dimensões|
|operationsPerSecond8|Operações por segundo (fragmento 8)|Count|Máximo||Sem dimensões|
|evictedkeys8|Chaves removidas (Fragmento 8)|Contagem|Total||Sem dimensões|
|totalkeys8|Total de Chaves (Fragmento 8)|Count|Máximo||Sem dimensões|
|expiredkeys8|Chaves expiradas (Fragmento 8)|Contagem|Total||Sem dimensões|
|usedmemory8|Memória usada (Fragmento 8)|Bytes|Máximo||Sem dimensões|
|usedmemoryRss8|Memória RSS usada (Fragmento 8)|Bytes|Máximo||Sem dimensões|
|serverLoad8|Carga do servidor (Fragmento 8)|Percent|Máximo||Sem dimensões|
|cacheWrite8|Gravação no cache (Fragmento 8)|BytesPerSecond|Máximo||Sem dimensões|
|cacheRead8|Leitura no cache (Fragmento 8)|BytesPerSecond|Máximo||Sem dimensões|
|percentProcessorTime8|CPU (Fragmento 8)|Percent|Máximo||Sem dimensões|
|connectedclients9|Clientes conectados (Fragmento 9)|Contagem|Máximo||Sem dimensões|
|totalcommandsprocessed9|Total de operações (Fragmento 9)|Contagem|Total||Sem dimensões|
|cachehits9|Ocorrências no cache (Fragmento 9)|Count|Total||Sem dimensões|
|cachemisses9|Perdas no cache (Fragmento 9)|Count|Total||Sem dimensões|
|getcommands9|Gets (Fragmento 9)|Count|Total||Sem dimensões|
|setcommands9|Sets (Fragmento 9)|Contagem|Total||Sem dimensões|
|operationsPerSecond9|Operações por segundo (fragmento 9)|Count|Máximo||Sem dimensões|
|evictedkeys9|Chaves removidas (Fragmento 9)|Count|Total||Sem dimensões|
|totalkeys9|Total de Chaves (Fragmento 9)|Count|Máximo||Sem dimensões|
|expiredkeys9|Chaves expiradas (Fragmento 9)|Count|Total||Sem dimensões|
|usedmemory9|Memória usada (Fragmento 9)|Bytes|Máximo||Sem dimensões|
|usedmemoryRss9|Memória RSS usada (Fragmento 9)|Bytes|Máximo||Sem dimensões|
|serverLoad9|Carga do servidor (Fragmento 9)|Percent|Máximo||Sem dimensões|
|cacheWrite9|Gravação no cache (Fragmento 9)|BytesPerSecond|Máximo||Sem dimensões|
|cacheRead9|Leitura no cache (Fragmento 9)|BytesPerSecond|Máximo||Sem dimensões|
|percentProcessorTime9|CPU (Fragmento 9)|Percent|Máximo||Sem dimensões|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|Porcentagem de CPU|Porcentagem de CPU|Porcentagem|Average|O percentual das unidades de computação alocadas que estão atualmente em uso pelas Máquinas Virtuais.|Sem dimensões|
|Entrada de Rede|Entrada na rede|Bytes|Total|O número de bytes recebidos em todos os adaptadores de rede pelas Máquinas Virtuais (Tráfego de Entrada).|Sem dimensões|
|Saída de Rede|Saída da rede|Bytes|Total|O número de bytes de saída em todos os adaptadores de rede pelas Máquinas Virtuais (Tráfego de Saída).|Sem dimensões|
|Disk Read Bytes/Sec|Leitura de Disco|BytesPerSecond|Average|Média de bytes lidos do disco durante o período de monitoramento.|Sem dimensões|
|Disk Write Bytes/Sec|Gravação de disco|BytesPerSecond|Average|Média de bytes gravados em disco durante o período de monitoramento.|Sem dimensões|
|Operações de leitura de disco/Seg|Operações de leitura de disco/Seg|CountPerSecond|Average|IOPS de leitura de disco.|Sem dimensões|
|Operações de gravação de disco/Seg|Operações de gravação de disco/Seg|CountPerSecond|Average|IOPS de gravação de disco.|Sem dimensões|

## <a name="microsoftclassiccomputedomainnamesslotsroles"></a>Microsoft.ClassicCompute/domainNames/slots/roles

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|Porcentagem de CPU|Porcentagem de CPU|Porcentagem|Average|O percentual das unidades de computação alocadas que estão atualmente em uso pelas Máquinas Virtuais.|RoleInstanceId|
|Entrada de Rede|Entrada na rede|Bytes|Total|O número de bytes recebidos em todos os adaptadores de rede pelas Máquinas Virtuais (Tráfego de Entrada).|RoleInstanceId|
|Saída de Rede|Saída da rede|Bytes|Total|O número de bytes de saída em todos os adaptadores de rede pelas Máquinas Virtuais (Tráfego de Saída).|RoleInstanceId|
|Disk Read Bytes/Sec|Leitura de Disco|BytesPerSecond|Average|Média de bytes lidos do disco durante o período de monitoramento.|RoleInstanceId|
|Disk Write Bytes/Sec|Gravação de disco|BytesPerSecond|Average|Média de bytes gravados em disco durante o período de monitoramento.|RoleInstanceId|
|Operações de leitura de disco/Seg|Operações de leitura de disco/Seg|CountPerSecond|Average|IOPS de leitura de disco.|RoleInstanceId|
|Operações de gravação de disco/Seg|Operações de gravação de disco/Seg|CountPerSecond|Average|IOPS de gravação de disco.|RoleInstanceId|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|TotalCalls|Total de Chamadas|Contagem|Total|Número total de chamadas.|ApiName, OperationName, região|
|SuccessfulCalls|Chamadas com Êxito|Count|Total|Número de chamadas com êxito.|ApiName, OperationName, região|
|TotalErrors|Total de Erros|Contagem|Total|Número total de chamadas com resposta de erro (código de resposta HTTP 4xx ou 5xx).|ApiName, OperationName, região|
|BlockedCalls|Chamadas Bloqueadas|Count|Total|Número de chamadas que excederam a taxa ou o limite de cota.|ApiName, OperationName, região|
|ServerErrors|Erros do Servidor|Count|Total|Número de chamadas com erro interno do serviço (código de resposta HTTP 5xx).|ApiName, OperationName, região|
|ClientErrors|Erros do Cliente|Count|Total|Número de chamadas com erro do lado do cliente (código de resposta HTTP 4xx).|ApiName, OperationName, região|
|DataIn|Entrada de Dados|Bytes|Total|Tamanho dos dados de entrada em bytes.|ApiName, OperationName, região|
|DataOut|Saída de Dados|Bytes|Total|Tamanho dos dados de saída em bytes.|ApiName, OperationName, região|
|Latência|Latência|MilliSeconds|Average|Latência em milissegundos.|ApiName, OperationName, região|
|CharactersTranslated|Caracteres Traduzidos|Count|Total|Número total de caracteres na solicitação de texto de entrada.|ApiName, OperationName, região|
|CharactersTrained|Caracteres treinados|Contagem|Total|Número total de caracteres treinados.|ApiName, OperationName, região|
|SpeechSessionDuration|Duração da Sessão de Fala|Segundos|Total|Duração total da sessão de fala em segundos.|ApiName, OperationName, região|
|TotalTransactions|Total de transações|Count|Total|Número total de transações.|Sem dimensões|
|TotalTokenCalls|Total de Chamadas de Token|Count|Total|Número total de chamadas de token.|ApiName, OperationName, região|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|Porcentagem de CPU|Porcentagem de CPU|Porcentagem|Média|A porcentagem das unidades de computação alocadas que estão atualmente em uso pela máquina virtual ou pelas máquinas virtuais|Sem dimensões|
|Entrada de Rede|Rede em Faturável|Bytes|Total|O número de bytes faturáveis recebidos em todas as interfaces de rede pelas máquinas virtuais (tráfego de entrada)|Sem dimensões|
|Saída de Rede|Rede fora de Faturável|Bytes|Total|O número de bytes faturáveis de saída em todas as interfaces de rede pelas máquinas virtuais (tráfego de saída)|Sem dimensões|
|Bytes de Leitura de Disco|Bytes de leitura de disco|Bytes|Total|Bytes lidos do disco durante o período de monitoramento|Sem dimensões|
|Bytes de Gravação do Disco|Bytes de gravação de disco|Bytes|Total|Bytes gravados no disco durante o período de monitoramento|Sem dimensões|
|Operações de leitura de disco/Seg|Operações de leitura de disco/Seg|CountPerSecond|Average|IOPS de Leitura de Disco|Sem dimensões|
|Operações de gravação de disco/Seg|Operações de gravação de disco/Seg|CountPerSecond|Average|IOPS de Gravação do Disco|Sem dimensões|
|Créditos de CPU Restantes|Créditos de CPU restantes|Contagem|Average|Número total de créditos disponíveis para intermitência|Sem dimensões|
|Créditos de CPU consumidos|Créditos de CPU consumidos|Count|Average|Número total de créditos consumido pela Máquina Virtual|Sem dimensões|
|Por Bytes de Leitura do Disco/s|Bytes de leitura do disco de dados/s (preterido)|CountPerSecond|Average|Bytes/s lidos de um único disco durante o período de monitoramento|SlotId|
|Por Bytes de Gravação de Disco/s|Bytes de gravação do disco de dados/s (preterido)|CountPerSecond|Average|Bytes/s gravados em um único disco durante o período de monitoramento|SlotId|
|Por operações de Leitura de Disco/s|Operações de leitura de disco de dados/s (preterido)|CountPerSecond|Average|Ler IOPS de um único disco durante o período de monitoramento|SlotId|
|Por Operações de Gravação de Disco/s|Operações de gravação do disco de dados/s (preterido)|CountPerSecond|Average|Gravar IOPS de um único disco durante o período de monitoramento|SlotId|
|QD Por Disco|Disco de dados QD (preterido)|Contagem|Average|Profundidade da Fila do Disco de Dados (ou Tamanho da Fila)|SlotId|
|SO Por Bytes de Leitura do Disco/s|Bytes de leitura do disco do so/s (preterido)|CountPerSecond|Average|Bytes/s lidos de um único disco durante o período de monitoramento para o disco do sistema operacional|Sem dimensões|
|SO Por Bytes de Gravação do Disco/s|Bytes de gravação de disco do sistema operacional/s (preterido)|CountPerSecond|Average|Bytes/s gravados em um único disco durante o período de monitoramento do disco do sistema operacional|Sem dimensões|
|SO Por Operações de Leitura do Disco/s|Operações de leitura de disco do so/s (preterido)|CountPerSecond|Average|Ler IOPS de um único disco durante o período de monitoramento para o disco do sistema operacional|Sem dimensões|
|SO Por Operações de Gravação do Disco/s|Operações de gravação de disco do sistema operacional/s (preterido)|CountPerSecond|Average|Gravar IOPS de um único disco durante o período de monitoramento para o disco do sistema operacional|Sem dimensões|
|QD do SO Por Disco|Disco do so QD (preterido)|Count|Average|Profundidade da Fila do Disco do SO (ou Tamanho da Fila)|Sem dimensões|
|Bytes de leitura do disco de dados/s|Bytes de Leitura do Disco de Dados/s (Visualização)|CountPerSecond|Average|Bytes/s lidos de um único disco durante o período de monitoramento|LUN|
|Bytes de gravação do disco de dados/s|Bytes de Gravação do Disco de Dados/s (Visualização)|CountPerSecond|Average|Bytes/s gravados em um único disco durante o período de monitoramento|LUN|
|Operações de leitura de disco de dados/s|Operações de Leitura do Disco de Dados/s (Visualização)|CountPerSecond|Average|Ler IOPS de um único disco durante o período de monitoramento|LUN|
|Operações de gravação do disco de dados/s|Operações de Gravação do Disco de Dados/s (Visualização)|CountPerSecond|Average|Gravar IOPS de um único disco durante o período de monitoramento|LUN|
|Profundidade da Fila de Disco de Dados|Profundidade da fila do disco de dados (visualização)|Contagem|Average|Profundidade da Fila do Disco de Dados (ou Tamanho da Fila)|LUN|
|Bytes de leitura do disco do so/s|Bytes de Leitura do Disco do SO/s (Visualização)|CountPerSecond|Average|Bytes/s lidos de um único disco durante o período de monitoramento para o disco do sistema operacional|Sem dimensões|
|Bytes de gravação de disco do sistema operacional/s|Bytes de Gravação do Disco do SO/s (Visualização)|CountPerSecond|Average|Bytes/s gravados em um único disco durante o período de monitoramento do disco do sistema operacional|Sem dimensões|
|Operações de leitura de disco do so/s|Operações de Leitura do Disco do SO/s (Visualização)|CountPerSecond|Average|Ler IOPS de um único disco durante o período de monitoramento para o disco do sistema operacional|Sem dimensões|
|Operações de gravação de disco do so/s|Operações de Gravação do Disco do SO/s (Visualização)|CountPerSecond|Average|Gravar IOPS de um único disco durante o período de monitoramento para o disco do sistema operacional|Sem dimensões|
|Profundidade da Fila de Disco do SO|Profundidade da fila de disco do so (visualização)|Count|Average|Profundidade da Fila do Disco do SO (ou Tamanho da Fila)|Sem dimensões|
|Fluxos de Entrada|Fluxos de entrada (visualização)|Contagem|Average|Os fluxos de entrada são o número de fluxos atuais na direção de entrada (tráfego entrando na VM)|Sem dimensões|
|Fluxos de Saída|Fluxos de saída (visualização)|Count|Average|Fluxos de saída são o número de fluxos atuais na direção de saída (tráfego saindo da VM)|Sem dimensões|
|Taxa Máxima de Criação de Fluxos de Entrada|Taxa máxima de criação de fluxos de entrada (visualização)|CountPerSecond|Average|A taxa de criação máxima de fluxos de entrada (tráfego entrando na VM)|Sem dimensões|
|Taxa Máxima de Criação de Fluxos de Saída|Taxa máxima de criação de fluxos de saída (versão prévia)|CountPerSecond|Average|A taxa de criação máxima de fluxos de saída (tráfego saindo da VM)|Sem dimensões|
|Cache do disco de dados Premium-acesso de leitura|Leitura do cache do disco de dados Premium (versão prévia)|Percent|Average|Cache do disco de dados Premium-acesso de leitura|LUN|
|Erro de leitura do cache do disco de dados Premium|Cache do disco de dados Premium-erro de leitura (visualização)|Percent|Average|Erro de leitura do cache do disco de dados Premium|LUN|
|Cache de disco do sistema operacional Premium-acesso de leitura|Cache de disco do sistema operacional Premium-acesso de leitura (versão prévia)|Percent|Average|Cache de disco do sistema operacional Premium-acesso de leitura|Sem dimensões|
|Erro de leitura do cache de disco do so premium|Erro de leitura do cache de disco do so premium (visualização)|Percent|Average|Erro de leitura do cache de disco do so premium|Sem dimensões|
|Rede no Total|Rede no Total|Bytes|Total|O número de bytes recebidos em todas as interfaces de rede pela máquina virtual ou pelas máquinas virtuais (tráfego de entrada)|Sem dimensões|
|Rede fora do Total|Rede fora do Total|Bytes|Total|O número de bytes de saída em todas as interfaces de rede pela máquina virtual ou pelas máquinas virtuais (tráfego de saída)|Sem dimensões|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|Porcentagem de CPU|Porcentagem de CPU|Porcentagem|Média|A porcentagem das unidades de computação alocadas que estão atualmente em uso pela máquina virtual ou pelas máquinas virtuais|VMName|
|Entrada de Rede|Rede em Faturável|Bytes|Total|O número de bytes faturáveis recebidos em todas as interfaces de rede pelas máquinas virtuais (tráfego de entrada)|VMName|
|Saída de Rede|Rede fora de Faturável|Bytes|Total|O número de bytes faturáveis de saída em todas as interfaces de rede pelas máquinas virtuais (tráfego de saída)|VMName|
|Bytes de Leitura de Disco|Bytes de leitura de disco|Bytes|Total|Bytes lidos do disco durante o período de monitoramento|VMName|
|Bytes de Gravação do Disco|Bytes de gravação de disco|Bytes|Total|Bytes gravados no disco durante o período de monitoramento|VMName|
|Operações de leitura de disco/Seg|Operações de leitura de disco/Seg|CountPerSecond|Average|IOPS de Leitura de Disco|VMName|
|Operações de gravação de disco/Seg|Operações de gravação de disco/Seg|CountPerSecond|Average|IOPS de Gravação do Disco|VMName|
|Créditos de CPU restantes|Créditos de CPU restantes|Count|Average|Número total de créditos disponíveis para intermitência|Sem dimensões|
|Créditos de CPU consumidos|Créditos de CPU consumidos|Count|Average|Número total de créditos consumido pela Máquina Virtual|Sem dimensões|
|Por Bytes de Leitura do Disco/s|Bytes de leitura do disco de dados/s (preterido)|CountPerSecond|Average|Bytes/s lidos de um único disco durante o período de monitoramento|SlotId|
|Por Bytes de Gravação de Disco/s|Bytes de gravação do disco de dados/s (preterido)|CountPerSecond|Average|Bytes/s gravados em um único disco durante o período de monitoramento|SlotId|
|Por operações de Leitura de Disco/s|Operações de leitura de disco de dados/s (preterido)|CountPerSecond|Average|Ler IOPS de um único disco durante o período de monitoramento|SlotId|
|Por Operações de Gravação de Disco/s|Operações de gravação do disco de dados/s (preterido)|CountPerSecond|Average|Gravar IOPS de um único disco durante o período de monitoramento|SlotId|
|QD Por Disco|Disco de dados QD (preterido)|Count|Average|Profundidade da Fila do Disco de Dados (ou Tamanho da Fila)|SlotId|
|SO Por Bytes de Leitura do Disco/s|Bytes de leitura do disco do so/s (preterido)|CountPerSecond|Average|Bytes/s lidos de um único disco durante o período de monitoramento para o disco do sistema operacional|Sem dimensões|
|SO Por Bytes de Gravação do Disco/s|Bytes de gravação de disco do sistema operacional/s (preterido)|CountPerSecond|Average|Bytes/s gravados em um único disco durante o período de monitoramento do disco do sistema operacional|Sem dimensões|
|SO Por Operações de Leitura do Disco/s|Operações de leitura de disco do so/s (preterido)|CountPerSecond|Average|Ler IOPS de um único disco durante o período de monitoramento para o disco do sistema operacional|Sem dimensões|
|SO Por Operações de Gravação do Disco/s|Operações de gravação de disco do sistema operacional/s (preterido)|CountPerSecond|Average|Gravar IOPS de um único disco durante o período de monitoramento para o disco do sistema operacional|Sem dimensões|
|QD do SO Por Disco|Disco do so QD (preterido)|Count|Average|Profundidade da Fila do Disco do SO (ou Tamanho da Fila)|Sem dimensões|
|Bytes de leitura do disco de dados/s|Bytes de Leitura do Disco de Dados/s (Visualização)|CountPerSecond|Average|Bytes/s lidos de um único disco durante o período de monitoramento|LUN, VMName|
|Bytes de gravação do disco de dados/s|Bytes de Gravação do Disco de Dados/s (Visualização)|CountPerSecond|Average|Bytes/s gravados em um único disco durante o período de monitoramento|LUN, VMName|
|Operações de leitura de disco de dados/s|Operações de Leitura do Disco de Dados/s (Visualização)|CountPerSecond|Average|Ler IOPS de um único disco durante o período de monitoramento|LUN, VMName|
|Operações de gravação do disco de dados/s|Operações de Gravação do Disco de Dados/s (Visualização)|CountPerSecond|Average|Gravar IOPS de um único disco durante o período de monitoramento|LUN, VMName|
|Profundidade da Fila de Disco de Dados|Profundidade da fila do disco de dados (visualização)|Count|Average|Profundidade da Fila do Disco de Dados (ou Tamanho da Fila)|LUN, VMName|
|Bytes de leitura do disco do so/s|Bytes de Leitura do Disco do SO/s (Visualização)|CountPerSecond|Average|Bytes/s lidos de um único disco durante o período de monitoramento para o disco do sistema operacional|VMName|
|Bytes de gravação de disco do sistema operacional/s|Bytes de Gravação do Disco do SO/s (Visualização)|CountPerSecond|Average|Bytes/s gravados em um único disco durante o período de monitoramento do disco do sistema operacional|VMName|
|Operações de leitura de disco do so/s|Operações de Leitura do Disco do SO/s (Visualização)|CountPerSecond|Average|Ler IOPS de um único disco durante o período de monitoramento para o disco do sistema operacional|VMName|
|Operações de gravação de disco do so/s|Operações de Gravação do Disco do SO/s (Visualização)|CountPerSecond|Average|Gravar IOPS de um único disco durante o período de monitoramento para o disco do sistema operacional|VMName|
|Profundidade da Fila de Disco do SO|Profundidade da fila de disco do so (visualização)|Count|Average|Profundidade da Fila do Disco do SO (ou Tamanho da Fila)|VMName|
|Fluxos de Entrada|Fluxos de entrada (visualização)|Count|Average|Os fluxos de entrada são o número de fluxos atuais na direção de entrada (tráfego entrando na VM)|VMName|
|Fluxos de Saída|Fluxos de saída (visualização)|Count|Average|Fluxos de saída são o número de fluxos atuais na direção de saída (tráfego saindo da VM)|VMName|
|Taxa Máxima de Criação de Fluxos de Entrada|Taxa máxima de criação de fluxos de entrada (visualização)|CountPerSecond|Average|A taxa de criação máxima de fluxos de entrada (tráfego entrando na VM)|VMName|
|Taxa Máxima de Criação de Fluxos de Saída|Taxa máxima de criação de fluxos de saída (versão prévia)|CountPerSecond|Average|A taxa de criação máxima de fluxos de saída (tráfego saindo da VM)|VMName|
|Cache do disco de dados Premium-acesso de leitura|Leitura do cache do disco de dados Premium (versão prévia)|Percent|Average|Cache do disco de dados Premium-acesso de leitura|LUN, VMName|
|Erro de leitura do cache do disco de dados Premium|Cache do disco de dados Premium-erro de leitura (visualização)|Percent|Average|Erro de leitura do cache do disco de dados Premium|LUN, VMName|
|Cache de disco do sistema operacional Premium-acesso de leitura|Cache de disco do sistema operacional Premium-acesso de leitura (versão prévia)|Percent|Average|Cache de disco do sistema operacional Premium-acesso de leitura|VMName|
|Erro de leitura do cache de disco do so premium|Erro de leitura do cache de disco do so premium (visualização)|Percent|Average|Erro de leitura do cache de disco do so premium|VMName|
|Rede no Total|Rede no Total|Bytes|Total|O número de bytes recebidos em todas as interfaces de rede pela máquina virtual ou pelas máquinas virtuais (tráfego de entrada)|VMName|
|Rede fora do Total|Rede fora do Total|Bytes|Total|O número de bytes de saída em todas as interfaces de rede pela máquina virtual ou pelas máquinas virtuais (tráfego de saída)|VMName|

## <a name="microsoftcontainerinstancecontainergroups"></a>Microsoft.ContainerInstance/containerGroups

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|CpuUsage|Uso da CPU|Count|Average|Uso de CPU em todos os núcleos em milicores.|containerName|
|MemoryUsage|Uso de Memória|Bytes|Average|Uso de memória total em bytes.|containerName|
|NetworkBytesReceivedPerSecond|Bytes de Rede Recebidos Por Segundo|Bytes|Average|Os bytes de rede recebidos por segundo.|Sem dimensões|
|NetworkBytesTransmittedPerSecond|Bytes de Rede Transmitidos Por Segundo|Bytes|Average|Os bytes de rede transmitidos por segundo.|Sem dimensões|

## <a name="microsoftcontainerregistryregistries"></a>Microsoft.ContainerRegistry/registries

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|TotalPullCount|Contagem total de pull|Contagem|Average|Número de pulls de imagem no total|Sem dimensões|
|SuccessfulPullCount|Contagem de pull bem-sucedida|Count|Average|Número de pulls de imagem bem-sucedidos|Sem dimensões|
|TotalPushCount|Contagem total de push|Contagem|Average|Número de envios por push de imagem no total|Sem dimensões|
|SuccessfulPushCount|Contagem de push bem-sucedida|Count|Average|Número de Pushes de imagem bem-sucedidos|Sem dimensões|
|RunDuration|Duração da execução|Milissegundos|Total|Duração da execução em milissegundos|Sem dimensões|

## <a name="microsoftcontainerservicemanagedclusters"></a>Microsoft.ContainerService/managedClusters

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|kube_node_status_allocatable_cpu_cores|Número total de núcleos de cpu disponíveis em um cluster gerenciado|Contagem|Total|Número total de núcleos de cpu disponíveis em um cluster gerenciado|Sem dimensões|
|kube_node_status_allocatable_memory_bytes|Número total de memória disponível em um cluster gerenciado|Bytes|Total|Número total de memória disponível em um cluster gerenciado|Sem dimensões|
|kube_pod_status_ready|Número de compartimentos no estado Pronto|Count|Total|Número de compartimentos no estado Pronto|namespace, pod|
|kube_node_status_condition|Status para as várias condições de nó|Count|Total|Status para as várias condições de nó|condição, status, status2, nó|
|kube_pod_status_phase|Número de pods por fase|Count|Total|Número de pods por fase|fase, namespace, pod|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|DCIApiCalls|Chamadas à API do Customer Insights|Count|Total||Sem dimensões|
|DCIMappingImportOperationSuccessfulLines|Linhas bem-sucedidas da operação de importação de mapeamento|Count|Total||Sem dimensões|
|DCIMappingImportOperationFailedLines|Linhas com falha da operação de importação de mapeamento|Count|Total||Sem dimensões|
|DCIMappingImportOperationTotalLines|Total de linhas da operação de importação de mapeamento|Count|Total||Sem dimensões|
|DCIMappingImportOperationRuntimeInSeconds|Tempo de execução da operação de importação de mapeamento em segundos|Segundos|Total||Sem dimensões|
|DCIOutboundProfileExportSucceeded|Exportação de perfil de saída com êxito|Count|Total||Sem dimensões|
|DCIOutboundProfileExportFailed|Exportação de perfil de saída com falha|Contagem|Total||Sem dimensões|
|DCIOutboundProfileExportDuration|Duração da exportação de perfil de saída|Segundos|Total||Sem dimensões|
|DCIOutboundKpiExportSucceeded|Exportação de KPI de saída com êxito|Contagem|Total||Sem dimensões|
|DCIOutboundKpiExportFailed|Exportação de KPI de saída com falha|Count|Total||Sem dimensões|
|DCIOutboundKpiExportDuration|Duração da exportação de KPI de saída|Segundos|Total||Sem dimensões|
|DCIOutboundKpiExportStarted|Início da exportação de KPI de saída|Segundos|Total||Sem dimensões|
|DCIOutboundKpiRecordCount|Contagem de registros de KPI de saída|Segundos|Total||Sem dimensões|
|DCIOutboundProfileExportCount|Contagem de exportações de perfil de saída|Segundos|Total||Sem dimensões|
|DCIOutboundInitialProfileExportFailed|Exportação de perfil inicial de saída com falha|Segundos|Total||Sem dimensões|
|DCIOutboundInitialProfileExportSucceeded|Exportação de perfil inicial de saída com êxito|Segundos|Total||Sem dimensões|
|DCIOutboundInitialKpiExportFailed|Exportação de KPI inicial de saída com falha|Segundos|Total||Sem dimensões|
|DCIOutboundInitialKpiExportSucceeded|Exportação de KPI inicial de saída com êxito|Segundos|Total||Sem dimensões|
|DCIOutboundInitialProfileExportDurationInSeconds|Duração da exportação de perfil inicial de saída em segundos|Segundos|Total||Sem dimensões|
|AdlaJobForStandardKpiFailed|Trabalho do ADLA para KPI padrão com falha em segundos|Segundos|Total||Sem dimensões|
|AdlaJobForStandardKpiTimeOut|Trabalho do ADLA para KPI padrão com TimeOut em segundos|Segundos|Total||Sem dimensões|
|AdlaJobForStandardKpiCompleted|Trabalho do ADLA para KPI padrão concluído em segundos|Segundos|Total||Sem dimensões|
|ImportASAValuesFailed|Contagem de valores do ASA de importação com falha|Count|Total||Sem dimensões|
|ImportASAValuesSucceeded|Contagem de valores do ASA de importação com êxito|Contagem|Total||Sem dimensões|
|DCIProfilesCount|Contagem de Instâncias do Perfil|Count|Last||Sem dimensões|
|DCIInteractionsPerMonthCount|Contagem de Interações por Mês|Count|Last||Sem dimensões|
|DCIKpisCount|Contagem de KPIs|Count|Last||Sem dimensões|
|DCISegmentsCount|Contagem de Segmentos|Count|Last||Sem dimensões|
|DCIPredictiveMatchPoliciesCount|Contagem de Correspondência Preditiva|Count|Last||Sem dimensões|
|DCIPredictionsCount|Contagem de Previsão|Contagem|Last||Sem dimensões|

## <a name="microsoftdataboxedgedataboxedgedevices"></a>Microsoft.DataBoxEdge/dataBoxEdgeDevices

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|NICReadThroughput|Taxa de transferência de leitura (rede)|BytesPerSecond|Average|A taxa de transferência de leitura da interface de rede no dispositivo no período de relatório para todos os volumes no gateway.|InstanceName|
|NICWriteThroughput|Taxa de transferência de gravação (rede)|BytesPerSecond|Average|A taxa de transferência de gravação da interface de rede no dispositivo no período de relatório para todos os volumes no gateway.|InstanceName|
|CloudReadThroughputPerShare|Taxa de transferência de download de nuvem (compartilhamento)|BytesPerSecond|Average|A taxa de transferência de download para o Azure de um compartilhamento durante o período de relatório.|Compartilhar|
|CloudUploadThroughputPerShare|Taxa de transferência de upload de nuvem (compartilhamento)|BytesPerSecond|Average|A taxa de transferência de upload para o Azure de um compartilhamento durante o período de relatório.|Compartilhar|
|BytesUploadedToCloudPerShare|Bytes de nuvem carregados (compartilhamento)|Bytes|Average|O número total de bytes que são carregados no Azure de um compartilhamento durante o período de relatório.|Compartilhar|
|TotalCapacity|Capacidade Total|Bytes|Average|Capacidade Total|Sem dimensões|
|AvailableCapacity|Capacidade Disponível|Bytes|Average|A capacidade disponível em bytes durante o período de relatório.|Sem dimensões|
|CloudUploadThroughput|Produtividade de Upload para Nuvem|BytesPerSecond|Average|A taxa de transferência de upload de nuvem para o Azure durante o período de relatório.|Sem dimensões|
|CloudReadThroughput|Produtividade de Download de Nuvem|BytesPerSecond|Average|A taxa de transferência de download de nuvem para o Azure durante o período de relatório.|Sem dimensões|
|BytesUploadedToCloud|Bytes de nuvem carregados (dispositivo)|Bytes|Average|O número total de bytes que são carregados no Azure de um dispositivo durante o período de relatório.|Sem dimensões|
|HyperVVirtualProcessorUtilization|Computação de borda – percentual de CPU|Percent|Average|Porcentagem de Uso da CPU|InstanceName|
|HyperVMemoryUtilization|Computação de borda-uso de memória|Percent|Average|Quantidade de RAM em Uso|InstanceName|

## <a name="microsoftdatafactorydatafactories"></a>Microsoft.DataFactory/datafactories

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|FailedRuns|Execuções com Falhas|Count|Total||pipelinename, ActivityName|
|SuccessfulRuns|Execuções com Êxito|Contagem|Total||pipelinename, ActivityName|

## <a name="microsoftdatafactoryfactories"></a>Microsoft.DataFactory/factories

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|PipelineFailedRuns|Métricas de execução do pipeline com falha|Count|Total||FailureType, Nome|
|PipelineSucceededRuns|Métricas de execução do pipeline bem-sucedido|Count|Total||FailureType, Nome|
|ActivityFailedRuns|Métricas de execução de atividades com falha|Count|Total||ActivityType, PipelineName, FailureType, Nome|
|ActivitySucceededRuns|Métricas de execução de atividades bem-sucedidas|Count|Total||ActivityType, PipelineName, FailureType, Nome|
|TriggerFailedRuns|Métricas de execuções do gatilho com falha|Count|Total||Nome, FailureType|
|TriggerSucceededRuns|Métricas de execuções do gatilho bem-sucedidas|Count|Total||Nome, FailureType|
|IntegrationRuntimeCpuPercentage|Utilização de CPU de tempo de execução de integração|Percent|Average||IntegrationRuntimeName, NodeName|
|IntegrationRuntimeAvailableMemory|Memória disponível de tempo de execução de integração|Bytes|Average||IntegrationRuntimeName, NodeName|
|MaxAllowedResourceCount|Contagem máxima de entidades permitidas|Contagem|Máximo||Sem dimensões|
|MaxAllowedFactorySizeInGbUnits|Tamanho máximo de fábrica permitido (unidade GB)|Count|Máximo||Sem dimensões|
|ResourceCount|Contagem total de entidades|Count|Máximo||Sem dimensões|
|FactorySizeInGbUnits|Tamanho total de fábrica (unidade GB)|Count|Máximo||Sem dimensões|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|JobEndedSuccess|Trabalhos com êxito|Count|Total|Contagem de trabalhos com êxito.|Sem dimensões|
|JobEndedFailure|Trabalhos com falha|Count|Total|Contagem de trabalhos com falha.|Sem dimensões|
|JobEndedCancelled|Trabalhos cancelados|Contagem|Total|Contagem de trabalhos cancelados.|Sem dimensões|
|JobAUEndedSuccess|Tempo de AU com êxito|Segundos|Total|Tempo total de AU dos trabalhos com êxito.|Sem dimensões|
|JobAUEndedFailure|Tempo de AU com falha|Segundos|Total|Tempo total de AU dos trabalhos com falha.|Sem dimensões|
|JobAUEndedCancelled|Hora de AU cancelada|Segundos|Total|Tempo total de AU para trabalhos cancelados.|Sem dimensões|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|TotalStorage|Armazenamento total|Bytes|Máximo|Quantidade total de dados armazenados na conta.|Sem dimensões|
|DataWritten|Dados Gravados|Bytes|Total|Quantidade total de dados gravados na conta.|Sem dimensões|
|DataRead|Leitura de dados|Bytes|Total|Quantidade total de dados lidos na conta.|Sem dimensões|
|WriteRequests|Solicitações de Gravação|Contagem|Total|Contagem de solicitações de gravação de dados para a conta.|Sem dimensões|
|ReadRequests|Solicitações de leitura|Count|Total|Contagem solicitações de leitura de dados para a conta.|Sem dimensões|

## <a name="microsoftdbformariadbservers"></a>Microsoft.DBforMariaDB/servers

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|cpu_percent|Porcentagem de CPU|Percent|Average|Porcentagem de CPU|Sem dimensões|
|memory_percent|Porcentagem de memória|Percent|Average|Porcentagem de memória|Sem dimensões|
|io_consumption_percent|Porcentagem de E/S|Percent|Average|Porcentagem de E/S|Sem dimensões|
|storage_percent|Porcentagem de armazenamento|Percent|Average|Porcentagem de armazenamento|Sem dimensões|
|storage_used|Armazenamento usado|Bytes|Average|Armazenamento usado|Sem dimensões|
|storage_limit|Limite de armazenamento|Bytes|Average|Limite de armazenamento|Sem dimensões|
|serverlog_storage_percent|Porcentagem de armazenamento do Log do Servidor|Percent|Average|Porcentagem de armazenamento do Log do Servidor|Sem dimensões|
|serverlog_storage_usage|Armazenamento do Log do Servidor usado|Bytes|Average|Armazenamento do Log do Servidor usado|Sem dimensões|
|serverlog_storage_limit|Limite de armazenamento do Log do Servidor|Bytes|Average|Limite de armazenamento do Log do Servidor|Sem dimensões|
|active_connections|Conexões ativas|Count|Average|Conexões ativas|Sem dimensões|
|connections_failed|Conexões com Falha|Count|Total|Conexões com Falha|Sem dimensões|
|seconds_behind_master|Retardo de replicação em segundos|Count|Average|Retardo de replicação em segundos|Sem dimensões|
|backup_storage_used|Armazenamento de backup usado|Bytes|Average|Armazenamento de backup usado|Sem dimensões|
|network_bytes_egress|Saída de Rede|Bytes|Total|Saída de Rede em conexões ativas|Sem dimensões|
|network_bytes_ingress|Entrada de Rede|Bytes|Total|Entrada de Rede em conexões ativas|Sem dimensões|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|cpu_percent|Porcentagem de CPU|Percent|Average|Porcentagem de CPU|Sem dimensões|
|memory_percent|Porcentagem de memória|Percent|Average|Porcentagem de memória|Sem dimensões|
|io_consumption_percent|Porcentagem de E/S|Percent|Average|Porcentagem de E/S|Sem dimensões|
|storage_percent|Porcentagem de armazenamento|Percent|Average|Porcentagem de armazenamento|Sem dimensões|
|storage_used|Armazenamento usado|Bytes|Average|Armazenamento usado|Sem dimensões|
|storage_limit|Limite de armazenamento|Bytes|Average|Limite de armazenamento|Sem dimensões|
|serverlog_storage_percent|Porcentagem de armazenamento do Log do Servidor|Percent|Average|Porcentagem de armazenamento do Log do Servidor|Sem dimensões|
|serverlog_storage_usage|Armazenamento do Log do Servidor usado|Bytes|Average|Armazenamento do Log do Servidor usado|Sem dimensões|
|serverlog_storage_limit|Limite de armazenamento do Log do Servidor|Bytes|Average|Limite de armazenamento do Log do Servidor|Sem dimensões|
|active_connections|Conexões ativas|Count|Average|Conexões ativas|Sem dimensões|
|connections_failed|Conexões com Falha|Count|Total|Conexões com Falha|Sem dimensões|
|seconds_behind_master|Retardo de replicação em segundos|Count|Average|Retardo de replicação em segundos|Sem dimensões|
|backup_storage_used|Armazenamento de backup usado|Bytes|Average|Armazenamento de backup usado|Sem dimensões|
|network_bytes_egress|Saída de Rede|Bytes|Total|Saída de Rede em conexões ativas|Sem dimensões|
|network_bytes_ingress|Entrada de Rede|Bytes|Total|Entrada de Rede em conexões ativas|Sem dimensões|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|cpu_percent|Porcentagem de CPU|Percent|Average|Porcentagem de CPU|Sem dimensões|
|memory_percent|Porcentagem de memória|Percent|Average|Porcentagem de memória|Sem dimensões|
|io_consumption_percent|Porcentagem de E/S|Percent|Average|Porcentagem de E/S|Sem dimensões|
|storage_percent|Porcentagem de armazenamento|Percent|Average|Porcentagem de armazenamento|Sem dimensões|
|storage_used|Armazenamento usado|Bytes|Average|Armazenamento usado|Sem dimensões|
|storage_limit|Limite de armazenamento|Bytes|Average|Limite de armazenamento|Sem dimensões|
|serverlog_storage_percent|Porcentagem de armazenamento do Log do Servidor|Percent|Average|Porcentagem de armazenamento do Log do Servidor|Sem dimensões|
|serverlog_storage_usage|Armazenamento do Log do Servidor usado|Bytes|Average|Armazenamento do Log do Servidor usado|Sem dimensões|
|serverlog_storage_limit|Limite de armazenamento do Log do Servidor|Bytes|Average|Limite de armazenamento do Log do Servidor|Sem dimensões|
|active_connections|Conexões ativas|Count|Average|Conexões ativas|Sem dimensões|
|connections_failed|Conexões com Falha|Contagem|Total|Conexões com Falha|Sem dimensões|
|backup_storage_used|Armazenamento de backup usado|Bytes|Average|Armazenamento de backup usado|Sem dimensões|
|network_bytes_egress|Saída de Rede|Bytes|Total|Saída de Rede em conexões ativas|Sem dimensões|
|network_bytes_ingress|Entrada de Rede|Bytes|Total|Entrada de Rede em conexões ativas|Sem dimensões|
|pg_replica_log_delay_in_seconds|Retardo da Réplica|Segundos|Máximo|Atraso de réplica em segundos|Sem dimensões|
|pg_replica_log_delay_in_bytes|Retardo Máximo entre Réplicas|Bytes|Máximo|Atraso em bytes da réplica mais defasante|Sem dimensões|

## <a name="microsoftdbforpostgresqlserversv2"></a>Microsoft.DBforPostgreSQL/serversv2

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|cpu_percent|Porcentagem de CPU|Percent|Average|Porcentagem de CPU|Sem dimensões|
|memory_percent|Porcentagem de memória|Percent|Average|Porcentagem de memória|Sem dimensões|
|IOPS|IOPS|Count|Average|Operações de e/s por segundo|Sem dimensões|
|storage_percent|Porcentagem de armazenamento|Percent|Average|Porcentagem de armazenamento|Sem dimensões|
|storage_used|Armazenamento usado|Bytes|Average|Armazenamento usado|Sem dimensões|
|active_connections|Conexões ativas|Contagem|Average|Conexões ativas|Sem dimensões|
|network_bytes_egress|Saída de Rede|Bytes|Total|Saída de Rede em conexões ativas|Sem dimensões|
|network_bytes_ingress|Entrada de Rede|Bytes|Total|Entrada de Rede em conexões ativas|Sem dimensões|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Tentativas de envio de mensagem de telemetria|Count|Total|Número de tentativas de envio de mensagens de telemetria do dispositivo para nuvem para o hub IoT|Sem dimensões|
|d2c.telemetry.ingress.success|Mensagens de telemetria enviadas|Count|Total|Número de mensagens de telemetria do dispositivo para nuvem enviadas com êxito para o hub IoT|Sem dimensões|
|c2d.commands.egress.complete.success|Comandos concluídos|Count|Total|Número de comandos da nuvem para o dispositivo concluídos com sucesso pelo dispositivo|Sem dimensões|
|c2d.commands.egress.abandon.success|Comandos abandonados|Count|Total|Número de comandos da nuvem para o dispositivo abandonados pelo dispositivo|Sem dimensões|
|c2d.commands.egress.reject.success|Comandos rejeitados|Count|Total|Número de comandos da nuvem para o dispositivo rejeitados pelo dispositivo|Sem dimensões|
|devices.totalDevices|Total de dispositivos (preterido)|Count|Total|Número de dispositivos registrados para o seu hub IoT|Sem dimensões|
|devices.connectedDevices.allProtocol|Dispositivos conectados (preteridos) |Count|Total|Número de dispositivos registrados ao seu hub IoT|Sem dimensões|
|d2c.telemetry.egress.success|Roteamento: mensagens de telemetria entregues|Count|Total|O número de vezes que as mensagens foram entregues com êxito a todos os pontos de extremidade usando o roteamento do Hub IoT. Se uma mensagem é encaminhada para vários pontos de extremidade, esse valor aumenta em um para cada entrega bem-sucedida. Se uma mensagem é entregue ao mesmo ponto de extremidade várias vezes, esse valor aumenta em um para cada entrega bem-sucedida.|Sem dimensões|
|d2c.telemetry.egress.dropped|Roteamento: mensagens de telemetria removidas |Contagem|Total|O número de vezes que as mensagens foram removidas pelo roteamento do Hub IoT devido à inatividade dos pontos de extremidade. Esse valor não conta as mensagens entregues à rota de fallback, pois as mensagens removidas não são entregues.|Sem dimensões|
|d2c.telemetry.egress.orphaned|Roteamento: mensagens de telemetria órfãs |Count|Total|O número de vezes que as mensagens ficaram órfãos do roteamento do Hub IoT porque não correspondiam a nenhuma regra de roteamento (incluindo a regra de fallback). |Sem dimensões|
|d2c.telemetry.egress.invalid|Roteamento: mensagens de telemetria incompatíveis|Contagem|Total|O número de vezes que o roteamento do Hub IoT não conseguiu entregar mensagens devido a uma incompatibilidade com o ponto de extremidade. Esse valor não inclui novas tentativas.|Sem dimensões|
|d2c.telemetry.egress.fallback|Roteamento: mensagens entregues ao fallback|Contagem|Total|O número de vezes que o roteamento do Hub IoT entregou mensagens ao ponto de extremidade associado à rota de fallback.|Sem dimensões|
|d2c.endpoints.egress.eventHubs|Roteamento: mensagens entregues ao Hub de Eventos|Contagem|Total|O número de vezes que o roteamento do Hub IoT entregou com êxito mensagens aos pontos de extremidade do Hub de Eventos.|Sem dimensões|
|d2c.endpoints.latency.eventHubs|Roteamento: latência de mensagem para o Hub de Eventos|Milissegundos|Average|A latência média (milissegundos) entre a entrada da mensagem no Hub IoT e a entrada da mensagem em um ponto de extremidade do Hub de Eventos.|Sem dimensões|
|d2c.endpoints.egress.serviceBusQueues|Roteamento: mensagens entregues à Fila do Barramento de Serviço|Count|Total|O número de vezes que o roteamento do Hub IoT entregou com êxito mensagens aos pontos de extremidade da fila do Barramento de Serviço.|Sem dimensões|
|d2c.endpoints.latency.serviceBusQueues|Roteamento: latência de mensagem para a Fila do Barramento de Serviço|Milissegundos|Average|A latência média (milissegundos) entre a entrada da mensagem no Hub IoT e a entrada da mensagem de telemetria em um ponto de extremidade da fila do Barramento de Serviço.|Sem dimensões|
|d2c.endpoints.egress.serviceBusTopics|Roteamento: mensagens entregues ao Tópico do Barramento de Serviço|Count|Total|O número de vezes que o roteamento do Hub IoT entregou com êxito mensagens aos pontos de extremidade do tópico do Barramento de Serviço.|Sem dimensões|
|d2c.endpoints.latency.serviceBusTopics|Roteamento: latência de mensagem para o Tópico do Barramento de Serviço|Milissegundos|Average|A latência média (milissegundos) entre a entrada da mensagem no Hub IoT e a entrada da mensagem de telemetria em um ponto de extremidade do tópico do Barramento de Serviço.|Sem dimensões|
|d2c.endpoints.egress.builtIn.events|Roteamento: mensagens entregues a mensagens/eventos|Contagem|Total|O número de vezes que o roteamento do Hub IoT entregou com êxito mensagens ao ponto de extremidade interno (mensagens/eventos). Essa métrica só começa a funcionar quando o roteamento está https://aka.ms/iotrouting) habilitado (para o Hub IOT.|Sem dimensões|
|d2c.Endpoints.Latency.builtIn.Events|Roteamento: latência de mensagem para mensagens/eventos|Milissegundos|Average|A latência média (milissegundos) entre a entrada da mensagem no Hub IoT e a entrada da mensagem de telemetria no ponto de extremidade interno (mensagens/eventos). Essa métrica só começa a funcionar quando o roteamento está https://aka.ms/iotrouting) habilitado (para o Hub IOT.|Sem dimensões|
|d2c.endpoints.egress.storage|Roteamento: mensagens entregues ao armazenamento|Count|Total|O número de vezes que o roteamento do Hub IoT entregou com êxito mensagens aos pontos de extremidade de armazenamento.|Sem dimensões|
|d2c.endpoints.latency.storage|Roteamento: latência de mensagem para armazenamento|Milissegundos|Average|A latência média (milissegundos) entre a entrada da mensagem no Hub IoT e a entrada da mensagem de telemetria em um ponto de extremidade de armazenamento.|Sem dimensões|
|d2c.endpoints.egress.storage.bytes|Roteamento: dados entregues ao armazenamento|Bytes|Total|A quantidade de dados (bytes) de roteamento do Hub IoT entregue aos pontos de extremidade de armazenamento.|Sem dimensões|
|d2c.endpoints.egress.storage.blobs|Roteamento: blobs entregues ao armazenamento|Contagem|Total|O número de vezes que o roteamento do Hub IoT entregou blobs aos pontos de extremidade de armazenamento.|Sem dimensões|
|EventGridDeliveries|Entregas da grade de eventos (visualização)|Count|Total|O número de eventos do Hub IoT publicados na grade de eventos. Use a dimensão de resultado para o número de solicitações bem-sucedidas e com falha. A dimensão EventType mostra o tipo de evento https://aka.ms/ioteventgrid) (.|Resultado, EventType|
|EventGridLatency|A latência média (milissegundos) de quando o evento do Hub IOT foi gerado quando o evento foi publicado na grade de eventos. Esse número é uma média entre todos os tipos de evento. Use a dimensão EventType para ver a latência de um tipo específico de evento.|EventType|
|d2c.twin.read.success|Leituras de gêmeos dos dispositivos bem-sucedidas|Count|Total|A contagem de todas as leituras de gêmeos iniciadas pelo dispositivo bem-sucedidas.|Sem dimensões|
|d2c.twin.read.failure|Leituras de gêmeos dos dispositivos com falhas|Contagem|Total|A contagem de todas as leituras de gêmeos iniciadas pelo dispositivo com falhas.|Sem dimensões|
|d2c.twin.read.size|Tamanho da resposta das leituras de gêmeos dos dispositivos|Bytes|Average|A média, o mínimo e o máximo de todas as leituras de gêmeos iniciadas pelo dispositivo bem-sucedidas.|Sem dimensões|
|d2c.twin.update.success|Atualizações de gêmeos dos dispositivos bem-sucedidas|Count|Total|A contagem de todas as atualizações de gêmeos iniciadas pelo dispositivo bem-sucedidas.|Sem dimensões|
|d2c.twin.update.failure|Atualizações de gêmeos dos dispositivos com falhas|Count|Total|A contagem de todas as atualizações de gêmeos iniciadas pelo dispositivo.|Sem dimensões|
|d2c.twin.update.size|Tamanho das atualizações de gêmeos dos dispositivos|Bytes|Average|A média, o mínimo e o máximo de todas as atualizações de gêmeos iniciadas pelo dispositivo bem-sucedidas.|Sem dimensões|
|c2d.methods.success|Invocações de método diretas bem-sucedidas|Count|Total|A contagem de todas as chamadas de método diretas bem-sucedidas.|Sem dimensões|
|c2d.methods.failure|Invocações de método diretas com falhas|Count|Total|A contagem de todas as chamadas de método diretas com falhas.|Sem dimensões|
|c2d.methods.requestSize|Tamanho da solicitação das invocações de método diretas|Bytes|Average|A média, o mínimo e o máximo de todas as solicitações de método diretas bem-sucedidas.|Sem dimensões|
|c2d.methods.responseSize|Tamanho da resposta das invocações de método diretas|Bytes|Average|A média, o mínimo e o máximo de todas as respostas de método diretas bem-sucedidas.|Sem dimensões|
|c2d.twin.read.success|Leituras de gêmeos de back-end bem-sucedidas|Count|Total|A contagem de todas as leituras de gêmeos bem-sucedidas iniciadas pelo back-end.|Sem dimensões|
|c2d.twin.read.failure|Leituras de gêmeos de back-end com falhas|Count|Total|A contagem de todas as falhas de leituras de gêmeos iniciadas pelo back-end.|Sem dimensões|
|c2d.twin.read.size|Tamanho da resposta das leituras de gêmeos de back-end|Bytes|Average|A média, o mínimo e o máximo de todas as leituras de gêmeos bem-sucedidas iniciadas pelo back-end.|Sem dimensões|
|c2d.twin.update.success|Atualizações de gêmeos de back-end bem-sucedidas|Contagem|Total|A contagem de todas as atualizações de gêmeos iniciadas pelo back-end bem-sucedidas.|Sem dimensões|
|c2d.twin.update.failure|Atualizações de gêmeos de back-end com falhas|Count|Total|A contagem de todas as falhas de atualizações de gêmeos iniciadas pelo back-end.|Sem dimensões|
|c2d.twin.update.size|Tamanho das atualizações de gêmeos de back-end|Bytes|Average|A média, o mínimo e o máximo de todas as atualizações de gêmeos iniciadas pelo back-end bem-sucedidas.|Sem dimensões|
|twinQueries.success|Consultas de gêmeos bem-sucedidas|Count|Total|A contagem de todas as consultas de gêmeos bem-sucedidas.|Sem dimensões|
|twinQueries.failure|Consultas de gêmeos com falhas|Contagem|Total|A contagem de todas as consultas de gêmeos com falhas.|Sem dimensões|
|twinQueries.resultSize|Tamanho do resultado das consultas de gêmeos|Bytes|Average|A média, o mínimo e o máximo do tamanho do resultado de todas as consultas de gêmeos bem-sucedidas.|Sem dimensões|
|jobs.createTwinUpdateJob.success|Criações de trabalhos de atualização de gêmeos bem-sucedidas|Count|Total|A contagem de todos os trabalhos de criação de atualização de gêmeos bem-sucedidos.|Sem dimensões|
|jobs.createTwinUpdateJob.failure|Criações de trabalhos de atualização de gêmeos com falhas|Contagem|Total|A contagem de todos os trabalhos de criação de atualização de gêmeos com falhas.|Sem dimensões|
|jobs.createDirectMethodJob.success|Criações de trabalhos de invocação de método bem-sucedidas|Count|Total|A contagem de todas as criações de trabalhos de invocação de método direto bem-sucedidas.|Sem dimensões|
|jobs.createDirectMethodJob.failure|Criações de trabalhos de invocação de método com falhas|Count|Total|A contagem de todas as criações de trabalhos de invocação de método direto com falhas.|Sem dimensões|
|jobs.listJobs.success|Chamadas para listar trabalhos bem-sucedidas|Count|Total|A contagem de todas as chamadas para listar trabalhos bem-sucedidas.|Sem dimensões|
|jobs.listJobs.failure|Chamadas para listar trabalhos com falhas|Count|Total|A contagem de todas as chamadas para listar trabalhos com falhas.|Sem dimensões|
|jobs.cancelJob.success|Cancelamentos de trabalho bem-sucedidos|Contagem|Total|A contagem de todas as chamadas para cancelar um trabalho bem-sucedidas.|Sem dimensões|
|jobs.cancelJob.failure|Cancelamentos de trabalho com falhas|Count|Total|A contagem de todas as chamadas para cancelar um trabalho com falhas.|Sem dimensões|
|jobs.queryJobs.success|Consultas de trabalho bem-sucedidas|Count|Total|A contagem de todas as chamadas para consultas de trabalho bem-sucedidas.|Sem dimensões|
|jobs.queryJobs.failure|Consultas de trabalho com falhas|Contagem|Total|A contagem de todas as chamadas para consultas de trabalho com falhas.|Sem dimensões|
|jobs.completed|Trabalhos concluídos|Contagem|Total|A contagem de todos os trabalhos concluídos.|Sem dimensões|
|jobs.failed|Trabalhos com falha|Contagem|Total|A contagem de todos os trabalhos com falha.|Sem dimensões|
|d2c.telemetry.ingress.sendThrottle|Número de erros de limitação|Count|Total|Número de erros de limitação devido a limitações da taxa de transferência do dispositivo|Sem dimensões|
|dailyMessageQuotaUsed|Número total de mensagens usadas|Count|Average|Número total de mensagens utilizadas hoje. Este é um valor cumulativo que é reiniciado para zero em 00:00 UTC diariamente.|Sem dimensões|
|deviceDataUsage|Uso total de dados do dispositivo|Bytes|Total|Bytes transferidos entre todos os dispositivos conectados ao lotHub|Sem dimensões|
|totalDeviceCount|Total de dispositivos (versão prévia)|Count|Average|Número de dispositivos registrados para o seu hub IoT|Sem dimensões|
|connectedDeviceCount|Dispositivos conectados (versão prévia)|Count|Average|Número de dispositivos registrados ao seu hub IoT|Sem dimensões|
|configurações|Métricas de configuração|Contagem|Total|Métricas para operações de configuração|Sem dimensões|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|RegistrationAttempts|Tentativas de Registro|Count|Total|Número de tentativas de registros do dispositivo|ProvisioningServiceName, IotHubName, Status|
|DeviceAssignments|Dispositivos atribuídos|Count|Total|Número de dispositivos atribuídos a um hub IoT|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Tentativas de atestado|Count|Total|Número de tentativas de atestado do dispositivo|ProvisioningServiceName, Status, Protocol|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.DocumentDB/databaseAccounts

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|AvailableStorage|Armazenamento disponível|Bytes|Total|Armazenamento total disponível relatado à granularidade de 5 minutos|CollectionName, DatabaseName, região|
|CassandraConnectionClosures|Fechamentos de Conexão do Cassandra|Count|Total|Número de conexões Cassandra que foram fechadas, relatadas a uma granularidade de 1 minuto|Região, ClosureReason|
|CassandraRequestCharges|Encargos de Solicitação do Cassandra|Count|Total|RUs consumido para solicitações Cassandra feitas|DatabaseName, CollectionName, Region, OperationType, ResourceType|
|CassandraRequests|Solicitações do Cassandra|Count|Contagem|Número de solicitações Cassandra feitas|DatabaseName, CollectionName, região, OperationType, ResourceType, ErrorCode|
|DataUsage|Uso de dados|Bytes|Total|Uso total de dados relatados à granularidade de 5 minutos|CollectionName, DatabaseName, região|
|DocumentCount|Contagem de documentos|Contagem|Total|Contagem total de documentos relatados à granularidade de 5 minutos|CollectionName, DatabaseName, região|
|DocumentQuota|Cota de documento|Bytes|Total|Cota de armazenamento total relatada à granularidade de 5 minutos|CollectionName, DatabaseName, região|
|IndexUsage|Uso do índice|Bytes|Total|Uso total do índice relatado à granularidade de 5 minutos|CollectionName, DatabaseName, região|
|MetadataRequests|MetadataRequests|Count|Count|Contagem de solicitações de metadados. O Cosmos DB mantém uma coleção de metadados do sistema para cada conta, o que permite que você enumere coleções, bancos de dados, etc, e suas configurações, gratuitamente.|DatabaseName, CollectionName, região, StatusCode, |
|MongoRequestCharge|MongoRequestCharge|Count|Total|Unidades Solicitadas do Mongo Consumidas|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|MongoRequests|Solicitações do Mongo|Count|Contagem|Número de Solicitações do Mongo Feitas|DatabaseName, CollectionName, Region, CommandName, ErrorCode|
|ProvisionedThroughput|Taxa de transferência provisionada|Count|Máximo|Taxa de transferência provisionada|DatabaseName, CollectionName|
|ReplicationLatency|Latência de replicação P99|MilliSeconds|Average|Latência de replicação P99 nas regiões de origem e de destino para conta habilitada geograficamente|SourceRegion, TargetRegion|
|ServiceAvailability|Disponibilidade do serviço|Percent|Average|Disponibilidade de solicitações de conta em uma hora, granularidade de dia ou mês|Sem dimensões|
|TotalRequestUnits|Total de Unidades Solicitadas|Count|Total|Unidades Solicitadas Consumidas|DatabaseName, CollectionName, região, StatusCode, OperationType|
|TotalRequests|Total de Solicitações|Count|Count|Número de solicitações feitas|DatabaseName, CollectionName, região, StatusCode, OperationType|

## <a name="microsofteventgridtopics"></a>Microsoft.EventGrid/topics

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|PublishSuccessCount|Eventos publicados|Contagem|Total|Total de eventos publicados neste tópico|Sem dimensões|
|PublishFailCount|Publicar eventos com falha|Count|Total|Total de eventos com falha ao publicar neste tópico|ErrorType, Error|
|UnmatchedEventCount|Eventos sem correspondência|Count|Total|Total de eventos sem correspondência com nenhuma das assinaturas de evento para este tópico|Sem dimensões|
|PublishSuccessLatencyInMs|Latência de êxito na publicação|Count|Total|Latência de êxito de publicação em milissegundos|Sem dimensões|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft.EventGrid/eventSubscriptions

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|MatchedEventCount|Eventos Correspondentes|Contagem|Total|Total de eventos correspondentes a essa assinatura de evento|Sem dimensões|
|DeliveryAttemptFailCount|Eventos com falha de entrega|Count|Total|Total de eventos não entregues a essa assinatura de evento|Error, ErrorType|
|DeliverySuccessCount|Eventos entregues|Count|Total|Total de eventos entregues a essa assinatura de evento|Sem dimensões|
|DestinationProcessingDurationInMs|Duração do processamento de destino|Milissegundos|Average|Duração do processamento de destino em milissegundos|Sem dimensões|
|DroppedEventCount|Eventos removidos|Contagem|Total|Total de eventos removidos correspondentes a essa assinatura de evento|DropReason|
|DeadLetteredCount|Eventos de mensagens mortas|Count|Total|Total de eventos de mensagens mortas correspondentes a essa assinatura de evento|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft.EventGrid/extensionTopics

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|PublishSuccessCount|Eventos publicados|Contagem|Total|Total de eventos publicados neste tópico|Sem dimensões|
|PublishFailCount|Eventos com falha|Contagem|Total|Total de eventos com falha ao publicar neste tópico|ErrorType, Error|
|UnmatchedEventCount|Eventos sem correspondência|Count|Total|Total de eventos sem correspondência com nenhuma das assinaturas de evento para este tópico|Sem dimensões|
|PublishSuccessLatencyInMs|Latência de êxito na publicação|Count|Total|Latência de êxito de publicação em milissegundos|Sem dimensões|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|SuccessfulRequests|Solicitações com êxito|Count|Total|Solicitações Bem-sucedidas para Microsoft.EventHub.|EntityName, |
|ServerErrors|Erros do Servidor.|Count|Total|Erros do Servidor para o Microsoft.EventHub.|EntityName, |
|UserErrors|Erros de Usuário.|Contagem|Total|Erros de Usuário para o Microsoft.EventHub.|EntityName, |
|QuotaExceededErrors|Cota de Erros Excedida.|Count|Total|Cota de Erros Excedida para o Microsoft.EventHub.|EntityName, |
|ThrottledRequests|Solicitações Limitadas.|Count|Total|Solicitações Limitadas para o Microsoft.EventHub.|EntityName, |
|IncomingRequests|Solicitações de Entrada|Count|Total|Solicitações de Entrada para o Microsoft.EventHub.|EntityName|
|IncomingMessages|Mensagens de Entrada|Count|Total|Mensagens de Entrada para o Microsoft.EventHub.|EntityName|
|OutgoingMessages|Mensagens de Saída|Contagem|Total|Mensagens de Saída para o Microsoft.EventHub.|EntityName|
|IncomingBytes|Bytes de Entrada.|Bytes|Total|Bytes de Entrada para o Microsoft.EventHub.|EntityName|
|OutgoingBytes|Bytes de Saída.|Bytes|Total|Bytes de Saída para o Microsoft.EventHub.|EntityName|
|ActiveConnections|ActiveConnections|Contagem|Average|Total de Conexões Ativas para o Microsoft.EventHub.|Sem dimensões|
|ConnectionsOpened|Conexões Abertas.|Count|Average|Conexões Abertas para o Microsoft.EventHub.|EntityName|
|ConnectionsClosed|Conexões Fechadas.|Count|Average|Conexões Fechadas para o Microsoft.EventHub.|EntityName|
|CaptureBacklog|Lista de Pendências de Captura.|Count|Total|Lista de Pendências de Captura para o Microsoft.EventHub.|EntityName|
|CapturedMessages|Mensagens Capturadas.|Count|Total|Mensagens Capturadas para o Microsoft.EventHub.|EntityName|
|CapturedBytes|Bytes Capturados.|Bytes|Total|Bytes Capturados para o Microsoft.EventHub.|EntityName|
|Size|Size|Bytes|Average|Tamanho de um EventHub em Bytes.|EntityName|
|INREQS|Solicitações de entrada (preteridas)|Count|Total|Total de solicitações de envio de entrada para um namespace (preterido)|Sem dimensões|
|SUCCREQ|Solicitações com êxito (preteridas)|Count|Total|Total de solicitações bem-sucedidas para um namespace (preterido)|Sem dimensões|
|FAILREQ|Solicitações com falha (preteridas)|Contagem|Total|Total de solicitações com falha para um namespace (preterido)|Sem dimensões|
|SVRBSY|Erros do servidor ocupado (preterido)|Count|Total|Total de erros do servidor ocupado para um namespace (preterido)|Sem dimensões|
|INTERR|Erros de servidor interno (preterido)|Count|Total|Total de erros internos do servidor para um namespace (preterido)|Sem dimensões|
|MISCERR|Outros erros (preterido)|Count|Total|Total de solicitações com falha para um namespace (preterido)|Sem dimensões|
|INMSGS|Mensagens de entrada (preteridas) (preteridas)|Count|Total|Total de mensagens de entrada para um namespace. Essa métrica foi preterida. Use a métrica de mensagens de entrada em vez disso (preterido)|Sem dimensões|
|EHINMSGS|Mensagens de Entrada (Preterida)|Count|Total|Total de mensagens de entrada para um namespace (preterido)|Sem dimensões|
|OUTMSGS|Mensagens de saída (preteridas) (preteridas)|Count|Total|Total de mensagens de saída para um namespace. Essa métrica foi preterida. Use a métrica de mensagens de saída em vez disso (preterido)|Sem dimensões|
|EHOUTMSGS|Mensagens de Saída (Preterida)|Count|Total|Total de mensagens de saída para um namespace (preterido)|Sem dimensões|
|EHINMBS|Bytes de entrada (preterido) (preterido)|Bytes|Total|Taxa de transferência de mensagem de entrada do Hub de Eventos para um namespace. Essa métrica foi preterida. Use a métrica de bytes de entrada em vez disso (preterido)|Sem dimensões|
|EHINBYTES|Bytes de entrada (Preterida)|Bytes|Total|Taxa de transferência de mensagem de entrada do hub de eventos para um namespace (preterido)|Sem dimensões|
|EHOUTMBS|Bytes de saída (preteridos) (preterido)|Bytes|Total|Taxa de transferência de mensagem de saída do Hub de Eventos para um namespace. Essa métrica foi preterida. Use a métrica de bytes de saída em vez disso (preterido)|Sem dimensões|
|EHOUTBYTES|Bytes de saída (Preterida)|Bytes|Total|Taxa de transferência de mensagem de saída do hub de eventos para um namespace (preterido)|Sem dimensões|
|EHABL|Arquivar mensagens de pendências (preterido)|Contagem|Total|Mensagens de arquivamento do hub de eventos no backlog para um namespace (preterido)|Sem dimensões|
|EHAMSGS|Arquivar mensagens (preterido)|Count|Total|Mensagens arquivadas do hub de eventos em um namespace (preterido)|Sem dimensões|
|EHAMBS|Taxa de transferência de mensagem de arquivamento (preterido)|Bytes|Total|Taxa de transferência de mensagem arquivada do hub de eventos em um namespace (preterido)|Sem dimensões|

## <a name="microsofteventhubclusters"></a>Microsoft.EventHub/clusters

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|SuccessfulRequests|Solicitações Bem-sucedidas (Versão Prévia)|Count|Total|Solicitações Bem-sucedidas para Microsoft.EventHub. (Visualização)|Sem dimensões|
|ServerErrors|Erros do Servidor. (Visualização)|Count|Total|Erros do Servidor para o Microsoft.EventHub. (Visualização)|Sem dimensões|
|UserErrors|Erros de Usuário. (Visualização)|Count|Total|Erros de Usuário para o Microsoft.EventHub. (Visualização)|Sem dimensões|
|QuotaExceededErrors|Cota de Erros Excedida. (Visualização)|Count|Total|Cota de Erros Excedida para o Microsoft.EventHub. (Visualização)|Sem dimensões|
|ThrottledRequests|Solicitações Limitadas. (Visualização)|Count|Total|Solicitações Limitadas para o Microsoft.EventHub. (Visualização)|Sem dimensões|
|IncomingRequests|Solicitações de Entrada (Versão Prévia)|Contagem|Total|Solicitações de Entrada para o Microsoft.EventHub. (Visualização)|Sem dimensões|
|IncomingMessages|Mensagens de Entrada (Versão Prévia)|Count|Total|Mensagens de Entrada para o Microsoft.EventHub. (Visualização)|Sem dimensões|
|OutgoingMessages|Mensagens de Saída (Versão Prévia)|Count|Total|Mensagens de Saída para o Microsoft.EventHub. (Visualização)|Sem dimensões|
|IncomingBytes|Bytes de Entrada. (Visualização)|Bytes|Total|Bytes de Entrada para o Microsoft.EventHub. (Visualização)|Sem dimensões|
|OutgoingBytes|Bytes de Saída. (Visualização)|Bytes|Total|Bytes de Saída para o Microsoft.EventHub. (Visualização)|Sem dimensões|
|ActiveConnections|ActiveConnections (Versão Prévia)|Count|Average|Total de Conexões Ativas para o Microsoft.EventHub. (Visualização)|Sem dimensões|
|ConnectionsOpened|Conexões Abertas. (Visualização)|Count|Average|Conexões Abertas para o Microsoft.EventHub. (Visualização)|Sem dimensões|
|ConnectionsClosed|Conexões Fechadas. (Visualização)|Count|Average|Conexões Fechadas para o Microsoft.EventHub. (Visualização)|Sem dimensões|
|CaptureBacklog|Lista de Pendências de Captura. (Visualização)|Contagem|Total|Lista de Pendências de Captura para o Microsoft.EventHub. (Visualização)|Sem dimensões|
|CapturedMessages|Mensagens Capturadas. (Visualização)|Count|Total|Mensagens Capturadas para o Microsoft.EventHub. (Visualização)|Sem dimensões|
|CapturedBytes|Bytes Capturados. (Visualização)|Bytes|Total|Bytes Capturados para o Microsoft.EventHub. (Visualização)|Sem dimensões|
|CPU|CPU (versão prévia)|Percent|Máximo|Utilização da CPU para o Cluster do Hub de Eventos como um percentual|Role|
|AvailableMemory|Memória disponível (versão prévia)|Count|Máximo|Memória disponível para o Cluster do Hub de Eventos em bytes|Role|

## <a name="microsofthdinsightclusters"></a>Microsoft.HDInsight/clusters

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|GatewayRequests|Solicitações de gateway|Contagem|Total|Número de solicitações de gateway|ClusterDnsName, HttpStatus|
|CategorizedGatewayRequests|Solicitações de gateway categorizadas|Count|Total|Número de solicitações de gateway por categorias (1xx/2xx/3xx/4xx/5xx)|ClusterDnsName, HttpStatus|
|NumActiveWorkers|Número de trabalhadores ativos|Contagem|Máximo|Número de trabalhadores ativos|ClusterDnsName, Metricname|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|ObservedMetricValue|Valor de Métrica Observado|Count|Average|O valor calculado pelo dimensionamento automático quando executado|MetricTriggerSource|
|MetricThreshold|Limite de Métrica|Count|Average|O limite do dimensionamento automático configurado quando o dimensionamento automático foi executado.|MetricTriggerRule|
|ObservedCapacity|Capacidade Observada|Count|Average|A capacidade relatada para o dimensionamento automático quando ele foi executado.|Sem dimensões|
|ScaleActionsInitiated|Ações de Escala Iniciadas|Count|Total|A direção da operação de escala.|ScaleDirection|

## <a name="microsoftinsightscomponents"></a>Microsoft.Insights/Components

(Versão prévia pública)

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|availabilityResults/availabilityPercentage|Disponibilidade|Porcentagem|Average|Percentual de testes de disponibilidade concluídos com êxito|availabilityResult/nome, availabilityResult/local|
|availabilityResults/contagem|Testes de disponibilidade|Count|Count|Contagem de testes de disponibilidade|availabilityResult/name, availabilityResult/location, availabilityResult/success|
|availabilityResults/duration|Duração do teste de disponibilidade|MilliSeconds|Average|Duração do teste de disponibilidade|availabilityResult/name, availabilityResult/location, availabilityResult/success|
|browserTimings/networkDuration|Tempo de conexão de rede para carregamento da página|MilliSeconds|Average|Tempo entre a solicitação do usuário e a conexão de rede. Inclui a pesquisa DNS e a conexão de transporte.|Sem dimensões|
|browserTimings/processingDuration|Tempo de processamento do cliente|MilliSeconds|Average|Tempo entre o recebimento do último byte de um documento até que o DOM seja carregado. As solicitações assíncronas ainda podem estar sendo processadas.|Sem dimensões|
|browserTimings/receiveDuration|Recebendo tempo de resposta|MilliSeconds|Average|Tempo entre o primeiro e o último byte, ou até a desconexão.|Sem dimensões|
|browserTimings/sendDuration|Enviar tempo de solicitação|MilliSeconds|Average|Tempo entre a conexão de rede e o recebimento do primeiro byte.|Sem dimensões|
|browserTimings/totalDuration|Procurar o tempo de carregamento de página|MilliSeconds|Average|Tempo de solicitação do usuário até que o DOM, as imagens, os scripts e as folhas de estilo sejam carregados.|Sem dimensões|
|dependencies/count|Chamadas de dependência|Count|Contagem|Contagem de chamadas feitas pelo aplicativo a recursos externos.|dependency/type, dependency/performanceBucket, dependency/success, operation/synthetic, cloud/roleInstance, cloud/roleName|
|dependencies/duration|Duração da dependência|MilliSeconds|Average|Duração das chamadas feitas pelo aplicativo a recursos externos.|dependency/type, dependency/performanceBucket, dependency/success, operation/synthetic, cloud/roleInstance, cloud/roleName|
|dependencies/failed|Falhas de chamada de dependência|Contagem|Count|Contagem de chamadas de dependência com falha feitas pelo aplicativo a recursos externos.|dependency/type, dependency/performanceBucket, operation/synthetic, cloud/roleInstance, cloud/roleName|
|pageViews/count|Exibições de página|Contagem|Count|Contagem de exibições de página.|operation/synthetic|
|pageViews/duration|Temo de carregamento de exibição de página|MilliSeconds|Average|Temo de carregamento de exibição de página|operation/synthetic|
|performanceCounters/requestExecutionTime|Tempo de execução de solicitação HTTP|MilliSeconds|Average|Tempo de execução da solicitação mais recente.|cloud/roleInstance|
|performanceCounters/requestsInQueue|Solicitações HTTP na fila do aplicativo|Contagem|Average|Comprimento da fila de solicitação de aplicativo.|cloud/roleInstance|
|performanceCounters/requestsPerSecond|Taxa de solicitação HTTP|CountPerSecond|Average|Taxa de todas as solicitações para o aplicativo por segundo do ASP.NET.|cloud/roleInstance|
|performanceCounters/exceptionsPerSecond|Taxa de exceção|CountPerSecond|Average|Contagem de exceções com e sem tratamento relatadas para o Windows, incluindo exceções do .NET e exceções não gerenciadas convertidas em exceções do .NET.|cloud/roleInstance|
|performanceCounters/processIOBytesPerSecond|Taxa de E/S do processo|BytesPerSecond|Average|Total de bytes por segundo lidos e gravados em arquivos, rede e dispositivos.|cloud/roleInstance|
|performanceCounters/processCpuPercentage|CPU do Processo|Percent|Average|O percentual de tempo decorrido que todos os threads do processo usaram o processador para executar instruções. Isso pode variar entre 0 e 100. Essa métrica indica o desempenho do processo de w3wp isoladamente.|cloud/roleInstance|
|performanceCounters/processorCpuPercentage|Tempo do processador|Percent|Average|O percentual de tempo que o processador gasta em threads não ociosos.|cloud/roleInstance|
|performanceCounters/memoryAvailableBytes|Memória disponível|Bytes|Average|Memória física disponível imediatamente para alocação a um processo ou para uso do sistema.|cloud/roleInstance|
|performanceCounters/processPrivateBytes|Bytes privados do processo|Bytes|Average|Memória atribuída exclusivamente aos processos do aplicativo monitorado.|cloud/roleInstance|
|requests/duration|Tempo de resposta do servidor|MilliSeconds|Average|Tempo entre o recebimento de uma solicitação HTTP e a finalização do envio da resposta.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|requests/count|Solicitações do servidor|Count|Count|Contagem de solicitações HTTP concluídas.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|requests/failed|Solicitações com falha|Count|Count|Contagem de solicitações HTTP marcadas como com falha. Na maioria dos casos, essas são solicitações com um código de resposta >= 400 e diferente de 401.|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, cloud/roleName|
|solicitações/taxa|Taxa de solicitações do servidor|CountPerSecond|Average|Taxa de solicitações de servidor por segundo|request/performanceBucket, request/resultCode, operation/synthetic, cloud/roleInstance, request/success, cloud/roleName|
|exceptions/count|Exceções|Count|Contagem|Contagem combinada de todas as exceções não capturadas.|cloud/roleName, cloud/roleInstance, client/type|
|exceptions/browser|Exceções do navegador|Count|Count|Contagem de exceções não identificadas lançadas no navegador.|Sem dimensões|
|exceptions/server|Exceções do servidor|Count|Contagem|Contagem de exceções não capturadas geradas no aplicativo para servidores.|cloud/roleName, cloud/roleInstance|
|traces/count|Rastreamentos|Contagem|Contagem|Contagem do documento de rastreamento|trace/severityLevel, operation/synthetic, cloud/roleName, cloud/roleInstance|

## <a name="microsoftkeyvaultvaults"></a>Microsoft.KeyVault/vaults

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|ServiceApiHit|Total de resultados da API de serviço|Contagem|Contagem|Número total de ocorrências da API de serviço|ActivityType, ActivityName|
|ServiceApiLatency|Latência da API de Serviço Geral|Milissegundos|Average|Latência geral das solicitações da API de serviço|ActivityType, ActivityName, StatusCode|
|ServiceApiResult|Resultados da API de Serviço Total|Count|Count|Número total de resultados da API de serviço|ActivityType, ActivityName, StatusCode|

## <a name="microsoftkustoclusters"></a>Microsoft.Kusto/Clusters

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|CacheUtilization|Utilização do cache|Percent|Average|Nível de utilização no escopo do cluster|Nenhum|
|QueryDuration|Duração da consulta|Milissegundos|Average|Duração das consultas em segundos|Status da consulta|
|IngestionUtilization|Utilização da ingestão|Percent|Average|Taxa de slots de ingestão usados no cluster|Nenhum|
|KeepAlive|Keep Alive|Contagem|Average|A verificação de integridade indica que o cluster responde às consultas|Nenhum|
|IngestionVolumeInMB|Volume de ingestão (em MB)|Contagem|Total|Volume total de dados ingeridos no cluster (em MB)|Banco de Dados|
|IngestionLatencyInSeconds|Latência de ingestão (em segundos)|Segundos|Average|Tempo de ingestão da origem (por exemplo, mensagem no Hub de Eventos) para o cluster em segundos|Nenhum|
|EventProcessedForEventHubs|Eventos processados (para os Hubs de Eventos)|Count|Total|Número de eventos processados pelo cluster durante a ingestão do Hub de Eventos|Nenhum|
|IngestionResult|Resultados da ingestão|Count|Count|Número de operações de ingestão|Status|
|CPU|CPU|Percent|Average|Nível de utilização da CPU|Nenhum|
| ContinuousExportNumOfRecordsExported | Número de registros exportados na exportação contínua | Contagem | Total | Número de registros exportados para cada artefato de armazenamento gravado durante a operação de exportação  | Nenhum |
| ExportUtilization | Utilização da exportação | Percent | Máximo | Utilização da exportação | Nenhum |
| ContinuousExportPendingCount | Contagem de exportação contínua pendente | Count | Máximo | O número de trabalhos de exportação contínuos pendentes prontos para execução | Nenhum |
| ContinuousExportMaxLatenessMinutes | Minutos de atraso máximo de exportação contínua | Count | Máximo | O tempo máximo em minutos de todas as exportações contínuas que estão pendentes e prontas para execução | Nenhum |

## <a name="microsoftlocationbasedservicesaccounts"></a>Microsoft.LocationBasedServices/accounts

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|Uso|Uso|Count|Contagem|Contagem de chamadas à API|ApiCategory, ApiName|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|RunsStarted|Execuções iniciadas|Count|Total|Número de execuções fluxo de trabalho iniciadas.|Sem dimensões|
|RunsCompleted|Execuções concluídas|Contagem|Total|Número de execuções de fluxo de trabalho concluídas.|Sem dimensões|
|RunsSucceeded|Execuções bem sucedidas|Count|Total|Número de execuções de fluxo de trabalho bem sucedidas.|Sem dimensões|
|RunsFailed|Execuções com falha|Count|Total|Número de execuções de fluxo de trabalho com falha.|Sem dimensões|
|RunsCancelled|Execuções canceladas|Contagem|Total|Número de execuções de fluxo de trabalho canceladas.|Sem dimensões|
|RunLatency|Latência da execução|Segundos|Average|Latência das execuções de fluxo de trabalho concluídas.|Sem dimensões|
|RunSuccessLatency|Latência de execução bem sucedida|Segundos|Average|Latência de execuções de fluxo de trabalho bem sucedidas.|Sem dimensões|
|RunThrottledEvents|Eventos de restrição de execução|Count|Total|Número de eventos de restrição de ações ou gatilhos de fluxo de trabalho.|Sem dimensões|
|RunFailurePercentage|Porcentagem de falha de execução|Percent|Total|Porcentagem de execuções do fluxo de trabalho com falha.|Sem dimensões|
|ActionsStarted|Ações iniciadas |Count|Total|Número de ações de fluxo de trabalho iniciadas.|Sem dimensões|
|ActionsCompleted|Ações concluídas |Contagem|Total|Número de ações de fluxo de trabalho concluídas.|Sem dimensões|
|ActionsSucceeded|Ações bem sucedidas |Count|Total|Número de ações de fluxo de trabalho bem sucedidas.|Sem dimensões|
|ActionsFailed|Ações com falha|Count|Total|Número de ações de fluxo de trabalho com falha.|Sem dimensões|
|ActionsSkipped|Ações ignoradas |Contagem|Total|Número de ações de fluxo de trabalho ignoradas.|Sem dimensões|
|ActionLatency|Latência da ação |Segundos|Average|Latência das ações de fluxo de trabalho concluídas.|Sem dimensões|
|ActionSuccessLatency|Latência das ações bem sucedidas |Segundos|Average|Latência das ações de fluxo de trabalho bem sucedidas.|Sem dimensões|
|ActionThrottledEvents|Eventos de restrição de ações|Count|Total|Número de eventos de restrição de ações de fluxo de trabalho.|Sem dimensões|
|TriggersStarted|Gatilhos iniciados |Count|Total|Número de gatilhos de fluxo de trabalho iniciadas.|Sem dimensões|
|TriggersCompleted|Gatilhos concluídos |Contagem|Total|Número de gatilhos de fluxo de trabalho concluídos.|Sem dimensões|
|TriggersSucceeded|Gatilhos bem sucedidos |Contagem|Total|Número de gatilhos de fluxo de trabalho bem sucedidos.|Sem dimensões|
|TriggersFailed|Gatilhos com falha |Count|Total|Número de gatilhos de fluxo de trabalho com falha.|Sem dimensões|
|TriggersSkipped|Gatilhos ignorados|Contagem|Total|Número de gatilhos de fluxo de trabalho ignorados.|Sem dimensões|
|TriggersFired|Gatilhos acionados |Contagem|Total|Número de gatilhos de fluxo de trabalho acionados.|Sem dimensões|
|TriggerLatency|Latência do gatilho |Segundos|Average|Latência dos gatilhos de fluxo de trabalho concluídos.|Sem dimensões|
|TriggerFireLatency|Latência do gatilho acionado |Segundos|Average|Latência dos gatilhos de fluxo de trabalho acionados.|Sem dimensões|
|TriggerSuccessLatency|Latência do gatilho bem sucedido |Segundos|Average|Latência dos gatilhos de fluxo de trabalho bem sucedidos.|Sem dimensões|
|TriggerThrottledEvents|Eventos de restrição do gatilho|Count|Total|Número de eventos de restrição do gatilho de fluxo de trabalho.|Sem dimensões|
|BillableActionExecutions|Execuções de ação faturáveis|Count|Total|Número de execuções de ação de fluxo de trabalho sendo faturadas.|Sem dimensões|
|BillableTriggerExecutions|Execuções de gatilho faturáveis|Count|Total|Número de execuções de gatilho do fluxo de trabalho sendo cobradas.|Sem dimensões|
|TotalBillableExecutions|Total de execuções faturáveis|Contagem|Total|Número de execuções de fluxo de trabalho sendo faturadas.|Sem dimensões|
|BillingUsageNativeOperation|Uso de Cobrança para Execuções de Operação Nativa|Count|Total|Número de execuções de operação nativa sendo cobradas.|Sem dimensões|
|BillingUsageStandardConnector|Uso de Cobrança para Execuções de Conector Padrão|Count|Total|Número de execuções do conector padrão sendo cobradas.|Sem dimensões|
|BillingUsageStorageConsumption|Uso de Cobrança para Execuções de Consumo de Armazenamento|Count|Total|Número de execuções de consumo de armazenamento sendo cobradas.|Sem dimensões|
|BillingUsageNativeOperation|Uso de Cobrança para Execuções de Operação Nativa|Count|Total|Número de execuções de operação nativa sendo cobradas.|Sem dimensões|
|BillingUsageStandardConnector|Uso de Cobrança para Execuções de Conector Padrão|Count|Total|Número de execuções do conector padrão sendo cobradas.|Sem dimensões|
|BillingUsageStorageConsumption|Uso de Cobrança para Execuções de Consumo de Armazenamento|Count|Total|Número de execuções de consumo de armazenamento sendo cobradas.|Sem dimensões|

## <a name="microsoftlogicintegrationserviceenvironments"></a>Microsoft.Logic/integrationServiceEnvironments

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|RunsStarted|Execuções iniciadas|Count|Total|Número de execuções fluxo de trabalho iniciadas.|Sem dimensões|
|RunsCompleted|Execuções concluídas|Count|Total|Número de execuções de fluxo de trabalho concluídas.|Sem dimensões|
|RunsSucceeded|Execuções bem sucedidas|Count|Total|Número de execuções de fluxo de trabalho bem sucedidas.|Sem dimensões|
|RunsFailed|Execuções com falha|Contagem|Total|Número de execuções de fluxo de trabalho com falha.|Sem dimensões|
|RunsCancelled|Execuções canceladas|Contagem|Total|Número de execuções de fluxo de trabalho canceladas.|Sem dimensões|
|RunLatency|Latência da execução|Segundos|Average|Latência das execuções de fluxo de trabalho concluídas.|Sem dimensões|
|RunSuccessLatency|Latência de execução bem sucedida|Segundos|Average|Latência de execuções de fluxo de trabalho bem sucedidas.|Sem dimensões|
|RunThrottledEvents|Eventos de restrição de execução|Count|Total|Número de eventos de restrição de ações ou gatilhos de fluxo de trabalho.|Sem dimensões|
|RunStartThrottledEvents|Executar Eventos Limitados por Início|Count|Total|Número de eventos restritos de início de fluxo de trabalho.|Sem dimensões|
|RunFailurePercentage|Porcentagem de falha de execução|Percent|Total|Porcentagem de execuções do fluxo de trabalho com falha.|Sem dimensões|
|ActionsStarted|Ações iniciadas |Count|Total|Número de ações de fluxo de trabalho iniciadas.|Sem dimensões|
|ActionsCompleted|Ações concluídas |Count|Total|Número de ações de fluxo de trabalho concluídas.|Sem dimensões|
|ActionsSucceeded|Ações bem sucedidas |Count|Total|Número de ações de fluxo de trabalho bem sucedidas.|Sem dimensões|
|ActionsFailed|Ações com falha |Count|Total|Número de ações de fluxo de trabalho com falha.|Sem dimensões|
|ActionsSkipped|Ações ignoradas |Count|Total|Número de ações de fluxo de trabalho ignoradas.|Sem dimensões|
|ActionLatency|Latência da ação |Segundos|Average|Latência das ações de fluxo de trabalho concluídas.|Sem dimensões|
|ActionSuccessLatency|Latência das ações bem sucedidas |Segundos|Average|Latência das ações de fluxo de trabalho bem sucedidas.|Sem dimensões|
|ActionThrottledEvents|Eventos de restrição de ações|Count|Total|Número de eventos de restrição de ações de fluxo de trabalho.|Sem dimensões|
|TriggersStarted|Gatilhos iniciados |Count|Total|Número de gatilhos de fluxo de trabalho iniciadas.|Sem dimensões|
|TriggersCompleted|Gatilhos concluídos |Count|Total|Número de gatilhos de fluxo de trabalho concluídos.|Sem dimensões|
|TriggersSucceeded|Gatilhos bem sucedidos |Contagem|Total|Número de gatilhos de fluxo de trabalho bem sucedidos.|Sem dimensões|
|TriggersFailed|Gatilhos com falha |Count|Total|Número de gatilhos de fluxo de trabalho com falha.|Sem dimensões|
|TriggersSkipped|Gatilhos ignorados|Count|Total|Número de gatilhos de fluxo de trabalho ignorados.|Sem dimensões|
|TriggersFired|Gatilhos acionados |Count|Total|Número de gatilhos de fluxo de trabalho acionados.|Sem dimensões|
|TriggerLatency|Latência do gatilho |Segundos|Average|Latência dos gatilhos de fluxo de trabalho concluídos.|Sem dimensões|
|TriggerFireLatency|Latência do gatilho acionado |Segundos|Average|Latência dos gatilhos de fluxo de trabalho acionados.|Sem dimensões|
|TriggerSuccessLatency|Latência do gatilho bem sucedido |Segundos|Average|Latência dos gatilhos de fluxo de trabalho bem sucedidos.|Sem dimensões|
|TriggerThrottledEvents|Eventos de restrição do gatilho|Count|Total|Número de eventos de restrição do gatilho de fluxo de trabalho.|Sem dimensões|
|IntegrationServiceEnvironmentWorkflowProcessorUsage|Uso do processador de fluxo de trabalho para Ambiente de Serviço de Integração|Percent|Average|Uso do processador do fluxo de trabalho para o ambiente do serviço de integração.|Sem dimensões|
|IntegrationServiceEnvironmentWorkflowMemoryUsage|Uso de memória do fluxo de trabalho para Ambiente de Serviço de Integração|Percent|Average|Uso de memória do fluxo de trabalho para o ambiente do serviço de integração.|Sem dimensões|
|IntegrationServiceEnvironmentConnectorProcessorUsage|Uso do processador do conector para Ambiente de Serviço de Integração|Percent|Average|Uso do processador do conector para o ambiente do serviço de integração.|Sem dimensões|
|IntegrationServiceEnvironmentConnectorMemoryUsage|Uso de memória do conector para Ambiente de Serviço de Integração|Percent|Average|Uso de memória do conector para o ambiente do serviço de integração.|Sem dimensões|

## <a name="microsoftmachinelearningservicesworkspaces"></a>Microsoft.MachineLearningServices/workspaces

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|Execuções concluídas|Execuções concluídas|Contagem|Total|Número de execuções concluídas com êxito para este espaço de trabalho|Cenário|
|Execuções iniciadas|Execuções iniciadas|Count|Total|Número de execuções iniciadas para este espaço de trabalho|Cenário|
|Execuções com Falhas|Execuções com Falhas|Count|Total|Número de execuções com falha para este espaço de trabalho|Cenário|

## <a name="microsoftmapsaccounts"></a>Microsoft.Maps/accounts

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|Uso|Uso|Count|Contagem|Contagem de chamadas à API|ApiCategory, ApiName, ResultType, ResponseCode|
|Disponibilidade|Disponibilidade|Porcentagem|Average|Disponibilidade das APIs|ApiCategory, ApiName|

## <a name="microsoftnetappnetappaccountscapacitypoolsvolumes"></a>Microsoft.NetApp/netAppAccounts/capacityPools/Volumes

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|AverageOtherLatency|Outra latência média|ms/op|Average|Outra latência média (que não seja de leitura ou gravação) em milissegundos por operação|Sem dimensões|
|AverageReadLatency|Latência média de leitura|ms/op|Average|Latência média de leitura em milissegundos por operação|Sem dimensões|
|AverageTotalLatency|Latência média total|ms/op|Average|Latência média total em milissegundos por operação|Sem dimensões|
|AverageWriteLatency|Latência média de gravação|ms/op|Average|Latência média de gravação em milissegundos por operação|Sem dimensões|
|FilesystemOtherOps|Outras operações do sistema de arquivos|ops|Average|Número de outras operações do sistema de arquivos (que não sejam de leitura ou gravação)|Sem dimensões|
|FilesystemReadOps|Operações de leitura do sistema de arquivos|ops|Average|Número de operações de leitura do sistema de arquivos|Sem dimensões|
|FilesystemTotalOps|Total de operações do sistema de arquivos|ops|Average|Soma de todas as operações do sistema de arquivos|Sem dimensões|
|FilesystemWriteOps|Operações de gravação do sistema de arquivos|ops|Average|Número de operações de gravação do sistema de arquivos|Sem dimensões|
|IoBytesPerOtherOps|Bytes de E/S por outras operações|bytes/op|Average|Número de bytes de entrada/saída por outras operações (que não sejam de leitura ou gravação)|Sem dimensões|
|IoBytesPerReadOps|Bytes de E/S por operações de leitura|bytes/op|Average|Número de bytes de entrada/saída por operação de leitura|Sem dimensões|
|IoBytesPerTotalOps|Bytes de E/S por operação em todas as operações|bytes/op|Average|Soma de todas as operações de bytes de entrada/saída|Sem dimensões|
|IoBytesPerWriteOps|Bytes de E/S por operações de gravação|bytes/op|Average|Número de bytes de entrada/saída por operação de gravação|Sem dimensões|
|OtherIops|Outros IOPS|operações/segundo|Average|Outras operações de entrada/saída por segundo|Sem dimensões|
|OtherThroughput|Outras taxas de transferência|MBps|Average|Outras taxas de transferência (que não sejam de leitura ou gravação) em megabytes por segundo|Sem dimensões|
|ReadIops|IOPS de leitura|operações/segundo|Average|Operações de entrada/saída de leitura por segundo|Sem dimensões|
|ReadThroughput|Taxa de transferência de leitura|MBps|Average|Taxa de transferência de leitura em megabytes por segundo|Sem dimensões|
|TotalIops|IOPS total|operações/segundo|Average|Soma de todas as operações de entrada/saída por segundo|Sem dimensões|
|TotalThroughput|Taxa de transferência total|MBps|Average|Soma de toda a taxa de transferência em megabytes por segundo|Sem dimensões|
|VolumeAllocatedSize|Tamanho alocado do volume|bytes|Average|Tamanho alocado do volume (não os bytes reais usados)|Sem dimensões|
|VolumeLogicalSize|Tamanho do volume lógico|bytes|Average|Tamanho lógico do volume (bytes usados)|Sem dimensões|
|VolumeSnapshotSize|Tamanho do instantâneo de volume|bytes|Average|Tamanho de todos os instantâneos no volume|Sem dimensões|
|WriteIops|IOPS de gravação|operações/segundo|Average|Operações de entrada/saída de gravação por segundo|Sem dimensões|
|WriteThroughput|Taxa de transferência de gravação|MBps|Average|Taxa de transferência de gravação em megabytes por segundo|Sem dimensões|

## <a name="microsoftnetappnetappaccountscapacitypools"></a>Microsoft.NetApp/netAppAccounts/capacityPools

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|VolumePoolAllocatedSize|Tamanho alocado do pool de volume|bytes|Average|Tamanho alocado do pool (não os bytes reais usados)|Sem dimensões|
|VolumePoolAllocatedUsed|Pool de volume alocado usado|bytes|Average|Tamanho usado alocado do pool|Sem dimensões|
|VolumePoolTotalLogicalSize|Tamanho lógico total do pool de volume|bytes|Average|Soma do tamanho lógico de todos os volumes que pertencem ao pool|Sem dimensões|
|VolumePoolTotalSnapshotSize|Tamanho total de instantâneos do pool de volume|bytes|Average|Soma de todos os instantâneos no pool|Sem dimensões|

## <a name="microsoftnetworknetworkinterfaces"></a>Microsoft.Network/networkInterfaces

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|BytesSentRate|Bytes Enviados|Count|Total|Número de bytes enviados pela Interface de Rede|Sem dimensões|
|BytesReceivedRate|Bytes Recebidos|Count|Total|Número de bytes recebidos pela Interface de Rede|Sem dimensões|
|PacketsSentRate|Pacotes Enviados|Count|Total|Número de bytes recebidos pela Interface de Rede|Sem dimensões|
|PacketsReceivedRate|Pacotes Recebidos|Contagem|Total|Número de pacotes recebidos pela Interface de Rede|Sem dimensões|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|VipAvailability|Disponibilidade do Caminho de Dados|Count|Average|Disponibilidade média do caminho de dados do Balanceador de Carga por duração de tempo|FrontendIPAddress, FrontendPort|
|DipAvailability|Status de investigação de integridade|Count|Average|Status médio de investigação de integridade do Balanceador de Carga por duração de tempo|ProtocolType, BackendPort, FrontendIPAddress, FrontendPort, BackendIPAddress|
|ByteCount|Contagem de Bytes|Count|Total|Número total de Bytes transmitidos no período|FrontendIPAddress, FrontendPort, Direction|
|PacketCount|Contagem de Pacotes|Count|Total|Número total de Pacotes transmitidos no período|FrontendIPAddress, FrontendPort, Direction|
|SYNCount|Contagem de SYN|Count|Total|Número total de Pacotes de SYN transmitidos no período|FrontendIPAddress, FrontendPort, Direction|
|SnatConnectionCount|Contagem de Conexões SNAT|Count|Total|Número total de novas conexões SNAT criadas no período|FrontendIPAddress, BackendIPAddress, ConnectionState|
|AllocatedSnatPorts|Portas SNAT alocadas (versão prévia)|Contagem|Total|Número total de portas SNAT alocadas no período|FrontendIPAddress, BackendIPAddress, ProtocolType|
|UsedSnatPorts|Portas SNAT usadas (versão prévia)|Count|Total|Número total de portas SNAT usadas no período|FrontendIPAddress, BackendIPAddress, ProtocolType|

## <a name="microsoftnetworkdnszones"></a>Microsoft.Network/dnszones

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|QueryVolume|Volume de consulta|Contagem|Total|Número de consultas atendidas para uma zona DNS|Sem dimensões|
|RecordSetCount|Contagem de Conjuntos de Registros|Count|Máximo|Número de Conjuntos de Registros em uma zona DNS|Sem dimensões|
|RecordSetCapacityUtilization|Utilização de capacidade de Conjuntos de Registros|Percent|Máximo|Porcentagem de capacidade de Conjuntos de Registros utilizada por uma zona DNS|Sem dimensões|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|PacketsInDDoS|DDoS de pacotes de entrada|CountPerSecond|Máximo|DDoS de pacotes de entrada|Sem dimensões|
|PacketsDroppedDDoS|DDoS de pacotes de entrada removidos|CountPerSecond|Máximo|DDoS de pacotes de entrada removidos|Sem dimensões|
|PacketsForwardedDDoS|DDoS de pacotes de entrada encaminhados|CountPerSecond|Máximo|DDoS de pacotes de entrada encaminhados|Sem dimensões|
|TCPPacketsInDDoS|DDoS de pacotes TCP de entrada|CountPerSecond|Máximo|DDoS de pacotes TCP de entrada|Sem dimensões|
|TCPPacketsDroppedDDoS|DDoS de pacotes TCP de entrada removidos|CountPerSecond|Máximo|DDoS de pacotes TCP de entrada removidos|Sem dimensões|
|TCPPacketsForwardedDDoS|DDoS de pacotes TCP de entrada encaminhados|CountPerSecond|Máximo|DDoS de pacotes TCP de entrada encaminhados|Sem dimensões|
|UDPPacketsInDDoS|DDoS de pacotes UDP de entrada|CountPerSecond|Máximo|DDoS de pacotes UDP de entrada|Sem dimensões|
|UDPPacketsDroppedDDoS|DDoS de pacotes UDP de entrada removidos|CountPerSecond|Máximo|DDoS de pacotes UDP de entrada removidos|Sem dimensões|
|UDPPacketsForwardedDDoS|DDoS de pacotes UDP de entrada encaminhados|CountPerSecond|Máximo|DDoS de pacotes UDP de entrada encaminhados|Sem dimensões|
|BytesInDDoS|DDoS de bytes de entrada|BytesPerSecond|Máximo|DDoS de bytes de entrada|Sem dimensões|
|BytesDroppedDDoS|DDoS de bytes de entrada removidos|BytesPerSecond|Máximo|DDoS de bytes de entrada removidos|Sem dimensões|
|BytesForwardedDDoS|DDoS de bytes de entrada encaminhados|BytesPerSecond|Máximo|DDoS de bytes de entrada encaminhados|Sem dimensões|
|TCPBytesInDDoS|DDoS de bytes de TCP de entrada|BytesPerSecond|Máximo|DDoS de bytes de TCP de entrada|Sem dimensões|
|TCPBytesDroppedDDoS|DDoS de bytes de TCP de entrada removidos|BytesPerSecond|Máximo|DDoS de bytes de TCP de entrada removidos|Sem dimensões|
|TCPBytesForwardedDDoS|DDoS de bytes de TCP de entrada encaminhados|BytesPerSecond|Máximo|DDoS de bytes de TCP de entrada encaminhados|Sem dimensões|
|UDPBytesInDDoS|DDoS de bytes de UDP de entrada|BytesPerSecond|Máximo|DDoS de bytes de UDP de entrada|Sem dimensões|
|UDPBytesDroppedDDoS|DDoS de bytes de UDP de entrada removidos|BytesPerSecond|Máximo|DDoS de bytes de UDP de entrada removidos|Sem dimensões|
|UDPBytesForwardedDDoS|DDoS de bytes de UDP de entrada encaminhados|BytesPerSecond|Máximo|DDoS de bytes de UDP de entrada encaminhados|Sem dimensões|
|IfUnderDDoSAttack|Sob ataque DDoS ou não|Count|Máximo|Sob ataque DDoS ou não|Sem dimensões|
|DDoSTriggerTCPPackets|Pacotes TCP de entrada a disparar a mitigação de DDoS|CountPerSecond|Máximo|Pacotes TCP de entrada a disparar a mitigação de DDoS|Sem dimensões|
|DDoSTriggerUDPPackets|Pacotes UDP de entrada a disparar a mitigação de DDoS|CountPerSecond|Máximo|Pacotes UDP de entrada a disparar a mitigação de DDoS|Sem dimensões|
|DDoSTriggerSYNPackets|Pacotes de entrada SYN para disparar a mitigação de DDoS|CountPerSecond|Máximo|Pacotes de entrada SYN para disparar a mitigação de DDoS|Sem dimensões|
|VipAvailability|Disponibilidade do Caminho de Dados|Count|Average|Disponibilidade média de Endereço IP por duração de tempo|Porta|
|ByteCount|Contagem de Bytes|Count|Total|Número total de Bytes transmitidos no período|Port, Direction|
|PacketCount|Contagem de Pacotes|Count|Total|Número total de Pacotes transmitidos no período|Port, Direction|
|SynCount|Contagem de SYN|Count|Total|Número total de Pacotes de SYN transmitidos no período|Port, Direction|

## <a name="microsoftnetworkazurefirewalls"></a>Microsoft.Network/azurefirewalls

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|ApplicationRuleHit|Contagem de impacto de regras de aplicativo|Contagem|Total|Número de vezes que as regras de aplicativo foram atingidas|Status, motivo, protocolo|
|NetworkRuleHit|Contagem de impacto das regras de rede|Contagem|Total|Número de vezes que as regras de rede foram atingidas|Status, motivo, protocolo|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|Taxa de transferência|Taxa de transferência|BytesPerSecond|Total|Número de bytes por segundo atendidos pelo Gateway de Aplicativo|Sem dimensões|
|UnhealthyHostCount|Contagem de hosts não íntegros|Contagem|Average|Número de hosts de back-end não íntegros|BackendSettingsPool|
|HealthyHostCount|Contagem de hosts íntegros|Count|Average|Número de hosts de back-end íntegros|BackendSettingsPool|
|TotalRequests|Total de Solicitações|Contagem|Total|Contagem de solicitações bem sucedidas atendidas pelo Gateway de Aplicativo|BackendSettingsPool|
|FailedRequests|Solicitações com Falha|Contagem|Total|Contagem de solicitações com falha atendidas pelo Gateway de Aplicativo|BackendSettingsPool|
|ResponseStatus|Status da Resposta|Count|Total|Status de resposta HTTP retornado pelo Gateway de Aplicativo|HttpStatusGroup|
|CurrentConnections|Conexões atuais|Count|Total|Contagem de conexões atuais estabelecidas com o Gateway de Aplicativo|Sem dimensões|
|CapacityUnits|Unidades de capacidade atuais|Contagem|Average|Unidades de capacidade consumidas|Sem dimensões|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|AverageBandwidth|Largura de banda de S2S do gateway|BytesPerSecond|Average|Largura de banda média site a site de um gateway em bytes por segundo|Sem dimensões|
|P2SBandwidth|Largura de banda de P2S do gateway|BytesPerSecond|Average|Largura de banda média ponto a site de um gateway em bytes por segundo|Sem dimensões|
|P2SConnectionCount|Contagem de conexões P2S|Contagem|Máximo|Contagem de conexões ponto a site de um gateway|Protocol|
|TunnelAverageBandwidth|Largura de Banda de Túnel|BytesPerSecond|Average|Largura de banda média de um túnel em bytes por segundo|ConnectionName, RemoteIP|
|TunnelEgressBytes|Bytes de Saída de Túnel|Bytes|Total|Bytes de saída de um túnel|ConnectionName, RemoteIP|
|TunnelIngressBytes|Bytes de Entrada de Túnel|Bytes|Total|Bytes de entrada de um túnel|ConnectionName, RemoteIP|
|TunnelEgressPackets|Pacotes de Saída de Túnel|Contagem|Total|Contagem de pacotes de saída de um túnel|ConnectionName, RemoteIP|
|TunnelIngressPackets|Pacotes de Entrada de Túnel|Count|Total|Contagem de pacotes de entrada de um túnel|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Remoção de Pacotes Incompatíveis de TS de Saída de Túnel|Count|Total|Contagem de remoção de pacotes de saída da incompatibilidade do seletor de tráfego de um túnel|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Remoção de Pacotes Incompatíveis de TS de Entrada de Túnel|Contagem|Total|Contagem de remoção de pacotes de entrada da incompatibilidade do seletor de tráfego de um túnel|ConnectionName, RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Bits de entrada no Azure por segundo|PeeringType|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Bits de saída no Azure por segundo|PeeringType|

## <a name="microsoftnetworkexpressroutecircuitspeerings"></a>Microsoft.Network/expressRouteCircuits/peerings

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Bits de entrada no Azure por segundo|Sem dimensões|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Bits de saída no Azure por segundo|Sem dimensões|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/connections

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Average|Bits de entrada no Azure por segundo|Sem dimensões|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Average|Bits de saída no Azure por segundo|Sem dimensões|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|QpsByEndpoint|Consultas pelo Ponto de Extremidade Retornado|Count|Total|Número de vezes que um ponto de extremidade do Gerenciador de Tráfego foi retornado no período determinado|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Status do Ponto de Extremidade por Ponto de Extremidade|Count|Máximo|1 se o status de investigação de um ponto de extremidade for “Habilitado”; caso contrário, 0.|EndpointName|

## <a name="microsoftnetworknetworkwatchersconnectionmonitors"></a>Microsoft.Network/networkWatchers/connectionMonitors

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|ProbesFailedPercent|Porcentagem de investigações com falha|Percent|Average|Porcentagem de investigações de monitoramento de conectividade com falha|Sem dimensões|
|AverageRoundtripMs|Média Tempo de ida e volta (ms)|MilliSeconds|Average|Tempo ida e volta de rede médio (ms) para investigações de monitoramento de conectividade enviadas entre a origem e o destino|Sem dimensões|

## <a name="microsoftnetworkfrontdoors"></a>Microsoft.Network/frontdoors

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|RequestCount|Contagem de solicitações|Count|Total|O número de solicitações de cliente atendidas pelo proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|RequestSize|Tamanho da solicitação|Bytes|Total|O número de bytes enviados como solicitações de clientes ao proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|ResponseSize|Tamanho da resposta|Bytes|Total|O número de bytes enviados como respostas do proxy HTTP/S para clientes|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendRequestCount|Contagem de solicitações de back-end|Contagem|Total|O número de solicitações enviadas do proxy HTTP/S aos back-ends|HttpStatus, HttpStatusGroup, Backend|
|BackendRequestLatency|Latência de solicitação de back-end|MilliSeconds|Average|O tempo calculado de quando a solicitação foi enviada pelo proxy HTTP/S ao back-end até o proxy HTTP/S receber o último byte de resposta do back-end|Back-end|
|TotalLatency|Latência total|MilliSeconds|Average|O tempo calculado de quando a solicitação do cliente foi recebida pelo proxy HTTP/S até o cliente confirmar o último byte de resposta do proxy HTTP/S|HttpStatus, HttpStatusGroup, ClientRegion, ClientCountry|
|BackendHealthPercentage|Percentual de integridade do back-end|Percent|Average|O percentual de investigações de integridade bem-sucedidas do proxy HTTP/S aos back-ends|Backend, BackendPool|
|WebApplicationFirewallRequestCount|Contagem de solicitações do Firewall de Aplicativo Web|Count|Total|O número de solicitações de cliente processadas pelo Firewall do Aplicativo Web|PolicyName, RuleName, Action|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|registration.all|Operações de registro|Count|Total|A contagem de todas as operações de registro bem-sucedidas (consultas de atualizações de criações e exclusões). |Sem dimensões|
|registration.create|Operação de criação de registro|Count|Total|A contagem de todas as criações de registro bem-sucedidas.|Sem dimensões|
|registration.update|Operação de atualização de registro|Count|Total|A contagem de todas as atualizações de registro bem-sucedidas.|Sem dimensões|
|registration.get|Operações de leitura do registro|Count|Total|A contagem de todas as consultas de registro bem-sucedidas.|Sem dimensões|
|registration.delete|Operação de exclusão de registro|Count|Total|A contagem de todas as exclusões de registro bem-sucedidas.|Sem dimensões|
|incoming|Mensagens de Entrada|Count|Total|A contagem de todas as chamadas de API enviadas com sucesso. |Sem dimensões|
|incoming.scheduled|Notificações por push agendadas enviadas|Contagem|Total|Notificações por push agendadas canceladas|Sem dimensões|
|incoming.scheduled.cancel|Notificações por push agendadas canceladas|Count|Total|Notificações por push agendadas canceladas|Sem dimensões|
|scheduled.pending|Notificações agendadas pendentes|Count|Total|Notificações Push Pendentes|Sem dimensões|
|installation.all|Operações de gerenciamento de instalação|Count|Total|Operações de Gerenciamento de Instalação|Sem dimensões|
|installation.get|Obter as Operações de Instalação|Count|Total|Obter as Operações de Instalação|Sem dimensões|
|installation.upsert|Criar ou atualizar operações de instalação|Count|Total|Operações de Criação ou Atualização de Instalação|Sem dimensões|
|installation.patch|Operações de Instalação de Patch|Count|Total|Operações de Instalação de Patch|Sem dimensões|
|installation.delete|Excluir operações de instalação|Count|Total|Operações de Exclusão de Instalação|Sem dimensões|
|outgoing.allpns.success|Notificações de sucesso|Count|Total|A contagem de todas as notificações bem-sucedidas.|Sem dimensões|
|outgoing.allpns.invalidpayload|Erros de conteúdo|Count|Total|A contagem de pushes que falharam porque o PNS retornou um erro de conteúdo inválido.|Sem dimensões|
|outgoing.allpns.pnserror|Erros de sistema de notificação externa|Count|Total|A contagem de pushes que falharam porque houve um problema na comunicação com o PNS (exclui problemas de autenticação).|Sem dimensões|
|outgoing.allpns.channelerror|Erros de canal|Contagem|Total|A contagem de pushes que falharam porque o canal era inválido, não associado com o aplicativo correto, restrito ou expirado.|Sem dimensões|
|outgoing.allpns.badorexpiredchannel|Erros de canal incorreto ou expirado|Contagem|Total|A contagem de pushes que falharam porque o canal/token/registrationId do registro estava expirado ou inválido.|Sem dimensões|
|outgoing.wns.success|Notificações do WNS bem-sucedidas|Count|Total|A contagem de todas as notificações bem-sucedidas.|Sem dimensões|
|outgoing.wns.invalidcredentials|Erros de autorização do WNS (credenciais inválidas)|Count|Total|A contagem de pushes que falharam porque o PNS não aceitou as credenciais fornecidas ou as credenciais estão bloqueadas. (O Windows Live não reconhece as credenciais).|Sem dimensões|
|outgoing.wns.badchannel|Erro de canal inválido do WNS|Count|Total|A contagem de pushes que falharam porque o ChannelURI do registro não foi reconhecido (status do WNS: 404 não encontrado).|Sem dimensões|
|outgoing.wns.expiredchannel|Erro de canal expirado do WNS|Count|Total|A contagem de pushes que falharam porque o ChannelURI está expirado (status do WNS: 410 não existe mais).|Sem dimensões|
|outgoing.wns.throttled|Notificações restritas do WNS|Count|Total|A contagem de pushes que falharam porque o WNS está limitando este aplicativo (status do WNS: 406 não aceitável).|Sem dimensões|
|outgoing.wns.tokenproviderunreachable|Erros de autorização do WNS (inacessível)|Count|Total|O Windows Live não está acessível.|Sem dimensões|
|outgoing.wns.invalidtoken|Erros de autorização do WNS (token inválido)|Count|Total|O token fornecido ao WNS não é válido (status do WNS: 401 não autorizado).|Sem dimensões|
|outgoing.wns.wrongtoken|Erros de autorização do WNS (token incorreto)|Contagem|Total|O token fornecido ao WNS é válido, mas para outro aplicativo (status do WNS: 403 proibido). Isso pode acontecer se o ChannelURI do registro estiver associado a outro aplicativo. Verifique se o aplicativo cliente está associado ao mesmo aplicativo cujas credenciais estão no hub de notificação.|Sem dimensões|
|outgoing.wns.invalidnotificationformat|Formato de notificação inválido do WNS|Count|Total|O formato da notificação é inválido (status do WNS: 400). Observe que o WNS não rejeita todos os conteúdos inválidos.|Sem dimensões|
|outgoing.wns.invalidnotificationsize|Erro de tamanho de notificação inválido do WNS|Count|Total|O conteúdo de notificação é muito grande (status do WNS: 413).|Sem dimensões|
|outgoing.wns.channelthrottled|Canal do WNS restrito|Contagem|Total|A notificação foi descartada porque o ChannelURI no registro está restrito (cabeçalho de resposta do WNS: X-WNS-NotificationStatus:channelThrottled).|Sem dimensões|
|outgoing.wns.channeldisconnected|Canal do WNS desconectado|Count|Total|A notificação foi descartada porque o ChannelURI no registro está restrito (cabeçalho de resposta do WNS: X-WNS-DeviceConnectionStatus: desconectado).|Sem dimensões|
|outgoing.wns.dropped|Notificações do WNS descartadas|Contagem|Total|A notificação foi descartada porque o ChannelURI do registro está restrito (X-WNS-NotificationStatus: dropped but not X-WNS-DeviceConnectionStatus: disconnected).|Sem dimensões|
|outgoing.wns.pnserror|Erros do WNS|Count|Total|Notificação não entregue devido a erros de comunicação com o WNS.|Sem dimensões|
|outgoing.wns.authenticationerror|Erros de autenticação do WNS|Count|Total|Notificação não entregue devido a erros de comunicação com o Windows Live, credenciais inválidas ou token incorreto.|Sem dimensões|
|outgoing.apns.success|Notificações do APNS bem-sucedidas|Contagem|Total|A contagem de todas as notificações bem-sucedidas.|Sem dimensões|
|outgoing.apns.invalidcredentials|Erros de autorização do APNS|Count|Total|A contagem de pushes que falharam porque o PNS não aceitou as credenciais fornecidas ou as credenciais estão bloqueadas.|Sem dimensões|
|outgoing.apns.badchannel|Erro de canal inválido do APNS|Count|Total|A contagem de Pushes que falharam porque o token é inválido (código de status do protocolo binário APNS: 8. Código de status do protocolo HTTP APNS: 400 com "BadDeviceToken").|Sem dimensões|
|outgoing.apns.expiredchannel|Erro de canal expirado do APNS|Count|Total|A contagem de tokens que foram invalidados pelo canal de comentários do APNS.|Sem dimensões|
|outgoing.apns.invalidnotificationsize|Erro de tamanho de notificação inválido do APNS|Count|Total|A contagem de Pushes que falharam porque o conteúdo era muito grande (código de status do protocolo binário APNS: 7).|Sem dimensões|
|outgoing.apns.pnserror|Erros do APNS|Count|Total|A contagem de pushes que falharam devido a erros de comunicação com o APNS.|Sem dimensões|
|outgoing.gcm.success|Notificações do GCM bem-sucedidas|Count|Total|A contagem de todas as notificações bem-sucedidas.|Sem dimensões|
|outgoing.gcm.invalidcredentials|Erros de autorização do GCM (credenciais inválidas)|Contagem|Total|A contagem de pushes que falharam porque o PNS não aceitou as credenciais fornecidas ou as credenciais estão bloqueadas.|Sem dimensões|
|outgoing.gcm.badchannel|Erro de canal inválido do GCM|Count|Total|A contagem de pushes que falharam porque o registrationId do registro não foi reconhecido (resultado GCM: registo inválido).|Sem dimensões|
|outgoing.gcm.expiredchannel|Erro de canal expirado do GCM|Count|Total|A contagem de pushes que falharam porque o registrationId do registro estava expirado (resultado GCM: NotRegistered).|Sem dimensões|
|outgoing.gcm.throttled|Notificações restritas do GCM|Contagem|Total|A contagem de pushes que falharam porque o GCM restringiu este aplicativo (código de status do GCM: 501-599 ou result:Unavailable).|Sem dimensões|
|outgoing.gcm.invalidnotificationformat|Formato de notificação inválido do GCM|Contagem|Total|A contagem de pushes que falharam porque o conteúdo não foi formatado corretamente (resultado do GCM: InvalidDataKey ou InvalidTtl).|Sem dimensões|
|outgoing.gcm.invalidnotificationsize|Erro de tamanho de notificação inválido do GCM|Contagem|Total|A contagem de pushes que falharam porque o conteúdo era muito grande (resultado do GCM: MessageTooBig).|Sem dimensões|
|outgoing.gcm.wrongchannel|Erro de canal incorreto do GCM|Count|Total|A contagem de pushes que falharam porque o registrationId do registro não está associado ao aplicativo atual (resultado do GCM: InvalidPackageName).|Sem dimensões|
|outgoing.gcm.pnserror|Erros do GCM|Count|Total|A contagem de pushes que falharam devido a erros de comunicação com o GCM.|Sem dimensões|
|outgoing.gcm.authenticationerror|Erros de autenticação do GCM|Count|Total|A contagem de pushes que falharam porque o PNS não aceitou as credenciais fornecidas, as credenciais estão bloqueadas ou o SenderId não está configurado corretamente no aplicativo (resultado do GCM: MismatchedSenderId).|Sem dimensões|
|outgoing.mpns.success|Notificações de sucesso do MPNS|Count|Total|A contagem de todas as notificações bem-sucedidas.|Sem dimensões|
|outgoing.mpns.invalidcredentials|Credenciais inválidas do MPNS|Count|Total|A contagem de pushes que falharam porque o PNS não aceitou as credenciais fornecidas ou as credenciais estão bloqueadas.|Sem dimensões|
|outgoing.mpns.badchannel|Erro de canal inválido do MPNS|Count|Total|A contagem de pushes que falharam porque o ChannelURI do registro não foi reconhecido (status do MPNS: 404 não encontrado).|Sem dimensões|
|outgoing.mpns.throttled|Notificações restritas do MPNS|Count|Total|A contagem de pushes que falharam porque o MPNS está limitando este aplicativo (WNS MPNS: 406 não aceitável).|Sem dimensões|
|outgoing.mpns.invalidnotificationformat|Erro de tamanho de notificação inválido do MPNS|Count|Total|A contagem de pushes que falharam porque o conteúdo da notificação era muito grande.|Sem dimensões|
|outgoing.mpns.channeldisconnected|Canal do MPNS desconectado|Contagem|Total|A contagem de pushes que falharam porque o ChannelURI do registro foi desconectado (status do MPNS: 412 não encontrado).|Sem dimensões|
|outgoing.mpns.dropped|Notificações descartadas do MPNS|Count|Total|A contagem de pushes que foram descartados pelo MPNS (cabeçalho de resposta do MPNS: X-NotificationStatus: QueueFull ou Suprimido).|Sem dimensões|
|outgoing.mpns.pnserror|Erros do MPNS|Contagem|Total|A contagem de pushes que falharam devido a erros de comunicação com o MPNS.|Sem dimensões|
|outgoing.mpns.authenticationerror|Erros de autenticação do MPNS|Contagem|Total|A contagem de pushes que falharam porque o PNS não aceitou as credenciais fornecidas ou as credenciais estão bloqueadas.|Sem dimensões|
|notificationhub.pushes|Todas as notificações de saída|Contagem|Total|Todas as notificações de saída do hub de notificação|Sem dimensões|
|incoming.all.requests|Todas as solicitações recebidas|Contagem|Total|Total de solicitações recebidas para um hub de notificação|Sem dimensões|
|incoming.all.failedrequests|Todas as solicitações com falha recebidas|Count|Total|Total de solicitações com falha recebidas para um hub de notificação|Sem dimensões|

## <a name="microsoftoperationalinsightsworkspaces"></a>Microsoft.OperationalInsights/workspaces

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|Average_% Free Inodes|% de Inodes livres|Count|Average|Average_% Free Inodes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Free Space|% de Espaço Livre|Count|Average|Average_% Free Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Inodes|% de Inodes Usados|Count|Average|Average_% Used Inodes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Space|% de Espaço Usado|Count|Average|Average_% Used Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Read Bytes/sec|Bytes/s Lidos em Disco|Count|Average|Average_Disk Read Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Reads/sec|Leituras de Disco/s|Contagem|Average|Average_Disk Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Transfers/sec|Transferências de Disco/s|Contagem|Average|Average_Disk Transfers/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Write Bytes/sec|Bytes/s Gravados em Disco|Count|Average|Average_Disk Write Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Writes/sec|Gravações de Disco/s|Contagem|Average|Average_Disk Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Megabytes|Megabytes Livres|Count|Average|Average_Free Megabytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Logical Disk Bytes/sec|Bytes de Disco Lógico/s|Count|Average|Average_Logical Disk Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Available Memory|% de Memória Disponível|Count|Average|Average_% Available Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Available Swap Space|% de Espaço de Permuta Disponível|Count|Average|Average_% Available Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Memory|% de Memória Usada|Count|Average|Average_% Used Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Used Swap Space|% de Espaço de Permuta Usado|Contagem|Average|Average_% Used Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes Memory|MBytes de Memória Disponíveis|Count|Average|Average_Available MBytes Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes Swap|MBytes de Espaço de Permuta Disponíveis|Count|Average|Average_Available MBytes Swap|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page Reads/sec|Leituras de Página/s|Count|Average|Average_Page Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Page Writes/sec|Gravações de Página/s|Contagem|Average|Average_Page Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pages/sec|Páginas/s|Contagem|Average|Average_Pages/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used MBytes Swap Space|MBytes de Espaço de Permuta Usado|Contagem|Average|Average_Used MBytes Swap Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used Memory MBytes|MBytes de Memória Usada|Contagem|Average|Average_Used Memory MBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes Transmitted|Total de Bytes Transmitidos|Count|Average|Average_Total Bytes Transmitted|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes Received|Total de Bytes Recebidos|Contagem|Average|Average_Total Bytes Received|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Bytes|Total de Bytes|Count|Average|Average_Total Bytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Packets Transmitted|Total de Pacotes Transmitidos|Count|Average|Average_Total Packets Transmitted|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Packets Received|Total de Pacotes Recebidos|Contagem|Average|Average_Total Packets Received|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Rx Errors|Total de Erros de Rx|Contagem|Average|Average_Total Rx Errors|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Tx Errors|Total de Erros de Tx|Count|Average|Average_Total Tx Errors|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Total Collisions|Total de Colisões|Count|Average|Average_Total Collisions|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. de segundos/Leitura do Disco|Média de segundos/Leitura do Disco|Count|Average|Average_Avg. de segundos/Leitura do Disco|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. de segundos/Transferência do Disco|Média de segundos/Transferência do Disco|Contagem|Average|Average_Avg. de segundos/Transferência do Disco|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. de segundos/Gravação do Disco|Média de segundos/Gravação do Disco|Count|Average|Average_Avg. de segundos/Gravação do Disco|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Physical Disk Bytes/sec|Bytes/s do Disco Físico|Contagem|Average|Average_Physical Disk Bytes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pct Privileged Time|% de Tempo Privilegiado|Contagem|Average|Average_Pct Privileged Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Pct User Time|% de Tempo do Usuário|Count|Average|Average_Pct User Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Used Memory kBytes|KBytes de Memória Usada|Count|Average|Average_Used Memory kBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Virtual Shared Memory|Memória Virtual Compartilhada|Count|Average|Average_Virtual Shared Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% DPC Time|% de Tempo de DPC|Count|Average|Average_% DPC Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Idle Time|% de Tempo Ocioso|Contagem|Average|Average_% Idle Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Interrupt Time|% de Tempo de Interrupção|Count|Average|Average_% Interrupt Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% IO Wait Time|% de Tempo de Espera de E/S|Count|Average|Average_% IO Wait Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Nice Time|% de Tempo Adequado|Count|Average|Average_% Nice Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Privileged Time|% de Tempo Privilegiado|Count|Average|Average_% Privileged Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Processor Time|% Tempo do Processador|Count|Average|Average_% Processor Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% User Time|% de Tempo do Usuário|Count|Average|Average_% User Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Physical Memory|Memória Física Livre|Count|Average|Average_Free Physical Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Space in Paging Files|Espaço Livre em Arquivos de Paginação|Count|Average|Average_Free Space in Paging Files|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Virtual Memory|Memória Virtual Livre|Count|Average|Average_Free Virtual Memory|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Processes|Processos|Count|Average|Average_Processes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Size Stored In Paging Files|Tamanho Armazenado em Arquivos de Paginação|Count|Average|Average_Size Stored In Paging Files|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Uptime|Tempo de Atividade|Count|Average|Average_Uptime|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Users|Usuários|Count|Average|Average_Users|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. de segundos/Leitura do Disco|Média de segundos/Leitura do Disco|Contagem|Average|Average_Avg. de segundos/Leitura do Disco|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Avg. de segundos/Gravação do Disco|Média de segundos/Gravação do Disco|Count|Average|Average_Avg. de segundos/Gravação do Disco|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Current Disk Queue Length|Comprimento da Fila do Disco Atual|Count|Average|Average_Current Disk Queue Length|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Reads/sec|Leituras de Disco/s|Contagem|Average|Average_Disk Reads/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Transfers/sec|Transferências de Disco/s|Count|Average|Average_Disk Transfers/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Disk Writes/sec|Gravações de Disco/s|Count|Average|Average_Disk Writes/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Free Megabytes|Megabytes Livres|Count|Average|Average_Free Megabytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Free Space|% de Espaço Livre|Contagem|Average|Average_% Free Space|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Available MBytes|MBytes Disponíveis|Contagem|Average|Average_Available MBytes|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Committed Bytes In Use|% de Bytes Confirmados em Uso|Count|Average|Average_% Committed Bytes In Use|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Received/sec|Bytes Recebidos/s|Contagem|Average|Average_Bytes Received/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Sent/sec|Bytes Enviados/s|Count|Average|Average_Bytes Sent/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Bytes Total/sec|Bytes Totais/s|Count|Average|Average_Bytes Total/sec|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_% Processor Time|% Tempo do Processador|Contagem|Average|Average_% Processor Time|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Average_Processor Queue Length|Tamanho da fila do processador|Count|Average|Average_Processor Queue Length|Computer, ObjectName, InstanceName, CounterPath, SourceSystem|
|Pulsação|Pulsação|Contagem|Total|Pulsação|Computador, OSType, Versão, SourceComputerId|
|Atualização|Atualização|Count|Average|Atualização|Computer, Product, Classification, UpdateState, Optional, Approved|
|evento|evento|Count|Average|evento|Source, EventLog, Computer, EventCategory, EventLevel, EventLevelName, EventID|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|QueryDuration|Duração da consulta|Milissegundos|Average|Duração da consulta DAX no último intervalo|Sem dimensões|
|QueryPoolJobQueueLength|Threads: Tamanho da fila de trabalhos do pool consultas|Contagem|Average|Número de trabalhos na fila do pool de threads de consulta.|Sem dimensões|
|qpu_high_utilization_metric|Alta utilização de QPU|Count|Total|QPU de alta utilização no último minuto, 1 para QPU de alta utilização, caso contrário, 0|Sem dimensões|
|memory_metric|Memória|Bytes|Average|Memória. Intervalo de 0 a 3 GB para A1, 0 a 5 GB para A2, 0 a 10 GB para A3, 0 a 25 GB para A4, 0 a 50 GB para A5 e 0 a 100 GB para A6|Sem dimensões|
|memory_thrashing_metric|Sobrecarga de memória|Percent|Average|Sobrecarga de memória média.|Sem dimensões|

## <a name="microsoftrelaynamespaces"></a>Microsoft.Relay/namespaces

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|ListenerConnections-Success|ListenerConnections-Success|Count|Total|ListenerConnections bem-sucedido para o Microsoft.Relay.|EntityName|
|ListenerConnections-ClientError|ListenerConnections-ClientError|Count|Total|ClientError no ListenerConnections para o Microsoft.Relay.|EntityName|
|ListenerConnections-ServerError|ListenerConnections-ServerError|Count|Total|ServerError no ListenerConnections para o Microsoft.Relay.|EntityName|
|SenderConnections-Success|SenderConnections-Success|Count|Total|SenderConnections bem-sucedido para o Microsoft.Relay.|EntityName|
|SenderConnections-ClientError|SenderConnections-ClientError|Count|Total|ClientError no SenderConnections para o Microsoft.Relay.|EntityName|
|SenderConnections-ServerError|SenderConnections-ServerError|Contagem|Total|ServerError no SenderConnections para o Microsoft.Relay.|EntityName|
|ListenerConnections-TotalRequests|ListenerConnections-TotalRequests|Count|Total|Total de ListenerConnections para o  Microsoft.Relay.|EntityName|
|SenderConnections-TotalRequests|SenderConnections-TotalRequests|Contagem|Total|Total de solicitações SenderConnections para o Microsoft.Relay.|EntityName|
|ActiveConnections|ActiveConnections|Contagem|Total|Total de ActiveConnections para o Microsoft.Relay.|EntityName|
|ActiveListeners|ActiveListeners|Count|Total|Total de ActiveListeners para o Microsoft.Relay.|EntityName|
|BytesTransferred|BytesTransferred|Count|Total|Total de BytesTransferred para o Microsoft.Relay.|EntityName|
|ListenerDisconnects|ListenerDisconnects|Contagem|Total|Total de ListenerDisconnects para o Microsoft.Relay.|EntityName|
|SenderDisconnects|SenderDisconnects|Contagem|Total|Total de SenderDisconnects para o Microsoft.Relay.|EntityName|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|SearchLatency|Latência de pesquisa|Segundos|Average|Latência média de pesquisa do serviço de pesquisa|Sem dimensões|
|SearchQueriesPerSecond|Consultas de pesquisa por segundo|CountPerSecond|Average|Consultas de pesquisa por segundo para o serviço de pesquisa|Sem dimensões|
|ThrottledSearchQueriesPercentage|Porcentagem das consultas de pesquisa limitadas|Percent|Average|Porcentagem de consultas de pesquisa que eram limitadas para o serviço de pesquisa|Sem dimensões|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|SuccessfulRequests|Solicitações Bem-sucedidas (Versão Prévia)|Count|Total|Total de solicitações bem-sucedidas para um namespace (Versão prévia)|EntityName|
|ServerErrors|Erros do Servidor. (Visualização)|Contagem|Total|Erros do Servidor para o Microsoft.ServiceBus. (Visualização)|EntityName|
|UserErrors|Erros de Usuário. (Visualização)|Count|Total|Erros de Usuário para o Microsoft.ServiceBus. (Visualização)|EntityName|
|ThrottledRequests|Solicitações Limitadas. (Visualização)|Contagem|Total|Solicitações Limitadas para o Microsoft.ServiceBus. (Visualização)|EntityName|
|IncomingRequests|Solicitações de Entrada (Versão Prévia)|Contagem|Total|Solicitações de Entrada para o Microsoft.ServiceBus. (Visualização)|EntityName|
|IncomingMessages|Mensagens de Entrada (Versão Prévia)|Contagem|Total|Mensagens de Entrada para Microsoft.ServiceBus. (Visualização)|EntityName|
|OutgoingMessages|Mensagens de Saída (Versão Prévia)|Count|Total|Mensagens de Saída para o Microsoft.ServiceBus. (Visualização)|EntityName|
|ActiveConnections|ActiveConnections (Versão Prévia)|Count|Total|Conexões Totalmente Ativas para o Microsoft.ServiceBus. (Visualização)|Sem dimensões|
|Size|Tamanho (versão prévia)|Bytes|Average|Tamanho de uma fila/tópico em bytes. (Visualização)|EntityName|
|Mensagens|Contagem de mensagens em uma fila/tópico. (Visualização)|Count|Average|Contagem de mensagens em uma fila/tópico. (Visualização)|EntityName|
|ActiveMessages|Contagem de mensagens em uma fila/tópico. (Visualização)|Contagem|Average|Contagem de mensagens em uma fila/tópico. (Visualização)|EntityName|
|DeadletteredMessages|Contagem de mensagens mortas em uma fila/tópico. (Visualização)|Count|Average|Contagem de mensagens mortas em uma fila/tópico. (Visualização)|EntityName|
|ScheduledMessages|Contagem de mensagens agendadas em uma fila/tópico. (Visualização)|Count|Average|Contagem de mensagens agendadas em uma fila/tópico. (Visualização)|EntityName|
|CPUXNS|Uso da CPU por namespace|Percent|Máximo|Métrica de uso de CPU do namespace premium do barramento de serviço|Sem dimensões|
|WSXNS|Uso do tamanho da memória por namespace|Percent|Máximo|Métrica de uso de memória do namespace premium do barramento de serviço|Sem dimensões|

## <a name="microsoftservicefabricmeshapplications"></a>Microsoft.ServiceFabricMesh/applications

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|AllocatedCpu|AllocatedCpu|Count|Average|CPU alocada para este contêiner em milicores|ApplicationName, ServiceName, CodePackageName, ServiceId|
|AllocatedMemory|AllocatedMemory|Bytes|Average|Memória alocada para este contêiner em MB|ApplicationName, ServiceName, CodePackageName, ServiceId|
|ActualCpu|ActualCpu|Count|Average|Uso real da CPU em milicores|ApplicationName, ServiceName, CodePackageName, ServiceId|
|ActualMemory|ActualMemory|Bytes|Average|Uso real da memória em MB|ApplicationName, ServiceName, CodePackageName, ServiceId|
|CpuUtilization|CpuUtilization|Percent|Average|Utilização da CPU para este contêiner como porcentagem de AllocatedCpu|ApplicationName, ServiceName, CodePackageName, ServiceId|
|MemoryUtilization|MemoryUtilization|Percent|Average|Utilização da CPU para este contêiner como porcentagem de AllocatedCpu|ApplicationName, ServiceName, CodePackageName, ServiceId|
|ApplicationStatus|ApplicationStatus|Contagem|Average|Status do aplicativo de malha Service Fabric|ApplicationName, status|
|ServiceStatus|ServiceStatus|Count|Average|Status de integridade de um serviço no aplicativo de malha Service Fabric|ApplicationName, status, ServiceName|
|ServiceReplicaStatus|ServiceReplicaStatus|Count|Average|Status de integridade de uma réplica de serviço no aplicativo de malha Service Fabric|ApplicationName, status, ServiceName, nome de réplica|
|ContainerStatus|ContainerStatus|Count|Average|Status do contêiner no aplicativo de malha Service Fabric|ApplicationName, ServiceName, CodePackageName, ServiceId, status|
|RestartCount|RestartCount|Count|Average|Reiniciar a contagem de um contêiner no aplicativo de malha Service Fabric|ApplicationName, status, ServiceName, nome de réplica, CodePackageName|

## <a name="microsoftsignalrservicesignalr"></a>Microsoft.SignalRService/SignalR

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|ConnectionCount|Contagem de conexão|Contagem|Máximo|A quantidade de conexões do usuário.|Ponto de extremidade|
|MessageCount|Contagem de Mensagens|Count|Total|A quantidade total de mensagens.|Sem dimensões|
|InboundTraffic|Tráfego de entrada|Bytes|Total|O tráfego de entrada do serviço|Sem dimensões|
|OutboundTraffic|Tráfego de saída|Bytes|Total|O tráfego de saída do serviço|Sem dimensões|
|UserErrors|Erros do usuário|Percent|Máximo|A porcentagem de erros do usuário|Sem dimensões|
|SystemErrors|Erros do sistema|Percent|Máximo|A porcentagem de erros do sistema|Sem dimensões|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|cpu_percent|Percentual de CPU|Percent|Average|Percentual de CPU|Sem dimensões|
|physical_data_read_percent|Porcentagem de E/S de dados|Percent|Average|Porcentagem de E/S de dados|Sem dimensões|
|log_write_percent|Porcentagem de E/S de log|Percent|Average|Percentual de e/s de log. Não aplicável a data warehouses.|Sem dimensões|
|dtu_consumption_percent|Porcentual de DTU|Percent|Average|Porcentagem de DTU. Aplica-se a bancos de dados baseados em DTU.|Sem dimensões|
|armazenamento|Espaço de dados usado|Bytes|Máximo|Tamanho total do banco de dados. Não aplicável a data warehouses.|Sem dimensões|
|connection_successful|Conexões Bem-sucedidas|Count|Total|Conexões Bem-sucedidas|Sem dimensões|
|connection_failed|Conexões com Falha|Count|Total|Conexões com Falha|Sem dimensões|
|blocked_by_firewall|Bloqueado por Firewall|Count|Total|Bloqueado por Firewall|Sem dimensões|
|deadlock|Deadlocks|Count|Total|Deadlocks. Não aplicável a data warehouses.|Sem dimensões|
|storage_percent|Porcentagem de espaço de dados usada|Percent|Máximo|Porcentagem de tamanho do banco de dados. Não aplicável a data warehouses ou a bancos de dados de hiperescala.|Sem dimensões|
|xtp_storage_percent|Percentual de armazenamento do OLTP na memória|Percent|Average|Porcentagem de armazenamento OLTP na memória. Não aplicável a data warehouses.|Sem dimensões|
|workers_percent|Porcentagem de funcionários|Percent|Average|Porcentagem de trabalhadores. Não aplicável a data warehouses.|Sem dimensões|
|sessions_percent|Porcentagem de sessões|Percent|Average|Porcentagem de sessões. Não aplicável a data warehouses.|Sem dimensões|
|dtu_limit|Limite de DTU|Count|Average|Limite de DTU. Aplica-se a bancos de dados baseados em DTU.|Sem dimensões|
|dtu_used|DTU usado|Count|Average|DTU usado. Aplica-se a bancos de dados baseados em DTU.|Sem dimensões|
|cpu_limit|Limite de CPU|Contagem|Average|Limite de CPU. Aplica-se a bancos de dados baseados em vCore.|Sem dimensões|
|cpu_used|CPU usada|Contagem|Average|CPU usada. Aplica-se a bancos de dados baseados em vCore.|Sem dimensões|
|dwu_limit|Limite de DWU|Count|Máximo|Limite de DWU. Aplica-se somente a data warehouses.|Sem dimensões|
|dwu_consumption_percent|Porcentagem de DWU|Percent|Máximo|Porcentagem de DWU. Aplica-se somente a data warehouses.|Sem dimensões|
|dwu_used|DWU usado|Count|Máximo|DWU usado. Aplica-se somente a data warehouses.|Sem dimensões|
|dw_cpu_percent|Percentual de CPU no nível do nó no DW|Percent|Average|Percentual de CPU no nível do nó no DW|DwLogicalNodeId|
|dw_physical_data_read_percent|Percentual de E/S de Dados no nível do nó no DW|Percent|Average|Percentual de E/S de Dados no nível do nó no DW|DwLogicalNodeId|
|cache_hit_percent|Percentual de ocorrência no cache|Percent|Máximo|Porcentagem de acesso ao cache. Aplica-se somente a data warehouses.|Sem dimensões|
|cache_used_percent|Percentual de cache usado|Percent|Máximo|Percentual de cache usado. Aplica-se somente a data warehouses.|Sem dimensões|
|local_tempdb_usage_percent|Porcentagem de local de tempdb|Percent|Average|Porcentagem de tempdb local. Aplica-se somente a data warehouses.|Sem dimensões|
|app_cpu_billed|CPU do aplicativo cobrada|Contagem|Total|CPU do aplicativo cobrada. Aplica-se a bancos de dados sem servidor.|Sem dimensões|
|app_cpu_percent|Porcentagem de CPU do aplicativo|Percent|Average|Porcentagem de CPU do aplicativo. Aplica-se a bancos de dados sem servidor.|Sem dimensões|
|app_memory_percent|Porcentagem de memória usada do aplicativo|Percent|Average|Porcentagem de memória usada do aplicativo. Aplica-se a bancos de dados sem servidor.|Sem dimensões|
|allocated_data_storage|Espaço de dados alocado|Bytes|Average|Espaço de dados alocado. Não aplicável a data warehouses.|Sem dimensões|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|cpu_percent|Percentual de CPU|Percent|Average|Percentual de CPU|Sem dimensões|
|physical_data_read_percent|Porcentagem de E/S de dados|Percent|Average|Porcentagem de E/S de dados|Sem dimensões|
|log_write_percent|Porcentagem de E/S de log|Percent|Average|Porcentagem de E/S de log|Sem dimensões|
|dtu_consumption_percent|Porcentual de DTU|Percent|Average|Porcentagem de DTU. Aplica-se a pools elásticos baseados em DTU.|Sem dimensões|
|storage_percent|Porcentagem de espaço de dados usada||Percent|Average|Porcentagem de armazenamento|Sem dimensões|
|workers_percent|Porcentagem de funcionários|Percent|Average|Porcentagem de funcionários|Sem dimensões|
|sessions_percent|Porcentagem de sessões|Percent|Average|Porcentagem de sessões|Sem dimensões|
|eDTU_limit|Limite de eDTU|Contagem|Average|limite de eDTU. Aplica-se a pools elásticos baseados em DTU.|Sem dimensões|
|storage_limit|Tamanho máximo de dados|Bytes|Average|Limite de armazenamento|Sem dimensões|
|eDTU_used|eDTU usado|Count|Average|eDTU usado. Aplica-se a pools elásticos baseados em DTU.|Sem dimensões|
|storage_used|Espaço de dados usado|Bytes|Average|Armazenamento usado|Sem dimensões|
|xtp_storage_percent|Percentual de armazenamento do OLTP na memória|Percent|Average|Percentual de armazenamento do OLTP na memória|Sem dimensões|
|cpu_limit|Limite de CPU|Contagem|Average|Limite de CPU. Aplica-se a pools elásticos baseados em vCore.|Sem dimensões|
|cpu_used|CPU usada|Count|Average|CPU usada. Aplica-se a pools elásticos baseados em vCore.|Sem dimensões|
|allocated_data_storage|Espaço de dados alocado|Bytes|Average|Espaço de dados alocado|Sem dimensões|
|allocated_data_storage_percent|Porcentagem alocada de espaço de dados|Percent|Máximo|Porcentagem alocada de espaço de dados|Sem dimensões|

## <a name="microsoftsqlmanagedinstances"></a>Microsoft.Sql/managedInstances

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|virtual_core_count|Contagem de núcleos virtuais|Count|Average|Contagem de núcleos virtuais|Sem dimensões|
|avg_cpu_percent|Percentual médio de CPU|Percent|Average|Percentual médio de CPU|Sem dimensões|
|reserved_storage_mb|Espaço de armazenamento reservado|Contagem|Average|Espaço de armazenamento reservado|Sem dimensões|
|storage_space_used_mb|Espaço de armazenamento usado|Count|Average|Espaço de armazenamento usado|Sem dimensões|
|io_requests|Contagem de solicitações de E/S|Count|Average|Contagem de solicitações de E/S|Sem dimensões|
|io_bytes_read|Bytes de E/S lidos|Bytes|Average|Bytes de E/S lidos|Sem dimensões|
|io_bytes_written|Bytes de E/S gravados|Bytes|Average|Bytes de E/S gravados|Sem dimensões|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|UsedCapacity|Capacidade utilizada|Bytes|Média|Capacidade utilizada pela conta|Sem dimensões|
|Transações|Transações|Contagem|Total|O número de solicitações feitas a um serviço de armazenamento ou à operação de API especificada. Esse número inclui solicitações bem-sucedidas e com falha, bem como solicitações que produziram erros. Use a dimensão ResponseType para obter o número de tipo diferente de resposta.|ResponseType, GeoType, ApiName, Authentication|
|Entrada|Entrada|Bytes|Total|A quantidade de dados de entrada, em bytes. Esse número inclui a entrada de um cliente externo no Armazenamento do Azure, bem como a entrada no Azure.|GeoType, ApiName, Authentication|
|Saída|Saída|Bytes|Total|A quantidade de dados de saída, em bytes. Esse número inclui a saída de um cliente externo no Armazenamento do Azure, bem como a saída no Azure. Como resultado, esse número não reflete a saída faturável.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Latência de Servidor Com Sucesso|Milissegundos|Média|A latência média usada pelo Armazenamento do Azure para processar uma solicitação bem-sucedida, em milissegundos. Esse valor não inclui a latência de rede especificada em AverageE2ELatency.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Latência de E2E com Sucesso|Milissegundos|Média|A latência média de ponta a ponta das solicitações bem-sucedidas feitas a um serviço de armazenamento ou à operação de API especificada, em milissegundos. Esse valor inclui o tempo de processamento necessário no Armazenamento do Azure para ler a solicitação, enviar a resposta e receber a confirmação da resposta.|GeoType, ApiName, Authentication|
|Disponibilidade|Disponibilidade|Porcentagem|Média|O percentual de disponibilidade para o serviço de armazenamento ou a operação de API especificada. A disponibilidade é calculada considerando o valor de TotalBillableRequests e dividindo-o pelo número de solicitações aplicáveis, incluindo as que produziram erros inesperados. Todos os erros inesperados resultam na redução da disponibilidade para o serviço de armazenamento ou a operação de API especificada.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|BlobCapacity|Capacidade do Blob|Bytes|Average|A quantidade de armazenamento utilizada pelo serviço Blob da conta de armazenamento em bytes.|BlobType, camada|
|BlobCount|Contagem de Blobs|Contagem|Total|O número de Blobs no serviço Blob da conta de armazenamento.|BlobType|       |BlobCount|Contagem de Blobs|Count|Average|O número de Blobs no serviço Blob da conta de armazenamento.|BlobType, camada|
|ContainerCount|Contagem de Contêineres de Blobs|Contagem|Média|O número de contêineres no serviço Blob da conta de armazenamento.|Sem dimensões|
|IndexCapacity|Capacidade de Índice|Bytes|Average|A quantidade de armazenamento usada pelo índice ADLS Gen2 (hierárquico) em bytes.|Sem dimensões|
|Transações|Transações|Contagem|Total|O número de solicitações feitas a um serviço de armazenamento ou à operação de API especificada. Esse número inclui solicitações bem-sucedidas e com falha, bem como solicitações que produziram erros. Use a dimensão ResponseType para obter o número de tipo diferente de resposta.|ResponseType, GeoType, ApiName, Authentication|
|Entrada|Entrada|Bytes|Total|A quantidade de dados de entrada, em bytes. Esse número inclui a entrada de um cliente externo no Armazenamento do Azure, bem como a entrada no Azure.|GeoType, ApiName, Authentication|
|Saída|Saída|Bytes|Total|A quantidade de dados de saída, em bytes. Esse número inclui a saída de um cliente externo no Armazenamento do Azure, bem como a saída no Azure. Como resultado, esse número não reflete a saída faturável.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Latência de Servidor Com Sucesso|Milissegundos|Média|A latência média usada pelo Armazenamento do Azure para processar uma solicitação bem-sucedida, em milissegundos. Esse valor não inclui a latência de rede especificada em AverageE2ELatency.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Latência de E2E com Sucesso|Milissegundos|Média|A latência média de ponta a ponta das solicitações bem-sucedidas feitas a um serviço de armazenamento ou à operação de API especificada, em milissegundos. Esse valor inclui o tempo de processamento necessário no Armazenamento do Azure para ler a solicitação, enviar a resposta e receber a confirmação da resposta.|GeoType, ApiName, Authentication|
|Disponibilidade|Disponibilidade|Porcentagem|Média|O percentual de disponibilidade para o serviço de armazenamento ou a operação de API especificada. A disponibilidade é calculada considerando o valor de TotalBillableRequests e dividindo-o pelo número de solicitações aplicáveis, incluindo as que produziram erros inesperados. Todos os erros inesperados resultam na redução da disponibilidade para o serviço de armazenamento ou a operação de API especificada.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|FileCapacity|Capacidade do Arquivo|Bytes|Average|A quantidade de armazenamento utilizada pelo serviço Arquivo da conta de armazenamento em bytes.|Sem dimensões|
|FileCount|Contagem de Arquivos|Count|Average|O número de arquivos no serviço Arquivo da conta de armazenamento.|Sem dimensões|
|FileShareCount|Contagem de Compartilhamentos de Arquivo|Count|Average|O número de compartilhamentos de arquivos no serviço Arquivo da conta de armazenamento.|Sem dimensões|
|Transações|Transações|Contagem|Total|O número de solicitações feitas a um serviço de armazenamento ou à operação de API especificada. Esse número inclui solicitações bem-sucedidas e com falha, bem como solicitações que produziram erros. Use a dimensão ResponseType para obter o número de tipo diferente de resposta.|ResponseType, GeoType, ApiName, Authentication|
|Entrada|Entrada|Bytes|Total|A quantidade de dados de entrada, em bytes. Esse número inclui a entrada de um cliente externo no Armazenamento do Azure, bem como a entrada no Azure.|GeoType, ApiName, Authentication|
|Saída|Saída|Bytes|Total|A quantidade de dados de saída, em bytes. Esse número inclui a saída de um cliente externo no Armazenamento do Azure, bem como a saída no Azure. Como resultado, esse número não reflete a saída faturável.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Latência de Servidor Com Sucesso|Milissegundos|Média|A latência média usada pelo Armazenamento do Azure para processar uma solicitação bem-sucedida, em milissegundos. Esse valor não inclui a latência de rede especificada em AverageE2ELatency.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Latência de E2E com Sucesso|Milissegundos|Média|A latência média de ponta a ponta das solicitações bem-sucedidas feitas a um serviço de armazenamento ou à operação de API especificada, em milissegundos. Esse valor inclui o tempo de processamento necessário no Armazenamento do Azure para ler a solicitação, enviar a resposta e receber a confirmação da resposta.|GeoType, ApiName, Authentication|
|Disponibilidade|Disponibilidade|Porcentagem|Média|O percentual de disponibilidade para o serviço de armazenamento ou a operação de API especificada. A disponibilidade é calculada considerando o valor de TotalBillableRequests e dividindo-o pelo número de solicitações aplicáveis, incluindo as que produziram erros inesperados. Todos os erros inesperados resultam na redução da disponibilidade para o serviço de armazenamento ou a operação de API especificada.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|QueueCapacity|Capacidade da Fila|Bytes|Média|A quantidade de armazenamento utilizada pelo serviço Fila da conta de armazenamento em bytes.|Sem dimensões|
|QueueCount|Contagem de Filas|Contagem|Média|O número de filas no serviço Fila da conta de armazenamento.|Sem dimensões|
|QueueMessageCount|Contagem de Mensagens da Fila|Contagem|Média|O número aproximado de mensagens da fila no serviço Fila da conta de armazenamento.|Sem dimensões|
|Transações|Transações|Contagem|Total|O número de solicitações feitas a um serviço de armazenamento ou à operação de API especificada. Esse número inclui solicitações bem-sucedidas e com falha, bem como solicitações que produziram erros. Use a dimensão ResponseType para obter o número de tipo diferente de resposta.|ResponseType, GeoType, ApiName, Authentication|
|Entrada|Entrada|Bytes|Total|A quantidade de dados de entrada, em bytes. Esse número inclui a entrada de um cliente externo no Armazenamento do Azure, bem como a entrada no Azure.|GeoType, ApiName, Authentication|
|Saída|Saída|Bytes|Total|A quantidade de dados de saída, em bytes. Esse número inclui a saída de um cliente externo no Armazenamento do Azure, bem como a saída no Azure. Como resultado, esse número não reflete a saída faturável.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Latência de Servidor Com Sucesso|Milissegundos|Média|A latência média usada pelo Armazenamento do Azure para processar uma solicitação bem-sucedida, em milissegundos. Esse valor não inclui a latência de rede especificada em AverageE2ELatency.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Latência de E2E com Sucesso|Milissegundos|Média|A latência média de ponta a ponta das solicitações bem-sucedidas feitas a um serviço de armazenamento ou à operação de API especificada, em milissegundos. Esse valor inclui o tempo de processamento necessário no Armazenamento do Azure para ler a solicitação, enviar a resposta e receber a confirmação da resposta.|GeoType, ApiName, Authentication|
|Disponibilidade|Disponibilidade|Porcentagem|Média|O percentual de disponibilidade para o serviço de armazenamento ou a operação de API especificada. A disponibilidade é calculada considerando o valor de TotalBillableRequests e dividindo-o pelo número de solicitações aplicáveis, incluindo as que produziram erros inesperados. Todos os erros inesperados resultam na redução da disponibilidade para o serviço de armazenamento ou a operação de API especificada.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|TableCapacity|Capacidade da Tabela|Bytes|Média|A quantidade de armazenamento utilizada pelo serviço Tabela da conta de armazenamento em bytes.|Sem dimensões|
|TableCount|Contagem de Tabelas|Contagem|Média|O número de tabelas no serviço Tabela da conta de armazenamento.|Sem dimensões|
|TableEntityCount|Contagem de Entidades de Tabela|Contagem|Média|O número de entidades de tabelas no serviço Tabela da conta de armazenamento.|Sem dimensões|
|Transações|Transações|Contagem|Total|O número de solicitações feitas a um serviço de armazenamento ou à operação de API especificada. Esse número inclui solicitações bem-sucedidas e com falha, bem como solicitações que produziram erros. Use a dimensão ResponseType para obter o número de tipo diferente de resposta.|ResponseType, GeoType, ApiName, Authentication|
|Entrada|Entrada|Bytes|Total|A quantidade de dados de entrada, em bytes. Esse número inclui a entrada de um cliente externo no Armazenamento do Azure, bem como a entrada no Azure.|GeoType, ApiName, Authentication|
|Saída|Saída|Bytes|Total|A quantidade de dados de saída, em bytes. Esse número inclui a saída de um cliente externo no Armazenamento do Azure, bem como a saída no Azure. Como resultado, esse número não reflete a saída faturável.|GeoType, ApiName, Authentication|
|SuccessServerLatency|Latência de Servidor Com Sucesso|Milissegundos|Média|A latência média usada pelo Armazenamento do Azure para processar uma solicitação bem-sucedida, em milissegundos. Esse valor não inclui a latência de rede especificada em AverageE2ELatency.|GeoType, ApiName, Authentication|
|SuccessE2ELatency|Latência de E2E com Sucesso|Milissegundos|Média|A latência média de ponta a ponta das solicitações bem-sucedidas feitas a um serviço de armazenamento ou à operação de API especificada, em milissegundos. Esse valor inclui o tempo de processamento necessário no Armazenamento do Azure para ler a solicitação, enviar a resposta e receber a confirmação da resposta.|GeoType, ApiName, Authentication|
|Disponibilidade|Disponibilidade|Porcentagem|Média|O percentual de disponibilidade para o serviço de armazenamento ou a operação de API especificada. A disponibilidade é calculada considerando o valor de TotalBillableRequests e dividindo-o pelo número de solicitações aplicáveis, incluindo as que produziram erros inesperados. Todos os erros inesperados resultam na redução da disponibilidade para o serviço de armazenamento ou a operação de API especificada.|GeoType, ApiName, Authentication|

## <a name="microsoftstoragesyncstoragesyncservices"></a>microsoft.storagesync/storageSyncServices

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|ServerSyncSessionResult|Resultado da sessão de sincronização|Count|Average|Métrica que registra um valor de 1 cada vez que o ponto de extremidade do servidor conclui com êxito uma sessão de sincronização com o ponto de extremidade de nuvem|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionAppliedFilesCount|Arquivos Sincronizados|Count|Total|Contagem de arquivos sincronizados|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncSyncSessionPerItemErrorsCount|Arquivos não sincronizando|Count|Total|Falha na sincronização da contagem de arquivos|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncBatchTransferredFileBytes|Bytes sincronizados|Bytes|Total|Tamanho total do arquivo transferido para sessões de sincronização|SyncGroupName, ServerEndpointName, SyncDirection|
|StorageSyncServerHeartbeat|Status Online do Servidor|Count|Máximo|Métrica que registra um valor de 1 cada vez que o servidor registrado registra com êxito uma pulsação com o ponto de extremidade de nuvem|ServerName|
|StorageSyncRecallIOTotalSizeBytes|Recall da camada de nuvem|Bytes|Total|Tamanho total dos dados recuperados pelo servidor|ServerName|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|ResourceUtilization|% de utilização do SU|Percent|Máximo|% de utilização do SU|LogicalName, PartitionID|
|InputEvents|Eventos de entrada|Count|Total|Eventos de entrada|LogicalName, PartitionID|
|InputEventBytes|Bytes de evento de entrada|Bytes|Total|Bytes de evento de entrada|LogicalName, PartitionID|
|LateInputEvents|Eventos de entrada atrasados|Contagem|Total|Eventos de entrada tardia|LogicalName, PartitionID|
|OutputEvents|Eventos de saída|Count|Total|Eventos de saída|LogicalName, PartitionID|
|ConversionErrors|Erros de conversão de dados|Contagem|Total|Erros de conversão de dados|LogicalName, PartitionID|
|Erros|Erros de tempo de execução|Count|Total|Erros de tempo de execução|LogicalName, PartitionID|
|DroppedOrAdjustedEvents|Eventos fora de ordem|Count|Total|Eventos fora de ordem|LogicalName, PartitionID|
|AMLCalloutRequests|Solicitações de função|Count|Total|Solicitações de função|LogicalName, PartitionID|
|AMLCalloutFailedRequests|Solicitações de função com falha|Count|Total|Solicitações de função com falha|LogicalName, PartitionID|
|AMLCalloutInputEvents|Eventos de função|Count|Total|Eventos de função|LogicalName, PartitionID|
|DeserializationError|Erros de Desserialização de Entrada|Count|Total|Erros de Desserialização de Entrada|LogicalName, PartitionID|
|EarlyInputEvents|Eventos de Entrada Antecipados|Count|Total|Eventos de Entrada Antecipados|LogicalName, PartitionID|
|OutputWatermarkDelaySeconds|Atraso de Marca d'água|Segundos|Máximo|Atraso de Marca d'água|LogicalName, PartitionID|
|InputEventsSourcesBacklogged|Eventos de Entrada Acumulados|Contagem|Máximo|Eventos de Entrada Acumulados|LogicalName, PartitionID|
|InputEventsSourcesPerSecond|Fontes de Entrada Recebidas|Count|Total|Fontes de Entrada Recebidas|LogicalName, PartitionID|

## <a name="microsofttimeseriesinsightsenvironments"></a>Microsoft.TimeSeriesInsights/environments

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|IngressReceivedMessages|Mensagens de entrada recebidas|Contagem|Total|Contagem de mensagens lidas de todos os Hubs de Eventos ou origens de evento do Hub IoT|Sem dimensões|
|IngressReceivedInvalidMessages|Mensagens de entrada inválidas recebidas|Count|Total|Contagem de mensagens inválidas lidas de todos os Hubs de Eventos ou origens de evento do Hub IoT|Sem dimensões|
|IngressReceivedBytes|Bytes de entrada recebidos|Bytes|Total|Contagem de bytes lidos de todas as fontes de evento|Sem dimensões|
|IngressStoredBytes|Bytes de entrada armazenados|Bytes|Total|Tamanho total de eventos processados com êxito e disponíveis para consulta|Sem dimensões|
|IngressStoredEvents|Eventos de entrada armazenados|Contagem|Total|Contagem de eventos nivelados processados com êxito e disponíveis para consulta|Sem dimensões|
|IngressReceivedMessagesTimeLag|Tempo de atraso das mensagens de entrada recebidas|Segundos|Máximo|Diferença entre a hora em que a mensagem é enfileirada na origem do evento e a hora em que ela é processada na entrada|Sem dimensões|
|IngressReceivedMessagesCountLag|Atraso na contagem das mensagens de entrada recebidas|Contagem|Average|Diferença entre o número de sequência da última mensagem enfileirada na partição da origem do evento e o número de sequência da mensagem que está sendo processada na entrada|Sem dimensões|
|WarmStorageMaxProperties|Propriedades máximas de armazenamento quente|Count|Máximo|Número máximo de propriedades usadas permitidas pelo ambiente para SKU S1/S2 e número máximo de propriedades permitidas pelo armazenamento quente para o SKU PAYG|Sem dimensões|
|WarmStorageUsedProperties|Propriedades usadas de armazenamento quente |Count|Máximo|Número de propriedades usadas pelo ambiente para SKU S1/S2 e número de propriedades usadas pelo armazenamento quente para SKU PAYG|Sem dimensões|

## <a name="microsofttimeseriesinsightsenvironmentseventsources"></a>Microsoft.TimeSeriesInsights/environments/eventsources

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|IngressReceivedMessages|Mensagens de entrada recebidas|Count|Total|Contagem de mensagens lidas na origem do evento|Sem dimensões|
|IngressReceivedInvalidMessages|Mensagens de entrada inválidas recebidas|Count|Total|Contagem de mensagens inválidas lidas na origem do evento|Sem dimensões|
|IngressReceivedBytes|Bytes de entrada recebidos|Bytes|Total|Contagem de bytes lidos da fonte de evento|Sem dimensões|
|IngressStoredBytes|Bytes de entrada armazenados|Bytes|Total|Tamanho total de eventos processados com êxito e disponíveis para consulta|Sem dimensões|
|IngressStoredEvents|Eventos de entrada armazenados|Count|Total|Contagem de eventos nivelados processados com êxito e disponíveis para consulta|Sem dimensões|
|IngressReceivedMessagesTimeLag|Tempo de atraso das mensagens de entrada recebidas|Segundos|Máximo|Diferença entre a hora em que a mensagem é enfileirada na origem do evento e a hora em que ela é processada na entrada|Sem dimensões|
|IngressReceivedMessagesCountLag|Atraso na contagem das mensagens de entrada recebidas|Contagem|Average|Diferença entre o número de sequência da última mensagem enfileirada na partição da origem do evento e o número de sequência da mensagem que está sendo processada na entrada|Sem dimensões|
|WarmStorageMaxProperties|Propriedades máximas de armazenamento quente|Count|Máximo|Número máximo de propriedades usadas permitidas pelo ambiente para SKU S1/S2 e número máximo de propriedades permitidas pelo armazenamento quente para o SKU PAYG|Sem dimensões|
|WarmStorageUsedProperties|Propriedades usadas de armazenamento quente |Count|Máximo|Número de propriedades usadas pelo ambiente para SKU S1/S2 e número de propriedades usadas pelo armazenamento quente para SKU PAYG|Sem dimensões|

## <a name="microsoftvmwarecloudsimplevirtualmachines"></a>Microsoft.VMwareCloudSimple/virtualMachines

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|DiskReadBytesPerSecond|Disk Read Bytes/Sec|BytesPerSecond|Average|Taxa de transferência média do disco devido a operações de leitura no período de exemplo.|Sem dimensões|
|DiskWriteBytesPerSecond|Disk Write Bytes/Sec|BytesPerSecond|Average|Taxa de transferência média do disco devido a operações de gravação no período de exemplo.|Sem dimensões|
|Bytes de Leitura de Disco|Bytes de leitura de disco|Bytes|Total|Taxa de transferência total do disco devido a operações de leitura no período de exemplo.|Sem dimensões|
|Bytes de Gravação do Disco|Bytes de gravação de disco|Bytes|Total|Taxa de transferência total do disco devido a operações de gravação no período de exemplo.|Sem dimensões|
|DiskReadOperations|Operações de leitura de disco|Count|Total|O número de operações de leitura de e/s no período de exemplo anterior. Observe que essas operações podem ser de tamanho variável.|Sem dimensões|
|DiskWriteOperations|Operações de gravação de disco|Count|Total|O número de operações de gravação de e/s no período de exemplo anterior. Observe que essas operações podem ser de tamanho variável.|Sem dimensões|
|Operações de leitura de disco/Seg|Operações de leitura de disco/Seg|CountPerSecond|Average|O número médio de operações de leitura de e/s no período de exemplo anterior. Observe que essas operações podem ser de tamanho variável.|Sem dimensões|
|Operações de gravação de disco/Seg|Operações de gravação de disco/Seg|CountPerSecond|Average|O número médio de operações de gravação de e/s no período de exemplo anterior. Observe que essas operações podem ser de tamanho variável.|Sem dimensões|
|DiskReadLatency|Latência de leitura de disco|Milissegundos|Average|Latência de leitura total. A soma das latências de leitura do dispositivo e do kernel.|Sem dimensões|
|DiskWriteLatency|Latência de Gravação de disco|Milissegundos|Average|Latência de gravação total. A soma das latências de gravação do kernel e do dispositivo.|Sem dimensões|
|NetworkInBytesPerSecond|Rede em bytes/s|BytesPerSecond|Average|Taxa de transferência média de rede para o tráfego recebido.|Sem dimensões|
|NetworkOutBytesPerSecond|Bytes de saída de rede/s|BytesPerSecond|Average|Taxa de transferência média de rede para o tráfego transmitido.|Sem dimensões|
|Entrada de Rede|Entrada na rede|Bytes|Total|Taxa de transferência total de rede para o tráfego recebido.|Sem dimensões|
|Saída de Rede|Saída da rede|Bytes|Total|Taxa de transferência total de rede para o tráfego transmitido.|Sem dimensões|
|MemoryUsed|Memória usada|Bytes|Average|A quantidade de memória da máquina que está sendo usada pela VM.|Sem dimensões|
|MemoryGranted|Memória concedida|Bytes|Average|A quantidade de memória que foi concedida à VM pelo host. A memória não é concedida ao host até que ele seja tocado uma vez e a memória concedida possa ser trocada ou bloqueada se o VMkernel precisar da memória.|Sem dimensões|
|MemoryActive|Memória ativa|Bytes|Average|A quantidade de memória usada pela VM na última pequena janela de tempo. Esse é o número "verdadeiro" da quantidade de memória necessária para a VM no momento. A memória adicional não utilizada pode ser trocada ou por balão sem impacto no desempenho do convidado.|Sem dimensões|
|Porcentagem de CPU|Porcentagem de CPU|Porcentagem|Average|A utilização da CPU. Esse valor é relatado com 100% que representa todos os núcleos de processador no sistema. Por exemplo, uma VM de 2 vias usando 50% de um sistema de quatro núcleos está usando completamente dois núcleos.|Sem dimensões|
|PercentageCpuReady|Percentual de CPU pronta|Milissegundos|Total|O tempo de preparação é o tempo gasto aguardando que as CPUs fiquem disponíveis no intervalo de atualização anterior.|Sem dimensões|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|CpuPercentage|Porcentagem de CPU|Percent|Average|Porcentagem de CPU|Instância|
|MemoryPercentage|Porcentagem de Memória|Percent|Average|Percentual de Memória|Instância|
|DiskQueueLength|Comprimento da Fila do Disco|Count|Average|Comprimento da Fila do Disco|Instância|
|HttpQueueLength|Comprimento da Fila Http|Count|Average|Comprimento da Fila Http|Instância|
|BytesReceived|Entrada de Dados|Bytes|Total|Entrada de Dados|Instância|
|BytesSent|Saída de Dados|Bytes|Total|Saída de Dados|Instância|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (excluindo funções)

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|CpuTime|Tempo de CPU|Segundos|Total|Tempo de CPU|Instância|
|Solicitações|Solicitações|Contagem|Total|Solicitações|Instância|
|BytesReceived|Entrada de Dados|Bytes|Total|Entrada de Dados|Instância|
|BytesSent|Saída de Dados|Bytes|Total|Saída de Dados|Instância|
|Http101|Http 101|Count|Total|Http 101|Instância|
|Http2xx|Http 2xx|Contagem|Total|Http 2xx|Instância|
|Http3xx|Http 3xx|Count|Total|Http 3xx|Instância|
|Http401|Http 401|Count|Total|Http 401|Instância|
|Http403|Http 403|Count|Total|Http 403|Instância|
|Http404|Http 404|Count|Total|Http 404|Instância|
|Http406|Http 406|Contagem|Total|Http 406|Instância|
|Http4xx|Http 4xx|Count|Total|Http 4xx|Instância|
|Http5xx|Erros do Servidor Http|Count|Total|Erros do Servidor Http|Instância|
|MemoryWorkingSet|Conjunto de trabalho de memória|Bytes|Average|Conjunto de trabalho de memória|Instância|
|AverageMemoryWorkingSet|Conjunto de trabalho de memória média|Bytes|Average|Conjunto de trabalho de memória média|Instância|
|AverageResponseTime|Tempo Médio de Resposta|Segundos|Average|Tempo Médio de Resposta|Instância|
|AppConnections|Conexões|Count|Average|Conexões|Instância|
|Identificadores|Núm. de Identificadores|Contagem|Average|Núm. de Identificadores|Instância|
|Threads|Contagem de Threads|Contagem|Average|Contagem de Threads|Instância|
|PrivateBytes|Bytes Privados|Bytes|Average|Bytes Privados|Instância|
|IoReadBytesPerSecond|E/S de bytes de leitura por segundo|BytesPerSecond|Total|E/S de bytes de leitura por segundo|Instância|
|IoWriteBytesPerSecond|E/S de bytes de gravação por segundo|BytesPerSecond|Total|E/S de bytes de gravação por segundo|Instância|
|IoOtherBytesPerSecond|E/S de outros bytes por segundo|BytesPerSecond|Total|E/S de outros bytes por segundo|Instância|
|IoReadOperationsPerSecond|E/S de operações de leitura por segundo|BytesPerSecond|Total|E/S de operações de leitura por segundo|Instância|
|IoWriteOperationsPerSecond|E/S de operações de gravação por segundo|BytesPerSecond|Total|E/S de operações de gravação por segundo|Instância|
|IoOtherOperationsPerSecond|E/S de outras operações por segundo|BytesPerSecond|Total|E/S de outras operações por segundo|Instância|
|RequestsInApplicationQueue|Solicitações na fila do aplicativo|Count|Average|Solicitações na fila do aplicativo|Instância|
|CurrentAssemblies|Módulos (Assemblies) Atuais|Contagem|Average|Módulos (Assemblies) Atuais|Instância|
|TotalAppDomains|Total de domínios de aplicativo|Contagem|Average|Total de domínios de aplicativo|Instância|
|TotalAppDomainsUnloaded|Total de domínios de aplicativo descarregados|Count|Average|Total de domínios de aplicativo descarregados|Instância|
|Gen0Collections|Coletas de lixo da Ger 0|Contagem|Total|Coletas de lixo da Ger 0|Instância|
|Gen1Collections|Coletas de lixo da Ger 1|Count|Total|Coletas de lixo da Ger 1|Instância|
|Gen2Collections|Coletas de lixo da Ger 2|Count|Total|Coletas de lixo da Ger 2|Instância|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (funções)

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|BytesReceived|Entrada de Dados|Bytes|Total|Entrada de Dados|Instância|
|BytesSent|Saída de Dados|Bytes|Total|Saída de Dados|Instância|
|Http5xx|Erros do Servidor Http|Count|Total|Erros do Servidor Http|Instância|
|MemoryWorkingSet|Conjunto de trabalho de memória|Bytes|Average|Conjunto de trabalho de memória|Instância|
|AverageMemoryWorkingSet|Conjunto de trabalho de memória média|Bytes|Average|Conjunto de trabalho de memória média|Instância|
|FunctionExecutionUnits|Unidades de Execução de Função|MB/milissegundos|Total|[Unidades de execução de função](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ#how-can-i-view-graphs-of-execution-count-and-gb-seconds)|Instância|
|FunctionExecutionCount|Contagem de Execuções de Função|Count|Total|Contagem de Execuções de Função|Instância|
|PrivateBytes|Bytes Privados|Bytes|Average|Bytes Privados|Instância|
|IoReadBytesPerSecond|E/S de bytes de leitura por segundo|BytesPerSecond|Total|E/S de bytes de leitura por segundo|Instância|
|IoWriteBytesPerSecond|E/S de bytes de gravação por segundo|BytesPerSecond|Total|E/S de bytes de gravação por segundo|Instância|
|IoOtherBytesPerSecond|E/S de outros bytes por segundo|BytesPerSecond|Total|E/S de outros bytes por segundo|Instância|
|IoReadOperationsPerSecond|E/S de operações de leitura por segundo|BytesPerSecond|Total|E/S de operações de leitura por segundo|Instância|
|IoWriteOperationsPerSecond|E/S de operações de gravação por segundo|BytesPerSecond|Total|E/S de operações de gravação por segundo|Instância|
|IoOtherOperationsPerSecond|E/S de outras operações por segundo|BytesPerSecond|Total|E/S de outras operações por segundo|Instância|
|RequestsInApplicationQueue|Solicitações na fila do aplicativo|Count|Average|Solicitações na fila do aplicativo|Instância|
|CurrentAssemblies|Módulos (Assemblies) Atuais|Contagem|Average|Módulos (Assemblies) Atuais|Instância|
|TotalAppDomains|Total de domínios de aplicativo|Count|Average|Total de domínios de aplicativo|Instância|
|TotalAppDomainsUnloaded|Total de domínios de aplicativo descarregados|Count|Average|Total de domínios de aplicativo descarregados|Instância|
|Gen0Collections|Coletas de lixo da Ger 0|Count|Total|Coletas de lixo da Ger 0|Instância|
|Gen1Collections|Coletas de lixo da Ger 1|Contagem|Total|Coletas de lixo da Ger 1|Instância|
|Gen2Collections|Coletas de lixo da Ger 2|Count|Total|Coletas de lixo da Ger 2|Instância|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|CpuTime|Tempo de CPU|Segundos|Total|Tempo de CPU|Instância|
|Solicitações|Solicitações|Count|Total|Solicitações|Instância|
|BytesReceived|Entrada de Dados|Bytes|Total|Entrada de Dados|Instância|
|BytesSent|Saída de Dados|Bytes|Total|Saída de Dados|Instância|
|Http101|Http 101|Count|Total|Http 101|Instância|
|Http2xx|Http 2xx|Count|Total|Http 2xx|Instância|
|Http3xx|Http 3xx|Count|Total|Http 3xx|Instância|
|Http401|Http 401|Count|Total|Http 401|Instância|
|Http403|Http 403|Count|Total|Http 403|Instância|
|Http404|Http 404|Count|Total|Http 404|Instância|
|Http406|Http 406|Count|Total|Http 406|Instância|
|Http4xx|Http 4xx|Count|Total|Http 4xx|Instância|
|Http5xx|Erros do Servidor Http|Count|Total|Erros do Servidor Http|Instância|
|MemoryWorkingSet|Conjunto de trabalho de memória|Bytes|Average|Conjunto de trabalho de memória|Instância|
|AverageMemoryWorkingSet|Conjunto de trabalho de memória média|Bytes|Average|Conjunto de trabalho de memória média|Instância|
|AverageResponseTime|Tempo Médio de Resposta|Segundos|Average|Tempo Médio de Resposta|Instância|
|FunctionExecutionUnits|Unidades de Execução de Função|Count|Total|Unidades de Execução de Função|Instância|
|FunctionExecutionCount|Contagem de Execuções de Função|Contagem|Total|Contagem de Execuções de Função|Instância|
|AppConnections|Conexões|Contagem|Average|Conexões|Instância|
|Identificadores|Núm. de Identificadores|Count|Average|Núm. de Identificadores|Instância|
|Threads|Contagem de Threads|Contagem|Average|Contagem de Threads|Instância|
|PrivateBytes|Bytes Privados|Bytes|Average|Bytes Privados|Instância|
|IoReadBytesPerSecond|E/S de bytes de leitura por segundo|BytesPerSecond|Total|E/S de bytes de leitura por segundo|Instância|
|IoWriteBytesPerSecond|E/S de bytes de gravação por segundo|BytesPerSecond|Total|E/S de bytes de gravação por segundo|Instância|
|IoOtherBytesPerSecond|E/S de outros bytes por segundo|BytesPerSecond|Total|E/S de outros bytes por segundo|Instância|
|IoReadOperationsPerSecond|E/S de operações de leitura por segundo|BytesPerSecond|Total|E/S de operações de leitura por segundo|Instância|
|IoWriteOperationsPerSecond|E/S de operações de gravação por segundo|BytesPerSecond|Total|E/S de operações de gravação por segundo|Instância|
|IoOtherOperationsPerSecond|E/S de outras operações por segundo|BytesPerSecond|Total|E/S de outras operações por segundo|Instância|
|RequestsInApplicationQueue|Solicitações na fila do aplicativo|Contagem|Average|Solicitações na fila do aplicativo|Instância|
|CurrentAssemblies|Módulos (Assemblies) Atuais|Contagem|Average|Módulos (Assemblies) Atuais|Instância|
|TotalAppDomains|Total de domínios de aplicativo|Count|Average|Total de domínios de aplicativo|Instância|
|TotalAppDomainsUnloaded|Total de domínios de aplicativo descarregados|Count|Average|Total de domínios de aplicativo descarregados|Instância|
|Gen0Collections|Coletas de lixo da Ger 0|Count|Total|Coletas de lixo da Ger 0|Instância|
|Gen1Collections|Coletas de lixo da Ger 1|Contagem|Total|Coletas de lixo da Ger 1|Instância|
|Gen2Collections|Coletas de lixo da Ger 2|Count|Total|Coletas de lixo da Ger 2|Instância|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|Solicitações|Solicitações|Count|Total|Solicitações|Instância|
|BytesReceived|Entrada de Dados|Bytes|Total|Entrada de Dados|Instância|
|BytesSent|Saída de Dados|Bytes|Total|Saída de Dados|Instância|
|Http101|Http 101|Contagem|Total|Http 101|Instância|
|Http2xx|Http 2xx|Count|Total|Http 2xx|Instância|
|Http3xx|Http 3xx|Count|Total|Http 3xx|Instância|
|Http401|Http 401|Contagem|Total|Http 401|Instância|
|Http403|Http 403|Count|Total|Http 403|Instância|
|Http404|Http 404|Contagem|Total|Http 404|Instância|
|Http406|Http 406|Contagem|Total|Http 406|Instância|
|Http4xx|Http 4xx|Contagem|Total|Http 4xx|Instância|
|Http5xx|Erros do Servidor Http|Contagem|Total|Erros do Servidor Http|Instância|
|AverageResponseTime|Tempo Médio de Resposta|Segundos|Average|Tempo Médio de Resposta|Instância|
|CpuPercentage|Porcentagem de CPU|Percent|Average|Porcentagem de CPU|Instância|
|MemoryPercentage|Porcentagem de Memória|Percent|Average|Percentual de Memória|Instância|
|DiskQueueLength|Comprimento da Fila do Disco|Count|Average|Comprimento da Fila do Disco|Instância|
|HttpQueueLength|Comprimento da Fila Http|Count|Average|Comprimento da Fila Http|Instância|
|ActiveRequests|Solicitações Ativas|Count|Total|Solicitações Ativas|Instância|
|TotalFrontEnds|Total de Front-Ends|Count|Average|Total de Front-Ends|Sem dimensões|
|SmallAppServicePlanInstances|Trabalhadores do Plano do Serviço de Aplicativo Pequeno|Count|Average|Trabalhadores do Plano do Serviço de Aplicativo Pequeno|Sem dimensões|
|MediumAppServicePlanInstances|Funções de trabalho médias do Plano do Serviço de Aplicativo|Count|Média|Trabalhadores do Plano do Serviço de Aplicativo Médio|Sem dimensões|
|LargeAppServicePlanInstances|Trabalhadores do Plano do Serviço de Aplicativo Grande|Count|Average|Trabalhadores do Plano do Serviço de Aplicativo Grande|Sem dimensões|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Métrica|Nome de exibição da métrica|Unidade|Tipo de agregação|DESCRIÇÃO|Dimensões|
|---|---|---|---|---|---|
|WorkersTotal|Total de Funcionários|Count|Average|Total de Funcionários|Sem dimensões|
|WorkersAvailable|Trabalhadores Disponíveis|Contagem|Average|Trabalhadores Disponíveis|Sem dimensões|
|WorkersUsed|Funcionários Utilizados|Contagem|Average|Funcionários Utilizados|Sem dimensões|
|CpuPercentage|Porcentagem de CPU|Percent|Average|Porcentagem de CPU|Instância|
|MemoryPercentage|Porcentagem de Memória|Percent|Average|Percentual de Memória|Instância|

## <a name="next-steps"></a>Próximas etapas
* [Leia sobre as métricas no Azure Monitor](data-platform.md)
* [Criar alertas para métricas](alerts-overview.md)
* [Exportar as métricas de armazenamento, Hub de eventos ou Log Analytics](diagnostic-logs-overview.md)
