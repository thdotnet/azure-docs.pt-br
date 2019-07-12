---
title: Guia de mapeamento de fluxo de dados de desempenho e otimização no Azure Data Factory | Microsoft Docs
description: Saiba mais sobre os principais fatores que afetam o desempenho de fluxos de dados no Azure Data Factory quando você usa o mapeamento de fluxos de dados.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.date: 05/16/2019
ms.openlocfilehash: 1ee266d7d9846a357dce613817affdb0cde5bfdc
ms.sourcegitcommit: e6cb7ca206a125c05acfd431b5a64391a8dcc6b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67569021"
---
# <a name="mapping-data-flows-performance-and-tuning-guide"></a>Desempenho de fluxos de dados de mapeamento e guia de ajuste

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Azure Data Factory mapeando os dados fluem fornecem uma interface de navegador sem código para projetar, implantar e orquestrar as transformações de dados em grande escala.

> [!NOTE]
> Se você não estiver familiarizado com o ADF mapeando os dados fluem em geral, consulte [visão geral de fluxos de dados](concepts-data-flow-overview.md) antes de ler este artigo.
>

> [!NOTE]
> Quando estiver criando e testando fluxos de dados da UI do ADF, certifique-se de ativar a opção de depuração para que você possa executar seus fluxos de dados em tempo real sem esperar que um cluster para aquecimento.
>

![Botão de depuração](media/data-flow/debugb1.png "depurar")

## <a name="monitor-data-flow-performance"></a>Monitorar desempenho de fluxo de dados

Enquanto a criação de seus dados de mapeamento flui no navegador, você pode teste de unidade cada transformação individual, clicando na guia de visualização de dados no painel inferior configurações para cada transformação. A próxima etapa, que você deve tomar é testar sua dados fluxo ponta a ponta no designer de pipeline. Adicionar uma atividade de execução de fluxo de dados e use o botão de depuração para testar o desempenho do fluxo de dados. No painel inferior da janela do pipeline, você verá um ícone dos óculos em "ações":

![Monitor de fluxo de dados](media/data-flow/mon002.png "2 do Monitor de fluxo de dados")

Clicar nesse ícone exibirá o plano de execução e o perfil de desempenho subsequentes do fluxo de dados. Você pode usar essas informações para estimar o desempenho do fluxo de dados em relação a fontes de dados de tamanhos diferentes. Observe que você pode assumir o intervalo de tempo de configuração de execução de trabalho de cluster 1 minuto em seus cálculos de desempenho geral e se você estiver usando o Azure Integration Runtime padrão, você talvez precise adicionar 5 minutos de tempo de giro de cluster também.

![Monitoramento do fluxo de dados](media/data-flow/mon003.png "3 do Monitor de fluxo de dados")

## <a name="optimizing-for-azure-sql-database-and-azure-sql-data-warehouse"></a>Otimização para o banco de dados SQL do Azure e Azure SQL Data Warehouse

![Parte de origem](media/data-flow/sourcepart3.png "parte de origem")

### <a name="partition-your-source-data"></a>Particionar seus dados de origem

* Vá para "Otimizar" e selecione "Origem". Defina uma coluna de tabela específica ou um tipo em uma consulta.
* Se você escolher "coluna", em seguida, escolha a coluna de partição.
* Além disso, defina o número máximo de conexões para seu banco de dados do SQL Azure. Você pode tentar uma configuração mais alta para obter conexões paralelas com seu banco de dados. No entanto, alguns casos podem resultar em desempenho mais rápido com um número limitado de conexões.
* Suas tabelas de banco de dados de origem não precisam ser particionados.
* A definição de uma consulta em sua transformação de código-fonte que corresponde ao esquema de particionamento da tabela do banco de dados permitirá que o mecanismo de banco de dados de origem aproveitar a eliminação de partição.
* Se seu código-fonte já não estiver particionada, o ADF ainda usará particionamento no ambiente de transformação do Spark com base na chave que você selecionar na transformação de fonte de dados.

### <a name="set-batch-size-and-query-on-source"></a>Defina o tamanho do lote e a consulta na fonte

![Código-fonte](media/data-flow/source4.png "fonte")

* Definindo o tamanho de lote instruirá o ADF para armazenar dados em conjuntos de na memória em vez de linha por linha. É uma configuração opcional e você poderá ficar sem recursos em nós de computação se eles não são dimensionados corretamente.
* Definir uma consulta pode permitirá que você filtre direita de linhas na origem antes que ainda chegam para o fluxo de dados para processamento, o que pode tornar a aquisição de dados inicial mais rápida.
* Se você usar uma consulta, você pode adicionar dicas de consulta opcional para o BD SQL do Azure, ou seja, READ UNCOMMITTED

### <a name="set-isolation-level-on-source-transformation-settings-for-sql-datasets"></a>Definir o nível de isolamento nas configurações de transformação de origem para conjuntos de dados SQL

* Leitura não confirmada fornecem resultados mais rápidos de consulta sobre a transformação de origem

![Nível de isolamento](media/data-flow/isolationlevel.png "nível de isolamento")

### <a name="set-sink-batch-size"></a>Definir tamanho do lote de coletor

![Coletor](media/data-flow/sink4.png "do coletor")

* Para evitar o processamento de linha por linha de seus fluxos de dados, defina o "tamanho do lote" nas configurações do coletor para BD SQL do Azure. Isso instruirá o que ADF para processar banco de dados grava em lotes com base no tamanho fornecido.

### <a name="set-partitioning-options-on-your-sink"></a>Conjunto de opções no coletor de particionamento

* Mesmo se você não tiver dados particionados nas tabelas de BD SQL do Azure de destino, vá para a guia otimizar e particionamento do conjunto.
* Com muita frequência, apenas informando o ADF para usar Round Robin particionamento nos clusters Spark de execução resulta em dados muito mais rápidos, carregamento em vez de forçar todas as conexões de um nó ou uma partição única.

### <a name="increase-size-of-your-compute-engine-in-azure-integration-runtime"></a>Aumentar o tamanho do seu mecanismo de computação no tempo de execução de integração do Azure

![Nova IV](media/data-flow/ir-new.png "nova IV")

* Aumente o número de núcleos, o que aumenta o número de nós e fornecer mais poder de processamento de consulta e gravar em seu banco de dados do SQL Azure.
* Tente as opções "Otimizada de computação" e "Otimizado para memória" para aplicar mais recursos a seus nós de computação.

### <a name="unit-test-and-performance-test-with-debug"></a>Teste de unidade e teste de desempenho com a depuração

* Quando os fluxos de dados de teste de unidade, definida no botão "Dados de fluxo Debug" como ON.
* Dentro do designer de fluxo de dados, use a guia de visualização de dados em transformações para exibir os resultados de sua lógica de transformação.
* Teste de unidade que os dados fluem do designer de pipeline, colocando uma atividade de fluxo de dados sobre o design do pipeline de tela e use o botão de "Debug" para testar.
* Teste no modo de depuração funcionará em um ambiente de cluster de prontas em tempo real sem a necessidade de aguardar uma rotação de cluster just-in-time-up.

### <a name="disable-indexes-on-write"></a>Desabilitar índices na gravação
* Use uma atividade de procedimento armazenado de pipeline ADF antes de sua atividade de fluxo de dados que desabilita os índices em suas tabelas de destino que estão sendo gravados a partir de coletor.
* Depois de sua atividade de fluxo de dados, adicione outra atividade de procedimento armazenado que habilitado esses índices.

### <a name="increase-the-size-of-your-azure-sql-db"></a>Aumentar o tamanho do seu banco de dados do SQL Azure
* Agende um redimensionamento de sua fonte e coletor SQL do Azure antes de sua execução limita seu pipeline para aumentar a taxa de transferência e minimizar a limitação do Azure depois de chegar a DTU.
* Após a conclusão da execução de seu pipeline, você pode redimensionar seus bancos de dados de volta para sua taxa de execução normal.

## <a name="optimizing-for-azure-sql-data-warehouse"></a>Otimização para o Azure SQL Data Warehouse

### <a name="use-staging-to-load-data-in-bulk-via-polybase"></a>Usar a preparação para carregar dados em massa por meio do Polybase

* Para evitar o processamento de linha por linha de seus fluxos de dados, defina a opção de "Preparo" nas configurações do coletor para que o ADF pode aproveitar o Polybase para evitar inserções de linha por linha no DW. Isso instruirá o ADF para usar o Polybase para que os dados podem ser carregados em massa.
* Quando você executa sua atividade de fluxo de dados de um pipeline, com preparação ativado, você precisará selecionar o local do repositório de Blob de seus dados de preparo para carregamento em massa.

### <a name="increase-the-size-of-your-azure-sql-dw"></a>Aumentar o tamanho do seu Azure SQL DW

* Agende um redimensionamento de sua fonte e coletor SQL DW do Azure antes de você executar o pipeline para aumentar a taxa de transferência e minimizar a limitação do Azure quando você atingir os limites DWU.

* Após a conclusão da execução de seu pipeline, você pode redimensionar seus bancos de dados de volta para sua taxa de execução normal.

## <a name="optimize-for-files"></a>Otimizar para arquivos

* Você pode controlar quantas partições que usará o ADF. Em cada transformação de código-fonte e coletor, bem como cada transformação individual, você pode definir um esquema de particionamento. Para arquivos menores, você pode achar selecionar "Partição única", às vezes, pode funcionar melhor e mais rápido do que solicitando que o Spark para particionar seus arquivos pequenos.
* Se você não tiver informações suficientes sobre sua fonte de dados, você pode escolher "Round-Robin" particionamento e definir o número de partições.
* Se você explorar seus dados e descobre que tem colunas que podem ser chaves de hash boa, use o opção de particionamento de Hash.

### <a name="file-naming-options"></a>Opções de nomenclatura de arquivo

* É a natureza de padrão de gravação de dados transformados no ADF mapeando os dados fluem gravar em um conjunto de dados que tem um serviço vinculado do ADLS ou Blob. Você deve definir esse conjunto de dados para apontar para uma pasta ou contêiner, não é um arquivo nomeado.
* O uso de fluxos de dados do Azure Databricks Spark para execução, o que significa que a saída será dividida em vários arquivos com base no padrão ou o particionamento do Spark ou o particionamento de esquema que você escolheu explicitamente.
* Uma operação bastante comum em fluxos de dados do ADF é escolher "Saída para um arquivo único", de modo que todos os arquivos da parte de saída são mesclados em um arquivo de saída única.
* No entanto, esta operação requer que a saída reduz a uma única partição em um único nó de cluster.
* Tenha isso em mente ao escolher essa opção popular. Você pode ficar sem recursos de nó de cluster se você estiver combinando vários arquivos de origem grandes em uma partição de arquivo de saída única.
* Para evitar o esgotamento de recursos do nó de computação, você pode manter o padrão ou o esquema de particionamento explícita no ADF, que otimiza o desempenho de, e, em seguida, adicione uma atividade de cópia subsequentes no pipeline que mescla todos da parte de arquivos da pasta de saída para um único novo arquivo. Essencialmente, essa técnica separa a ação de transformação de mesclagem de arquivos e alcança o mesmo resultado que a configuração "saída para um arquivo único".

### <a name="looping-through-file-lists"></a>Loop através de listas de arquivos

Na maioria dos casos, os dados fluem no ADF executará melhor de um pipeline que permite a transformação de origem de fluxo de dados iterar por meio de vários arquivos. Em outras palavras, é preferível usar curingas ou listas de arquivos dentro de sua fonte de dados de fluxo que para iterar sobre uma grande lista de arquivos usando ForEach no pipeline, chamar um fluxo de dados executado em cada iteração. O processo de fluxo de dados serão executadas mais rapidamente, permitindo que o loop deve ocorrer dentro do fluxo de dados.

Por exemplo, se eu tiver uma lista de arquivos de dados de julho de 2019 que desejo processar em uma pasta no armazenamento de BLOBs, seria mais eficaz para chamar uma atividade de fluxo de dados executar uma vez do seu pipeline e usar um caractere curinga em seu código-fonte como este :

```DateFiles/*_201907*.txt```

Isso executará melhor do que o Blob Store em um pipeline que, em seguida, itera em todos os arquivos correspondentes usando ForEach com uma atividade de execução de fluxo de dados dentro de uma pesquisa.

## <a name="next-steps"></a>Próximas etapas

Consulte os outros de fluxo de dados artigos relacionados ao desempenho:

- [Guia de otimizar o fluxo de dados](concepts-data-flow-optimize-tab.md)
- [Atividade de fluxo de dados](control-flow-execute-data-flow-activity.md)
- [Monitorar o desempenho de fluxo de dados](concepts-data-flow-monitoring.md)
