---
title: Exemplos de Notebooks Jupyter
titleSuffix: Azure Machine Learning service
description: Encontre e use notebooks Jupyter de exemplo para explorar o SDK do Python no Serviço do Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 07/31/2019
ms.custom: seodec18
ms.openlocfilehash: 7fdf9c52df502bc94519ab6c65be2e9fb546ce48
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68699130"
---
# <a name="explore-azure-machine-learning-service-with-jupyter-notebooks"></a>Explore o serviço do Azure Machine Learning com notebooks Jupyter

O [exemplo de repositório de Notebooks do Azure Machine Learning](https://github.com/azure/machinelearningnotebooks) inclui as amostras mais recentes do SDK do Python do Azure Machine Learning. Esses notebooks Jupyter foram projetados para ajudá-lo a explorar o SDK e servem como modelos para seus próprios projetos de aprendizado de máquina.

Este artigo mostra como acessar o repositório nos seguintes ambientes:

- [VM do Notebook do Azure Machine Learning](#notebookvm)
- [Trazer seu próprio servidor de notebook](#byo)
- [Máquina Virtual de Ciência de Dados](#dsvm)
- [Azure Notebooks](#aznb)

> [!NOTE]
> Depois de clonar o repositório, você encontrará notebooks do tutorial na pasta **tutorials** e notebooks específicos do recurso na pasta **how-to-use-azureml**.

<a name="notebookvm"></a>
## <a name="get-samples-on-azure-machine-learning-notebook-vm"></a>Obtenha amostras da VM do Notebook do Azure Machine Learning

A maneira mais fácil de começar a usar as amostras é concluir o [início rápido de notebook baseado em nuvem](quickstart-run-cloud-notebook.md). Após a conclusão, você terá um servidor de notebook dedicado pré-carregado com o SDK e o repositório de exemplo. Não há necessidade de downloads nem de instalação.

<a name="byo"></a>

## <a name="get-samples-on-your-notebook-server"></a>Obter amostras em seu servidor de notebook

Caso deseje trazer seu próprio servidor de notebook para o desenvolvimento local, siga estas etapas:

[!INCLUDE [aml-your-server](../../../includes/aml-your-server.md)]

Essas instruções instalam os pacotes base do SDK necessários para o início rápido e os notebooks do tutorial. Outros notebooks de exemplo podem exigir a instalação de componentes extras. Para obter mais informações, confira [Instalar o SDK do Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/install).

<a name="dsvm"></a>
## <a name="get-samples-on-dsvm"></a>Obter exemplos no DSVM

A DSVM (Máquina Virtual de Ciência de Dados) é uma imagem de VM personalizada, especificamente criada para ciência de dados. Se você [criar uma DSVM](how-to-configure-environment.md#dsvm), o SDK e o servidor de notebook serão instalados e configurados para você. No entanto, você ainda precisará criar um workspace e clonar o repositório de exemplo.

[!INCLUDE [aml-dsvm-server](../../../includes/aml-dsvm-server.md)]

<a name="aznb"></a>
## <a name="get-samples-on-azure-notebooks"></a>Obter amostras do Azure Notebooks

No [Azure Notebooks](https://notebooks.azure.com/), o SDK e o servidor de notebook são instalados e configurados para você. O Azure Notebooks fornece um ambiente de notebook leve e totalmente gerenciado para você explorar.

Para acessar o repositório de exemplo no Azure Notebooks, navegue até o workspace do Azure Machine Learning por meio do [portal do Azure](https://portal.azure.com). Na seção **Visão Geral**, selecione **Introdução ao Azure Notebooks**.

## <a name="next-steps"></a>Próximas etapas

Explore os [notebooks de exemplo](https://aka.ms/aml-notebooks) para descobrir o que o Serviço do Azure Machine Learning pode fazer ou experimente estes tutoriais:

- [Treinar e implantar um modelo de classificação de imagem com MNIST](tutorial-train-models-with-aml.md)

- [Preparar os dados e usar aprendizado de máquina automatizado para treinar um modelo de regressão com o conjunto de dados de táxi de NYC](tutorial-data-prep.md)
