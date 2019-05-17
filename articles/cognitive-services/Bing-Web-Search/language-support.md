---
title: Suporte de idioma – API de Pesquisa na Web do Bing
titleSuffix: Azure Cognitive Services
description: Uma lista de idiomas naturais, países e regiões compatíveis com a API de Pesquisa de Notícias do Bing.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: article
ms.date: 05/15/2019
ms.author: aahi
ms.openlocfilehash: e657c4678c76e8ff667c1a3f30409fc157f52d16
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65798253"
---
# <a name="language-and-region-support-for-the-bing-web-search-api"></a>Suporte de idioma e região para a API de Pesquisa na Web do Bing

A API de Pesquisa na Web do Bing dá suporte a mais de trinta países ou regiões, muitos com mais de um idioma. Especificar um país ou região com uma consulta ajuda a refinar os resultados da pesquisa com base nos interesses nesse país ou região. Os resultados podem incluir links para o Bing e esses links podem localizar a experiência do usuário do Bing, de acordo com o país/região ou idioma especificado.

É possível especificar um país ou região usando o parâmetro de consulta `cc`. Quando um país ou região é especificado, você deve especificar um ou mais códigos de idioma com o [cabeçalho `Accept-Language`](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#headers). Use a [tabela de Mercados](#markets) para obter uma lista dos idiomas com suporte com cada mercado.

Como alternativa, você pode especificar o mercado usando o parâmetro de consulta `mkt` e um código da tabela **Mercados**. Especificar um mercado, especifica simultaneamente um país ou região e um idioma preferido. Você pode definir explicitamente o idioma com o parâmetro de consulta `setLang`.

## <a name="countriesregions"></a>Países/regiões

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

|País/região|Linguagem|Código de mercado|
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
|Noruega|Norueguês|no-NO|
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

* [Referência da API de Pesquisa de Imagem do Bing](//docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
