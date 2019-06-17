---
title: Interface visual
titleSuffix: Azure Machine Learning service
description: Saiba mais sobre os termos, conceitos e fluxo de trabalho que compõem a interface visual (visualização) para o serviço de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/15/2019
ms.openlocfilehash: be07e0f3438ea93312d4eb440e7e63b8f98e11b8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077369"
---
# <a name="what-is-the-visual-interface-for-azure-machine-learning-service"></a>O que é a interface visual para o serviço de Azure Machine Learning? 

A interface visual (visualização) para o serviço de Azure Machine Learning permite que você preparar dados, treinar, testar, implantar, gerenciar e acompanhar os modelos de aprendizado de máquina sem precisar escrever código.

Não há necessidade de programação, conectar-se visualmente [datasets](#dataset) e [módulos](#module) para construir seu modelo.

A interface visual usa o serviço de Azure Machine Learning [espaço de trabalho](concept-workspace.md) para:

+ Gravar artefatos da [experimentar](#experiment) é executado no espaço de trabalho.
+ Acesso [conjuntos de dados](#dataset).
+ Use o [recursos de computação](#compute) no espaço de trabalho para executar o experimento. 
+ Registre [modelos](concept-azure-machine-learning-architecture.md#models).
+ [Implantar](#deployment) modelos como serviços da web em recursos de computação no espaço de trabalho.

![Visão geral da interface visual](media/ui-concept-visual-interface/overview.png)

## <a name="workflow"></a>Fluxo de trabalho

A interface visual fornece uma tela visual, interativa para rapidamente criar, testar e iterar em um modelo. 

+ Você arrastar e soltar [módulos](#module) para a tela.
+ Conectar os módulos juntos para formar uma [experimentar](#experiment).
+ Execute o experimento usando o recurso de computação do espaço de trabalho do serviço de Machine Learning.
+ Itere em seu design de modelo, editando o teste e executá-la novamente.
+ Quando você estiver pronto, converta seu **teste de treinamento** para um **teste preditivo**.
+ [Implantar](#deployment) o teste preditivo como um web service para que seu modelo pode ser acessado por outras pessoas.

## <a name="experiment"></a>Experimento

Criar um experimento a partir do zero ou usar um experimento de exemplo existente como um modelo.  Sempre que você executar um experimento, artefatos são armazenados em seu espaço de trabalho.

Um teste consiste em conjuntos de dados e módulos analíticos, que você conecta para construir um modelo. Especificamente, um teste válido possui três características:

* Conjuntos de dados podem estar conectados apenas a módulos.
* Módulos podem estar conectados para conjuntos de dados ou outros módulos.
* Todas as portas de entrada para os módulos devem ter alguma conexão ao fluxo de dados.
* Todos os necessários parâmetros para cada módulo devem ser definidos.

Para obter um exemplo de um experimento simple, consulte [guia de início rápido: Preparar e visualizar os dados sem escrever código no Azure Machine Learning](ui-quickstart-run-experiment.md).

Para obter uma explicação mais completa de uma solução de análise preditiva, consulte [Tutorial: Prever o preço de automóveis com a interface visual](ui-tutorial-automobile-price-train-score.md).

## <a name="dataset"></a>Conjunto de dados

Um conjunto de dados é dados que foi carregados para a interface visual para uso no processo de modelagem. Um número de conjuntos de dados de exemplo está incluído para que você possa experimentar, e você pode carregar mais conjuntos de dados conforme necessário.

## <a name="module"></a>Módulo

Um módulo é um algoritmo que você pode executar em seus dados. A interface visual tem um número de módulos que variam de funções de entrada de dados para treinamento, pontuação e processos de validação.

Um módulo pode ter um conjunto de parâmetros que você pode usar para configurar os algoritmos internos do módulo. Quando você seleciona um módulo nas telas, os parâmetros do módulo são exibidos no painel à direita da tela de propriedades. Você pode modificar os parâmetros nesse painel para ajustar seu modelo.

![Propriedades do módulo](media/ui-concept-visual-interface/properties.png)

Para alguns ajuda navegando por meio da biblioteca de algoritmos de aprendizado de máquina disponíveis, consulte [visão geral de referência de módulo e algoritmos](../algorithm-module-reference/module-reference.md)

## <a name="compute"></a> Recursos de computação

Recursos do espaço de trabalho para executar seu experimento ou host seus modelos implantados como serviços da web de computação de uso. Os destinos de computação com suporte são:


| Destino de computação | Treinamento | Implantação |
| ---- |:----:|:----:|
| Computação do Azure Machine Learning | ✓ | |
| Serviço de Kubernetes do Azure | | ✓ |

Destinos são anexados ao seu aprendizado de máquina de computação [espaço de trabalho](concept-workspace.md). Gerenciar seus destinos de computação em seu espaço de trabalho na [portal do Azure](https://portal.azure.com).

## <a name="deployment"></a>Implantação

Depois que o modelo de análise preditiva estiver pronto, implantá-lo como um serviço web diretamente da interface visual.

Os serviços web fornecem uma interface entre um aplicativo e seu modelo de pontuação. Um aplicativo externo pode se comunicar com o modelo de pontuação em tempo real. Uma chamada para um serviço web retorna resultados de previsão para um aplicativo externo. Para fazer uma chamada a um serviço Web, você passa uma chave de API que foi criada quando você implantou o serviço Web. O serviço web baseia-se em REST, uma opção popular de arquitetura para projetos de programação da web.

Para saber como implantar seu modelo, consulte [Tutorial: Implantar um modelo de aprendizado de máquina com a interface visual](ui-tutorial-automobile-price-deploy.md).

## <a name="next-steps"></a>Próximas etapas

* Conheça os fundamentos da análise preditiva e machine learning com [guia de início rápido: Preparar e visualizar os dados sem escrever código no Azure Machine Learning](ui-quickstart-run-experiment.md).
* Use um dos exemplos e modifique Suite às suas necessidades:
    * [Exemplo 1: regressão: Prever o preço](ui-sample-regression-predict-automobile-price-basic.md)
    * [Exemplo 2: regressão: Prever o preço e comparar algoritmos](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
    * [Exemplo 3: classificação: Prever o risco de crédito](ui-sample-classification-predict-credit-risk-basic.md)
    * [Exemplo 4: classificação: Prever o risco de crédito (custo confidencial)](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
    * [Exemplo 5: classificação: Prever a rotatividade, desejo e venda](ui-sample-classification-predict-churn.md)
