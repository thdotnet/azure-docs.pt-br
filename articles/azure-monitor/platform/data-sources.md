---
title: Fontes de dados no Azure Monitor | Microsoft Docs
description: Descreve os dados disponíveis para monitorar a integridade e desempenho de seus recursos do Azure e dos aplicativos executados neles.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2019
ms.author: bwren
ms.openlocfilehash: b77fb3ab5651147c59b9f0afd22a2d6d0159c90e
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357474"
---
# <a name="sources-of-monitoring-data-for-azure-monitor"></a>Fontes de dados de monitoramento para o Azure Monitor
O Azure Monitor se baseia em uma [plataforma de dados de monitoramento comuns](data-platform.md) que inclui [Logs](data-platform-logs.md) e [métricas](data-platform-metrics.md). Coletar dados para essa plataforma permite que os dados de vários recursos a serem analisados em conjunto, usando um conjunto comum de ferramentas no Azure Monitor. Os dados de monitoramento também podem ser enviados para outros locais para oferecer suporte a determinados cenários, e alguns recursos podem gravar em outros locais antes de poderem ser coletadas em Logs ou métricas.

Este artigo descreve as diferentes fontes de dados coletados pelo Azure Monitor, além dos dados de monitoramento criados pelos recursos do Azure de monitoramento. São fornecidos links para obter informações detalhadas sobre a configuração necessária para coletar esses dados para diferentes locais.

## <a name="application-tiers"></a>Camadas de aplicativo

Fontes de dados de monitoramento de aplicativos do Azure podem ser organizados em camadas, as camadas mais altas de seu próprio aplicativo e as camadas inferiores, sendo os componentes da plataforma Azure. O método de acesso aos dados de cada camada varia. As camadas de aplicativo são resumidas na tabela a seguir e as fontes de dados em cada camada de monitoramento são apresentadas nas seções a seguir. Ver [monitoramento de locais de dados no Azure](data-locations.md) para obter uma descrição de cada local de dados e como você pode acessar seus dados.


![Camadas de monitoramento](../media/overview/overview.png)


### <a name="azure"></a>Azure
A tabela a seguir descreve brevemente as camadas de aplicativo que são específicas para o Azure. Seguir o link para obter mais detalhes sobre cada uma nas seções a seguir.

| Camada | DESCRIÇÃO | Método de coleta |
|:---|:---|:---|
| [Azure Tenant](#azure-tenant) | Dados sobre a operação de serviços do Azure no nível de locatário como Azure Active Directory. | Exibir dados do AAD no portal ou configurar a coleta para o Azure Monitor usando uma configuração de diagnóstico de locatário. |
| [Assinatura do Azure](#azure-subscription) | Dados relacionados à integridade e gerenciamento de serviços entre recursos na sua assinatura do Azure, como o Gerenciador de recursos e a integridade do serviço. | Exibir no portal ou configurar a coleta para o Azure Monitor usando um perfil de log. |
| [Recursos do Azure](#azure-resources) |  Dados sobre a operação e o desempenho de cada recurso do Azure. | As métricas coletadas automaticamente, exibir no Metrics Explorer.<br>Configure configurações de diagnóstico para coletar logs no Azure Monitor.<br>Monitoramento de soluções e informações disponíveis para o monitoramento mais detalhado para tipos específicos de recursos. |

### <a name="azure-other-cloud-or-on-premises"></a>Azure, outra nuvem ou local 
A tabela a seguir descreve brevemente as camadas de aplicativo que podem estar no Azure, outra nuvem ou local. Seguir o link para obter mais detalhes sobre cada uma nas seções a seguir.

| Camada | DESCRIÇÃO | Método de coleta |
|:---|:---|:---|
| [Sistema operacional (convidado)](#operating-system-guest) | Dados sobre o sistema operacional em recursos de computação. | Instale o agente do Log Analytics para coletar fontes de dados do cliente para o agente de dependência e o Azure Monitor para coletar dependências com suporte do Azure Monitor para VMs.<br>Para máquinas virtuais do Azure, instale a extensão de diagnóstico do Azure para coletar logs e métricas para o Azure Monitor. |
| [Código do aplicativo](#application-code) | Dados sobre o desempenho e a funcionalidade do aplicativo propriamente dito e do código, incluindo os rastreamentos de desempenho, logs de aplicativos e telemetria do usuário. | Instrumente seu código para coletar dados para o Application Insights. |
| [Fontes personalizadas](#custom-sources) | Dados de serviços externos ou outros componentes ou dispositivos. | Colete dados de log ou métricas para o Azure Monitor de qualquer cliente REST. |

## <a name="azure-tenant"></a>Locatário do Azure
A telemetria relacionada ao seu locatário do Azure é coletada de serviços de todos os locatários como o Azure Active Directory.

![Coleção de locatário do Azure](media/data-sources/tenant.png)

### <a name="azure-active-directory-audit-logs"></a>Logs de Auditoria do Azure Active Directory
Os [relatórios do Azure Active Directory](../../active-directory/reports-monitoring/overview-reports.md) contêm o histórico de atividade de entrada e a trilha de auditoria das alterações feitas em um locatário específico. 

| Destino | DESCRIÇÃO | Referência |
|:---|:---|:---|
| Logs do Azure Monitor | Configure logs do Azure AD para ser coletado no Azure Monitor para analisá-los com outros dados de monitoramento. | [Integrar logs do Azure AD com os logs do Azure Monitor (versão prévia)](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md) |
| Armazenamento do Azure | Exporte logs do Azure AD para o armazenamento do Azure para arquivamento. | [Tutorial: Arquivar logs do Azure AD a uma conta de armazenamento do Azure (visualização)](../../active-directory/reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md) |
| Hub de evento | Logs do Stream do Azure AD para outros locais usando os Hubs de eventos. | [Tutorial: Stream de logs do Azure Active Directory para um hub de eventos do Azure (visualização)](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md). |



## <a name="azure-subscription"></a>Assinatura do Azure
Telemetria relacionada à integridade e à operação de sua assinatura do Azure.

![Assinatura do Azure](media/data-sources/azure-subscription.png)

### <a name="azure-activity-log"></a>Log de Atividades do Azure 
O [log de atividades do Azure](activity-logs-overview.md) inclui registros de integridade de serviço juntamente com registros em todas as alterações de configuração feitas os recursos em sua assinatura do Azure. O Log de Atividades está disponível para todos os recursos do Azure e representa a exibição _externa_ desses recursos.

| Destino | DESCRIÇÃO | Referência |
|:---|:---|
| Log de atividades | O log de atividades é coletado em seu próprio armazenamento de dados que você pode exibir no menu do Azure Monitor ou usar para criar alertas do log de atividades. | [Consultar o log de atividades no portal do Azure](activity-log-view.md#azure-portal) |
| Logs do Azure Monitor | Configure Logs do Azure Monitor para coletar o log de atividades para analisá-lo com outros dados de monitoramento. | [Coletar e analisar logs de atividades do Azure no espaço de trabalho do Log Analytics no Azure Monitor](activity-log-collect.md) |
| Armazenamento do Azure | Exporte o log de atividades para o armazenamento do Azure para arquivamento. | [Arquivar log de atividades](activity-log-export.md#archive-activity-log)  |
| Hubs de Eventos | Stream do log de atividades para outros locais usando os Hubs de eventos | [Log de atividades do Stream para o Hub de eventos](activity-log-export.md#stream-activity-log-to-event-hub). |

### <a name="azure-service-health"></a>Integridade do Serviço do Azure
[A Integridade do Serviço do Azure](../../service-health/service-health-overview.md) fornece informações sobre a integridade dos serviços do Azure na assinatura dos quais o aplicativo e os recursos dependem.

| Destino | DESCRIÇÃO | Referência |
|:---|:---|:---|
| Log de atividades<br>Logs do Azure Monitor | Registros de integridade do serviço são armazenados no log de atividades do Azure, portanto, você pode exibi-los no portal do Azure ou execute quaisquer outras atividades que podem ser executadas com o log de atividades. | [Exibir as notificações de integridade do serviço usando o portal do Azure](service-notifications.md) |


## <a name="azure-resources"></a>Recursos do Azure
Métricas e logs de diagnóstico em nível de recurso fornecem informações sobre a operação _interna_ dos recursos do Azure. Eles estão disponíveis para a maioria dos serviços Azure e soluções de monitoramento e insights coletam dados adicionais para serviços específicos.

![Coleção de recursos do Azure](media/data-sources/azure-resources.png)


### <a name="platform-metrics"></a>Métricas de plataforma 
Maioria dos serviços Azure enviará [métricas de plataforma](data-platform-metrics.md) que reflitam seu desempenho e operação diretamente para o banco de dados de métricas. As [métricas específicas variam para cada tipo de recurso](metrics-supported.md). 

| Destino | DESCRIÇÃO | Referência |
|:---|:---|:---|
| Métricas do Azure Monitor | Métricas de plataforma gravará o banco de dados de métricas do Azure Monitor sem nenhuma configuração. Acessar as métricas de plataforma do Metrics Explorer.  | [Introdução ao Gerenciador de métricas do Azure](metrics-getting-started.md)<br>[Métricas compatíveis com o Azure Monitor](metrics-supported.md) |
| Logs do Azure Monitor | Copie as métricas de plataforma para Logs para análise de tendências e outro usando o Log Analytics. | [Direcionar o diagnóstico do Azure ao Log Analytics](collect-azure-metrics-logs.md#azure-diagnostics-direct-to-log-analytics) |
| Hubs de Eventos | Métricas de Stream em outros locais usando os Hubs de eventos. |[Transmitir os dados de monitoramento do Azure para um hub de eventos para consumo por uma ferramenta externa](stream-monitoring-data-event-hubs.md) |

### <a name="diagnostic-logs"></a>Logs de diagnóstico
[Os logs de diagnóstico](diagnostic-logs-overview.md) fornecem informações sobre a _interno_ operação de um recurso do Azure.  Os logs de diagnóstico não estão habilitados por padrão. Você deve habilitá-los e especificar um destino para cada recurso. 

Os requisitos de configuração e o conteúdo dos logs de diagnóstico variam por tipo de recurso, e nem todos os serviços ainda criam logs de diagnóstico. Ver [com suporte a serviços, esquemas e categorias para os Logs de diagnóstico do Azure](diagnostic-logs-schema.md) para obter detalhes sobre cada serviço e links para procedimentos de configuração detalhadas. Se o serviço não estiver listado neste artigo, em seguida, esse serviço no momento, não gravar os logs de diagnóstico.

| Destino | DESCRIÇÃO | Referência |
|:---|:---|:---|
| Logs do Azure Monitor | Envie Logs de diagnóstico para Logs do Azure Monitor para análise com outros dados coletados do log. Alguns recursos podem gravar diretamente para o Azure Monitor, enquanto outros gravar em uma conta de armazenamento antes de serem importadas para um espaço de trabalho do Log Analytics. | [Logs de diagnóstico do Azure Stream ao espaço de trabalho do Log Analytics no Azure Monitor](diagnostic-logs-stream-log-store.md)<br>[Usar o portal do Azure para coletar logs do armazenamento do Azure](azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage)  |
| Armazenamento | Diagnóstico de enviar logs para o armazenamento do Azure para o arquivamento. | [Arquivar Logs de diagnóstico do Azure](archive-diagnostic-logs.md) |
| Hubs de Eventos | Logs de diagnóstico do Stream em outros locais usando os Hubs de eventos. |[Logs de diagnóstico do Azure Stream para um hub de eventos](diagnostic-logs-stream-event-hubs.md) |

## <a name="operating-system-guest"></a>Sistema operacional (convidado)
Recursos de computação no Azure, em outras nuvens e localmente têm um sistema operacional convidado para monitorar. Com a instalação de um ou mais agentes, você pode coletar a telemetria do convidado para o Azure Monitor para analisá-los com as mesmas ferramentas de monitoramento como os próprios serviços do Azure.

![Coleta de recursos de computação do Azure](media/data-sources/compute-resources.png)

### <a name="azure-diagnostic-extension"></a>Extensão de diagnóstico do Azure
Habilitar a extensão de diagnóstico do Azure para máquinas virtuais do Azure permite que você para coletar logs e incluindo a máquina virtual da Web do serviço de nuvem do Azure (clássico) e funções de trabalho, máquinas virtuais, de recursos de computação de métricas do sistema operacional convidado do Azure conjuntos de dimensionamento e Service Fabric.

| Destino | DESCRIÇÃO | Referência |
|:---|:---|:---|
| Armazenamento | Quando você habilita a extensão de diagnóstico, ele gravará uma conta de armazenamento por padrão. | [Armazenar e exibir dados de diagnóstico no Armazenamento do Azure](diagnostics-extension-to-storage.md) |
| Métricas do Azure Monitor | Quando você configura a extensão de diagnóstico para coletar contadores de desempenho, eles são gravados no banco de dados de métricas do Azure Monitor. | [Enviar métricas de SO convidado para a métrica do Azure Monitor armazenam usando um modelo do Resource Manager para uma máquina virtual do Windows](collect-custom-metrics-guestos-resource-manager-vm.md) |
| Logs do Application Insights | Colete logs e contadores de desempenho do recurso de computação que dão suporte a seu aplicativo para ser analisado com outros dados de aplicativo. | [Enviar dados de diagnóstico de serviço de nuvem, Máquina Virtual ou do Service Fabric ao Application Insights](diagnostics-extension-to-application-insights.md) |
| Hubs de Eventos | Configure a extensão de diagnóstico para transmitir os dados para outros locais usando os Hubs de eventos.  | [Fluxo de dados de diagnóstico do Azure no afunilamento usando os Hubs de eventos](diagnostics-extension-stream-event-hubs.md) |

### <a name="log-analytics-agent"></a>Agente do log Analytics 
Instale o agente do Log Analytics para monitoramento abrangente e gerenciamento de suas máquinas virtuais Windows ou Linux. A máquina virtual pode estar em execução no Azure, em outra nuvem ou localmente.

| Destino | DESCRIÇÃO | Referência |
|:---|:---|:---|
| Logs do Azure Monitor | O agente do Log Analytics se conecta ao Azure Monitor diretamente ou por meio do System Center Operations Manager e permite que você colete dados de fontes de dados que você configurar ou de soluções que fornecem informações adicionais sobre aplicativos de monitoramento em execução na máquina virtual. | [Fontes de dados do agente no Azure Monitor](agent-data-sources.md)<br>[Conectar o Operations Manager para o Azure Monitor](om-agents.md) |


### <a name="azure-monitor-for-vms"></a>Azure Monitor para VMs 
[O Azure Monitor para VMs](../insights/vminsights-overview.md) fornece uma experiência de monitoramento personalizada para máquinas virtuais fornecendo recursos além da funcionalidade do Azure Monitor principal, incluindo o status do serviço e a integridade da VM. Ele requer um agente de dependência em máquinas virtuais Windows e Linux que se integra com o agente do Log Analytics para coletar dados de descobertas sobre processos em execução na máquina virtual e as dependências de processo externo.

| Destino | DESCRIÇÃO | Referência |
|:---|:---|:---|
| Logs do Azure Monitor | Armazena dados sobre processos e dependências no agente. | [Usando o Azure Monitor para máquinas virtuais (versão prévia) são mapeados para entender os componentes do aplicativo](../insights/vminsights-maps.md) |
| Armazenamento de VM | O Azure Monitor para VMs armazena informações de estado de integridade em um local personalizado. Isso só está disponível para o Azure Monitor para VMs no portal do Azure, além de [API REST do Azure Resource health](/rest/api/resourcehealth/). | [Entender a integridade de suas máquinas virtuais do Azure](../insights/vminsights-health.md)<br>[O Azure Resource health API REST](https://docs.microsoft.com/rest/api/resourcehealth/) |



## <a name="application-code"></a>Código do aplicativo
Monitoramento de aplicativos detalhados no Azure Monitor é feito com [Application Insights](https://docs.microsoft.com/azure/application-insights/) que coleta dados de aplicativos em execução em uma variedade de plataformas. O aplicativo pode estar em execução no Azure, em outra nuvem ou localmente.

![Coleta de dados do aplicativo](media/data-sources/applications.png)


### <a name="application-data"></a>Dados do aplicativo
Quando você habilita o Application Insights para um aplicativo por meio da instalação de um pacote de instrumentação, ele coleta as métricas e logs relacionados ao desempenho e à operação do aplicativo. Application Insights armazena os dados coletados na mesma plataforma de dados do Azure Monitor usada por outras fontes de dados. Ele inclui ferramentas abrangentes para analisar esses dados, mas você também pode analisá-lo com dados de outras fontes usando ferramentas como o Metrics Explorer e o Log Analytics.

| Destino | DESCRIÇÃO | Referência |
|:---|:---|:---|
| Logs do Azure Monitor | Dados operacionais sobre seu aplicativo, incluindo exibições de página, solicitações de aplicativos, exceções e rastreamentos. | [Analisar dados de log no Azure Monitor](../log-query/log-query-overview.md) |
|                    | Informações de dependência entre componentes de aplicativos para dar suporte ao mapa do aplicativo e a correlação de telemetria. | [Correlação de telemetria no Application Insights](../app/correlation.md) <br> [Mapa do aplicativo](../app/app-map.md) |
|            | Resultados de testes de disponibilidade que testar a disponibilidade e capacidade de resposta de seu aplicativo de diferentes locais na Internet pública. | [Monitorar a disponibilidade e a capacidade de resposta de qualquer site da Web](../app/monitor-web-app-availability.md) |
| Métricas do Azure Monitor | Application Insights coleta as métricas que descrevem o desempenho e a operação do aplicativo além das métricas personalizadas que você define no seu aplicativo para o banco de dados de métricas do Azure Monitor. | [Com base em log e agregadas previamente métricas no Application Insights](../app/pre-aggregated-metrics-log-metrics.md)<br>[API do Application Insights para métricas e eventos personalizados](../app/api-custom-events-metrics.md) |
| Armazenamento do Azure | Envie dados de aplicativo para o armazenamento do Azure para arquivamento. | [Exportar telemetria do Application Insights](../app/export-telemetry.md) |
|            | Detalhes de testes de disponibilidade são armazenados no armazenamento do Azure. Use o Application Insights no portal do Azure para baixar para análise local. Resultados de testes de disponibilidade são armazenados em Logs do Azure Monitor. | [Monitorar a disponibilidade e a capacidade de resposta de qualquer site da Web](../app/monitor-web-app-availability.md) |
|            | Dados de rastreamento do Profiler são armazenados no armazenamento do Azure. Use o Application Insights no portal do Azure para baixar para análise local.  | [Analisar os aplicativos de produção no Azure com o Application Insights](../app/profiler-overview.md) 
|            | Depure instantâneo de dados que são capturados para um subconjunto de exceções são armazenados no armazenamento do Azure. Use o Application Insights no portal do Azure para baixar para análise local.  | [Como os instantâneos funcionam](../app/snapshot-debugger.md#how-snapshots-work) |

## <a name="monitoring-solutions-and-insights"></a>Soluções de monitoramento e Insights
[Soluções de monitoramento](../insights/solutions.md) e [Insights](../insights/insights-overview.md) coletar dados para fornecer informações adicionais sobre a operação de um determinado serviço ou aplicativo. Eles podem abordar os recursos em diferentes camadas do aplicativo e até mesmo várias camadas.

### <a name="monitoring-solutions"></a>Soluções de monitoramento

| Destino | DESCRIÇÃO | Referência
|:---|:---|:---|
| Logs do Azure Monitor | Soluções de monitoramento de coletam dados em logs do Azure Monitor onde pode ser analisado usando a linguagem de consulta ou [modos de exibição](view-designer.md) que normalmente são incluídos na solução. | [Detalhes de coleta de dados para o monitoramento de soluções no Azure](../insights/solutions-inventory.md) |


### <a name="azure-monitor-for-containers"></a>O Azure Monitor para contêineres
[O Azure Monitor para contêineres](../insights/container-insights-overview.md) fornece uma experiência de monitoramento personalizada para [serviço de Kubernetes do Azure (AKS)](/azure/aks/). Ele coleta dados adicionais sobre esses recursos descritos na tabela a seguir.

| Destino | DESCRIÇÃO | Referência |
|:---|:---|:---|
| Logs do Azure Monitor | Armazenamentos de dados de monitoramento para o AKS, incluindo eventos, logs e inventário. Dados de métrica também são armazenados nos Logs para aproveitar sua funcionalidade de análise no portal. | [Compreender o desempenho de cluster do AKS com o Azure Monitor para contêineres](../insights/container-insights-analyze.md) |
| Métricas do Azure Monitor | Dados de métrica são armazenados no banco de dados de métrica para a unidade de visualização e alertas. | [Exibir métricas de contêiner no metrics explorer](../insights/container-insights-analyze.md#view-container-metrics-in-metrics-explorer) |
| Serviço de Kubernetes do Azure | Em ordem para uma experiência quase de tempo real, o Azure Monitor para contêineres apresenta os dados diretamente do serviço do Kubernetes do Azure no portal do Azure. | [Como exibir logs de contêiner em tempo real com o Azure Monitor para contêineres (versão prévia)](../insights/container-insights-live-logs.md) |

### <a name="azure-monitor-for-vms"></a>Azure Monitor para VMs
[O Azure Monitor para VMs](../insights/vminsights-overview.md) fornece uma experiência personalizada para monitorar as máquinas virtuais. Uma descrição dos dados coletados pelo Monitor do Azure para VMs está incluída na [sistema operacional (convidado)](#operating-system-guest) seção acima.

## <a name="custom-sources"></a>Fontes personalizadas
Além das camadas padrão de um aplicativo, você precisa monitorar outros recursos que têm a telemetria que não pode ser coletada com outras fontes de dados. Para esses recursos, grave esses dados em métricas ou Logs usando uma API do Azure Monitor.

![Coleção personalizada](media/data-sources/custom.png)

| Destino | Método | DESCRIÇÃO | Referência |
|:---|:---|:---|:---|
| Logs do Azure Monitor | API do Coletor de Dados | Coletar dados de log de qualquer cliente REST e armazenar no espaço de trabalho do Log Analytics. | [Enviar dados de log para o Azure Monitor com a API do coletor de dados HTTP (visualização pública)](data-collector-api.md) |
| Métricas do Azure Monitor | API de métricas personalizadas | Colete dados de métrica de qualquer cliente REST e o armazenamento no banco de dados de métricas do Azure Monitor. | [Enviar métricas personalizadas para um recurso do Azure para o repositório de métrica do Azure Monitor, usando uma API REST](metrics-store-custom-rest-api.md) |


## <a name="other-services"></a>Outros serviços
Outros serviços no Azure gravam dados para a plataforma de dados do Azure Monitor. Isso permite que você a analisar os dados coletados por esses serviços com os dados coletados pelo Monitor do Azure e aproveitar o mesmo ferramentas de análise e visualização.

| Serviço | Destino | DESCRIÇÃO | Referência |
|:---|:---|:---|:---|
| [Central de Segurança do Azure](/azure/security-center/) | Logs do Azure Monitor | A Central de segurança do Azure armazena os dados de segurança coletados em um espaço de trabalho do Log Analytics, que permite a serem analisados com outros dados de log coletados pelo Azure Monitor.  | [Coleta de dados na Central de Segurança do Azure](../../security-center/security-center-enable-data-collection.md) |
| [Azure Sentinel](/azure/sentinel/) | Logs do Azure Monitor | Sentinela do Azure armazena os dados coletados de fontes de dados diferentes em um espaço de trabalho do Log Analytics, que permite a serem analisados com outros dados de log coletados pelo Azure Monitor.  | [Conectar fontes de dados](/azure/sentinel/quickstart-onboard) |


## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre os [tipos de dados de monitoramento coletados pelo Azure Monitor](data-platform.md) e como exibir e analisar esses dados.
- Lista os [locais diferentes onde os recursos do Azure armazenam dados](data-locations.md) e como acessá-lo. 
