---
title: O que é automatizado ML / automl
titleSuffix: Azure Machine Learning service
description: Saiba como Serviço do Azure Machine Learning pode escolher um algoritmo para você e automaticamente gerar um modelo proveniente para economizar tempo usando os parâmetros e os critérios que você fornecer ao selecionar o melhor algoritmo para seu modelo.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 06/20/2019
ms.custom: seodec18
ms.openlocfilehash: b9fe8ff710cbfe7fbb4a4d8bd351028bb50efcb0
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67331732"
---
# <a name="what-is-automated-machine-learning"></a>O que é o aprendizado de máquina automatizado?

Automatizado aprendizado de máquina, também conhecido como autoML, é o processo de automatizar as tarefas demoradas e iterativas de desenvolvimento do modelo de aprendizado de máquina. Ele permite que os cientistas de dados, os analistas e desenvolvedores criar modelos de ML com alta escala, a eficiência e produtividade ao mesmo tempo mantendo a qualidade do modelo.

Desenvolvimento de modelos de aprendizado de máquina tradicional é intensivo de recursos, que exigem conhecimento do domínio significativa e tempo para produzir e comparar dezenas de modelos. Aplica ML automatizado quando desejar que o Azure Machine Learning para treinar e ajustar um modelo usando a métrica de destino que você especificar. O serviço, em seguida, itera por meio de algoritmos de ML emparelhados com seleções de recurso, em que cada iteração produz um modelo com uma pontuação de treinamento. Quanto maior a pontuação, melhor o modelo é considerado "ajuste" seus dados.

Com o aprendizado de máquina automatizado, você vai acelerar o tempo necessário para obter modelos de ML pronta para produção com grande facilidade e eficiência.

## <a name="when-to-use-automated-ml"></a>Quando usar ML automatizado

ML automatizado democratiza a processo de desenvolvimento do modelo de aprendizado de máquina e permite que seus usuários, independentemente de seus conhecimentos de ciência de dados, para identificar um pipeline de aprendizado de máquina de ponta a ponta para qualquer problema.

Os desenvolvedores em diversos setores, analistas e cientistas de dados podem usar ML automatizado para:

+ Implementar soluções de aprendizado de máquina sem extenso conhecimento de programação
+ Economize tempo e recursos
+ Aproveitar as práticas recomendadas de ciência de dados
+ Fornecer a solução de problemas agile

## <a name="how-automated-ml-works"></a>Funciona como automatizada de ML

Usando o **serviço do Azure Machine Learning**, você pode criar e executar seus testes de treinamento de AM automatizadas com estas etapas:

1. **Identificar o problema de ML** precisam ser resolvidos: classificação, de previsão ou regressão

1. **Especificar a origem e o formato dos dados de treinamento rotulado**: Matrizes Numpy ou dados do Pandas

1. **Configurar o destino de computação para treinamento de modelo**, como seu [computador local, Azure Machine Learning computa, VMs remotas ou do Azure Databricks](how-to-set-up-training-targets.md).  Saiba mais sobre treinamento automatizado [em um recurso remoto](how-to-auto-train-remote.md).

1. **Configurar os parâmetros de aprendizado de máquina automatizada** que determinar quantas iterações em modelos diferentes, as configurações de hiperparâmetro, avançados de pré-processamento/personalização e quais as métricas para observar ao determinar o melhor modelo.  Você pode definir as configurações de teste de treinamento automática [no portal do Azure](how-to-create-portal-experiments.md) ou [com o SDK](how-to-configure-auto-train.md).

1. **Envie o execução de treinamento.**

  ![Automatizado de Machine learning](./media/how-to-automated-ml/automl-concept-diagram2.png)

Durante o treinamento, o serviço de Azure Machine Learning cria um número de em paralelos pipelines que tente parâmetros e algoritmos diferentes. Ele será interrompido depois que ele atinja os critérios de saída definidos no teste.

Você também pode inspecionar as informações registradas em log de execução, quais [contém métricas](how-to-understand-accuracy-metrics.md) coletadas durante a execução. A execução de treinamento produz um objeto serializado do Python (`.pkl` arquivo) que contém o modelo e o pré-processamento de dados.

Enquanto a criação de modelo é automatizada, você também pode [Aprenda a usar recursos como importantes ou relevantes são](how-to-configure-auto-train.md#explain) para os modelos gerados.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Pré-processamento

Em cada computador automatizada de teste de aprendizado, seus dados é pré-processada usando os métodos padrão e, opcionalmente, por meio de avançados de pré-processamento.

### <a name="automatic-preprocessing-standard"></a>Automático (padrão) de pré-processamento

Em cada computador automatizada de teste de aprendizado, seus dados são dimensionados automaticamente ou normalizados para ajudar a algoritmos funcionam bem.  Durante o treinamento de modelo, uma das seguintes técnicas de dimensionamento ou normalização será aplicada a cada modelo.

|Dimensionando&nbsp;&&nbsp;normalização| DESCRIÇÃO |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Padronizar recursos removendo a média e expandindo a variância de unidade  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Transforma a recursos por cada recurso por essa coluna mínimo e máximo de dimensionamento  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |Dimensionar cada recurso pelo seu valor absoluto máximo |
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |Esse recurso do Scaler pelo seu intervalo de quantil |
| [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |Redução de dimensionalidade linear usando a decomposição de valor Singular dos dados para ele do projeto para um espaço dimensional inferior |
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |Este transformador executa a redução de dimensionalidade linear por meio de Decomposição de valor truncado de singular (SVD). Ao contrário do PCA, este estimador centraliza os dados antes de computar a decomposição de valor singular. Isso significa que ele pode trabalhar com matrizes scipy.sparse com eficiência |
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Cada exemplo (ou seja, cada linha da matriz de dados) pelo menos um componente diferente de zero é novamente dimensionado independentemente dos outros exemplos, para que a norma dele (l1 ou l2) é igual a um |

### <a name="advanced-preprocessing-optional-featurization"></a>Avançadas de pré-processamento: personalização opcional

O pré-processamento adicional avançadas e personalização também estão disponíveis, como ausência de imputação de valores, codificação e transformações. [Saiba mais sobre quais personalização está incluída](how-to-create-portal-experiments.md#preprocess). Habilite essa configuração com:

+ Portal do Azure: Selecionando o **Preprocess** caixa de seleção na **configurações avançadas** [com estas etapas](how-to-create-portal-experiments.md).

+ SDK do Python: Especificando `"preprocess": True` para o [ `AutoMLConfig` classe](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).


## <a name="time-series-forecasting"></a>Previsão de série temporal
Criação de previsões é parte integrante de qualquer empresa, seja por demanda de receita, estoque, vendas ou cliente. Você pode usar ML automatizado para combinar técnicas e abordagens e obter uma recomendado de alta qualidade-série de tempo de previsão. 

Um teste automatizado de série temporal é tratado como um problema de regressão multivariada. Últimos valores de série temporal são "dinâmicos" para se tornar dimensões adicionais para o regressor junto com outros indicadores. Essa abordagem, ao contrário dos métodos de série de tempo clássico, tem uma vantagem de incorporar naturalmente diversas variáveis contextuais e suas relações uns aos outros durante o treinamento. ML automatizado aprende um modelo único, mas geralmente internamente ramificado para todos os itens no horizontes conjunto de dados e previsão. Assim, há mais dados disponíveis para estimar os parâmetros de modelo e generalização a série não visto torna-se possível. 

Saiba mais e ver um exemplo de [automatizada de aprendizado de máquina para previsão de série temporal](how-to-auto-train-forecast.md).

## <a name="ensemble-models"></a>Modelos de Ensemble

Você pode treinar modelos de ensemble usando o aprendizado de máquina automatizado com o [algoritmo de seleção de ensemble Caruana com a inicialização de Ensemble classificado](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf). Aprendizado de Ensemble melhora o desempenho de previsão e resultados de aprendizado de máquina combinando vários modelos em vez de usar modelos únicos. A iteração de ensemble aparece como a última iteração de seu tempo de execução.

## <a name="use-with-onnx-in-c-apps"></a>Use com ONNX no C# aplicativos

Com o Azure Machine Learning, você pode usar ML automatizado para criar um modelo de Python e que ele seja convertido no formato ONNX. O tempo de execução ONNX dá suporte a C#, portanto, você pode usar o modelo criado automaticamente no seu C# aplicativos sem a necessidade de recodificar ou qualquer um das latências de rede que apresentam os pontos de extremidade REST. Tente um exemplo de como esse fluxo [neste bloco de anotações do Jupyter](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-with-onnx/auto-ml-classification-with-onnx.ipynb).

## <a name="automated-ml-across-microsoft"></a>ML automatizada em toda a Microsoft

ML automatizado também está disponível em outras soluções da Microsoft, como:

|Integrações|DESCRIÇÃO|
|------------|-----------|
|[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/automl-overview)|Seleção automática de modelos e treinamento em aplicativos .NET usando o Visual Studio e Visual Studio Code do ML.NET automatizadas ML (visualização).|
|[HDInsight](../../hdinsight/spark/apache-spark-run-machine-learning-automl.md)|Escalar horizontalmente seus trabalhos de treinamento de AM automatizados no Spark em clusters do HDInsight em paralelo.|
|[PowerBI](https://docs.microsoft.com/power-bi/service-machine-learning-automated)|Chame modelos do machine learning diretamente no Power BI (visualização).|
|[SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/)|Crie novos modelos de machine learning sobre seus dados em clusters do SQL Server 2019 big data.|

## <a name="next-steps"></a>Próximas etapas

Consulte exemplos e saiba como criar modelos usando o aprendizado de máquina automatizado:

+ Siga o [Tutorial: Treinar automaticamente um modelo de classificação com o Machine Learning Automatizado do Azure](tutorial-auto-train-models.md)

+ Defina as configurações de teste de treinamento automático:
  + Na interface do portal do Azure, [usar estas etapas](how-to-create-portal-experiments.md).
  + Com o SDK do Python [usar estas etapas](how-to-configure-auto-train.md).

+ Saiba como automaticamente usando dados de série temporal, de treinamento [usar estas etapas](how-to-auto-train-forecast.md).

+ Experimente [exemplos de Jupyter Notebook para aprendizado de máquina automatizado](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)
