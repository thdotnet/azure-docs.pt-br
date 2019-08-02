---
title: Como configurar o monitoramento nos Gêmeos Digitais do Azure | Microsoft Docs
description: Como configurar o monitoramento nos Gêmeos Digitais do Azure.
author: kingdomofends
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 06/03/2018
ms.author: v-adgera
ms.custom: seodec18
ms.openlocfilehash: 66a0b98e2cae14c3708cf8a6aa3f8bd498f41c9f
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68609647"
---
# <a name="how-to-configure-monitoring-in-azure-digital-twins"></a>Como configurar o monitoramento nos Gêmeos Digitais do Azure

Os Gêmeos Digitais do Azure dão suporte a registro em log, monitoramento e análise robustos. As soluções que os desenvolvedores podem usar Azure Monitor logs, logs de diagnóstico, log de atividades e outros serviços para dar suporte às necessidades complexas de monitoramento de um aplicativo de IoT. As opções de registro em log podem ser combinadas para consultar ou exibir registros em vários serviços e para fornecer cobertura de registro em log granular para muitos serviços.

Este artigo resume as opções de registro em log e monitoramento e como combiná-las de maneiras específicas para os Gêmeos Digitais do Azure.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="review-activity-logs"></a>Revisar logs de atividade

Os [logs de atividade](../azure-monitor/platform/activity-logs-overview.md) do Azure fornecem insights rápidos em históricos de operações e eventos de nível de assinatura para cada instância de serviço do Azure.

Os eventos de nível de assinatura incluem:

* Criação de recursos
* Remoção de recursos
* Criação de segredos do aplicativo
* Integração com outros serviços

O log de atividades dos Gêmeos Digitais do Azure é habilitado por padrão e pode ser encontrado no portal do Azure ao:

1. Selecionar sua instância de Gêmeos Digitais do Azure.
1. Escolher o **log de atividades** para exibir o painel de exibição:

    [![Log de atividades](media/how-to-configure-monitoring/activity-log.png)](media/how-to-configure-monitoring/activity-log.png#lightbox)

Para o registro de log de atividades avançadas:

1. Selecione a opção **Logs** para exibir a **Visão geral da Análise do Log de Atividades**:

    [![Selecionado](media/how-to-configure-monitoring/activity-log-select.png)](media/how-to-configure-monitoring/activity-log-select.png#lightbox)

1. A **Visão geral da Análise do Log de Atividades** resume dados de log de atividade essenciais:

    [![Visão geral do log Analytics de atividades]( media/how-to-configure-monitoring/log-analytics-overview.png)]( media/how-to-configure-monitoring/log-analytics-overview.png#lightbox)

>[!TIP]
>Use **logs de atividade** para insights rápidos de eventos no nível da assinatura.

## <a name="enable-customer-diagnostic-logs"></a>Habilite logs de diagnóstico do cliente

As [Configurações de diagnóstico](../azure-monitor/platform/diagnostic-logs-overview.md) do Azure podem ser definidas para cada instância do Azure para complementar o log de atividades. Enquanto os logs de atividade pertencem aos eventos de nível de assinatura, o registro de log de diagnóstico fornece insights sobre o histórico operacional dos próprios recursos.

Exemplos de registro de log de diagnóstico incluem:

* O tempo de execução para uma função definida pelo usuário
* O código de status de resposta de uma solicitação de API bem-sucedida
* Recuperar uma chave do aplicativo de um cofre

Para habilitar logs de diagnóstico, siga estas etapas:

1. Abra os recursos no portal do Azure.
1. Selecione **configurações de diagnóstico**:

    [![Configurações de diagnóstico um](media/how-to-configure-monitoring/diagnostic-settings-one.png)](media/how-to-configure-monitoring/diagnostic-settings-one.png#lightbox)

1. Selecione **Ativar diagnóstico** para coletar dados (se não tiver sido habilitado anteriormente).
1. Preencha os campos solicitados e selecione como e onde os dados serão salvos:

    [![Configurações de diagnóstico duas](media/how-to-configure-monitoring/diagnostic-settings-two.png)](media/how-to-configure-monitoring/diagnostic-settings-two.png#lightbox)

    Os logs de diagnóstico são frequentemente salvos usando o [armazenamento de arquivos do Azure](../storage/files/storage-files-deployment-guide.md) e compartilhados com [logs de Azure monitor](../azure-monitor/log-query/get-started-portal.md). Ambas as opções podem ser selecionadas.

>[!TIP]
>Use **logs de diagnóstico** para obter insights sobre operações de recursos.

## <a name="azure-monitor-and-log-analytics"></a>Log Analytics e Azure Monitor

Aplicativos de IoT unem diferentes recursos, dispositivos, locais e dados em um só local. O registro em log refinado fornece informações detalhadas sobre cada parte, serviço ou componente específicos da arquitetura geral do aplicativo, mas uma visão geral unificada costuma ser necessária para manutenção e depuração.

Azure Monitor inclui o poderoso serviço log Analytics, que permite que as fontes de log sejam exibidas e analisadas em um único local. O Azure Monitor, portanto, é muito útil para analisar logs de dentro de aplicativos sofisticados de IoT.

Os exemplos de uso incluem:

* Consulta a várias históricos de log de diagnóstico
* Visualizar os logs para várias funções definidas pelo usuário
* Exibir os logs para dois ou mais serviços dentro de um período de tempo específico

A consulta de log completa é fornecida por meio de [logs de Azure monitor](../azure-monitor/log-query/log-query-overview.md). Para configurar esses recursos avançados:

1. No portal do Microsoft Azure, pesquise **Log Analytics**.
1. Você verá suas instâncias de **espaço de trabalho log Analytics** disponíveis. Para consultar, escolha um e selecione **Logs**:

    [![Análise de log](media/how-to-configure-monitoring/log-analytics.png)](media/how-to-configure-monitoring/log-analytics.png#lightbox)

1. Se você ainda não tiver uma instância de **espaço de trabalho log Analytics** , poderá criar um espaço de trabalho selecionando o botão **Adicionar** :

    [![Criar OMS](media/how-to-configure-monitoring/log-analytics-oms.png)](media/how-to-configure-monitoring/log-analytics-oms.png#lightbox)

Depois que a instância do **espaço de trabalho do log Analytics** for provisionada, você poderá usar consultas poderosas para localizar entradas em vários logs ou Pesquisar usando critérios específicos usando o gerenciamento de **log**:

   [![Gerenciamento de log](media/how-to-configure-monitoring/log-analytics-management.png)](media/how-to-configure-monitoring/log-analytics-management.png#lightbox)

Para obter mais informações sobre as operações de consulta avançada, confira [Introdução às consultas](../azure-monitor/log-query/get-started-queries.md).

> [!NOTE]
> Você pode experimentar um atraso de 5 minutos ao enviar eventos para **log Analytics espaço de trabalho** pela primeira vez.

Os logs de Azure Monitor também fornecem serviços de notificação de erro e alerta avançados, que podem ser exibidos selecionando **diagnosticar e resolver problemas**:

   [![Alertas e notificações de erro](media/how-to-configure-monitoring/log-analytics-notifications.png)](media/how-to-configure-monitoring/log-analytics-notifications.png#lightbox)

>[!TIP]
>Use **log Analytics espaço de trabalho** para consultar históricos de log para várias funcionalidades, assinaturas ou serviços do aplicativo.

## <a name="other-options"></a>Outras opções

Os Gêmeos Digitais do Azure também dão suporte a registro em log específico de aplicativo e auditoria de segurança. Para obter uma visão completa de todas as opções de registro em log do Azure disponíveis para sua instância de Gêmeos Digitais do Azure, consulte o artigo [Auditoria de log do Azure](../security/fundamentals/log-audit.md).

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre o [Log de Atividades](../azure-monitor/platform/activity-logs-overview.md) do Azure.

- Aprofunde-se nas configurações de diagnóstico do Azure lendo uma [Visão geral dos logs de diagnóstico](../azure-monitor/platform/diagnostic-logs-overview.md).

- Leia mais sobre [logs de Azure monitor](../azure-monitor/log-query/get-started-portal.md).
