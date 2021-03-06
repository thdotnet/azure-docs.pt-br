---
title: "Introdução ao Apache Storm no HDInsight | Microsoft Docs"
description: "Obtenha uma introdução ao Apache Storm no HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 72d54080-1e48-4a5e-aa50-cce4ffc85077
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/11/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 24d86e17a063164c31c312685c0742ec4a5c2f1b
ms.openlocfilehash: 354292f51d07bff00e7a6811e4d1d7beb7844920
ms.lasthandoff: 03/11/2017


---
# <a name="introduction-to-apache-storm-on-hdinsight-real-time-analytics-for-hadoop"></a>Introdução ao Apache Storm no HDInsight: análise em tempo real para o Hadoop

O Apache Storm no HDInsight permite que você crie soluções de análise em tempo real distribuídas no Azure.

O Apache Storm é um sistema de computação distribuído e tolerante a falhas, sendo um software livre, que permite processar dados em tempo real com o Hadoop. As soluções do Storm também podem oferecer um processamento de dados garantido, com a capacidade de reproduzir dados que não tenham sido processados com sucesso da primeira vez.

## <a name="why-use-storm-on-hdinsight"></a>Por que usar o Storm no HDInsight

O Apache Storm no HDInsight é um cluster gerenciado integrado ao ambiente do Azure. O Storm e outros componentes do Hadoop no HDInsight baseiam-se no HDP (Hortonworks Data Platform), enquanto o sistema operacional para o cluster é o Ubuntu (uma distribuição do Linux). Essa configuração fornece uma plataforma compatível com ferramentas e serviços populares no ecossistema do Hadoop.

> [!IMPORTANT]
> O Linux é o único sistema operacional usado no HDInsight versão 3.4 ou superior. Para saber mais, veja [Substituição do HDInsight no Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

O Apache Storm no HDInsight oferece os seguintes benefícios:

* Executa como um serviço gerenciado com um SLA de 99,9% de tempo de atividade.

* Personalização fácil executando scripts em relação ao cluster durante ou após a criação. Para obter mais informações, confira [Personalizar clusters HDInsight usando a ação de script](hdinsight-hadoop-customize-cluster-linux.md).

* Use a linguagem de sua escolha: os componentes do Storm podem ser escritos em várias linguagens, como **Java**, **C#** e **Python**.
  
  * Integração do Visual Studio ao HDInsight para desenvolvimento, gerenciamento e monitoramento de topologias em C#. Para obter mais informações, confira [Desenvolver topologias Storm C# com as Ferramentas do HDInsight para Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

  * Dá suporte à interface Java **Trident**. Essa interface permite que você crie topologias Storm que dão suporte ao processamento de mensagens "exatamente uma vez", à persistência de armazenamento de dados "transacional" e a um conjunto de operações de análise de fluxo comuns.

* Fácil ampliação e redução do cluster: adicione ou remova nós de trabalho sem afetar a execução de topologias Storm.

* Integre com os seguintes serviços do Azure:
  
    * Hubs de evento
    * Rede Virtual
    * Banco de Dados SQL
    * Armazenamento do Azure
    * do Azure.
  
  * Combine de forma segura as funcionalidades de vários clusters HDInsight usando a Rede Virtual do Azure: crie pipelines analíticas que usam clusters Hadoop, HBase ou HDInsight.

Para obter uma lista de empresas que estão usando o Apache Storm em suas soluções de análise em tempo real, consulte [Empresas que estão usando o Apache Storm](https://storm.apache.org/documentation/Powered-By.html).

Para começar a usar o Storm, consulte [Introdução ao uso do Storm no HDInsight][gettingstarted].

### <a name="ease-of-creation"></a>Fácil de criar

Você pode provisionar um novo Storm no cluster HDInsight em minutos. Especifique o nome do cluster, o tamanho, a conta de administrador e a conta de armazenamento. O Azure cria o cluster com topologias de exemplo e um painel de gerenciamento da Web.

> [!NOTE]
> Você também pode provisionar os clusters Storm usando a [CLI do Azure](../xplat-cli-install.md) ou o [Azure PowerShell](/powershell/azureps-cmdlets-docs).

Após 15 minutos do envio da solicitação, você terá um novo cluster Storm em execução e pronto para o primeiro pipeline de análise em tempo real.

### <a name="ease-of-use"></a>Fácil de uso

* __Proteger a conectividade do Shell__: você pode acessar os nós principais do cluster HDInsight pela Internet usando o SSH. O SSH permite que você execute comandos diretamente no cluster.

  Para obter mais informações, confira [Usar SSH com HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

* __Conectividade da Web__: clusters HDInsight fornecem a interface da Web do Ambari. A interface de usuário da Web do Ambari permite que você monitore, configure e gerencie facilmente serviços no cluster. O Storm no HDInsight também fornece a interface do usuário do Storm, que permite que você monitore e gerencie topologias Storm em execução no navegador.

  Para obter mais informações, confira [Administrar o HDInsight usando a interface do usuário do Ambari](hdinsight-hadoop-manage-ambari.md) e [Monitorar e gerenciar usando a interface do usuário do Storm](hdinsight-storm-deploy-monitor-topology-linux.md#monitor-and-manage-using-the-storm-ui).

* __Azure PowerShell e CLI__: o Azure PowerShell e a CLI do Azure fornecem utilitários de linha de comando que você pode usar no sistema cliente para trabalhar com o HDInsight e outros serviços do Azure.

* __Integração do Visual Studio__: o Data Lake Tools para Visual Studio inclui modelos de projeto para criar topologias Storm em C#, bem como ferramentas para monitorar Storm no HDInsight. Você pode criar, implantar, monitorar e gerenciar topologias em C# do Visual Studio.

  Para obter mais informações, confira [Desenvolver topologias Storm C# com as Ferramentas do HDInsight para Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

* __Integração a outros serviços do Azure__

  * Para desenvolvimento em __Java__, a Microsoft usa componentes Storm existentes para integração a outros serviços do Azure sempre que possível. Em alguns casos, pode ser necessário um componente específico do serviço ou uma solução.

    * __Azure Data Lake Store__: topologias em Java podem acessar o Data Lake Store usando o bolt do Storm HDFS com um esquema de URI de `adl://`. Para obter um exemplo de uso do bolt do Storm HDFS, confira [Uso do Azure Data Lake Store com Apache Storm no HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-write-data-lake-store).

    * __Armazenamento do Azure__ (quando usado como armazenamento para HDInsight): topologias em Java podem acessar o Armazenamento do Azure usando o bolt do Storm HDFS com um esquema de URI de `wasb://`.

    * __Hubs de Eventos do Azure__: podem ser acessados usando os componentes EventHubSpout e EventHubBolt fornecidos pela Microsoft. Esses componentes são escritos em Java e fornecidos como um arquivo .jar autônomo.

    Para obter mais informações sobre como desenvolver soluções em Java, confira [Desenvolver uma topologia baseada em Java para Storm no HDInsight](hdinsight-storm-develop-java-topology.md).

  * Para desenvolvimento em __C#__, geralmente você pode usar o SDK do .NET para o serviço do Azure. Em alguns casos, o SDK pode contar com estruturas que não estão disponíveis no Linux (o sistema operacional host para HDInsight 3.4 e superior.) Nesse caso, você pode usar componentes Java dentro de sua solução em C#.

    * Exemplos para trabalhar com __banco de dados SQL__, __DocumentDB__, __EventHub__ e __HBase__ são incluídos como modelos no Azure Data Lake Tools for Visual Studio. Para saber mais,confira [Desenvolver uma topologia em C# para Storm no HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).

    * __Hubs de Eventos do Azure__: para obter um exemplo do uso de componentes Java de uma solução em C#, confira [Processar eventos de Hubs de Eventos do Azure com Storm no HDInsight (C#)](hdinsight-storm-develop-csharp-event-hub-topology.md).

    Para obter mais informações sobre como desenvolver soluções em C#, confira [Desenvolver uma topologia em C# para Storm no HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).

### <a name="reliability"></a>Confiabilidade

O Apache Storm sempre faz com que cada mensagem de entrada seja totalmente processada, mesmo quando a análise de dados é difundida por centenas de nós.

O **nó Nimbus** fornece uma funcionalidade semelhante ao Hadoop JobTracker e atribui tarefas a outros nós no cluster por meio do **Zookeeper**. Os Nós do Zookeeper fazem a coordenação do cluster e facilitam a comunicação entre o Nimbus e o processo **Supervisor** nos nós de trabalho. Quando um nó de processamento falha, o nó Nimbus é informado e atribui a tarefa e os dados associados a outro nó.

A configuração padrão do Apache Storm é ter apenas um nó Nimbus. O Storm no HDInsight executa dois nós Nimbus. Se o nó primário falhar, o cluster HDInsight alternará para o nó secundário enquanto o nó primário é recuperado.

![Diagrama do Nimbus, Zookeeper e Supervisor](./media/hdinsight-storm-overview/nimbus.png)

### <a name="scale"></a>Escala

Embora você possa especificar o número de nós no cluster durante a criação, convém aumentar ou reduzir o cluster de acordo com a carga de trabalho. Todos os clusters HDInsight permitem que você altere o número de nós no cluster, mesmo durante o processamento de dados.

> [!NOTE]
> Para tirar proveito dos novos nós adicionados por meio do dimensionamento, você precisa balancear novamente as topologias iniciadas antes do tamanho do cluster ser aumentado.

### <a name="support"></a>Suporte

O Storm no HDInsight é fornecido com suporte completo de nível empresarial 24 horas por dia, 7 dias por semana. O Storm no HDInsight também tem um SLA de 99,9%. Isso significa que garantimos que o cluster terá conectividade externa em, no mínimo, 99,9% do tempo.

## <a name="common-use-cases-for-real-time-analytics"></a>Casos de uso comuns de análise em tempo real

Abaixo estão alguns cenários comuns em que você pode usar o Apache Storm no HDInsight. Para saber mais sobre cenários reais, confira [Como as empresas estão usando o Storm](https://storm.apache.org/documentation/Powered-By.html).

* Internet das coisas (IoT)
* Detecção de fraude
* Análise das redes sociais
* ETL (extrair, transformar e carregar)
* Monitoramento de rede
* Pesquisar
* Mobile Engagement

## <a name="how-is-data-in-hdinsight-storm-processed"></a>Como os dados no HDInsight Storm são processados

O Apache Storm processa **topologias** em vez dos trabalhos do MapReduce com que você pode estar familiarizado no HDInsight ou no Hadoop. Um Storm no cluster HDInsight contém dois tipos de nós: os nós principais que executam o **Nimbus** e nós de trabalho que executam o **Supervisor**.

* **Nimbus**: semelhante ao JobTracker no Hadoop, é responsável por distribuir o código no cluster, atribuindo tarefas às máquinas virtuais e monitorando falhas. O HDInsight oferece dois nós Nimbus, de forma que não haja um único ponto de falha para Storm no HDInsight
* **Supervisor**: O supervisor de cada nó de trabalho é responsável por iniciar e interromper os **processos de trabalho** no nó.
* **Processo de trabalho**: executa um subconjunto de uma **topologia**. Uma topologia em execução é distribuída entre vários processos de trabalho no cluster.
* **Topologia**: define um gráfico de computação que processa os **fluxos** de dados. Diferentemente dos trabalhos do MapReduce, as topologias são executadas até que você as interrompa.
* **Fluxo**: uma coleção não vinculada de **tuplas**. Os fluxos são produzidos por **spouts** e **bolts**, e eles serão consumidos por **bolts**.
* **Tupla**: uma lista nomeada de valores inseridos dinamicamente.
* **Spout**: consome os dados de uma fonte de dados e emite um ou mais **fluxos**.
  
  > [!NOTE]
  > Frequentemente, os dados são lidos de uma fila, como hubs Kafka ou Evento do Azure. Se houver uma interrupção, a fila assegurará que os dados sejam persistidos.

* **Bolt**: consome os **fluxos**, faz o processamento nas **tuplas** e pode emitir **fluxos**. Os bolts também são responsáveis por gravar dados em armazenamentos externos, como uma fila, o HBase, o HDInsight, um blob ou outro armazenamento de dados.
* **Apache Thrift**: é uma estrutura de software para o desenvolvimento de serviço escalável em qualquer idioma. Ele permite que você compile serviços que funcionam entre C++, Java, Python, PHP, Ruby, Erlang, Perl, Haskell, C#, Cocoa, JavaScript, Node.js, Smalltalk e outras linguagens.

Para obter mais informações sobre os componentes do Storm, consulte o [Tutorial do Storm][apachetutorial] em apache.org.

## <a name="what-programming-languages-can-i-use"></a>Quais linguagens de programação posso usar

### <a name="c35"></a>C&#35;

As Ferramentas do Data Lake para Visual Studio permitem que os desenvolvedores do .NET projetem e implementem uma topologia em C#. Você também pode criar topologias híbridas que usam componentes Java e C#.

Para obter mais informações, consulte [Desenvolver topologias C# para o Apache Storm no HDInsight usando o Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).

### <a name="java"></a>Java

A maioria dos exemplos com Java encontrados são Java ou Trident simples. Trident é uma abstração de alto nível que torna mais fácil realizar coisas como junções, agregações, agrupamentos e filtragens. No entanto, o Trident age em lotes de tuplas, ao passo que uma solução bruta em Java processa um fluxo com uma tupla de cada vez.

Para obter mais informações sobre o Trident, consulte o [Tutorial do Trident](https://storm.apache.org/documentation/Trident-tutorial.html) em apache.org.

Para obter exemplos de topologias Java e Trident, consulte o [lista de exemplo de topologias Storm](hdinsight-storm-example-topology.md) ou os exemplos Storm-Starter no seu cluster HDInsight.

Os exemplos de storm-starter estão localizados no diretório ** /usr/hdp/current/storm-client/contrib/storm-starter** no cluster HDInsight.

### <a name="python"></a>Python

Para obter um exemplo do uso de componentes de Python, consulte [Desenvolver topologias Storm usando Python no HDInsight](hdinsight-storm-develop-python-topology.md).

## <a name="what-are-some-common-development-patterns"></a>Quais são alguns dos padrões de desenvolvimento comuns

### <a name="guaranteed-message-processing"></a>Processamento de mensagem garantido

O Storm pode oferecer diferentes níveis de processamento de mensagem garantido. Por exemplo, um aplicativo básico Storm pode garantir um processamento pelo menos uma vez e o Trident pode garantir o processamento exatamente uma vez.

Para obter mais informações, consulte [Garantias do processamento de dados](https://storm.apache.org/about/guarantees-data-processing.html) em apache.org.

### <a name="ibasicbolt"></a>IBasicBolt

O padrão de leitura de uma tupla de entrada, emitindo zero ou mais tuplas, e depois confirmando a tupla de entrada imediatamente no final do método de execução é comum. O Storm oferece a interface [IBasicBolt](https://storm.apache.org/apidocs/backtype/storm/topology/IBasicBolt.html) para automatizar esse padrão.

### <a name="joins"></a>Junções

O modo como os fluxos de dados são unidos varia entre aplicativos. Por exemplo, você pode juntar cada tupla de vários fluxos em um novo fluxo ou pode juntar somente lotes de tuplas para uma janela específica. De qualquer forma, a junção pode ser realizada usando [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), que é uma maneira de definir como as tuplas são roteadas aos bolts.

No exemplo de Java a seguir, fieldsGrouping é usado para rotear tuplas provenientes dos componentes "1", "2" e "3" para o bolt **MyJoiner** .

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

### <a name="batching"></a>Envio em lote

O Storm oferece um mecanismo de cronometragem interno conhecido como "tupla em escala", que pode ser usado para emitir um lote a cada X segundos.

Para obter um exemplo de como usar uma tupla de escala de um componente em C#, confira [PartialBoltCount.cs](https://github.com/hdinsight/hdinsight-storm-examples/blob/3b2c960549cac122e8874931df4801f0934fffa7/EventCountExample/EventCountTopology/src/main/java/com/microsoft/hdinsight/storm/examples/PartialCountBolt.java).

Se estiver usando o Trident, ele se baseia no processamento de lotes de tuplas.

### <a name="caching"></a>Cache

O cache na memória geralmente é usado como um mecanismo para acelerar o processamento, uma vez que mantém os ativos usados com mais frequência na memória. Como uma topologia é distribuída entre vários nós, e entre vários processos dentro de cada nó, considere o uso de [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29). `fieldsGrouping` garante que as tuplas contendo os campos que são usados para pesquisa em cache sejam sempre roteadas para o mesmo processo. Essa funcionalidade de agrupamento evita a duplicação de entradas de cache entre os processos.

### <a name="streaming-top-n"></a>Transmitindo os principais valores N

Quando a sua topologia depende do cálculo de um valor “N principal”, você deve calcular o valor N principal paralelamente e depois mesclar o resultado desses cálculos em um valor global. Isso pode ser feito usando [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) para rotear por campo para processamento em paralelo e depois rotear para um bolt que determina globalmente o valor N principal.

Para obter um exemplo do cálculo de um valor de "N principal", consulte o exemplo [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java) .

## <a name="what-type-of-logging-does-storm-use"></a>Que tipo de registro em log o Storm utiliza

O Storm usa o Apache Log4j para registrar informações em log. Por padrão, uma grande quantidade de dados é registrada e pode ser difícil classificar as informações. Você pode incluir um arquivo de configuração de log como parte de sua topologia do Storm para controlar o comportamento de log.

Para uma topologia de exemplo que demonstra como configurar o log, veja o exemplo [WordCount baseado em Java](hdinsight-storm-develop-java-topology.md) para Storm no HDInsight.

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre as soluções de análise em tempo real com o Apache Storm no HDInsight:

* [Introdução ao Storm no HDInsight][gettingstarted]
* [Topologias de exemplo para Storm no HDInsight](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md

