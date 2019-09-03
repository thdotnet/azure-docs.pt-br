---
title: Introdução à Máquina Virtual de Ciência de Dados do Azure para Linux e Windows | Microsoft Docs
description: Principais cenários de análise e componentes para Máquinas Virtuais de Ciência de Dados do Windows e Linux.
keywords: ferramentas de ciência de dados, máquina virtual de ciência de dados, ferramentas para ciência de dados, ciência de dados do linux
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.assetid: d4f91270-dbd2-4290-ab2b-b7bfad0b2703
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/22/2019
ms.author: vijetaj
ms.openlocfilehash: 22192d67a946f9f466848b65b9cd763b16e6c5f0
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70099378"
---
# <a name="what-is-the-azure-data-science-virtual-machine-for-linux-and-windows"></a>O que é a Máquina Virtual de Ciência de Dados do Azure para Linux e Windows?

A DSVM (Máquina Virtual de Ciência de Dados) é uma imagem de VM personalizada plataforma de nuvem do Azure especificamente criada para ciência de dados. Ela tem muitas ferramentas conhecidas de ciência de dados, entre outras, pré-instaladas e pré-configuradas que ajudam a começar a criar rapidamente aplicativos inteligentes para análise avançada. 

As configurações de ferramenta são rigorosamente testadas por cientistas de dados e desenvolvedores da Microsoft e pela comunidade científica de dados mais ampla. Esse teste ajuda a garantir a estabilidade e a viabilidade geral.

O DSVM está disponível em:
+ Windows Server 2016, Windows Server 2012
+ Ubuntu 16.04 LTS e CentOS 7.4

> [!NOTE]
> Todas as ferramentas de VM para aprendizado profundo foram incluídas na Máquina Virtual de Ciência de Dados. 


## <a name="what-can-i-do-with-the-dsvm"></a>O que posso fazer com a DSVM?
A meta da Máquina Virtual de Ciência de Dados é fornecer a profissionais de dados com todos os níveis de habilidade em todos os setores um ambiente de ciência de dados descomplicado, pré-configurado e totalmente integrado. Em vez de distribuir um workspace comparável por conta própria, você pode provisionar uma DSVM. Essa escolha pode economizar dias ou até mesmo _semanas_ necessárias para os processos de instalação, configuração e gerenciamento de pacotes. Depois de alocar o DSVM, você pode começar a trabalhar imediatamente no seu projeto de ciência de dados.

A DSVM foi desenvolvida e configurada para trabalhar com amplos cenários de uso. Você pode reduzir ou aumentar o ambiente de acordo com as necessidades do seu projeto. Você também pode usar sua linguagem preferida para programar tarefas de ciência de dados e instalar outras ferramentas para personalizar o sistema de acordo com suas necessidades.

### <a name="preconfigured-analytics-desktop-in-the-cloud"></a>Área de trabalho de análise pré-configurada na nuvem
A DSVM fornece uma configuração de linha de base para equipes de ciência de dados que buscam substituir as respectivas áreas de trabalho locais por uma área de trabalho de nuvem gerenciada. Essa linha de base garante que todos os cientistas de dados em uma equipe tenham uma configuração consistente com a qual podem verificar experiências e promover colaboração. Ela também reduz os custos, reduzindo a carga de sysadmin. Essa redução no fardo poupa o tempo necessário para avaliar, instalar e manter pacotes de software para análise avançada.

### <a name="data-science-training-and-education"></a>Educação e treinamento de ciência de dados
Os treinadores corporativos e educadores que dão aulas de ciência de dados geralmente fornecem uma imagem de máquina virtual. A imagem garante que os alunos tenham uma configuração consistente e que as amostras funcionem de maneira previsível. 

A DSVM cria um ambiente sob demanda com uma configuração consistente que facilita o suporte e os desafios de incompatibilidade. Nos casos em que esses ambientes precisam ser criados com frequência, especialmente para aulas rápidas de treinamento, os alunos são consideravelmente beneficiados.

### <a name="on-demand-elastic-capacity-for-large-scale-projects"></a>Capacidade elástica sob demanda para projetos em larga escala
As maratonas/competições de ciência de dados ou modelagem e exploração de dados em larga escala exigem que a capacidade de hardware seja escalada horizontalmente, geralmente por um curto período. A DSVM pode ajudar a replicar o ambiente de ciência de dados rapidamente em servidores escalados horizontalmente sob demanda, o que permite a realização de experimentos que recursos de computação altamente potentes podem executar.

### <a name="custom-compute-power-for-azure-notebooks"></a>O poder da computação personalizada para o Azure Notebooks
O [Azure Notebooks](../../notebooks/azure-notebooks-overview.md) é um serviço hospedado gratuito para desenvolver, executar e compartilhar os notebooks do Jupyter na nuvem sem instalação. A camada de serviço gratuita está limitada a 4 GB de memória e 1 GB de dados. 

Para liberar todos os limites, você pode anexar um projeto do Notebooks a uma DSVM ou a qualquer outra VM que esteja em execução em um servidor Jupyter. Se você entrar no Azure Notebooks com uma conta usando o Azure Active Directory (como uma conta corporativa), os Notebooks exibirão automaticamente as DSVMs em todas as assinaturas associadas a essa conta. Você pode [anexar uma DSVM ao Azure Notebooks](../../notebooks/configure-manage-azure-notebooks-projects.md#compute-tier) para expandir o poder de computação disponível.

### <a name="short-term-experimentation-and-evaluation"></a>Avaliação e experimento de curto prazo
Você pode usar a DSVM para avaliar ou aprender a usar ferramentas como essas, com esforço mínimo de configuração:

- Microsoft Machine Learning Server
- SQL Server
- Ferramentas do Visual Studio
- Jupyter
- Kits de ferramentas de aprendizado profundo e aprendizado de máquina
- Novas ferramentas populares na comunidade 

Como pode configurar a DSVM rapidamente, você pode aplicá-la a outros cenários de uso de curto prazo. Esses cenários incluem replicar testes publicados, executar demonstrações e seguir as instruções passo a passo em sessões online e tutoriais de conferência.

### <a name="deep-learning"></a>Aprendizado
Na DSVM, seus modelos de treinamento podem usar algoritmos de aprendizado profundo em hardware baseado em GPUs (unidades de processamento gráfico). Aproveitando os recursos de dimensionamento de VM da plataforma Azure, a DSVM ajuda a usar o hardware baseado em GPU na nuvem de acordo com suas necessidades. É possível mudar para uma VM baseada em GPU durante o treinamento de modelos grandes ou quando houver necessidade de cálculos em alta velocidade mantendo o mesmo disco do SO.  

A edição do Windows Server 2016 da DSVM vem pré-instalada com drivers de GPU, estruturas e versões de GPU de estruturas de aprendizado profundo. Na edição para Linux, o aprendizado profundo em GPUs está habilitada nos DSVMs CentOS e Ubuntu. 

Você também pode implantar a edição para Ubuntu, CentOS ou Windows 2016 da DSVM em uma máquina virtual do Azure não baseada em GPUs. Nesse caso, todas as estruturas de aprendizado profundo farão o fallback para o modo de CPU.
 
[Saiba mais sobre o aprendizado profundo e as estruturas de IA disponíveis](dsvm-deep-learning-ai-frameworks.md).

<a name="included"></a>

## <a name="whats-included-on-the-dsvm"></a>O que está incluso na DSVM?
A Máquina Virtual de Ciência de Dados tem muitas ferramentas conhecidas de ciência de dados e de aprendizado já instaladas e configuradas. Ela também inclui ferramentas que facilitam o trabalho com vários produtos de análise e dados do Azure. Esses produtos incluem o Microsoft Machine Learning Server (R, Python) para criar modelos de previsão e o SQL Server 2017 para a exploração em larga escala de conjuntos de dados. A DSVM inclui outras ferramentas da comunidade open-source e da Microsoft, bem como [código de exemplo e notebooks](dsvm-samples-and-walkthroughs.md). 

Aqui está uma lista das ferramentas e plataformas:
+ [Linguagens de programação compatíveis](dsvm-languages.md)

+ [Plataformas de dados com compatíveis](dsvm-data-platforms.md)

+ [Ferramentas de desenvolvimento e IDEs](dsvm-tools-development.md)

+ [Estruturas de aprendizado profundo e de IA](dsvm-deep-learning-ai-frameworks.md)

+ [Ferramentas de aprendizado de máquina e ciência de dados](dsvm-ml-data-science-tools.md)

+ [Ferramentas de ingestão de dados](dsvm-tools-ingestion.md)

+ [Ferramentas de visualização e exploração de dados](dsvm-tools-explore-and-visualize.md)

A tabela a seguir relaciona e compara os principais componentes incluídos nas edições do Windows e Linux da Máquina Virtual de Ciência de Dados.

| **Ferramenta**                                                           | **Edição do Windows** | **Edição do Linux** |
| :------------------------------------------------------------------ |:-------------------:|:------------------:|
| [Microsoft R Open](https://mran.microsoft.com/open/) com pacotes populares pré-instalados   |S                      | S             |
| O [Microsoft Machine Learning Server (R, Python)](https://docs.microsoft.com/machine-learning-server/) Developer Edition inclui: <br />  estrutura de alto desempenho paralela e distribuída &nbsp;&nbsp;&nbsp;&nbsp; [RevoScaleR/revoscalepy](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-revoscaler) (R e Python)<br />  &nbsp;&nbsp;&nbsp;&nbsp; [MicrosoftML](https://docs.microsoft.com/machine-learning-server/r/concept-what-is-the-microsoftml-package), novos algoritmos de aprendizado de máquina de última geração da Microsoft <br />  &nbsp;&nbsp;&nbsp;&nbsp; [Operacionalização de R e Python](https://docs.microsoft.com/machine-learning-server/what-is-operationalization)                                            |S                      | S |
| [Microsoft Office](https://products.office.com/business/office-365-proplus-business-software) ProPlus com ativação compartilhada: Excel, Word e PowerPoint   |S                      |N              |
| [Anaconda Python](https://www.continuum.io/) 2.7 e 3.5 com pacotes populares pré-instalados    |S                      |S              |
| [JuliaPro](https://juliacomputing.com/products/juliapro.html) com pacotes populares para linguagem Julia pré-instalados                         |S                      |S              |
| Bancos de dados relacionais                                                            | [SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition| [PostgreSQL](https://www.postgresql.org/) (CentOS),<br/>[SQL Server 2017](https://www.microsoft.com/sql-server/sql-server-2017) <br/> Developer Edition (Ubuntu) |
| Ferramentas de Banco de Dados                                                       |  SQL Server Management Studio <br/> SQL Server Integration Services<br/> [bcp, sqlcmd](https://docs.microsoft.com/sql/tools/command-prompt-utility-reference-database-engine)<br />  Drivers ODBC/JDBC|  [SQuirreL SQL](http://squirrel-sql.sourceforge.net/) (ferramenta de consultas), <br />  bcp, sqlcmd <br />  Drivers ODBC/JDBC|
| Análise no banco de dados escalonável com os serviços de aprendizado de máquina do SQL Server (R, Python) | S     |N              |
| [Servidor do Jupyter Notebook](https://jupyter.org/) com os kernels a seguir:                                  | S     | S |
|     &nbsp;&nbsp;&nbsp;&nbsp; R | S | S |
|     &nbsp;&nbsp;&nbsp;&nbsp; Python | S | S |
|     &nbsp;&nbsp;&nbsp;&nbsp; Julia | S | S |
|     &nbsp;&nbsp;&nbsp;&nbsp; PySpark | S | S |
|     &nbsp;&nbsp;&nbsp;&nbsp; [Sparkmagic](https://github.com/jupyter-incubator/sparkmagic) | N | Y (somente Ubuntu) |
|     &nbsp;&nbsp;&nbsp;&nbsp; SparkR     | N | S |
| JupyterHub (servidor de notebooks multiusuário)| N | S |
| JupyterLab (servidor de notebooks multiusuário) | N | Y (somente Ubuntu) |
| Ferramentas de desenvolvimento, IDEs e editores de código:| | |
| &nbsp;&nbsp;&nbsp;&nbsp; [Visual Studio 2019 (Community Edition)](https://www.visualstudio.com/community/) com plug-in do Git, Azure HDInsight (Hadoop), Azure Data Lake, SQL Server Data Tools, [Node.js](https://github.com/Microsoft/nodejstools), [Python](https://aka.ms/ptvs) e [RTVS (Ferramentas do R para Visual Studio)](https://microsoft.github.io/RTVS-docs/) | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp; [Visual Studio Code](https://code.visualstudio.com/) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [RStudio Server](https://www.rstudio.com/products/rstudio/#Server) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [PyCharm Community Edition](https://www.jetbrains.com/pycharm/) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Atom](https://atom.io/) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Juno (IDE de Julia)](https://junolab.org/)| S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; Vim e Emacs | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; Git e Git Bash | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; OpenJDK | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; .NET Framework | S | N |
| Power BI Desktop | S | N |
| SDKs para acessar o Azure e o pacote de serviços Cortana Intelligence | S | S |
| Ferramentas de movimentação de dados e gerenciamento: | | |
| &nbsp;&nbsp;&nbsp;&nbsp; Gerenciador de Armazenamento do Azure | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [CLI do Azure](https://docs.microsoft.com/cli/azure) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; Azure PowerShell | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp; [Azcopy](https://docs.microsoft.com/azure/storage/storage-use-azcopy) | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp; [Driver FUSE do Blob](https://github.com/Azure/azure-storage-fuse) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Adlcopy (Azure Data Lake Storage)](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob) | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp; [Ferramenta de Migração de Dados do Azure Cosmos DB](https://docs.microsoft.com/azure/documentdb/documentdb-import-data) | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp; [Gateway de gerenciamento de dados da Microsoft](https://msdn.microsoft.com/library/dn879362.aspx): mover dados entre o local e a nuvem | S | N |
| &nbsp;&nbsp;&nbsp;&nbsp; Ferramentas de linha de comando Unix/Linux | S | S |
| [Apache Drill](https://drill.apache.org) para exploração de dados | S | S |
| Ferramentas de aprendizado de máquina: |||
| &nbsp;&nbsp;&nbsp;&nbsp; Integração ao [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) (R, Python) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [XGBoost](https://github.com/dmlc/xgboost) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Weka](https://www.cs.waikato.ac.nz/ml/weka/) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Rattle](https://togaware.com/rattle/) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [LightGBM](https://github.com/Microsoft/LightGBM) | N | Y (somente Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp; [CatBoost](https://tech.yandex.com/catboost/) | N | Y (somente Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp; [H2O](https://www.h2o.ai/h2o/), [Sparkling Water](https://www.h2o.ai/sparkling-water/) | N | Y (somente Ubuntu) |
| Ferramentas de aprendizado profundo que funcionam em uma GPU ou CPU: |  |  |
| &nbsp;&nbsp;&nbsp;&nbsp; [Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/) (Windows 2016) | S | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [TensorFlow](https://www.tensorflow.org/) | S (Windows 2016) | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Horovod](https://github.com/uber/horovod) | N | S (Ubuntu) |
| &nbsp;&nbsp;&nbsp;&nbsp; [MXNet](https://mxnet.io/) | S (Windows 2016) | S|
| &nbsp;&nbsp;&nbsp;&nbsp; [Caffe e Caffe2](https://github.com/caffe2/caffe2) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Chainer](https://chainer.org/) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Torch](http://torch.ch/) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Theano](https://github.com/Theano/Theano) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Keras](https://keras.io/)| N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [PyTorch](https://pytorch.org/)| N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [NVidia Digits](https://github.com/NVIDIA/DIGITS) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [Servidor Modelo MXNet](https://github.com/awslabs/mxnet-model-server) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [TensorFlow Serving](https://www.tensorflow.org/serving/) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [TensorRT](https://developer.nvidia.com/tensorrt) | N | S |
| &nbsp;&nbsp;&nbsp;&nbsp; [CUDA, cuDNN, NVIDIA Driver](https://developer.nvidia.com/cuda-toolkit) | S | S |

## <a name="next-steps"></a>Próximas etapas

Saiba mais com estes artigos:

+ Windows:
  + [Configurar uma DSVM do Windows](provision-vm.md)
  + [Dez coisas que você pode fazer em uma DSVM do Windows](vm-do-ten-things.md)

+ Linux:
  + [Configurar uma DSVM do Linux (Ubuntu)](dsvm-ubuntu-intro.md)
  + [Configurar uma DSVM do Linux (CentOS)](linux-dsvm-intro.md)
  + [Ciência de dados em uma DSVM do Linux](linux-dsvm-walkthrough.md)
