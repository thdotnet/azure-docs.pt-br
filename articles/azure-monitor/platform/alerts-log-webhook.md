---
title: Ações de Webhook para alertas de log no alertas do Azure
description: Este artigo descreve como criar uma regra de alerta de log usando o espaço de trabalho do Log Analytics ou Application Insights, como o alerta envia dados como um webhook de HTTP e os detalhes das diferentes personalizações possíveis.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: 6aa007c621e76cb0c188a7dab6279fd9e387b2b3
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705183"
---
# <a name="webhook-actions-for-log-alert-rules"></a>Ações de webhook para regras de alerta do log
Quando um [o alerta de log é criado no Azure](alerts-log.md), você tem a opção de [configurá-lo por meio de grupos de ação](action-groups.md) para executar uma ou mais ações. Este artigo descreve as ações de webhook diferentes que estão disponíveis e mostra como configurar um webhook personalizado baseado em JSON.

> [!NOTE]
> Você também pode usar o [esquema de alerta comum](https://aka.ms/commonAlertSchemaDocs) para suas integrações de webhook. O esquema de alerta comum tem a vantagem de ter uma única carga alerta extensível e unificada em todos os serviços alerta no Azure Monitor. [Saiba mais sobre as definições de alerta de esquema comuns.](https://aka.ms/commonAlertSchemaDefinitions)

## <a name="webhook-actions"></a>Ações de Webhook

Com as ações de webhook, você pode invocar um processo externo por meio de uma única solicitação HTTP POST. O serviço que é chamado deve dar suporte a webhooks e determinar como usar qualquer conteúdo que ele recebe.

As ações de webhook exigem as propriedades indicadas na tabela a seguir.

| Propriedade | DESCRIÇÃO |
|:--- |:--- |
| **URL de Webhook** |A URL do webhook. |
| **Carga JSON personalizada** |A carga personalizada para enviar com o webhook quando esta opção for escolhida durante a criação do alerta. Para obter mais informações, consulte [gerenciar alertas de log](alerts-log.md).|

> [!NOTE]
> O **exibição Webhook** botão junto com o **incluir conteúdo JSON personalizado para webhook** opção para o alerta de log exibe o conteúdo do webhook de exemplo para a personalização que foi fornecido. Ele não contém dados reais, mas é representativo do esquema JSON que é usado para alertas do log. 

Webhooks incluem uma URL e uma carga formatada em JSON, que os dados enviados para o serviço externo. Por padrão, a carga inclui os valores na tabela a seguir. Você pode optar por substituir essa carga por uma personalizado de sua preferência. Nesse caso, use as variáveis na tabela para cada um dos parâmetros para incluir seus valores na carga personalizada.


| Parâmetro | Variável | DESCRIÇÃO |
|:--- |:--- |:--- |
| *AlertRuleName* |#alertrulename |Nome da regra de alerta. |
| *Severidade* |#severity |Severidade definida para o alerta do log disparado. |
| *AlertThresholdOperator* |#thresholdoperator |Operador de limite para a regra de alerta, que usa o maior ou menor que. |
| *AlertThresholdValue* |#thresholdvalue |O valor de limite para a regra de alerta. |
| *LinkToSearchResults* |#linktosearchresults |Link para o portal de análise que retorna os registros da consulta que criou o alerta. |
| *ResultCount* |#searchresultcount |Número de registros nos resultados da pesquisa. |
| *Hora de término do intervalo de pesquisa* |#searchintervalendtimeutc |Hora de término para a consulta em UTC, com o formato mm/dd/aaaa hh: mm: ss AM/PM. |
| *Intervalo de pesquisa* |#searchinterval |Janela de tempo para a regra de alerta, com o formato hh: mm ss. |
| *Hora de início de intervalo de pesquisa* |#searchintervalstarttimeutc |Hora de início para a consulta em UTC, com o formato mm/dd/aaaa hh: mm: ss AM/PM. 
| *SearchQuery* |#searchquery |A consulta da pesquisa de log usada pela regra de alerta. |
| *SearchResults* |"IncludeSearchResults": true|Registros retornados pela consulta como uma tabela JSON, limitada aos primeiros 1.000 registros, se "IncludeSearchResults": true for adicionado em uma definição personalizada do webhook JSON como uma propriedade de nível superior. |
| *Tipo de alerta*| #alerttype | O tipo de regra de alerta de log configurado como [medição métrica](alerts-unified-log.md#metric-measurement-alert-rules) ou [número de resultados](alerts-unified-log.md#number-of-results-alert-rules).|
| *WorkspaceID* |#workspaceid |ID do seu espaço de trabalho do Log Analytics. |
| *ID do aplicativo* |#applicationid |ID de suas ideias de aplicativo app. |
| *ID da assinatura* |#subscriptionid |ID da assinatura do Azure usada. 

> [!NOTE]
> *LinkToSearchResults* passa parâmetros, como *SearchQuery*, *StartTime de intervalo de pesquisa*, e *hora de término do intervalo de pesquisa* na URL para o Azure Portal para exibição na seção de análise. O portal do Azure tem um limite de tamanho do URI de aproximadamente 2.000 caracteres. O portal será *não* abrir links fornecidas nos alertas se os valores de parâmetro excederem o limite. Você pode inserir manualmente os detalhes para exibir os resultados no portal do Analytics. Ou, você pode usar o [API REST do Application Insights Analytics](https://dev.applicationinsights.io/documentation/Using-the-API) ou o [API REST do Log Analytics](/rest/api/loganalytics/) para recuperar os resultados por meio de programação. 

Por exemplo, você pode especificar a seguinte carga personalizada que inclui um único parâmetro chamado *text*. O serviço que chama esse webhook espera que esse parâmetro.

```json

    {
        "text":"#alertrulename fired with #searchresultcount over threshold of #thresholdvalue."
    }
```
Essa carga de exemplo resolve para algo semelhante ao seguinte quando ele é enviado para o webhook:

```json
    {
        "text":"My Alert Rule fired with 18 records over threshold of 10 ."
    }
```
Porque todas as variáveis em um webhook personalizado devem ser especificadas dentro de um compartimento JSON, como "#searchinterval", o webhook resultante também tem dados de variável dentro de compartimentos, como "00: 05:00."

Para incluir os resultados da pesquisa em uma carga personalizada, certifique-se de que **IncludeSearchResults** é definido como uma propriedade de nível superior na carga JSON. 

## <a name="sample-payloads"></a>Cargas de exemplo
Esta seção mostra as cargas de exemplo para webhooks para alertas do log. As cargas de exemplo incluem exemplos de quando a carga é padrão e quando ele é personalizado.

### <a name="standard-webhook-for-log-alerts"></a>Padrão do webhook para alertas do log 
Ambos os exemplos têm uma carga fictícia com apenas duas colunas e duas linhas.

#### <a name="log-alert-for-log-analytics"></a>Alerta do log para o Log Analytics
O conteúdo de exemplo a seguir é para uma ação padrão do webhook *sem uma opção JSON personalizado* que é usado para alertas com base no Log Analytics:

```json
{
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule",
    "SearchQuery":"Perf | where ObjectName == \"Processor\" and CounterName == \"% Processor Time\" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5m), Computer",
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://portal.azure.com/#Analyticsblade/search/index?_timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": "log alert rule",
    "Severity": "Warning",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "WorkspaceId":"12345a-1234b-123c-123d-12345678e",
    "AlertType": "Metric measurement"
 }
 ```

> [!NOTE]
> O valor do campo "Severity" pode mudar se você já [alternado de sua preferência de API](alerts-log-api-switch.md) para alertas do log no Log Analytics.


#### <a name="log-alert-for-application-insights"></a>Alerta do log para o Application Insights
O conteúdo de exemplo a seguir é para um padrão do webhook *sem uma opção JSON personalizado* quando ele é usado para alertas do log com base no Application Insights:
    
```json
{
    "schemaId":"Microsoft.Insights/LogAlert","data":
    { 
    "SubscriptionId":"12345a-1234b-123c-123d-12345678e",
    "AlertRuleName":"AcmeRule",
    "SearchQuery":"requests | where resultCode == \"500\"",
    "SearchIntervalStartTimeUtc": "2018-03-26T08:10:40Z",
    "SearchIntervalEndtimeUtc": "2018-03-26T09:10:40Z",
    "AlertThresholdOperator": "Greater Than",
    "AlertThresholdValue": 0,
    "ResultCount": 2,
    "SearchIntervalInSeconds": 3600,
    "LinkToSearchResults": "https://portal.azure.com/AnalyticsBlade/subscriptions/12345a-1234b-123c-123d-12345678e/?query=search+*+&timeInterval.intervalEnd=2018-03-26T09%3a10%3a40.0000000Z&_timeInterval.intervalDuration=3600&q=Usage",
    "Description": null,
    "Severity": "3",
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        },
    "ApplicationId": "123123f0-01d3-12ab-123f-abc1ab01c0a1",
    "AlertType": "Number of results"
    }
}
```

#### <a name="log-alert-with-custom-json-payload"></a>Alerta do log com conteúdo JSON personalizado
Por exemplo, para criar uma carga personalizada que inclui apenas o nome do alerta e os resultados da pesquisa, você pode usar o seguinte: 

```json
    {
       "alertname":"#alertrulename",
       "IncludeSearchResults":true
    }
```

O conteúdo de exemplo a seguir é para uma ação de webhook personalizados para qualquer alerta do log:
    
```json
    {
    "alertname":"AcmeRule","IncludeSearchResults":true,
    "SearchResult":
        {
        "tables":[
                    {"name":"PrimaryResult","columns":
                        [
                        {"name":"$table","type":"string"},
                        {"name":"Id","type":"string"},
                        {"name":"TimeGenerated","type":"datetime"}
                        ],
                    "rows":
                        [
                            ["Fabrikam","33446677a","2018-02-02T15:03:12.18Z"],
                            ["Contoso","33445566b","2018-02-02T15:16:53.932Z"]
                        ]
                    }
                ]
        }
    }
```


## <a name="next-steps"></a>Próximas etapas
- Saiba mais sobre [alertas de log nos alertas do Azure](alerts-unified-log.md).
- Entender como [gerenciar alertas de log no Azure](alerts-log.md).
- Criar e gerenciar [grupos de ações no Azure](action-groups.md).
- Saiba mais sobre o [Application Insights](../../azure-monitor/app/analytics.md).
- Saiba mais sobre [registrar consultas](../log-query/log-query-overview.md). 

