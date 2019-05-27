---
title: Ferramentas do Apache Hive com Data Lake (Apache Hadoop) para Visual Studio – Azure HDInsight
description: Saiba como usar as ferramentas de Data Lake para Visual Studio para executar consultas do Apache Hive com Apache Hadoop no Azure HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: hrasheed
ms.openlocfilehash: 7480dafe435e555bfba81ebd9242bb5724c0bf3f
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65861588"
---
# <a name="run-apache-hive-queries-using-the-data-lake-tools-for-visual-studio"></a>Executar consultas do Apache Hive usando as ferramentas do Data Lake para Visual Studio

Saiba como usar as ferramentas do Data Lake para Visual Studio para consultar o Apache Hive. As ferramentas de Data Lake permitem que você facilmente crie, envie e monitore consultas de Hive para Apache Hadoop no Azure HDInsight.

## <a id="prereq"></a>Pré-requisitos

* Um cluster do Apache Hadoop no HDInsight. Ver [Introdução ao HDInsight no Linux](./apache-hadoop-linux-tutorial-get-started.md).

* Visual Studio (uma das versões a seguir):

    * Visual Studio 2015, 2017 (qualquer edição)
    * Visual Studio 2013 Community/Professional/Premium/Ultimate com Atualização 4

* Ferramentas do HDInsight para Visual Studio ou ferramentas do Azure Data Lake para Visual Studio. Confira [Começar a usar as ferramentas Hadoop para HDInsight do Visual Studio](apache-hadoop-visual-studio-tools-get-started.md) para obter informações sobre como instalar e configurar as ferramentas.

## <a id="run"></a> Executar consultas Apache Hive usando o Visual Studio

Você tem duas opções para criar e executar consultas do Hive:

* Criar consultas ad hoc
* Criar um aplicativo Hive

### <a name="ad-hoc"></a>Ad hoc

Consultas ad hoc podem ser executadas em qualquer um **lote** ou **interativo** modo.

1. Abra **Visual Studio**.

2. Partir **Gerenciador de servidores**, navegue até **Azure** > **HDInsight**.

3. Expandir **HDInsight**e o botão direito do mouse no cluster em que você deseja executar a consulta e, em seguida, selecione **escrever uma consulta Hive**.

4. Insira a seguinte consulta de hive:

    ```hql
    SELECT * FROM hivesampletable;
    ```

5. Selecione **Executar**. Observe que o modo de execução é padronizado para **interativo**.

    ![Captura de tela de Executar consultas interativas do Hive](./media/apache-hadoop-use-hive-visual-studio/vs-execute-hive-query.png)

6. Para executar a mesma consulta no **lote** modo, a lista de alternância na lista suspensa da **interativo** para **lote**. Observe que o botão de execução é alterado de **Execute** à **enviar**.

    ![Captura de tela do envio de uma consulta do Hive](./media/apache-hadoop-use-hive-visual-studio/vs-batch-query.png)

    O editor do Hive é compatível com o IntelliSense. Agora as Ferramentas do Data Lake para Visual Studio dão suporte à obtenção de metadados remotos quando você edita o script do Hive. Por exemplo, se você digitar `SELECT * FROM`, o IntelliSense listará todos os nomes de tabela sugeridos. Quando um nome de tabela for especificado, o IntelliSense listará os nomes de coluna. As ferramentas dão suporte a quase todas as instruções DML Hive, subconsultas e UDFs internos. O IntelliSense sugere apenas os metadados dos clusters selecionados na Barra de Ferramentas do HDInsight.

    ![Captura de tela de um exemplo 1 de IntelliSense das Ferramentas do Visual Studio do HDInsight](./media/apache-hadoop-use-hive-visual-studio/vs-intellisense-table-name.png "U-SQL IntelliSense")
   
    ![Captura de tela de um exemplo 2 de IntelliSense das Ferramentas do Visual Studio do HDInsight](./media/apache-hadoop-use-hive-visual-studio/vs-intellisense-column-name.png "U-SQL IntelliSense")

7. Selecione **Enviar** ou **Enviar (Avançado)**.

   Se você selecionar a opção de envio avançado, configure **Nome do Trabalho**, **Argumentos**, **Configurações Adicionais** e **Diretório de Status** para o script:

    ![Captura de tela de uma consulta Hive do HDInsight Hadoop](./media/apache-hadoop-use-hive-visual-studio/hdinsight.visual.studio.tools.submit.jobs.advanced.png "Enviar consultas")

### <a name="hive-application"></a>Aplicativo de hive

1. Abra **Visual Studio**.

2. Na barra de menus, navegue até **arquivo** > **New** > **projeto**.

3. Dos **novo projeto** janela, navegue até **modelos** > **Azure Data Lake** > **HIVE (HDInsight)**  >  **Hive aplicativo**. 

4. Forneça um nome para este projeto e, em seguida, selecione **Okey**.

5. Abra o arquivo **Script.hql** criado com esse projeto e cole as seguintes instruções HiveQL:

    ```hiveql
    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    Essas instruções executam as seguintes ações:

   * `DROP TABLE`: Se a tabela existir, esta instrução a excluirá.

   * `CREATE EXTERNAL TABLE`: Cria uma nova tabela ‘externa’ no Hive. As tabelas externas armazenam apenas a definição da tabela no Hive (os dados são mantidos no local original).

     > [!NOTE]  
     > As tabelas externas devem ser usadas quando você espera que os dados subjacentes sejam atualizados por uma fonte externa. Por exemplo, um trabalho de MapReduce ou serviço do Azure.
     >
     > Remover uma tabela externa **não** exclui os dados, somente a definição de tabela.

   * `ROW FORMAT`: Informa ao Hive como os dados são formatados. Nesse caso, os campos em cada log são separados por um espaço.

   * `STORED AS TEXTFILE LOCATION`: Informa ao Hive o local em que os dados são armazenados no diretório de exemplo/de dados e que eles estão armazenados como texto.

   * `SELECT`: Seleciona uma contagem de todas as linhas, nas quais a coluna `t4` contém o valor `[ERROR]`. Essa instrução retorna um valor de `3`, já que há três linhas que contêm esse valor.

   * `INPUT__FILE__NAME LIKE '%.log'`: informa ao Hive que só devemos retornar dados de arquivos que terminam em .log. Essa cláusula restringe a pesquisa para o arquivo sample.log que contém os dados.

6. Na barra de ferramentas, selecione o **Cluster HDInsight** que você deseja usar nessa consulta. Selecione **Enviar** para executar as instruções como um trabalho do Hive.

   ![Barra de envio](./media/apache-hadoop-use-hive-visual-studio/toolbar.png)

7. O **Resumo do Trabalho do Hive** aparecerá e exibirá informações sobre o trabalho em execução. Use o link **Atualizar** para atualizar as informações do trabalho, até o **Status do Trabalho** ser alterado para **Concluído**.

   ![resumo do trabalho exibindo um trabalho concluído](./media/apache-hadoop-use-hive-visual-studio/jobsummary.png)

8. Use o link **Saída de Trabalho** para exibir a saída desse trabalho. Ele exibe `[ERROR] 3`, que é o valor retornado por essa consulta.

### <a name="additional-example"></a>Exemplo adicional

Este exemplo se baseia no `log4jLogs` tabela criada na etapa anterior.

1. Partir **Gerenciador de servidores**, o cluster com o botão direito e selecione **escrever uma consulta Hive**.

2. Insira a seguinte consulta de hive:

    ```hql
    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Essas instruções executam as seguintes ações:

    * `CREATE TABLE IF NOT EXISTS`: Crie uma tabela, se ela ainda não existir. Uma vez que a palavra-chave `EXTERNAL` não é usada, essa instrução cria uma tabela interna. As tabelas internas são armazenadas no data warehouse do Hive e gerenciadas por ele.
    
    > [!NOTE]  
    > Ao contrário das tabelas `EXTERNAL`, o descarte de uma tabela interna excluirá também os dados subjacentes.

    * `STORED AS ORC`: Armazena os dados no formato colunar de linha otimizado (ORC). Esse é um formato altamente otimizado e eficiente para o armazenamento de dados do Hive.
    
    * `INSERT OVERWRITE ... SELECT`: Seleciona linhas da tabela `log4jLogs` que contêm `[ERROR]` e, então, insere os dados na tabela `errorLogs`.

3. Executar a consulta no **lote** modo.

4. Para verificar se o trabalho criou a tabela, use o **Gerenciador de Servidores** e expanda **Azure** > **HDInsight** > seu cluster HDInsight > **Bancos de Dados do Hive** > **padrão**. As tabelas **errorLogs** e **log4jLogs** são listadas.

## <a id="nextsteps"></a>Próximas etapas

Como você pode ver, as ferramentas do HDInsight para o Visual Studio fornecem uma maneira fácil de trabalhar com as consultas do Hive no HDInsight.

Para obter informações gerais sobre o Hive no HDInsight:

* [Use o Apache Hive com o Apache Hadoop no HDInsight](hdinsight-use-hive.md)

Para obter informações sobre outros modos possíveis de trabalhar com Hadoop no HDInsight:

* [Use o Apache Pig com o Apache Hadoop no HDInsight](hdinsight-use-pig.md)

* [Usar o MapReduce com o Apache Hadoop no HDInsight](hdinsight-use-mapreduce.md)

Para obter mais informações sobre as ferramentas do HDInsight para o Visual Studio:

* [Introdução às ferramentas do HDInsight para Visual Studio](apache-hadoop-visual-studio-tools-get-started.md)