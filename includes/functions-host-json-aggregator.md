---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: da0cbab59d9c801419ac4b3704fee3275f337fd9
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67172076"
---
Especifica quantas invocações de função são agregadas ao [calcular métricas para o Application Insights](../articles/azure-functions/functions-monitoring.md#configure-the-aggregator). 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Propriedade |Padrão  | DESCRIÇÃO |
|---------|---------|---------| 
|batchSize|1000|Número máximo de solicitações para agregação.| 
|flushTimeout|00:00:30|Período de tempo máximo para agregação.| 

As invocações de função são agregadas quando o primeiro dos dois limites é atingido.