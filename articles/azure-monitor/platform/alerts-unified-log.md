---
title: Alertas de log no Azure Monitor
description: Dispare emails, notificações, chame URLs de sites (webhooks) ou automação quando as condições de consulta analítica especificadas forem atendidas para os Alertas do Azure.
author: msvijayn
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 5/31/2019
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: ae35c735cffeb8cd85af1f32bb2d14ede6dc6b69
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66427420"
---
# <a name="log-alerts-in-azure-monitor"></a>Alertas de log no Azure Monitor

Este artigo fornece detalhes sobre os alertas de Log, que são um dos tipos de alertas com suporte nos [Alertas do Azure](../../azure-monitor/platform/alerts-overview.md) e que permitem que os usuários usem a plataforma de análise do Azure como base para alertas.

O Alerta de Log consiste em regras de Pesquisa de Logs criadas para o [Azure Monitor Logs](../../azure-monitor/learn/tutorial-viewdata.md) ou o [Application Insights](../../azure-monitor/app/cloudservices.md#view-azure-diagnostics-events). Para saber mais sobre seu uso, consulte [Criar alertas de log no Azure](../../azure-monitor/platform/alerts-log.md)

> [!NOTE]
> Os dados de log populares do [Azure Monitor Logs](../../azure-monitor/learn/tutorial-viewdata.md) agora também estão disponíveis na plataforma de métricas no Azure Monitor. Para obter uma exibição detalhada, confira [Alerta de métrica para logs](../../azure-monitor/platform/alerts-metric-logs.md)


## <a name="log-search-alert-rule---definition-and-types"></a>Regra de alerta de pesquisa de log - definição e tipos

As regras de pesquisa de log são criadas pelos Alertas do Azure para executar consultas de log automaticamente em intervalos regulares.  Se os resultados da consulta de log corresponderem a critérios específicos, um registro de alerta será criado. A regra pode executar automaticamente uma ou mais ações usando os [Grupos de Ação](../../azure-monitor/platform/action-groups.md). A função [Colaborador do Monitoramento do Azure](../../azure-monitor/platform/roles-permissions-security.md) pode ser necessária para criar, modificar e atualizar alertas de log e direitos de acesso e execução de consulta para os destinos de análise na regra de alerta ou na consulta de alerta. No caso de criação de usuário não tem acesso a todos os destinos de análise na regra de alerta ou consulta de alerta - a criação da regra pode falhar ou a regra de alerta do log será executada com resultados parciais.

As regras de pesquisa de log são definidas pelos detalhes a seguir:

- **Consulta de log**.  A consulta que é executada cada vez que a regra de alerta é acionada.  Os registros retornados por essa consulta são usados para determinar se um alerta é disparado. A consulta do Analytics pode ser para um workspace específico do Log Analytics ou para um aplicativo do Application Insights e, até mesmo, abranger [vários recursos do Log Analytics e do Application Insights](../../azure-monitor/log-query/cross-workspace-query.md#querying-across-log-analytics-workspaces-and-from-application-insights), desde que o usuário tenha direitos de acesso, bem como de consulta a todos os recursos. 
    > [!IMPORTANT]
    > [consulta de recursos cruzados](../../azure-monitor/log-query/cross-workspace-query.md#querying-across-log-analytics-workspaces-and-from-application-insights) suporte em alertas de log para o Application Insights e log de alertas para [configurado usando scheduledQueryRules API do Log Analytics](../../azure-monitor/platform/alerts-log-api-switch.md) apenas.

    Alguns comandos e combinações de análise são incompatíveis com o uso em alertas de log; para obter mais detalhes, confira [Consultas de alertas de log no Azure Monitor](../../azure-monitor/platform/alerts-log-query.md).

- **Período de tempo**.  Especifica o intervalo de tempo para a consulta. A consulta retorna somente os registros que foram criados dentro desse intervalo de tempo atual. O período de tempo restringe os dados buscados para consulta de log para evitar abusos e contorna qualquer comando de tempo (como atrás) usados na consulta de log. <br>*Por exemplo, se o período de tempo está definido para 60 minutos e a consulta é executada às 13h15, somente os registros criados entre 12h15 e 13h15 são retornados para executar a consulta de log. Agora, se a consulta de log usa a hora como o comando atrás (7d), a consulta de log deve ser executada somente para dados entre 12h15 e 13h15 - como se os dados existissem para somente os últimos 60 minutos. E não por sete dias de dados conforme especificado na consulta de log.*

- **Frequência**.  Especifica a frequência com que a consulta deve ser executada. Pode ser qualquer valor entre 5 minutos e 24 horas. Deve ser igual a ou menor que o período de tempo.  Se o valor for maior que o período de tempo, haverá o risco de que registros sejam perdidos.<br>*Por exemplo, considere um período de tempo de 30 minutos e uma frequência de 60 minutos.  Se a consulta for executada à 1:00, retornará registros entre 12:30 e 1:00.  A próxima vez em que a consulta será executada é às 2:00, quando ela retornará registros entre 1:30 e 2:00.  Todos os registros criados entre 13h00 e 13h30 nunca seriam avaliados.*

- **Limite**.  Os resultados da pesquisa de logs são avaliados para determinar se um alerta deve ser criado.  O limite é diferente para os diferentes tipos de regras de alerta da pesquisa de logs.

As regras de pesquisa de logs para o [Logs do Azure Monitor](../../azure-monitor/learn/tutorial-viewdata.md) ou o [Application Insights](../../azure-monitor/app/cloudservices.md#view-azure-diagnostics-events) podem ser de dois tipos. Cada um desses tipos é descrito detalhadamente nas seções a seguir.

- **[Número de resultados](#number-of-results-alert-rules)** . Alerta único criado quando o número de registros retornados pela pesquisa de logs excedeu um número especificado.
- **[Medida métrica](#metric-measurement-alert-rules)** .  Alerta criado para cada objeto nos resultados da pesquisa de logs com valores que excedem o limite especificado.

As diferenças entre tipos de regra de alerta são conforme descrito a seguir.

- A regra de alerta *Número de resultados* sempre cria um único tempo de alerta, enquanto a regra de alerta *Medição métrica* cria um alerta para cada objeto que exceder o limite.
- Regras de alerta de *Número de resultados* criarão um alerta quando o limite for excedido uma única vez. Regras de alerta de *Medição métrica* podem criar um alerta quando o limite é excedido um determinado número de vezes em um intervalo de tempo específico.

### <a name="number-of-results-alert-rules"></a>Regras de alerta de Número de resultados

Regras de alerta de **Número de resultados** criam um único alerta quando o número de registros retornados pela consulta de pesquisa excede o limite especificado. Esse tipo de regra de alerta é ideal para trabalhar com eventos como logs de eventos do Windows, Syslog, WebApp Response e Logs Personalizados.  Pode ser útil criar um alerta quando um evento de erro específico é criado ou quando vários eventos de erros são criados dentro de um período de tempo específico.

**Limite**: O limite para uma regra de alerta de Número de resultados é maior ou menor que um valor específico.  Se o número de registros retornados pela pesquisa de logs corresponderem a esses critérios, um alerta será criado.

Para alertar sobre um único evento, defina o número de resultados para maior que 0 e verifique a ocorrência de um único evento que foi criado desde a última vez em que a consulta foi executada. Alguns aplicativos podem registrar um erro ocasional que não necessariamente gerará um alerta.  Por exemplo, o aplicativo pode repetir o processo que criou o evento de erro e depois ter êxito na próxima vez.  Nesse caso, não convém criar o alerta, a menos que vários eventos sejam criados dentro de um período de tempo específico.  

Em alguns casos, pode ser útil criar um alerta na ausência de um evento.  Por exemplo, um processo pode registrar eventos regulares para indicar que ele está funcionando corretamente.  Se ele não registrar um desses eventos dentro de um período de tempo específico, um alerta deverá ser criado.  Nesse caso você definiria o limite como **menos de 1**.

#### <a name="example-of-number-of-records-type-log-alert"></a>Exemplo de alerta de log do tipo de Número de Registros

Considere um cenário onde você deseja saber quando seu aplicativo baseado na web fornece uma resposta para os usuários com o código de 500 (ou seja) erro interno do servidor. Você criaria uma regra de alerta com os detalhes a seguir:  

- **Consulta:** solicitações | onde resultCode = = "500"<br>
- **Período de Tempo:** 30 minutos<br>
- **Frequência de alerta:** cinco minutos<br>
- **Valor do Limite:** Maior que 0<br>

Alerta deve executar a consulta a cada 5 minutos, com 30 minutos de dados - procurar qualquer registro onde o código de resultado foi 500. Se mesmo um registro desse tipo é encontrado, ele aciona o alerta e dispara a ação configurada.

### <a name="metric-measurement-alert-rules"></a>Regras de alerta com medição métrica

**Medição métrica** regras de alerta criar um alerta para cada objeto em uma consulta com um valor que excede um limite especificado e as condições de gatilho especificadas. Diferentemente **número de resultados** regras de alerta **medição métrica** regras de alerta funcionam quando o resultado da análise fornece uma série de tempo. Eles têm as diferenças distintas a seguir em relação às regras de alerta de **Número de resultados**.

- **Função de agregação**: Determina o cálculo que será realizado e, potencialmente, um campo numérico a agregar.  Por exemplo, **count()** retornará o número de registros na consulta e **avg(CounterValue)** retornará a média do campo CounterValue durante o intervalo. Função de agregação na consulta deve ser nomeada/chamada: AggregatedValue e fornecer um valor numérico. 

- **Campo Grupo**: Um registro com um valor agregado será criado para cada instância do campo e um alerta pode ser gerado para cada um deles.  Por exemplo, se você quisesse gerar um alerta para cada computador, você usaria **by Computer**. Caso haja vários campos de grupo especificados na consulta do alerta, o usuário pode especificar qual campo deve ser usado para classificar os resultados usando o parâmetro **Agregar em** (metricColumn)

    > [!NOTE]
    > A opção *Agregar em* (metricColumn) está disponível para alertas de log do tipo Medição métrica para o Application Insights e registra alertas somente para o [Log Analytics configurado usando a API scheduledQueryRules](../../azure-monitor/platform/alerts-log-api-switch.md).

- **Intervalo**:  Define o intervalo de tempo durante o qual os dados são agregados.  Por exemplo, se você especificasse **cinco minutos**, um registro seria criado para cada instância do campo de grupo agregada em intervalos de 5 minutos durante o período de tempo especificado para o alerta.

    > [!NOTE]
    > A função bin deve ser usada na consulta para especificar o intervalo. Como bin() pode resultar em intervalos de tempo diferentes, o alerta converterá automaticamente o comando bin em um comando bin_at com o tempo de execução apropriado, a fim de garantir resultados com um ponto fixo. O tipo Medição métrica do alerta de log foi criado para funcionar com consultas que têm até três instâncias do comando bin()
    
- **Limite**: O limite para regras de alerta de Medição métrica é definido por um valor de agregação e um número de violações.  Se qualquer ponto de dados na pesquisa de logs exceder esse valor, ele será considerado uma violação.  Se o número de violações de qualquer objeto nos resultados exceder o valor especificado, um alerta será criado para esse objeto.

A configuração incorreta da opção *Agregar em* ou *metricColumn* pode fazer com que as regras de alerta sejam disparadas incorretamente. Para saber mais, confira [Solução de problemas quando a regra de alerta com medição métrica está incorreta](alert-log-troubleshoot.md#metric-measurement-alert-rule-is-incorrect).

#### <a name="example-of-metric-measurement-type-log-alert"></a>Exemplo de alerta de log do tipo Medida da Métrica

Considere um cenário em que você deseje um alerta se qualquer computador exceda 90% de utilização do processador por três vezes em 30 minutos.  Você criaria uma regra de alerta com os detalhes a seguir:  

- **Consulta:** Perf | where ObjectName == "Processor" and CounterName == "% Processor Time" | summarize AggregatedValue = avg(CounterValue) by bin(TimeGenerated, 5m), Computer<br>
- **Período de Tempo:** 30 minutos<br>
- **Frequência de alerta:** cinco minutos<br>
- **Lógica de alerta – condição e limite:** Maior do que 90<br>
- **Campo Grupo (agregar em):** Computador
- **Disparar alerta com base em:** Total de violações maior que 2<br>

A consulta criaria um valor médio para cada computador em intervalos de cinco minutos.  Essa consulta seria executada cada cinco minutos para os dados coletados durante os 30 minutos anteriores. Uma vez que o campo de grupo (agregar em) escolhido é o 'Computador' colunar – o AggregatedValue é dividido para vários valores de 'Computador' e utilização média do processador para cada computador é determinada para um compartimento temporal de 5 minutos.  O resultado da consulta de exemplo para (digamos) três computadores seria conforme mostrado abaixo.


|TimeGenerated [UTC] |Computador  |AggregatedValue  |
|---------|---------|---------|
|20xx-xx-xxT01:00:00Z     |   srv01.contoso.com      |    72     |
|20xx-xx-xxT01:00:00Z     |   srv02.contoso.com      |    91     |
|20xx-xx-xxT01:00:00Z     |   srv03.contoso.com      |    83     |
|...     |   ...      |    ...     |
|20xx-xx-xxT01:30:00Z     |   srv01.contoso.com      |    88     |
|20xx-xx-xxT01:30:00Z     |   srv02.contoso.com      |    84     |
|20xx-xx-xxT01:30:00Z     |   srv03.contoso.com      |    92     |

Se o resultado da consulta precisasse ser plotado, ele apareceria como.

![Resultados da consulta de exemplo](media/alerts-unified-log/metrics-measurement-sample-graph.png)

Neste exemplo, podemos ver em compartimentos de 5 minutos para cada um dos três computadores – utilização média de processador conforme computada para 5 minutos. Limite de 90 sendo violado por srv01 apenas uma vez no compartimento 1:25. Em comparação, srv02 excede o limite de 90 nos compartimentos 1:10, 1:15 e 1:25; enquanto srv03 excede o limite de 90 em 1:10, 1:15, 1:20 e 1:30.
Já que o alerta é configurado para disparar com base em um total de violações que ultrapasse duas, vemos que apenas srv02 e srv03 atendem aos critérios. Assim, alertas separados seriam criados para srv02 e srv03, já que eles violaram o limite de 90% duas vezes durante vários compartimentos temporais.  Se o parâmetro *Disparar alerta com base em:* fosse, em vez disso, configurado para a opção *Violações contínuas*, um alerta seria acionado **apenas** para srv03, já que ele violou o limite para três compartimentos temporais consecutivos, de 1:10 a 1:20. Isso já **não** ocorreria para srv02, pois ele violou o limite em dois compartimentos temporais consecutivos, de 1:10 a 1:15.

## <a name="log-search-alert-rule---firing-and-state"></a>Regra de alerta de pesquisa de logs – acionamento e estado

A regra de alerta de pesquisa de logs funciona com a lógica definida pelo usuário, de acordo com a configuração e a consulta de análise personalizada usada. Desde a lógica de monitoramento incluindo a condição exata ou o motivo por que a regra de alerta deve disparar é encapsulado em uma consulta do analytics - que pode ser diferente em cada regra de alerta do log. Alertas do Azure tem informações escassos da causa raiz subjacente específica (ou) cenário que está sendo avaliada quando a condição de regra de alerta de pesquisa de log de limite é atingida ou excedida. Portanto, alertas de log são conhecidos como sem estado. E regras de alerta do log serão manter disparando, desde que a condição de alerta é atendida pelo resultado da consulta de análise personalizada fornecida. Sem o alerta cada obtendo resolvido, como a lógica a causa exata da falha de monitoramento é mascarada dentro da consulta do analytics fornecida pelo usuário. Existir no momento, nenhum mecanismo para alertas do Azure Monitor conclusivamente deduzir a causa raiz que está sendo resolvida.

Nos permite ver os mesmos, com um exemplo prático. Suponha que você tenha uma regra de alerta de log chamada *alerta do Log de Contoso*, de acordo com a configuração na [exemplo fornecido para o alerta de log do tipo de número de resultados](#example-of-number-of-records-type-log-alert) – em que a consulta de alerta personalizada foi projetada para procurar por 500 código de resultado nos logs.

- Em 1:05 PM quando o alerta do Log de Contoso foi executado pelos alertas do Azure, o resultado da pesquisa de log gerou zero registro com o código de resultado com 500. Uma vez que o zero está abaixo do limite e o alerta não é disparado.
- Na próxima iteração em 1:10 PM quando o alerta do Log de Contoso foi executado pelos alertas do Azure, o resultado da pesquisa de log fornecido cinco registros com o código de resultado como 500. Uma vez que cinco excede o limite e o alerta é disparado com as ações associadas será disparado.
- Em 1:15 PM quando o alerta do Log de Contoso foi executado pelos alertas do Azure, o resultado da pesquisa de log fornecida dois registros com o código de resultado de 500. Uma vez que dois excede o limite e o alerta é disparado com as ações associadas será disparado.
- Agora na próxima iteração em 1:20 PM quando o alerta do Log de Contoso foi executado por um alerta do Azure, o resultado da pesquisa de log fornecida novamente zero registro com o código de resultado de 500. Uma vez que o zero está abaixo do limite e o alerta não é disparado.

Mas, no caso de listados acima, a 1:15 PM - alertas do Azure não podem determinar que os problemas subjacentes vistos em 1:10 persistirem e se há novas falhas net. Como a consulta fornecida pelo usuário pode levando em conta os registros anteriores - alertas do Azure podem ter certeza. Desde que a lógica para o alerta é encapsulada na consulta de alerta - portanto, os dois registros com o código de resultado 500 visto em 1:15 PM podem ou não podem ser vistos já às 1: 22H. Portanto, para err com cautela, quando o alerta de Log Contoso é executado no 1:15 PM, ação configurada é acionada novamente. Agora em 1:20 PM quando zero registro são vistos com código de resultado 500 - alertas do Azure não podem ter certeza de que a causa do código de resultado 500 visto em 1: 22H00 e 1:15 PM agora está resolvida e alertas do Azure Monitor com confiança podem deduzir os problemas de 500 Erro não ocorrerá pela mesma razão s novamente. Portanto, o alerta do Log de Contoso será não alterado para resolvido no painel de alerta do Azure e/ou as notificações enviadas informando que a resolução do alerta. Em vez disso, o usuário que compreende a condição exata ou o motivo para a lógica inserida na consulta do analytics, é possível [marcar alerta como fechado](alerts-managing-alert-states.md) conforme necessário.

## <a name="pricing-and-billing-of-log-alerts"></a>Preços e cobrança dos Alertas de Log

Preços aplicáveis aos Alertas de Log estão disponíveis na página [Preços do Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/). Nas listas do Azure, os Alertas de Log são representados como tipo `microsoft.insights/scheduledqueryrules` com:

- Alertas de log no Application Insights mostrados com o nome exato do alerta, juntamente com o grupo de recursos e propriedades do alerta
- Alertas de log no Log Analytics mostrados com o nome exato do alerta, juntamente com o grupo de recursos e propriedades do alerta, quando criados usando [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules)

A [API herdada do Log Analytics](../../azure-monitor/platform/api-alerts.md) tem ações de alerta e agendamentos como parte de Pesquisa Salva do Log Analytics e não [Recursos do Azure](../../azure-resource-manager/resource-group-overview.md) adequados. Portanto, para habilitar a cobrança para esses alertas de log herdados criados para o Log Analytics usando o portal do Azure **sem** [mudar para a nova API](../../azure-monitor/platform/alerts-log-api-switch.md) ou via [API herdada do Log Analytics](../../azure-monitor/platform/api-alerts.md), regras de pseudoalerta ocultas são criadas no `microsoft.insights/scheduledqueryrules` para cobrança no Azure. As regras de pseudoalerta oculta criadas para cobrança em `microsoft.insights/scheduledqueryrules` conforme mostrado em `<WorkspaceName>|<savedSearchId>|<scheduleId>|<ActionId>` juntamente com propriedades de alerta e grupo de recursos.

> [!NOTE]
> Se caracteres inválidos como `<, >, %, &, \, ?, /` estiverem presentes, eles serão substituídos por `_` no nome da regra de pseudoalerta oculta e, portanto, também na fatura do Azure.

Remova os recursos ocultos scheduleQueryRules criados para cobrança de regras de alerta usando [API herdada do Log Analytics](api-alerts.md), o usuário pode executar qualquer uma das seguintes ações:

- Qualquer usuário pode [alternar a preferência de API para as regras de alerta no workspace do Log Analytics](../../azure-monitor/platform/alerts-log-api-switch.md) e sem perda de suas regras de alerta ou movimentação de monitoramento para a [API scheduledQueryRules](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) em conformidade com o Azure Resource Manager. Isso elimina a necessidade de regras de pseudoalerta ocultas para cobrança.
- Ou se o usuário não desejar mudar a preferência de API, ele precisará **excluir** a agenda original e a ação de alerta usando a [API herdada do Log Analytics](api-alerts.md) ou excluir no [portal do Azure a regra de alerta de log original](../../azure-monitor/platform/alerts-log.md#view--manage-log-alerts-in-azure-portal)

Além para recursos ocultos scheduleQueryRules criados para cobrança de regras de alerta usando [API herdada do Log Analytics](api-alerts.md), qualquer operação de modificação, como PUT falhará. Como o `microsoft.insights/scheduledqueryrules` regras de tipo de pseudo são para fins de cobrança criadas usando as regras de alerta [API herdada do Log Analytics](api-alerts.md). Qualquer modificação da regra de alerta deve ser feita usando [API herdada do Log Analytics](api-alerts.md) (ou), o usuário pode [alternar preferência de API para as regras de alerta](../../azure-monitor/platform/alerts-log-api-switch.md) usar [scheduledQueryRules API](https://docs.microsoft.com/rest/api/monitor/scheduledqueryrules) em vez disso.

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre a [criação de alertas de log no Azure](../../azure-monitor/platform/alerts-log.md).
* Entenda os [webhooks nos alertas de log no Azure](alerts-log-webhook.md).
* Saiba mais sobre os [Alertas do Azure](../../azure-monitor/platform/alerts-overview.md).
* Saiba mais sobre o [Application Insights](../../azure-monitor/app/analytics.md).
* Saiba mais sobre o [Log Analytics](../../azure-monitor/log-query/log-query-overview.md).