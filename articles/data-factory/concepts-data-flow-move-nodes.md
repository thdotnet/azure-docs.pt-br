---
title: Nós de Movimentação de Fluxo de Dados do Azure Data Factory
description: Como mover os nós em um Azure Data Factory mapeamento dados diagrama de fluxo
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: 951a5d4fcbd561b085b0377bde48e820dc8972a2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65519962"
---
# <a name="mapping-data-flow-move-nodes"></a>Nós de movimentação de fluxo de dados de mapeamento

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Opções de transformação de agregação](media/data-flow/agghead.png "cabeçalho agregador")

A superfície de design de Fluxo de Dados do Azure Data Factory é uma superfície de "construção" na qual você cria fluxos de dados de cima para baixo, da esquerda para a direita. Há uma caixa de ferramentas anexada a cada transformação com um símbolo de adição (+). Concentre-se na lógica de negócios em vez de conectar nós por meio de bordas em um ambiente de DAG de forma livre.

Portanto, sem um paradigma do tipo "arrastar e soltar", o modo de "mover" um nó de transformação é alterar o fluxo de entrada. Em vez disso, você moverá as transformações alterando o "fluxo de entrada".

## <a name="streams-of-data-inside-of-data-flow"></a>Fluxos de dados dentro do fluxo de dados

No Fluxo de Dados do Azure Data Factory, os fluxos declaram o fluxo de dados. No painel de configurações de transformação, você verá um campo de "Fluxo de entrada". Isso informa qual fluxo de dados de entrada está alimentando a transformação. Você pode alterar a localização física do seu nó de transformação no gráfico clicando no nome do fluxo de entrada e selecionando outro fluxo de dados. A transformação atual, juntamente com todas as transformações subsequentes no fluxo, será movida para a nova localização.

Se você estiver movendo uma transformação com uma ou mais transformações depois dela, a nova localização no fluxo de dados será adicionado por meio de um novo branch.

Se não tiver transformações subsequentes após o nó que você selecionou, somente essa transformação será movida para a nova localização.

## <a name="next-steps"></a>Próximas etapas

Depois de concluir o design de fluxo de dados, ative o botão de depuração e testá-lo no modo de depuração ou diretamente na [designer de fluxo de dados](concepts-data-flow-debug-mode.md) ou [depuração de pipeline](control-flow-execute-data-flow-activity.md).
