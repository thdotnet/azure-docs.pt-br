---
title: Dimensionar tamanhos de cluster – Azure HDInsight
description: Dimensione um cluster Azure HDInsight elasticamente para corresponder a sua carga de trabalho.
author: ashishthaps
ms.author: ashish
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/13/2019
ms.openlocfilehash: 622261d0f7e602635aa6a638357278a9c63a6ecd
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65990503"
---
# <a name="scale-hdinsight-clusters"></a>Dimensionar clusters HDInsight

O HDInsight proporciona elasticidade, oferecendo a opção de escalar e reduzir verticalmente o número de nós de trabalho em seus clusters. Este elasticidade, permite que você reduza um cluster depois de horas ou nos finais de semana e o expanda durante picos de demanda corporativa.

Se você tiver o processamento em lotes periódicas, o cluster HDInsight pode ser escalado verticalmente alguns minutos antes da operação, para que o cluster tem memória suficiente e potência da CPU.  Posteriormente, depois que o processamento for concluído e uso reduzir novamente, você poderá reduzir verticalmente o cluster HDInsight, obtendo menos nós de trabalho.

Você pode dimensionar um cluster manualmente usando um dos métodos descritos abaixo ou usar [AutoEscala](hdinsight-autoscale-clusters.md) opções para que o sistema automaticamente aumentar e diminuir em resposta a CPU, memória e outras métricas.

## <a name="utilities-to-scale-clusters"></a>Utilitários para dimensionar clusters

A Microsoft fornece os seguintes utilitários para dimensionar clusters:

|Utilitário | Descrição|
|---|---|
|[PowerShell Az](https://docs.microsoft.com/powershell/azure)|[Conjunto AzHDInsightClusterSize](https://docs.microsoft.com/powershell/module/az.hdinsight/set-azhdinsightclustersize) - ClusterName \<nome do Cluster > - TargetInstanceCount \<NewSize >|
|[PowerShell AzureRM](https://docs.microsoft.com/powershell/azure/azurerm) |[Conjunto AzureRmHDInsightClusterSize](https://docs.microsoft.com/powershell/module/azurerm.hdinsight/set-azurermhdinsightclustersize) - ClusterName \<nome do Cluster > - TargetInstanceCount \<NewSize >|
|[CLI do Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)| [hdinsight AZ redimensionar](https://docs.microsoft.com/cli/azure/hdinsight?view=azure-cli-latest#az-hdinsight-resize) – grupo de recursos \<grupo de recursos > – nome \<nome do Cluster >-contagem de instância de destino \<NewSize >|
|[CLI do Azure](hdinsight-administer-use-command-line.md)|redimensionamento do cluster hdinsight do Azure \<clusterName > \<contagem de instâncias de destino > |
|[Portal do Azure](https://portal.azure.com)|Abra o painel do cluster HDInsight, selecione **tamanho do Cluster** no menu à esquerda, em seguida, no painel de tamanho do Cluster, digite o número de nós de trabalho e selecione Salvar.|  

![Dimensionar cluster](./media/hdinsight-scaling-best-practices/scale-cluster-blade.png)

Usando qualquer um desses métodos, você pode aumentar ou reduzir verticalmente seu cluster HDInsight em apenas alguns minutos.

> [!IMPORTANT]  
> * A CLI do Azure clássica é preterida e só deve ser usada com o modelo de implantação clássico. Para todas as outras implantações, use o [CLI do Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).  
> * O módulo PowerShell AzureRM foi preterido.  Use o [módulo Az](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.4.0) sempre que possível.

## <a name="impact-of-scaling-operations"></a>Impacto de operações de dimensionamento

Quando você **adicionar** nós em seu cluster em execução HDInsight (escalar verticalmente), todos os trabalhos pendentes ou em execução não serão afetados. Novos trabalhos podem ser enviados com segurança enquanto o processo de colocação em escala está em execução. Se a operação de dimensionamento falhar por algum motivo, a falha será tratada para deixar seu cluster em um estado funcional.

Se você **remover** nós (escala para baixo), todos os trabalhos pendentes ou em execução serão falhar quando a operação de dimensionamento for concluído. Essa falha ocorre devido a alguns dos serviços reiniciar durante o processo de dimensionamento. Também há um risco de que seu cluster pode obter preso no modo de segurança durante uma operação de dimensionamento de manual.

## <a name="how-to-safely-scale-down-a-cluster"></a>Como dimensionar com segurança verticalmente um cluster

### <a name="scale-down-a-cluster-with-running-jobs"></a>Reduzir verticalmente um cluster com a execução de trabalhos

Para evitar que os trabalhos em execução falhar durante a operação de redução, você pode tentar três coisas:

1. Aguarde até que os trabalhos concluir antes de reduzir verticalmente o cluster.
1. Encerre manualmente os trabalhos.
1. Reenvie os trabalhos após a conclusão da operação de escala.

Para ver uma lista de pendentes e trabalhos em execução, você pode usar o YARN **ResourceManager UI**, siga estas etapas:

1. Entre no [Portal do Azure](https://portal.azure.com).
2. Da esquerda, navegue até **todos os serviços** > **Analytics** > **Clusters de HDInsight**e, em seguida, selecione seu cluster.
3. No modo de exibição principal, navegue até **painéis do Cluster** > **Ambari base**. Insira suas credenciais do cluster.
4. Na UI do Ambari, selecione **YARN** na lista de serviços no menu à esquerda.  
5. Na página do YARN, selecione **Links rápidos** e passe o mouse sobre o nó principal ativo, em seguida, selecione **UI ResourceManager**.

    ![Interface do usuário do ResourceManager](./media/hdinsight-scaling-best-practices/resourcemanager-ui.png)

Você pode acessar diretamente a interface do usuário do ResourceManager com `https://<HDInsightClusterName>.azurehdinsight.net/yarnui/hn/cluster`.

Você vê uma lista de trabalhos, com os respectivos estados atuais. Na captura de tela, há um trabalho em execução no momento:

![Aplicativos da interface do usuário do ResourceManager](./media/hdinsight-scaling-best-practices/resourcemanager-ui-applications.png)

Para eliminar manualmente o aplicativo que está em execução, execute o seguinte comando no shell SSH:

```bash
yarn application -kill <application_id>
```

Por exemplo:

```bash
yarn application -kill "application_1499348398273_0003"
```

### <a name="getting-stuck-in-safe-mode"></a>Fique preso no modo de segurança

Quando você escala verticalmente um cluster, o HDInsight usa interfaces de gerenciamento do Apache Ambari para primeiro desativar os nós de trabalho extras, o qual replicam seus blocos do HDFS para outros nós de trabalho online. Depois disso, o HDInsight com segurança dimensiona o cluster para baixo. HDFS entra em modo de segurança durante a operação de dimensionamento e deveria sair quando o dimensionamento fosse concluído. Em alguns casos, no entanto, HDFS fica preso no modo de segurança durante uma operação de dimensionamento devido à replicação insuficiente do bloco de arquivos.

Por padrão, o HDFS está configurado com um `dfs.replication` configuração 3, que controla quantas cópias de cada bloco de arquivo estão disponíveis. Cada cópia de um bloco de arquivo é armazenada em um nó diferente do cluster.

Quando o HDFS detecta que o número esperado de cópias de bloco não está disponível, o HDFS entra em modo de segurança e Ambari gera alertas. Se o HDFS entra em modo de segurança para uma operação de dimensionamento, mas, em seguida, não é possível sair do modo de segurança porque o número necessário de nós não é detectado para replicação, o cluster pode ficar preso no modo de segurança.

### <a name="example-errors-when-safe-mode-is-turned-on"></a>Erros de exemplo quando o modo de segurança está ativado

```
org.apache.hadoop.hdfs.server.namenode.SafeModeException: Cannot create directory /tmp/hive/hive/819c215c-6d87-4311-97c8-4f0b9d2adcf0. Name node is in safe mode.
```

```
org.apache.http.conn.HttpHostConnectException: Connect to hn0-clustername.servername.internal.cloudapp.net:10001 [hn0-clustername.servername. internal.cloudapp.net/1.1.1.1] failed: Connection refused
```

Você pode examinar os logs de nó de nome na pasta `/var/log/hadoop/hdfs/`, próximos ao horário em que o cluster foi dimensionado, para ver quando ele entrou em modo de segurança. Os arquivos de log são nomeados `Hadoop-hdfs-namenode-hn0-clustername.*`.

A causa raiz dos erros anteriores é que o Hive depende de arquivos temporários do HDFS durante a execução de consultas. Quando o HDFS entra em modo de segurança, o Hive não consegue executar consultas porque não pode gravar no HDFS. Os arquivos temporários do HDFS ficam localizados na unidade local, montada para as VMs de cada nó de trabalho e replicados entre os outros nós de trabalho em, no mínimo, três réplicas.

### <a name="how-to-prevent-hdinsight-from-getting-stuck-in-safe-mode"></a>Como impedir que o HDInsight fique preso no modo de segurança

Há várias maneiras de impedir que o HDInsight permaneça no modo de segurança:

* Pare todos os trabalhos do Hive antes de reduzir verticalmente o HDInsight. Como alternativa, agende o processo de redução vertical para evitar conflito com trabalhos do Hive em execução.
* Limpe manualmente os arquivos do diretório `tmp` temporários do Hive no HDFS antes de reduzir verticalmente.
* Reduza verticalmente o HDInsight somente até o mínimo de três nós de trabalho. Evite chegar até somente um nó de trabalho.
* Execute o comando para sair do modo de segurança, se necessário.

As seções a seguir descrevem essas opções.

#### <a name="stop-all-hive-jobs"></a>Interromper todos os trabalhos do Hive

Interrompa todos os trabalhos do Hive antes de reduzir verticalmente a um nó de trabalho. Se sua carga de trabalho é agendada, execute a redução vertical após a conclusão do trabalho do Hive.

Parando trabalhos do Hive antes do dimensionamento, ajuda a minimizar o número de arquivos temporários na pasta tmp (se houver).

#### <a name="manually-clean-up-hives-scratch-files"></a>Limpar manualmente os arquivos temporários do Hive

Se o Hive deixou arquivos temporários na pasta, você pode limpar manualmente esses arquivos antes de reduzir verticalmente, evitando o modo de segurança.

1. Verificar qual local está sendo usada para arquivos temporários do Hive, observando o `hive.exec.scratchdir` propriedade de configuração. Esse parâmetro é definido dentro do `/etc/hive/conf/hive-site.xml`:

    ```xml
    <property>
        <name>hive.exec.scratchdir</name>
        <value>hdfs://mycluster/tmp/hive</value>
    </property>
    ```

1. Interrompa os serviços do Hive e verifique se todas as consultas e os trabalhos foram concluídos.
2. Listar o conteúdo do diretório transitório encontrado acima, `hdfs://mycluster/tmp/hive/` para ver se ele contém todos os arquivos:

    ```
    hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    ```

    Aqui está um exemplo de saída quando há arquivos:

    ```
    sshuser@hn0-scalin:~$ hadoop fs -ls -R hdfs://mycluster/tmp/hive/hive
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c
    drwx------   - hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/_tmp_space.db
    -rw-r--r--   3 hive hdfs         27 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.info
    -rw-r--r--   3 hive hdfs          0 2017-07-06 13:40 hdfs://mycluster/tmp/hive/hive/4f3f4253-e6d0-42ac-88bc-90f0ea03602c/inuse.lck
    drwx------   - hive hdfs          0 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699
    -rw-r--r--   3 hive hdfs         26 2017-07-06 20:30 hdfs://mycluster/tmp/hive/hive/c108f1c2-453e-400f-ac3e-e3a9b0d22699/inuse.info
    ```

3. Se souber que Hive terminou de usar esses arquivos, você poderá removê-los. Verifique se o Hive não tem nenhuma consulta em execução examinando a página de interface do usuário do Yarn ResourceManager.

    Linha de comando de exemplo para remover arquivos do HDFS:

    ```
    hadoop fs -rm -r -skipTrash hdfs://mycluster/tmp/hive/
    ```

#### <a name="scale-hdinsight-to-three-or-more-worker-nodes"></a>Escala HDInsight para três ou mais nós de trabalho

Se os clusters de ficar preso no modo de segurança com frequência ao dimensionar para menos de três nós de trabalho e as etapas anteriores não funcionarem, você pode evitar que seu cluster ficar modo de segurança completamente, mantendo a pelo menos três nós de trabalho.

Manter três nós de trabalho é mais caro do que reduzir verticalmente a apenas um nó de trabalho, mas ele impedirá que o cluster fique preso no modo de segurança.

#### <a name="run-the-command-to-leave-safe-mode"></a>Executar o comando para sair do modo de segurança

A opção final é executar o comando de modo de segurança deixar. Se você souber que o motivo para entrar no modo de seguro de HDFS é devido à replicação insuficiente do arquivo de Hive, você pode executar o seguinte comando para sair do modo de seguro:


```bash
hdfs dfsadmin -D 'fs.default.name=hdfs://mycluster/' -safemode leave
```

### <a name="scale-down-an-apache-hbase-cluster"></a>Reduzir verticalmente um cluster do Apache HBase

Servidores de região são balanceados automaticamente em alguns minutos depois de concluir uma operação de dimensionamento. Para balancear manualmente servidores de região, conclua as seguintes etapas:

1. Conecte-se ao cluster HDInsight usando SSH. Para obter mais informações, confira [Usar SSH com HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Inicie o shell do HBase:

    ```bash
    hbase shell
    ```

3. Use o seguinte comando para balancear manualmente os servidores de região:

    ```bash
    balancer
    ```

## <a name="next-steps"></a>Próximas etapas

* [Dimensionar automaticamente os clusters de HDInsight do Azure](hdinsight-autoscale-clusters.md)
* [Introdução ao Azure HDInsight](hadoop/apache-hadoop-introduction.md)
* [Dimensionar clusters](hdinsight-administer-use-portal-linux.md#scale-clusters)
