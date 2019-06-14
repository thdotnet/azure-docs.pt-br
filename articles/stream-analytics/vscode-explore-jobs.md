---
title: Explore os trabalhos do Azure Stream Analytics com o Visual Studio Code (visualização)
description: Este artigo mostra como exportar um trabalho do Azure Stream Analytics para um projeto local, listar trabalhos e entidades de trabalho do modo de exibição.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 05/15/2019
ms.topic: conceptual
ms.openlocfilehash: 8674d478646c8f9be6b32521c6624752ac6df052
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65827794"
---
# <a name="explore-azure-stream-analytics-with-visual-studio-code-preview"></a>Explorar o Azure Stream Analytics com o código do Visual Studio (versão prévia)

O Azure Stream Analytics para a extensão do Visual Studio Code fornece aos desenvolvedores uma experiência leve para gerenciar seus trabalhos do Stream Analytics. Ele pode ser usado no Windows, Mac e Linux. Com a extensão do Azure Stream Analytics, você pode:

- [Criar](quick-create-vs-code.md), iniciar e parar trabalhos
- Exportar os trabalhos existentes em um projeto local
- Listar trabalhos e exibir as entidades de trabalho

## <a name="export-a-job-to-a-local-project"></a>Exportar um trabalho para um projeto local

Para exportar um trabalho para um projeto local, localize o trabalho que você deseja exportar na **Stream Analytics Explorer** no Visual Studio Code. Em seguida, selecione uma pasta para o seu projeto. O projeto é exportado para a pasta selecionada, e você pode continuar a gerenciar o trabalho do Visual Studio Code. Para obter mais informações sobre como usar o Visual Studio Code para gerenciar trabalhos do Stream Analytics, consulte o Visual Studio Code [quickstart](quick-create-vs-code.md).

![Exportar trabalho ASA no Visual Studio Code](./media/vscode-explore-jobs/export-job.png)

## <a name="list-job-and-view-job-entities"></a>Listar o trabalho e exibir as entidades de trabalho

Você pode usar o modo de exibição de trabalho para interagir com trabalhos do Azure Stream Analytics do Microsoft Visual Studio.


1. Clique o **Azure** ícone na barra de atividade de código do Visual Studio e, em seguida, expanda **nó do Stream Analytics**. Os trabalhos devem aparecer em suas assinaturas.

   ![Abrir do Stream Analytics Explorer](./media/vscode-explore-jobs/open-explorer.png)

2. Expanda o nó de trabalho, você pode abrir e exibir a consulta de trabalho, configuração, entradas, saídas e funções. 

3. Clique com botão direito no nó do seu trabalho e escolha o **abra a exibição de trabalho no Portal** o nó para abrir a exibição de trabalho no portal do Azure.

   ![Exibição de trabalho aberto no portal](./media/vscode-explore-jobs/open-job-view.png)

## <a name="next-steps"></a>Próximas etapas

* [Criar um trabalho de nuvem do Azure Stream Analytics no Visual Studio Code (visualização)](quick-create-vs-code.md)
