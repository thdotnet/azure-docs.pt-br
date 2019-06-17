---
title: Seleção de algoritmo de ML e ajuste automatizado
titleSuffix: Azure Machine Learning service
description: Saiba como Serviço do Azure Machine Learning pode escolher um algoritmo para você e automaticamente gerar um modelo proveniente para economizar tempo usando os parâmetros e os critérios que você fornecer ao selecionar o melhor algoritmo para seu modelo.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 06/10/2019
ms.custom: seodec18
ms.openlocfilehash: 1dcdbbf0a2a71fa38b6eacd6a8d179cdad979937
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67059301"
---
# <a name="what-is-automated-machine-learning"></a>O que é o aprendizado de máquina automatizado?

Automatizado aprendizado de máquina, também conhecido como ML automatizado, é o processo de automatizando as tarefas demoradas e iterativas de desenvolvimento do modelo de aprendizado de máquina. Ele permite que os cientistas de dados, os analistas e desenvolvedores criar modelos de ML com alta escala, a eficiência e produtividade ao mesmo tempo mantendo a qualidade do modelo.

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

Você também pode inspecionar as informações registradas de execução, que contêm as métricas coletadas durante a execução. A execução de treinamento produz um objeto serializado do Python (`.pkl` arquivo) que contém o modelo e o pré-processamento de dados.

Enquanto a criação de modelo é automatizada, você também pode [Aprenda a usar recursos como importantes ou relevantes são](how-to-configure-auto-train.md#explain) para os modelos gerados.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="time-series-forecasting"></a>Previsão da série temporal
Criação de previsões é parte integrante de qualquer empresa, seja por demanda de receita, estoque, vendas ou cliente. Automatizada ml usa um número combinado de técnicas e abordagens para recomendar uma série de tempo de alta qualidade de previsão. Experimentos de série temporal no ml automatizado são tratados como um problema de regressão multivariada. Após a série temporal valores são "dinâmicos" para se tornar dimensões adicionais para o regressor junto com outros indicadores. 

Essa abordagem, ao contrário dos métodos de série de tempo clássico, tem uma vantagem de incorporar naturalmente diversas variáveis contextuais e suas relações uns aos outros durante o treinamento. Em aplicativos de previsão do mundo real, vários fatores podem influenciar uma previsão. Por exemplo, quando a previsão de vendas, interações de tendências históricas, taxa de câmbio e preço de unidade em conjunto o resultado de vendas. Um benefício adicional é que todas as recentes inovações em modelos de regressão se aplicam imediatamente à previsão.

Até que ponto no futuro, a previsão deve estender (o horizonte de previsão) faz parte da especificação de previsão básica. Definindo o parâmetro obrigatório do `max_horizon` no teste define quantos períodos de unidade (com base no intervalo de tempo de seus dados de treinamento, por exemplo, mensais, semanais o forecaster deve prever out. 

ML automatizado aprende um modelo único, mas geralmente internamente ramificado para todos os itens no horizontes conjunto de dados e previsão. Assim, há mais dados disponíveis para estimar os parâmetros de modelo e generalização a série não visto torna-se possível. 

Recursos extraídos dos dados de treinamento desempenham um papel fundamental. ML automatizado executa as etapas de pré-processamento padrão e gera recursos adicionais da série temporal (por exemplo, ano, mês, dia da semana etc.) para capturar efeitos sazonais e maximizar a precisão da previsão. 

Se apropriado para seu cenário, você pode direcionar ML automatizada para criar retardos (`target_lags`) ou agregação de janela rolante de dados (`target_rolling_window_size`) do seu destino (`y_value`) após valores. 

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

## <a name="ensemble-models"></a>Modelos de Ensemble

Você pode treinar modelos de ensemble usando o aprendizado de máquina automatizado com o [algoritmo de seleção de ensemble Caruana com a inicialização de Ensemble classificado](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf). Aprendizado de Ensemble melhora o desempenho de previsão e resultados de aprendizado de máquina combinando vários modelos em vez de usar modelos únicos. A iteração de ensemble aparece como a última iteração de seu tempo de execução.

## <a name="training-metric-output"></a>Saída da métrica de treinamento

Há várias maneiras de exibir as métricas de precisão de treinamento para cada iteração de execução.

* Use o widget do Jupyter.
* Use o `get_metrics()` função em qualquer `Run` objeto.
* Exiba as métricas no portal do Azure em seu experimento.

### <a name="classification-metrics"></a>Métricas de classificação

As métricas a seguir são salvas em cada iteração de execução para uma tarefa de classificação.

|Métrica|DESCRIÇÃO|Cálculo|Parâmetros adicionais
--|--|--|--|
AUC_Macro| AUC é a área embaixo da Curva característica operacional do receptor. Macro é a média aritmética do AUC para cada classe.  | [Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average="macro"|
AUC_Micro| AUC é a área embaixo da Curva característica operacional do receptor. Micro é calculado globalmente combinando os positivos verdadeiros e falsos positivos de cada classe| [Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average="micro"|
AUC_Weighted  | AUC é a área embaixo da Curva característica operacional do receptor. Ponderada é a média aritmética da pontuação para cada classe, ponderada pelo número de instâncias verdadeiras em cada classe| [Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|average="weighted"
accuracy|Accuracy é o percentual de rótulos previstos que coincidem exatamente com os rótulos verdadeiros. |[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |Nenhum|
average_precision_score_macro|A precisão média resume uma curva de recolhimento de precisão como a média ponderada de precisões atingidas em cada limite, com o aumento no recolhimento do limite anterior usado como o peso. Macro é a média aritmética da pontuação média de precisão de cada classe|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="macro"|
average_precision_score_micro|A precisão média resume uma curva de recolhimento de precisão como a média ponderada de precisões atingidas em cada limite, com o aumento no recolhimento do limite anterior usado como o peso. Micro é calculado globalmente combinando os positivos verdadeiros e falsos positivos em cada corte|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="micro"|
average_precision_score_weighted|A precisão média resume uma curva de recolhimento de precisão como a média ponderada de precisões atingidas em cada limite, com o aumento no recolhimento do limite anterior usado como o peso. Ponderada é a média aritmética da pontuação de precisão média para cada classe, ponderada pelo número de instâncias verdadeiras em cada classe|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="weighted"|
balanced_accuracy|Precisão equilibrada é a média aritmética do recolhimento de cada classe.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="macro"|
f1_score_macro|Pontuação F1 é a média harmônica de precisão e recuperação. Macro é a média aritmética da pontuação F1 cada classe|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="macro"|
f1_score_micro|Pontuação F1 é a média harmônica de precisão e recuperação. Micro é computado globalmente contando o total de positivos verdadeiros, falsos negativos e falsos positivos|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="micro"|
f1_score_weighted|Pontuação F1 é a média harmônica de precisão e recuperação. Média ponderada por frequência de classe ou pontuação F1 para cada classe|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="weighted"|
log_loss|Essa é a função de perda usada na regressão logística (multinomial) e nas extensões dela como redes neurais, definidas como a probabilidade logarítmica negativa dos rótulos verdadeiros dadas as previsões de um classificador probabilístico. Para obter um único exemplo com rótulo verdadeiro yt no {0,1} e a probabilidade estimada yp que yt = 1, a perda logarítmica é -log P(yt&#124;yp) = -(yt log(yp) + (1 - yt) log(1 - yp))|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|Nenhum|
norm_macro_recall|O Recolhimento de macro normalizado é o recolhimento de macro normalizado para que o desempenho aleatório tenha uma pontuação de 0 e o desempenho perfeito tenha uma pontuação de 1. Isso é obtido por norm_macro_recall := (recall_score_macro - R)/(1 - R), em que R é o valor esperado de recall_score_macro para previsões aleatórias (i.e., R=0,5 para classificação binária e R=(1/C) para problemas de classificação da classe C)|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average = "macro" e (recall_score_macro - R)/(1 - R), em que R é o valor esperado de recall_score_macro para previsões aleatórias (i.e., R=0,5 para classificação binária e R=(1/C) para problemas de classificação da classe C)|
precision_score_macro|A precisão é o percentual de elementos rotulados como uma determinada classe que realmente estão nessa classe. Macro é a média aritmética de precisão para cada classe|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="macro"|
precision_score_micro|A precisão é o percentual de elementos rotulados como uma determinada classe que realmente estão nessa classe. Micro é calculado globalmente contando o total de positivos verdadeiros e falsos positivos|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="micro"|
precision_score_weighted|A precisão é o percentual de elementos rotulados como uma determinada classe que realmente estão nessa classe. Weighted é a média aritmética da precisão para cada classe, ponderada pelo número de instâncias verdadeiras em cada classe|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="weighted"|
recall_score_macro|O recolhimento é o percentual de elementos que realmente estão em uma determinada classe que são rotulados corretamente. Macro é a média aritmética de recolhimento para cada classe|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="macro"|
recall_score_micro|O recolhimento é o percentual de elementos que realmente estão em uma determinada classe que são rotulados corretamente. Micro é computado globalmente contando o total de positivos verdadeiros e falsos negativos|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="micro"|
recall_score_weighted|O recolhimento é o percentual de elementos que realmente estão em uma determinada classe que são rotulados corretamente. Weighted é a média aritmética de recolhimento para cada classe, ponderada pelo número de instâncias verdadeiras em cada classe|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="weighted"|
weighted_accuracy|Precisão ponderada é a precisão em que o peso dado a cada exemplo é igual à proporção de instâncias verdadeiras na classe verdadeira desse exemplo|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|sample_weight é um vetor igual à proporção dessa classe para cada elemento no destino|

### <a name="regression-and-forecasting-metrics"></a>Regressão e métricas de previsão

As métricas a seguir são salvas em cada iteração de execução para uma tarefa de previsão ou de regressão.

|Métrica|DESCRIÇÃO|Cálculo|Parâmetros adicionais
--|--|--|--|
explained_variance|A variação explicada é a proporção na qual um modelo matemático responde pela variação de determinado conjunto de dados. É o percentual de redução na variação dos dados originais para a variação dos erros. Quando a média dos erros é 0, ela é igual à variação explicada.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|Nenhum|
r2_score|R2 é o coeficiente de determinação ou a redução de percentual em erros quadráticos, comparado a um modelo de linha de base que gera a média. Quando a média dos erros é 0, ela é igual à variação explicada.|[Cálculo](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|Nenhum|
spearman_correlation|Correlação de Spearman é uma medida não paramétrica da monotonicidade da relação entre dois conjuntos de dados. Diferentemente da correlação de Pearson, a correlação de Spearman não supõe que os dois conjuntos de dados estão distribuídos normalmente. Como outros coeficientes de correlação, esse varia entre -1 e +1 com 0 implicando nenhuma correlação. As correlações de -1 ou +1 implicam uma relação monotônica exata. Correlações positivas implicam que, à medida que x aumenta, y também. Correlações negativas implicam que, à medida que x aumenta, y diminui.|[Cálculo](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|Nenhum|
mean_absolute_error|Erro médio absoluto é o valor esperado do valor absoluto da diferença entre a meta e a previsão|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Nenhum|
normalized_mean_absolute_error|Erro médio absoluto normalizado é o Erro médio absoluto dividido pelo intervalo dos dados|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Dividir pelo intervalo dos dados|
median_absolute_error|O erro absoluto mediano é a mediana de todas as diferenças absolutas entre a meta e a previsão. Essa perda é robusta para exceções.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Nenhum|
normalized_median_absolute_error|Erro absoluto mediano normalizado é o Erro absoluto mediano dividido pelo intervalo dos dados|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Dividir pelo intervalo dos dados|
root_mean_squared_error|A raiz do erro quadrático médio é a raiz quadrada da diferença esperada ao quadrado entre a média e a previsão|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Nenhum|
normalized_root_mean_squared_error|A raiz do erro quadrático médio normalizado é a raiz do erro quadrático médio dividida pelo intervalo dos dados|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Dividir pelo intervalo dos dados|
root_mean_squared_log_error|A raiz do erro de log quadrático médio é a raiz quadrada do erro logarítmico quadrático esperado|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Nenhum|
normalized_root_mean_squared_log_error|A raiz do erro de log quadrático médio normalizada é a raiz do erro de log quadrático médio dividida pelo intervalo dos dados|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Dividir pelo intervalo dos dados|


## <a name="use-with-onnx-in-c-apps"></a>Use com ONNX no C# aplicativos

Com o Azure Machine Learning, você pode usar ML automatizado para criar um modelo de Python e que ele seja convertido no formato ONNX. O tempo de execução ONNX dá suporte a C#, portanto, você pode usar o modelo criado automaticamente no seu C# aplicativos sem a necessidade de recodificar ou qualquer um das latências de rede que apresentam os pontos de extremidade REST. Tente um exemplo de como esse fluxo [neste bloco de anotações do Jupyter](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-with-onnx/auto-ml-classification-with-onnx.ipynb).

## <a name="automated-ml-across-microsoft"></a>ML automatizada em toda a Microsoft

ML automatizado também está disponível em outras soluções da Microsoft, como:

|Integrações|DESCRIÇÃO|
|------------|-----------|
|[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/automl-overview)|Seleção automática de modelos e treinamento em aplicativos .NET usando o Visual Studio e Visual Studio Code do ML.NET automatizadas ML (visualização).|
|[HDIsnight](../../hdinsight/spark/apache-spark-run-machine-learning-automl.md)|Escalar horizontalmente seus trabalhos de treinamento de AM automatizados no Spark em clusters do HDInsight em paralelo.|
|[PowerBI](https://docs.microsoft.com/power-bi/service-machine-learning-automated)|Chame modelos do machine learning diretamente no Power BI (visualização).|
|[SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/)|Crie novos modelos de machine learning sobre seus dados em clusters do SQL Server 2019 big data.|

## <a name="next-steps"></a>Próximas etapas

Veja exemplos e saiba como criar modelos usando o aprendizado de máquina automatizado:

+ Siga o [Tutorial: Treinar automaticamente um modelo de classificação com o Machine Learning Automatizado do Azure](tutorial-auto-train-models.md)

+ Defina as configurações de teste de treinamento automático:
  + Na interface do portal do Azure, [usar estas etapas](how-to-create-portal-experiments.md).
  + Com o SDK do Python [usar estas etapas](how-to-configure-auto-train.md).

+ Saiba como automaticamente usando dados de série temporal, de treinamento [usar estas etapas](how-to-auto-train-forecast.md).

+ Experimente [exemplos de Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)
