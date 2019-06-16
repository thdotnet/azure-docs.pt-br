---
title: Configurar uma transformação de origem no recurso de mapeamento de fluxo de dados do Azure Data Factory
description: Saiba como configurar uma transformação de origem no fluxo de dados de mapeamento.
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 5b53819c1d30f6cd62c5941d4b44d70a4996daad
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67117889"
---
# <a name="source-transformation-for-mapping-data-flow"></a>Transformação de origem para o mapeamento de fluxo de dados 

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Uma transformação de origem configura sua fonte de dados para o fluxo de dados. Um fluxo de dados pode incluir mais de uma transformação de origem. Ao projetar os dados fluem, sempre começam com uma transformação de origem.

Cada fluxo de dados requer uma transformação de pelo menos uma fonte. Adicione fontes de tantos conforme necessário para concluir suas transformações de dados. Você pode associar essas fontes junto com uma transformação de junção ou de uma transformação de união.

> [!NOTE]
> Quando você depura seu fluxo de dados, os dados são lidos da origem usando a configuração de amostragem ou os limites de origem de depuração. Para gravar dados em um coletor, você deve executar seu fluxo de dados de um pipeline de atividade de fluxo de dados. 

![Opções de transformação na guia Configurações de origem de origem](media/data-flow/source.png "fonte")

Associe sua transformação de fluxo de dados de origem com exatamente um conjunto de dados do Data Factory. O conjunto de dados define a forma e o local dos dados que você deseja gravar ou ler. Você pode usar listas de curingas e arquivos em sua fonte para trabalhar com mais de um arquivo por vez.

## <a name="data-flow-staging-areas"></a>Áreas de preparação de fluxo de dados

Fluxo de dados funciona com *preparo* conjuntos de dados que estão todos no Azure. Use esses conjuntos de dados de preparo quando você estiver transformando seus dados. 

Fábrica de dados tem acesso a quase 80 conectores nativos. Para incluir dados dessas outras fontes em seu fluxo de dados, use a ferramenta de atividade de cópia a fim de preparar esses dados em uma das áreas de preparo de conjunto de dados de fluxo de dados.

## <a name="options"></a>Opções

Escolha opções de amostragem e o esquema para seus dados.

### <a name="allow-schema-drift"></a>Permitir o descompasso de esquema
Selecione **permitir que o descompasso do esquema** se as colunas de origem serão alterado com frequência. Essa configuração permite que todos os campos de origem de entrada flua por meio de transformações para o coletor.

### <a name="validate-schema"></a>Validar esquema

Se a versão da fonte de dados de entrada não corresponder ao esquema definido, o fluxo de dados não funcionará.

![Configurações de fonte de pública, mostrando as opções de esquema de validação, permitir o descompasso do esquema e amostragem](media/data-flow/source1.png "código-fonte público 1")

### <a name="sample-the-data"></a>Os dados de exemplo
Habilitar **amostragem** para limitar o número de linhas de sua fonte. Use essa configuração quando você testa ou dados de exemplo da fonte para fins de depuração.

## <a name="define-schema"></a>Definir o esquema

Quando os arquivos de origem não são fortemente tipados (por exemplo, arquivos simples em vez de arquivos Parquet), defina os tipos de dados para cada campo aqui na transformação de origem.  

![Na guia esquema de definir as configurações de transformação de origem](media/data-flow/source2.png "fonte 2")

Posteriormente, você pode alterar os nomes de coluna em uma transformação de select. Use uma transformação coluna derivada para alterar os tipos de dados. Para fontes com rigidez de tipos, você pode modificar os tipos de dados em uma transformação selecione mais tarde. 

![Tipos de dados em uma transformação de select](media/data-flow/source003.png "tipos de dados")

### <a name="optimize-the-source-transformation"></a>Otimizar a transformação de origem

Sobre o **otimizar** guia para a transformação de origem, você poderá ver um **origem** tipo de partição. Essa opção está disponível somente quando seu código-fonte é o banco de dados SQL. Isso ocorre porque a fábrica de dados tenta fazer conexões em paralelo para executar consultas grandes no seu código-fonte do banco de dados SQL.

![Configurações de partição de origem](media/data-flow/sourcepart3.png "particionamento")

Você não precisa particionar os dados em sua fonte de banco de dados SQL, mas as partições são úteis para consultas grandes. Você pode basear a partição em uma coluna ou uma consulta.

### <a name="use-a-column-to-partition-data"></a>Usar uma coluna para particionar dados

Da sua tabela de origem, selecione uma coluna de partição no. Também defina o número de partições.

### <a name="use-a-query-to-partition-data"></a>Use uma consulta para dados de partição

Você pode escolher particionar as conexões com base em uma consulta. Basta digite o conteúdo de um predicado WHERE. Por exemplo, insira o ano de 1980 >.

## <a name="source-file-management"></a>Gerenciamento de arquivos de origem

Escolha as configurações para gerenciar arquivos em seu código-fonte. 

![Novas configurações de origem](media/data-flow/source2.png "novas configurações")

* **Caminho curinga**: Na sua pasta de origem, escolha uma série de arquivos que correspondem a um padrão. Essa configuração substitui qualquer arquivo em sua definição de conjunto de dados.

Exemplos de curinga:

* ```*``` Representa qualquer conjunto de caracteres
* ```**``` Representa o aninhamento de diretório recursiva
* ```?``` Substitui um caractere
* ```[]``` Corresponde a um dos mais caracteres entre colchetes

* ```/data/sales/**/*.csv``` Obtém todos os arquivos csv /data/sales
* ```/data/sales/20??/**``` Obtém todos os arquivos no século 20
* ```/data/sales/2004/*/12/[XY]1?.csv``` Obtém todos os arquivos csv em 2004 em dezembro, começando com X ou Y prefixado por um número de dígitos de 2

Contêiner deve ser especificada no conjunto de dados. Seu caminho curinga, portanto, também deve incluir o caminho da pasta na pasta raiz.

* **Lista de arquivos**: Isso é um conjunto de arquivos. Crie um arquivo de texto que inclui uma lista de arquivos do caminho relativo para processar. Aponte para esse arquivo de texto.
* **Coluna para armazenar o nome do arquivo**: Store o nome do arquivo de origem em uma coluna em seus dados. Insira um novo nome para armazenar a cadeia de caracteres de nome de arquivo.
* **Após a conclusão**: Optar por não fazer nada com o arquivo de origem depois que os dados de execuções de fluxo, exclua o arquivo de origem ou mover o arquivo de origem. Os caminhos para a movimentação são relativos.

Para mover arquivos de origem para o pós-processamento de outro local, primeiro selecione "Mover" para a operação de arquivo. Em seguida, defina o diretório "de". Se você não estiver usando curingas para seu caminho, em seguida, a "configuração de" será a mesma pasta que sua pasta de origem.

Se você tiver um caminho de origem com curinga, por exemplo:

```/data/sales/20??/**/*.csv```

Você pode especificar "de" como

```/data/sales```

E "para" como

```/backup/priorSales```

Nesse caso, todos os subdiretórios em /data/sales que foram originados são movidos em relação ao /backup/priorSales.

### <a name="sql-datasets"></a>Conjuntos de dados SQL

Se for sua origem no banco de dados SQL ou SQL Data Warehouse, você tem opções adicionais para o gerenciamento de arquivos de origem.

* **Consulta**: Insira uma consulta SQL para sua fonte. Essa configuração substitui qualquer tabela que você escolheu no conjunto de dados. Observe que **Order By** cláusulas não são suportadas aqui, mas você pode definir uma instrução SELECT FROM completa. Você também pode usar funções de tabela definido pelo usuário. **Selecione * de udfGetData()** é um UDF em SQL que retorna uma tabela. Essa consulta produzirá uma tabela de origem que você pode usar em seu fluxo de dados.
* **Tamanho do lote**: Insira um tamanho de lote para dividir dados grandes em leituras.

> [!NOTE]
> Operações de arquivo executado somente quando você inicia o fluxo de dados de um pipeline executado (uma depuração de pipeline ou execução executar) que usa a atividade de fluxo de dados executado em um pipeline. As operações de arquivo *não* executar no modo de depuração de fluxo de dados.

### <a name="projection"></a>Projeção

Assim como os esquemas em conjuntos de dados, a projeção em uma fonte define as colunas de dados, tipos e formatos de dados de origem. 

![As configurações na guia projeção](media/data-flow/source3.png "projeção")

Se seu arquivo de texto não tiver nenhum esquema definido, selecione **detectar o tipo de dados** para que a fábrica de dados será de exemplo e inferir os tipos de dados. Selecione **formato de padrão de definir** para detecção automática formatos de dados padrão. 

Você pode modificar os tipos de dados de coluna em uma transformação de coluna derivada mais tarde. Use uma transformação de select para modificar os nomes de coluna.

![As configurações padrão para formatos de dados](media/data-flow/source2.png "formatos padrão")

## <a name="next-steps"></a>Próximas etapas

Começar a criar uma [transformação coluna derivada](data-flow-derived-column.md) e uma [Selecionar transformação](data-flow-select.md).
