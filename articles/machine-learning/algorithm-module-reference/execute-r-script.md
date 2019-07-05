---
title: 'Execute Script R: Referência de módulo'
titleSuffix: Azure Machine Learning service
description: Saiba como usar o módulo Executar Script R no serviço de Azure Machine Learning para executar código R.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: peterclu
ms.date: 06/01/2019
ROBOTS: NOINDEX
ms.openlocfilehash: bfddcd3db4825ea1875474aa16544aa15412bdea
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67518047"
---
# <a name="execute-r-script"></a>Executar script R

Este artigo descreve como usar o **Executar Script R** módulo executar código R em seu teste de interface visual.

Com o R, você pode executar tarefas que não são atualmente suportadas por módulos existentes, como: 
- Criar transformações de dados personalizadas
- Use suas próprias métricas para avaliar previsões
- Criar modelos usando algoritmos que não são implementados como módulos independentes no interface visual

## <a name="r-version-support"></a>Suporte de versão do R

A interface visual do serviço de Azure Machine Learning usa a CRAN (rede de arquivamento abrangente R) distribuição de R. A versão usada atualmente é CRAN 3.5.1.

## <a name="supported-r-packages"></a>Pacotes de R com suporte

O ambiente de R é pré-instalado com mais de 100 pacotes. Para obter uma lista completa, consulte a seção [pacotes de R pré-instaladas](#pre-installed-r-packages).

Você também pode adicionar o código a seguir para qualquer **Executar Script R** módulo e ver os pacotes instalados.

```R
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  dataframe1 <- data.frame(installed.packages())
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
```

## <a name="installing-r-packages"></a>Instalando pacotes R
Para instalar pacotes R adicionais, use o `install.packages()` método. Certifique-se de especificar o repositório CRAN. Pacotes são instalados para cada **Executar Script R** módulo e não são compartilhadas entre si **Executar Script R** módulos.

Este exemplo mostra como instalar Zoo:
```R
# R version: 3.5.1
# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a R DataFrame
#   Param<dataframe2>: a R DataFrame
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  
  if(!require(zoo)) install.packages("zoo",repos = "http://cran.us.r-project.org")
  library(zoo)
  # Return datasets as a Named List
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
```

## <a name="how-to-configure-execute-r-script"></a>Como configurar o executar Script R

O **Executar Script R** módulo contém o código de exemplo que você pode usar como ponto de partida. Para configurar o **Executar Script R** módulo, fornecem um conjunto de entradas e o código seja executado.

![Módulo de R](media/module/execute-r-script.png)

Conjuntos de dados armazenados na interface visual são convertidos automaticamente em um quadro de dados R quando carregado com este módulo.

1.  Adicione a **Executar Script R** módulo ao seu experimento.

    > [!NOTE]
    > Todos os dados passados para o **Executar Script R** módulo é convertido para o R `data.frame` formato.

1. Conecte-se qualquer entrada necessária para o script. Entradas são opcionais e podem incluir dados e código de R adicional.

    * **DataSet1**: Fazer referência a primeira entrada como `dataframe1`. O conjunto de dados de entrada deve ser formatado como um CSV, TSV, ARFF ou você pode se conectar a um conjunto de dados do Azure Machine Learning.

    * **Dataset2**: Fazer referência a segunda entrada como `dataframe2`. Esse conjunto de dados também deve ser formatado como um CSV, TSV, o arquivo ARFF, ou como um conjunto de dados do Azure Machine Learning.

    * **Pacote de script**: A terceira entrada aceita arquivos ZIP. O arquivo compactado pode conter vários arquivos e vários tipos de arquivo.

1. No **script R** caixa de texto, digite ou cole o script de R válido.

    Para ajudá-lo a começar a usar, o **Script R** caixa de texto é preenchida previamente com o código de exemplo, o que você pode editar ou substituir.

    * O script deve conter uma função chamada `azureml_main`, que é o ponto de entrada para esse módulo.

    * A função de ponto de entrada pode conter até dois argumentos de entrada: `Param<dataframe1>` e `Param<dataframe2>`
 
    > [!NOTE]
    >  Código R existente, talvez seja necessário pequenas alterações para ser executado em um teste de interface visual. Por exemplo, dados de entrada que você fornece no formato CSV devem ser explicitamente convertidos para um conjunto de dados antes de você pode usá-lo em seu código. Tipos de dados e a coluna usados no idioma R também diferem de algumas maneiras dos tipos de dados e a coluna usados na interface do visual.

1.  **Propagação aleatória**: Digite um valor a ser usado dentro do ambiente de R como o valor de semente aleatória. Esse parâmetro é equivalente a chamar `set.seed(value)` no código R.  

1. Execute o experimento.  

## <a name="results"></a>Resultados

O **Executar Script R** módulos podem retornar várias saídas, mas elas devem ser fornecidas como quadros de dados de R. Quadros de dados são convertidos automaticamente para conjuntos de dados de interface visual para compatibilidade com outros módulos.

Mensagens padrão e erros de R são retornados para o log do módulo.

## <a name="sample-scripts"></a>Scripts de exemplo

Há várias maneiras que você pode estender seu experimento usando script R personalizado.  Esta seção fornece código de exemplo para tarefas comuns.


### <a name="add-r-script-as-an-input"></a>Adicionar script R como uma entrada

O **Executar Script R** módulo oferece suporte a arquivos de script do R arbitrários como entradas. Para fazer isso, eles devem ser carregados para o seu espaço de trabalho como parte do arquivo ZIP.

1. Para carregar um arquivo ZIP que contém o código R para seu espaço de trabalho, clique em **New**, clique em **Dataset**e, em seguida, selecione **de arquivo local** e o **arquivo Zip**opção.  

1. Verifique se o arquivo compactado está disponível na **conjuntos de dados salvos** lista.

1.  Conectar-se o conjunto de dados para o **pacote de Script** porta de entrada.

1. Todos os arquivos que estão contidos no arquivo ZIP que estão disponíveis durante o tempo de execução do experimento. 

    Se o arquivo de pacote de script contiver uma estrutura de diretório, a estrutura é preservada. No entanto, você deve alterar seu código para preceder o diretório **. / Script de pacote** ao caminho.

### <a name="process-data"></a>Processar dados

O exemplo a seguir mostra como dimensionar e normalizar dados de entrada:

```R
# R version: 3.5.1
# The script MUST contain a function named azureml_main
# which is the entry point for this module.
# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a R DataFrame
#   Param<dataframe2>: a R DataFrame
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  # If a zip file is connected to the third input port, it is
  # unzipped under "./Script Bundle". This directory is added
  # to sys.path.
  series <- dataframe1$width
  # find the maximum and minimum values of width column in dataframe1
  max_v <- max(series)
  min_v <- min(series)
  # calculate the scale and bias
  scale <- max_v - min_v
  bias <- min_v / dis
  # apply min-max normalizing
  dataframe1$width <- dataframe1$width / scale - bias
  dataframe2$width <- dataframe2$width / scale - bias
  # Return datasets as a Named List
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
 ```

### <a name="read-a-zip-file-as-input"></a>Ler um arquivo ZIP como entrada

Este exemplo mostra como usar um conjunto de dados em um arquivo ZIP como entrada para o **Executar Script R** módulo.

1. Criar o arquivo de dados no formato CSV e nomeie-a como "mydatafile".
1. Crie um arquivo ZIP e adicione o arquivo CSV para o arquivo morto.
1. Carregue o arquivo compactado ao seu espaço de trabalho do Azure Machine Learning. 
1. Conectar-se o conjunto de dados resultante para o **ScriptBundle** entrada de seu **Executar Script R** módulo.
1. Usando o código a seguir para ler os dados CSV do arquivo compactado.

```R
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  mydataset<-read.csv("./Script Bundle/mydatafile.csv",encoding="UTF-8");  
  # Return datasets as a Named List
  return(list(dataset1=mydataset, dataset2=dataframe2))
}
```

### <a name="replicate-rows"></a>Replicar linhas

Este exemplo mostra como replicar registros positivos em um conjunto de dados para equilibrar o exemplo:

```R
azureml_main <- function(dataframe1, dataframe2){
  data.set <- dataframe1[dataframe1[,1]==-1,]  
  # positions of the positive samples
  pos <- dataframe1[dataframe1[,1]==1,]
  # replicate the positive samples to balance the sample  
  for (i in 1:20) data.set <- rbind(data.set,pos)  
  row.names(data.set) <- NULL
  # Return datasets as a Named List
  return(list(dataset1=data.set, dataset2=dataframe2))
}
```

### <a name="pass-r-objects-between-execute-r-script-modules"></a>Passar objetos R entre módulos de executar Script R

Você pode passar objetos R entre instâncias do **Executar Script R** módulo usando o mecanismo de serialização interna. Este exemplo supõe que você deseja mover o objeto de R chamado `A` entre dois **Executar Script R** módulos.

1. Adicionar a primeira **Executar Script R** módulo para o experimento e digite o seguinte código na **Script R** caixa de texto para criar um objeto serializado `A` como uma coluna no módulo de saída do tabela de dados:  
  
    ```R
    azureml_main <- function(dataframe1, dataframe2){
      print("R script run.")
      # some codes generated A
      
      serialized <- as.integer(serialize(A,NULL))  
      data.set <- data.frame(serialized,stringsAsFactors=FALSE)

      return(list(dataset1=data.set, dataset2=dataframe2))
    }
    ```

    A conversão explícita para o tipo de inteiro é feita porque a função de serialização gera dados no R `Raw` formato, o que não é compatível com a interface visual.

1. Adicionar uma segunda instância das **Executar Script R** módulo e conecte-se à porta de saída do módulo anterior.

1. Digite o seguinte código na **Script R** caixa de texto para extrair o objeto `A` da tabela de dados de entrada. 

    ```R
    azureml_main <- function(dataframe1, dataframe2){
      print("R script run.")
      A <- unserialize(as.raw(dataframe1$serialized))  
      # Return datasets as a Named List
      return(list(dataset1=dataframe1, dataset2=dataframe2))
    }
    ```

## <a name="pre-installed-r-packages"></a>Pacotes de R pré-instalados

A lista atual de pacotes de R pré-instalados disponíveis para uso:

|              |            | 
|--------------|------------| 
| Pacote      | Versão    | 
| askpass      | 1,1        | 
| assertthat   | 0.2.1      | 
| backports    | 1.1.4      | 
| Base de dados de         | 3.5.1      | 
| base64enc    | 0.1-3      | 
| BH           | 1.69.0-1   | 
| bindr        | 0.1.1      | 
| bindrcpp     | 0.2.2      | 
| bitops       | 1.0-6      | 
| ciar         | 1.3-22     | 
| Vassoura        | 0.5.2      | 
| callr        | 3.2.0      | 
| Sinal de interpolação        | 6.0-84     | 
| caTools      | 1.17.1.2   | 
| cellranger   | 1.1.0      | 
| class        | 7.3-15     | 
| cli          | 1.1.0      | 
| mídia        | 0.6.0      | 
| cluster      | 2.0.7-1    | 
| codetools    | 0.2-16     | 
| espaço de cor   | 1.4-1      | 
| compilador     | 3.5.1      | 
| Lápis       | 1.3.4      | 
| curl         | 3.3        | 
| data.table   | 1.12.2     | 
| datasets     | 3.5.1      | 
| DBI          | 1.0.0      | 
| dbplyr       | 1.4.1      | 
| digest       | 0.6.19     | 
| dplyr        | 0.7.6      | 
| e1071        | 1.7-2      | 
| avaliar     | 0,14       | 
| fansi        | 0.4.0      | 
| forcats      | 0.3.0      | 
| foreach      | 1.4.4      | 
| externa      | 0.8-71     | 
| fs           | 1.3.1      | 
| gdata        | 2.18.0     | 
| Genéricos     | 0.0.2      | 
| ggplot2      | 3.2.0      | 
| glmnet       | 2.0-18     | 
| Cola         | 1.3.1      | 
| gower        | 0.2.1      | 
| gplots       | 3.0.1.1    | 
| Elementos gráficos     | 3.5.1      | 
| grDevices    | 3.5.1      | 
| Grade         | 3.5.1      | 
| gtable       | 0.3.0      | 
| gtools       | 3.8.1      | 
| refúgio        | 2.1.0      | 
| highr        | 0.8        | 
| hms          | 0.4.2      | 
| htmltools    | 0.3.6      | 
| httr         | 1.4.0      | 
| ipred        | 0.9-9      | 
| Iteradores    | 1.0.10     | 
| jsonlite     | 1.6        | 
| KernSmooth   | 2.23-15    | 
| knitr        | 1.23       | 
| Rotulação     | 0.3        | 
| malha      | 0.20-38    | 
| lava         | 1.6.5      | 
| lazyeval     | 0.2.2      | 
| lubridate    | 1.7.4      | 
| magrittr     | 1.5        | 
| Markdown     | 1          | 
| MASS         | 7.3-51.4   | 
| matriz       | 1.2-17     | 
| Métodos      | 3.5.1      | 
| mgcv         | 1.8-28     | 
| MIME         | 0.7        | 
| ModelMetrics | 1.2.2      | 
| modelr       | 0.1.4      | 
| munsell      | 0.5.0      | 
| nlme         | 3.1-140    | 
| nnet         | 7.3-12     | 
| numDeriv     | 2016.8-1.1 | 
| openssl      | 1.4        | 
| Paralelo     | 3.5.1      | 
| Pilar       | 1.4.1      | 
| pkgconfig    | 2.0.2      | 
| plogr        | 0.2.0      | 
| plyr         | 1.8.4      | 
| prettyunits  | 1.0.2      | 
| processx     | 3.3.1      | 
| prodlim      | 2018.04.18 | 
| progresso     | 1.2.2      | 
| ps           | 1.3.0      | 
| purrr        | 0.3.2      | 
| quadprog     | 1.5-7      | 
| quantmod     | 0.4-15     | 
| R6           | 2.4.0      | 
| randomForest | 4.6-14     | 
| RColorBrewer | 1.1-2      | 
| Rcpp         | 1.0.1      | 
| RcppRoll     | 0.3.0      | 
| readr        | 1.3.1      | 
| readxl       | 1.3.1      | 
| receitas      | 0.1.5      | 
| rematch      | 1.0.1      | 
| reprex       | 0.3.0      | 
| reshape2     | 1.4.3      | 
| reticulate   | 1.12       | 
| rlang        | 0.4.0      | 
| rmarkdown    | 1.13       | 
| ROCR         | 1.0-7      | 
| rpart        | 4.1-15     | 
| rstudioapi   | 0,1        | 
| rvest        | 0.3.4      | 
| pode ser dimensionado       | 1.0.0      | 
| selectr      | 0.4-1      | 
| Espacial      | 7.3-11     | 
| splines      | 3.5.1      | 
| SQUAREM      | 2017.10-1  | 
| stats        | 3.5.1      | 
| stats4       | 3.5.1      | 
| stringi      | 1.4.3      | 
| stringr      | 1.3.1      | 
| sobrevivência     | 2.44-1.1   | 
| sys          | 3.2        | 
| tcltk        | 3.5.1      | 
| tibble       | 2.1.3      | 
| tidyr        | 0.8.3      | 
| tidyselect   | 0.2.5      | 
| tidyverse    | 1.2.1      | 
| timeDate     | 3043.102   | 
| tinytex      | 0.13       | 
| ferramentas        | 3.5.1      | 
| tseries      | 0.10-47    | 
| TTR          | 0.23-4     | 
| utf8         | 1.1.4      | 
| Utils        | 3.5.1      | 
| vctrs        | 0.1.0      | 
| viridisLite  | 0.3.0      | 
| caixa estreita      | 0.3-2      | 
| withr        | 2.1.2      | 
| xfun         | 0.8        | 
| xml2         | 1.2.0      | 
| xts          | 0.11-2     | 
| yaml         | 2.2.0      | 
| zeallot      | 0.1.0      | 
| zoológico          | 1.8-6      | 

## <a name="next-steps"></a>Próximas etapas

Consulte a [conjunto de módulos disponíveis](module-reference.md) ao serviço de Azure Machine Learning. 