---
title: Transformação de Divisão Condicional do Fluxo de Dados de Mapeamento do Azure Data Factory
description: Transformação de Divisão Condicional do Fluxo de Dados do Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: f9fd346d4c4eaed0797d564fe52dd44e9f0e240a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65795683"
---
# <a name="mapping-data-flow-conditional-split-transformation"></a>Mapeamento de fluxo de dados condicionais transformação de divisão

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![condicional para dividir a caixa de ferramentas](media/data-flow/conditionalsplit2.png "condicional para dividir a caixa de ferramentas")

A transformação de divisão condicional pode rotear linhas de dados para diferentes fluxos dependendo do conteúdo dos dados. A implementação da transformação de divisão condicional é semelhante a uma estrutura de decisão CASE em uma linguagem de programação. A transformação avalia expressões, e com base nos resultados, direciona a linha de dados para o fluxo especificado. Essa transformação também fornece uma saída padrão, para que se uma linha não corresponder a nenhuma expressão ela seja direcionada para a saída padrão.

![divisão condicional](media/data-flow/conditionalsplit1.png "opções de divisão condicional")

## <a name="multiple-paths"></a>Vários caminhos

Para adicionar outras condições, selecione "Adicionar Stream" no painel de configuração inferior e clique na caixa de texto do construtor de expressões para criar sua expressão.

![condicional dividir multi](media/data-flow/conditionalsplit3.png "condicional dividir multi")

## <a name="next-steps"></a>Próximas etapas

Usado com divisão condicional comuns transformações de fluxo de dados: [Junção de transformação](data-flow-join.md), [Loopup transformação](data-flow-lookup.md), [Selecionar transformação](data-flow-select.md)
