---
title: Descompasso de dados de monitoramento (versão prévia)
titleSuffix: Azure Machine Learning service
description: Saiba como monitorar o serviço de Azure Machine Learning para descompasso de dados.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: cody-dkdc
ms.author: copeters
ms.date: 06/20/2019
ms.openlocfilehash: 56761c32484d4f5b27800e56143c62d3731da852
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67333137"
---
# <a name="what-is-data-drift-monitoring-preview"></a>O que são dados descompasso monitoramento (versão prévia)?

Descompasso de dados é a alteração na distribuição de dados. No contexto de aprendizado de máquina, modelos de aprendizado de máquina treinada pode enfrentar desempenho degradado previsão por causa de descompasso. Descompasso entre dados de treinamento e os dados usados para fazer previsões de monitoramento pode ajudar a detectar problemas de desempenho.

Modelos de aprendizado de máquina só são tão bons quanto os dados usados para treiná-los. Implantação de modelos para produção sem monitoramento de seu desempenho pode levar a impactos prejudiciais e não detectados. Com o monitoramento de descompasso de dados, você pode detectar e se adaptar a dessincronização de dados. 

## <a name="when-to-monitor-for-data-drift"></a>Quando monitorar os descompassos de dados?

As métricas que podemos monitorar incluem:

+ Magnitude de descompasso (descompasso coeficiente)
+ Causa do descompasso (descompasso contribuição por recurso)
+ Métrica de distância (Wasserstein, energia, etc.)

Com esse monitoramento em vigor, alertas ou ações podem ser definidas quando descompasso é detectado e o cientista de dados pode investigar a causa raiz do problema. 

Se você acha que os dados de entrada para seu modelo implantado podem mudar, considere a possibilidade de usar detecção de descompasso de dados.

## <a name="how-data-drift-is-monitored-in-azure-machine-learning-service"></a>Como os descompassos de dados é monitorado no serviço do Azure Machine Learning

Usando o **serviço do Azure Machine Learning**, descompasso de dados é monitorado por meio de conjuntos de dados ou implantações. Para monitorar os descompassos de dados, um conjunto de dados de linha de base - geralmente treinamento conjunto de dados para um modelo – é especificado. Um segundo conjunto de dados – geralmente os dados de entrada do modelo são coletados de uma implantação - é testado com o conjunto de dados de linha de base. Ambos os conjuntos de dados são [perfilados](how-to-create-dataset-snapshots.md) e o serviço de monitoramento de descompasso de entrada para os dados. Um modelo de aprendizado de máquina é treinado para detectar diferenças entre dois conjuntos de dados. O desempenho do modelo é convertido para o coeficiente de descompasso, que mede a magnitude do descompasso entre dois conjuntos de dados. Usando o [possibilidade de interpretação de modelo](machine-learning-interpretability-explainability.md) os recursos que contribuíram para o coeficiente de descompasso são computados. O perfil de conjunto de dados, informações estatísticas sobre cada recurso são controladas. 

## <a name="data-drift-metric-output"></a>Saída da métrica dados descompasso

Há várias maneiras de exibir as métricas de descompasso:

* Use o widget do Jupyter.
* Use o `get_metrics()` função em qualquer `datadriftRun` objeto.
* Exibir as métricas no portal do Azure em seu modelo

As métricas a seguir são salvas em cada iteração de execução para uma tarefa de descompasso de dados:

|Métrica|DESCRIÇÃO|
--|--|
wasserstein_distance|Distância de estatística definida para distribuição numérica unidimensional.|
energy_distance|Distância de estatística definida para distribuição numérica unidimensional.|
datadrift_coefficient|Formalmente Matthews coeficiente de correlação, um número real que varia de -1 a 1. No contexto de descompasso, 0 não indica nenhum descompasso e 1 indica o descompasso do máximo.|
datadrift_contribution|Importância do recurso dos recursos que contribuem para o descompasso.|

## <a name="next-steps"></a>Próximas etapas

Consulte exemplos e saiba como monitorar o descompasso de dados:

+ [Saiba como monitorar o descompasso de dados em modelos implantados por meio do serviço de Kubernetes do Azure (AKS)](how-to-monitor-data-drift.md)
+ Experimente [exemplos de Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/data-drift/)