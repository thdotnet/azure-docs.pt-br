---
title: Como escolher o modo de reconhecimento de Fala do Bing | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Como escolher o melhor modo de reconhecimento na Fala do Bing.
services: cognitive-services
author: nitinme
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: nitinme
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 1860acb571c837a5eb4c75be69a96d1b22682118
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70965684"
---
# <a name="bing-speech-recognition-modes"></a>Modos de reconhecimento da Fala do Bing

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

As APIs de Conversão de Fala em Texto do Bing dão suporte a vários modos de reconhecimento de fala. Escolha o modo que produz os melhores resultados de reconhecimento para seu aplicativo.

| Modo | Descrição |
|---|---|
| *interativo* | Reconhecimento de "comando e controle" para cenários de aplicativo do usuário interativo. Os usuários falam frases curtas planejadas como comandos para um aplicativo. |
| *ditado* | Reconhecimento contínuo para cenários de ditado. Os usuários falam sentenças mais longas do que o exibido como texto. Os usuários adotam um estilo de fala mais formal. |
| *conversa* | Reconhecimento contínuo para transcrever conversas entre humanos. Os usuários adotam um estilo de fala menos formal e podem alternar entre sentenças maiores e frases mais curtas.

> [!NOTE]
> Esses modos são aplicáveis quando você usar as [APIs REST](../GetStarted/GetStartedREST.md). As [bibliotecas de cliente](../GetStarted/GetStartedClientLibraries.md) usam parâmetros diferentes para especificar o modo de reconhecimento. Para obter mais informações, consulte a biblioteca de cliente de sua escolha.

Para saber obter mais informações, confira a página [Modos de Reconhecimento](../concepts.md#recognition-modes).
