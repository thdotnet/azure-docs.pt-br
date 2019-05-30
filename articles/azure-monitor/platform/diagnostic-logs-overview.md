---
title: Visão Geral dos Logs de Diagnóstico
description: Saiba mais sobre os logs de diagnóstico do Azure no Azure Monitor e como usá-los para compreender os eventos que ocorrem dentro de um recurso do Azure.
author: nkiest
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: nikiest
ms.subservice: logs
ms.openlocfilehash: 8902e29baa5802e3416bcda97ca59a5576d41829
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244873"
---
# <a name="overview-of-azure-diagnostic-logs"></a>Visão Geral dos Logs de Diagnóstico

**Os logs de diagnóstico** fornecem dados avançados e frequentes sobre a operação de um recurso do Azure. O Monitor do Azure disponibiliza dois tipos de logs de diagnóstico:

* **Registros de locatário** - esses registros são provenientes de serviços no nível de inquilino que existem fora de uma assinatura do Azure, como os logs do Active Directory do Azure.
* **Registros de recursos** - esses registros são provenientes dos serviços do Azure que implantam recursos em uma assinatura do Azure, como Grupos de segurança de rede ou Contas de armazenamento.

    ![Logs de diagnóstico do recurso versus outros tipos de logs](media/diagnostic-logs-overview/Diagnostics_Logs_vs_other_logs_v5.png)

O conteúdo desses logs varia de acordo com o tipo de recurso e serviço do Azure. Por exemplo, os contadores de regras do Network Security Group e as auditorias do Key Vault são dois tipos de logs de diagnóstico.

Esses logs diferem de [log de atividades](activity-logs-overview.md). O log de atividades fornece informações sobre as operações que foram executadas em recursos em sua assinatura usando o Resource Manager, por exemplo, criar uma máquina virtual ou excluir um aplicativo lógico. O log de atividades é um log de nível de assinatura. Os logs de diagnóstico no nível do recurso fornecem informações sobre as operações executadas dentro do próprio recurso, por exemplo, obtenção de um segredo de um Key Vault.

Esses logs também diferem dos logs de diagnóstico no nível do SO guest. Os logs de diagnóstico do sistema operacional convidado são aqueles coletados por um agente em execução dentro de uma máquina virtual ou outro tipo de recurso com suporte. Os logs de diagnóstico no nível do recurso não exigem um agente e capturam dados específicos ao recurso da própria plataforma do Azure, enquanto os logs de diagnóstico no nível do sistema operacional convidado capturam dados do sistema operacional e de aplicativos em execução em uma máquina virtual.

Nem todos os serviços suportam os logs de diagnóstico descritos aqui. [Este artigo contém uma lista de seção quais serviços oferecem suporte a logs de diagnóstico](./../../azure-monitor/platform/diagnostic-logs-schema.md).

## <a name="what-you-can-do-with-diagnostic-logs"></a>O que você pode fazer com os Logs de Diagnóstico
Aqui estão algumas coisas que você pode fazer com os Logs de Diagnóstico:

![Posicionamento lógico dos Logs de Diagnóstico](./media/diagnostic-logs-overview/Diagnostics_Logs_Actions.png)

* Salve-os em uma [**Conta de Armazenamento**](../../azure-monitor/platform/archive-diagnostic-logs.md) para auditoria ou inspeção manual. Você pode especificar o tempo de retenção (em dias) usando as **Configurações de Diagnóstico do Recurso**.
* [Stream eles **dos Hubs de eventos** ](diagnostic-logs-stream-event-hubs.md) para ingestão por um serviço de terceiros ou uma solução de análises personalizadas, como o Power BI.
* Analise-os com o [Azure Monitor](../../azure-monitor/platform/collect-azure-metrics-logs.md), onde os dados são gravados imediatamente para registar o Azure Monitor com a necessidade de gravar primeiro os dados para o armazenamento.  

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Você pode usar uma conta de armazenamento ou um namespace de Hubs de Evento que não esteja na mesma assinatura que os logs emissores. O usuário que define a configuração deve ter o devido acesso RBAC para ambas as assinaturas.

> [!NOTE]
>  Atualmente, não é possível arquivar logs de fluxo de rede para uma conta de armazenamento que esteja atrás de uma rede virtual protegida.

## <a name="diagnostic-settings"></a>Configurações de Diagnóstico

Logs de diagnóstico de recursos são configurados usando configurações de diagnóstico de recursos. Os logs de diagnóstico do inquilino são configurados usando uma configuração de diagnóstico de locatário. **Configurações de Diagnóstico** para um controle de recursos:

* Para onde os logs e métricas de diagnóstico são enviados (Conta de Armazenamento, Hubs de Eventos e/ou Azure Monitor).
* Quais categorias de log são enviadas e se os dados de métrica também são enviados.
* Quanto tempo cada categoria de log deve ser mantida em uma conta de armazenamento.
    - Uma retenção de zero dias significa que os logs serão mantidos indefinidamente. Caso contrário, o valor pode ser qualquer número de dias entre 1 e 365.
    - Se as políticas de retenção estiverem definidas, mas o armazenamento de logs em uma Conta de Armazenamento estiver desabilitado (por exemplo, se apenas as opções Hubs de Eventos ou Log Analytics forem selecionadas), as políticas de retenção não terão efeito.
    - As políticas de retenção são aplicadas por dia, para que, ao final de um dia (UTC), os logs do dia após a política de retenção sejam excluídos. Por exemplo, se você tiver uma política de retenção de um dia, no início do dia de hoje, os logs de anteontem serão excluídos. A exclusão começa à meia-noite UTC, mas observe que pode levar até 24 horas para que os logs sejam excluídos da conta de armazenamento.

Essas configurações são configuradas das configurações de diagnóstico no portal, com os comandos do PowerShell do Azure e a CLI, ou que usam o [API de REST do Azure Monitor](https://docs.microsoft.com/rest/api/monitor/).

> [!NOTE]
> Atualmente, não há suporte para o envio da métrica multidimensional por meio das configurações de diagnóstico. As métricas com dimensões são exportadas como métricas dimensionais simples, agregadas nos valores da dimensão.
>
> *Por exemplo*: A métrica “Mensagens de Entrada” em um Hub de Eventos pode ser explorada e mapeada por nível da fila. No entanto, quando exportada por meio das configurações de diagnóstico, a métrica será representada como todas as mensagens de entrada em todas as filas no Hub de Eventos.
>
>

## <a name="supported-services-categories-and-schemas-for-diagnostic-logs"></a>Serviços com suporte, categorias e esquemas para logs de diagnóstico

[Consulte este artigo](../../azure-monitor/platform/diagnostic-logs-schema.md) para obter uma lista de serviços com suporte e as categorias de log e os esquemas usados por esses serviços.

## <a name="next-steps"></a>Próximas etapas

* [Transmitir logs de diagnóstico de recurso os **Hubs de Eventos**](diagnostic-logs-stream-event-hubs.md)
* [Alterar as configurações de diagnóstico do recurso usando a API REST do Azure Monitor](https://docs.microsoft.com/rest/api/monitor/)
* [Analisar logs do Armazenamento do Azure com o Azure Monitor](collect-azure-metrics-logs.md)
