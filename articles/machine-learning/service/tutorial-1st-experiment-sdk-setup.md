---
title: 'Tutorial: Criar seu primeiro experimento de ML: Configuração'
titleSuffix: Azure Machine Learning service
description: Nesta série de tutoriais, você concluirá as etapas de ponta a ponta para começar a usar o SDK de Python do Azure Machine Learning em execução em notebooks Jupyter.  A parte um abrange a criação de um ambiente de servidor de notebook de nuvem, bem como a criação de um workspace para gerenciar seus experimentos e modelos de machine learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 08/28/2019
ms.openlocfilehash: d968d6e799b75940d1fb73aa31c22eb84068df7d
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70860415"
---
# <a name="tutorial-get-started-creating-your-first-ml-experiment-with-the-python-sdk"></a>Tutorial: Introdução à criação de seu primeiro experimento de ML com o SDK de Python

Neste tutorial, você concluirá as etapas de ponta a ponta para começar a usar o SDK de Python do Azure Machine Learning em execução em notebooks Jupyter. Este tutorial é **parte um de uma série de tutoriais de duas partes** e aborda a configuração e instalação do ambiente Python, bem como a criação de um workspace para gerenciar seus experimentos e modelos de machine learning. [**A parte dois**](tutorial-1st-experiment-sdk-train.md) se baseia nisso para treinar vários modelos de machine learning e apresentar o processo de gerenciamento de modelos usando o portal do Azure e o SDK.

Neste tutorial, você irá:

> [!div class="checklist"]
> * Criar um [Workspace do Azure Machine Learning](concept-workspace.md) para usar no próximo tutorial.
> * Criar uma VM de Jupyter Notebook baseada em nuvem com o SDK do Python do Azure Machine Learning instalado e pré-configurado.

Se você não tiver uma assinatura do Azure, crie uma conta gratuita antes de começar. Experimente a [versão gratuita ou paga do Serviço do Azure Machine Learning](https://aka.ms/AMLFree) hoje mesmo.

## <a name="create-a-workspace"></a>Criar um workspace

Um Workspace do Azure Machine Learning é o recurso fundamental na nuvem que você usa para experimentar, treinar e implantar modelos de machine learning. Ele vincula sua assinatura do Azure e o grupo de recursos a um objeto facilmente consumido no SDK. Se você já tiver um workspace do Serviço do Azure Machine Learning, passe para a [próxima seção](#azure). Caso contrário, crie um agora.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="azure"></a>Criar um servidor de notebook em nuvem

Este exemplo usa o servidor de notebook em nuvem em seu workspace para uma experiência pré-configurada e sem instalação. Use [seu próprio ambiente](how-to-configure-environment.md#local) se preferir ter controle sobre seu ambiente, pacotes e dependências.

Em seu workspace, você cria um recurso de nuvem para começar a usar notebooks Jupyter. Esse recurso é uma máquina virtual do Linux baseada em nuvem pré-configurada com tudo de que você precisa para executar o Serviço do Azure Machine Learning.

1. Abra seu workspace na [portal do Azure](https://portal.azure.com/).  Se você não souber como localizar seu workspace no portal, veja como [localizar seu workspace](how-to-manage-workspace.md#view).

1. Na página do seu workspace no portal do Azure, selecione **VMs de Notebook** à esquerda.

1. Selecione **+Novo** para criar uma VM de notebook.

     ![Selecione Nova VM](./media/tutorial-1st-experiment-sdk-setup/add-workstation.png)

1. Forneça um nome para sua VM. 
   + O nome da VM do seu Notebook deve ter entre 2 e 16 caracteres. Os caracteres válidos são letras, dígitos e o caractere -.  
   + O nome também deve ser exclusivo em toda a sua assinatura do Azure.

1. Em seguida, selecione **Criar**. Pode levar um tempo para configurar sua VM.

1. Aguarde até que o status seja alterado para **Em execução**.
   Depois que sua VM estiver em execução, use a seção **VMs de Notebook** para iniciar a interface da Web do Jupyter.

1. Selecione **Jupyter** na coluna **URI** para sua VM.

    ![Iniciar o servidor do Jupyter notebook](./media/tutorial-1st-experiment-sdk-setup/start-server.png)

   O link inicia seu servidor de notebook e abre a página da Web do Jupyter notebook em uma nova guia do navegador.  O link só funcionará para a pessoa que cria a VM. Cada usuário do workspace deve criar sua própria VM.


## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você concluiu estas tarefas:

* Criou um workspace do Serviço do Azure Machine Learning.
* Criou e configurou um servidor de notebook em nuvem no seu workspace.

Na **parte dois** do tutorial, você executa o código em `tutorial-1st-experiment-sdk-train.ipynb` para treinar um modelo de machine learning. 

> [!div class="nextstepaction"]
> [Tutorial: Treinar seu primeiro modelo](tutorial-1st-experiment-sdk-train.md)

> [!IMPORTANT]
> Se não planeja seguir a parte 2 deste tutorial ou nenhum outro tutorial, você deve [interromper a VM do servidor de notebook na nuvem](tutorial-1st-experiment-sdk-train.md#clean-up-resources) quando não a estiver usando para reduzir o custo.


