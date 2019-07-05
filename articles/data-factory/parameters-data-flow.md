---
title: Mapeando parâmetros de fluxo de dados do Azure Data Factory
description: Saiba como parametrizar um fluxo de dados de mapeamento de pipelines do data factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/10/2019
ms.openlocfilehash: 0a7140f70db78c8511f3c4da00b2f9c11c368163
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477670"
---
# <a name="mapping-data-flow-parameters"></a>Mapeando parâmetros de fluxo de dados

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Mapeamento de fluxos de dados no Azure Data Factory suporta o uso de parâmetros. Você pode definir parâmetros dentro de sua definição de fluxo de dados, que pode ser usado em todo seu expressões. Os valores de parâmetro podem ser definidos pelo pipeline chamado por meio da atividade de execução de fluxo de dados. Você tem três opções para definir os valores no fluxo de dados expressões de atividade:

* Use a linguagem de expressão de fluxo de controle de pipeline para definir um valor dinâmico
* Use a linguagem de expressão de fluxo de dados para definir um valor dinâmico
* Use qualquer linguagem de expressão para definir um valor literal estático

Use esse recurso para tornar seus fluxos de dados para fins gerais, flexível e reutilizável. Você pode parametrizar expressões com esses parâmetros e configurações de fluxo de dados.

> [!NOTE]
> Para usar expressões de fluxo de controle do pipeline, o parâmetro de fluxo de dados deve ser do tipo cadeia de caracteres.

## <a name="create-parameters-in-mapping-data-flow"></a>Criar parâmetros de fluxo de dados de mapeamento

Para adicionar parâmetros ao seu fluxo de dados, clique na parte em branco da tela de fluxo de dados para ver as propriedades gerais. No painel de configurações, você verá uma guia chamada 'Parameters'. Clique no botão 'Novo' para gerar um novo parâmetro. Para cada parâmetro, atribua um nome, selecione um tipo e, opcionalmente, defina um valor padrão.

![Criar parâmetros de fluxo de dados](media/data-flow/create-params.png "criar fluxo de dados de parâmetros")

Parâmetros podem ser utilizados em qualquer expressão de fluxo de dados. Parâmetros começam com $ e são imutáveis. Você encontrará a lista de parâmetros disponíveis dentro do construtor de expressão na guia 'Parameters'.

![Expressão de parâmetro de fluxo de dados](media/data-flow/parameter-expression.png "expressão de parâmetro de fluxo de dados")

## <a name="use-parameters-in-your-data-flow"></a>Usar parâmetros em seu fluxo de dados

* Você pode usar valores de parâmetro dentro de suas expressões de transformação. Você encontrará uma lista de parâmetros na guia parâmetros no construtor de expressões. ![Usar parâmetros de fluxo de dados](media/data-flow/params9.png "parâmetros de fluxo de dados de uso")

* Parâmetros também são usados para configurar valores dinâmicos para seu código-fonte e configurações de transformação do coletor. Quando você clica dentro de campos configuráveis, você verá o link "Adicionar contect dinâmico" são exibidos. Clicar em lá, você será levado para um construtor de expressões em que você pode usar parâmetros para usar valores dinâmicos. ![Fluxo de dados de conteúdo dinâmico](media/data-flow/params6.png "conteúdo dinâmico de fluxo de dados")

## <a name="set-mapping-data-flow-parameters-from-pipeline"></a>Definir os parâmetros de fluxo de dados de mapeamento de pipeline

Depois de criar seu fluxo de dados com parâmetros, você pode executá-lo com a atividade de fluxo de dados Execute um pipeline. Depois de adicionar a atividade a tela de pipeline, você será apresentado com os dados disponíveis no guia de 'Parameters' da atividade de parâmetros do fluxo.

![Configuração de um parâmetro de fluxo de dados](media/data-flow/parameter-assign.png "definição de um parâmetro de fluxo de dados")

Se seu tipo de dados do parâmetro é a cadeia de caracteres, quando você clicar na caixa de texto para definir valores de parâmetro, você pode optar por inserir um pipeline ou uma expressão de fluxo de dados. Se você escolher a expressão de pipeline, você será apresentado com o painel de expressão do pipeline. Certifique-se de incluir as funções de pipeline dentro da sintaxe de interpolação de cadeia de caracteres usando ' @{<expression>}', por exemplo:

```'@{pipeline().RunId}'```

Se o parâmetro não é do tipo cadeia de caracteres, você sempre será apresentado com o construtor de expressões de fluxo de dados. Aqui, você pode inserir qualquer expressão ou valores literais que você deseja que corresponde ao tipo de dados do parâmetro. A seguir estão exemplos de expressão de fluxo de dados e uma cadeia de caracteres literal no construtor de expressão de:

* ```toInteger(Role)```
* ```'this is my static literal string'```

Cada fluxo de dados de mapeamento pode ter qualquer combinação de parâmetros de expressão de fluxo de dados e pipeline. 

![Exemplo de parâmetros de fluxo de dados](media/data-flow/parameter-example.png "exemplo de parâmetros de fluxo de dados")



## <a name="next-steps"></a>Próximas etapas
* [Executar a atividade de fluxo de dados](control-flow-execute-data-flow-activity.md)
* [Expressões de fluxo de controle](control-flow-expression-language-functions.md)
