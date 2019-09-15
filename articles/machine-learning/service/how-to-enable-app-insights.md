---
title: Configurar informações de Aplicativo Azure para monitorar modelos de ML
titleSuffix: Azure Machine Learning
description: Monitorar serviços Web implantados com Azure Machine Learning usando o Aplicativo Azure insights
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 07/12/2019
ms.custom: seoapril2019
ms.openlocfilehash: 785507e9ae12d8da564a223c8cdf544a98b8de61
ms.sourcegitcommit: e97a0b4ffcb529691942fc75e7de919bc02b06ff
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/15/2019
ms.locfileid: "71002879"
---
# <a name="monitor-your-azure-machine-learning-models-with-application-insights"></a>Monitore seus modelos do Azure Machine Learning com o Application Insights

Neste artigo, você aprenderá a configurar o Aplicativo Azure insights para Azure Machine Learning. O Application Insights lhe dá a oportunidade de monitorar:
* Solicitar taxas, tempos de resposta e taxas de falha.
* Taxas de dependência, tempos de resposta e taxas de falha.
* Exceções.

[Saiba mais sobre o Application Insights](../../azure-monitor/app/app-insights-overview.md). 


## <a name="prerequisites"></a>Pré-requisitos

* Se você não tiver uma assinatura do Azure, crie uma conta gratuita antes de começar. Experimente a [versão gratuita ou paga do Azure Machine Learning](https://aka.ms/AMLFree) hoje.

* Um workspace do Azure Machine Learning, um diretório local que contém seus scripts e o SDK de Aprendizado de Máquina do Azure para Python instalado. Para saber como obter esses pré-requisitos, consulte [como configurar um ambiente de desenvolvimento](how-to-configure-environment.md).
* Um modelo de aprendizado de máquina treinado para ser implantado para o serviço de Kubernetes do Azure (AKS) ou Instância de Contêiner do Azure (ACI). Se você não tiver um, consulte o tutorial [Modelo de classificação de imagem de trem](tutorial-train-models-with-aml.md).


## <a name="use-sdk-to-configure"></a>Usar o SDK para configurar 

### <a name="update-a-deployed-service"></a>Atualizar um serviço implantado
1. Identificar o serviço no seu workspace. O valor de `ws` é o nome do seu workspace.

    ```python
    from azureml.core.webservice import Webservice
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Atualizar seu serviço e habilitar o Application Insights. 

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Rastreamentos de log personalizados em seu serviço
Se você deseja registrar rastreamentos personalizados, siga o processo de implantação padrão para o AKS ou ACI no documento [Como e onde implantar](how-to-deploy-and-where.md). Em seguida, execute as seguintes etapas:

1. Atualize o arquivo de pontuação adicionando instruções de impressão.
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. Atualize a configuração de serviço.
    
    ```python
    config = Webservice.deploy_configuration(enable_app_insights=True)
    ```

3. Criar uma imagem e implantá-lo no [AKS](how-to-deploy-to-aks.md) ou [ACI](how-to-deploy-to-aci.md).  

### <a name="disable-tracking-in-python"></a>Desabilitar o controle em Python

Para desabilitar o Application Insights, use o seguinte código:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```
    
## <a name="use-portal-to-configure"></a>Usar o portal para configurar

Você pode ativar e desativar o Application Insights no portal do Azure.

1. No [portal do Azure](https://portal.azure.com), abra o workspace.

1. Na guia **Implantações**, selecione o serviço no qual você deseja ativar o Application Insights.

   [![Lista de serviços na guia implantações](media/how-to-enable-app-insights/Deployments.PNG)](./media/how-to-enable-app-insights/Deployments.PNG#lightbox)

3. Selecione **Editar**.

   [![Botão Editar](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

4. Em **Configurações avançadas**, marque a caixa de seleção **Ativar diagnósticos do AppInsights**.

   [![Selecionada caixa de seleção para habilitar diagnósticos](media/how-to-enable-app-insights/AdvancedSettings.png)](./media/how-to-enable-app-insights/AdvancedSettings.png#lightbox)

1. Selecione **Atualizar** na parte inferior da tela para aplicar as alterações. 

### <a name="disable"></a>Desabilitar
1. No [portal do Azure](https://portal.azure.com), abra o workspace.
1. Selecione **implantações**, selecione o serviço e selecione **editar**.

   [![Use o botão Editar](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

1. Na **configurações avançadas**, desmarque as **AppInsights habilitar diagnóstico** caixa de seleção. 

   [![Caixa de seleção desmarcada para habilitar o diagnóstico](media/how-to-enable-app-insights/uncheck.png)](./media/how-to-enable-app-insights/uncheck.png#lightbox)

1. Selecione **Atualizar** na parte inferior da tela para aplicar as alterações. 
 

## <a name="evaluate-data"></a>Avaliar dados
Os dados do serviço são armazenados em sua conta do Application Insights, dentro do mesmo grupo de recursos que Azure Machine Learning.
Para exibi-lo:
1. Acesse o workspace do serviço do Machine Learning no [portal do Azure](https://portal.azure.com) e clique no link do Application Insights.

    [![AppInsightsLoc](media/how-to-enable-app-insights/AppInsightsLoc.png)](./media/how-to-enable-app-insights/AppInsightsLoc.png#lightbox)

1. Selecione a guia de **Visão geral** para ver um conjunto básico de métricas para o serviço.

   [![Visão geral](media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

3. Para examinar os rastreamentos personalizados, selecione **Analytics**.
4. Na seção de esquema, selecione **Traces**. Em seguida, selecione **executar** para executar sua consulta. Os dados devem aparecer em um formato de tabela e devem ser mapeados para suas chamadas personalizadas em seu arquivo de pontuação. 

   [![Rastreamentos personalizados](media/how-to-enable-app-insights/logs.png)](./media/how-to-enable-app-insights/logs.png#lightbox)

Para saber mais sobre como usar o Application Insights, consulte [o que é o Application Insights?](../../azure-monitor/app/app-insights-overview.md).


## <a name="example-notebook"></a>Caderno de exemplo

O bloco de anotações [Enable-app-Revisions-in-Production-Service. ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/enable-app-insights-in-production-service/enable-app-insights-in-production-service.ipynb) demonstra conceitos neste artigo. 
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Próximas etapas
Você também pode coletar dados em seus modelos em produção. Leia o artigo [coletar dados para modelos em produção](how-to-enable-data-collection.md). 

Leia também [Azure monitor para contêineres](https://docs.microsoft.com/azure/monitoring/monitoring-container-insights-overview?toc=%2fazure%2fmonitoring%2ftoc.json).
