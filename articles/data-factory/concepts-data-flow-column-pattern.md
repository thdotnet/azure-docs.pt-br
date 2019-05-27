---
title: Padrões de Coluna do Fluxo de Dados do Mapeamento do Azure Data Factory
description: Saiba como usar padrões de coluna do Azure Data Factory no mapeamento de fluxo de dados para criar padrões de modelo generalizado para transformar os campos em um fluxo de dados sem levar em consideração os metadados de esquema subjacente
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 5131329f6675bc86374f5a5c081e0aaa7d36c0fe
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155230"
---
# <a name="azure-data-factory-mapping-data-flows-column-patterns"></a>Padrões de coluna de fluxos de dados de mapeamento do Azure data factory

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Várias transformações de Fluxo de Dados do Azure Data Factory dão suporte à ideia de "Padrões de colunas" para que você possa criar colunas de modelo com base nos padrões em vez de nomes de coluna embutidos em código. Você pode usar esse recurso no Construtor de Expressões para definir padrões para correspondência das colunas para transformação em vez de exigir nomes de campo exatos específicos. Padrões são úteis se a entrada campos de origem são alterados com frequência, especialmente no caso de alterar as colunas nos arquivos de texto ou bancos de dados NoSQL. Essa condição é, às vezes, conhecida como "Descompasso do esquema".

![padrões de coluna](media/data-flow/columnpattern2.png "Padrões de coluna")

Padrões de coluna são úteis para lidar com cenários de Descompasso de esquema, bem como cenários gerais. É uma boa opção para condições em que não é possível saber totalmente cada nome de coluna. Você pode padronizar a correspondência no nome de coluna e no tipo de dados de coluna e criar uma expressão para a transformação que executará essa operação em relação a qualquer campo no fluxo de dados que corresponde aos seus padrões `name` & `type`.

Ao adicionar uma expressão a uma transformação que aceita padrões, escolha "Adicionar coluna padrão". Padrões de coluna permitem que a coluna de descompasso do esquema corresponda aos padrões.

Ao criar padrões de coluna de modelo, use `$$` na expressão para declarar uma referência para cada campo correspondente do fluxo de dados de entrada.

Se optar por usar uma das funções regex do Construtor de Expressões, você poderá, posteriormente, usar $1, $2, $3 ... para fazer referência a subpadrões com correspondência de sua expressão regex.

Um exemplo de cenário de Padrão de Coluna está usando SUM com uma série de campos de entrada. Os cálculos SUM agregados ficam na transformação de agregação. Você pode usar SUM em cada correspondência de tipos de campo que corresponda a "integer" e, em seguida, usar $$ para fazer referência a cada correspondência na sua expressão.
