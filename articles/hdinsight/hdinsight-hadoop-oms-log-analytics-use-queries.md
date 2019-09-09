---
title: Consultar logs de Azure Monitor para monitorar os clusters do Azure HDInsight
description: Saiba como executar consultas em logs de Azure Monitor para monitorar trabalhos em execução em um cluster HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: hrasheed
ms.openlocfilehash: 031879ac1d0d2dd1148c0c37ee72c60d093f8a7d
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70809369"
---
# <a name="query-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>Consultar logs de Azure Monitor para monitorar clusters HDInsight

Conheça alguns cenários básicos sobre como usar os logs de Azure Monitor para monitorar os clusters do Azure HDInsight:

* [Analisar métricas de cluster HDInsight](#analyze-hdinsight-cluster-metrics)
* [Procurar as mensagens de log específicas](#search-for-specific-log-messages)
* [Criar alertas de eventos](#create-alerts-for-tracking-events)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>Pré-requisitos

* Você deve ter configurado um cluster HDInsight para usar logs de Azure Monitor e adicionar soluções de monitoramento de logs de Azure Monitor específicas do cluster HDInsight ao espaço de trabalho. Para obter instruções, consulte [usar logs de Azure monitor com clusters HDInsight](hdinsight-hadoop-oms-log-analytics-tutorial.md).

## <a name="analyze-hdinsight-cluster-metrics"></a>Analisar métricas de cluster HDInsight

Saiba como procurar métricas específicas para seu cluster HDInsight.

1. Abra o espaço de trabalho do Log Analytics que está associado ao seu cluster HDInsight do portal do Azure.
2. Selecione o bloco **pesquisa de logs**.
3. Digite a consulta a seguir na caixa de pesquisa para procurar todas as métricas de todas as métricas disponíveis para todos os clusters HDInsight configurados para usar logs de Azure Monitor e, em seguida, selecione **executar**.

        search *

    ![Pesquisar todas as métricas](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics.png "Pesquisar todas as métricas")

    A saída deve se parecer com:

    ![Pesquisar saída de todas as métricas](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-metrics-output.png "Pesquisar saída de todas as métricas")

5. No painel esquerdo, em **Tipo**, selecione uma métrica que você deseja examinar em profundidade e selecione **Aplicar**. A captura de tela mostra que o tipo `metrics_resourcemanager_queue_root_default_CL` é selecionado.

    > [!NOTE]  
    > Talvez você precise selecionar o botão **[+]Mais** para localizar a métrica que você está procurando. Além disso, o botão **Aplicar** está na parte inferior da lista. Portanto, você deve rolar para baixo para vê-lo.

    Observe que a consulta na caixa de texto muda para a mostrada na caixa realçada na captura de tela a seguir:

    ![Pesquisar métricas específicas](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-metrics.png "Pesquisar métricas específicas")

6. Para obter detalhes sobre esta métrica específica. Por exemplo, é possível refinar a saída existente com base na média de recursos usados em um intervalo de 10 minutos, categorizada por nome do cluster usando esta consulta:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize AggregatedValue = avg(UsedAMResourceMB_d) by ClusterName_s, bin(TimeGenerated, 10m)

7. Em vez de refinar com base na média de recursos usados, é possível usar a seguinte consulta para refinar os resultados com base em quando os recursos máximos foram usados (assim como o 90º e o 95º percentil) em uma janela de 10 minutos:

        search in (metrics_resourcemanager_queue_root_default_CL) * | summarize ["max(UsedAMResourceMB_d)"] = max(UsedAMResourceMB_d), ["pct95(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 95), ["pct90(UsedAMResourceMB_d)"] = percentile(UsedAMResourceMB_d, 90) by ClusterName_s, bin(TimeGenerated, 10m)

## <a name="search-for-specific-log-messages"></a>Procure as mensagens de log específicas

Saiba como procurar mensagens de erro durante uma janela de tempo específica. Estas etapas são apenas um exemplo de como você pode chegar à mensagem de erro na qual você está interessado. É possível usar qualquer propriedade disponível para procurar os erros que você está tentando localizar.

1. Abra o espaço de trabalho do Log Analytics que está associado ao seu cluster HDInsight do portal do Azure.
2. Selecione o bloco **pesquisa de logs**.
3. Digite a consulta a seguir para pesquisar todas as mensagens de erro de todos os clusters HDInsight configurados para usar logs de Azure Monitor e, em seguida, selecione **executar**. 

         search "Error"

    Você verá uma saída semelhante à esta saída:

    ![Pesquisar a saída de todos os erros](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-all-errors-output.png "Pesquisar a saída de todos os erros")

4. No painel esquerdo, na categoria **Tipo**, selecione um tipo de erro que você deseja ver de maneira aprofundada e então clique selecionar **Aplicar**.  Observe que os resultados são refinados para mostrar apenas o erro do tipo selecionado.
5. É possível obter mais detalhes sobre essa lista de erros específica usando as opções disponíveis no painel esquerdo. Por exemplo:

    - Para ver mensagens de erro de um nó de trabalho específico:

        ![Pesquisar erros específicos Saída1](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-refined.png "Pesquisar erros específicos Saída1")

    - Para ver se ocorreu um erro em uma determinada hora:

        ![Pesquisar erros específicos output2](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-time.png "Pesquisar erros específicos output2")

6. Para ver o erro específico. É possível clicar em **[+] mostrar mais** para examinar a mensagem de erro real.

    ![Pesquisar erros específicos output3](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-search-specific-error-arrived.png "Pesquisar erros específicos output3")

## <a name="create-alerts-for-tracking-events"></a>Criar alertas para eventos de rastreamento

A primeira etapa para criar um alerta é chegar a uma consulta com base na qual o alerta é disparado. É possível usar qualquer consulta que você desejar para criar um alerta.

1. Abra o espaço de trabalho do Log Analytics que está associado ao seu cluster HDInsight do portal do Azure.
2. Selecione o bloco **pesquisa de logs**.
3. Execute a consulta a seguir na qual você deseja criar um alerta e, em seguida, pressione **RUN**.

        metrics_resourcemanager_queue_root_default_CL | where AppsFailed_d > 0

    A consulta fornece uma lista de aplicativos com falha em execução em clusters de HDInsight.

4. Selecione **Nova Regra de Alerta** no topo da página.

    ![Inserir consulta para criar um alert1](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert-query.png "Inserir consulta para criar um alert1")

5. Na janela **Criar regra**, digite a consulta e outros detalhes para criar um alerta e, em seguida, selecione **Criar regra de alerta**.

    ![Inserir consulta para criar um alert2](./media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-create-alert.png "Inserir consulta para criar um alert2")

Para editar ou excluir um alerta existente:

1. Abra o espaço de trabalho do Log Analytics no portal do Azure.
2. No menu esquerdo, selecione **Alerta**.
3. Selecione o alerta que você deseja editar ou excluir.
4. Você tem as seguintes opções: **Salvar**, **Descartar**, **Desabilitar** e **Excluir**.

    ![Log de Azure Monitor do HDInsight logs de exclusão de alertas](media/hdinsight-hadoop-oms-log-analytics-use-queries/hdinsight-log-analytics-edit-alert.png)

Para obter mais informações, confira [Criar, exibir e gerenciar alertas de métrica usando o Azure Monitor](../azure-monitor/platform/alerts-metric.md).

## <a name="see-also"></a>Consulte também

* [Criar exibições personalizadas usando o designer de exibição no Azure Monitor](../azure-monitor/platform/view-designer.md)
* [Criar, exibir e gerenciar alertas de métrica usando Azure Monitor](../azure-monitor/platform/alerts-metric.md)
