---
title: 'Referência: DSVM do Windows'
description: Detalhes sobre as ferramentas incluídas no Windows VM de Ciência de Dados
author: gvashishtha
ms.service: machine-learning
ms.subservice: data-science-vm
ms.author: gopalv
ms.date: 09/11/2019
ms.topic: reference
ms.openlocfilehash: be4397de477891f2a698c9f7dcb131da79479529
ms.sourcegitcommit: 8a717170b04df64bd1ddd521e899ac7749627350
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/23/2019
ms.locfileid: "71200020"
---
# <a name="reference-windows-data-science-virtual-machine"></a>Referência: Máquina Virtual de Ciência de Dados do Windows

Veja abaixo uma lista de ferramentas disponíveis no seu Máquina Virtual de Ciência de Dados do Windows. 

## <a name="microsoft-machine-learning-server-developer-edition"></a>Microsoft Machine Learning Server Developer edition

É possível usar o Microsoft Enterprise Library para a análise porque o Machine Learning Server Developer Edition está instalado na VM. Anteriormente conhecida como Microsoft R Server, o Machine Learning Server é uma plataforma de análise amplamente implementável. Ele é escalonável e tem suporte comercial.

A Machine Learning Server dá suporte variadas estatísticas de Big Data, modelagem preditiva e tarefas de aprendizado de máquina. Ela oferece suporte a uma gama completa de análises: exploração, análise, visualização e modelagem. Ao usar e estender o R e Python de software livre, a Machine Learning Server é compatível com funções e scripts de R e Python. Também é compatível com pacotes CRAN, pip e Conda para analisar dados em escala empresarial.

A Machine Learning Server também aborda as limitações de memória interna do R de software livre, adicionando processamento paralelo e em partes dos dados. Isso significa que é possível executar análises em dados muito maiores do que cabe na memória principal. 

O Visual Studio Community está incluído na VM. Ele contém as Ferramentas do R para Visual Studio e a extensão PTVS (Ferramentas Python para Visual Studio), que fornecem um IDE completo para trabalhar com o R ou Python. Nós também fornecemos outros IDEs, como [RStudio](https://www.rstudio.com) e [PyCharm Community Edition](https://www.jetbrains.com/pycharm/) na VM.

## <a name="python"></a>Python

Para o desenvolvimento usando o Python, as distribuições do Anaconda Python 2.7 e 3.6 estão instaladas. Essas distribuições têm o Python base com aproximadamente 300 dos mais populares pacotes de matemática, engenharia e análise de dados. É possível usar a PTVS, que é instalada no Visual Studio Community 2017. Ou você pode usar um dos IDEs em pacote com Anaconda como IDLE ou Spyder. Pesquise e abra um desses pacotes (tecla do logotipo do Windows + S).

> [!NOTE]
> Para apontar as Ferramentas Python para Visual Studio para o Anaconda Python 2.7, você precisa criar ambientes personalizados para cada versão. Para definir esses caminhos de ambiente no Visual Studio 2017 Community, vá até **Ferramentas** > **Ferramentas Python** > **Ambientes Python**. Em seguida, selecione **+ Custom**.

O Anaconda Python 3.6 é instalado em C:\Anaconda. O Anaconda Python 2.7 é instalado em C:\Anaconda\envs\python2. Para ver etapas detalhadas, confira a [Documentação do PTVS](https://docs.microsoft.com/visualstudio/python/installing-python-interpreters).

## <a name="the-jupyter-notebook"></a>O Jupyter Notebook

A Distribuição do Anaconda também acompanha o Jupyter Notebook, um ambiente para compartilhamento de código e análise. Um servidor do Jupyter Notebook foi pré-configurado com os kernels do Python 2.7, Python 3.x, PySpark, Julia e R. Para inicializar o servidor do Jupyter e abrir o navegador a fim de acessar o servidor do Notebook, use o ícone **Jupyter Notebook** da área de trabalho.

Nós empacotamos vários notebooks de exemplo em Python e R. Após acessar o Jupyter, os notebooks mostram como trabalhar com as seguintes tecnologias:

* Machine Learning Server
* Serviços do Machine Learning do SQL Server, análise no banco de dados
* Python
* Kit de Ferramentas Cognitivas da Microsoft
* TensorFlow
* Outras tecnologias do Azure

Encontre o link para os exemplos na home page do Notebook após a autenticação no Jupyter Notebook usando a senha criada anteriormente.

## <a name="visual-studio-community-2017"></a>Visual Studio Community 2017

A DSVM inclui o Visual Studio Community. Esta é uma versão gratuita do IDE popular da Microsoft que pode ser usada para fins de avaliação e para pequenas equipes. Veja os [Termos de Licença para Software Microsoft](https://www.visualstudio.com/support/legal/mt171547).

Abra o Visual Studio usando o ícone da área de trabalho ou o menu **Iniciar**. Pesquise programas (tecla do logotipo do Windows + S), seguido de **Visual Studio**. Nesse local, você pode criar projetos em linguagens como C#, Python, R e Node.js. Plug-ins instalados tornam conveniente trabalhar com os seguintes serviços do Azure:

* Catálogo de Dados do Azure
* Azure HDInsight para Hadoop e Spark
* Azure Data Lake

Um plug-in chamado Azure Machine Learning para Visual Studio Code também é integrado ao Azure Machine Learning e ajuda a compilar aplicativos de IA rapidamente.

> [!NOTE]
> Você poderá receber uma mensagem informando que o período de avaliação expirou. Insira suas credenciais de conta da Microsoft. Ou crie uma nova conta gratuita para obter acesso ao Visual Studio Community.

## <a name="sql-server-2017-developer-edition"></a>SQL Server 2017 Developer Edition

A DSVM vem com uma versão de desenvolvedor do SQL Server 2017 com Serviços do Machine Learning. Esta edição do SQL Server é fornecida em R ou Python e pode executar análises no banco de dados. 

Os Serviços de Machine Learning fornecem uma plataforma para desenvolver e implantar aplicativos inteligentes. É possível usar essas linguagens e os vários pacotes da comunidade para criar modelos e gerar previsões para seus dados do SQL Server. Você pode manter a análise próxima aos dados porque os Serviços de Machine Learning, no banco de dados, integram as linguagens R e Python dentro do SQL Server. Isso elimina os custos e os riscos de segurança associados à movimentação de dados.

> [!NOTE]
> O SQL Server Developer Edition só pode ser usado para fins de desenvolvimento e teste. Você precisa de uma licença para executá-la em produção.

É possível acessar o SQL Server abrindo o Microsoft SQL Server Management Studio. O nome da VM é preenchido como o **Nome do Servidor**. Use a autenticação do Windows quando estiver conectado como o administrador no Windows. Quando estiver no SQL Server Management Studio, você pode criar outros usuários, criar bancos de dados, importar dados e executar consultas SQL.

Para habilitar a análise no banco de dados usando os Serviços de Machine Learning do SQL Server, execute o comando a seguir como uma única ação no SQL Server Management Studio depois de entrar como administrador do servidor:

```
CREATE LOGIN [%COMPUTERNAME%\SQLRUserGroup] FROM WINDOWS
```

Substitua `%COMPUTERNAME%` pelo nome da VM.

## <a name="azure"></a>Azure

Várias ferramentas do Azure são instaladas na VM:

* Um atalho da área de trabalho vai para a documentação do SDK do Azure.
* Use AzCopy para copiar dados para dentro e fora da sua conta de armazenamento do Azure. Para ver o uso, insira **Azcopy** em um prompt de comando.
* Use o Gerenciador de Armazenamento do Azure para procurar os objetos armazenados na sua conta de armazenamento do Azure. Ele também copia dados de e para o Armazenamento do Azure. Para acessar essa ferramenta, insira **Gerenciador de Armazenamento** no campo **Pesquisar**. Ou encontre a ferramenta no menu **Iniciar** do Windows.
* AdlCopy copia dados para o Azure Data Lake. Para ver o uso, insira **adlcopy** em um prompt de comando.
* A ferramenta dtui copia dados para dentro e fora do Azure Cosmos DB, um banco de dados NoSQL na nuvem. Insira **dtui** em um prompt de comando.
* O runtime de integração copia dados entre as fontes de dados locais e a nuvem. É usado em ferramentas como o Azure Data Factory.
* Use o Azure PowerShell para administrar os recursos do Azure na linguagem de scripts do PowerShell. Ela também é instalada na VM.

## <a name="power-bi"></a>Power BI

A DSVM vem com o Power BI Desktop instalado para ajudá-lo a criar dashboards e visualizações. Use essa ferramenta para extrair dados de fontes diferentes, criar painéis e relatórios e publicá-los na nuvem. Para obter mais informações, confira o site do [Power BI](https://powerbi.microsoft.com). Você pode encontrar o Power BI Desktop no menu **Iniciar**.

> [!NOTE]
> É necessária uma conta do Microsoft Office 365 para acessar o Power BI.

## <a name="azure-machine-learning-sdk-for-python"></a>SDK do Azure Machine Learning para Python

Cientistas de dados e desenvolvedores de IA usam O SDK do Azure Machine Learning para o Python a fim de compilar e executar fluxos de trabalho de aprendizado de máquina com o [Serviço do Azure Machine Learning](../service/overview-what-is-azure-ml.md). Você pode interagir com o serviço em Jupyter Notebooks ou em outro IDE do Python usando estruturas de software livre, como o TensorFlow e o scikit-learn.

O SDK Python é pré-instalado na Máquina Virtual de Ciência de Dados da Microsoft. Para começar a usar o SDK do Python, confira [Usar o Python para introdução ao Azure Machine Learning](../service/quickstart-create-workspace-with-python.md).

## <a name="more-microsoft-development-tools"></a>Mais ferramentas de desenvolvimento da Microsoft

Use o [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) para encontrar e baixar outras ferramentas de desenvolvimento da Microsoft. Também há um atalho para a ferramenta na área de trabalho de Máquina Virtual de Ciência de Dados da Microsoft.  

## <a name="important-directories-on-the-virtual-machine"></a>Diretórios importantes na máquina virtual

| Item | Diretório |
| --- | --- |
| Configurações de servidor do Jupyter Notebook | C:\ProgramData\jupyter |
| Diretório base de amostras do Bloco de Anotações do Jupyter | C:\dsvm\notebooks e c:\users\\<nome de usuário\>\notebooks |
| Outras amostras | C:\dsvm\samples |
| Anaconda, padrão: Python 3.6 | C:\Anaconda |
| Ambiente do Anaconda Python 2.7 | C:\Anaconda\envs\python2 |
| Microsoft Machine Learning Server (autônomo) para Python | C:\Arquivos de Programas\Microsoft\ML Server\PYTHON_SERVER |
| Instância R padrão, Machine Learning Server (autônomo) | C:\Arquivos de Programas\Microsoft\ML Server\R_SERVER |
| Diretório da instância no banco de dados do Serviços de Machine Learning do SQL Server | C:\Arquivos de Programas\Microsoft SQL Server\MSSQL14.MSSQLSERVER |
| Ferramentas diversas | C:\dsvm\tools |

> [!NOTE]
> Na edição do Windows Server 2012 da DSVM e na edição do Windows Server 2016 antes de março de 2018, o ambiente padrão do Anaconda é Python 2.7. O ambiente secundário é o Python 3.5 localizado em C:\Anaconda\envs\py35.

## <a name="next-steps"></a>Próximas etapas

Tem mais dúvidas? Considere criar um [tíquete de suporte](https://azure.microsoft.com/support/create-ticket/).
