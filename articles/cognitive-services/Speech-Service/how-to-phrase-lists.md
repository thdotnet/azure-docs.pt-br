---
title: Listas de frase - serviços de fala
titlesuffix: Azure Cognitive Services
description: Saiba como fornecer os serviços de fala com uma lista de frases usando o `PhraseListGrammar` objeto para melhorar os resultados de reconhecimento de fala em texto.
services: cognitive-services
author: rhurey
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 5/02/2019
ms.author: rhurey
ms.openlocfilehash: a3be5d28ebe394771a2d8b492f1f6a9c8a82fb9e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66515318"
---
# <a name="phrase-lists-for-speech-to-text"></a>Listas de frase para a conversão de fala em texto

Ao fornecer os serviços de fala com uma lista de frases, você pode melhorar a precisão do reconhecimento de fala. Listas de frase são usadas para identificar as frases conhecidas em dados de áudio, como o nome de uma pessoa ou um local específico.

Por exemplo, se você tiver um comando "Mover para" e um destino possíveis de "Ward" que pode ser falado, você pode adicionar uma entrada de "Mover para Ward". A adição de uma frase aumentará a probabilidade de que quando o áudio é reconhecido que será reconhecido "Mover para Ward" em vez de "Mover na direção".

Palavras ou frases completas podem ser adicionados a uma lista de frase. Durante o reconhecimento, uma entrada em uma lista de frase é usada se uma correspondência exata é incluída no áudio. Compilando o exemplo anterior, se a lista de frase inclui "Mover para Ward" e a frase capturado é "Mover na direção lenta", o resultado do reconhecimento será "Mover para Ward lenta".

## <a name="how-to-use-phrase-lists"></a>Como usar a lista de frase

Os exemplos abaixo ilustram como criar uma lista de frases usando o `PhraseListGrammar` objeto.

```C++
auto phraselist = PhraseListGrammar::FromRecognizer(recognizer);
phraselist->AddPhrase("Move to Ward");
phraselist->AddPhrase("Move to Bill");
phraselist->AddPhrase("Move to Ted");
```

```cs
PhraseListGrammar phraseList = PhraseListGrammar.FromRecognizer(recognizer);
phraseList.AddPhrase("Move to Ward");
phraseList.AddPhrase("Move to Bill");
phraseList.AddPhrase("Move to Ted");
```

```Python
phrase_list_grammar = speechsdk.PhraseListGrammar.from_recognizer(reco)
phrase_list_grammar.addPhrase("Move to Ward")
phrase_list_grammar.addPhrase("Move to Bill")
phrase_list_grammar.addPhrase("Move to Ted")
```

```JavaScript
var phraseListGrammar = SpeechSDK.PhraseListGrammar.fromRecognizer(reco);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

```Java
PhraseListGrammar phraseListGrammar = PhraseListGrammar.fromRecognizer(recognizer);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

>[!Note]
> O número máximo de listas de frase que serão usadas pelo serviço de fala para corresponder a fala é 1024 frases.

Você também pode limpar as frases associadas com o `PhraseListGrammar` por chamada Clear ().

```C++
phraselist->Clear();
```

```cs
phraseList.Clear();
```

```Python
phrase_list_grammar.clear()
```

```JavaScript
phraseListGrammar.clear();
```

```Java
phraseListGrammar.clear();
```

> [!NOTE]
> Altera para um `PhraseListGrammar` passam a vigorar no reconhecimento de Avançar ou após a reconexão para os serviços de fala do objeto.

## <a name="next-steps"></a>Próximas etapas

* [Documentação de referência do SDK de fala](speech-sdk.md)
