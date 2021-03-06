---
title: Usar o Pig do Hadoop com .NET no HDInsight | Microsoft Docs
description: Aprenda a usar o SDK do .NET do Hadoop para enviar trabalhos do Pig para Hadoop no HDInsight.
services: hdinsight
documentationcenter: .net
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: fa11d49a-328c-47e7-b16d-e7ed2a453195
ms.service: hdinsight
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/03/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 7c28fda22a08ea40b15cf69351e1b0aff6bd0a95
ms.openlocfilehash: db01dd7ff7590c6cb43b5f8bc39674f585160856
ms.lasthandoff: 03/07/2017


---
# <a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Executar trabalhos do Pig usando o SDK do .NET para Hadoop no HDInsight
[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Este documento fornece um exemplo de uso do SDK do .NET para Hadoop para enviar trabalhos do Pig para um Hadoop no cluster HDInsight.

O SDK do .NET do HDInsight fornece bibliotecas de cliente .NET que facilitam o trabalho com clusters HDInsight no .NET. O Pig permite que você crie operações MapReduce ao modelar uma série de transformações de dados. Neste documento, você aprenderá como usar um aplicativo C# básico para enviar um trabalho do Pig para um cluster do HDInsight.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir as etapas deste artigo, você precisa do seguinte.

* Um cluster do Azure HDInsight (Hadoop no HDInsight) (Windows ou Linux).

  > [!IMPORTANT]
  > O Linux é o único sistema operacional usado no HDInsight versão 3.4 ou superior. Para saber mais, veja [Substituição do HDInsight no Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* Visual Studio 2012, 2013, 2015 ou 2017.

## <a name="create-the-application"></a>Criar o aplicativo

O SDK do .NET do HDInsight fornece bibliotecas de cliente .NET que facilitam o trabalho com clusters HDInsight do .NET. 

1. No Visual Studio, no menu **Arquivo**, selecione **Novo** e **Projeto**.

2. Para o novo projeto, digite ou selecione os valores a seguir:
   
   | Propriedade | Valor |
   | ------ | ------ |
   | Categoria | Modelos/Visual C#/Windows |
   | Modelo | Aplicativo de console |
   | Nome | SubmitPigJob |

3. Clique em **OK** para criar o projeto.

4. No menu **Ferramentas**, selecione **Gerenciador de Pacotes da Biblioteca** ou **Gerenciador de Pacotes Nuget** e depois selecione **Console do Gerenciador de Pacotes**.

5. Execute o seguinte comando para instalar os pacotes do SDK do .NET:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. No Gerenciador de Soluções, clique duas vezes em **Program.cs** para abri-lo. Substitua o código existente pelo seguinte.
   
    ```csharp
    using Microsoft.Azure.Management.HDInsight.Job;
    using Microsoft.Azure.Management.HDInsight.Job.Models;
    using Hyak.Common;

    namespace SubmitHDInsightJobDotNet
    {
        class Program
        {
            private static HDInsightJobManagementClient _hdiJobManagementClient;

            private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
            private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
            private const string ExistingClusterUsername = "<Cluster Username>";
            private const string ExistingClusterPassword = "<Cluster User Password>";

            static void Main(string[] args)
            {
                System.Console.WriteLine("The application is running ...");

                var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                SubmitPigJob();

                System.Console.WriteLine("Press ENTER to continue ...");
                System.Console.ReadLine();
            }

            private static void SubmitPigJob()
            {
                var parameters = new PigJobSubmissionParameters
                {
                    Query = @"LOGS = LOAD '/example/data/sample.log';
                                LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                RESULT = order FREQUENCIES by COUNT desc;
                                DUMP RESULT;"
                };

                System.Console.WriteLine("Submitting the Pig job to the cluster...");
                var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                System.Console.WriteLine("Validating that the response is as expected...");
                System.Console.WriteLine("Response status code is " + response.StatusCode);
                System.Console.WriteLine("Validating the response object...");
                System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
            }
        }
    }
    ```
    
7. Pressione **F5** para iniciar o aplicativo.

8. Pressione **ENTER** para sair do aplicativo.

## <a name="summary"></a>Resumo

Como você pode ver, o SDK para .NET do Hadoop permite criar aplicativos .NET que enviam trabalhos do Pig para um cluster HDInsight e monitorar o status do trabalho.

## <a name="next-steps"></a>Próximas etapas

Para obter informações sobre o Pig no HDInsight, consulte [Usar o Pig com Hadoop no HDInsight](hdinsight-use-pig.md).

Para saber mais sobre como usar o Hadoop no HDInsight, veja os seguintes documentos:

* [Usar o Hive com Hadoop no HDInsight](hdinsight-use-hive.md)
* [Usar o MapReduce com Hadoop no HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/

