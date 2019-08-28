---
title: Crie e gerencie os workspaces
titleSuffix: Azure Machine Learning service
description: Saiba como criar, exibir e excluir workspaces do Azure Machine Learning no portal do Azure.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: shipatel
author: shivp950
ms.date: 05/10/2019
ms.custom: seodec18
ms.openlocfilehash: 81e1104d71706194ba1c54e42722b4508df09091
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69534860"
---
# <a name="create-and-manage-azure-machine-learning-service-workspaces"></a>Criar e gerenciar workspaces de serviço do Azure Machine Learning

Neste artigo, você vai criar, exibir e excluir [**Workspaces do Azure Machine Learning**](concept-workspace.md) no portal do Azure para o [serviço do Azure Machine Learning](overview-what-is-azure-ml.md).  O portal é a maneira mais fácil de começar com espaços de trabalho, mas conforme suas necessidades mudam ou requisitos para aumentar a automação, você também pode criar e excluir espaços de trabalho [usando a CLI](reference-azure-machine-learning-cli.md), [com código Python](https://aka.ms/aml-sdk) ou [por meio da extensão vs Code](how-to-vscode-tools.md#get-started-with-azure-machine-learning).

## <a name="create-a-workspace"></a>Criar um espaço de trabalho

Para criar um workspace, você precisa de uma assinatura do Azure. Se você não tiver uma assinatura do Azure, crie uma conta gratuita antes de começar. Experimente a [versão gratuita ou paga do Serviço do Azure Machine Learning](https://aka.ms/AMLFree) hoje mesmo.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

### <a name="download-a-configuration-file"></a>Baixar um arquivo de configuração

1. Se você for criar uma [VM do bloco de anotações](tutorial-1st-experiment-sdk-setup.md#azure), ignore esta etapa.

1. Se você planeja usar código em seu ambiente local que referencia esse workspace, selecione **Baixar config.json** da seção **Visão geral** do workspace.  

   ![Baixe o config.json](./media/how-to-manage-workspace/configure.png)
   
   Coloque o arquivo na estrutura de diretório com seus scripts de Python ou Jupyter Notebooks. Ele pode estar no mesmo diretório, um subdiretório denominado *.azureml* ou em um diretório pai. Quando você cria uma VM de notebook, esse arquivo é adicionado ao diretório correto na VM para você.


## <a name="view"></a>Exibir um espaço de trabalho

1. No canto superior esquerdo do portal, selecione **Todos os serviços**.

1. No campo filtro **todos os serviços** , digite **serviço de Machine Learning**.  

1. Selecione **Machine Learning espaços de trabalho de serviço**.

   ![Procurar Azure Machine Learning espaço de trabalho do serviço](media/how-to-manage-workspace/all-services.png)

1. Examine a lista de workspaces encontrada. É possível filtrar com base na assinatura, grupos de recursos e locais.  

1. Selecione um espaço de trabalho para exibir suas propriedades.
   ![Propriedades do espaço de trabalho](media/how-to-manage-workspace/allservices_view_workspace_full.PNG)

## <a name="delete-a-workspace"></a>Excluir um workspace

Use o botão Excluir na parte superior do workspace que você deseja excluir.

  ![Botão Excluir](media/how-to-manage-workspace/delete-workspace.png)

## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Próximas etapas

Siga o tutorial completo para aprender a usar um workspace para criar, treinar e implantar modelos com o serviço do Azure Machine Learning.

> [!div class="nextstepaction"]
> [Tutorial: Treinar modelos](tutorial-train-models-with-aml.md)
