---
title: Ferramentas de HDInsight do Azure - usar o Visual Studio Code para o Hive, LLAP ou PySpark
description: Saiba como usar as Ferramentas do Azure HDInsight para Visual Studio Code para criar e enviar consultas e scripts.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: ebc41fc74d24708a177bf554029df8384c49df05
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657244"
---
# <a name="use-azure-hdinsight-tools-for-visual-studio-code"></a>Usar a Ferramenta do Azure HDInsight para Visual Studio Code

Saiba como usar as Ferramentas do Azure HDInsight para Visual Studio Code para criar e enviar trabalho em lotes do Apache Hive, consultas interativas do Apache Hive e scripts PySpark para o Apache Spark. Em primeiro lugar, descreveremos como instalar as ferramentas do HDInsight no Visual Studio Code e, em seguida, examinaremos como enviar trabalhos para o Hive e o Spark.  

As Ferramentas do Azure HDInsight podem ser instaladas nas plataformas compatíveis com o Visual Studio Code, que incluem Windows, Linux e macOS. Abaixo você encontrará os pré-requisitos para as diferentes plataformas.


## <a name="prerequisites"></a>Pré-requisitos

Os itens a seguir são necessários para concluir as etapas neste artigo:

- Um cluster HDInsight. Para criar um cluster, consulte [Introdução ao HDInsight](hadoop/apache-hadoop-linux-create-cluster-get-started-portal.md).
- [Visual Studio Code](https://code.visualstudio.com/).
- [Mono](https://www.mono-project.com/docs/getting-started/install/). Mono só é necessário para Linux e MacOS.
- A [Extensão da Conta do Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) para o Visual Studio Code.
- [Configurar o ambiente interativo do PySpark para Visual Studio Code](set-up-pyspark-interactive-environment.md).
- Um diretório local chamado **HDexample**.  Este artigo usa **C:\HD\HDexample**.

## <a name="install-azure-hdinsight-tools"></a>Instalar as Ferramentas do Azure HDInsight

Depois de concluir os pré-requisitos, você pode instalar as Ferramentas do Azure HDInsight para Visual Studio Code.  Conclua as etapas a seguir para instalar as Ferramentas do Azure HDInsight:

1. Abra o Visual Studio Code.

2. Na barra de menus, navegue para **Exibir** > **Extensões**.

3. Digite **HDInsight** na caixa de pesquisa.

4. Selecione **Ferramentas do Azure HDInsight** nos resultados da pesquisa e selecione **Instalar**.  

   ![Instalação do Python no HDInsight para Visual Studio Code](./media/hdinsight-for-vscode/install-hdInsight-plugin.png)

5. Selecione **Recarregar** para ativar a extensão **Ferramentas do Azure HDInsight** depois da instalação.


## <a name="open-hdinsight-work-folder"></a>Abrir a pasta de trabalho do HDInsight

Conclua as etapas a seguir para abrir uma pasta de trabalho e criar um arquivo no Visual Studio Code:

1. Na barra de menus, navegue para **Arquivo** > **Abrir Pasta...**  > **C:\HD\HDexample** e, em seguida, selecione o botão **Selecionar Pasta**. A pasta aparece na exibição do **Explorador** à esquerda.

2. Na exibição do **Explorador**, selecione a pasta, **HDexample** e, em seguida, o ícone **Novo Arquivo** ao lado da pasta de trabalho.

   ![Novo arquivo](./media/hdinsight-for-vscode/new-file.png)

3. Nomeie o novo arquivo com qualquer um de `.hql` (consultas de Hive) ou o `.py` extensão de arquivo (script Spark).  Este exemplo usa **HelloWorld.hql**.

## <a name="set-the-azure-environment"></a>Configurar o ambiente do Azure

1. [Conectar-se](#connect-to-azure-account) conta do Azure ou vincular um cluster, se você ainda não tiver feito isso.

2. Na barra de menus, navegue para **Exibir** > **Paleta de Comandos...** , e insira **HDInsight: Definir Ambiente do Azure**.

3. Selecione um ambiente como sua entrada de log padrão.

4. Enquanto isso, a ferramenta já salvou sua entrada de logon padrão em **.VSCode\settings.json**. Você também pode atualizá-la diretamente nesse arquivo de configuração. 

   ![Definir a configuração de entrada de logon padrão](./media/hdinsight-for-vscode/set-default-login-entry-configuration.png)

## <a name="connect-to-azure-account"></a>Conectar-se à conta do Azure

Antes de enviar scripts para clusters do HDInsight a partir do Visual Studio Code, você precisa se conectar à sua conta do Azure ou vincular um cluster (usando nome de usuário/senha ou a conta de domínio associado do Ambari).  Conclua as seguintes etapas para se conectar ao Azure:

1. Na barra de menus, navegue para **Exibir** > **Paleta de Comandos...** , e insira **HDInsight: Logon**.

    ![Logon das Ferramentas do HDInsight para Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-extension-login.png)

2. Siga as instruções em entrar a **saída** painel.
    + Para o ambiente global do Azure, o comando **HDInsight: Login** disparará a ação **Entrar no Azure** no Gerenciador do HDInsight e vice-versa.

        ![Instruções de entrada no Azure](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-signin.png)

    + Para outros ambientes, siga as instruções de entrada.

        ![Instruções de entrada para outro ambiente](./media/hdinsight-for-vscode/hdi-azure-hdinsight-hdinsight-signin.png)

   Depois que você estiver conectado, o nome da conta do Azure será mostrado na barra de status no canto inferior esquerdo da janela do Visual Studio Code.  
  

## <a name="link-a-cluster"></a>Vincular um cluster

### <a name="link-azure-hdinsight"></a>Link: Azure HDInsight

É possível vincular um cluster normal usando um nome de usuário gerenciado do [Apache Ambari](https://ambari.apache.org/) ou vincular um cluster Hadoop seguro do pacote Enterprise Security usando um nome de usuário de domínio (como: user1@contoso.com).

1. Na barra de menus, navegue para **Exibir** > **Paleta de Comandos...** , e insira **HDInsight: Vincular um Cluster**.

   ![comando para vincular cluster](./media/hdinsight-for-vscode/link-cluster-command.png)

2. Selecione o tipo de cluster vinculado **Azure HDInsight**.

3. Insira a URL do cluster do HDInsight.

4. Insira o nome de usuário do Ambari, o padrão é **admin**.

5. Insira a senha do Ambari.

6. Selecione o tipo de cluster.

7. Verifique a exibição **SAÍDA**.

   > [!NOTE]  
   > O nome de usuário e a senha vinculados serão usados se o cluster foi registrado na assinatura do Azure e vinculou um cluster.  



### <a name="link-generic-livy-endpoint"></a>Link: Ponto de Extremidade Genérico do Livy

1. Na barra de menus, navegue para **Exibir** > **Paleta de Comandos...** , e insira **HDInsight: Vincular um Cluster**.

2. Selecione o tipo de cluster vinculado **Ponto de Extremidade Genérico do Livy**.

3. Digite o ponto de extremidade genérico do Livy, por exemplo: http\://10.172.41.42:18080.

4. Selecione o tipo de autorização, **Básico** ou **Nenhum**.  Se **Básico**, então:  
    &emsp;a. Insira o nome de usuário do Ambari, o padrão é **admin**.  
    &emsp;b. Insira a senha do Ambari.

5. Verifique a exibição **SAÍDA**.

## <a name="list-hdinsight-clusters"></a>Listar clusters HDInsight

1. Na barra de menus, navegue para **Exibir** > **Paleta de Comandos...** , e insira **HDInsight: Listar clusters**.

2. Selecione a assinatura desejada.

3. Verifique a exibição **SAÍDA**.  A exibição mostrará os clusters vinculados e todos os clusters sob sua assinatura do Azure.

    ![Definir uma configuração do cluster padrão](./media/hdinsight-for-vscode/list-cluster-result.png)

## <a name="set-default-cluster"></a>Definir cluster padrão

1. Abra de novo a pasta **HDexample** criada [anteriormente](#open-hdinsight-work-folder), se estiver fechada.  

2. Selecione o arquivo **HelloWorld.hql** criado [anteriormente](#open-hdinsight-work-folder) e ele abrirá no editor de scripts.

3. Clique com o botão direito do mouse no editor de scripts e selecione **HDInsight: Definir cluster padrão**.  

4. [Conectar-se](#connect-to-azure-account) conta do Azure ou vincular um cluster, se você ainda não tiver feito isso.

5. Selecione um cluster como o padrão para o arquivo de script atual. As ferramentas atualizam automaticamente o arquivo de configuração **.VSCode\settings.json**. 

   ![Definir configuração do cluster padrão](./media/hdinsight-for-vscode/set-default-cluster-configuration.png)


## <a name="submit-interactive-hive-queries-hive-batch-scripts"></a>Enviar consultas interativas do Hive, scripts em lotes do Hive

Com as Ferramentas do HDInsight para Visual Studio Code, você pode enviar consultas interativas e scripts em lotes do Hive a clusters do HDInsight.

1. Abra de novo a pasta **HDexample** criada [anteriormente](#open-hdinsight-work-folder), se estiver fechada.  

2. Selecione o arquivo **HelloWorld.hql** criado [anteriormente](#open-hdinsight-work-folder) e ele abrirá no editor de scripts.


3. Copie e cole o código a seguir no arquivo do Hive e depois salve-o.

    ```hiveql
    SELECT * FROM hivesampletable;
    ```

4. [Conectar-se](#connect-to-azure-account) conta do Azure ou vincular um cluster, se você ainda não tiver feito isso.

5. Clique com o botão direito do mouse no editor de scripts, selecione **HDInsight: Hive Interativo** para enviar a consulta, ou use o atalho **Ctrl + Alt + I**.  Selecione **HDInsight: Lote do Hive** para enviar o script ou usar atalho **Ctrl + Alt + H**.  

6. Selecione o cluster se você ainda não especificou um cluster padrão. As ferramentas também permitem que você envie um bloco de código, em vez do arquivo de script inteiro, usando o menu de contexto. Após alguns instantes, os resultados da consulta aparecem em uma nova guia.

   ![Resultado do Hive interativo](./media/hdinsight-for-vscode/interactive-hive-result.png)

    - Painel **RESULTADOS**: É possível salvar o resultado inteiro como um arquivo CSV, JSON ou Excel para o caminho local ou apenas selecionar várias linhas.

    - Painel **MENSAGENS**: Ao selecionar o número de **Linha**, ele salta para a primeira linha do script em execução.

## <a name="submit-interactive-pyspark-queries"></a>Enviar consultas interativas do PySpark

Você pode enviar consultas interativas do PySpark, seguindo as etapas abaixo:

1. Abra de novo a pasta **HDexample** criada [anteriormente](#open-hdinsight-work-folder), se estiver fechada.  

2. Crie um novo arquivo **HelloWorld.py** seguindo as etapas [anteriores](#open-hdinsight-work-folder).

3. Copie e cole o seguinte código no arquivo de script:
   ```python
   from operator import add
   lines = spark.read.text("/HdiSamples/HdiSamples/FoodInspectionData/README").rdd.map(lambda r: r[0])
   counters = lines.flatMap(lambda x: x.split(' ')) \
                .map(lambda x: (x, 1)) \
                .reduceByKey(add)

   coll = counters.collect()
   sortedCollection = sorted(coll, key = lambda r: r[1], reverse = True)

   for i in range(0, 5):
        print(sortedCollection[i])
   ```

4. [Conectar-se](#connect-to-azure-account) conta do Azure ou vincular um cluster, se você ainda não tiver feito isso.

5. Escolha a todo o código e clique com botão direito no editor de scripts, selecione **HDInsight: PySpark Interativo** para enviar a consulta, ou use o atalho **Ctrl+Alt+I**.

   ![Clique com botão direito do pyspark interativo](./media/hdinsight-for-vscode/pyspark-interactive-right-click.png)

6. Selecione o cluster se você ainda não especificou um cluster padrão. Após alguns instantes, o **interativa do Python resulta** aparecem em uma nova guia. As ferramentas também permitem que você envie um bloco de código, em vez do arquivo de script inteiro, usando o menu de contexto. 

   ![janela interativa do python interativo de pyspark](./media/hdinsight-for-vscode/pyspark-interactive-python-interactive-window.png) 

7. ENTER **"% % info"** , em seguida, pressione **Shift + Enter** para exibir informações de trabalho. (Opcional)

   ![Exibir informações de trabalho](./media/hdinsight-for-vscode/pyspark-interactive-view-job-information.png)

8. A ferramenta também oferece suporte a **Spark SQL** consulta.

   ![Exibir o resultado Pyspark interativo](./media/hdinsight-for-vscode/pyspark-ineteractive-select-result.png)

   O status de envio aparece à esquerda da barra de status inferior ao executar consultas. Não envie outras consultas quando o status for **PySpark Kernel (ocupado)** .  

   > [!NOTE] 
   >
   > Quando **Python extensão habilitada** está desmarcada, nas configurações do (a configuração padrão é marcada), os resultados da interação de pyspark enviado usará a janela antiga.
   >
   > ![extensão do python interativo do pyspark desabilitado](./media/hdinsight-for-vscode/pyspark-interactive-python-extension-disabled.png)


## <a name="submit-pyspark-batch-job"></a>Enviar o trabalho em lotes de PySpark

1. Abra de novo a pasta **HDexample** criada [anteriormente](#open-hdinsight-work-folder), se estiver fechada.  

2. Crie um novo arquivo **BatchFile.py** seguindo as etapas [anteriores](#open-hdinsight-work-folder).

3. Copie e cole o seguinte código no arquivo de script:

    ```python
    from __future__ import print_function
    import sys
    from operator import add
    from pyspark.sql import SparkSession
    if __name__ == "__main__":
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
    
        lines = spark.read.text('/HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv').rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' '))\
                    .map(lambda x: (x, 1))\
                    .reduceByKey(add)
        output = counts.collect()
        for (word, count) in output:
            print("%s: %i" % (word, count))
        spark.stop()
    ```

4. [Conectar-se](#connect-to-azure-account) conta do Azure ou vincular um cluster, se você ainda não tiver feito isso.

5. Clique com o botão direito do mouse no editor de scripts e, em seguida, selecione **HDInsight: Lote do PySpark**, ou use o atalho **Ctrl + Alt + I**. 

6. Selecione um cluster ao qual enviar seu trabalho PySpark. 

   ![Enviar o resultado do trabalho do Python](./media/hdinsight-for-vscode/submit-pythonjob-result.png) 

Depois de enviar um trabalho Python, os logs de envio aparecem na janela **SAÍDA** no Visual Studio Code. A **URL de interface do usuário do Spark** e a **URL de interface do usuário do Yarn** também são mostradas. Você pode abrir a URL em um navegador da Web para acompanhar o status do trabalho.

## <a name="apache-livy-configuration"></a>Configuração do Apache Livy

A configuração do [Apache Livy](https://livy.incubator.apache.org/) é compatível, ela pode ser definida no **.VSCode\settings.json** na pasta do espaço de trabalho. Atualmente, a configuração de Livy é compatível apenas com script do Python. Para obter mais detalhes, consulte o [LEIAME do Livy](https://github.com/cloudera/livy/blob/master/README.rst ).

<a id="triggerlivyconf"></a>**Como disparar a configuração de Livy**

Método 1  
1. Na barra de menus, navegue para **Arquivo** > **Preferências** > **Configurações**.  
2. Na caixa de texto **Configurações de Pesquisa**, insira **Envio de Trabalho através do HDInsight: Conf Livy**.  
3. Selecione **Editar no settings.json** para o resultado da pesquisa relevante.

Método 2   
Envie um arquivo, observe que a pasta .vscode é adicionada automaticamente à pasta de trabalho. Você pode encontrar a configuração de Livy clicando em **.vscode\settings.json**.

+ As configurações do projeto:

    ![Configuração de Livy](./media/hdinsight-for-vscode/hdi-livyconfig.png)

>[!NOTE]
>Para as configurações **driverMomory** e **executorMomry**, defina o valor com unidade, por exemplo, 1 g ou 1.024 m. 

+ As configurações de Livy compatíveis:   

    **POST /batches**   
    Corpo da solicitação

    | name | description | type | 
    | :- | :- | :- | 
    | Arquivo | Arquivo que contém o aplicativo a ser executado | caminho (obrigatório) | 
    | proxyUser | Usuário a ser representado ao executar o trabalho | cadeia de caracteres | 
    | className | Classe principal Java/Spark do aplicativo | cadeia de caracteres |
    | args | Argumentos de linha de comando do aplicativo | lista de cadeias de caracteres | 
    | jars | jars a serem usados nesta sessão | Lista de cadeias de caracteres | 
    | pyFiles | Arquivos Python a serem usados nesta sessão | Lista de cadeias de caracteres |
    | files | arquivos a serem usados nesta sessão | Lista de cadeias de caracteres |
    | driverMemory | Quantidade de memória a ser usada para o processo de driver | cadeia de caracteres |
    | driverCores | Quantidade de núcleos a ser usado para o processo de driver | int |
    | executorMemory | Quantidade de memória a ser usada por processo de executor | cadeia de caracteres |
    | executorCores | Número de núcleos a serem usados para cada executor | int |
    | numExecutors | Número de executores a serem iniciados para esta sessão | int |
    | archives | Arquivos a serem usados nesta sessão | Lista de cadeias de caracteres |
    | queue | O nome da fila YARN ao qual foi enviado | cadeia de caracteres |
    | name | O nome desta sessão | cadeia de caracteres |
    | conf | Propriedades de configuração do Spark | Mapa de key=val |

    Corpo da resposta   
    O objeto de lote criado.

    | name | description | type | 
    | :- | :- | :- | 
    | id | A id da sessão | int | 
    | appId | A ID de aplicativo desta sessão |  string |
    | appInfo | As informações detalhadas do aplicativo | Mapa de key=val |
    | log | As linhas do log | lista de cadeias de caracteres |
    | state |   O estado do lote | cadeia de caracteres |

>[!NOTE]
>A configuração de Livy atribuída será exibida no painel de saída ao enviar o script.

## <a name="integrate-with-azure-hdinsight-from-explorer"></a>Integrar ao Azure HDInsight pelo Explorer

O **Azure HDInsight** foi adicionado à exibição do Explorador. Você pode procurar e gerenciar clusters diretamente por meio do **Azure HDInsight**.

1. [Conectar-se](#connect-to-azure-account) conta do Azure ou vincular um cluster, se você ainda não tiver feito isso.

2. Na barra de menus, navegue para **Exibir** > **Explorador**.

3. No painel à esquerda, expanda **AZURE HDINSIGHT**.  As assinaturas e os clusters disponíveis (Spark, Hadoop e HBase são compatíveis) serão listados. 

   ![Assinatura do Azure HDInsight](./media/hdinsight-for-vscode/hdi-azure-hdinsight-subscription.png)

4. Expanda o cluster para exibir o esquema de banco de dados de metadados e tabela do Hive.

   ![Cluster do Azure HDInsight](./media/hdinsight-for-vscode/hdi-azure-hdinsight-cluster.png)


## <a name="preview-hive-table"></a>Visualizar a tabela de Hive
Você pode visualizar a tabela de Hive em seu cluster a (s) diretamente por meio **Azure HDInsight** explorer.
1. [Conecte-se](#connect-to-azure-account) à sua conta do Azure se ainda não tiver feito isso.

2. Clique em **Azure** ícone na coluna mais à esquerda.

3. No painel esquerdo, expanda AZURE HDINSIGHT. As assinaturas disponíveis e os clusters serão listados.

4. Expanda o cluster para exibir o esquema de banco de dados de metadados e tabela do Hive.

5. Clique com botão direito na tabela Hive, por exemplo hivesampletable. Selecione **visualização**. 

   ![HDInsight para vscode visualizar tabela de hive](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-hive-table.png)

6. O **visualizar resultados** janela será aberta.

   ![Janela de resultados do HDInsight para vscode visualização](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-results-window.png)
   
- **RESULTADOS** painel

   É possível salvar o resultado inteiro como um arquivo CSV, JSON ou Excel para o caminho local ou apenas selecionar várias linhas.

- **MENSAGENS** painel
   1. Quando o número de linhas na tabela é maior que 100 linhas, a mensagem mostra: **As primeiras 100 linhas são exibidas para a tabela de Hive**.
   2. Quando o número de linhas na tabela é menor que ou igual a 100 linhas, a mensagem mostra: **60 linhas são exibidas para a tabela de Hive**.
   3. Quando não há nenhum conteúdo na tabela, a mensagem mostra: **0 linha é exibida para a tabela de Hive**.

>[!NOTE]
>
>No Linux, instale xclip para habilitar cópia de dados de tabela.
>
>![HDInsight para vscode no linux](./media/hdinsight-for-vscode/hdinsight-for-vscode-preview-linux-install-xclip.png)
## <a name="additional-features"></a>Recursos adicionais

O HDInsight para Visual Studio Code é compatível com os seguintes recursos:

- **Preenchimento automático do IntelliSense**. Sugestões aparecem em pop-up para palavras-chave, métodos, variáveis e assim por diante. Os diferentes ícones representam diferentes tipos dos objetos.

    ![Tipos de objeto do IntelliSense das Ferramentas do HDInsight para Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-auto-complete-objects.png)
- **Marcador de erro do IntelliSense**. O serviço de linguagem sublinha os erros de edição do script do Hive.     
- **Destaques da sintaxe**. O serviço de linguagem usa cores diferentes para diferenciar variáveis, palavras-chave, tipo de dados, funções e assim por diante. 

    ![Destaques da sintaxe das Ferramentas do HDInsight para Visual Studio Code](./media/hdinsight-for-vscode/hdinsight-for-vscode-syntax-highlights.png)

## <a name="reader-only-role"></a>Somente a função de leitor

Os usuários com o cluster **Reader** **somente** **função** não pode enviar o trabalho para o cluster HDInsight nem exibir o banco de dados de Hive. Você precisa entrar em contato com o administrador de cluster para atualizar sua função para [ **HDInsight** **Cluster** **operador** ](https://docs.microsoft.com/azure/hdinsight/hdinsight-migrate-granular-access-cluster-configurations#add-the-hdinsight-cluster-operator-role-assignment-to-a-user) no [ Portal do Azure](https://ms.portal.azure.com/). Se você souber que as credenciais do Ambari, você pode vincular manualmente o cluster seguindo as instruções abaixo.

### <a name="browse-hdinsight-cluster"></a>Procurar Cluster do HDInsight  

Ao clicar em Gerenciador de HDInsight do Azure para expandir um cluster HDInsight, você deverá vincular o cluster se você for a única função de leitor para o cluster. Siga as etapas abaixo para vincular ao cluster por meio de credenciais do Ambari. 

### <a name="submit-job-to-hdinsight-cluster"></a>Enviar trabalho para o cluster do HDInsight

Ao enviar trabalho para um cluster HDInsight, você será solicitado a vincular o cluster se você for a única função de leitor para o cluster. Siga as etapas abaixo para vincular ao cluster por meio de credenciais do Ambari. 

### <a name="link-to-cluster"></a>Vincular ao cluster

1.  Insira o nome de usuário do Ambari 
2.  Insira a senha de usuário de Ambari.

   ![Ferramentas do HDInsight para o nome de usuário de código do Visual Studio](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-username.png)

   ![Ferramentas do HDInsight para a senha de código do Visual Studio](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-password.png)

> [!NOTE]
>
>Você pode usar o HDInsight: Cluster da lista para verificar se o cluster vinculado.
>
>![Ferramentas do HDInsight para o leitor de código do Visual Studio vinculado](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-reader-linked.png)

## <a name="azure-data-lake-storage-gen2-adls-gen2"></a>Azure Data Lake Storage Gen2 (ADLS Gen2)

### <a name="browse-an-adls-gen2-account"></a>Procurar uma conta do ADLS Gen2

Ao clicar em Gerenciador de HDInsight do Azure para expandir uma conta ADLS Gen2, você será solicitado a inserir o armazenamento **chave de acesso** se sua conta do Azure não tem acesso ao armazenamento Gen2. A conta do ADLS Gen2 será automaticamente expandido depois que a chave de acesso é validada com êxito. 

### <a name="submit-jobs-to-hdinsight-cluster-with-adls-gen2"></a>Enviar trabalhos ao cluster HDInsight com ADLS Gen2

Ao enviar trabalho para um cluster HDInsight com ADLS Gen2, você será solicitado a inserir o armazenamento **chave de acesso** se sua conta do Azure tem sem acesso de gravação para o armazenamento Gen2.  O trabalho será enviado com êxito depois que a chave de acesso é validada com êxito. 

![Ferramentas do HDInsight para Visual Studio código AccessKey](./media/hdinsight-for-vscode/hdi-azure-hdinsight-azure-accesskey.png)   

> [!NOTE]
> 
>Você pode obter a chave de acesso para a conta de armazenamento do portal do Azure. Para obter informações, consulte [exibir e copiar chaves de acesso](https://docs.microsoft.com/azure/storage/common/storage-account-manage#access-keys).

## <a name="unlink-cluster"></a>Desvincular cluster

1. Na barra de menus, navegue para **Exibir** > **Paleta de Comandos...** e, em seguida, insira **HDInsight: Desvincular um Cluster**.  

2. Selecione o cluster a desvincular.  

3. Verifique a exibição **SAÍDA**.  

## <a name="sign-out"></a>Sair  

Na barra de menus, navegue para **Exibir** > **Paleta de Comandos...** e, em seguida, insira **HDInsight: Logoff**.  Haverá um pop-up no canto inferior direito informando **Logoff bem sucedido!** .


## <a name="next-steps"></a>Próximas etapas
Para assistir a um vídeo de demonstração do uso do HDInsight para o Visual Studio Code, consulte [HDInsight para Visual Studio Code](https://go.microsoft.com/fwlink/?linkid=858706)
