---
title: Agregar transformação no mapeamento de fluxo de dados - Azure Data Factory | Microsoft Docs
description: Saiba como agregar dados em grande escala no Azure Data Factory com a transformação agregação de fluxo de dados de mapeamento.
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 21135b26d4bc840b3fcb091e675e5e6bd24d8548
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312113"
---
# <a name="aggregate-transformation-in-mapping-data-flow"></a>Transformação agregação no mapeamento de fluxo de dados 

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

A transformação de agregação é o lugar para definir as agregações de colunas em seus fluxos de dados. Usando o construtor de expressões, você pode definir tipos diferentes de agregações, como SUM, MIN, MAX e COUNT que pode ser agrupado por colunas computadas ou existentes.

## <a name="group-by"></a>Agrupar por
Selecione uma coluna existente ou crie uma nova coluna computada para usar como um grupo por cláusula para a agregação. Para usar uma coluna existente, selecione a coluna desejada na lista suspensa. Para criar uma nova coluna computada, passe o mouse sobre a cláusula e clique em 'Coluna computada'. Isso abrirá o [construtor de expressões de fluxo de dados](concepts-data-flow-expression-builder.md). Depois de criar a coluna computada, insira o nome da coluna de saída sob o campo 'Nome'. Se você quiser adicionar um grupo adicional pela cláusula, passe o mouse sobre uma cláusula existente e clique em '+'.

![Configurações de agrupamento transformação agregação](media/data-flow/agg.png "configurações de agrupamento transformação agregação")

> [!NOTE]
> Uma cláusula group by é opcional em uma transformação agregação.

## <a name="aggregate-column"></a>Coluna de agregação 
Escolha a guia 'Agregações' para criar expressões de agregação. Você pode escolher uma coluna existente e substituir o valor de agregação ou criar um novo campo com um novo nome. A expressão de agregação é inserida na caixa direita ao lado de seletor de nome de coluna. Para editar a expressão, clique na caixa de texto para abrir o construtor de expressões. Para adicionar uma agregação adicional, passe o mouse sobre uma expressão existente e clique em '+' para criar uma nova coluna de agregação ou [padrão de coluna](concepts-data-flow-column-pattern.md).

![Agregar configurações de transformação agregação](media/data-flow/agg2.png "agregar configurações de transformação agregação")

> [!NOTE]
> Cada expressão de agregação deve conter pelo menos uma função de agregação.

> [!NOTE]
> No modo de depuração, o construtor de expressões não pode produzir as visualizações de dados com funções de agregação. Para exibir as visualizações de dados para transformações agregação, feche o construtor de expressões e exibir os dados por meio da guia de 'visualização de dados '.

## <a name="next-steps"></a>Próximas etapas

-Definir agregação baseada em janela usando o [transformação de janela](data-flow-window.md)