---
title: Log e auditoria do Azure | Microsoft Docs
description: Saiba mais sobre como você pode usar os dados de log para obter informações detalhadas sobre seu aplicativo.
services: security
documentationcenter: na
author: UnifyCloud
manager: barbkess
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/14/2019
ms.author: TomSh
ms.openlocfilehash: d64cdce34127b066aedc8a5fcd6ec3a891b38c5e
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71262843"
---
# <a name="azure-logging-and-auditing"></a>Log e auditoria do Azure

O Azure fornece uma ampla gama de opções de registro de log e auditoria de segurança configuráveis para ajudá-lo a identificar lacunas em seus mecanismos e políticas de segurança. Este artigo discute a geração, a coleta e a análise de logs de segurança de serviços hospedados no Azure.

> [!Note]
> Determinadas recomendações neste artigo podem resultar em maior uso de recursos de computação, rede ou dados e aumentar os custos de licença ou assinatura.

## <a name="types-of-logs-in-azure"></a>Tipos de logs no Azure

Os aplicativos em nuvem são complexos com muitas partes móveis. Os logs fornecem dados para ajudar a manter seus aplicativos em funcionamento. Os logs ajudam você a solucionar problemas anteriores ou evitar potenciais problemas. E eles podem ajudá-lo a melhorar o desempenho ou a capacidade de manutenção do aplicativo ou automatizar ações que, de outra forma, exigiriam intervenção manual.

Os logs do Azure são categorizados nos seguintes tipos:
* **Logs de controle/gerenciamento** fornecem informações sobre as operações CREATE, UPDATE e DELETE do Azure Resource Manager. Para obter mais informações, confira [Logs de atividades do Azure](../../azure-monitor/platform/activity-logs-overview.md).

* **Logs de plano de dados** fornecem informações sobre eventos gerados como parte de uso de recursos do Azure. Exemplos desse tipo de log são os logs do aplicativo, de segurança e do sistema de eventos do Windows em uma VM (máquina virtual) e os [logs de diagnóstico](../../azure-monitor/platform/resource-logs-overview.md) que são configurados por meio do Azure Monitor.

* Os **eventos processados** fornecem informações sobre os eventos/alertas analisados que foram processados em seu nome. Exemplos desse tipo são os [Alertas da Central de Segurança do Azure](../../security-center/security-center-managing-and-responding-alerts.md), nos quais a [Central de Segurança do Azure](../../security-center/security-center-intro.md) processou e analisou sua assinatura e fornece alertas de segurança concisos.

A tabela a seguir lista os tipos mais importantes de logs disponíveis no Azure:

| Categoria do log | Tipo de log | Uso | Integração |
| ------------ | -------- | ------ | ----------- |
|[Logs de atividade](../../azure-monitor/platform/activity-logs-overview.md)|Eventos de plano de controle nos recursos do Azure Resource Manager|  Fornecem informações sobre as operações executadas em recursos de sua assinatura.|    API REST, [Azure Monitor](../../azure-monitor/platform/activity-logs-overview.md)|
|[Logs de diagnóstico do Azure](../../azure-monitor/platform/resource-logs-overview.md)|Dados frequentes sobre a operação de recursos do Azure Resource Manager na assinatura|    Fornecem informações sobre as operações que o recurso executou por conta própria.| Azure Monitor, [Stream](../../azure-monitor/platform/resource-logs-overview.md)|
|[Relatórios do Azure AD](../../active-directory/reports-monitoring/overview-reports.md)|Logs e relatórios | Relata atividades de conexão do usuário e informações de atividades do sistema sobre gerenciamento de usuários e grupos.|[API do Graph](../../active-directory/develop/active-directory-graph-api-quickstart.md)|
|[Máquinas virtuais e serviços de nuvem](../../azure-monitor/learn/quick-collect-azurevm.md)|Serviço de log de eventos do Windows e syslog do Linux|  Capturam dados do sistema e dados de logs nas máquinas virtuais e transferem os dados para uma conta de armazenamento de sua escolha.|   Windows (usando o armazenamento [WAD](../../monitoring-and-diagnostics/azure-diagnostics.md) [Diagnóstico do Azure para Windows]) e Linux no Azure Monitor|
|[Análise do Armazenamento do Azure](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)|Log de armazenamento, fornece dados de métrica de uma conta de armazenamento|Fornece informações das solicitações de rastreamento, analisa tendências de uso e diagnostica problemas com a conta de armazenamento.|   API REST ou [biblioteca de cliente](https://msdn.microsoft.com/library/azure/mt347887.aspx)|
|[Logs de fluxo do NSG (Grupo de Segurança de Rede)](../../network-watcher/network-watcher-nsg-flow-logging-overview.md)|Formato JSON, mostra os fluxos de entrada e saída por regra|Exibe informações sobre o tráfego IP de entrada e saída por meio de um Grupo de Segurança de Rede.|[Observador de Rede do Azure](../../network-watcher/network-watcher-monitoring-overview.md)|
|[Application Insights](../../azure-monitor/app/app-insights-overview.md)|Logs, exceções e diagnóstico personalizado|  Fornece um serviço APM (monitoramento de desempenho de aplicativos) para desenvolvedores da Web em várias plataformas.| API REST, [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)|
|Dados do processo/alertas de segurança|    Alertas da central de segurança do Azure, alertas de logs de Azure Monitor|    Fornece informações e alertas de segurança.|  APIs REST, JSON|

### <a name="activity-logs"></a>Logs de atividade

Os [logs de atividades do Azure](../../azure-monitor/platform/activity-logs-overview.md) fornecem informações sobre as operações executadas em recursos em sua assinatura. Os logs de atividades eram conhecidos como “logs de auditoria” ou “logs operacionais”, pois eles relatam [eventos de plano de controle](https://driftboatdave.com/2016/10/13/azure-auditing-options-for-your-custom-reporting-needs/) de suas assinaturas. 

Os logs de atividades ajudam você a determinar o "o quê, quem e quando" para operações de gravação (ou seja, PUT, POST ou DELETE). Os logs de atividades também ajudam a entender o status da operação e outras propriedades relevantes. Os logs de atividade não incluem operações de leitura (GET).

Neste artigo, PUT, POST e DELETE referem-se a todas as operações de gravação que o log de atividades contém nos recursos. Por exemplo, é possível usar os logs de atividades para encontrar um erro ao solucionar problemas ou para monitorar como um usuário de sua organização modificou um recurso.

![Diagrama do log de atividades](./media/log-audit/azure-log-audit-fig1.png)

Você pode recuperar os eventos do log de atividades usando o portal do Azure, a [CLI do Azure](../../storage/common/storage-azure-cli.md), os cmdlets do PowerShell e a [API REST do Azure Monitor](../../azure-monitor/platform/rest-api-walkthrough.md). Os logs de atividades têm um período de retenção de dados de 90 dias.

Cenários de integração para um evento de log de atividades:

* [Criar um alerta de email ou webhook disparado por um evento de log de atividades](../../monitoring-and-diagnostics/monitor-alerts-unified-log-webhook.md).

* [Transmiti-lo para um hub de eventos](../../azure-monitor/platform/activity-logs-stream-event-hubs.md) para ingestão por um serviço de terceiros ou uma solução de análise personalizada, como o Power BI.

* Analisá-lo no PowerBI usando o [Pacote de conteúdo do PowerBI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).

* [Salvá-lo em uma conta de armazenamento para inspeção manual ou arquivamento](../../azure-monitor/platform/archive-activity-log.md). Você pode especificar o tempo de retenção (em dias) usando os perfis de log.

* Consultar e exibi-lo no portal do Azure.

* Consultar por meio de cmdlet do PowerShell, da CLI do Azure ou da API REST.

* Exporte o log de atividades com perfis de log para [Azure monitor logs](../../log-analytics/log-analytics-queries.md).

É possível usar uma conta de armazenamento ou um [namespace de hub de eventos](../../event-hubs/event-hubs-resource-manager-namespace-event-hub-enable-capture.md) que não está na mesma assinatura que está emitindo o log. Qualquer que seja a pessoa que define a configuração deve ter o devido acesso [RBAC (controle de acesso baseado em função)](../../role-based-access-control/role-assignments-portal.md) para ambas as assinaturas.

### <a name="azure-diagnostics-logs"></a>Logs de diagnóstico do Azure

Os logs de diagnóstico do Azure são emitidos por um recurso que fornece dados avançados e frequentes sobre a operação do recurso. O conteúdo desses logs varia de acordo com o tipo de recurso. Por exemplo, os [logs do sistema de eventos do Windows](../../azure-monitor/platform/data-sources-windows-events.md) são uma categoria de logs de diagnóstico para VMs e [logs de blobs, de tabelas e de filas são](../../storage/common/storage-monitor-storage-account.md) categorias de logs de diagnóstico para contas de armazenamento. Os logs de diagnóstico são diferentes dos logs de atividades, que fornecem insights sobre as operações que foram executadas em recursos em sua assinatura.

![Diagramas de logs de diagnóstico do Azure](./media/log-audit/azure-log-audit-fig2.png)

Os logs de diagnóstico do Azure oferecem várias opções de configuração, como o portal do Azure, o PowerShell, a CLI do Azure e a API REST.

**Cenários de integração**

* Salve-os em uma [conta de armazenamento](../../azure-monitor/platform/archive-diagnostic-logs.md) para auditoria ou inspeção manual. Você pode especificar o tempo (em dias) de retenção usando as configurações de diagnóstico.

* [Transmita-os para os Hubs de Eventos](../../azure-monitor/platform/resource-logs-stream-event-hubs.md) para ingestão por um serviço de terceiros ou uma solução de análises personalizadas, como o [PowerBI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/).

* Analise-os com [logs de Azure monitor](../../log-analytics/log-analytics-queries.md).

**Serviços compatíveis, esquema dos logs de diagnóstico e categorias de logs compatíveis por tipo de recurso**


| Serviço | Esquema e documentação | Tipo de recurso | Categoria |
| ------- | ------------- | ------------- | -------- |
|Azure Load Balancer| [Logs de Azure Monitor para Load Balancer (versão prévia)](../../load-balancer/load-balancer-monitor-log.md)|Microsoft.Network/loadBalancers<br>Microsoft.Network/loadBalancers|    LoadBalancerAlertEvent<br>LoadBalancerProbeHealthStatus|
|Grupos de segurança de rede|[Logs de Azure Monitor para grupos de segurança de rede](../../virtual-network/virtual-network-nsg-manage-log.md)|Microsoft.Network/networksecuritygroups<br>Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent<br>NetworkSecurityGroupRuleCounter|
|Gateway de Aplicativo do Azure|[Log de diagnóstico do Gateway de Aplicativo](../../application-gateway/application-gateway-diagnostics.md)|Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways<br>Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog<br>ApplicationGatewayPerformanceLog<br>ApplicationGatewayFirewallLog|
|Azure Key Vault|[Logs do Key Vault](../../key-vault/key-vault-logging.md)|Microsoft.KeyVault/vaults|AuditEvent|
|Azure Search|[Habilitação e uso da análise de tráfego de pesquisa](../../search/search-traffic-analytics.md)|Microsoft.Search/searchServices|OperationLogs|
|Repositório Azure Data Lake|[Acessar os logs de diagnóstico do Data Lake Store](../../data-lake-store/data-lake-store-diagnostic-logs.md)|Microsoft.DataLakeStore/accounts<br>Microsoft.DataLakeStore/accounts|Audit<br>Requests|
|Análise Azure Data Lake|[Acessar os logs de diagnóstico do Data Lake Analytics](../../data-lake-analytics/data-lake-analytics-diagnostic-logs.md)|Microsoft.DataLakeAnalytics/accounts<br>Microsoft.DataLakeAnalytics/accounts|Audit<br>Requests|
|Aplicativos Lógicos do Azure|[Esquema de controle personalizado dos Aplicativos Lógicos B2B](../../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)|Microsoft.Logic/workflows<br>Microsoft.Logic/integrationAccounts|WorkflowRuntime<br>IntegrationAccountTrackingEvents|
|Lote do Azure|[Logs de diagnóstico do Lote do Azure](../../batch/batch-diagnostics.md)|Microsoft.Batch/batchAccounts|ServiceLog|
|Automação do Azure|[Logs de Azure Monitor para a automação do Azure](../../automation/automation-manage-send-joblogs-log-analytics.md)|Microsoft.Automation/automationAccounts<br>Microsoft.Automation/automationAccounts|JobLogs<br>JobStreams|
|Hubs de eventos do Azure|[Logs de diagnóstico dos Hubs de Eventos](../../event-hubs/event-hubs-diagnostic-logs.md)|Microsoft.EventHub/namespaces<br>Microsoft.EventHub/namespaces|ArchiveLogs<br>OperationalLogs|
|Stream Analytics do Azure|[Logs de diagnóstico de trabalho](../../stream-analytics/stream-analytics-job-diagnostic-logs.md)|Microsoft.StreamAnalytics/streamingjobs<br>Microsoft.StreamAnalytics/streamingjobs|Execução<br>Criação|
|Barramento de Serviço do Azure|[Logs de diagnóstico dos Barramento de Serviço](../../service-bus-messaging/service-bus-diagnostic-logs.md)|Microsoft.ServiceBus/namespaces|OperationalLogs|

### <a name="azure-active-directory-reporting"></a>Relatórios do Azure Active Directory

O Azure AD (Azure Active Directory) inclui relatórios de segurança, de atividades e de auditoria para o diretório de um usuário. O [relatório de auditoria do Azure AD](../../active-directory/active-directory-reporting-azure-portal.md) ajuda a identificar as ações privilegiadas que ocorreram na instância do Azure AD do usuário. Ações com privilégios incluem alterações de elevação (por exemplo, criação de funções ou redefinições de senha), alteração de configurações de política (por exemplo, políticas de senha) ou alterações na configuração de diretório (por exemplo, as alterações às configurações de federação de domínio).

Os relatórios fornecem o registro de auditoria para o nome do evento, o usuário que executou a ação, o recurso de destino afetado pela alteração e a data e hora (em UTC). Os usuários podem recuperar a lista de eventos de auditoria do Azure AD por meio do [portal do Azure](https://portal.azure.com/), conforme descrito em [Exibir os logs de auditoria](../../active-directory/reports-monitoring/overview-reports.md). 

Os relatórios incluídos estão listados na tabela a seguir:

| Relatórios de segurança | Relatórios de atividades | Relatórios de auditoria |
| :--------------- | :--------------- | :------------ |
|Entradas de fontes desconhecidas| Uso do aplicativo: resumo| Relatório de auditoria de diretório|
|Entradas após várias falhas|  Uso do aplicativo: detalhado||
|Entradas de várias geografias|    Painel do aplicativo||
|Entradas de endereços IP com atividade suspeita|   Erros de provisionamento de conta||
|Atividades de entrada irregulares|    Dispositivos de usuário individual||
|Entradas de dispositivos possivelmente infectados|   Atividade de usuário individual||
|Usuários com atividade de entrada anômala| Relatório de atividade de grupos||
||Relatório de atividade de registro de redefinição de senha||
||Atividade de redefinição de senha||

Os dados desses relatórios podem ser úteis para os aplicativos, como sistemas SIEM (Gerenciamento de Eventos e Informações de Segurança), auditoria e ferramentas de business intelligence. As APIs de relatório do Azure AD fornecem acesso programático aos dados através de um conjunto de APIs baseadas em REST. Você pode chamar essas [APIs](../../active-directory/active-directory-reporting-api-getting-started-azure-portal.md) em várias ferramentas e linguagens de programação.

A retenção de eventos no relatório de auditoria do Azure AD varia entre 7-90 dias, dependendo do tipo de licença associado ao locatário. 

> [!Note]
> Para saber mais sobre a retenção de relatórios, confira [Políticas de retenção de relatório do Azure AD](../../active-directory/reports-monitoring/reference-reports-data-retention.md).

Se você estiver interessado em manter os eventos de auditoria por mais tempo, use a API de Relatórios para efetuar o pull dos [eventos de auditoria](../../active-directory/active-directory-reporting-activity-audit-logs.md) regularmente para um armazenamento de dados separado.

### <a name="virtual-machine-logs-that-use-azure-diagnostics"></a>Logs de máquina virtual que usam o Diagnóstico do Azure

O [Diagnóstico do Azure](../../monitoring-and-diagnostics/azure-diagnostics.md) é a funcionalidade do Azure que habilita a coleta de dados de diagnóstico em um aplicativo implantado. Você pode usar a extensão de diagnóstico de qualquer uma das várias fontes. As compatíveis no momento são as [funções de trabalho e Web do serviço de nuvem do Azure](../../cloud-services/cloud-services-choose-me.md).

![Logs de máquina virtual que usam o Diagnóstico do Azure](./media/log-audit/azure-log-audit-fig3.png)

### <a name="azure-virtual-machineslearnpathsdeploy-a-website-with-azure-virtual-machines-that-are-running-microsoft-windows-and-service-fabricservice-fabricservice-fabric-overviewmd"></a>[Máquinas virtuais do Azure](/learn/paths/deploy-a-website-with-azure-virtual-machines/) que executam o Microsoft Windows e o [Service Fabric](../../service-fabric/service-fabric-overview.md)

É possível habilitar o Diagnóstico do Azure em uma máquina virtual realizando uma da seguintes ações:

* [Usar o Visual Studio para rastrear máquinas virtuais do Azure](/visualstudio/azure/vs-azure-tools-debug-cloud-services-virtual-machines)

* [Configurar o Diagnóstico do Azure remotamente em uma máquina virtual do Azure](../../virtual-machines/virtual-machines-dotnet-diagnostics.md)

* [Usar o PowerShell para configurar o diagnóstico em máquinas virtuais do Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

* [Criar uma máquina virtual do Windows com monitoramento e diagnóstico usando um modelo do Azure Resource Manager](../../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="storage-analytics"></a>Análise de Armazenamento

A [Análise de Armazenamento do Azure](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) registra em log e fornece dados de métrica para uma conta de armazenamento. Você pode usar esses dados para rastrear solicitações, analisar tendências de uso e diagnosticar problemas com sua conta de armazenamento. O log da Análise de Armazenamento está disponível para os [serviços de Armazenamento de Blobs, de Filas e de Tabelas do Azure](../../storage/common/storage-introduction.md). A análise de armazenamento registra informações detalhadas sobre solicitações bem-sucedidas e com falha para um serviço de armazenamento.

Você pode usar essas informações para monitorar solicitações individuais e diagnosticar problemas com um serviço de armazenamento. As solicitações são registradas em uma base de melhor esforço. As entradas de log são criadas somente se há solicitações feitas no ponto de extremidade de serviço. Por exemplo, se uma conta de armazenamento tiver atividades em seu ponto de extremidade de blob, mas não em seus pontos de extremidade de tabela ou fila, somente os logs pertencentes ao serviço de Armazenamento de Blobs serão criados.

Para usar a Análise de Armazenamento, habilite-a separadamente para cada serviço que deseja monitorar. Você pode habilitá-la no [portal do Azure](https://portal.azure.com/). Para obter mais informações, confira [Monitorar uma conta de armazenamento no portal do Azure](../../storage/common/storage-monitor-storage-account.md). Você também pode habilitar a análise de armazenamento programaticamente por meio da API REST ou da biblioteca de cliente. Use a operação Definir Propriedades do Serviço para habilitar o Storage Analytics individualmente em cada serviço.

Os dados agregados são armazenados em um blob conhecido (para registro em log) e em tabelas conhecidas (para métricas), que podem ser acessados usando as APIs de serviço de Armazenamento de Tabelas e de Blobs.

A Análise de Armazenamento tem um limite de 20 TB (terabyte) na quantidade de dados armazenados, que não depende do limite total da conta de armazenamento. Todos os logs são armazenados em [blobs de bloco](../../storage/common/storage-analytics.md) em um contêiner denominado $logs, que é criado automaticamente quando você habilita a Análise de Armazenamento para uma conta de armazenamento.

> [!Note]
> * Para obter mais informações sobre cobrança e políticas de retenção de dados, confira [Análise de Armazenamento e cobrança](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
> * Para obter mais informações sobre limites de contas de armazenamento, confira [Escalabilidade e metas de desempenho do Armazenamento do Azure](../../storage/common/storage-scalability-targets.md).

A Análise de Armazenamento registra os seguintes tipos de solicitações anônimas e autenticadas:

| Autenticada  | Anônima|
| :------------- | :-------------|
| Solicitações bem-sucedidas | Solicitações bem-sucedidas |
|Solicitações com falha, incluindo tempo limite, limitação, rede, autorização e outros erros | Solicitações que usam uma assinatura de acesso compartilhado, incluindo solicitações com êxito e com falha |
| Solicitações que usam uma assinatura de acesso compartilhado, incluindo solicitações com êxito e com falha |Erros de tempo limite para o cliente e para o servidor |
|   Solicitações de dados de análise |    Solicitações GET com falha com o código de erro 304 (não modificado) |
| As solicitações feitas pela própria análise de armazenamento, como criação de log ou exclusão, não estão conectadas. Uma lista completa dos dados registrados está documentada em [Mensagens de operações e status registradas da Análise de Armazenamento](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) e [Formato do log da Análise de Armazenamento](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). | Todas as outras solicitações anônimas com falha não estão conectadas. Uma lista completa dos dados registrados está documentada em [Mensagens de operações e status registradas da Análise de Armazenamento](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) e [Formato do log da Análise de Armazenamento](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format). |

### <a name="azure-networking-logs"></a>Logs de rede do Azure

O log e monitoramento de rede no Azure é completo e abrange duas categorias amplas:

* [Observador de Rede](../../network-watcher/network-watcher-monitoring-overview.md): Monitoramento de rede baseado em cenário fornecido com os recursos do Observador de Rede. Esse serviço inclui a captura de pacotes, próximo salto, verificação do fluxo de IP, exibição do grupo de segurança e logs de fluxo de NSG. O monitoramento no nível do cenário fornece uma exibição completa dos recursos de rede em contraste com o monitoramento de recursos de rede individual.

* [Monitoramento de recursos](../../network-watcher/network-watcher-monitoring-overview.md): O monitoramento no nível do recurso é composto de quatro recursos: logs de diagnóstico, métricas, solução de problemas e integridade dos recursos. Todos esses recursos são compilados no nível do recurso da rede.

![Logs de rede do Azure](./media/log-audit/azure-log-audit-fig4.png)

O Observador de Rede é um serviço regional que permite monitorar e diagnosticar as condições em um nível do cenário da rede em, para e a partir do Azure. As ferramentas de diagnóstico e visualização da rede disponíveis com o Observador de Rede ajudam a entender, diagnosticar e ter informações para sua rede no Azure.

### <a name="network-security-group-flow-logging"></a>Registro de fluxo de Grupo de Segurança de Rede

Os [logs de fluxo de NSG](../../network-watcher/network-watcher-nsg-flow-logging-overview.md) são um recurso do Observador de Rede que você pode usar para exibir informações sobre o tráfego IP de entrada e saída por meio de um NSG. Esses logs de fluxo são gravados no formato JSON e mostram:
* Fluxos de entrada e saída por regra.
* A NIC à qual o fluxo se aplica.
* Informações de cinco tuplas sobre o fluxo: o IP de origem ou de destino, a porta de origem ou de destino e o protocolo.
* Indica se o tráfego foi permitido ou negado.

Embora os logs de fluxo sejam destinados aos NSGs, eles não são exibidos da mesma forma como os outros logs. Os logs de fluxo são armazenados apenas em uma conta de armazenamento.

As mesmas políticas de retenção que são vistas em outros logs aplicam-se aos logs de fluxo. Os logs têm uma política de retenção que você pode definir de 1 dia até 365 dias. Se uma política de retenção não for definida, os logs serão mantidos para sempre.

**Logs de diagnóstico**

Eventos periódicos e ESPONTANEOS são criados por recursos de rede e registrados em contas de armazenamento e enviados para um hub de eventos ou logs de Azure Monitor. Os logs fornecem informações sobre a integridade de um recurso. Eles podem ser exibidos em ferramentas como Power BI e Azure Monitor logs. Para saber como exibir os logs de diagnóstico, consulte [Azure monitor logs](../../azure-monitor/insights/azure-networking-analytics.md).

![Logs de diagnóstico](./media/log-audit/azure-log-audit-fig5.png)

Os logs de diagnóstico estão disponíveis para o [Load Balancer](../../load-balancer/load-balancer-monitor-log.md), os [Grupos de Segurança da Rede](../../virtual-network/virtual-network-nsg-manage-log.md), as Rotas e o [Gateway de Aplicativo](../../application-gateway/application-gateway-diagnostics.md).

O Observador de Rede fornece uma exibição dos logs de diagnóstico. Essa exibição contém todos os recursos de rede que oferecem suporte ao log de diagnóstico. Nessa exibição, você pode habilitar e desabilitar os recursos de rede de modo rápido e prático.


Além das funcionalidades de log mencionadas anteriormente, o Observador de Rede conta com as seguintes funcionalidades no momento:
- [Topologia](../../network-watcher/view-network-topology.md): Fornece uma exibição no nível da rede que mostra as diversas interconexões e associações entre os recursos de rede em um grupo de recursos.

- [Captura de Pacote Variável](../../network-watcher/network-watcher-packet-capture-overview.md): Captura os dados do pacote dentro e fora de uma máquina virtual. As opções avançadas de filtragem e os controles de ajuste fino, como as configurações de limitação de tempo e de tamanho, fornecem versatilidade. Os dados do pacote podem ser armazenados em um armazenamento de blobs ou no disco local no formato de arquivo *.cap*.

- [Verificação de fluxo de IP](../../network-watcher/network-watcher-ip-flow-verify-overview.md): Verifica se um pacote é permitido ou negado com base nos parâmetros do pacote com cinco tuplas das informações do fluxo (isto é, IP de destino, IP de origem, porta de destino, porta de origem e protocolo). Se o pacote for negado por um grupo de segurança, a regra e o grupo que negaram o pacote serão retornados.

- [Próximo salto](../../network-watcher/network-watcher-next-hop-overview.md): Determina o próximo salto para os pacotes encaminhados na malha de rede do Azure, de modo que você possa diagnosticar as rotas definidas pelo usuário configuradas incorretamente.

- [Vista de grupo de segurança](../../network-watcher/network-watcher-security-group-view-overview.md): Obtém as regras de segurança efetivas e aplicadas que são usadas em uma VM.

- [Solução de problemas de conexão e do gateway de rede virtual](../../network-watcher/network-watcher-troubleshoot-manage-rest.md): Ajuda você a solucionar problemas de conexões e gateways de rede virtual.

- [Limites de assinatura da rede](../../network-watcher/network-watcher-monitoring-overview.md): Permite exibir o uso dos recursos de rede em relação aos limites.

### <a name="application-insights"></a>Application Insights

O [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md) é um serviço de APM para desenvolvedores da Web em várias plataformas. Use-o para monitorar aplicativos Web ativos. Ele detecta anomalias de desempenho automaticamente. Ele inclui ferramentas de análise avançadas para ajudar você a diagnosticar problemas e entender o que os usuários realmente fazem com seu aplicativo.

O Application Insights foi projetado para ajudar você a aprimorar continuamente o desempenho e a usabilidade do seu aplicativo.

Ele funciona com aplicativos em uma ampla variedade de plataformas incluindo .NET, Node.js e Java EE, estejam eles hospedados localmente ou na nuvem. Ele é integrado ao seu processo de DevOps e tem pontos de conexão com várias ferramentas de desenvolvimento.

![Diagrama do Application Insights](./media/log-audit/azure-log-audit-fig6.png)

O Application Insights é indicado para a equipe de desenvolvimento, para ajudá-lo a compreender como está o desempenho de seu aplicativo e como ele está sendo usado. Ele monitora:

* **Solicitar taxas, tempos de resposta e taxas de falha**: Descubra quais páginas são mais populares, em que momentos do dia e onde os usuários estão. Confira as páginas que têm melhor desempenho. Se as taxas de falha e os tempos de resposta ficam altos quando há mais solicitações, você pode ter um problema de alocação de recursos.

* **Taxas de dependência, tempos de resposta e taxas de falha**: Descubra se os serviços externos estão causando lentidão.

* **Exceções**: Analise as estatísticas agregadas ou escolha instâncias específicas e faça uma busca detalhada no rastreamento de pilha e nas solicitações relacionadas. A maioria das exceções de navegador e servidor são relatadas.

* **Exibição de página e desempenho do navegador**: Obtenha relatórios de navegadores dos usuários.

* **Chamadas AJAX**: Obtêm as taxas de falha, os tempos de resposta e as taxas de páginas da Web.

* **Contagens de seção e usuários**.

* **Contadores de desempenho**: Obtêm dados de seus computadores de servidor Linux ou Windows, como CPU, memória e uso da rede.

* **Diagnóstico de host**: Obtêm dados do Docker ou do Azure.

* **Logs de rastreamento de diagnóstico**: Obtêm dados do seu aplicativo para que você possa correlacionar eventos de rastreamento com solicitações.

* **Métricas e eventos personalizados**: Obtêm dados que você escreve em código de cliente ou servidor, para acompanhar os eventos de negócios, como itens vendidos ou vitórias.

A tabela a seguir lista e descreve cenários de integração:

| Cenário de integração | DESCRIÇÃO |
| --------------------- | :---------- |
|[Mapa do aplicativo](../../azure-monitor/app/app-map.md)|Os componentes de seu aplicativo, com as principais métricas e alertas.|
|[Pesquisa de diagnóstico para dados da instância](../../azure-monitor/app/diagnostic-search.md)| pesquise e filtre eventos como solicitações, exceções, chamadas de dependência, rastreamentos de log e exibições de página.|
|[Metrics Explorer para dados agregados](../../azure-monitor/app/metrics-explorer.md)|explore, filtre e segmente dados agregados, como taxas de solicitações, falhas e exceções; tempos de resposta e tempos de carregamento de página.|
|[Painéis](../../azure-monitor/app/overview-dashboard.md)|faça um mashup de dados de vários recursos e compartilhe com outras pessoas. Excelente para aplicativos com vários componentes e para exibição contínua no ambiente de equipe.|
|[Live Metrics Stream](../../azure-monitor/app/live-stream.md)|quando implantar um novo build, acompanhe esses indicadores de desempenho quase em tempo real para verificar se tudo está funcionando conforme esperado.|
|[Analytics](../../azure-monitor/app/analytics.md)|responda perguntas difíceis sobre o desempenho e o uso do seu aplicativo usando essa poderosa linguagem de consulta.|
|[Alertas automáticos e manuais](../../azure-monitor/app/alerts.md)|Alertas automáticos se adaptam aos padrões normais de telemetria do seu aplicativo e são disparados quando há algo fora do padrão normal. Você também pode definir alertas em níveis específicos de métricas padrão ou personalizadas.|
|[Visual Studio](../../azure-monitor/app/visual-studio.md)|Exiba dados de desempenho no código. Vá até o código dos rastreamentos de pilha.|
|[Power BI](../../azure-monitor/app/export-power-bi.md)|Integre as métricas de uso com outro business intelligence.|
|[API REST](https://dev.applicationinsights.io/)|Escreva o código para executar consultas em suas métricas e dados brutos.|
|[Exportação contínua](../../azure-monitor/app/export-telemetry.md)|Exportação em massa de dados brutos para armazenamento no momento de sua chegada.|

### <a name="azure-security-center-alerts"></a>Alertas da Central de Segurança do Azure

A detecção de ameaças da Central de Segurança do Azure funciona coletando informações de segurança de seus recursos do Azure, de rede e de soluções de parceiros conectados automaticamente. Ele analisa essas informações geralmente correlacionando informações de várias fontes para identificar ameaças. Os alertas de segurança são priorizados na Central de Segurança, juntamente com recomendações sobre como corrigir a ameaça. Para obter mais informações, confira [Central de Segurança do Azure](../../security-center/security-center-intro.md).

![Diagrama da Central de Segurança do Azure](./media/log-audit/azure-log-audit-fig7.png)

A Central de Segurança emprega análise de segurança avançada, que vai além das abordagens baseadas em assinatura. Ela aplica avanços em dados grandes e tecnologias de [aprendizado de máquina](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/) para avaliar eventos em toda a malha de nuvem. Dessa forma, ela detecta ameaças que seriam impossíveis de identificar usando abordagens manuais e prevendo a evolução dos ataques. Essas análises de segurança incluem:

* **Inteligência integrada contra ameaças**: Procura atores mal-intencionados conhecidos aplicando a inteligência global em ameaças de produtos e serviços da Microsoft, da DCU (Unidade de Crimes Digitais) da Microsoft, do MSRC (Microsoft Security Response Center) e de feeds externos.

* **Análise comportamental**: Aplica padrões conhecidos para descobrir os comportamentos mal-intencionados.

* **Detecção de anomalias**: Usa estatísticas de criação de perfil para criar uma linha de base histórica. Ela o alertará sobre desvios das linhas de base estabelecidas em conformidade com um vetor de possível ataque.

Muitas operações de segurança e equipes de resposta a incidentes contam com uma solução SIEM como ponto de partida para separação e investigação de alertas de segurança. Com a integração de log do Azure, você pode sincronizar alertas da central de segurança e eventos de segurança de máquina virtual, coletados pelo diagnóstico do Azure e logs de auditoria, com seus logs de Azure Monitor ou solução SIEM quase em tempo real.

## <a name="azure-monitor-logs"></a>Logs do Azure Monitor

Os logs de Azure Monitor são um serviço no Azure que ajuda a coletar e analisar dados gerados pelos recursos em seus ambientes de nuvem e locais. Ele fornece informações em tempo real usando a pesquisa integrada e painéis personalizados para analisar prontamente milhões de registros em todas as suas cargas de trabalho e todos os seus servidores, independentemente de sua localização física.

![Diagrama de logs de Azure Monitor](./media/log-audit/azure-log-audit-fig8.png)

No centro de Azure Monitor logs está o espaço de trabalho Log Analytics, que é hospedado no Azure. Os logs de Azure Monitor coletam dados no espaço de trabalho de fontes conectadas Configurando fontes de dados e adicionando soluções à sua assinatura. As fontes de dados e as soluções criam tipos de registro diferentes, cada um com seu próprio conjunto de propriedades. Mas as fontes e as soluções ainda podem ser analisadas juntas em consultas para o workspace. Esse funcionalidade permite que você use as mesmas ferramentas e métodos para trabalhar com uma variedade de dados coletados por diversas fontes.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

As fontes conectadas são os computadores e outros recursos que geram os dados coletados pelos logs de Azure Monitor. As fontes podem incluir os agentes instalados em computadores [Windows](../../log-analytics/log-analytics-agent-windows.md) e [Linux](../../log-analytics/log-analytics-quick-collect-linux-computer.md) que se conectam diretamente ou agentes em um [grupo de gerenciamento do System Center Operations Manager conectado](../../azure-monitor/platform/om-agents.md). Os logs de Azure Monitor também podem coletar dados de uma [conta de armazenamento do Azure](../../azure-monitor/platform/resource-logs-collect-storage.md).

[Fontes de dados](../../azure-monitor/platform/agent-data-sources.md) são os vários tipos de dados coletados de cada fonte conectada. Essas fontes incluem eventos e [dados de desempenho](../../azure-monitor/platform/data-sources-performance-counters.md) de agentes [Windows](../../azure-monitor/platform/data-sources-windows-events.md) e Linux, além de fontes como [logs do IIS](../../azure-monitor/platform/data-sources-iis-logs.md) e [logs de texto personalizado](../../azure-monitor/platform/data-sources-custom-logs.md). Você configura cada fonte de dados que deseja coletar e a configuração é fornecida automaticamente para cada fonte conectada.

Há quatro maneiras de [coletar logs e métricas para os serviços do Azure](../../azure-monitor/platform/resource-logs-collect-storage.md):

* Diagnóstico do Azure direto para logs de Azure Monitor (**diagnóstico** na tabela a seguir)

* Diagnóstico do Azure o armazenamento do Azure para Azure Monitor logs (**armazenamento** na tabela a seguir)

* Conectores para serviços do Azure (**Conector** na tabela a seguir)

* Scripts para coletar e postar dados em logs de Azure Monitor (células em branco na tabela a seguir e para serviços que não estão listados)

| Serviço | Tipo de recurso | Logs | metrics | Solução |
| :------ | :------------ | :--- | :------ | :------- |
|Gateway de Aplicativo do Azure| Microsoft.Network/<br>applicationGateways|  Diagnóstico|Diagnóstico|    [Análise de Gateway de](../../azure-monitor/insights/azure-networking-analytics.md) [Aplicativo do Azure](../../azure-monitor/insights/azure-networking-analytics.md#azure-application-gateway-analytics-solution-in-azure-monitor)|
|Application Insights||     Conector|  Conector|  [Conector do Application](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/) [Insights (Visualização)](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)|
|Contas de Automação do Azure| Microsoft.Automation/<br>AutomationAccounts|    Diagnóstico||       [Mais informações](../../automation/automation-manage-send-joblogs-log-analytics.md)|
|Contas do Lote do Azure|  Microsoft.Batch/<br>batchAccounts|  Diagnóstico|    Diagnóstico||
|Serviços de Nuvem clássicos||       Armazenamento||       [Mais informações](../../azure-monitor/platform/azure-storage-iis-table.md)|
|Serviços Cognitivos|    Microsoft.CognitiveServices/<br>accounts|       Diagnóstico|||
|Análise Azure Data Lake| Microsoft.DataLakeAnalytics/<br>accounts|   Diagnóstico|||
|Repositório Azure Data Lake| Microsoft.DataLakeStore/<br>accounts|   Diagnóstico|||
|Namespace do hub de eventos do Azure| Microsoft.EventHub/<br>namespaces|  Diagnostics|    Diagnóstico||
|Hub IoT do Azure| Microsoft.Devices/<br>IotHubs||     Diagnóstico||
|Azure Key Vault|   Microsoft.KeyVault/<br>vaults|  Diagnóstico  || [Análise do Cofre de Chaves](../../azure-monitor/insights/azure-key-vault.md)|
|Azure Load Balancer|   Microsoft.Network/<br>loadBalancers|    Diagnóstico|||
|Aplicativos Lógicos do Azure|  Microsoft.Logic/<br>workflows|  Diagnóstico|    Diagnóstico||
||Microsoft.Logic/<br>integrationAccounts||||
|Grupos de segurança de rede|   Microsoft.Network/<br>networksecuritygroups|Diagnóstico||   [Análise do Grupo de Segurança de Rede do Azure](../../azure-monitor/insights/azure-networking-analytics.md#azure-application-gateway-and-network-security-group-analytics)|
|Cofres de recuperação|   Microsoft.RecoveryServices/<br>vaults|||[Análise dos Serviços de Recuperação do Azure (Visualização)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
|Serviços Search|   Microsoft.Search/<br>searchServices|    Diagnóstico|    Diagnóstico||
|Namespace do Barramento de Serviço| Microsoft.ServiceBus/<br>namespaces|    Diagnostics|Diagnóstico|    [Análise do Barramento de Serviço (Visualização)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
|Service Fabric||       Armazenamento||    [Análise do Service Fabric (visualização)](../../service-fabric/service-fabric-diagnostics-oms-setup.md)|
|SQL (v12)| Microsoft.Sql/<br>servers/<br>databases||       Diagnóstico||
||Microsoft.Sql/<br>servers/<br>elasticPools||||
|Armazenamento|||         Script| [Análise do Azure Storage (Visualização)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution)|
|Máquinas Virtuais do Azure|    Microsoft.Compute/<br>virtualMachines|  Extensão|  Extensão||
||||Diagnóstico||
|Conjuntos de dimensionamento de máquinas virtuais|    Microsoft.Compute/<br>virtualMachines    ||Diagnóstico||
||Microsoft.Compute/<br>virtualMachineScaleSets/<br>virtualMachines||||
|Farms do servidor Web|Microsoft.Web/<br>serverfarms||   Diagnóstico
|Sites|  Microsoft.Web/<br>sites ||      Diagnóstico|    [Mais informações](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring)|
||Microsoft.Web/<br>sites/<br>slots||||


## <a name="log-integration-with-on-premises-siem-systems"></a>Integração de log com sistemas SIEM locais

Com a Integração de Logs do Azure, você pode integrar registros brutos de recursos do Azure a seu sistema de SIEM (gerenciamento de evento e informações de segurança) local. Downloads de AzLog foram desabilitados em 27 de junho de 2018. Para obter diretrizes sobre o que fazer para prosseguir com a análise da postagem [Usar o Azure Monitor para a integração com ferramentas SIEM](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/)

![Diagrama de Integração de Log](./media/log-audit/azure-log-audit-fig9.png)

A Integração de Log coleta o Diagnóstico do Azure das Máquinas Virtuais do Windows, dos logs de atividades do Azure, dos alertas da Central de Segurança do Azure e dos logs do provedor de recursos do Azure. Esta integração fornece um painel unificado para todos os seus ativos, quer sejam locais ou na nuvem, para que você possa agregar, correlacionar, analisar e emitir alertas de eventos de segurança.

A Integração do Log dá suporte à integração dos logs de atividades do Azure, logs de eventos do Windows de Máquinas Virtuais do Windows com sua assinatura do Azure, alertas da Central de Segurança do Azure, logs de diagnóstico do Azure e logs de auditoria do Azure AD.

| Tipo de log | Logs de Azure Monitor que dão suporte a JSON (Splunk, ArcSight e IBM QRadar) |
| :------- | :-------------------------------------------------------- |
|Logs de auditoria do Azure AD|   Sim|
|Logs de atividade| Sim|
|Alertas da Central de Segurança |Sim|
|Logs de diagnóstico (logs de recurso)|  Sim|
|Logs da VM|   Sim, por meio de eventos encaminhados e não por meio do JSON|

[Introdução à Integração de Logs do Azure](azure-log-integration-get-started.md): Este tutorial explica as etapas de instalação da Integração de Logs do Azure, bem como a integração de logs do armazenamento do Azure, logs de atividades do Azure, alertas da Central de Segurança do Azure e logs de auditoria do Microsoft Azure Active Directory.

Cenários de integração para SIEM:

* [Etapas de configuração de parceiro](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): Essa postagem de blog mostra a você como configurar a Integração de Logs do Azure para trabalhar com as soluções de parceiros Splunk, HP ArcSight e IBM QRadar.

* [Perguntas frequentes sobre a Integração de Logs do Azure](azure-log-integration-faq.md): Este artigo de perguntas frequentes responde às perguntas sobre a Integração de Logs do Azure.

* [Integração de alertas da Central de Segurança com a Integração de Logs do Azure](../../security-center/security-center-export-data-to-siem.md): Este artigo discute como sincronizar alertas da central de segurança, os eventos de segurança de máquina virtual coletados pelos logs de diagnóstico do Azure e os logs de auditoria do Azure com seus logs de Azure Monitor ou solução SIEM.

## <a name="next-steps"></a>Próximas etapas

- [Auditoria e log](management-monitoring-overview.md): Proteger dados, mantendo a visibilidade e respondendo rapidamente aos alertas de segurança em tempo hábil.

- [Log de segurança e coleta de logs de auditoria no Azure](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/): Aplique essas configurações para ter certeza de que as instâncias do Azure estão coletando os logs de segurança e auditoria corretos.

- [Definir as configurações de auditoria de um conjunto de sites](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US): Se você for um administrador de uma coleção de sites, recupere o histórico de ações de usuários individuais e o histórico de ações realizadas durante um intervalo de datas específico. 

- [Pesquisar o log de auditoria no Centro de Segurança e Conformidade do Office 365](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US): Use o Centro de Segurança e Conformidade do Office 365 para pesquisar o log de auditoria unificado e exiba as atividades de usuário e do administrador em sua organização do Office 365.


