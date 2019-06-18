---
title: Registro em log de diagnóstico
titleSuffix: Azure Cognitive Services
description: Este guia fornece instruções passo a passo para habilitar o log de diagnóstico para um serviço cognitivo do Azure. Esses logs fornecem dados avançados e frequentes sobre a operação de um recurso que são usados para identificação de problemas e depuração.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.topic: article
ms.date: 06/14/2019
ms.author: erhopf
ms.openlocfilehash: e1a6a44d7ff9d5786388fc47245ef5c79cb9be82
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67155726"
---
# <a name="enable-diagnostic-logging-for-azure-cognitive-services"></a>Habilitar o log de diagnóstico para serviços Cognitivos do Azure

Este guia fornece instruções passo a passo para habilitar o log de diagnóstico para um serviço cognitivo do Azure. Esses logs fornecem dados avançados e frequentes sobre a operação de um recurso que são usados para identificação de problemas e depuração. Antes de continuar, você deve ter uma conta do Azure com uma assinatura para pelo menos um serviço cognitivo, tais como [pesquisa do Bing na Web](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/overview), [serviços de fala](https://docs.microsoft.com/azure/cognitive-services/speech-service/overview), ou [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/what-is-luis).

## <a name="prerequisites"></a>Pré-requisitos

Para habilitar o log de diagnóstico, você precisará em algum lugar para armazenar os dados de log. Este tutorial usa o armazenamento do Azure e o Log Analytics.

* [O armazenamento do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) -retém os logs de diagnóstico para auditoria de políticas, análise estática ou backup. A conta de armazenamento não precisa estar na mesma assinatura que o recurso que emite os logs, contanto que o usuário que define a configuração tenha acesso RBAC apropriado a ambas as assinaturas.
* [Log Analytics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics) – uma ferramenta de análise e pesquisa de logs flexíveis que permite a análise dos logs brutos gerados por um recurso do Azure.

> [!NOTE]
> Opções de configuração adicionais estão disponíveis. Para obter mais informações, consulte [coletar e consumir dados de log de seus recursos do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="enable-diagnostic-log-collection"></a>Habilitar a coleta de log de diagnóstico  

Vamos começar ativando o diagnóstico de log usando o portal do Azure.

> [!NOTE]
> Para habilitar esse recurso usando o PowerShell ou a CLI do Azure, use as instruções fornecidas no [coletar e consumir dados de log de seus recursos do Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs#how-to-enable-collection-of-diagnostic-logs).

1. Navegue até o Portal do Azure. Em seguida, localize e selecione um recurso dos serviços Cognitivos. Por exemplo, sua assinatura para a pesquisa do Bing na Web.   
2. Em seguida, no menu de navegação à esquerda, localize **Monitoring** e selecione **configurações de diagnóstico**. Essa tela contém todas as configurações de diagnóstico criadas anteriormente para esse recurso.
3. Se houver um recurso criado anteriormente que você deseja usar, você pode selecioná-lo agora. Caso contrário, selecione **+ Adicionar configuração de diagnóstico**.
4. Insira um nome para a configuração. Em seguida, selecione **arquivar em uma conta de armazenamento** e **enviar para log Analytics**.
5. Quando solicitado a configurar, selecione a conta de armazenamento e o espaço de trabalho do OMS que você deseja usar para armazenar os logs de diagnóstico a você. **Observação**: Se você não tiver uma conta de armazenamento ou o espaço de trabalho do OMS, siga os prompts para criar um.
6. Selecione **auditoria**, **RequestResponse**, e **AllMetrics**. Em seguida, defina o período de retenção para os seus dados de log de diagnóstico. Se uma política de retenção é definida como zero, os eventos para essa categoria de log serão armazenados indefinidamente.
7. Clique em **Salvar**.

Pode levar até duas horas para que os dados de log estão disponíveis para consultar e analisar. Portanto, não se preocupe se você não vir algo imediatamente.

## <a name="view-and-export-diagnostic-data-from-azure-storage"></a>Exibir e exportar dados de diagnóstico do armazenamento do Azure

Armazenamento do Azure é uma solução de armazenamento de objeto robusto que é otimizada para armazenar grandes quantidades de dados não estruturados. Nesta seção, você aprenderá a consultar a sua conta de armazenamento total de transações por um período de tempo de 30 dias e exportar os dados do excel.

1. No portal do Azure, localize o recurso de armazenamento do Azure que você criou na última seção.
2. No menu de navegação à esquerda, localize **Monitoring** e selecione **métricas**.
3. Use os suspensos disponíveis para configurar sua consulta. Neste exemplo, vamos definir o intervalo de tempo como **últimos 30 dias** e a métrica para **transação**.
4. Quando a consulta for concluída, você verá uma visualização da transação nos últimos 30 dias. Para exportar esses dados, use o **exportar para o Excel** botão localizado na parte superior da página.

Saiba mais sobre o que você pode fazer com os dados de diagnóstico no [armazenamento do Azure](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction).

## <a name="view-logs-in-log-analytics"></a>Exibir logs no Log Analytics

Siga estas instruções para explorar os dados do log analytics para o recurso.

1. No portal do Azure, localize e selecione **do Log Analytics** no menu de navegação à esquerda.
2. Localize e selecione o recurso que você criou quando habilitar o diagnóstico.
3. Sob **gerais**, localize e selecione **Logs**. Nessa página, você pode executar consultas em relação a seus logs.

### <a name="sample-queries"></a>Consultas de exemplo

Aqui estão algumas consultas Kusto básicas que você pode usar para explorar seus dados de log.

Execute esta consulta para todos os logs de diagnóstico dos serviços Cognitivos do Azure por um período de tempo especificado:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
```

Execute esta consulta para ver os 10 logs mais recentes:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
| take 10
```

Execute esta consulta para agrupar operações por **recurso**:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES" |
summarize count() by Resource
```
Execute esta consulta para localizar o tempo médio necessário para executar uma operação:

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
| summarize avg(DurationMs)
by OperationName
```

Execute esta consulta para ver o volume de operações ao longo do tempo, divididas por OperationName com contagens guardadas para cada 10 segundos.

```kusto
AzureDiagnostics
| where ResourceProvider == "MICROSOFT.COGNITIVESERVICES"
| summarize count()
by bin(TimeGenerated, 10s), OperationName
| render areachart kind=unstacked
```

## <a name="next-steps"></a>Próximas etapas

* Para entender como habilitar o registro em log e também as categorias de log e métricas com suporte a vários serviços do Azure, leia ambos os [visão geral das métricas](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics) no Microsoft Azure e [visão geral dos Azure Logs de diagnóstico ](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview) artigos.
* Leia estes artigos para saber mais sobre os hubs de eventos:
  * [O que é Hub de Eventos do Azure?](https://docs.microsoft.com/azure/event-hubs/event-hubs-what-is-event-hubs)
  * [Introdução aos Hubs de Evento](https://docs.microsoft.com/azure/event-hubs/event-hubs-csharp-ephcs-getstarted)
* Leia [Baixar métricas e logs de diagnósticos do Armazenamento do Microsoft Azure](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-dotnet#download-blobs).
* Leia [entender pesquisas de log nos logs do Azure Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-log-search-new).
