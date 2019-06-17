---
title: Exibir eventos de log de atividades do Azure no Azure Monitor
description: Exibir o log de atividades do Azure no Azure Monitor e recuperar-se com o PowerShell, CLI e API REST.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: 32578f77f2b3f30d80953bdd1099d22c945c640b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66248108"
---
# <a name="view-and-retrieve-azure-activity-log-events"></a>Exibir e recuperar os eventos de log de atividades do Azure

O [Log de atividades do Azure](activity-logs-overview.md) fornece ideias sobre eventos de nível de assinatura que ocorreram no Azure. Este artigo fornece detalhes sobre diferentes métodos para exibir e recuperar os eventos de Log de atividades.

## <a name="azure-portal"></a>Portal do Azure
Exibir o Log de atividades para todos os recursos do **Monitor** menu no portal do Azure. Exibir o Log de atividades para um recurso específico do **Log de atividades** opção no menu daquele recurso.

![Exibir Log de atividades](./media/activity-logs-overview/view-activity-log.png)

Você pode filtrar eventos de Log de atividades pelos seguintes campos:

* **TimeSpan**: A hora de início e término para eventos.
* **Categoria**: A categoria de evento, conforme descrito em [categorias no Log de atividades](activity-logs-overview.md#categories-in-the-activity-log).
* **Assinatura**: Um ou mais nomes de assinatura do Azure.
* **Grupo de recursos**: Um ou mais grupos de recursos em assinaturas selecionadas.
* **Recurso (nome)** :-o nome de um recurso específico.
* **Tipo de recurso**: O tipo de recurso, por exemplo _Compute/virtualmachines_.
* **Nome da operação** -o nome de uma operação do Azure Resource Manager, por exemplo _Microsoft.SQL/servers/Write_.
* **Gravidade**: O nível de severidade do evento. Os valores disponíveis são _informativo_, _aviso_, _erro_, _crítico_.
* **Evento iniciado por**: O usuário que realizou a operação.
* **Abrir pesquisa**: Abra a caixa de pesquisa de texto que procura essa cadeia de caracteres em todos os campos em todos os eventos.

### <a name="view-change-history"></a>Exibir histórico de alterações

Ao revisar o Log de atividades, ele pode ajudar a determinar o que aconteceu alterações durante essa hora do evento. Você pode exibir essas informações com **histórico de alterações**. Selecione um evento do Log de atividades que você deseja examinar mais profundamente em. Selecione o **(visualização) do histórico de alterações** guia para exibir qualquer associado alterações com esse evento.

![Lista de histórico de alteração para um evento](media/activity-logs-overview/change-history-event.png)

Se houver quaisquer alterações associadas com o evento, você verá uma lista das alterações que você pode selecionar. Isso abre o **(visualização) do histórico de alterações** página. Nesta página você ver as alterações para o recurso. Como você pode ver no exemplo a seguir, somos capazes de ver não apenas que a VM alterado tamanhos, mas o que o tamanho da VM anterior era antes da alteração e o que ela foi alterada para.

![Página de histórico de alteração, mostrando as diferenças](media/activity-logs-overview/change-history-event-details.png)

Para saber mais sobre o histórico de alterações, consulte [obter alterações de recurso](../../governance/resource-graph/how-to/get-resource-changes.md).


## <a name="log-analytics-workspace"></a>Espaço de trabalho do Log Analytics
Clique em **Logs** na parte superior das **Log de atividades** página para abrir o [análise de Log de atividades, solução de monitoramento](activity-log-collect.md) para a assinatura. Isso permitirá que você exibir a análise de Log de atividades e executar [consultas de log](../log-query/log-query-overview.md) com o **AzureActivity** tabela. Se seu Log de atividades não está conectado a um espaço de trabalho do Log Analytics, você precisará executar essa configuração.



## <a name="powershell"></a>PowerShell
Use o [Get-AzLog](https://docs.microsoft.com/powershell/module/az.monitor/get-azlog) cmdlet para recuperar o Log de atividades do PowerShell. A seguir estão alguns exemplos comuns.

> [!NOTE]
> `Get-AzLog` fornece apenas 15 dias de histórico. Use o **- MaxEvents** parâmetro para consultar os últimos N eventos além de 15 dias. Para acessar os eventos com mais de 15 dias, use a API REST ou SDK. Se você não incluir **StartTime**, o valor padrão será **EndTime** menos uma hora. Se você não incluir **EndTime**, o valor padrão será a hora atual. Todas as horas estão no padrão UTC.


Obter entradas de log criadas após uma determinada data e hora:

```powershell
Get-AzLog -StartTime 2016-03-01T10:30
```

Obter entradas de log entre um intervalo de tempo de data:

```powershell
Get-AzLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Obter entradas de log de um grupo de recursos específico:

```powershell
Get-AzLog -ResourceGroup 'myrg1'
```

Obter entradas de log de um provedor de recursos específico entre um intervalo de tempo de data:

```powershell
Get-AzLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Obter entradas de log com um chamador específico:

```powershell
Get-AzLog -Caller 'myname@company.com'
```

Obter os últimos 1000 eventos:

```powershell
Get-AzLog -MaxEvents 1000
```


## <a name="cli"></a>CLI
Use [log de atividades do monitor az](cli-samples.md#view-activity-log-for-a-subscription) para recuperar o Log de atividades de CLI. A seguir estão alguns exemplos comuns.


Exiba todas as opções disponíveis.

```azurecli
az monitor activity-log list -h
```

Obter entradas de log de um grupo de recursos específico:

```azurecli
az monitor activity-log list --resource-group <group name>
```

Obter entradas de log com um chamador específico:

```azurecli
az monitor activity-log list --caller myname@company.com
```

Obter logs por chamador em um tipo de recurso, dentro de um intervalo de datas:

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="rest-api"></a>API REST
Use o [API de REST do Azure Monitor](https://docs.microsoft.com/rest/api/monitor/) para recuperar o Log de atividades de um cliente REST. A seguir estão alguns exemplos comuns.

Obter Logs de atividade com filtro:

``` HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2018-01-21T20:00:00Z' and eventTimestamp le '2018-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'
```

Obter Logs de atividade com o filtro e selecione:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2015-01-21T20:00:00Z' and eventTimestamp le '2015-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Obter Logs de atividade com select:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Obter Logs de atividade sem o filtro ou selecione:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01
```


## <a name="next-steps"></a>Próximas etapas

* [Leia uma visão geral do Log de atividades](activity-logs-overview.md)
* [Arquivar o Log de atividades para o armazenamento ou transmita-o para os Hubs de eventos](activity-log-export.md)
* [Transmissão do Log de Atividades do Azure para os Hubs de Eventos](activity-logs-stream-event-hubs.md)
* [Arquivar o Log de atividades do Azure para armazenamento](archive-activity-log.md)

