---
title: Dicionário Dinâmico - API de Texto do Tradutor
titlesuffix: Azure Cognitive Services
description: Como usar o recurso de dicionário dinâmico da API de Tradução de Texto.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: a815434cb8797acf6b92a8fe4a4f1ff69508975d
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839211"
---
# <a name="how-to-use-a-dynamic-dictionary"></a>Como usar um dicionário dinâmico

Se você já souber a tradução que deseja aplicar a uma palavra ou frase, poderá fornecê-la como marcação dentro da solicitação. O dicionário dinâmico é seguro somente para substantivos compostos como nomes próprios e nomes de produtos.

**Sintaxe:**

<mstrans:dictionary translation=”translation of phrase”>frase</mstrans:dictionary>

**Requisitos:**

* O `From` e `To` idiomas devem ser diferentes. 
* Você deve incluir o `From` parâmetro em sua solicitação de conversão de API em vez de usar o recurso de detecção automática. 

**Exemplo: en-de:**

Entrada de origem: A palavra <mstrans:dictionary translation=\"wordomatic\">palavra ou frase</mstrans:dictionary> é uma entrada de dicionário.

Saída de destino: Das Wort "wordomatic" ist ein Wörterbucheintrag.

Esse recurso funciona da mesma forma com e sem o modo HTML.

O recurso deve ser usado com moderação. A maneira apropriada e muito melhor de personalizar a tradução é usando o Tradutor Personalizado. O Tradutor Personalizado faz uso total das probabilidades de estatística e contexto. Se você tiver ou puder criar dados de treinamento que mostrem o trabalho ou a frase no contexto, obterá resultados muito melhores. Você pode encontrar mais informações sobre o Tradutor Personalizado em [https://aka.ms/CustomTranslator](https://aka.ms/CustomTranslator).
