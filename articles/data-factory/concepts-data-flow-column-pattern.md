---
title: Padrões de Coluna do Fluxo de Dados do Mapeamento do Azure Data Factory
description: Saiba como usar padrões de coluna do Azure Data Factory no mapeamento de fluxo de dados para criar padrões de modelo generalizado para transformar os campos em um fluxo de dados sem levar em consideração os metadados de esquema subjacente
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 08cdaafe00b7dc586ea75f6ff03fdb89107edee9
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66430762"
---
# <a name="azure-data-factory-mapping-data-flows-column-patterns"></a>Padrões de coluna de fluxos de dados de mapeamento do Azure data factory

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Várias transformações de Fluxo de Dados do Azure Data Factory dão suporte à ideia de "Padrões de colunas" para que você possa criar colunas de modelo com base nos padrões em vez de nomes de coluna embutidos em código. Você pode usar esse recurso no construtor de expressões para definir padrões para correspondência das colunas para a transformação em vez de exigir nomes de campo exato e específico. Padrões são úteis se a entrada campos de origem são alterados com frequência, especialmente no caso de alterar as colunas nos arquivos de texto ou bancos de dados NoSQL. Essa condição é, às vezes, conhecida como "Descompasso do esquema".

![padrões de coluna](media/data-flow/columnpattern2.png "Padrões de coluna")

Padrões de coluna são úteis para lidar com cenários de Descompasso de esquema, bem como cenários gerais. É uma boa opção para condições em que não é possível saber totalmente cada nome de coluna. Você pode padronizar a correspondência no nome de coluna e no tipo de dados de coluna e criar uma expressão para a transformação que executará essa operação em relação a qualquer campo no fluxo de dados que corresponde aos seus padrões `name` & `type`.

Ao adicionar uma expressão a uma transformação que aceita padrões, escolha "Adicionar coluna padrão". Padrões de coluna permitem que a coluna de descompasso do esquema corresponda aos padrões.

Ao criar padrões de coluna de modelo, use `$$` na expressão para declarar uma referência para cada campo correspondente do fluxo de dados de entrada.

Se você optar por usar uma das funções de construtor de expressões de regex, você pode, posteriormente, usar $1, 2 de US $, $3... Para fazer referência as subpadrões combinados a partir de sua expressão regex.

Um exemplo de cenário de Padrão de Coluna está usando SUM com uma série de campos de entrada. Os cálculos SUM agregados ficam na transformação de agregação. Você pode usar soma em cada correspondência dos tipos de campo que correspondem à "integer" e, em seguida, use $$ para cada correspondência na sua expressão de referência.
