---
title: Configurar uma transformação do coletor no recurso de mapeamento de fluxo de dados do Azure Data Factory
description: Saiba como configurar uma transformação do coletor no mapeamento de fluxo de dados.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 4341cbb0e24330d535f5211c088f0068eab33af7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65596272"
---
# <a name="sink-transformation-for-a-data-flow"></a>Transformação de um fluxo de dados do coletor

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Depois de transformar seu fluxo de dados, você pode coletar os dados em um conjunto de dados de destino. Na transformação coletor, escolha uma definição de conjunto de dados para os dados de saída de destino. Você pode ter muitas transformações de coletor requer que seu fluxo de dados.

Descompasso do esquema e as alterações nos dados de entrada, até os dados de saída do coletor para uma pasta sem um esquema definido no conjunto de dados de saída. Você pode também considerar as alterações de coluna em suas fontes, selecionando **permitir que o descompasso do esquema** na origem. Em seguida, mapear todos os campos no coletor.

![As opções na guia coletor, incluindo a opção de mapeamento automático](media/data-flow/sink1.png "1 do coletor")

Para todos os campos de entrada do coletor, ative **mapeamento automático**. Para escolher os campos para o coletor para o destino, ou para alterar os nomes dos campos no destino, desative **mapeamento automático**. Em seguida, abra o **mapeamento** guia para mapear campos de saída.

![As opções na guia mapeamento](media/data-flow/sink2.png "2 do coletor")

## <a name="output"></a>Saída 
Para o armazenamento de BLOBs do Azure ou tipos de coletor do armazenamento do Data Lake, saída os dados transformados em uma pasta. Spark gera arquivos de dados de saída particionada com base no esquema de particionamento usado pela transformação de coletor. 

Você pode definir o esquema de particionamento do **otimizar** guia. Se quiser que o Data Factory para mesclar sua saída em um único arquivo, selecione **partição única**.

![As opções na guia Otimizar](media/data-flow/opt001.png "opções de coletor")

## <a name="field-mapping"></a>Mapeamento de campo

Sobre o **mapeamento** guia da transformação seu coletor, você pode mapear as colunas de entrada à esquerda para os destinos à direita. Quando o coletor de fluxos de dados para arquivos, o Data Factory sempre escrever novos arquivos para uma pasta. Quando você mapear para um conjunto de dados do banco de dados, você pode gerar uma nova tabela que usa este esquema definindo **salvar política** à **substituir**. Ou inserir novas linhas em uma tabela existente e, em seguida, mapear os campos ao esquema existente. 

![Na guia mapeamento](media/data-flow/sink2.png "Coletores")

Na tabela de mapeamento, você pode multiselect para vincular várias colunas, desvincular várias colunas ou mapear várias linhas para o mesmo nome de coluna.

Para mapear sempre o conjunto de campos de entrada para um destino, como elas estão e aceitar totalmente definições de esquema flexível, selecione **permitir que o descompasso do esquema**.

![Guia de mapeamento, mostrando os campos são mapeados para colunas no conjunto de dados](media/data-flow/multi1.png "várias opções")

Para redefinir os mapeamentos de colunas, selecione **remapear**.

![Guia coletor](media/data-flow/sink1.png "um coletor")

Selecione **validar esquema** falha o coletor se o esquema é alterado.

Selecione **limpar a pasta** para truncar o conteúdo da pasta de coletor antes de gravar os arquivos de destino nessa pasta de destino.

## <a name="file-name-options"></a>Opções de nome de arquivo

Configure a nomenclatura de arquivo: 

   * **Padrão**: Permitir que o Spark para nomear arquivos com base nos padrões de parte.
   * **Padrão de**: Insira um padrão para os arquivos de saída. Por exemplo, **empréstimos [n]** criará loans1.csv, loans2.csv e assim por diante.
   * **Por partição**: Insira um nome de arquivo por partição.
   * **Como os dados na coluna**: Configure o arquivo de saída para o valor de uma coluna.
   * **A saída para um único arquivo**: Com essa opção, o ADF combinará os arquivos de saída particionada em um único nomeado de arquivo. Para usar essa opção, seu conjunto de dados deve resolver para um nome de pasta. Além disso, esteja ciente de que essa operação de mesclagem pode falhar com base no tamanho do nó.

> [!NOTE]
> Início de operações de arquivo apenas quando você estiver executando a atividade de execução de fluxo de dados. Eles não iniciarem no modo de fluxo de dados de depuração.

## <a name="database-options"></a>Opções de banco de dados

Escolha as configurações de banco de dados:

* **Atualizar método**: O padrão é permitir que inserções. Clara **permitir insert** se você quiser interromper a inserção de novas linhas de sua fonte. Para atualizar, upsert ou excluir linhas, primeiro adicione uma transformação de linha de alteração para linhas de marca para essas ações. 
* **Recrie a tabela**: Descartar ou criar sua tabela de destino antes do fluxo de dados é concluída.
* **Truncar tabela**: Remova todas as linhas de sua tabela de destino antes do fluxo de dados é concluída.
* **Tamanho do lote**: Insira um número de gravações de bucket em partes. Use esta opção para cargas de dados grandes. 
* **Habilitar preparo**: Use o PolyBase ao carregar o Data Warehouse do Azure como seu conjunto de dados de coletor.

![Na guia Configurações, mostrando opções de coletor do SQL](media/data-flow/alter-row2.png "opções SQL")

> [!NOTE]
> No fluxo de dados, você pode direcionar o Data Factory para criar uma nova definição de tabela no banco de dados de destino. Para criar a definição da tabela, defina um conjunto de dados na transformação coletor que tem um novo nome de tabela. No conjunto de dados SQL, abaixo do nome da tabela, selecione **editar** e insira um novo nome de tabela. Em seguida, na transformação coletor, ative **permitir que o descompasso do esquema**. Definir **Importar esquema** à **None**.

![As configurações do conjunto de dados SQL, mostrando onde editar o nome da tabela](media/data-flow/dataset2.png "esquema SQL")

> [!NOTE]
> Quando você atualiza ou excluir linhas no seu coletor do banco de dados, você deve definir a coluna de chave. Essa configuração permite que a transformação de linha de alteração determinar a linha exclusiva na biblioteca de movimentação de dados (DML).

## <a name="next-steps"></a>Próximas etapas

Agora que você criou seu fluxo de dados, adicione uma [atividade de fluxo de dados ao seu pipeline](concepts-data-flow-overview.md).
