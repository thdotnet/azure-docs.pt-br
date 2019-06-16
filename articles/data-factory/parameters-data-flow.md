---
title: Parâmetros de fluxo de dados de mapeamento de Data Factory do Azure
description: Saiba como parametrizar um fluxo de dados de mapeamento de pipelines do data factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: af5f421cc3802f3a7ad44bb294f5066c32569f8b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082879"
---
# <a name="mapping-data-flow-parameters"></a>Mapeamento de parâmetros de fluxo de dados

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Mapeando dados fluxos no data factory suportam o uso de parâmetros. Você pode definir parâmetros dentro de sua definição de fluxo de dados, que pode ser usado em todo seu expressões. Os parâmetros, em seguida, podem ser definidos pelo pipeline de chamada por meio da atividade de execução de fluxo de dados. Você tem três opções a serem usadas para definir os valores no fluxo de dados as expressões de atividade:

* Use a linguagem de expressão de fluxo de controle de pipeline para definir um valor dinâmico
* Use a linguagem de expressão de fluxo de dados para definir um valor dinâmico
* Use qualquer linguagem de expressão para definir um valor literal estático

Use esse recurso para tornar seus fluxos de dados para fins gerais, flexível e reutilizável. Você pode parametrizar expressões com esses parâmetros e configurações de fluxo de dados.

> [!NOTE]
> Para usar expressões de fluxo de controle do pipeline, o parâmetro de fluxo de dados deve ser do tipo cadeia de caracteres.

* Adicione uma atividade de execução de fluxo de dados para a tela do pipeline.
* Se seu fluxo de dados tiver parâmetros, você verá a lista de parâmetros disponíveis em parâmetros tab.* * Clique na caixa de texto ao lado de cada parâmetro para inserir o valor do parâmetro.
* Você pode optar por criar sua expressão de parâmetro por meio da linguagem de expressão de fluxo de controle pipeline ou expressões de fluxo de dados.

![3 de parâmetros de fluxo de dados](media/data-flow/params3.png "3 parâmetros de fluxo de dados")

## <a name="create-parameters-in-data-flow"></a>Criar parâmetros de fluxo de dados

![1 de parâmetros de fluxo de dados](media/data-flow/params1.png "1 de parâmetros de fluxo de dados")

Para adicionar parâmetros ao seu fluxo de dados, clique na parte em branco da tela de fluxo de dados para ver as propriedades gerais. No painel de configurações, você verá uma guia chamada de parâmetros. Clique no botão novo para gerar novos parâmetros, que, em seguida, podem ser definidos do pipeline, passando valores ao seu fluxo de dados. Insira um nome de parâmetro e selecione o tipo de dados para cada parâmetro.

Dentro de expressões de fluxo de dados, você pode utilizar os parâmetros usando os valores definidos do pipeline. Parâmetros começam com $ e são imutáveis. Você também encontrará a lista de seus parâmetros disponíveis dentro do construtor de expressão na guia Parâmetros. Você pode usar esses valores em suas expressões, embora você não pode atribuir novos valores para os parâmetros.

![Parâmetros de 2 de fluxo de dados](media/data-flow/params2.png "2 parâmetros de fluxo de dados")

## <a name="set-data-flow-parameters-from-pipeline"></a>Parâmetros de fluxo de dados do conjunto de pipeline

Depois de criar seu fluxo de dados com parâmetros, agora você pode executar esse fluxo de dados de um pipeline com atividade de fluxo de dados Execute. Depois que essa atividade é adicionada a sua tela de design do pipeline, você será apresentado com os dados disponíveis nos parâmetros da atividade Definir guia Parâmetros do fluxo.

![Linguagem de expressão de parâmetros de fluxo de dados](media/data-flow/params4.png "linguagem de expressão de parâmetros de fluxo de dados")

Quando você clica na caixa de texto para valores de parâmetro de preenchimento, você será apresentado com o construtor de expressões de fluxo de dados. Aqui, você pode inserir qualquer expressão ou valores literais que você deseja que coincidirem com o tipo de dados do parâmetro. A seguir estão exemplos de expressão de fluxo de dados e uma cadeia de caracteres literal no construtor de expressão de:

* ```toInteger(Role)```
* ```'this is my static literal string'```

Se seu tipo de dados do parâmetro é uma cadeia de caracteres, em seguida, você pode optar por inserir um pipeline ou uma expressão de fluxo de dados. Se você escolher a expressão de pipeline, em vez disso, será apresentado com o painel de expressão do pipeline. Certifique-se de incluir as funções de pipeline dentro da sintaxe de interpolação de cadeia de caracteres usando ' @{<expression>}', por exemplo:

```'@{pipeline().RunId}'```

![Exemplo de parâmetros de fluxo de dados](media/data-flow/params5.png "exemplo de parâmetros de fluxo de dados")

## <a name="next-steps"></a>Próximas etapas

* [Executar a atividade de fluxo de dados](control-flow-execute-data-flow-activity.md)
* [Expressões de fluxo de controle](control-flow-expression-language-functions.md)
