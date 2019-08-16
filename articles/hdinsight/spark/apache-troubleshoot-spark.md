---
title: Solucionar problemas do Spark usando o Azure HDInsight
description: Obtenha respostas para perguntas comuns sobre como trabalhar com o Apache Spark e o Azure HDInsight.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.topic: troubleshooting
ms.date: 08/15/2019
ms.custom: seodec18
ms.openlocfilehash: c88136fee7a75b8f3b8e504b1ff1e6673a31bcf7
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69543166"
---
# <a name="troubleshoot-apache-spark-by-using-azure-hdinsight"></a>Solucionar problemas do Apache Spark usando o Azure HDInsight

Saiba mais sobre os principais problemas e suas soluções ao trabalhar com cargas de [Apache Spark](https://spark.apache.org/) no [Apache Ambari](https://ambari.apache.org/).

## <a name="how-do-i-configure-an-apache-spark-application-by-using-apache-ambari-on-clusters"></a>Como fazer para configurar um aplicativo Apache Spark usando o Ambari nos clusters?

### <a name="resolution-steps"></a>Etapas de resolução

Os valores de configuração do Spark podem ser ajustados ajudam a evitar uma exceção Apache Spark aplicativo OutofMemoryError. As etapas a seguir mostram os valores de configuração padrão do Spark no Azure HDInsight: 

1. Na lista de clusters, selecione **Spark2**.

    ![Selecione o cluster na lista](./media/apache-troubleshoot-spark/update-config-1.png)

2. Selecione a guia **Configurações** .

    ![Selecione a guia Configurações](./media/apache-troubleshoot-spark/update-config-2.png)

3. Na lista de configurações, selecione **Custom-spark2-defaults**.

    ![Selecione custom-spark-defaults](./media/apache-troubleshoot-spark/update-config-3.png)

4. Procure a configuração do valor que você precisa ajustar, como **spark.executor.memory**. Nesse caso, o valor de **4608m** é alto demais.

    ![Selecione o campo spark.executor.memory](./media/apache-troubleshoot-spark/update-config-4.png)

5. Defina o valor para a configuração recomendada. O valor de **2048m** é recomendado para essa configuração.

    ![Altere o valor para 2048m](./media/apache-troubleshoot-spark/update-config-5.png)

6. Salve o valor e, em seguida, salve a configuração. Na barra de ferramentas, selecione **Salvar**.

    ![Salvar a configuração e a configuração](./media/apache-troubleshoot-spark/update-config-6a.png)

    Você será notificado se todas as configurações precisarem de atenção. Observe os itens e, em seguida, selecione **Continuar Assim Mesmo**. 

    ![Selecione Continuar Assim Mesmo](./media/apache-troubleshoot-spark/update-config-6b.png)

    Escreva uma observação sobre as alterações de configuração e selecione **Salvar**.

    ![Insira uma observação sobre as alterações feitas](./media/apache-troubleshoot-spark/update-config-6c.png)

7. Sempre que uma configuração é salva, você é solicitado a reiniciar o serviço. Selecione **Reiniciar**.

    ![Selecione reiniciar](./media/apache-troubleshoot-spark/update-config-7a.png)

    Confirme a reinicialização.

    ![Selecione Confirmar Reiniciar Tudo](./media/apache-troubleshoot-spark/update-config-7b.png)

    Você pode examinar os processos em execução.

    ![Examinar processos em execução](./media/apache-troubleshoot-spark/update-config-7c.png)

8. Você pode adicionar configurações. Na lista de configurações, selecione **Custom-spark2-defaults** e, em seguida, selecione **Adicionar Propriedade**.

    ![Selecione adicionar propriedade](./media/apache-troubleshoot-spark/update-config-8.png)

9. Defina uma nova propriedade. Você pode definir uma única propriedade usando uma caixa de diálogo para configurações específicas, como o tipo de dados. Ou você pode definir várias propriedades usando uma definição por linha. 

    Neste exemplo, a propriedade **spark.driver.memory** é definida com um valor de **4g**.

    ![Definir nova propriedade](./media/apache-troubleshoot-spark/update-config-9.png)

10. Salve a configuração e, em seguida, reinicie o serviço conforme descrito nas etapas 6 e 7.

Essas alterações valem para todo o cluster, mas podem ser substituídas quando você enviar o trabalho do Spark.

### <a name="additional-reading"></a>Leitura adicional

[Envio de trabalho do Apache Spark em clusters do HDInsight](https://web.archive.org/web/20190112152841/https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)

## <a name="how-do-i-configure-an-apache-spark-application-by-using-a-jupyter-notebook-on-clusters"></a>Como fazer para configurar um aplicativo Apache Spark usando um Jupyter Notebook nos clusters?

### <a name="resolution-steps"></a>Etapas de resolução

1. Para determinar quais configurações do Apache Spark precisam ser definidas e com quais valores, consulte O que causa uma exceção OutofMemoryError de um aplicativo Apache Spark.

2. Na primeira célula do bloco de anotações do Jupyter, após a diretiva **%%configure**, especifique as configurações do Spark em um formato JSON válido. Altere os valores reais conforme necessário:

    ![Adicionar uma configuração](./media/apache-troubleshoot-spark/add-configuration-cell.png)

### <a name="additional-reading"></a>Leitura adicional

[Envio de trabalho do Apache Spark em clusters do HDInsight](https://web.archive.org/web/20190112152841/https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)


## <a name="how-do-i-configure-an-apache-spark-application-by-using-apache-livy-on-clusters"></a>Como fazer para configurar um aplicativo Apache Spark usando o Apache Livy nos clusters?

### <a name="resolution-steps"></a>Etapas de resolução

1. Para determinar quais configurações do Apache Spark precisam ser definidas e com quais valores, consulte O que causa uma exceção OutofMemoryError de um aplicativo Apache Spark. 

2. Envie o aplicativo Spark ao Livy usando um cliente REST, como cURL. Use um comando semelhante ao seguinte. Altere os valores reais conforme necessário:

    ```apache
    curl -k --user 'username:password' -v -H 'Content-Type: application/json' -X POST -d '{ "file":"wasb://container@storageaccountname.blob.core.windows.net/example/jars/sparkapplication.jar", "className":"com.microsoft.spark.application", "numExecutors":4, "executorMemory":"4g", "executorCores":2, "driverMemory":"8g", "driverCores":4}'  
    ```

### <a name="additional-reading"></a>Leitura adicional

[Envio de trabalho do Apache Spark em clusters do HDInsight](https://web.archive.org/web/20190112152841/https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)

## <a name="how-do-i-configure-an-apache-spark-application-by-using-spark-submit-on-clusters"></a>Como fazer para configurar um aplicativo Apache Spark usando o envio spark nos clusters?

### <a name="resolution-steps"></a>Etapas de resolução

1. Para determinar quais configurações do Apache Spark precisam ser definidas e com quais valores, consulte O que causa uma exceção OutofMemoryError de um aplicativo Apache Spark.

2. Inicie o shell do Spark usando um comando semelhante ao seguinte. Altere o valor real das configurações conforme necessário: 

    ```apache
    spark-submit --master yarn-cluster --class com.microsoft.spark.application --num-executors 4 --executor-memory 4g --executor-cores 2 --driver-memory 8g --driver-cores 4 /home/user/spark/sparkapplication.jar
    ```

### <a name="additional-reading"></a>Leitura adicional

[Envio de trabalho do Apache Spark em clusters do HDInsight](https://web.archive.org/web/20190112152841/https://blogs.msdn.microsoft.com/azuredatalake/2017/01/06/spark-job-submission-on-hdinsight-101/)

## <a name="next-steps"></a>Próximas etapas

Se você não encontrou seu problema ou não conseguiu resolver seu problema, visite um dos seguintes canais para obter mais suporte:

* [Visão geral do gerenciamento de memória do Spark](https://spark.apache.org/docs/latest/tuning.html#memory-management-overview).

* Depurando o [aplicativo Spark em clusters HDInsight](https://blogs.msdn.microsoft.com/azuredatalake/2016/12/19/spark-debugging-101/).

* Obtenha respostas de especialistas do Azure por meio do [suporte da Comunidade do Azure](https://azure.microsoft.com/support/community/).

* Conecte- [@AzureSupport](https://twitter.com/azuresupport) se com a conta de Microsoft Azure oficial para melhorar a experiência do cliente. Conectando a Comunidade do Azure aos recursos certos: respostas, suporte e especialistas.

* Se precisar de mais ajuda, você poderá enviar uma solicitação de suporte do [portal do Azure](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecione **suporte** na barra de menus ou abra o Hub **ajuda + suporte** . Para obter informações mais detalhadas, consulte [como criar uma solicitação de suporte do Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). O acesso ao gerenciamento de assinaturas e ao suporte de cobrança está incluído na sua assinatura do Microsoft Azure, e o suporte técnico é fornecido por meio de um dos [planos de suporte do Azure](https://azure.microsoft.com/support/plans/).
