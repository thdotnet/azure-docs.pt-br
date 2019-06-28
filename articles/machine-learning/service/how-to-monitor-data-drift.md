---
title: Como detectar descompasso de dados (visualização) nas implantações do AKS
titleSuffix: Azure Machine Learning service
description: Saiba como detectar descompasso de dados no serviço Kubernetes do Azure implantado modelos no serviço Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: copeters
author: cody-dkdc
ms.date: 06/20/2019
ms.openlocfilehash: e4deeab28fb643ff32624ba9dd16574e621f508c
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332880"
---
# <a name="how-to-detect-data-drift-preview-on-models-deployed-to-azure-kubernetes-service"></a>Como detectar descompasso de dados (versão prévia) em modelos implantados no serviço de Kubernetes do Azure
Neste artigo, você aprenderá a monitorar [descompasso dados](concept-data-drift.md) entre os dados de conjunto de dados e Inferência de tipos de treinamento de um modelo implantado. 

Descompasso de dados é uma das principais razões a onde a precisão do modelo degrada ao longo do tempo. Isso acontece quando os dados fornecidos a um modelo na produção são diferentes de dados usados para treinar o modelo. O serviço de Azure Machine Learning pode monitorar descompasso de dados usando o Detector de descompasso de dados. Se o descompasso for detectado, o serviço pode enviar um alerta para você.  

> [!Note]
> Esse serviço está em (visualização) e limitadas em Opções de configuração. Consulte nosso [documentação da API](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/?view=azure-ml-py) e [notas de versão](azure-machine-learning-release-notes.md) para obter detalhes e atualizações. 

Com o serviço de Azure Machine Learning, você pode monitorar as entradas para um modelo implantado no AKS e comparar esses dados para o conjunto de dados de treinamento para o modelo. Em intervalos regulares, os dados de inferência de tipos são [de instantâneo e criação de perfil](how-to-explore-prepare-data.md), em seguida, calculado em relação ao conjunto de dados de linha de base para produzir uma análise de descompasso de dados que: 

+ Mede a magnitude do descompasso de dados, chamado de coeficiente de descompasso.
+ Medidas dados descompasso contribuição por recurso, informando quais recursos causou descompasso de dados.
+ Métricas de distância de medidas. Atualmente, Wasserstein e a distância de energia são computados.
+ Distribuições de medidas de recursos. Estimativa de densidade de kernel atualmente e histogramas.
+ Envie alertas de dados descompasso por email.

Para obter detalhes sobre como essas métricas são computadas, consulte o [conceito de descompasso dados](concept-data-drift.md) artigo.

## <a name="prerequisites"></a>Pré-requisitos

- Se você não tiver uma assinatura do Azure, crie uma conta gratuita antes de começar. Teste hoje mesmo a [versão gratuita ou paga do Serviço do Azure Machine Learning](https://aka.ms/AMLFree).

- Um espaço de trabalho do serviço do Machine Learning do Azure e o SDK do Azure Machine Learning para Python instalados. Aprenda como obter esses pré-requisitos usando o documento [Como configurar um ambiente de desenvolvimento](how-to-configure-environment.md).

- [Configurar seu ambiente](how-to-configure-environment.md)e, em seguida, instale o descompasso de dados usando o seguinte comando do SDK:

    ```
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

- Configurar o [coletor de dados de modelo](how-to-enable-data-collection.md) coletar dados da implantação do modelo de AKS e confirme se os dados é coletados no `modeldata` contêiner de blob.

## <a name="import-dependencies"></a>Importar dependências 
Importe as dependências usadas neste guia:

```python
# Azure ML service packages 
from azureml.core import Experiment, Run, RunDetails
from azureml.contrib.datadrift import DataDriftDetector, AlertConfiguration
``` 

## <a name="configure-data-drift"></a>Configurar o descompasso de dados 

O exemplo de Python a seguir demonstra como configurar o `DataDriftDetector` objeto:

```python
# if email address is specified, setup AlertConfiguration
alert_config = AlertConfiguration('your_email@contoso.com')

# create a new DatadriftDetector object
datadrift = DataDriftDetector.create(ws, model.name, model.version, services, frequency="Day", alert_config=alert_config)
    
print('Details of Datadrift Object:\n{}'.format(datadrift))
```

Para obter mais informações, consulte o [DataDrift](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/?view=azure-ml-py) referência.

## <a name="submit-a-datadriftdetector-run"></a>Enviar uma execução DataDriftDetector

Com o DataDriftDetector configurado, você pode enviar uma [dessincronização de dados executar](https://docs.microsoft.com/python/api/azureml-contrib-datadrift/azureml.contrib.datadrift.datadriftdetector%28class%29?view=azure-ml-py#run-target-date--services--compute-target-name-none--create-compute-target-false--feature-list-none--drift-threshold-none-) em uma determinada data para o modelo. 

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

## <a name="get-data-drift-analysis-results"></a>Obter os resultados da análise de dados descompasso

O exemplo de Python a seguir demonstra como plotar dados relevantes descompasso métricas. Você pode usar as métricas retornadas para criar visualizações personalizadas:

```python
# start and end are datetime objects 
drift_metrics = datadrift.get_output(start_time=start, end_time=end)

# Show all data drift result figures, one per serivice.
# If setting with_details is False (by default), only the data drift magnitude will be shown; if it's True, all details will be shown.
drift_figures = datadrift.show(with_details=True)
```

![Mostrar de descompasso de dados](media/how-to-monitor-data-drift/drift_show.png)

Para obter detalhes sobre as métricas que são computadas, consulte o [conceito de descompasso dados](concept-data-drift.md) artigo.

## <a name="schedule-data-drift-detection"></a>Detecção de descompasso de dados de agendamento 

Habilitar uma agenda de descompasso de dados executa um DataDriftDetector executado na frequência especificada. Se o coeficiente de descompasso está acima do limite especificado, um email é enviado. 

```python
datadrift.enable_schedule()
datadrift.disable_schedule()
```

A configuração do detector de descompasso de dados pode ser vista na página de detalhes do modelo no portal do Azure.

![Dados descompassos de configuração do portal do Azure](media/how-to-monitor-data-drift/drift_config.png)

## <a name="view-results-in-azure-ml-workspace-ui"></a>Exibir resultados na interface do usuário de espaço de trabalho de AM do Azure

Para exibir os resultados na IU do espaço de trabalho de AM do Azure, navegue até a página de modelo. Na guia Detalhes do modelo, a configuração de descompasso de dados é mostrada. Uma guia 'Descompasso de dados (visualização)' agora está disponível a visualização das métricas de descompasso de dados. 

![Descompasso de dados do portal do Azure](media/how-to-monitor-data-drift/drift_ui.png)

## <a name="setting-up-alerts"></a>Configurar Alertas 

Definindo o coeficiente de descompasso limite de alertas e fornecendo um endereço de email, uma [do Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) alerta por email será enviada se o coeficiente de descompasso está acima do limite. Todas as métricas de descompasso de dados são armazenadas no recurso de insights do aplicativo associado com o espaço de trabalho do serviço de Azure Machine Learning para que você possa configurar alertas personalizados ou ações. Você pode seguir o link do alerta de email para a consulta de informações do aplicativo.

![Alerta de Email de descompasso de dados](media/how-to-monitor-data-drift/drift_email.png)

## <a name="next-steps"></a>Próximas etapas

* Para obter um exemplo completo de como usar o descompasso de dados, consulte o [descompasso de dados do Azure ML notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/data-drift/azure-ml-datadrift.ipynb). Este bloco de anotações do Jupyter demonstra o uso de um [conjunto de dados do Azure aberto](https://docs.microsoft.com/azure/open-datasets/overview-what-are-open-datasets) para treinar um modelo para prever o clima, implantá-lo no AKS e o monitoramento descompasso de dados. 

* Podemos agradeceríamos muito seus comentários, sugestões ou perguntas conforme descompasso de dados move na direção geral de disponibilidade. Use o botão de comentários de produto abaixo! 
