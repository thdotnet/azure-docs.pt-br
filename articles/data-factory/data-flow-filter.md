---
title: Transformação do Filtro de Fluxo de Dados de Mapeamento do Azure Data Factory
description: Transformação do Filtro de Fluxo de Dados de Mapeamento do Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: e0b41850c149ff7095333cf77b780dec1f03b882
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66234422"
---
# <a name="azure-data-factory-filter-transformation"></a>Transformação de filtro de fábrica de dados do Azure

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

As transformações de filtro proporcionam filtragem de linha. Crie uma expressão que defina a condição do filtro. Clique na caixa de texto para iniciar o Construtor de Expressões. Dentro do construtor de expressões, crie uma expressão de filtro para controlar quais linhas do fluxo de dados atual podem passar (filtrar) para a próxima transformação. Considere a transformação de filtro da cláusula WHERE de uma instrução SQL.

## <a name="filter-on-loanstatus-column"></a>Filtrar coluna loan_status:

```
in([‘Default’, ‘Charged Off’, ‘Fully Paid’], loan_status).
```

Filtro na coluna de ano na demonstração Filmes:

```
year > 1980
```

## <a name="next-steps"></a>Próximas etapas

Tente uma coluna de filtragem de transformação, o [Selecionar transformação](data-flow-select.md)
