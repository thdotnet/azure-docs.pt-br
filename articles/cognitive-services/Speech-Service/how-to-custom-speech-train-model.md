---
title: Treinar um modelo de fala personalizado - serviços de fala
titlesuffix: Azure Cognitive Services
description: Um conversão de fala em texto de treinamento é necessário para melhorar a precisão do reconhecimento de modelo de linha de base da Microsoft ou de um modelo personalizado que você pretende criar. Um modelo é treinado usando transcrições rotulada como humanos e texto relacionado. Esses conjuntos de dados, juntamente com os dados de áudio carregados anteriormente, são usados para refinar e treinar o modelo de fala em texto para reconhecer palavras, frases, acrônimos, nomes e outros termos de produto específico.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 49195efa54c8e6eca9186a9e2fc33da84ff34413
ms.sourcegitcommit: c0419208061b2b5579f6e16f78d9d45513bb7bbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67625382"
---
# <a name="train-a-model-for-custom-speech"></a>Treinar um modelo de fala personalizado

Um conversão de fala em texto de treinamento é necessário para melhorar a precisão do reconhecimento de modelo de linha de base da Microsoft ou de um modelo personalizado que você pretende criar. Um modelo é treinado usando transcrições rotulada como humanos e texto relacionado. Esses conjuntos de dados, juntamente com os dados de áudio carregados anteriormente, são usados para refinar e treinar o modelo de fala em texto para reconhecer palavras, frases, acrônimos, nomes e outros termos de produto específico. Mais no domínio conjuntos de dados que você fornece (dados que estão relacionados a usuários que informará e o que esperar reconhecer), mais precisos será seu modelo, que resulta em melhor reconhecimento. Tenha em mente, alimentando dados não relacionados ao seu treinamento, você pode reduzir ou prejudicar a precisão de seu modelo.

## <a name="use-training-to-resolve-accuracy-issues"></a>Usar o treinamento para resolver problemas de precisão

Se você estiver encontrando problemas de reconhecimento com seu modelo, usando rotulada como humanos transcrições e dados relacionados para treinamento adicional podem ajudar a melhorar a precisão. Use esta tabela para determinar qual conjunto de dados a serem seguidas para resolver seus problemas:

| Caso de uso | Tipo de dados |
|----------|-----------|
| Melhorar a precisão do reconhecimento no vocabulário específico do setor e a gramática, como a terminologia médica ou jargão IT | Texto relacionado (sentenças/declarações) |
| Defina a forma de uma palavra ou um termo que tenha a pronúncia não padrão, como nomes de produto ou acrônimos fonética e exibida. | Texto relacionado (pronúncia) |
| Melhorar a precisão do reconhecimento em estilos, acentos ou ruídos de fundo específica de fala | Transcrições de áudio + rotulada como humanos |
> [!IMPORTANT]
> Se você ainda não carregou um conjunto de dados, consulte [preparar e testar seus dados](how-to-custom-speech-test-data.md). Este documento fornece instruções para carregar dados e diretrizes para criação de conjuntos de dados de alta qualidade.

## <a name="train-and-evaluate-a-model"></a>Treinar e avaliar um modelo

A primeira etapa para treinar um modelo é carregar dados de treinamento. Use [preparar e testar seus dados](how-to-custom-speech-test-data.md) para obter instruções passo a passo preparar transcrições rotulada como humanos e texto relacionado (declarações e pronúncia). Depois que você carregou dados de treinamento, siga estas instruções para começar a treinar seu modelo:

1. Navegue até **fala em texto > fala personalizado > treinamento**.
2. Clique em **modelo de treinamento**.
3. Em seguida, fornecer seu treinamento uma **nome** e **descrição**.
4. Do **modelo de linha de base e cenário** menu suspenso, selecione o cenário que melhor se adapta a seu domínio. Se você não tiver certeza de qual cenário de escolha, selecione **geral**. O modelo de linha de base é o ponto de partida para treinamento. Se você não tiver uma preferência, você pode usar a versão mais recente.
5. Dos **selecionar dados de treinamento** , escolha um ou vários transcrição de áudio + rotulada como humanos conjuntos de dados que você deseja usar para treinamento.
6. Quando o treinamento estiver concluído, você pode optar por executar teste em que o modelo treinado recentemente de precisão. Esta etapa é opcional.
7. Selecione **criar** para criar seu modelo personalizado.

A tabela de treinamento exibe uma nova entrada que corresponde a esse modelo de recém-criado. A tabela também exibe o status:  Processamento, com êxito, com falha.

## <a name="evaluate-the-accuracy-of-a-trained-model"></a>Avaliar a precisão de um modelo treinado

Você pode inspecionar os dados e avaliar a precisão do modelo usando estes documentos:

* [Inspecionar seus dados](how-to-custom-speech-inspect-data.md)
* [Avaliar seus dados](how-to-custom-speech-evaluate-data.md)


Se você escolheu testar a precisão, é importante selecionar um conjunto de dados acústico diferente daquele usado com o seu modelo para ter uma noção realista de desempenho do modelo.

## <a name="next-steps"></a>Próximas etapas

* [Implantar seu modelo](how-to-custom-speech-deploy-model.md)

## <a name="additional-resources"></a>Recursos adicionais

* [Preparar e testar seus dados](how-to-custom-speech-test-data.md)
* [Inspecionar seus dados](how-to-custom-speech-inspect-data.md)
* [Avaliar seus dados](how-to-custom-speech-evaluate-data.md)
* [Treinar seu modelo](how-to-custom-speech-train-model.md)
