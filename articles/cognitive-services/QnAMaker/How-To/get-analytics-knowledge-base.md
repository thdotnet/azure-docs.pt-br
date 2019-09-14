---
title: Análise na base de conhecimentos-QnA Maker
titleSuffix: Azure Cognitive Services
description: O QnA Maker armazena todos os logs de chat e outros dados telemétricos, se você habilitou o App Insights durante a criação do serviço do QnA Maker. Execute os exemplos de consultas para obter seus logs de bate-papo do App Insights.
services: cognitive-services
author: diberry
manager: nitinme
displayName: chat history, history, chat logs, logs
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: diberry
ms.openlocfilehash: 5c55084a57e46931049841f5011941b2115e9e69
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70961523"
---
# <a name="get-analytics-on-your-knowledge-base"></a>Obter a análise em sua base de dados de conhecimento

O QnA Maker armazena todos os logs de bate-papo e outros dados de telemetria, se você tiver habilitado o App Insights durante a [criação do seu serviço QnA Maker](./set-up-qnamaker-service-azure.md). Execute os exemplos de consultas para obter seus logs de bate-papo do App Insights.

1. Vá até o recurso do App Insights.

    ![Selecione o seu recurso do Application Insights](../media/qnamaker-how-to-analytics-kb/resources-created.png)

2. Selecione **Log (Analytics)** . Uma nova janela será aberta onde você pode consultar os dados de telemetria do QnA Maker.

3. Cole a consulta a seguir e execute-a.

    ```kusto
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, name, resultCode, duration, performanceBucket
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId
    ) on id
    | extend question = tostring(customDimensions['Question'])
    | extend answer = tostring(customDimensions['Answer'])
    | extend score = tostring(customDimensions['Score'])
    | project timestamp, resultCode, duration, id, question, answer, score, performanceBucket,KbId 
    ```

    Clique em **Executar** para executar a consulta.

    [![Executar a consulta para determinar as perguntas, as respostas e a Pontuação dos usuários](../media/qnamaker-how-to-analytics-kb/run-query.png)](../media/qnamaker-how-to-analytics-kb/run-query.png#lightbox)

## <a name="run-queries-for-other-analytics-on-your-qna-maker-knowledge-base"></a>Executar consultas para outras análises em sua base de dados de conhecimento do QnA Maker

### <a name="total-90-day-traffic"></a>Tráfego total de 90 dias

```kusto
    //Total Traffic
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by bin(timestamp, 1d), KbId
```

### <a name="total-question-traffic-in-a-given-time-period"></a>Tráfego total de perguntas em um determinado período de tempo

```kusto
    //Total Question Traffic in a given time period
    let startDate = todatetime('2018-02-18');
    let endDate = todatetime('2018-03-12');
    requests
    | where timestamp <= endDate and timestamp >=startDate
    | where url endswith "generateAnswer" and name startswith "POST" 
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by KbId
```

### <a name="user-traffic"></a>Tráfego de usuários

```kusto
    //User Traffic
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, name, resultCode, duration
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId 
    ) on id
    | extend UserId = tostring(customDimensions['UserId'])
    | summarize ChatCount=count() by bin(timestamp, 1d), UserId, KbId
```

### <a name="latency-distribution-of-questions"></a>Distribuição de latência das perguntas

```kusto
    //Latency distribution of questions
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | project timestamp, id, name, resultCode, performanceBucket, KbId
    | summarize count() by performanceBucket, KbId
```

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Escolha capactiy](../tutorials/choosing-capacity-qnamaker-deployment.md)
