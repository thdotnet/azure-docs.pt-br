---
title: Detectar descompasso de dados (visualização) nas implantações do AKS
titleSuffix: Azure Machine Learning service
description: Detecte descompasso de dados no serviço Kubernetes do Azure implantado modelos no serviço Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: copeters
author: cody-dkdc
ms.date: 07/08/2019
ms.openlocfilehash: 3b8152bde8b7e44dde1b0b9c82216333778f83da
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67806015"
---
# <a name="detect-data-drift-preview-on-models-deployed-to-azure-kubernetes-service-aks"></a>Detectar descompasso de dados (versão prévia) em modelos implantados para o serviço de Kubernetes do Azure (AKS)

Neste artigo, você aprenderá como monitorar o descompasso de dados entre o conjunto de dados de treinamento e Inferência de dados de um modelo implantado. No contexto de aprendizado de máquina, modelos de aprendizado de máquina treinada pode enfrentar desempenho degradado previsão por causa de descompasso. Com o serviço de Azure Machine Learning, você pode monitorar o descompasso de dados e o serviço pode enviar um alerta por email para você quando um descompasso é detectado.

## <a name="what-is-data-drift"></a>O que é o descompasso de dados?

Descompasso de dados ocorre quando dados fornecidos a um modelo na produção são diferentes de dados usados para treinar o modelo. É um dos motivos principais em que a precisão do modelo degrada ao longo do tempo, portanto, os dados de monitoramento descompasso ajuda a detectar problemas de desempenho do modelo. 

## <a name="what-can-i-monitor"></a>O que pode monitorar?

Com o serviço de Azure Machine Learning, você pode monitorar as entradas para um modelo implantado no AKS e comparar esses dados para o conjunto de dados de treinamento para o modelo. Em intervalos regulares, os dados de inferência de tipos são [de instantâneo e criação de perfil](how-to-explore-prepare-data.md), em seguida, calculado em relação ao conjunto de dados de linha de base para produzir uma análise de descompasso de dados que: 

+ Mede a magnitude do descompasso de dados, chamado de coeficiente de descompasso.
+ Medidas dados descompasso contribuição por recurso, informando quais recursos causou descompasso de dados.
+ Métricas de distância de medidas. Atualmente, Wasserstein e a distância de energia são computados.
+ Distribuições de medidas de recursos. Estimativa de densidade de kernel atualmente e histogramas.
+ Envie alertas de dados descompasso por email.

> [!Note]
> Esse serviço está em (visualização) e limitadas em Opções de configuração. Consulte nosso [documentação da API](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/?view=azure-ml-py) e [notas de versão](azure-machine-learning-release-notes.md) para obter detalhes e atualizações. 

### <a name="how-data-drift-is-monitored-in-azure-machine-learning-service"></a>Como os descompassos de dados é monitorado no serviço do Azure Machine Learning

Usando o serviço Azure Machine Learning, o descompasso de dados é monitorado por meio de conjuntos de dados ou implantações. Para monitorar os descompassos de dados, um conjunto de dados de linha de base - geralmente treinamento conjunto de dados para um modelo – é especificado. Um segundo conjunto de dados – geralmente os dados de entrada do modelo são coletados de uma implantação - é testado com o conjunto de dados de linha de base. Ambos os conjuntos de dados são [perfilados](how-to-explore-prepare-data.md#explore-with-summary-statistics) e o serviço de monitoramento de descompasso de entrada para os dados. Um modelo de aprendizado de máquina é treinado para detectar diferenças entre dois conjuntos de dados. O desempenho do modelo é convertido para o coeficiente de descompasso, que mede a magnitude do descompasso entre dois conjuntos de dados. Usando o [possibilidade de interpretação de modelo](machine-learning-interpretability-explainability.md), os recursos que contribuem para o coeficiente de descompasso são computados. O perfil de conjunto de dados, informações estatísticas sobre cada recurso são controladas. 

## <a name="prerequisites"></a>Pré-requisitos

- Uma assinatura do Azure. Se você não tiver uma, crie uma conta gratuita, antes de começar. Experimente a [versão gratuita ou paga do Serviço do Azure Machine Learning](https://aka.ms/AMLFree) hoje mesmo.

- Um espaço de trabalho do serviço do Machine Learning do Azure e o SDK do Azure Machine Learning para Python instalados. Use as instruções descritas em [Criar um workspace do Serviço do Azure Machine Learning](setup-create-workspace.md#sdk) para fazer o seguinte:

    - Criar um ambiente Miniconda
    - Instalar o SDK do Azure Machine Learning para Python
    - Criar um workspace
    - Gravar um arquivo de configuração do espaço de trabalho (aml_config/config.json).

- Instale o descompasso de dados usando o seguinte comando do SDK:

    ```shell
    pip install azureml-contrib-datadrift
    ```

- Criar uma [conjunto de dados](how-to-create-register-datasets.md) dos dados de treinamento do seu modelo.

- Especifique o conjunto de dados de treinamento quando [registrando](concept-model-management-and-deployment.md) o modelo. O exemplo a seguir demonstra como usar o `datasets` parâmetro para especificar o conjunto de dados de treinamento:

    ```python
    model = Model.register(model_path=model_file,
                        model_name=model_name,
                        workspace=ws,
                        datasets=datasets)

    print(model_name, image_name, service_name, model)
    ```

- [Habilitar a coleta de dados de modelo](how-to-enable-data-collection.md) coletar dados da implantação do modelo de AKS e confirme se os dados é coletados no `modeldata` contêiner de blob.

## <a name="configure-data-drift"></a>Configurar o descompasso de dados
Para configurar o descompasso de dados para o seu teste, importe as dependências conforme mostrado no exemplo de Python a seguir. 

Este exemplo demonstra como configurar o [ `DataDriftDetector` ](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift.datadriftdetector.datadriftdetector?view=azure-ml-py) objeto:

```python
# Import Azure ML packages
from azureml.core import Experiment, Run, RunDetails
from azureml.contrib.datadrift import DataDriftDetector, AlertConfiguration

# if email address is specified, setup AlertConfiguration
alert_config = AlertConfiguration('your_email@contoso.com')

# create a new DatadriftDetector object
datadrift = DataDriftDetector.create(ws, model.name, model.version, services, frequency="Day", alert_config=alert_config)
    
print('Details of Datadrift Object:\n{}'.format(datadrift))
```

## <a name="submit-a-datadriftdetector-run"></a>Enviar uma execução DataDriftDetector

Com o `DataDriftDetector` objeto configurado, você pode enviar uma [dessincronização de dados executar](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift.datadriftdetector%28class%29?view=azure-ml-py#run-target-date--services--compute-target-name-none--create-compute-target-false--feature-list-none--drift-threshold-none-) em uma determinada data para o modelo. Como parte da execução, habilitar alertas DataDriftDetector, definindo o `drift_threshold` parâmetro. Se o [datadrift_coefficient](#metrics) estiver acima de determinada `drift_threshold`, um email é enviado.

```python
# adhoc run today
target_date = datetime.today()

# create a new compute - creates datadrift-server
run = datadrift.run(target_date, services, feature_list=feature_list, create_compute_target=True)

# or specify existing compute cluster
run = datadrift.run(target_date, services, feature_list=feature_list, compute_target='cpu-cluster')

# show details of the data drift run
exp = Experiment(ws, datadrift._id)
dd_run = Run(experiment=exp, run_id=run)
RunDetails(dd_run).show()
```

## <a name="visualize-drift-metrics"></a>Visualizar métricas de descompasso

<a name="metrics"></a>

Depois de enviar seu DataDriftDetector executar, são capazes de ver as métricas de descompasso que são salvas em cada iteração de execução para uma tarefa de descompasso de dados:


|Métrica|DESCRIÇÃO|
--|--|
wasserstein_distance|Distância de estatística definida para distribuição numérica unidimensional.|
energy_distance|Distância de estatística definida para distribuição numérica unidimensional.|
datadrift_coefficient|Calculado da mesma forma como o coeficiente de correlação de Matthew, mas essa saída é um número real variando de 0 a 1. No contexto de descompasso, 0 não indica nenhum descompasso e 1 indica o descompasso do máximo.|
datadrift_contribution|Importância do recurso dos recursos que contribuem para o descompasso.|

Há várias maneiras de exibir as métricas de descompasso:

* Use o widget do Jupyter.
* Use o [ `get_metrics()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py#get-metrics-name-none--recursive-false--run-type-none--populate-false-) função em qualquer `datadrift` executar objeto.
* Exibir as métricas no portal do Azure em seu modelo

O exemplo de Python a seguir demonstra como plotar dados relevantes descompasso métricas. Você pode usar as métricas retornadas para criar visualizações personalizadas:

```python
# start and end are datetime objects 
drift_metrics = datadrift.get_output(start_time=start, end_time=end)

# Show all data drift result figures, one per serivice.
# If setting with_details is False (by default), only the data drift magnitude will be shown; if it's True, all details will be shown.
drift_figures = datadrift.show(with_details=True)
```

![Consulte descompasso de dados detectado pelo Azure Machine Learning](media/how-to-monitor-data-drift/drift_show.png)


## <a name="schedule-data-drift-scans"></a>Verificações de descompasso de dados de agendamento 

Quando você habilita a detecção de descompasso de dados, um DataDriftDetector é executado na frequência especificada, agendada. Se o datadrift_coefficient atingir a determinado `drift_threshold`, um email é enviado com cada execução agendada. 

```python
datadrift.enable_schedule()
datadrift.disable_schedule()
```

A configuração do detector de descompasso de dados pode ser vista na página de detalhes do modelo no portal do Azure.

![Dados descompassos de configuração do portal do Azure](media/how-to-monitor-data-drift/drift_config.png)

## <a name="view-results-in-azure-ml-workspace-ui"></a>Exibir resultados na interface do usuário de espaço de trabalho de AM do Azure

Para exibir os resultados na IU do espaço de trabalho de AM do Azure, navegue até a página de modelo. Na guia Detalhes do modelo, a configuração de descompasso de dados é mostrada. Uma guia 'Descompasso de dados (visualização)' agora está disponível a visualização das métricas de descompasso de dados. 

![Descompasso de dados do portal do Azure](media/how-to-monitor-data-drift/drift_ui.png)

## <a name="receiving-drift-alerts"></a>Receber alertas de descompasso

Definindo o coeficiente de descompasso limite de alertas e fornecendo um endereço de email, uma [do Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) alerta por email é enviada automaticamente sempre que o coeficiente de descompasso está acima do limite. 

Para configurar alertas personalizados e as ações que, todas as métricas de descompasso de dados são armazenadas na [Application Insights](how-to-enable-app-insights.md) recurso que foi criado, juntamente com o espaço de trabalho do serviço de Azure Machine Learning. Você pode seguir o link do alerta de email para a consulta do Application Insights.

![Alerta de Email de descompasso de dados](media/how-to-monitor-data-drift/drift_email.png)

## <a name="retrain-your-model-after-drift"></a>Treinar novamente seu modelo depois de descompasso

Quando dados descompasso afeta negativamente o desempenho do seu modelo implantado, é hora de treinar novamente o modelo. O seguinte [ `diff()` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py#diff-rhs-dataset--compute-target-none--columns-none-
) método lhe dá uma ideia inicial do que mudou entre os conjuntos de dados de treinamento de novos e antigos. 

```python
from azureml.core import Dataset

old_training_dataset.diff(new_training_dataset)
```

Com base na saída do código anterior, você talvez queira treinar novamente seu modelo. Para fazer isso, continue com as etapas a seguir.

* Investigue os dados coletados e preparar dados para treinar o novo modelo.
* Dividi-la em dados de treinamento e teste.
* Treine o modelo novamente usando os novos dados.
* Avalie o desempenho do modelo recém-gerado.
* Implante o novo modelo se o desempenho é melhor do que o modelo de produção.

## <a name="next-steps"></a>Próximas etapas

* Para obter um exemplo completo de como usar o descompasso de dados, consulte o [descompasso de dados do Azure ML notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/data-drift/azure-ml-datadrift.ipynb). Este bloco de anotações do Jupyter demonstra o uso de um [conjunto de dados do Azure aberto](https://docs.microsoft.com/azure/open-datasets/overview-what-are-open-datasets) para treinar um modelo para prever o clima, implantá-lo no AKS e o monitoramento descompasso de dados. 

* Podemos agradeceríamos muito seus comentários, sugestões ou perguntas conforme descompasso de dados move na direção geral de disponibilidade. Use o botão de comentários de produto abaixo! 
