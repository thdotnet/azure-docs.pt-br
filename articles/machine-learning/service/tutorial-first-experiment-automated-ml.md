---
title: Criar seu primeiro experimento automatizado de aprendizado de máquina
titleSuffix: Azure Machine Learning service
description: Aprenda a treinar e implantar um modelo de classificação com aprendizado de máquina automatizado na página de aterrissagem do workspace do Azure Machine Learning (versão prévia).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.author: tzvikei
author: tsikiksr
ms.reviewer: nibaccam
ms.date: 09/09/2019
ms.openlocfilehash: 0dd4447736469644875dff914c6284b087be87d0
ms.sourcegitcommit: d70c74e11fa95f70077620b4613bb35d9bf78484
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70910216"
---
# <a name="tutorial-create-your-first-classification-model-with-automated-machine-learning"></a>Tutorial: Crie seu primeiro modelo de classificação com o aprendizado de máquina automatizado

Neste tutorial, você aprenderá a criar seu primeiro experimento de aprendizado de máquina automatizado na página de aterrissagem do workspace (versão prévia) sem escrever nenhuma linha de código. Este exemplo cria um modelo de classificação para prever se um cliente assinará um depósito a prazo fixo com uma instituição financeira.

Com o aprendizado de máquina automatizado, você pode automatizar tarefas intensivas e demoradas. O aprendizado de máquina automatizado itera rapidamente em muitas combinações de algoritmos e hiperparâmetros para ajudar você a encontrar o melhor modelo com base em uma métrica de sucesso de sua escolha.

Neste tutorial, você aprenderá a fazer as seguintes tarefas:

> [!div class="checklist"]
> * Criar um workspace do Serviço do Azure Machine Learning.
> * Executar um experimento de aprendizado de máquina automatizado.
> * Exibir detalhes do experimento.
> * Implantar o modelo.

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure. Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://aka.ms/AMLFree).

* Baixe o arquivo de dados [**bankmarketing_train.csv**](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv). A coluna **y** indica se um cliente assinou um depósito a prazo fixo, que é posteriormente identificado como a coluna de destino para as previsões neste tutorial. 

## <a name="create-a-workspace"></a>Criar um workspace

Um Workspace do Azure Machine Learning é o recurso fundamental na nuvem que você usa para experimentar, treinar e implantar modelos de machine learning. Ele vincula sua assinatura do Azure e o grupo de recursos a um objeto facilmente consumido no serviço. 

Você cria um workspace pelo portal do Azure, um console baseado na Web para gerenciar seus recursos do Azure. 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

>[!IMPORTANT] 
> Anote seu **workspace** e sua **assinatura**. Você precisará dessas informações para criar o experimento no local certo. 

## <a name="create-and-run-the-experiment"></a>Criar e executar o experimento

Conclua a configuração do experimento a seguir e execute as etapas na página de aterrissagem do workspace, uma interface consolidada que inclui ferramentas de aprendizado de máquina para realizar cenários de ciência de dados para praticantes de todos os níveis de habilidade.

1. Entre na [página de aterrissagem do workspace](https://ml.azure.com/workspaceportal/).

1. Selecione a assinatura e o workspace criado.

1. Selecione **Introdução**.

1.  Selecione **ML Automatizada** na seção **Criação**, no painel esquerdo.
Você verá a tela **Introdução**, pois este é seu primeiro experimento com o Aprendizado de Máquina Automatizado.

    ![Azure Machine Learning Studio](media/tutorial-1st-experiment-automated-ml/get-started.png)

1. Selecione **Criar um experimento**. 

1. Insira **my-1st-automl-experiment** como o nome do experimento.

1. Selecione **Criar uma nova computação**. 

    1. Configure o contexto de computação do experimento.
        
        Campo | Valor
        ----|---
        Nome da computação |  Insira um nome exclusivo que identifique o contexto da computação. Para este exemplo, use **automl-compute**.
        Tamanho da máquina virtual| Selecione o tamanho da máquina virtual da computação. Use o padrão, **Standard_DS12_V2**.
        Configurações adicionais| *Nó mínimo*: 1. Para habilitar a criação de perfil de dados, você precisa ter um ou mais nós. <br> *Nó máximo*: 6.
 
    1. Para criar a computação, selecione **Criar**. Isso leva alguns minutos para ser concluído. 

    1. Quando a criação for concluída, selecione a nova computação na lista suspensa e, em seguida, selecione **Avançar**.

    >[!NOTE]
    >Para este tutorial, você usará a conta de armazenamento padrão e o contêiner criado com a nova computação. Eles são populados automaticamente no formulário.

1. Selecione **Carregar do arquivo local**. A partir de agora, você criará um novo conjunto de dados com o arquivo **bankmarketing_train.csv** baixado anteriormente neste tutorial. 

    1. Clique em **Procurar** e selecione o arquivo **bankmarketing_train.csv** no computador local. 

    1. Dê um nome exclusivo ao conjunto de dados e forneça uma descrição opcional. 

    1. Clique em **Próximo** para carregá-lo no contêiner padrão que foi automaticamente configurado durante a criação do workspace. A versão prévia pública só é compatível com carregamentos de arquivo local. 

    1. Após o upload, o formulário **Configurações e versão prévia** será preenchidos de modo inteligente com base no tipo de arquivo. Verifique se o formulário foi preenchido da seguinte maneira.
        
        Campo|Valor
        ---|---
        Formato de arquivo| Delimitado
        Delimitador| Vírgula
        Codificação| UTF-8
        Cabeçalhos da coluna| Todos os arquivos têm os mesmos cabeçalhos            Ignorar linhas | Nenhum

        >[!NOTE]
        > Se alguma das configurações deste formulário for atualizada, a versão prévia também será atualizada.

        Selecione **Avançar**.
    

    1. O formulário **Esquema** permite configurar ainda mais os dados do experimento. Para este exemplo, selecione o botão de alternância do recurso **day_of_week** para não incluí-lo neste experimento. Clique em **Concluído** para concluir o upload do arquivo e a criação do conjunto de dados do experimento.

        ![Configuração da guia Visualização](media/tutorial-1st-experiment-automated-ml/schema-tab-config.gif)

        
1. Selecione **Classificação** como a tarefa de previsão.

1. Selecione **y** como a coluna de destino, o que você quer prever. Essa coluna indica se o cliente assinou um depósito a prazo ou não.

1. Expanda **Configurações Avançadas** e preencha os campos conforme demonstrado a seguir.

    Configurações avançadas|Valor
    ------|------
    Métrica principal| AUC_weighted 
    Critérios de saída| Quando qualquer um desses critérios é atendido, o trabalho de treinamento termina antes de ser totalmente concluído: <br> *Tempo de trabalho de treinamento (minutos)* : 5  <br> *Número máximo de iterações*: 10 
    Pré-processamento| Habilita o pré-processamento feito pelo aprendizado de máquina automatizado. Isso inclui limpeza, preparação e transformação automáticas de dados para gerar recursos sintéticos.
    Validação| Selecione Validação cruzada K vezes e **2** para o número de validações cruzadas. 
    Simultaneidade| Selecione **5** para o número máximo de iterações simultâneas.

   >[!NOTE]
   > Para este experimento, não defina uma métrica nem um limite máximo de núcleos por iterações. Também não bloqueie a possibilidade de testar os algoritmos.

1. Clique em **Iniciar** para executar o experimento.

   Quando o experimento for iniciado, você verá uma tela em branco com uma mensagem de status na parte superior.

O processo de preparação do experimento leva alguns minutos. Quando o processo é concluído, a mensagem de status muda para **Execução em Andamento**.

##  <a name="view-experiment-details"></a>Exibir detalhes do experimento

À medida que o experimento progride, a tela atualiza o **Gráfico de iteração** e a **Lista de iteração** com as diferentes iterações (modelos) que são executadas. A lista de iterações está em ordem por pontuação de métrica. Por padrão, o modelo que pontua o mais alto com base em nossa métrica **AUC_weighted** está na parte superior da lista.

>[!WARNING]
> Nos trabalhos de treinamento, a execução de cada pipeline leva vários minutos até terminar.

[![Painel de detalhes da execução](media/tutorial-1st-experiment-automated-ml/run-details.png)](media/tutorial-1st-experiment-automated-ml/run-details-expanded.png#lightbox)

## <a name="deploy-the-model"></a>Implantar o modelo

Ao usar o aprendizado de máquina automatizado na página de aterrissagem do workspace, você pode implantar o melhor modelo como um serviço Web para prever novos dados e identificar possíveis áreas de oportunidade. Para este experimento, a implantação significa que a instituição financeira agora tem uma solução iterativa e escalonável para identificar clientes potenciais para depósito a prazo fixo.

Nesse contexto de experimento, **VotingEnsemble** é considerado o melhor modelo, com base na métrica **AUC_weighted**.  Implantamos esse modelo, mas saiba que a implantação demora cerca de 20 minutos para ser concluída.

1. Na página **Detalhes da Execução**, clique no botão **Implantar o Melhor Modelo** no canto superior direito.

1. Preencha o painel **Implantar o Melhor Modelo** da seguinte maneira:

    Campo| Valor
    ----|----
    Nome da implantação| my-automl-deploy
    Descrição da implantação| Minha primeira implantação de experimento de aprendizado de máquina automatizado
    Script de pontuação| Gerar automaticamente
    Script do ambiente| Gerar automaticamente
    
1. Selecione **Implantar**.

    Uma mensagem de conclusão da implantação é exibida quando a implantação é concluída com êxito.
    
Agora você tem um serviço Web operacional para gerar previsões.

## <a name="clean-up-resources"></a>Limpar recursos

Os arquivos de implantação são maiores que os dados e os arquivos de teste, portanto, eles custam mais para serem armazenados. Exclua somente os arquivos de implantação para minimizar os custos de sua conta ou se você quiser manter o workspace e os arquivos do experimento. Caso contrário, se você não planeja usar nenhum dos arquivos, exclua o grupo de recursos inteiro.  

### <a name="delete-the-deployment-instance"></a>Excluir a instância de implantação

Exclua apenas a instância de implantação do portal do Azure se você quiser manter o grupo de recursos e o workspace para outros tutoriais e explorações. 

1. Vá para o [Portal do Azure](https://portal.azure.com//). Navegue até o workspace e, no lado esquerdo, no painel **Ativos**, selecione **Implantações**. 

1. Selecione a implantação que você deseja excluir e selecione **Excluir**. 

1. Selecione **Continuar**.

### <a name="delete-the-resource-group"></a>Exclua o grupo de recursos

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Próximas etapas

Neste tutorial de aprendizado de máquina automatizado, você usou a página de aterrissagem do workspace para criar e implantar um modelo de classificação. Confira estes artigos para obter mais informações e ver as próximas etapas:

> [!div class="nextstepaction"]
> [Consumir um serviço Web](how-to-consume-web-service.md)

+ Saiba mais sobre o [pré-processamento](how-to-create-portal-experiments.md#preprocess).
+ Saiba mais sobre a [criação de perfil de dados](how-to-create-portal-experiments.md#profile).
+ Saiba mais sobre o [aprendizado de máquina automatizado](concept-automated-ml.md).

>[!NOTE]
> Esse conjunto de dados de marketing bancário é disponibilizado sob a [licença Creative Commons (CCO: Domínio Público)](https://creativecommons.org/publicdomain/zero/1.0/). Todos os direitos no conteúdo individual do banco de dados são licenciados sob a [Licença de Conteúdo do Banco de Dados](https://creativecommons.org/publicdomain/zero/1.0/) e estão disponíveis no [Kaggle](https://www.kaggle.com/janiobachmann/bank-marketing-dataset). Esse conjunto de dados estava originalmente disponível no [banco de dados de aprendizado de máquina da UCI](https://archive.ics.uci.edu/ml/datasets/bank+marketing).<br><br>
> [Moro et al., 2014] S. Moro, P. Cortez and P. Rita. Uma abordagem controlada por dados para prever o sucesso do telemarketing bancário. Sistemas de suporte a decisões, Elsevier, 62:22-31, junho de 2014.
