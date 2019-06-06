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
ms.openlocfilehash: 9765af13a22a0ae9db8a45c396e7d5a32b71b79c
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730314"
---
# <a name="how-to-configure-monitoring-in-azure-digital-twins"></a>Como configurar o monitoramento nos Gêmeos Digitais do Azure

Os Gêmeos Digitais do Azure dão suporte a registro em log, monitoramento e análise robustos. Os desenvolvedores de soluções podem usar os logs, logs de diagnóstico, o log de atividades e outros serviços do Azure Monitor para suportar as necessidades de monitoramento complexas de um aplicativo de IoT. As opções de registro em log podem ser combinadas para consultar ou exibir registros em vários serviços e para fornecer cobertura de registro em log granular para muitos serviços.

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

    [![Seleção](media/how-to-configure-monitoring/activity-log-select.png)](media/how-to-configure-monitoring/activity-log-select.png#lightbox)

1. A **Visão geral da Análise do Log de Atividades** resume dados de log de atividade essenciais:

    [![Visão geral da análise de log de atividade]( media/how-to-configure-monitoring/log-analytics-overview.png)]( media/how-to-configure-monitoring/log-analytics-overview.png#lightbox)

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
1. Clique em **Configurações de Diagnóstico**:

    [![Configurações de diagnóstico um](media/how-to-configure-monitoring/diagnostic-settings-one.png)](media/how-to-configure-monitoring/diagnostic-settings-one.png#lightbox)

1. Clique em **Ativar diagnósticos** para coletar dados (se habilitado anteriormente).
1. Preencha os campos solicitados e selecione como e onde os dados serão salvos:

    [![Duas configurações de diagnóstico](media/how-to-configure-monitoring/diagnostic-settings-two.png)](media/how-to-configure-monitoring/diagnostic-settings-two.png#lightbox)

    Os logs de diagnóstico geralmente são salvos usando [Azure File Storage](../storage/files/storage-files-deployment-guide.md) e é compartilhada com [registra em log do Azure Monitor](../azure-monitor/log-query/get-started-portal.md). Ambas as opções podem ser selecionadas.

>[!TIP]
>Use **logs de diagnóstico** para obter insights sobre operações de recursos.

## <a name="azure-monitor-and-log-analytics"></a>Log Analytics e Azure Monitor

Aplicativos de IoT unem diferentes recursos, dispositivos, locais e dados em um só local. O registro em log refinado fornece informações detalhadas sobre cada parte, serviço ou componente específicos da arquitetura geral do aplicativo, mas uma visão geral unificada costuma ser necessária para manutenção e depuração.

O Azure Monitor inclui o serviço de análise de log poderosa, que permite que o registro em log fontes para serem exibidos e analisados em um único local. O Azure Monitor, portanto, é muito útil para analisar logs de dentro de aplicativos sofisticados de IoT.

Os exemplos de uso incluem:

* Consulta a várias históricos de log de diagnóstico
* Visualizar os logs para várias funções definidas pelo usuário
* Exibir os logs para dois ou mais serviços dentro de um período de tempo específico

Consultar log completo é fornecido por meio [registra em log do Azure Monitor](../azure-monitor/log-query/log-query-overview.md). Para configurar esses recursos avançados:

1. No portal do Microsoft Azure, pesquise **Log Analytics**.
1. Você verá seu disponíveis **espaço de trabalho do Log Analytics** instâncias. Para consultar, escolha um e selecione **Logs**:

    [![O log analytics](media/how-to-configure-monitoring/log-analytics.png)](media/how-to-configure-monitoring/log-analytics.png#lightbox)

1. Se você ainda não tiver um **espaço de trabalho do Log Analytics** instância, você pode criar um espaço de trabalho clicando o **Add** botão:

    [![Criar o OMS](media/how-to-configure-monitoring/log-analytics-oms.png)](media/how-to-configure-monitoring/log-analytics-oms.png#lightbox)

Uma vez sua **espaço de trabalho do Log Analytics** instância é provisionada, você pode usar consultas avançadas para localizar entradas nos logs de múltiplos ou pesquisar usando critérios específicos usando **gerenciamento de Log**:

   [![Gerenciamento de log](media/how-to-configure-monitoring/log-analytics-management.png)](media/how-to-configure-monitoring/log-analytics-management.png#lightbox)

Para obter mais informações sobre as operações de consulta avançada, confira [Introdução às consultas](../azure-monitor/log-query/get-started-queries.md).

> [!NOTE]
> Você pode ter um atraso de 5 minutos, ao enviar eventos a serem **espaço de trabalho do Log Analytics** pela primeira vez.

Os logs do Azure Monitor também fornecem o erro poderoso e serviços de notificação de alerta, que podem ser exibidos clicando **diagnosticar e resolver problemas**:

   [![Notificações de alerta e erro](media/how-to-configure-monitoring/log-analytics-notifications.png)](media/how-to-configure-monitoring/log-analytics-notifications.png#lightbox)

>[!TIP]
>Use **espaço de trabalho do Log Analytics** para históricos de log de consulta para vários serviços, assinaturas ou as funcionalidades do aplicativo.

## <a name="other-options"></a>Outras opções

Os Gêmeos Digitais do Azure também dão suporte a registro em log específico de aplicativo e auditoria de segurança. Para obter uma visão completa de todas as opções de registro em log do Azure disponíveis para sua instância de Gêmeos Digitais do Azure, consulte o artigo [Auditoria de log do Azure](../security/azure-log-audit.md).

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre o [Log de Atividades](../azure-monitor/platform/activity-logs-overview.md) do Azure.

- Aprofunde-se nas configurações de diagnóstico do Azure lendo uma [Visão geral dos logs de diagnóstico](../azure-monitor/platform/diagnostic-logs-overview.md).

- Leia mais sobre [registra em log do Azure Monitor](../azure-monitor/log-query/get-started-portal.md).
