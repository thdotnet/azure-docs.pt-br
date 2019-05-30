---
title: Logs de Gatilho do Azure Cosmos DB
description: Aprenda a expor os logs de Gatilho do Azure Cosmos DB ao pipeline de registro em log do Azure Functions
author: ealsur
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: maquaran
ms.openlocfilehash: bf5216dc3b296c98176387c6e2cfff7c31daedab
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241037"
---
# <a name="how-to-configure-and-read-the-azure-cosmos-db-trigger-logs"></a>Como configurar e ler os logs do Gatilho do Azure Cosmos DB

Este artigo descreve como é possível configurar seu ambiente do Azure Functions para enviar os logs de Gatilho do Azure Cosmos DB para a [solução de monitoramento](../azure-functions/functions-monitoring.md) configurada.

## <a name="included-logs"></a>Logs incluídos

O Gatilho do Azure Cosmos DB usa a [Biblioteca do Processador do Feed de Alterações](./change-feed-processor.md) internamente e a biblioteca gera um conjunto de logs de integridade que podem ser usados para monitorar operações internas para [fins de solução de problemas](./troubleshoot-changefeed-functions.md).

Os logs de integridade descrevem como o Gatilho do Azure Cosmos DB se comporta ao tentar executar operações durante cenários de balanceamento de carga ou de inicialização.

## <a name="enabling-logging"></a>Como habilitar o registro em log

Para habilitar o registro em log do Gatilho do Azure Cosmos DB, localize o arquivo `host.json` no projeto do Azure Functions ou no aplicativo do Azure Functions e [configure o nível de registro em log necessário](../azure-functions/functions-monitoring.md#log-configuration-in-hostjson). É necessário habilitar os rastreamentos de `Host.Triggers.CosmosDB` conforme mostrado no exemplo a seguir:

```js
{
  "version": "2.0",
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "Host.Triggers.CosmosDB": "Trace"
    }
  }
}
```

Após a implantação da Função do Azure com a configuração atualizada, você verá os logs do Gatilho do Azure Cosmos DB como parte dos seus rastreamentos. É possível exibir os logs em seu provedor de registro em log configurado na *Categoria* `Host.Triggers.CosmosDB`.

## <a name="query-the-logs"></a>Consultar os logs

Execute a seguinte consulta para consultar os logs gerados pelo Gatilho do Azure Cosmos DB na [Análise do Azure Application Insights](../azure-monitor/app/analytics.md):

```sql
traces
| where customDimensions.Category == "Host.Triggers.CosmosDB"
```

## <a name="next-steps"></a>Próximas etapas

* [Habilitar o monitoramento](../azure-functions/functions-monitoring.md) em seus aplicativos do Azure Functions.
* Saiba como [Diagnose and troubleshoot common issues](./troubleshoot-changefeed-functions.md) (Diagnosticar e solucionar problemas comuns) ao usar o Gatilho do Azure Cosmos DB no Azure Functions.