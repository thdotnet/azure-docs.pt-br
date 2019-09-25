---
title: Visão geral dos logs de recursos do Azure | Microsoft Docs
description: Entenda os serviços com suporte e o esquema de eventos para logs de recursos do Azure.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: reference
ms.date: 09/20/2019
ms.author: robb
ms.subservice: logs
ms.openlocfilehash: bfcd2ded96c2679ba9177a760a8b11dc7d2c9a77
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71262538"
---
# <a name="azure-resource-logs-overview"></a>Visão geral dos logs de recursos do Azure
Os logs de recursos do Azure são [os logs de plataforma](platform-logs-overview.md) emitidos pelos recursos do Azure que descrevem sua operação interna. Todos os logs de recursos compartilham um esquema comum de nível superior com a flexibilidade de cada serviço para emitir propriedades exclusivas para seus próprios eventos.

> [!NOTE]
> Os logs de recursos eram anteriormente conhecidos como logs de diagnóstico.

## <a name="collecting-resource-logs"></a>Coletando logs de recursos
Os logs de recursos são gerados automaticamente por recursos do Azure com suporte, mas não são coletados a menos que você os Configure usando uma [configuração de diagnóstico](diagnostic-settings.md). Crie uma configuração de diagnóstico para cada recurso do Azure para encaminhar os logs para os seguintes destinos:

| Destination | Cenário |
|:---|:---|:---|
| [Espaço de Trabalho do Log Analytics](resource-logs-collect-storage.md) | Analise os logs com outros dados de monitoramento e aproveite Azure Monitor recursos como consultas de log e alertas de log. |
| [Armazenamento do Azure](archive-diagnostic-logs.md) | Arquive os logs para auditoria ou backup. |
| [Hub de eventos](resource-logs-stream-event-hubs.md) | Transmita os logs para sistemas de registro em log e telemetria de terceiros.  |

## <a name="compute-resources"></a>Recursos de computação
Os logs de recursos diferem dos logs no nível do sistema operacional convidado nos recursos de computação do Azure. Os recursos de computação exigem um agente para coletar logs e métricas de seu sistema operacional convidado, incluindo dados como logs de eventos, syslog e contadores de desempenho. Use a [extensão de diagnóstico](agents-overview.md#azure-diagnostic-extension) para rotear dados de log de máquinas virtuais do Azure e o [agente de log Analytics](agents-overview.md#log-analytics-agent) para coletar logs e métricas de máquinas virtuais no Azure, em outras nuvens e localmente em um espaço de trabalho log Analytics. Consulte [fontes de dados de monitoramento para obter Azure monitor](data-sources.md) para obter detalhes.

## <a name="resource-logs-schema"></a>Esquema de logs de recursos
Cada serviço do Azure tem seu próprio esquema quando os logs de recursos são gravados em um dos destinos, mas todos compartilham um esquema de nível superior na tabela a seguir. Consulte [esquemas específicos do serviço](#service-specific-schemas) abaixo para obter links para o esquema para cada serviço. 

| NOME | Obrigatório/Opcional | DESCRIÇÃO |
|---|---|---|
| time | Obrigatório | O carimbo de data/hora (UTC) do evento. |
| resourceId | Obrigatório | A ID do recurso que emitiu o evento. Para serviços de locatário, isso é o /tenants/tenant-id/providers/provider-name do formulário. |
| tenantId | Necessário para os logs de locatário | A ID de locatário do que esse evento está vinculado ao locatário do Active Directory. Essa propriedade só é usada para logs de nível de locatário, ele não aparece nos logs de nível do recurso. |
| operationName | Obrigatório | O nome da operação representada por esse evento. Se o evento representa uma operação RBAC, esse é o nome da operação RBAC (por exemplo, Microsoft.Storage/storageAccounts/blobServices/blobs/Read). Normalmente modeladas na forma de uma operação do Resource Manager, mesmo que não sejam as operações do Resource Manager documentadas reais (`Microsoft.<providerName>/<resourceType>/<subtype>/<Write/Read/Delete/Action>`) |
| operationVersion | Opcional | A api-version associada à operação, se a operationName foi executada usando uma API (por exemplo, `http://myservice.windowsazure.net/object?api-version=2016-06-01`). Se não houver nenhuma API que corresponde a essa operação, a versão representará a versão dessa operação, caso as propriedades associadas à operação sejam alteradas no futuro. |
| category | Obrigatório | A categoria de log do evento. Categoria é a granularidade na qual você pode habilitar ou desabilitar os logs em determinado recurso. As propriedades exibidas no blob de propriedades de um evento são as mesmas em uma categoria de log e um tipo de recurso específicos. As categorias de log típicas são “Auditoria”, “Operacional”, “Execução” e “Solicitação”. |
| resultType | Opcional | O status do evento. Os valores típicos incluem Iniciado, Em Andamento, Com Êxito, Com Falha, Ativo e Resolvido. |
| resultSignature | Opcional | O substatus do evento. Se essa operação corresponder a uma chamada à API REST, esse será o código de status HTTP da chamada REST correspondente. |
| resultDescription | Opcional | A descrição de texto estático dessa operação, por exemplo, “Obter arquivo de armazenamento”. |
| durationMs | Opcional | A duração da operação em milissegundos. |
| callerIpAddress | Opcional | O endereço IP do chamador, caso a operação corresponda a uma chamada à API proveniente de uma entidade com um endereço IP disponível publicamente. |
| correlationId | Opcional | Um GUID usado para agrupar um conjunto de eventos relacionados. Normalmente, se dois eventos têm o mesmo operationName, mas dois status diferentes (por exemplo, “Iniciado” e “Com Êxito”), eles compartilham a mesma ID de correlação. Isso também pode representar outras relações entre os eventos. |
| identity | Opcional | Um blob JSON que descreve a identidade do usuário ou do aplicativo que realizou a operação. Normalmente, isso inclui a autorização e as declarações/token JWT do Active Directory. |
| Nível | Opcional | O nível de severidade do evento. Precisa ser Informativo, Aviso, Erro ou Crítico. |
| location | Opcional | A região do recurso que emite o evento, por exemplo, “Leste dos EUA” ou “Sul da França” |
| properties | Opcional | As propriedades estendidas relacionadas a essa categoria específica de eventos. Todas as propriedades personalizadas/exclusivas precisam ser colocadas dentro desta “Parte B” do esquema. |

## <a name="service-specific-schemas"></a>Esquemas específicos do serviço
O esquema para os logs de diagnóstico de recurso varia de acordo com o tipo de recurso `resourceId` que é definido pela `category` Propriedade) e as propriedades. A lista a seguir mostra todos os serviços do Azure que dão suporte a logs de recursos com links para o serviço e esquema específico de categoria, quando disponível.

| Serviço | Esquema e Documentos |
| --- | --- |
| Active Directory do Azure | [Visão geral](../../active-directory/reports-monitoring/concept-activity-logs-azure-monitor.md), esquema de [log de auditoria](../../active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema.md) e [esquemas de entradas](../../active-directory/reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md) |
| Serviços de análise | https://azure.microsoft.com/blog/azure-analysis-services-integration-with-azure-diagnostic-logs/ |
| Gerenciamento de API | [Logs de Diagnóstico de Gerenciamento de API](../../api-management/api-management-howto-use-azure-monitor.md#diagnostic-logs) |
| Gateways do Aplicativo |[Log de diagnóstico do Gateway de Aplicativo](../../application-gateway/application-gateway-diagnostics.md) |
| Automação do Azure |[Análise de log para automação do Azure](../../automation/automation-manage-send-joblogs-log-analytics.md) |
| Lote do Azure |[Logs de diagnóstico do Lote do Azure](../../batch/batch-diagnostics.md) |
| Banco de Dados do Azure para MySQL | [Banco de dados do Azure para logs de Diagnóstico do MySQL](../../mysql/concepts-server-logs.md#diagnostic-logs) |
| Banco de Dados do Azure para PostgreSQL | [Banco de dados do Azure para logs de Diagnóstico do MySQL](../../postgresql/concepts-server-logs.md#diagnostic-logs) |
| Serviços Cognitivos | [Log de diagnóstico para serviços cognitivas do Azure](../../cognitive-services/diagnostic-logging.md) |
| Rede de Distribuição de Conteúdo | [Logs de diagnóstico do Azure para CDN](../../cdn/cdn-azure-diagnostic-logs.md) |
| CosmosDB | [Registro em log do Azure Cosmos DB](../../cosmos-db/logging.md) |
| Data Factory | [Monitorar data factories usando o Azure Monitor](../../data-factory/monitor-using-azure-monitor.md) |
| Data Lake Analytics |[Acessando os logs de diagnóstico do Azure Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake Store |[Acessando os logs de diagnóstico do Azure Data Lake Store](../../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Hubs de Eventos |[Logs de diagnóstico dos Hubs de Eventos do Azure](../../event-hubs/event-hubs-diagnostic-logs.md) |
| ExpressRoute | Esquema não disponível. |
| Firewall do Azure | Esquema não disponível. |
| Hub IoT | [Operações do Hub IoT](../../iot-hub/iot-hub-monitor-resource-health.md#use-azure-monitor) |
| Key Vault |[Logs do Cofre da Chave do Azure](../../key-vault/key-vault-logging.md) |
| Load Balancer |[Log Analytics para o Azure Load Balancer](../../load-balancer/load-balancer-monitor-log.md) |
| Aplicativos Lógicos |[Esquema de controle personalizado dos Aplicativos Lógicos B2B](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Grupos de segurança de rede |[Análise de logs para NSGs (grupos de segurança de rede)](../../virtual-network/virtual-network-nsg-manage-log.md) |
| Proteção contra DDOS | [Gerenciar Proteção contra DDoS do Azure Standard](../../virtual-network/manage-ddos-protection.md) |
| Power BI dedicado | [Log de diagnóstico para Power BI Embedded no Azure](https://docs.microsoft.com/power-bi/developer/azure-pbie-diag-logs) |
| Serviços de Recuperação | [Modelo de dados para os Backup do Azure](../../backup/backup-azure-reports-data-model.md)|
| Search |[Habilitação e uso da análise de tráfego de pesquisa](../../search/search-traffic-analytics.md) |
| Barramento de Serviço |[Logs de diagnóstico do Barramento de Serviço do Azure](../../service-bus-messaging/service-bus-diagnostic-logs.md) |
| Banco de Dados SQL | [Log de diagnósticos do Banco de Dados SQL do Azure](../../sql-database/sql-database-metrics-diag-logging.md) |
| Stream Analytics |[Logs de diagnóstico do trabalho](../../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Gerenciador de Tráfego | [Esquema de log do Gerenciador de Tráfego](../../traffic-manager/traffic-manager-diagnostic-logs.md) |
| Redes Virtuais | Esquema não disponível. |
| Gateways de Rede Virtual | Esquema não disponível. |

## <a name="next-steps"></a>Próximas etapas

* [Saiba mais sobre outros logs da plataforma Azure](platform-logs-overview.md) que você pode usar para monitorar o Azure.
