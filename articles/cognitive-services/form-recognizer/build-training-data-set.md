---
title: Como criar um conjunto de dados de treinamento para um modelo personalizado - reconhecedor de formulário
titleSuffix: Azure Cognitive Services
description: Saiba como garantir que seu conjunto de dados de treinamento é otimizado para treinar um modelo de formulário reconhecedor.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: pafarley
ms.openlocfilehash: 611d5f7983c61fab12c55a46fedf35a3c420c4c8
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454813"
---
# <a name="build-a-training-data-set-for-a-custom-model"></a>Criar um conjunto de dados de treinamento para um modelo personalizado

Quando você usa o modelo de formulário reconhecedor personalizado, você fornecer seus próprios dados de treinamento para que o modelo poderá fazê-lo aos seus formulários específicos do setor. Você pode treinar um modelo com cinco formulários preenchidos ou um formulário vazio (incluir a palavra "empty" no nome do arquivo) além de dois formulários preenchidos. Mesmo se você tiver suficiente formulários preenchidos para treinar com, a adição de um formulário vazio ao seu conjunto de dados de treinamento pode melhorar a precisão do modelo.

## <a name="training-data-tips"></a>Dicas de dados de treinamento

É importante usar um conjunto de dados que é otimizado para treinamento. Use as dicas a seguir para garantir que você obtém os melhores resultados do [modelo de treinamento](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/TrainCustomModel) operação:

* Se possível, use os documentos em PDF baseado em texto, em vez de documentos com base em imagem. PDFs digitalizados são tratadas como imagens.
* Use um formulário vazio e dois formulários preenchidos, se você os tiver disponíveis.
* Para formulários preenchidos, use os exemplos que têm todos os seus campos preenchidos.
* Use formulários com valores diferentes em cada campo.
* Se suas imagens de formulário de qualidade inferior, use um conjunto de dados maior (10 a 15 imagens, por exemplo).

## <a name="general-input-requirements"></a>Requisitos gerais de entrada

Verifique se que seu conjunto de dados de treinamento também segue os requisitos de entrada para todo o conteúdo do reconhecedor de formulário.
[!INCLUDE [input requirements](./includes/input-requirements.md)]

## <a name="next-steps"></a>Próximas etapas

Agora que você aprendeu como criar um conjunto de dados de treinamento, siga um início rápido para treinar um modelo de formulário reconhecedor personalizado e começar a usá-lo em seus formulários.

* [Início Rápido: Treinar um modelo e extrair dados de formulário usando o cURL](./quickstarts/curl-train-extract.md)
* [Início Rápido: Treinar um modelo e extrair dados de formulário usando a API REST com o Python](./quickstarts/python-train-extract.md)

