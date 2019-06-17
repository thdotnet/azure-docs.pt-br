---
title: 'Edite metadados: Referência do módulo'
titleSuffix: Azure Machine Learning service
description: Saiba como usar o módulo editar metadados no serviço do Azure Machine Learning para alterar os metadados que está associado a colunas em um conjunto de dados.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 282652adb917450c262e08bf10c3c6e537b829e7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67072715"
---
# <a name="edit-metadata-module"></a>Módulo editar metadados

Este artigo descreve um módulo da interface visual (visualização) para o serviço de Azure Machine Learning.

Use o módulo de editar dados para alterar os metadados que está associado a colunas em um conjunto de dados. O tipo de valor e os dados do conjunto de dados será alterado após o uso do módulo editar metadados.

Alterações de metadados típicas podem incluir:
  
+ Tratando colunas Boolianas ou numéricas como valores categóricos.
  
+ Que indica qual coluna contém o **classe** rótulo ou contém os valores que você deseja categoriar ou predizer.
  
+ Marcar colunas como recursos.
  
+ Alterando os valores de data/hora para valores numéricos ou vice-versa.
  
+ A renomeação das colunas.
  
 Use editar metadados sempre que precisar modificar a definição de uma coluna, normalmente para atender aos requisitos para um módulo de downstream. Por exemplo, alguns módulos funcionam apenas com tipos de dados específicos ou exigem sinalizadores nas colunas, como `IsFeature` ou `IsCategorical`.  
  
 Depois de executar a operação necessária, você pode redefinir os metadados para seu estado original.
  
## <a name="configure-edit-metadata"></a>Configurar editar metadados
  
1. No Azure Machine Learning, adicione o módulo editar metadados ao seu experimento e conecte o conjunto de dados que você deseja atualizar. Você pode encontrar o conjunto de dados sob **Data Transformation** na **manipular** categoria.
  
1. Selecione **inicie o seletor de coluna** e escolha a coluna ou conjunto de colunas para trabalhar com. Você pode escolher colunas individualmente por nome ou índice, ou você pode escolher um grupo de colunas por tipo.  
  
1. Selecione o **tipo de dados** opção se você precisar atribuir um tipo de dados diferentes para as colunas selecionadas. Talvez seja necessário alterar o tipo de dados para determinadas operações. Por exemplo, se seu conjunto de dados de origem tiver números tratados como texto, você deve alterá-los para um tipo de dados numérico antes de usar operações matemáticas.

    + Os tipos de dados com suporte são **cadeia de caracteres**, **inteiro**, **Double**, **booliano**, e **DateTime**.

    + Se você selecionar várias colunas, você deve aplicar as alterações de metadados *todos os* colunas selecionadas. Por exemplo, digamos que você escolha dois ou três colunas numéricas. Você pode alterá-los todos para uma cadeia de caracteres no tipo de dados e renomeá-los em uma única operação. No entanto, é possível alterar uma coluna para um tipo de dados de cadeia de caracteres e a outra coluna de um float em um inteiro.
  
    + Se você não especificar um novo tipo de dados, os metadados da coluna são inalterado.

    + O tipo de coluna e valores serão alterado depois de executar a operação de editar metadados. Você pode recuperar o tipo de dados original a qualquer momento usando editar metadados para redefinir o tipo de dados de coluna.  

    > [!NOTE]
    > Se você alterar qualquer tipo de número para o **DateTime** de tipo, deixe o **formato DateTime** espaço em branco o campo. Atualmente, não é possível especificar o formato de dados de destino.  

1. Selecione o **categórico** opção para especificar que os valores nas colunas selecionadas devem ser tratados como categorias.

    Por exemplo, você pode ter uma coluna que contém os números 0, 1 e 2, mas sabe que os números, na verdade, dizer "Smoker", "Não-smoker" e "Desconhecido". Nesse caso, sinalizando que a coluna como categórica você garantir que os valores são usados apenas para agrupar os dados e não em cálculos numéricos.
  
1. Use o **campos** opção se você quiser alterar a maneira que o Azure Machine Learning usa os dados em um modelo.

    + **Recurso**: Use esta opção para sinalizar uma coluna como um recurso em módulos que operam apenas em colunas de recurso. Por padrão, todas as colunas são inicialmente tratadas como recursos.  
  
    + **Rótulo**: Use esta opção para marcar o rótulo, que também é conhecido como a variável de destino ou de atributo previsível. Muitos módulos exigem que essa coluna de rótulo exatamente uma está presente no conjunto de dados.

        Em muitos casos, o Azure Machine Learning pode deduzir que uma coluna contém um rótulo de classe. Definindo esses metadados, você pode garantir que a coluna é identificada corretamente. Essa opção não altera os valores de dados. Ele altera apenas a forma como alguns algoritmos de aprendizado de máquina tratar os dados.
  
    > [!TIP]
    > Você tem dados que não cabem nessas categorias? Por exemplo, seu conjunto de dados pode conter valores como identificadores exclusivos que não são úteis como variáveis. Às vezes, essas IDs podem causar problemas quando usados em um modelo.
    >
    > Felizmente, aprendizado de máquina do Azure mantém todos os seus dados, para que você não precisa excluir essas colunas do conjunto de dados. Quando você precisar executar operações em um conjunto especial de colunas, basta remover todas as outras colunas temporariamente usando o [selecionar colunas no conjunto de dados](select-columns-in-dataset.md) módulo. Mais tarde você pode mesclar as colunas de volta para o conjunto de dados usando o [adicionar colunas](add-columns.md) módulo.  
  
1. Use as opções a seguir para limpar as seleções anteriores e a restauração de metadados para os valores padrão.  
  
    + **Limpar recurso**: Use esta opção para remover o sinalizador de recurso.  
  
         Todas as colunas são inicialmente tratadas como recursos. Para módulos que executam operações matemáticas, você talvez precise usar essa opção para impedir que colunas numéricas que estão sendo tratados como variáveis.
  
    + **Limpar rótulo**: Use esta opção para remover o **rótulo** metadados da coluna especificada.  
  
    + **Limpar pontuação**: Use esta opção para remover o **pontuação** metadados da coluna especificada.  
  
         Você atualmente não é possível marcar explicitamente uma coluna como uma pontuação no Azure Machine Learning. No entanto, algumas operações resultam em uma coluna que está sendo sinalizada como uma pontuação internamente. Além disso, um módulo R personalizado pode gerar valores de pontuação.

1. Para **novos nomes de coluna**, digite o novo nome das colunas da coluna selecionada.  
  
    + Nomes de coluna podem usar apenas caracteres que são compatíveis com UTF-8 codificação. Cadeias de caracteres vazias, valores nulos ou nomes consistem inteiramente de espaços não são permitidos.  
  
    + Para renomear várias colunas, digite os nomes como uma lista separada por vírgulas na ordem dos índices de coluna.  
  
    + Todas as colunas selecionadas devem ser renomeadas. Você não pode omitir ou ignorar colunas.  
  
1. Execute o experimento.  

## <a name="next-steps"></a>Próximas etapas

Consulte a [conjunto de módulos disponíveis](module-reference.md) para o serviço de Azure Machine Learning.
