---
title: Referência de domínio predefinido-LUIS
titleSuffix: Azure Cognitive Services
description: Referência para os domínios predefinidos, que são conjuntos predefinidos de tentativas e entidades de idioma Noções básicas sobre inteligente serviços (LUÍS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 17d72ad51974bb0fb741ae19ebb19f313e646c62
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/30/2019
ms.locfileid: "71677647"
---
# <a name="prebuilt-domain-reference-for-your-luis-app"></a>Referência de domínio predefinida para seu aplicativo LUIS
Esta referência contém informações sobre o [domínios pré-compilada](luis-how-to-use-prebuilt-domains.md), que são conjuntos predefinidos de entidades que oferece LUÍS e tentativas.

[Domínios personalizados](luis-how-to-start-new-app.md), por outro lado, iniciar sem tentativas e modelos. Você pode adicionar quaisquer tentativas de domínio pré-compilada e entidades a um modelo personalizado.

## <a name="custom-domains-per-language"></a>Domínios personalizados por idioma

A tabela a seguir resume os domínios com suporte no momento. O suporte para inglês geralmente é mais completo do que outros. 

| Tipo de Entidade       | EN-US      | ZH-CN   | DE    | FR     | ES    | IT      | PT-BR |  JP  |      KO |        NL |    TR |
|:-----------------:|:-------:|:-------:|:-----:|:------:|:-----:|:-------:| :-------:| :-------:| :-------:| :-------:|  :-------:| 
| Calendário  | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
|Comunicação  | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Email     | ✓    | ✓       | ✓   | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| HomeAutomation          | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Observações     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Locais   | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| RestaurantReservation  | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| ToDo     | ✓    | ✓       | ✓    | ✓     | ✓     | ✓  | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Utilidades      | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Clima        | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |
| Web    | ✓    | ✓        | ✓    | ✓      | ✓     | ✓       | ✓  | ✓      | ✓    | ✓    | ✓     | ✓  |

**Não há suporte para** domínios predefinidos no:

* Francês canadense
* Híndi
* Mexicano espanhol

## <a name="next-steps"></a>Próximas etapas

Aprenda a [entidade simples](reference-entity-simple.md). 