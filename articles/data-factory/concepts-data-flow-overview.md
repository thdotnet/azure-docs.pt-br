---
title: Visão Geral do Fluxo de Dados do Mapeamento do Azure Data Factory
description: Uma explicação da visão geral dos Fluxos de Dados de Mapeamento no Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 6f4c124c59584c8538d85ac61650661ae559a77b
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70123984"
---
# <a name="what-are-mapping-data-flows"></a>O que são os fluxos de dados de mapeamento?

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

O mapeamento de fluxos de dados é uma transformação de dados designada visualmente em Azure Data Factory. Os fluxos de dados permitem que os engenheiros de dados desenvolvam a lógica de transformação de dados gráficos sem escrever código. Os fluxos de dados resultantes são executados como atividades em Pipelines do Azure Data Factory usando clusters do Azure Databricks expandidos.

A intenção do Fluxo de Dados do Azure Data Factory é fornecer uma experiência totalmente visual sem necessidade de codificação. Seus Fluxos de Dados serão executados em seu próprio cluster em execução para o processamento de dados expandidos. O Azure Data Factory manipula a conversão de código, a otimização de caminho e a execução de seus trabalhos de fluxo de dados.

Comece criando fluxos de dados no modo de depuração para que você possa validar sua lógica de transformação de forma interativa. Em seguida, adicione uma atividade de Fluxo de Dados ao seu pipeline para executar e testar seu fluxo de dados na depuração de pipeline ou use "Disparar agora" no pipeline para testar seu Fluxo de Dados de uma Atividade de pipeline.

Você, em seguida, agendará e monitorará suas atividades de fluxo de dados usando pipelines do Azure Data Factory que executam a atividade de Fluxo de Dados.

A opção de alternância do Modo de Depuração na superfície de design de Fluxo de Dados permite a criação interativa de transformações de dados. O modo de depuração fornece um ambiente de preparação de dados e de visualização de dados para a construção do fluxo de dados.

## <a name="begin-building-your-data-flow-logical-graph"></a>Começar a criar seu grafo lógico de fluxo de dados

Comece a criar fluxos de dados usando o sinal de + em recursos de fábrica para criar um novo fluxo de dados.

![novo fluxo de dados](media/data-flow/newdataflow2.png "novo fluxo de dados")

Comece Configurando sua transformação de origem e, em seguida, adicione a transformação de dados a cada etapa subsequente usando o sinal +. Ao criar seu gráfico lógico, você pode alternar entre modos de grafo e de configuração usando o botão "mostrar grafo" e "ocultar grafo".

![Mostrar grafo](media/data-flow/showg.png "Mostrar grafo")

## <a name="configure-transformation-logic"></a>Configurar a lógica de transformação

![Ocultar grafo](media/data-flow/hideg.png "Ocultar grafo")

Ocultar o grafo permitirá que você navegue pelos nós de transformação mais tarde.

![Navegar](media/data-flow/showhide.png "navegar")

## <a name="next-steps"></a>Próximas etapas

* [Começar com uma transformação de origem](data-flow-source.md)
