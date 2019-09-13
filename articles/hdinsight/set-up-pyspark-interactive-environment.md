---
title: Ferramentas do Azure HDInsight – ambiente interativo do PySpark para Visual Studio Code
description: Saiba como usar as Ferramentas do Azure HDInsight para Visual Studio Code para criar e enviar consultas e scripts.
keywords: VSCode, Ferramentas do Azure HDInsight, Hive, Python, PySpark, Spark, HDInsight, Hadoop, LLAP, Hive Interativo, Consulta Interativa
ms.service: hdinsight
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 06/13/2019
ms.openlocfilehash: d220d81b8dc57541113f7ef1e477bb77d394e206
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70879297"
---
# <a name="set-up-the-pyspark-interactive-environment-for-visual-studio-code"></a>Configurar o ambiente interativo do PySpark para o Visual Studio Code

As etapas a seguir mostram como configurar o ambiente interativo do PySpark no VS Code.

Usamos comando **python/pip** para criar o ambiente virtual em seu caminho de Página Inicial. Se você deseja usar outra versão, precisa alterar a versão padrão do comando **python/pip** manualmente. Para obter mais detalhes, confira [update-alternatives](https://linux.die.net/man/8/update-alternatives).

1. Instale [Python](https://www.python.org/downloads/) e [pip](https://pip.pypa.io/en/stable/installing/).
   
   + Instale o Python de [https://www.python.org/downloads/](https://www.python.org/downloads/).
   + Instale o Pip [https://pip.pypa.io/en/stable/installing](https://pip.pypa.io/en/stable/installing/) do (se ele não estiver instalado na instalação do Python).
   + Valide se o Python e o Pip foram instalados com êxito usando os comandos a seguir. (Opcional)
 
        ![Versão do pip do Python](./media/set-up-pyspark-interactive-environment/check-python-pip-version.png)

     > [!NOTE]
     > É recomendável instalar o Python manualmente em vez de usar a versão padrão do macOS.


2. Instale **virtualenv** executando o comando a seguir.
   
   ```
   pip install virtualenv
   ```

## <a name="other-packages"></a>Outros pacotes

Se você encontrar uma mensagem de erro, instale os pacotes necessários executando os seguintes comandos:

   ![pacote libkrb5](./media/set-up-pyspark-interactive-environment/install-libkrb5-package.png)

```
sudo apt-get install libkrb5-dev
```

```
sudo apt-get install python-dev
```

Reinicie o VS Code e, em seguida, retorne para o editor de scripts que está executando o **HDInsight: PySpark Interativo**.

## <a name="next-steps"></a>Próximas etapas

### <a name="demo"></a>Demonstração
* HDInsight para VS Code: [Vídeo](https://go.microsoft.com/fwlink/?linkid=858706)

### <a name="tools-and-extensions"></a>Ferramentas e extensões
* [Usar a Ferramenta do Azure HDInsight para Visual Studio Code](hdinsight-for-vscode.md)
* [Use o Azure Toolkit for IntelliJ para criar e enviar aplicativos Scala do Apache Spark](spark/apache-spark-intellij-tool-plugin.md)
* [ Use o Azure Toolkit for IntelliJ para depurar os aplicativos do Apache Spark remotamente por meio do SSH ](spark/apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [Use o Azure Toolkit for IntelliJ para depurar aplicativos Apache Spark remotamente por meio de VPN](spark/apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Use as ferramentas do HDInsight no Azure Toolkit for Eclipse para criar aplicativos do Apache Spark](spark/apache-spark-eclipse-tool-plugin.md)
* [Use os blocos de anotações do Apache Zeppelin com um cluster do Apache Spark no HDInsight](spark/apache-spark-zeppelin-notebook.md)
* [Kernels disponíveis para o notebook do Jupyter no cluster do Apache Spark para HDInsight](spark/apache-spark-jupyter-notebook-kernels.md)
* [Usar pacotes externos com blocos de notas Jupyter](spark/apache-spark-jupyter-notebook-use-external-packages.md)
* [Instalar o Jupyter em seu computador e conectar-se a um cluster Spark do HDInsight](spark/apache-spark-jupyter-notebook-install-locally.md)
* [Visualize dados do Apache Hive com o Microsoft Power BI no Azure HDInsight](hadoop/apache-hadoop-connect-hive-power-bi.md)
* [Usar o Apache Zeppelin para executar consultas do Apache Hive no Azure HDInsight](./interactive-query/hdinsight-connect-hive-zeppelin.md)
