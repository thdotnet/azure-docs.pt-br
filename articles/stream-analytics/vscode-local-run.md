---
title: Testar as consultas do Azure Stream Analytics localmente com o Visual Studio Code (visualização)
description: Este artigo descreve como testar consultas localmente com o Azure Stream Analytics Tools para Visual Studio Code.
ms.service: stream-analytics
author: su-jie
ms.author: sujie
ms.date: 05/15/2019
ms.topic: conceptual
ms.openlocfilehash: f477a0f99c3eaa82568d8188bfaae03818fb72dc
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827944"
---
# <a name="test-stream-analytics-queries-locally-with-visual-studio-code"></a>Testar as consultas do Stream Analytics localmente com o Visual Studio Code

Você pode usar o Azure Stream Analytics tools para Visual Studio Code para testar seus trabalhos do Stream Analytics localmente com os dados de exemplo.

Use esta [quickstart](quick-create-vs-code.md) para aprender a criar um trabalho de Stream Analytics usando o Visual Studio Code.

## <a name="run-queries-locally"></a>Executar consultas localmente

Você pode usar a extensão do Azure Stream Analytics para Visual Studio Code para testar seus trabalhos do Stream Analytics localmente com os dados de exemplo.

1. Depois de criar o trabalho do Stream Analytics, use **Ctrl + Shift + P** para abrir a paleta de comandos. Em seguida, digite e selecione **ASA: Adicionar Entrada**.

    ![Adicionar entrada do ASA no Visual Studio code](./media/vscode-local-run/add-input.png)

2. Selecione **entrada Local**.

    ![Adicionar entrada de local de ASA no Visual Studio code](./media/vscode-local-run/add-local-input.png)

3. Selecione **+ nova entrada Local**.

    ![Adicionar um local de ASA nova entrada no Visual Studio code](./media/vscode-local-run/add-new-local-input.png)

4. Insira o mesmo alias de entrada que você usou em sua consulta.

    ![Adicionar um novo alias de entrada de ASA local](./media/vscode-local-run/new-local-input-alias.png)

5. No **LocalInput_DefaultLocalStream.json** de arquivos, insira o caminho do arquivo onde se encontra o arquivo de dados local.

    ![Insira o caminho do arquivo local no Visual Studio](./media/vscode-local-run/local-file-path.png)

6. Retornar para o seu editor de consulta e selecione **executados localmente**.

    ![Selecione Executar localmente no editor de consultas](./media/vscode-local-run/run-locally.png)

## <a name="next-steps"></a>Próximas etapas

* [Criar um trabalho de nuvem do Azure Stream Analytics no Visual Studio Code (visualização)](quick-create-vs-code.md)

* [Explore os trabalhos do Azure Stream Analytics com o Visual Studio Code (visualização)](vscode-explore-jobs.md)
