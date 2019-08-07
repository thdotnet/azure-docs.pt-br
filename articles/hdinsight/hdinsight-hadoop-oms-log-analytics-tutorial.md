---
title: Usar logs de Azure Monitor para monitorar os clusters do Azure HDInsight
description: Saiba como usar os logs de Azure Monitor para monitorar trabalhos em execução em um cluster HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 08/05/2019
ms.openlocfilehash: 7d015f485a51ae1f929e2ecaf1a05811d21594a2
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816020"
---
# <a name="use-azure-monitor-logs-to-monitor-hdinsight-clusters"></a>Usar logs de Azure Monitor para monitorar clusters HDInsight

Saiba como habilitar logs de Azure Monitor para monitorar as operações de cluster do Hadoop no HDInsight e como adicionar uma solução de monitoramento do HDInsight.

[Os logs de Azure monitor](../log-analytics/log-analytics-overview.md) são um serviço no Azure monitor que monitora seus ambientes locais e de nuvem para manter a disponibilidade e o desempenho. Ele coleta dados gerados pelos recursos em seus ambientes de nuvem e locais e de outras ferramentas de monitoramento para fornecer análise de várias fontes.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

* **Um espaço de trabalho do Log Analytics**. Você pode considerar esse espaço de trabalho como um ambiente de logs de Azure Monitor exclusivo com seu próprio repositório de dados, fontes de dados e soluções. Para obter instruções, consulte [Criar um espaço de trabalho do Log Analytics](../azure-monitor/learn/quick-collect-azurevm.md#create-a-workspace).

* **Um cluster Azure HDInsight**. No momento, você pode usar os logs de Azure Monitor com os seguintes tipos de cluster HDInsight:

  * Hadoop
  * HBase
  * Consulta Interativa
  * Kafka
  * Spark
  * Storm

  Para obter instruções sobre como criar um cluster HDInsight, consulte [Introdução ao Azure HDInsight](hadoop/apache-hadoop-linux-tutorial-get-started.md).  

* **Azure PowerShell módulo AZ**.  Consulte [apresentando o novo módulo Azure PowerShell AZ](https://docs.microsoft.com/powershell/azure/new-azureps-module-az).

> [!NOTE]  
> É recomendável colocar o cluster HDInsight e o espaço de trabalho do Log Analytics na mesma região para melhorar o desempenho. Os logs de Azure Monitor não estão disponíveis em todas as regiões do Azure.

## <a name="enable-azure-monitor-logs-by-using-the-portal"></a>Habilitar logs de Azure Monitor usando o portal

Nesta seção, você configurará um cluster HDInsight Hadoop existente para usar um espaço de trabalho do Azure Log Analytics para monitorar trabalhos, logs de depuração, etc.

1. No [portal do Azure](https://portal.azure.com/), selecione o cluster.  Consulte [lista e mostrar clusters](./hdinsight-administer-use-portal-linux.md#showClusters) para obter instruções. O cluster é aberto em uma nova página do Portal.

1. Da esquerda, sob **Monitoramento**, selecione **Conjunto de Gerenciamento de Operações**.

1. Na exibição principal, sob **Monitoramento de OMS**, selecione **Habilitar**.

1. Na lista suspensa **Selecionar espaço de trabalho**, selecione um espaço de trabalho do Log Analytics existente.

1. Clique em **Salvar**.  Levará alguns minutos para salvar a configuração.

    ![Habilitar o monitoramento para clusters HDInsight](./media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-enable-monitoring.png "Habilitar o monitoramento para clusters HDInsight")

## <a name="enable-azure-monitor-logs-by-using-azure-powershell"></a>Habilitar logs de Azure Monitor usando Azure PowerShell

Você pode habilitar os logs de Azure Monitor usando o cmdlet [Enable-AzHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/az.hdinsight/enable-azhdinsightoperationsmanagementsuite) do módulo Azure PowerShell AZ.

```powershell
# Enter user information
$resourceGroup = "<your-resource-group>"
$cluster = "<your-cluster>"
$LAW = "<your-Log-Analytics-workspace>"
# End of user input

# obtain workspace id for defined Log Analytics workspace
$WorkspaceId = (Get-AzOperationalInsightsWorkspace -ResourceGroupName $resourceGroup -Name $LAW).CustomerId

# obtain primary key for defined Log Analytics workspace
$PrimaryKey = (Get-AzOperationalInsightsWorkspace -ResourceGroupName $resourceGroup -Name $LAW | Get-AzOperationalInsightsWorkspaceSharedKeys).PrimarySharedKey

# Enables Operations Management Suite
Enable-AzHDInsightOperationsManagementSuite -ResourceGroupName $resourceGroup -Name $cluster -WorkspaceId $WorkspaceId -PrimaryKey $PrimaryKey
```

Para desabilitar, use o cmdlet [Disable-AzHDInsightOperationsManagementSuite](https://docs.microsoft.com/powershell/module/az.hdinsight/disable-azhdinsightoperationsmanagementsuite) :

```powershell
Disable-AzHDInsightOperationsManagementSuite -Name "<your-cluster>"
```

## <a name="install-hdinsight-cluster-management-solutions"></a>Instalar solução de gerenciamento do cluster do Microsoft Azure HDInsight

O HDInsight fornece soluções de gerenciamento específicas de cluster que você pode adicionar para logs de Azure Monitor. As [soluções de gerenciamento](../log-analytics/log-analytics-add-solutions.md) adicionam funcionalidade a logs de Azure monitor, fornecendo dados adicionais e ferramentas de análise. Essas soluções coletam métricas de desempenho importantes de seus clusters HDInsight e fornecem as ferramentas para pesquisá-las. Essas soluções também fornecem visualizações e painéis para a maioria dos tipos de cluster com suporte no HDInsight. Usando as métricas que coleta com a solução, você pode criar alertas e regras de monitoramentos personalizadas.

Estas são as soluções de Microsoft Azure HDInsight disponíveis:

* Monitoramento do HDInsight
* Monitoramento do HBase no HDInsight
* Monitoramento de Consulta Interativo do Microsoft Azure HDInsight
* Monitoramento do Kafka HDInsight
* Monitoramento do HDInsight Spark
* Monitoramento do HDInsight Storm

Para obter instruções para instalar uma solução de gerenciamento, consulte [Soluções de Gerenciamento do Azure](../azure-monitor/insights/solutions.md#install-a-monitoring-solution). Para experimentar, instale uma solução de monitoramento do Hadoop do HDInsight. Quando estiver pronto, você verá uma peça **HDInsightHadoop** listada em **Resumo**. Selecione a peça **HDInsightHadoop**. A solução HDInsightHadoop é semelhante a:

![Modo de exibição de solução de monitoramento do HDInsight](media/hdinsight-hadoop-oms-log-analytics-tutorial/hdinsight-oms-hdinsight-hadoop-monitoring-solution.png)

Como o cluster é uma marca nova, o relatório não mostra todas as atividades.

## <a name="configuring-performance-counters"></a>Configurando contadores de desempenho

O Azure monitor também dá suporte à coleta e análise de métricas de desempenho para os nós no cluster. Para obter mais informações sobre como habilitar e configurar esse recurso, consulte [fontes de dados de desempenho do Linux no Azure monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-performance-counters#linux-performance-counters).

## <a name="next-steps"></a>Próximas etapas

* [Consultar logs de Azure Monitor para monitorar clusters HDInsight](hdinsight-hadoop-oms-log-analytics-use-queries.md)
