---
title: Suporte de idioma – QnA Maker
titleSuffix: Azure Cognitive Services
description: O idioma de uma base de dados de conhecimento afeta a capacidade do QnA Maker de extrair automaticamente as perguntas e respostas das fontes, bem como a relevância dos resultados fornecidos pelo QnA Maker em resposta às consultas do usuário. Uma lista de idiomas naturais e cultura com suporte do QnA Maker para sua base de dados de conhecimento. Não misture idiomas na mesma base de dados de conhecimento.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/21/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: bec09e5cd053774902083c6dd042c1ed4b293d20
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71066675"
---
# <a name="language-support-for-qna-maker"></a>Suporte a idiomas para QnA Maker

O idioma de uma base de dados de conhecimento afeta a capacidade do QnA Maker de extrair automaticamente as perguntas e respostas das [fontes](../Concepts/data-sources-supported.md), bem como a relevância de resultados fornecidos pelo QnA Maker em resposta às consultas do usuário.

## <a name="auto-extraction"></a>Extração automática
O QnA Maker dá suporte à extração de perguntas/respostas em qualquer página de idioma, mas a eficácia da extração é muito maior nos idiomas a seguir, pois o QnA Maker usa palavras-chave para identificar perguntas.

|Idiomas compatíveis| Localidade|
|-----|----|
|Inglês|en-*|
|Francês|fr-*|
|Italiano|it-*|
|Alemão|de-*|
|Espanhol|es-*|

## <a name="primary-language-detection"></a>Detecção de idioma primário

O idioma principal usado para detecção é definido para o recurso QnA Maker e todas as bases de dados de conhecimento criadas nesse recurso, quando o primeiro documento ou URL é adicionado à primeira base de dados de conhecimento. O idioma não pode ser alterado. 

Se o usuário planeja oferecer suporte a vários idiomas, ele precisa ter um novo recurso QnA Maker para cada idioma. Saiba como [criar uma base de dados de conhecimento QnA Maker baseada em linguagem](../how-to/language-knowledge-base.md).  

Verifique o idioma principal com as seguintes etapas:

1. Entre no [Portal do Azure](https://portal.azure.com).  
1. Procure e selecione o Azure Search recurso criado como parte do recurso de QnA Maker. O nome do recurso de Azure Search começará com o mesmo nome que o recurso de QnA Maker e terá o **serviço de pesquisa**de tipo. Tenha em mente, apenas um recurso de QnA Maker pode ser vinculado a um recurso de Azure Search.
1. Na página **visão geral** do recurso de pesquisa, selecione **índices**. 
1. Selecione o índice **testkb**.
1. Selecione a guia **campos** . 
1. Exiba a coluna **analisador** para os campos **perguntas** e **resposta** . 


## <a name="query-matching-and-relevance"></a>Correspondência de consulta e relevância
O QnA Maker depende de [analisadores de idioma](https://docs.microsoft.com/rest/api/searchservice/language-support) no Azure Search para fornecer resultados. Os recursos de nova classificação especiais estão disponíveis para idiomas En-* que permitem uma maior relevância.

Enquanto os recursos do Azure Search estão no mesmo nível dos idiomas com suporte, o QnA Maker tem um classificador adicional que fica acima dos resultados de pesquisa do Azure. Neste modelo de classificação, usamos alguns recursos semânticos e baseados em palavras especiais em en-*, que ainda não estão disponíveis para outros idiomas. Nós não disponibilizamos esses recursos, pois fazem parte do trabalho interno do classificador de QnA Maker. 

QnA Maker [detecta automaticamente o idioma da base de dados de conhecimento](#primary-language-detection) durante a criação e define o analisador adequadamente. Você pode criar bases de dados de conhecimento nos idiomas a seguir. 

|Idiomas compatíveis|
|-----|
|Árabe|
|Armênio|
Bengali|
|basco|
|Búlgaro|
|Catalão|
|Chinês_Simplificado|
|Chinês_Tradicional|
|Croata|
|Tcheco|
|Dinamarquês|
|Holandês|
|Inglês|
|Estoniano|
|Finlandês|
|Francês|
|Galego|
|Alemão|
|Grego|
|Guzerate|
|Hebraico|
|Híndi|
|Húngaro|
|Islandês|
|Indonésio|
|Irlandês|
|Italiano|
|Japonês|
|canarim|
|Coreano|
|Letão|
|Lituano|
|Malaiala|
|Malaio|
|Norueguês|
|Polonês|
|Português|
|Panjabi|
|Romeno|
|Russo|
|Cirílico sérvio|
|sérvio Latino|
|Eslovaco|
|Esloveno|
|Espanhol|
|Sueco|
|Tâmil|
|Telugu|
|Tailandês|
|Turco|
|Ucraniano|
|Urdu|
|Vietnamita|
