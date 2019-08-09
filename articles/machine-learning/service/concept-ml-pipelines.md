---
title: 'Pipelines: fluxos de trabalho de aprendizado de máquina de otimizar'
titleSuffix: Azure Machine Learning service
description: Neste artigo, aprenda sobre os pipelines de aprendizado de máquina que você pode criar com o SDK do Azure Machine Learning para Python e as vantagens de usar pipelines. Os pipelines de ML (Machine learning) são usados por cientistas de dados para criar, otimizar e gerenciar os fluxos de trabalho de aprendizado de máquina.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: sanpil
author: sanpil
ms.date: 08/08/2019
ms.custom: seodec18
ms.openlocfilehash: a9965dbbca939f566048312af921061a188ee50d
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68884227"
---
# <a name="build-reusable-ml-pipelines-in-azure-machine-learning-service"></a>Criar pipelines de ML reutilizáveis no serviço Azure Machine Learning

Neste artigo, saiba mais sobre os pipelines de aprendizado de máquina que você pode criar com o SDK do Azure Machine Learning para Python, bem como as vantagens de usá-los.

## <a name="what-are-machine-learning-pipelines"></a>O que são pipelines de aprendizado de máquina?

Usando pipelines de aprendizado de máquina (ML), os cientistas de dados, os engenheiros de dados e os profissionais de TI podem colaborar com as etapas envolvidas em:
+ Preparação de dados, como normalizações e transformações
+ Treinamento do modelo
+ Avaliação do modelo
+ Implantação

O diagrama a seguir mostra um exemplo de processo de pipeline:

![Pipelines de aprendizado de máquina no Serviço do Azure Machine Learning](./media/concept-ml-pipelines/pipeline-flow.png)

<a name="compare"></a>
### <a name="which-azure-pipeline-technology-should-i-use"></a>Qual tecnologia de pipeline do Azure devo usar?

A nuvem do Azure fornece vários outros pipelines, cada um com uma finalidade diferente. A tabela a seguir lista os pipelines diferentes e para que eles são usados:

| Pipeline | O que faz | Pipe canônico |
| ---- | ---- | ---- |
| Pipelines de Azure Machine Learning | Define fluxos de trabalho de aprendizado de máquina reutilizáveis que podem ser usados como um modelo para seus cenários de aprendizado de máquina. | Modelo de > de dados |
| [Pipelines do Azure Data Factory](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) | Agrupa as atividades de movimentação de dados, transformação e controle necessárias para executar uma tarefa.  | Dados > dados |
| [Pipelines do Azure](https://azure.microsoft.com/services/devops/pipelines/) | Integração e entrega contínuas de seu aplicativo para qualquer plataforma/qualquer nuvem  | Aplicativo/serviço de > de código |

## <a name="why-build-pipelines-with-azure-machine-learning"></a>Por que criar pipelines com o Azure Machine Learning?

Use o [SDK do Azure Machine Learning para Python](#the-python-sdk-for-pipelines) para criar pipelines de ML, bem como enviar e acompanhar execuções de pipeline individuais.

Com pipelines, você pode otimizar o fluxo de trabalho com simplicidade, velocidade, portabilidade e reutilização. Ao criar pipelines com Azure Machine Learning, você pode se concentrar em sua experiência, aprendizado de máquina, em vez de infraestrutura e automação.

Os pipelines são construídos a partir de várias **etapas**, que são unidades computacionais distintas no pipeline. Cada etapa pode ser executada de forma independente e usar recursos de computação isolados. Isso permite que vários cientistas de dados funcionem no mesmo pipeline ao mesmo tempo sem sobrecarregar recursos de computação, além de facilitar o uso de diferentes tipos/tamanhos de computação para cada etapa.

Depois que o pipeline é criado, muitas vezes, há mais ajustes finos no loop de treinamento do pipeline. Quando você executa novamente um pipeline, a execução salta para as etapas distintas que precisam ser executadas novamente, como um script de treinamento atualizado e ignora o que não foi alterado. O mesmo paradigma se aplica a scripts inalterados usados para a execução da etapa. Essa funcionalidade ajuda a evitar a execução de etapas dispendiosas e demoradas, como a ingestão de dados e a transformação se os dados subjacentes não forem alterados.

Com o Azure Machine Learning, você pode usar vários kits de instruções e estruturas, como PyTorch ou TensorFlow, para cada etapa em seu pipeline. O Azure é coordenado entre os vários [destinos de computação](concept-azure-machine-learning-architecture.md) usados, de modo que seus dados intermediários possam ser compartilhados com facilidade com os destinos de computação downstream.

Você pode [acompanhar as métricas para seus testes de pipeline](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiments) diretamente no portal do Azure. Depois que um pipeline tiver sido publicado, você poderá configurar um ponto de extremidade REST que permite executar novamente o pipeline de qualquer plataforma ou pilha.

## <a name="key-advantages"></a>Principais vantagens

As principais vantagens de usar pipelines para seus fluxos de trabalho de aprendizado de máquina são:

|Principal vantagem|Descrição|
|:-------:|-----------|
|**Execuções&nbsp;autônomas**|Agende as etapas para execução em paralelo ou em sequência de maneira confiável e autônoma. A preparação e a modelagem de dados podem durar dias ou semanas, e os pipelines permitem que você se concentre em outras tarefas enquanto o processo está em execução. |
|**Computação heterogênea**|Use vários pipelines que são coordenados de forma confiável entre os recursos de computação heterogêneos e escalonáveis e os locais de armazenamento. Execute etapas de pipeline individuais em destinos de computação diferentes, como HDInsight, VMs de ciência de dados de GPU e databricks. Isso proporciona um uso eficiente das opções de computação disponíveis.|
|**Capacidade de reutilização**|Crie modelos de pipeline para cenários específicos, como retreinamento e pontuação de lote. Disparar pipelines publicados de sistemas externos por meio de chamadas REST simples.|
|**Acompanhamento e controle de versão**|Em vez de acompanhar manualmente os caminhos de dados e resultados durante a iteração, use o SDK de pipelines para fornecer explicitamente o nome e a versão de fontes de dados, entradas e saídas. Você também pode gerenciar os scripts e os dados separadamente para aumentar a produtividade.|
|**Colaboração**|Os pipelines permitem que os cientistas de dados colaborem em todas as áreas do processo de design do Machine Learning, ao mesmo tempo em que podem trabalhar simultaneamente em etapas de pipeline.|

## <a name="the-python-sdk-for-pipelines"></a>O SDK do Python para pipelines

Use o Python para criar os pipelines de ML. O SDK do Azure Machine Learning oferece constructos obrigatórios para o sequenciamento e a paralelização de etapas em seus pipelines quando nenhuma dependência de dados está presente. Você pode interagir com ele em notebooks Jupyter ou em outro IDE preferencial.

Usando dependências de dados declarativas, você pode otimizar suas tarefas. O SDK inclui uma estrutura de módulos predefinidos para tarefas comuns, como transferência de dados e publicação do modelo. Estenda a estrutura para modelar suas próprias convenções, implementando etapas personalizadas reutilizáveis nos pipelines. Gerencie também os destinos de computação e os recursos de armazenamento diretamente no SDK.

Salve os pipelines como modelos e implante-os em um ponto de extremidade REST, de modo que você possa agendar trabalhos de novo treinamento ou pontuação em lote.

Para ver como criar seus próprios pipelines, confira a [documentação de referência de pipelines do SDK do Python](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) e o notebook na próxima seção.

## <a name="example-notebooks"></a>Blocos de anotações de exemplo

Os seguintes notebooks demonstram pipelines com o Azure Machine Learning: [how-to-use-azureml/machine-learning-pipelines](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines).

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Próximas etapas

Saiba como [criar seu primeiro pipeline](how-to-create-your-first-pipeline.md).
