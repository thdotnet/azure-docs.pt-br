---
title: Usar o MapReduce e o PowerShell com Hadoop | Microsoft Docs
description: Aprenda a usar o PowerShell para executar trabalhos MapReduce remotamente com Hadoop no HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21b56d32-1785-4d44-8ae8-94467c12cfba
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/19/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: f3be777497d842f019c1904ec1990bd1f1213ba2
ms.openlocfilehash: c2bed4f1fddf99183faa0730f052ee79cf77f9f8


---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a>Executar trabalhos MapReduce com Hadoop no HDInsight usando o PowerShell

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Esse documento fornece um exemplo de uso do PowerShell do Azure para executar um trabalho MapReduce em um Hadoop no cluster HDInsight.

## <a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Pré-requisitos

Para concluir as etapas neste artigo, você precisará do seguinte:

* **Um cluster Azure HDInsight (Hadoop no HDInsight)**

  > [!IMPORTANT]
  > O Linux é o único sistema operacional usado no HDInsight versão 3.4 ou superior. Para saber mais, veja [Substituição do HDInsight no Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* **Uma estação de trabalho com o PowerShell do Azure**.

## <a name="a-idpowershellarun-a-mapreduce-job-using-azure-powershell"></a><a id="powershell"></a>Executar um trabalho MapReduce usando o PowerShell do Azure

O PowerShell do Azure fornece *cmdlets* que permitem executar remotamente trabalhos MapReduce no HDInsight. Internamente, isso é feito por meio de chamadas REST para [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (anteriormente chamado de Templeton) em execução no cluster HDInsight.

Os cmdlets a seguir são usados ao executar trabalhos MapReduce em um cluster HDInsight remoto.

* **Login-AzureRmAccount**: autentica o Azure PowerShell para sua assinatura do Azure.

* **New-AzureRmHDInsightMapReduceJobDefinition**: cria uma nova *definição de trabalho* usando as informações especificadas do MapReduce.

* **Start-AzureRmHDInsightJob**: envia a definição do trabalho para o HDInsight, inicia o trabalho e retorna um objeto *job* que pode ser usado para verificar o status do trabalho.

* **Wait-AzureRmHDInsightJob**: usa o objeto de trabalho para verificar o status do trabalho. Ele aguarda até que o trabalho seja concluído ou o tempo de espera seja excedido.

* **Get-AzureRmHDInsightJobOutput**: usado para recuperar a saída do trabalho.

As etapas a seguir demonstram como usar esses cmdlets para executar um trabalho no seu cluster HDInsight.

1. Usando um editor, salve o código a seguir como **mapreducejob.ps1**..
    
    ```powershell
    # Login to your Azure subscription
    # Is there an active Azure subscription?
    $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
    if(-not($sub))
    {
        Add-AzureRmAccount
    }

    # Get cluster info
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    $creds=Get-Credential -Message "Enter the login for the cluster"

    #Get the cluster info so we can get the resource group, storage, etc.
    $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroup = $clusterInfo.ResourceGroup
    $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
    $container=$clusterInfo.DefaultStorageContainer
    $storageAccountKey=(Get-AzureRmStorageAccountKey `
        -Name $storageAccountName `
    -ResourceGroupName $resourceGroup)[0].Value

    #Create a storage content and upload the file
    $context = New-AzureStorageContext `
        -StorageAccountName $storageAccountName `
        -StorageAccountKey $storageAccountKey

    #Define the MapReduce job
    #NOTE: If using an HDInsight 2.0 cluster, use hadoop-examples.jar instead.
    # -JarFile = the JAR containing the MapReduce application
    # -ClassName = the class of the application
    # -Arguments = The input file, and the output directory
    $wordCountJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
        -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
        -ClassName "wordcount" `
        -Arguments `
            "wasb:///example/data/gutenberg/davinci.txt", `
            "wasb:///example/data/WordCountOutput"

    #Submit the job to the cluster
    Write-Host "Start the MapReduce job..." -ForegroundColor Green
    $wordCountJob = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $wordCountJobDefinition `
        -HttpCredential $creds

    #Wait for the job to complete
    Write-Host "Wait for the job to complete..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds
    # Download the output
    Get-AzureStorageBlobContent `
        -Blob 'example/data/WordCountOutput/part-r-00000' `
        -Container $container `
        -Destination output.txt `
        -Context $context
    # Print the output of the job.
    Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds
    ```

2. Abra um novo prompt de comando do **PowerShell do Azure** . Altere os diretórios para o local do arquivo **mapreducejob.ps1** e use o seguinte comando para executar o script:
   
        .\mapreducejob.ps1
   
    Quando você executa o script, o nome do cluster do HDInsight e o nome da conta de HTTPS/Admin e a senha para o cluster são solicitados. Você também poderá receber uma solicitação para autenticar a sua assinatura do Azure.

3. Quando o trabalho for concluído, você receberá uma saída semelhante ao seguinte:
    
        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071
    
    Essa saída indica que o trabalho foi concluído com êxito.
    
    > [!NOTE]
    > Se o **ExitCode** for um valor diferente de 0, consulte [Solução de problemas](#troubleshooting).
    
    Este exemplo também armazenará os arquivos baixados em um arquivo **output.txt** no diretório no qual você executar o script.

### <a name="view-output"></a>Exibir saída

Abra o arquivo **output.txt** em um editor de texto para ver as palavras e contagens produzidas pelo trabalho.

> [!NOTE]
> Os arquivos de saída de um trabalho MapReduce são imutáveis. Portanto, se você executar esse exemplo novamente, será necessário alterar o nome do arquivo de saída.

## <a name="a-idtroubleshootingatroubleshooting"></a><a id="troubleshooting"></a>Solucionar problemas

Se nenhuma informação for retornada quando o trabalho for concluído, um erro pode ter ocorrido durante o processamento. Para exibir informações de erro para esse trabalho, adicione o seguinte comando ao final do arquivo **mapreducejob.ps1** , depois salve o arquivo e execute-o novamente.

```powershell
# Print the output of the WordCount job.
Write-Host "Display the standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

Isso retorna as informações que foram gravadas em STDERR no servidor quando você executou o trabalho, e pode ajudar a determinar por que o trabalho está falhando.

## <a name="a-idsummaryasummary"></a><a id="summary"></a>Resumo

Como você pode ver, o PowerShell do Azure fornece uma maneira fácil de executar trabalhos MapReduce em um cluster HDInsight, monitorar o status do trabalho e recuperar a saída.

## <a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Próximas etapas

Para obter informações gerais sobre trabalhos de MapReduce no HDInsight:

* [Usar o MapReduce no Hadoop do HDInsight](hdinsight-use-mapreduce.md)

Para obter informações sobre outros modos possíveis de trabalhar com Hadoop no HDInsight:

* [Usar o Hive com Hadoop no HDInsight](hdinsight-use-hive.md)
* [Usar o Pig com Hadoop no HDInsight](hdinsight-use-pig.md)




<!--HONumber=Jan17_HO3-->


