---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 01/25/2019
ms.openlocfilehash: a882a874574395095e98079cd0f8aa4a4987c749
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391770"
---
1. [Criar um workspace do Serviço do Azure Machine Learning](../articles/machine-learning/service/setup-create-workspace.md).

1. Clone [o repositório do GitHub](https://aka.ms/aml-notebooks).

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Adicione um arquivo de configuração do workspace ao diretório clonado usando um destes métodos:

    * No [portal do Azure](https://ms.portal.azure.com), selecione **Baixar config.json** na seção **Visão geral** do seu espaço de trabalho. 

    ![Baixe o config.json](./media/aml-dsvm-server/download-config.png)

    * Criar um novo workspace usando o código no notebook [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) em seu diretório clonado.

1. Inicie o servidor de notebook do seu diretório clonado.

    ```shell
    jupyter notebook
    ```