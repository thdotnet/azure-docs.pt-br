---
title: 'Tutorial: Prever o preço de automóveis com a interface visual'
titleSuffix: Azure Machine Learning service
description: Saiba como treinar, pontuar e implantar um modelo de machine learning usando uma interface visual do tipo "arrastar e soltar". Este tutorial é a primeira parte de uma série de duas partes sobre a previsão de preços de automóveis usando a regressão linear.
author: peterclu
ms.author: peterclu
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
ms.date: 04/06/2019
ms.openlocfilehash: 21f5a2d93b708e93f124bd44177bb7852dfbd86a
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67720559"
---
# <a name="tutorial-predict-automobile-price-with-the-visual-interface"></a>Tutorial: Prever o preço de automóveis com a interface visual

Neste tutorial, você analisará de maneira aprofundada o desenvolvimento de uma solução preditiva na interface visual do Serviço do Azure Machine Learning. Ao final deste tutorial, você terá uma solução que pode prever o preço de qualquer carro com base nas especificações técnicas enviadas a ela.

Na primeira parte do tutorial, você aprenderá a:

> [!div class="checklist"]
> * Importar e limpar dados
> * Treinar um modelo de machine learning
> * Pontuar e avaliar um modelo

Na [segunda parte](ui-tutorial-automobile-price-deploy.md) do tutorial, você aprenderá a implantar seu modelo preditivo como um serviço Web do Azure.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GY]

Uma versão concluída deste tutorial está disponível como um teste de exemplo.

Para encontrá-la, na **página Experimentos**, selecione **Adicionar Novo** e, em seguida, selecione o experimento **Amostra 1 – Regressão: Previsão de Preços de Automóveis (Básica)** .

## <a name="create-a-workspace"></a>Criar um workspace

Se você tiver um workspace do Serviço do Azure Machine Learning, passe para a [próxima seção](#open-the-visual-interface-webpage).

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="open-the-visual-interface-webpage"></a>Abrir a página da Web da interface visual

1. Abra seu workspace na [portal do Azure](https://portal.azure.com/).

1. No workspace, selecione **Interface visual**. Em seguida, selecione **Iniciar interface visual**. 

    ![Captura de tela do portal do Azure mostrando como acessar a Interface visual em um workspace do serviço do Machine Learning](./media/ui-tutorial-automobile-price-train-score/launch-ui.png)

## <a name="create-your-first-experiment"></a>Criar seu primeiro experimento

A ferramenta de interface visual fornece um lugar visual e interativo para a criação de modelos de análise preditiva. Arraste e solte conjuntos de dados e módulos de análise em uma tela interativa e conecte-os para criar um *experimento*.

1. Crie um teste selecionando **+Novo** na parte inferior da janela da interface visual.

    ![Adicionar um novo experimento](./media/ui-tutorial-automobile-price-train-score/add-new.png)

1. Selecione **Teste em branco**.

1. Selecione o nome do teste padrão **"Teste Criado em...** " na parte superior da tela e renomeie-o para algo significativo. Por exemplo, **Previsão de preços de automóveis**. O nome não precisa ser exclusivo.

## <a name="add-data"></a>Adicionar dados

A primeira coisa de que você precisa para o aprendizado de máquina são os dados. Há vários conjuntos de dados de exemplo incluídos nessa interface que podem ser usados. Você também pode importar dados de fontes existentes. Neste tutorial, use o conjunto de dados de exemplo **Dados de preços de automóveis (brutos)** . 

1. À esquerda da tela do experimento está uma paleta de conjuntos de dados e módulos. Selecione **Conjuntos de Dados Salvos**; em seguida, selecione **Exemplos** para exibir os conjuntos de dados de exemplo disponíveis.

1. Selecione o conjunto de dados, **Dados de preço de automóvel (brutos)** e arraste-o para a tela.

   ![Arraste os dados para a tela](./media/ui-tutorial-automobile-price-train-score/drag-data.png)

## <a name="select-columns"></a>Selecionar colunas

Selecione com quais colunas de dados você deseja trabalhar. Para começar, configure o módulo para mostrar todas as colunas disponíveis.

> [!TIP]
> Se você souber o nome dos dados ou do módulo desejado, use a barra de pesquisa na parte superior da paleta para localizá-los rapidamente. O restante do tutorial usará esse atalho.


1. Digite **Selecionar** na caixa de pesquisa para localizar o módulo **Selecionar colunas no conjunto de dados**.

1. Clique e arraste **Selecionar colunas no conjunto de dados** para a tela. Solte o módulo embaixo do conjunto de dados adicionado anteriormente.

1. Conecte o conjunto de dados a **Selecionar colunas no conjunto de dados**: clique na porta de saída do conjunto de dados, arraste para a porta de entrada de **Selecionar colunas no conjunto de dados** e, em seguida, solte o botão do mouse. O conjunto de dados e o módulo permanecem conectados mesmo se você mover um deles nas telas.

    > [!TIP]
    > Os módulos e conjuntos de dados têm portas de entrada e saída representadas por círculos pequenos – portas de entrada na parte superior, portas de saída na parte inferior. Crie um fluxo de dados por meio do seu teste quando você conectar a porta de saída de um módulo a uma porta de entrada de outro.
    >

    ![Conectar módulos](./media/ui-tutorial-automobile-price-train-score/connect-modules.gif)

    O ponto de exclamação vermelho indica que você não definiu as propriedades do módulo ainda.

1. Selecione o módulo **Selecionar colunas no conjunto de dados**.

1. No painel **Propriedades** à direita da tela, selecione **Editar colunas**.

    Na caixa de diálogo **Selecionar colunas**, selecione **TODAS AS COLUNAS** e inclua **todos os recursos**. A caixa de diálogo deve ter esta aparência:

     ![column-selector](./media/ui-tutorial-automobile-price-train-score/select-all.png)

1. No canto inferior direito, selecione **Ok** para fechar o seletor de coluna.

## <a name="run-the-experiment"></a>Execute o experimento

A qualquer momento, clique na porta de saída de um conjunto de dados ou de um módulo para ver qual é a aparência dos dados nesse momento no fluxo de dados. Se a opção **Visualizar** estiver desabilitada, primeiro será necessário executar o teste.

Um experimento é executado em um destino de computação, que é um recurso de computação anexado ao workspace. Depois de criar um destino de computação, você poderá reutilizá-lo para execuções futuras.

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

Depois que o destino de computação estiver disponível, o teste será executado. Quando a execução for concluída, uma marca de seleção verde será exibida em cada módulo.


## <a name="preview-the-data"></a>Visualizar os dados

Agora que você executou o experimento inicial, visualize os dados para entender mais sobre o conjunto de dados com o qual precisará trabalhar.

1. Selecione a porta de saída na parte inferior de **Selecionar colunas no conjunto de dados** e, em seguida, selecione **Visualizar**.

1. Clique em diferentes colunas na janela de dados para exibir informações sobre ela.

    No conjunto de dados de exemplo, cada linha representa uma instância de um automóvel e as variáveis associadas a cada automóvel aparecem como colunas. Há 205 linhas e 26 colunas nesse conjunto de dados.

     Cada vez que você clica em uma coluna de dados, as informações de **Estatísticas** e a imagem de **Visualização** dessa coluna são exibidas à esquerda. Por exemplo, ao clicar em **num-of-doors**, você verá que ela tem dois valores exclusivos e dois valores ausentes. Role para baixo para ver os valores: duas e quatro portas.

     ![Visualizar os dados](./media/ui-tutorial-automobile-price-train-score/preview-data.gif)

1. Clique em cada coluna para entender mais sobre o conjunto de dados e pense se essas colunas serão úteis para prever o preço de um automóvel.

## <a name="prepare-data"></a>Preparar dados

Um conjunto de dados geralmente requer algum pré-processamento antes de poder ser analisado. Talvez você tenha observado alguns valores ausentes ao visualizar o conjunto de dados. Os valores ausentes precisam ser limpos para que o modelo possa analisar os dados corretamente. Você removerá quaisquer linhas com valores ausentes. Além disso, a coluna **normalized-losses** tem uma grande proporção de valores ausentes; portanto, você a excluirá do modelo completamente.

> [!TIP]
> Limpar os valores ausentes dos dados de entrada é um pré-requisito para usar a maioria dos módulos.

### <a name="remove-column"></a>Remover coluna

Primeiro, remova a coluna **normalized-losses** por completo.

1. Selecione o módulo **Selecionar colunas no conjunto de dados**.

1. No painel **Propriedades** à direita da tela, selecione **Editar colunas**.

    * Mantenha as opções **Com regras** e **TODAS AS COLUNAS** selecionadas.

    * Nos menus suspensos, selecione **Excluir** e **nomes da coluna** e clique dentro da caixa de texto. Digite **normalized-losses**.

    * No canto inferior direito, selecione **Ok** para fechar o seletor de coluna.

    ![Excluir uma coluna](./media/ui-tutorial-automobile-price-train-score/exclude-column.png)
        
    Agora o painel de propriedades para Selecionar colunas no Conjunto de dados indica que ele passará todas as colunas do conjunto de dados, exceto **normalized-losses**.
        
    O painel de propriedades mostra que a coluna **normalized-losses** foi excluída.
        
    ![Painel de propriedade](./media/ui-tutorial-automobile-price-train-score/property-pane.png)
        
    É possível adicionar um comentário em um módulo ao clicar duas vezes nele e inserir o texto. Isso pode ajudar a ver rapidamente o que o módulo está fazendo em seu experimento. 

1. Clique duas vezes no módulo **Selecionar Colunas no Conjunto de Dados** e digite o comentário "Excluir perdas normalizadas." 
    
    Após digitar o comentário, clique fora do módulo. Uma seta para baixo é exibida para mostrar que o módulo contém um comentário.

1. Clique na seta para baixo para exibir o comentário.

    Agora o módulo mostra uma seta para cima para ocultar o comentário.
        
    ![Comentários](./media/ui-tutorial-automobile-price-train-score/comments.png)

### <a name="clean-missing-data"></a>Limpar dados ausentes

Quando você treina um modelo, precisa fazer algo sobre os dados que estão faltando. Nesse caso, você adicionará um módulo para remover qualquer linha restante que tenha dados ausentes.

1. Digite **Limpar** na caixa de pesquisa para encontrar o módulo **Limpar Dados Ausentes**.

1. Arraste o módulo **Limpar Dados Ausentes** para a tela do teste e conecte-o ao módulo **Selecionar Colunas no Conjunto de Dados**. 

1. No painel Propriedades, selecione **Remover linha inteira** em **Modo de limpeza**.

    Essas opções instruem o módulo **Limpar Dados Ausentes** a limpar os dados removendo as linhas que têm valores ausentes.

1. Clique duas vezes no módulo e digite o comentário “Remover linhas de valor ausente".
 
    ![Remover linhas](./media/ui-tutorial-automobile-price-train-score/remove-rows.png)

    Agora seu teste deve ter a seguinte aparência:
    
    ![select-column](./media/ui-tutorial-automobile-price-train-score/experiment-clean.png)

## <a name="visualize-the-results"></a>Visualizar os resultados

Coo você fez alterações nos módulos do seu teste, o status mudou para “Em rascunho”.  Para visualizar os novos dados limpos, primeiro você precisa executar o teste novamente.

1. Selecione **Executar** na parte inferior para executar o teste.

    Neste momento, é possível reutilizar o destino de computação que você criou anteriormente.

1. Selecione **Executar** na caixa de diálogo.

   ![Executar o experimento](./media/ui-tutorial-automobile-price-train-score/select-compute.png)

1. Após a conclusão da execução, clique com o botão direito do mouse no módulo **Limpar Dados Ausentes** para visualizar os novos dados limpos.

    ![Visualizar dados limpos](./media/ui-tutorial-automobile-price-train-score/visualize-cleaned.png)

1. Clique em colunas diferentes na janela de dados limpos para ver como os dados mudaram.

    ![Visualizar dados limpos](media/ui-tutorial-automobile-price-train-score/visualize-result.png)

    Agora há 193 linhas e 25 colunas.

    Ao clicar em **num-of-doors**, você verá que ele ainda tem dois valores exclusivos, mas agora tem 0 valores ausentes. Clique no restante das colunas para ver que não há valores ausentes no conjunto de dados. 

## <a name="train-the-model"></a>Treinar o modelo

Agora que os dados estão prontos, é possível construir um modelo preditivo. Você usará seus dados para treinar o modelo. Em seguida, você testará o modelo para ver o nível de precisão com o qual ele prevê os preços.

**Classificação** e **regressão** são dois tipos de técnicas de algoritmo de machine learning supervisionado. A **classificação** prevê uma resposta com base em um conjunto definido de categorias, como uma cor (vermelho, azul ou verde). A **regressão** é usada para prever um número.

Como você deseja prever o preço, que é um número, use um algoritmo de regressão. Para este exemplo, você usará um modelo de regressão linear.

Treine o modelo fornecendo a ele um conjunto de dados que inclua o preço. O modelo examina os dados e procura correlações entre os recursos de um carro e seu preço. Em seguida, teste o modelo fornecendo a ele um conjunto de recursos para automóveis com os quais ele está familiarizado e veja qual a proximidade da previsão feita pelo modelo com o preço conhecido.

Use seus dados para treinar o modelo e testá-lo, dividindo os dados em conjuntos de dados separados de treinamento e teste.

1. Digite **dividir dados** na caixa de pesquisa para encontrar o módulo **Dividir Dados** e conectá-lo à porta esquerda do módulo **Limpar Dados Ausentes**.

1. Selecione o módulo **Dividir Dados** ao qual você acabou de se conectar para selecioná-lo. No painel Propriedades, defina a Fração de linhas no primeiro conjunto de dados de saída como 0,7. Dessa forma, usaremos 70% dos dados para treinar o modelo e manteremos 30% para o teste.

    ![Captura de tela mostrando a configuração correta do painel Propriedades. Os valores de "Dividir Dados" devem ser "Dividir Linhas", 0,7, Divisão aleatória, 0 e Falso.](./media/ui-tutorial-automobile-price-train-score/split-data.png)

1. Clique duas vezes no módulo **Dividir Dados** e digite o comentário "Dividir o conjunto de dados em um conjunto de treinamento(0,7) e um conjunto de teste(0,3)"

1. Para selecionar o algoritmo de aprendizado, limpe a caixa de pesquisa da paleta de módulos.

1. Expanda o **Aprendizado de Máquina** e, em seguida, expanda **Inicializar Modelo**. Isso exibe várias categorias de módulos que podem ser usados para inicializar os algoritmos de Aprendizado de Máquina.

1. Para este teste, selecione **Regressão** > **Regressão Linear** e arraste-a para a tela do teste.

    ![Captura de tela mostrando a configuração correta do painel Propriedades. Os valores de "Dividir Dados" devem ser "Dividir Linhas", 0,7, Divisão aleatória, 0 e Falso.](./media/ui-tutorial-automobile-price-train-score/linear-regression-module.png)

1. Encontre e arraste o módulo **Treinar Modelo** para a tela do teste. Conecte a saída do módulo Regressão Linear à entrada esquerda do módulo Treinar Modelo e conecte a saída de dados de treinamento (porta esquerda) do módulo **Dividir Dados** à entrada direita do módulo **Treinar Modelo**.

    ![Captura de tela mostrando a configuração correta do módulo Treinar Modelo. O módulo Regressão Linear se conecta à porta esquerda do módulo Treinar Modelo e o módulo Dividir Dados se conecta à porta direita do módulo Treinar Modelo](./media/ui-tutorial-automobile-price-train-score/train-model.png)

1. Selecione o módulo **Treinar Modelo**. No painel Propriedades, selecione Iniciar seletor de colunas e, em seguida, digite **preço** ao lado de **Incluir nomes de coluna**. O preço é o valor que o modelo vai prever

    ![Captura de tela mostrando a configuração correta do módulo do seletor de colunas. Com regras > Incluir nomes de coluna > "preço"](./media/ui-tutorial-automobile-price-train-score/select-price.png)

    O experimento deverá ser semelhante ao seguinte:

    ![Captura de tela mostrando a configuração correta do experimento após a adição do módulo Treinar Modelo.](./media/ui-tutorial-automobile-price-train-score/train-graph.png)

### <a name="run-the-training-experiment"></a>Executar o teste de treinamento

[!INCLUDE [aml-ui-create-training-compute](../../../includes/aml-ui-create-training-compute.md)]

## <a name="score-and-evaluate-the-model"></a>Pontuar e avaliar o modelo

Agora que você treinou o modelo usando 70% de seus dados, use-o para pontuar os outros 30% dos dados e ver se o modelo funciona bem.

1. Digite **pontuar modelo** na caixa de pesquisa para encontrar o módulo **Pontuar Modelo** e arraste o módulo para a tela do teste. Conecte a saída do módulo **Treinar Modelo** à porta de entrada esquerda do módulo **Pontuar Modelo**. Conecte a saída de dados de teste (porta direita) do módulo **Dividir Dados** à porta de entrada direita do módulo **Pontuar Modelo**.

1. Digite **avaliar** na caixa de pesquisa para encontrar **Avaliar Modelo** e arraste o módulo para a tela do teste. Conecte a saída do módulo **Pontuar Modelo** à entrada esquerda do módulo **Avaliar Modelo**. O experimento final deve se parecer como o seguinte:

    ![Captura de tela mostrando a configuração correta final do teste.](./media/ui-tutorial-automobile-price-train-score/final-graph.png)

1. Execute o teste usando o mesmo destino de computação usado anteriormente.

1. Exiba a saída do módulo **Pontuar Modelo** selecionando a porta de saída do módulo **Pontuar Modelo** e selecione **Visualizar**. A saída mostra os valores previstos para o preço e os valores conhecidos dos dados de teste.

    ![Captura de tela da visualização de saída realçando a coluna "Rótulo Pontuado"](./media/ui-tutorial-automobile-price-train-score/score-result.png)

1. Para exibir a saída do módulo Avaliar Modelo, selecione a porta de saída e, em seguida, selecione Visualizar.

    ![Captura de tela mostrando os resultados da avaliação para o teste final.](./media/ui-tutorial-automobile-price-train-score/evaluate-result.png)

As seguintes estatísticas são mostradas para o modelo:

* **MAE (Média de Erros Absolutos)** : A média de erros absolutos (um erro é a diferença entre o valor previsto e o valor real).
* **RMSE (Raiz Quadrada da Média de Erros Quadrados)** : a raiz quadrada da média de erros quadrados de previsões feitas no conjunto de dados de teste.
* **Erro absoluto relativo**: a média de erros absolutos relativos à diferença absoluta entre os valores reais e a média de todos os valores reais.
* **Erro ao quadrado relativo**: a média de erros quadrados relativos à diferença quadrada entre os valores reais e a média de todos os valores reais.
* **Coeficiente de determinação**: Também conhecido como o valor de R-quadrado, essa é uma métrica estatística que indica se o modelo se encaixa bem nos dados.

Para cada estatística de erro, menos é melhor. Um valor menor indica que as previsões se aproximam mais dos valores reais. Para Coeficiente de Determinação, quanto mais próximo o valor estiver de um (1,0), melhores as previsões.

## <a name="manage-experiments-in-azure-machine-learning-service-workspace"></a>Gerenciar testes no workspace do Serviço do Azure Machine Learning

Os testes criados na interface visual podem ser gerenciados no workspace do Serviço do Azure Machine Learning. Use o workspace para ver informações mais detalhadas, como execuções de teste de indivíduos, logs de diagnóstico, grafos de execução, entre outros.

1. Abra seu workspace na [portal do Azure](https://portal.azure.com/).  

1. No workspace, selecione **Testes**. Em seguida, selecione o teste que você criou.

    ![Captura de tela mostrando como navegar para os testes no portal do Azure](./media/ui-tutorial-automobile-price-train-score/portal-experiments.png)

    Nessa página, você terá uma visão geral do teste e de suas execuções mais recentes.

    ![Captura de tela mostrando uma visão geral das estatísticas do teste no portal do Azure](./media/ui-tutorial-automobile-price-train-score/experiment-overview.png)

1. Selecione um número de execução para ver mais detalhes sobre uma execução específica.

    ![Captura de tela do relatório de execução detalhado](./media/ui-tutorial-automobile-price-train-score/run-details.png)

    O relatório de execução é atualizado em tempo real. Se você usou um módulo **Executar Script do Python** no teste, especifique os logs de script a serem gerados na guia **Logs**.

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [aml-ui-cleanup](../../../includes/aml-ui-cleanup.md)]

## <a name="next-steps"></a>Próximas etapas

Na primeira parte deste tutorial, você concluiu estas etapas:

* Criou um experimento
* Preparar os dados
* Treinar o modelo
* Pontuar e avaliar o modelo

Na segunda parte, você aprenderá a implantar seu modelo como um serviço Web do Azure.

> [!div class="nextstepaction"]
> [Continuar a implantação de modelos](ui-tutorial-automobile-price-deploy.md)
