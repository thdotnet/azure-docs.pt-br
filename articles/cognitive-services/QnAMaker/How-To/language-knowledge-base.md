---
title: Base de dados de conhecimento em idiomas além do inglês – QnA Maker
titleSuffix: Azure Cognitive Services
description: O QnA Maker dá suporte a conteúdo da base de dados de conhecimento em vários idiomas. No entanto, cada serviço de QnA Maker deve ser reservado para um único idioma. A primeira base de dados de conhecimento criada para um determinado serviço de QnA Maker define o idioma desse serviço.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: diberry
ms.openlocfilehash: 5e50c814fef24aa799549d055ad6496f5bdf05e0
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70961495"
---
# <a name="language-support-of-knowledge-base-content-for-qna-maker"></a>Suporte a idioma do conteúdo da base de dados de conhecimento para QnA Maker

O QnA Maker dá suporte a conteúdo da base de dados de conhecimento em vários idiomas. No entanto, cada serviço de QnA Maker deve ser reservado para um único idioma. A primeira base de dados de conhecimento criada para um determinado serviço de QnA Maker define o idioma desse serviço. Consulte [aqui](../Overview/languages-supported.md) para a lista de idiomas com suporte.

O idioma é automaticamente reconhecido a partir do conteúdo das fontes de dados que estão sendo extraídas. Após criar um novo Serviço de QnA Maker e uma nova Base de Dados de Conhecimento nesse serviço, você poderá verificar se o idioma foi definido corretamente.

1. Navegue até o [Portal do Azure](https://portal.azure.com/).

1. Selecione **grupos de recurso** e navegue até o grupo de recursos onde o serviço de QnA Maker está implantado e selecione o recurso **Azure Search**.

    ![Selecionar o recurso do Azure Search](../media/qnamaker-how-to-language-kb/select-azsearch.png)

1. Selecione **índices**e, em seguida, selecione o índice **testkb** . Esse é o primeiro índice de Azure Search criado e contém o conteúdo salvo de todas as bases de dados de conhecimento nesse serviço. 

1. Selecione **campos** para ver os campos no índice.

1. A coluna do _analisador_ `questions` dos `answer` campos e é definida como um idioma específico. Esse idioma foi detectado automaticamente durante a etapa de criação da base de dados de conhecimento dos arquivos e URLs importados. Este idioma não poderá ser alterado depois que o recurso for criado.

    ![Analisador selecionado](../media/qnamaker-how-to-language-kb/selected-analyzer.png)

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Criar um bot do QnA com o Serviço de Bot do Azure](../Tutorials/create-qna-bot.md)
