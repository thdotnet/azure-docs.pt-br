---
title: Configurações do aplicativo
titleSuffix: Azure Cognitive Services
description: Entenda as configurações de aplicativos para aplicativos de compreensão de idioma.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/29/2019
ms.author: diberry
ms.openlocfilehash: a8a1f6fe9372d013d310c557161ceb813b8a478b
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67163648"
---
# <a name="application-settings"></a>Configurações do aplicativo

Essas configurações de aplicativo são armazenadas do [exportado](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) aplicativo e [atualizado](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) com as APIs REST. Alterar as configurações de versão do aplicativo redefine o status de treinamento do aplicativo para não treinado.

|Configuração|Valor padrão|Observações|
|--|--|--|
|NormalizePunctuation|True|Remove a pontuação.|
|NormalizeDiacritics|True|Remove os sinais diacríticos.|

## <a name="diacritics-normalization"></a>Normalização de diacríticos 

Ativar a normalização de expressão para sinais diacríticos para seu arquivo de aplicativo LUIS JSON no `settings` parâmetro.

```JSON
"settings": [
    {"name": "NormalizeDiacritics", "value": "true"}
] 
```

As declarações a seguir mostram como normalização diacríticos afeta declarações:

|Com sinais diacríticos definidos como false|Com sinais diacríticos definido como true|
|--|--|
|`quiero tomar una piña colada`|`quiero tomar una pina colada`|
|||

### <a name="language-support-for-diacritics"></a>Suporte de linguagem para sinais diacríticos

#### <a name="brazilian-portuguese-pt-br-diacritics"></a>Português (Brasil) `pt-br` diacríticos

|Sinais diacríticos definido como false|Sinais diacríticos definido como false|
|-|-|
|`á`|`a`|
|`â`|`a`|
|`ã`|`a`|
|`à`|`a`|
|`ç`|`c`|
|`é`|`e`|
|`ê`|`e`|
|`í`|`i`|
|`ó`|`o`|
|`ô`|`o`|
|`õ`|`o`|
|`ú`|`u`| 
|||

#### <a name="dutch-nl-nl-diacritics"></a>Holandês `nl-nl` diacríticos

|Sinais diacríticos definido como false|Sinais diacríticos definido como false|
|-|-|
|`á`|`a`|
|`à`|`a`|
|`é`|`e`|
|`ë`|`e`|
|`è`|`e`|
|`ï`|`i`|
|`í`|`i`|
|`ó`|`o`|
|`ö`|`o`|
|`ú`|`u`| 
|`ü`|`u`|
|||

#### <a name="french-fr--diacritics"></a>Francês `fr-` diacríticos

Isso inclui subculturas tanto francês canadenses.

|Sinais diacríticos definido como false|Sinais diacríticos definido como false|
|--|--|
|`é`|`e`|
|`à`|`a`|
|`è`|`e`|
|`ù`|`u`|
|`â`|`a`| 
|`ê`|`e`| 
|`î`|`i`| 
|`ô`|`o`| 
|`û`|`u`| 
|`ç`|`c`| 
|`ë`|`e`| 
|`ï`|`i`| 
|`ü`|`u`| 
|`ÿ`|`y`| 

#### <a name="german-de-de-diacritics"></a>Alemão `de-de` diacríticos

|Sinais diacríticos definido como false|Sinais diacríticos definido como false|
|--|--|
|`ä`|`a`|
|`ö`|`o`| 
|`ü`|`u`| 

#### <a name="italian-it-it-diacritics"></a>Italiano `it-it` diacríticos

|Sinais diacríticos definido como false|Sinais diacríticos definido como false|
|--|--|
|`à`|`a`|
|`è`|`e`|
|`é`|`e`|
|`ì`|`i`| 
|`í`|`i`| 
|`î`|`i`| 
|`ò`|`o`| 
|`ó`|`o`| 
|`ù`|`u`|
|`ú`|`u`|

#### <a name="spanish-es--diacritics"></a>Espanhol `es-` diacríticos

Isso inclui mexicano espanhol e canadenses.

|Sinais diacríticos definido como false|Sinais diacríticos definido como false|
|-|-|
|`á`|`a`|
|`é`|`e`|
|`í`|`i`| 
|`ó`|`o`| 
|`ú`|`u`|
|`ü`|`u`|
|`ñ`|`u`|


## <a name="punctuation-normalization"></a>Normalização de pontuação

Ativar a normalização de expressão para pontuação para seu arquivo de aplicativo LUIS JSON no `settings` parâmetro.

```JSON
"settings": [
    {"name": "NormalizePunctuation", "value": "true"}
] 
```

As declarações a seguir mostram como sinais diacríticos afeta declarações:

|Com sinais diacríticos definidos como False|Com sinais diacríticos definidos como True|
|--|--|
|`Hmm..... I will take the cappuccino`|`Hmm I will take the cappuccino`|
|||

### <a name="punctuation-removed"></a>Pontuação removida

A pontuação a seguir é removida com `NormalizePunctuation` é definido como true.

|Pontuação|
|--|
|`-`| 
|`.`| 
|`'`|
|`"`|
|`\`|
|`/`|
|`?`|
|`!`|
|`_`|
|`,`|
|`;`|
|`:`|
|`(`|
|`)`|
|`[`|
|`]`|
|`{`|
|`}`|
|`+`|
|`¡`|
