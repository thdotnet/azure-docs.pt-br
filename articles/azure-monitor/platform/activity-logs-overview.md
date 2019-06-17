---
title: Visão geral do Log de Atividades do Azure
description: Saiba o que é o Log de Atividades do Azure e como usá-lo para compreender os eventos que ocorrem dentro de sua assinatura do Azure.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/19/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 6fc00bf0dfb83f349da91989a579f31be2027ff0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67071680"
---
# <a name="overview-of-azure-activity-log"></a>Visão geral do log de atividades do Azure

O **Log de atividades do Azure** fornece ideias sobre eventos de nível de assinatura que ocorreram no Azure. Isso inclui um intervalo de dados, de dados operacionais do Azure Resource Manager para atualizações em eventos de Integridade do Serviço. O Log de atividades era conhecido como _Logs de auditoria_ ou _Logs operacionais_, pois a categoria administrativa relata eventos de plano de controle de suas assinaturas. 

Use o Log de atividades, para determinar a _o que_, _quem_, e _quando_ para quaisquer operações (PUT, POST, DELETE) executadas nos recursos em sua assinatura de gravação. Também é possível compreender o status da operação e outras propriedades relevantes. 

O Log de atividades não inclui operações de leitura (GET) ou operações para recursos que usam o modelo clássico/RDFE.

## <a name="comparison-to-diagnostic-logs"></a>Comparação com os Logs de diagnóstico
Há um único Log de atividade para cada assinatura do Azure. Ele fornece dados sobre as operações em um recurso externo (o "plano de controle"). [Os Logs de diagnóstico](diagnostic-logs-overview.md) são emitidos por um recurso e fornecem informações sobre a operação do recurso (o "plano de dados"). Você deve habilitar as configurações de diagnóstico para cada recurso.

![Logs de atividade em comparação comparados os logs de diagnóstico](./media/activity-logs-overview/Activity_Log_vs_other_logs_v5.png)


> [!NOTE]
> O Log de Atividades do Azure é usado principalmente para atividades que ocorrem no Azure Resource Manager. Ele não controla os recursos usando o modelo Clássico/RDFE. Alguns tipos de recursos Clássicos têm um provedor de recursos de proxy no Azure Resource Manager (por exemplo, Microsoft.ClassicCompute). Se você interagir com um tipo de recurso Clássico por meio do Azure Resource Manager usando esses provedores de recursos de proxy, as operações aparecerão no Log de Atividades. Se você interagir com um tipo de recurso clássico fora dos proxies do Azure Resource Manager, suas ações somente serão registradas no Log de Operação. O Log de Operação pode ser pesquisado em uma seção separada do portal.

## <a name="activity-log-retention"></a>Retenção de Log de atividade
Depois de criar, entradas de Log de atividades não modificadas ou excluídas pelo sistema. Além disso, você não pode alterá-los na interface ou programaticamente. Eventos do Log de atividades são armazenados por 90 dias. Para armazenar esses dados por períodos mais longos, [coletá-los no Azure Monitor](activity-log-collect.md) ou [exportá-lo para o armazenamento ou Hubs de eventos](activity-log-export.md).

## <a name="view-the-activity-log"></a>Exibir o Log de atividades
Exibir o Log de atividades para todos os recursos do **Monitor** menu no portal do Azure. Exibir o Log de atividades para um recurso específico do **Log de atividades** opção no menu daquele recurso. Você também pode recuperar os registros de Log de atividades com o PowerShell, CLI ou API REST.  Ver [modo de exibição e como recuperar a atividade do Azure registrar eventos](activity-log-view.md).

![Exibir Log de atividades](./media/activity-logs-overview/view-activity-log.png)

## <a name="collect-activity-log-in-azure-monitor"></a>Coletar Log de atividades no Azure Monitor
Colete o Log de atividades em um espaço de trabalho do Log Analytics no Azure Monitor para analisá-lo com outros dados de monitoramento e reter os dados por mais de 90 dias. Ver [coletar e analisar logs de atividades do Azure no espaço de trabalho do Log Analytics no Azure Monitor](activity-log-collect.md).

![Log de atividades de consulta](./media/activity-logs-overview/query-activity-log.png)

## <a name="export-activity-log"></a>Exportar Log de atividades
Exportar o Log de atividades para o armazenamento do Azure para arquivamento ou transmiti-lo para um Hub de eventos para ingestão por um serviço de terceiros ou a solução de análise personalizada. Ver [exportar o Log de atividades do Azure](activity-log-export.md). Você também pode analisar eventos de Log de atividades no Power BI usando o [ **o pacote de conteúdo do Power BI**](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).

## <a name="alert-on-activity-log"></a>Alerta no Log de atividades
Você pode criar um alerta quando determinados eventos são criados no Log de atividades com um [alerta do Log de atividades](activity-log-alerts.md). Você também pode criar um alerta usando um [consulta de log](alerts-log-query.md) quando seu Log de atividades está conectado a um espaço de trabalho do Log Analytics, mas há um custo para fazer consultar alertas. Não há nenhum custo para alertas do Log de atividades.

## <a name="categories-in-the-activity-log"></a>Categorias no Log de Atividades
Cada evento no Log de atividades tem uma categoria específica que são descritos na tabela a seguir. Para obter todos os detalhes sobre o esquema dessas categorias, veja o [esquema de eventos de Log de Atividades do Azure](activity-log-schema.md). 

| Categoria | DESCRIÇÃO |
|:---|:---|
| Administrativo | Contém o registro de todos os de criação, operações de atualização, exclusão e ação executadas por meio do Gerenciador de recursos. Exemplos de eventos administrativos _criar máquina virtual_ e _excluir grupo de segurança de rede_.<br><br>Cada ação tomada por um usuário ou aplicativo usando o Resource Manager é modelada como uma operação em um tipo de recurso específico. Se for o tipo de operação _escrever_, _excluir_, ou _ação_, os registros de início e de sucesso ou falha da operação é registrada na categoria administrativa. Eventos administrativos também incluem todas as alterações ao controle de acesso baseado em função em uma assinatura. |
| Integridade do Serviço | Contém o registro de qualquer incidente de integridade do serviço ocorrido no Azure. Um exemplo de um evento de integridade do serviço _do SQL Azure no Leste dos EUA está passando por tempo de inatividade_. <br><br>Eventos de integridade do serviço são fornecidos em cinco variedades: _Ação necessária_, _recuperação assistida_, _incidente_, _manutenção_, _informações_, ou  _Segurança_. Esses eventos são criados somente se você tiver um recurso na assinatura que seria afetada pelo evento.
| Integridade de recursos | Contém o registro de quaisquer eventos de integridade do recurso que ocorreram para os recursos do Azure. É um exemplo de um evento de integridade do recurso _status de integridade da máquina Virtual alterado para indisponível_.<br><br>Eventos de integridade de recursos podem representar um dos quatro status de integridade: _Disponível_, _indisponível_, _degradado_, e _desconhecido_. Além disso, os eventos de integridade de recursos podem ser categorizados como sendo _iniciada na plataforma_ ou _iniciada pelo usuário_. |
| Alerta | Contém o registro de ativações de alertas do Azure. É um exemplo de um evento de alerta _% de CPU em myVM foi 80 nos últimos 5 minutos_.|
| Autoscale | Contém o registro de todos os eventos relacionados à operação do mecanismo de dimensionamento automático com base em quaisquer configurações de dimensionamento automático que você definiu na sua assinatura. É um exemplo de um evento de dimensionamento automático _Falha na ação de expansão do dimensionamento automático_. |
| Recomendações | Contém os eventos de recomendação do Assistente do Azure. |
| Segurança | Contém o registro de todos os alertas gerados pela Central de segurança do Azure. É um exemplo de um evento de segurança _arquivo de extensão dupla suspeito executado_. |
| Política | Contém registros de todas as operações de efeito ação realizadas pelo Azure Policy. Exemplos de eventos de política _auditoria_ e _Deny_. Cada ação tomada pelo Policy é modelada como uma operação em um recurso. |


## <a name="next-steps"></a>Próximas etapas

* [Criar um perfil de log para exportar o Log de atividades do Azure](activity-log-export.md)
* [Transmissão do Log de Atividades do Azure para os Hubs de Eventos](activity-logs-stream-event-hubs.md)
* [Arquivar o Log de atividades do Azure para armazenamento](archive-activity-log.md)

