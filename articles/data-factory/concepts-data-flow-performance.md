---
title: Guia de mapeamento de fluxo de dados de desempenho e otimização no Azure Data Factory | Microsoft Docs
description: Saiba mais sobre os principais fatores que afetam o desempenho de fluxos de dados no Azure Data Factory quando você usa o mapeamento de fluxos de dados.
author: kromerm
ms.topic: conceptual
ms.author: makromer
ms.service: data-factory
ms.date: 05/16/2019
ms.openlocfilehash: 7fca586083f70e0b0f7e593d5203392260cd2136
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66172334"
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

## <a name="optimizing-for-azure-sql-database"></a>Otimizando o banco de dados SQL do Azure

![Parte de origem](media/data-flow/sourcepart2.png "parte de origem")

### <a name="you-can-match-spark-data-partitioning-to-your-source-database-partitioning-based-on-a-database-table-column-key-in-the-source-transformation"></a>Você pode combinar o particionamento de dados para o particionamento de banco de dados de origem com base em uma chave de coluna de tabela de banco de dados na transformação de código-fonte do Spark

* Vá para "Otimizar" e selecione "Origem". Defina uma coluna de tabela específica ou um tipo em uma consulta.
* Se você escolher "coluna", em seguida, escolha a coluna de partição.
* Além disso, defina o número máximo de conexões para seu banco de dados do SQL Azure. Você pode tentar uma configuração mais alta para obter conexões paralelas com seu banco de dados. No entanto, alguns casos podem resultar em desempenho mais rápido com um número limitado de conexões.

### <a name="set-batch-size-and-query-on-source"></a>Defina o tamanho do lote e a consulta na fonte

![Código-fonte](media/data-flow/source4.png "fonte")

* Definindo o tamanho de lote instruirá o ADF para armazenar dados em conjuntos de na memória em vez de linha por linha. É uma configuração opcional e você poderá ficar sem recursos em nós de computação se eles não são dimensionados corretamente.
* Definir uma consulta pode permitirá que você filtre direita de linhas na origem antes que ainda chegam para o fluxo de dados para processamento, o que pode tornar a aquisição de dados inicial mais rápida.
* Se você usar uma consulta, você pode adicionar dicas de consulta opcional para o BD SQL do Azure, ou seja, READ UNCOMMITTED

### <a name="set-sink-batch-size"></a>Definir tamanho do lote de coletor

![Coletor](media/data-flow/sink4.png "do coletor")

* Para evitar o processamento de linha por linha de floes seus dados, defina o "tamanho do lote" nas configurações do coletor para BD SQL do Azure. Isso instruirá o que ADF para processar banco de dados grava em lotes com base no tamanho fornecido.

### <a name="set-partitioning-options-on-your-sink"></a>Conjunto de opções no coletor de particionamento

* Mesmo se você não tiver dados particionados nas tabelas de BD SQL do Azure de destino, vá para a guia otimizar e particionamento do conjunto.
* Com muita frequência, apenas informando o ADF para usar Round Robin particionamento nos clusters Spark de execução resulta em dados muito mais rápidos, carregamento em vez de forçar todas as conexões de um nó ou uma partição única.

### <a name="increase-size-of-your-compute-engine-in-azure-integration-runtime"></a>Aumentar o tamanho do seu mecanismo de computação no tempo de execução de integração do Azure

![Nova IV](media/data-flow/ir-new.png "nova IV")

* Aumente o número de núcleos, o que aumenta o número de nós e fornecer mais poder de processamento de consulta e gravar em seu banco de dados do SQL Azure.
* Tente as opções "Otimizada de computação" e "Otimizado para memória" para aplicar mais recursos a seus nós de computação.

### <a name="disable-indexes-on-write"></a>Desabilitar índices na gravação
* Use uma atividade de procedimento armazenado de pipeline ADF antes de sua atividade de fluxo de dados que desabilita os índices em suas tabelas de destino que estão sendo gravados a partir de coletor.
* Depois de sua atividade de fluxo de dados, adicione outra atividade de procedimento armazenado que habilitado esses índices.

### <a name="increase-the-size-of-your-azure-sql-db"></a>Aumentar o tamanho do seu banco de dados do SQL Azure
* Agende um redimensionamento de sua fonte e coletor SQL do Azure antes de sua execução limita seu pipeline para aumentar a taxa de transferência e minimizar a limitação do Azure depois de chegar a DTU.
* Após a conclusão da execução de seu pipeline, você pode redimensionar seus bancos de dados de volta para sua taxa de execução normal.

## <a name="next-steps"></a>Próximas etapas
Consulte os outros artigos de fluxo de dados:

- [Visão geral do fluxo de dados](concepts-data-flow-overview.md)
- [Atividade de fluxo de dados](control-flow-execute-data-flow-activity.md)

