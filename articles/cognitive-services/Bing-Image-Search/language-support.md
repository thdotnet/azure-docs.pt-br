---
title: Suporte de idioma – API de Pesquisa de Imagem do Bing
titleSuffix: Azure Cognitive Services
description: Descubra quais países/regiões e idiomas têm suporte para API de Pesquisa de Imagem do Bing.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-image-search
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: aahi
ms.openlocfilehash: ca3821b6088e45730334d1b0971e270b1d86dfce
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68881916"
---
# <a name="language-and-region-support-for-the-bing-image-search-api"></a>Suporte de idioma e região para a API de Pesquisa de Imagem do Bing

A API de Pesquisa de Imagem do Bing dá suporte a mais de trinta países/regiões, muitos com mais de um idioma. Especificar um país/região com uma consulta serve principalmente para refinar os resultados da pesquisa com base nos interesses nesse país/região. Além disso, os resultados podem conter links para o Bing e esses links podem localizar a experiência do usuário do Bing, de acordo com o país/região ou idioma especificado.

Para especificar o país/região e o idioma, defina o parâmetro de consulta `mkt` (mercado) com um código da tabela **Mercados** abaixo. O mercado especifica um país/região e um idioma. Se o usuário preferir ver e exibir o texto em outro idioma, defina o parâmetro de consulta `setLang` com o código de idioma apropriado.

Como alternativa, é possível especificar o país/região usando o parâmetro de consulta `cc`. Se você especificar um país/região, também será necessário especificar um ou mais códigos de idiomas usando o cabeçalho HTTP `Accept-Language`. Os idiomas com suporte variam por país/região e são fornecidos para cada país/região na tabela Mercados.

> [!NOTE]
> Atualmente, a API de Imagens Populares dá suporte apenas aos seguintes mercados:
> - en-US (inglês, Estados Unidos)
> - en-CA (inglês, Canadá)
> - en-AU (inglês, Austrália)
> - zh-CN (chinês, China)

## <a name="countriesregions"></a>Países/Regiões

|País/região|Código|
|-------|----|
|Argentina|AR|
|Austrália|AU|
|Áustria|AT|
|Bélgica|BE|
|Brasil|BR|
|Canadá|AC|
|Chile|CL|
|Dinamarca|DK|
|Finlândia|FI|
|França|FR|
|Alemanha|DE|
|Região Administrativa Especial de Hong Kong|HK|
|Índia|IN|
|Indonésia|ID|
|Itália|it|
|Japão|JP|
|Coreia do Sul|KR|
|Malásia|MY|
|México|MX|
|Países Baixos|NL|
|Nova Zelândia|NZ|
|Noruega|NÃO|
|China|CN|
|Polônia|PL|
|Portugal|PT|
|Filipinas|PH|
|Rússia|RU|
|Arábia Saudita|SA|
|África do Sul|ZA|
|Espanha|ES|
|Suécia|SE|
|Suíça|CH|
|Taiwan|TW|
|Turquia|TR|
|Reino Unido|GB|
|Estados Unidos|EUA|


## <a name="markets"></a>Mercados

|País/região|Idioma|Código de mercado|
|-------|--------|-----------|
|Argentina|Espanhol|es-AR|
|Austrália|Inglês|en-AU|
|Áustria|Alemão|de-AT|
|Bélgica|Holandês|nl-BE|
|Bélgica|Francês|fr-BE|
|Brasil|Português|pt-BR|
|Canadá|Inglês|en-CA|
|Canadá|Francês|fr-CA|
|Chile|Espanhol|es-CL|
|Dinamarca|Dinamarquês|da-DK|
|Finlândia|Finlandês|fi-FI|
|França|Francês|fr-FR|
|Alemanha|Alemão|de-DE|
|Região Administrativa Especial de Hong Kong|Chinês Tradicional|zh-HK|
|Índia|Inglês|en-IN|
|Indonésia|Inglês|en-ID|
|Itália|Italiano|it-IT|
|Japão|Japonês|ja-JP|
|Coreia do Sul|Coreano|ko-KR|
|Malásia|Inglês|en-MY|
|México|Espanhol|es-MX|
|Países Baixos|Holandês|nl-NL|
|Nova Zelândia|Inglês|en-NZ|
|China|Chinês|zh-CN|
|Polônia|Polonês|pl-PL|
|Portugal|Português|pt-PT|
|Filipinas|Inglês|en-PH|
|Rússia|Russo|ru-RU|
|Arábia Saudita|Árabe|ar-SA|
|África do Sul|Inglês|en-ZA|
|Espanha|Espanhol|es-ES|
|Suécia|Sueco|sv-SE|
|Suíça|Francês|fr-CH|
|Suíça|Alemão|de-CH|
|Taiwan|Chinês Tradicional|zh-TW|
|Turquia|Turco|tr-TR|
|Reino Unido|Inglês|en-GB|
|Estados Unidos|Inglês|pt-BR|
|Estados Unidos|Espanhol|es-US|

## <a name="next-steps"></a>Próximas etapas
Para obter mais informações sobre os pontos de extremidade da Pesquisa de Notícias do Bing, confira [Referência da API de Pesquisa de Imagem v7](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference).
