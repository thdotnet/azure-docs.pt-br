---
title: Idiomas compatíveis – API de Análise de Texto
titleSuffix: Azure Cognitive Services
description: 'Uma lista dos idiomas naturais compatíveis com a API de Análise de Texto. Este artigo explica quais idiomas têm suporte para cada operação: análise de sentimento, extração de frases-chave, detecção de idioma e reconhecimento de entidade.'
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: 69c14c6b98b572bc413f5a35696269e13344387e
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417318"
---
# <a name="language-and-region-support-for-the-text-analytics-api"></a>Idiomas e regiões compatíveis com a API de Análise de Texto

Este artigo explica quais linguagens têm suporte para cada operação: análise de sentimento, extração de frases-chave, detecção de idioma e reconhecimento de entidade nomeada.

## <a name="language-detection"></a>Detecção de Idioma

A API de análise de texto pode detectar uma ampla variedade de linguagens, variantes, dialetos e algumas linguagens cultural e regionais.  A detecção de idioma retorna o "script" de um idioma. Por exemplo, a frase "Eu tenho um cachorro" retornará `en`, em vez de `en-US`. O único especial é chinês, em que a funcionalidade de detecção de idioma retornará `zh_CHS` ou `zh_CHT` se ela puder determinar o script fornecido pelo texto fornecido. Em situações em que um script específico não pode ser identificado para um documento em chinês, ela retornará simplesmente `zh`.

Não publicamos a lista exata de idiomas para esse recurso, mas ele pode detectar uma ampla variedade de linguagens, variantes, dialetos e algumas linguagens cultural e regionais. 

Se você tiver um conteúdo expressado em uma linguagem usada com menos frequência, você pode tentar a detecção de idioma para ver se ele retorna um código. A resposta para idiomas que não pode ser detectado é `unknown`.

## <a name="sentiment-analysis-key-phrase-extraction-and-named-entity-recognition"></a>Análise de sentimento, extração de frases-chave e reconhecimento de entidade nomeada

Para análise de sentimento, extração de frases-chave e reconhecimento de entidade, a lista de idiomas compatíveis é mais seletiva, uma vez que os analisadores são refinados para acomodar as regras linguísticas de idiomas adicionais.

## <a name="language-list-and-status"></a>Status e lista de idiomas

O suporte a idiomas é implantado inicialmente em versão prévia, passando para o status GA (disponível ao público em geral), de modo independente um do outro e do serviço de Análise de Texto como um todo. É possível que idiomas permaneçam em versão prévia mesmo quando a API de Análise de Texto passar para disponível ao público em geral.

| Linguagem    | Código de idioma | Sentimento | Frases principais | Reconhecimento de Entidade Nomeada |   Observações  |
|:----------- |:-------------:|:---------:|:-----------:|:-----------:|:-----------:
| Árabe      | `ar`          |           |             | ✔ \*                     | |
| Tcheco       | `cs`          |           |             | ✔ \*                     | |
| Chinês simplificado | `zh-CN`|           |             | ✔ \*        |    |
| Dinamarquês      | `da`          | ✔ \*     | ✔           | ✔ \*            |     |
| Holandês       | `nl`          | ✔ \*     | ✔          |  ✔ \*           |     |
| Inglês     | `en`          | ✔        | ✔           |  ✔ \*\*     |      |
| Finlandês     | `fi`          | ✔ \*     | ✔           |  ✔ \*           |     |
| Francês      | `fr`          | ✔        | ✔           |  ✔ \*           |     |
| Alemão      | `de`          | ✔ \*     | ✔           |  ✔ \*          |     |
| Grego       | `el`          | ✔ \*     |             |            |     |
| Húngaro   | `hu`          |           |             |  ✔ \*          |     | 
| Italiano     | `it`          | ✔ \*     | ✔           |  ✔ \*           |     |
| Japonês    | `ja`          |          | ✔           |  ✔ \*          |     |
| Coreano      | `ko`          |          | ✔           |  ✔ \*          |     |
| Norueguês (Bokmål) | `no`  | ✔ \*     |  ✔          | ✔ \*            |     |
| Polonês      | `pl`          | ✔ \*     |  ✔          |  ✔ \*           |     |
| Português (Portugal) | `pt-PT`| ✔        |  ✔          | ✔ \*      |`pt` também é aceito|
| Português (Brasil)   | `pt-BR`|          |  ✔   |  ✔ \*       |     |
| Russo     | `ru`          | ✔ \*     | ✔           |  ✔ \*           |     |
| Espanhol     | `es`          | ✔        |            |   ✔ \*\*      |     | 
| Sueco     | `sv`          | ✔ \*     | ✔           |   ✔ \*          |     |
| Turco     | `tr`          | ✔ \*     |             |   ✔ \*          |  |

\* Suporte de linguagem está em versão prévia

\*\* Reconhecimento de entidade de chamada e [vinculação de entidade](how-tos/text-analytics-how-to-entity-linking.md) ambas estão disponíveis para esse idioma.    

## <a name="see-also"></a>Consulte também

[Página Documentação dos Serviços Cognitivos](https://docs.microsoft.com/azure/cognitive-services/)   
[Página do Produto de Serviços Cognitivos](https://azure.microsoft.com/services/cognitive-services/)
