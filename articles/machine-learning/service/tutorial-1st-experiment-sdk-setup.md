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
ms.date: 07/20/2019
ms.openlocfilehash: cc16f3c8ea287e78d7b7b4d9a56f5a2e82c26f01
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69515331"
---
# <a name="tutorial-get-started-creating-your-first-ml-experiment"></a>Tutorial: Introdução à criação do seu primeiro experimento de ML

Neste tutorial, você concluirá as etapas de ponta a ponta para começar a usar o SDK de Python do Azure Machine Learning em execução em notebooks Jupyter. Este tutorial é **parte um de uma série de tutoriais de duas partes** e aborda a configuração e instalação do ambiente Python, bem como a criação de um workspace para gerenciar seus experimentos e modelos de machine learning. [**A parte dois**](tutorial-1st-experiment-sdk-train.md) se baseia nisso para treinar vários modelos de machine learning e apresentar o processo de gerenciamento de modelos usando o portal do Azure e o SDK.

Neste tutorial, você irá:

> [!div class="checklist"]
> * Criar um workspace do machine learning para usar no próximo tutorial.
> * Criar uma VM de Jupyter Notebook baseada em nuvem com o SDK do Python do Azure Machine Learning instalado e pré-configurado.

## <a name="prerequisites"></a>Pré-requisitos

O único pré-requisito para este tutorial é uma assinatura do Azure. Se você não tiver uma assinatura do Azure, crie uma conta gratuita antes de começar. Experimente a [versão gratuita ou paga do Serviço do Azure Machine Learning](https://aka.ms/AMLFree) hoje mesmo.

## <a name="create-a-workspace"></a>Criar um workspace

Um workspace é o recurso fundamental na nuvem que você usa para experimentar, treinar e implantar modelos de machine learning. Ele vincula sua assinatura do Azure e o grupo de recursos a um objeto facilmente consumido no SDK. Se você já tiver um workspace do Serviço do Azure Machine Learning, passe para a [próxima seção](#azure). Caso contrário, crie um agora.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="azure"></a>Criar um servidor de notebook em nuvem

Este exemplo usa o servidor de notebook em nuvem em seu workspace para uma experiência pré-configurada e sem instalação. Use [seu próprio ambiente](how-to-configure-environment.md#local) se preferir ter controle sobre seu ambiente, pacotes e dependências.

Em seu workspace, você cria um recurso de nuvem para começar a usar notebooks Jupyter. Esse recurso é uma máquina virtual do Linux baseada em nuvem pré-configurada com tudo de que você precisa para executar o Serviço do Azure Machine Learning.

1. Abra seu workspace na [portal do Azure](https://portal.azure.com/).  Se você não souber como localizar seu workspace no portal, veja como [localizar seu workspace](how-to-manage-workspace.md#view).

1. Na página do seu workspace no portal do Azure, selecione **VMs de Notebook** à esquerda.

1. Selecione **+Novo** para criar uma VM de notebook.

     ![Selecione Nova VM](./media/tutorial-1st-experiment-sdk-setup/add-workstation.png)

1. Forneça um nome para sua VM. Em seguida, selecione **Criar**.

    > [!NOTE]
    > O nome da VM do seu Notebook deve ter entre 2 e 16 caracteres. Os caracteres válidos são letras, dígitos e o caractere -.  O nome também deve ser exclusivo em toda a sua assinatura do Azure.

1. Aguarde até que o status seja alterado para **Em execução**.

### <a name="launch-jupyter-web-interface"></a>Iniciar interface da Web do Jupyter

Depois que sua VM estiver em execução, use a seção **VMs de Notebook** para iniciar a interface da Web do Jupyter.

1. Selecione **Jupyter** na coluna **URI** para sua VM.

    ![Iniciar o servidor do Jupyter notebook](./media/tutorial-1st-experiment-sdk-setup/start-server.png)

    O link inicia seu servidor de notebook e abre a página da Web do Jupyter notebook em uma nova guia do navegador.  O link só funcionará para a pessoa que cria a VM. Cada usuário do workspace deve criar sua própria VM.

1. Na página da Web do Jupyter notebook, o nome da pasta superior é o seu nome de usuário.  Selecione esta pasta.

    > [!TIP]
    > Esta pasta está localizada no [contêiner de armazenamento](concept-workspace.md#resources) em seu espaço de trabalho, não na VM do notebook.  Você pode excluir a VM do notebook e ainda manter todo o seu trabalho.  Quando você criar uma nova VM do notebook posteriormente, ela será carregada dessa mesma pasta. Se você compartilhar seu workspace com outras pessoas, elas verão sua pasta e você verá as pastas delas.

1. Abra o subdiretório `samples-*` e, em seguida, abra `tutorials/tutorial-1st-experiment-sdk-train.ipynb` para executar a **parte dois** do tutorial.

## <a name="end"></a> Limpar recursos

Não conclua esta seção se você planeja continuar na **parte 2** do tutorial.

### <a name="stop-the-notebook-vm"></a>Interromper a VM de notebook

Se você usou um servidor de notebook de nuvem, pare a VM quando não a estiver usando para reduzir o custo.

1. Em seu workspace, selecione **VMs de Notebook**.

   ![Interromper o servidor da VM](./media/tutorial-1st-experiment-sdk-setup/stop-server.png)

1. Selecione a VM na lista.

1. Selecione **Interromper**.

1. Quando estiver pronto para usar o servidor novamente, selecione **Iniciar**.

### <a name="delete-everything"></a>Excluir tudo

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Você também pode manter o grupo de recursos, mas excluir um único workspace. Exiba as propriedades do workspace e, em seguida, selecione **Excluir**.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você concluiu estas tarefas:

* Criou um workspace do Serviço do Azure Machine Learning.
* Criou e configurou um servidor de notebook em nuvem no seu workspace.

Continue com a **parte 2** deste tutorial para treinar um modelo de machine learning simples.

> [!div class="nextstepaction"]
> [Tutorial: Treinar seu primeiro modelo](tutorial-1st-experiment-sdk-train.md)
