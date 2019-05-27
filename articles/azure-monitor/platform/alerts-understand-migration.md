---
title: Entender como a ferramenta de migração voluntária funciona para alertas do Azure Monitor
description: Entender como funciona a ferramenta de migração de alertas e solucionar problemas.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: b5a13254fc9dfd58db83a1bc8b9dd071cfbbdab2
ms.sourcegitcommit: db3fe303b251c92e94072b160e546cec15361c2c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66015587"
---
# <a name="understand-how-the-migration-tool-works"></a>Entender como funciona a ferramenta de migração

Como [anunciado anteriormente](monitoring-classic-retirement.md), alertas clássicos no Azure Monitor estão sendo desativadas em setembro de 2019 (foi originalmente de 2019 de julho). Uma ferramenta de migração está disponível no portal do Azure para clientes que usam regras de alerta clássicas e que queiram disparar a migração em si.

Este artigo explica como funciona a ferramenta de migração voluntária. Ele também descreve soluções para alguns problemas comuns.

> [!NOTE]
> Devido a atraso na disponibilização da ferramenta de migração, a data de desativação para a migração de alertas clássicos tiver sido [estendido até 31 de agosto de 2019](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/) da data do dia 30 de junho de 2019 originalmente anunciada.

## <a name="which-classic-alert-rules-can-be-migrated"></a>Quais regras de alerta clássicas podem ser migradas?

Embora a ferramenta pode migrar praticamente todas clássicas regras de alerta, há algumas exceções. As seguintes regras de alerta não serão migradas por meio da ferramenta (ou durante a migração automática em setembro de 2019):

- Regras de alerta clássicas nas métricas de convidado de máquina virtual (Windows e Linux). Consulte a [orientações para recriar essas regras de alerta em novos alertas de métrica](#guest-metrics-on-virtual-machines) mais adiante neste artigo.
- Regras de alerta clássicas nas métricas de armazenamento clássico. Consulte a [orientação para monitoramento de suas contas de armazenamento clássico](https://azure.microsoft.com/blog/modernize-alerting-using-arm-storage-accounts/).
- Regras de alerta clássicas em algumas métricas da conta de armazenamento. Ver [detalhes](#storage-account-metrics) mais adiante neste artigo.

Se sua assinatura tiver tais regras clássicas, você deverá migrá-los manualmente. Porque não podemos fornecer uma migração automática, quaisquer alertas de métrica clássicas, existentes desses tipos continuarão a funcionar até de 2020 de junho. Essa extensão oferece-lhe tempo para mover para novos alertas. No entanto, nenhum novo alerta clássica pode ser criada depois de agosto de 2019.

### <a name="guest-metrics-on-virtual-machines"></a>Métricas de convidado em máquinas virtuais

Antes de criar novos alertas de métrica em métricas de convidado, as métricas de convidado devem ser enviadas para o armazenamento do Azure Monitor métricas personalizadas. Siga estas instruções para habilitar o coletor do Azure Monitor em configurações de diagnóstico:

- [Habilitando métricas de convidado para VMs do Windows](collect-custom-metrics-guestos-resource-manager-vm.md)
- [Habilitando métricas de convidado para VMs do Linux](collect-custom-metrics-linux-telegraf.md)

Depois de concluir essas etapas, você pode criar novos alertas de métrica em métricas de convidado. E, depois de criar novos alertas de métrica, você pode excluir alertas clássicos.

### <a name="storage-account-metrics"></a>Métricas da conta de armazenamento

Todos os alertas clássicos em contas de armazenamento podem ser migrados, exceto alertas sobre essas métricas:

- PercentAuthorizationError
- PercentClientOtherError
- PercentNetworkError
- PercentClientOtherError
- PercentSuccess
- PercentThrottlingError
- PercentTimeoutError
- AnonymousThrottlingError
- SASThrottlingError

Alerta clássica regras nas métricas de porcentagem devem ser migradas com base em [o mapeamento entre as métricas de armazenamento novas e antigas](https://docs.microsoft.com/azure/storage/common/storage-metrics-migration#metrics-mapping-between-old-metrics-and-new-metrics). Os limites precisará ser modificada de maneira apropriada porque a nova métrica disponível é absoluto.

Regras de alerta clássicas no AnonymousThrottlingError e SASThrottlingError devem ser divididas em dois novos alertas porque não há nenhuma métrica combinada que fornece a mesma funcionalidade. Os limites precisará ser adaptado adequadamente.

## <a name="rollout-phases"></a>Fases de distribuição

A ferramenta de migração está distribuindo em fases para clientes que usam regras de alerta clássicas. Os proprietários da assinatura receberá um email quando a assinatura está pronta para ser migrada por meio da ferramenta.

> [!NOTE]
> Como a ferramenta está sendo revertida em fases, você poderá ver que a maioria das suas assinaturas ainda não está pronta para ser migrados durante as fases iniciais.

No momento, um subconjunto das assinaturas é marcado como pronta para a migração. O subconjunto inclui as assinaturas que têm regras de alerta clássicas somente nos seguintes tipos de recursos. Suporte para mais tipos de recursos será adicionado em futuras fases.

- Microsoft.apimanagement/service
- Microsoft.batch/batchaccounts
- Microsoft.cache/redis
- Microsoft.datafactory/datafactories
- Microsoft.dbformysql/servers
- Microsoft.dbforpostgresql/servers
- Microsoft.eventhub/namespaces
- Microsoft.logic/workflows
- Microsoft.network/applicationgateways
- Microsoft.network/dnszones
- Microsoft.network/expressroutecircuits
- Microsoft.network/loadbalancers
- Microsoft.network/networkwatchers/connectionmonitors
- Microsoft.network/publicipaddresses
- Microsoft.network/trafficmanagerprofiles
- Microsoft.network/virtualnetworkgateways
- Microsoft.search/searchservices
- Microsoft.servicebus/namespaces
- Microsoft.streamanalytics/streamingjobs
- Microsoft.timeseriesinsights/environments
- Microsoft.web/hostingenvironments/workerpools
- Microsoft.web/serverfarms
- Microsoft.web/sites

## <a name="who-can-trigger-the-migration"></a>Quem pode disparar a migração?

Qualquer usuário que tenha a função interna de Colaborador de monitoramento no nível da assinatura pode disparar a migração. Os usuários que têm uma função personalizada com as seguintes permissões também podem disparar a migração:

- */leitura
- Microsoft.Insights/actiongroups/*
- Microsoft.Insights/AlertRules/*
- Microsoft.Insights/metricAlerts/*

## <a name="common-problems-and-remedies"></a>Problemas comuns e soluções

Depois que você [disparar a migração](alerts-using-migration-tool.md), você receberá os endereços que você forneceu para notificá-lo que a migração foi concluída ou se nenhuma ação é necessária no email. Esta seção descreve alguns problemas comuns e como lidar com eles.

### <a name="validation-failed"></a>Falha na validação

Devido a algumas alterações recentes a regras de alerta clássicas em sua assinatura, a assinatura não pode ser migrada. Esse problema é temporário. Você pode reiniciar a migração, depois que o status de migração move de volta **prontos para a migração** em alguns dias.

### <a name="policy-or-scope-lock-preventing-us-from-migrating-your-rules"></a>Impedindo a migrar suas regras de bloqueio de política ou escopo

Como parte da migração, serão criados novos alertas de métrica e novos grupos de ação e, em seguida, as regras de alerta clássicas serão excluídas. No entanto, há uma política ou o escopo de bloqueio impedindo a criação de recursos. Dependendo do escopo ou a política de bloqueio, algumas ou todas as regras não pôde ser migradas. Você pode resolver esse problema removendo o bloqueio de escopo ou a política temporariamente e disparar a migração novamente.

## <a name="next-steps"></a>Próximas etapas

- [Como usar a ferramenta de migração](alerts-using-migration-tool.md)
- [Preparar para a migração](alerts-prepare-migration.md)
