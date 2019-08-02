---
title: Atributos da entidade do Academic Graph ‒ API de Conhecimento Acadêmico
titlesuffix: Azure Cognitive Services
description: Saiba mais sobre os atributos da entidade que você pode usar com o Grafo Acadêmico na API de Conhecimento Acadêmico.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/27/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 596ce35a0d744691edd75d6071d4758691e0031b
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705026"
---
# <a name="entity-attributes"></a>Atributos da entidade

O grafo acadêmico é composto de 7 tipos de entidades. Todas as entidades terão uma ID de entidade e um tipo de entidade.

## <a name="common-entity-attributes"></a>Atributos de entidades comuns
Nome    |Descrição                |Tipo       | Operações
------- | ------------------------- | --------- | ----------------------------
Id      |ID da Entidade                  |Int64      |Igual a
Ty      |Tipo de entidade                |enum   |Igual a

## <a name="entity-type-enum"></a>Enumeração de tipo de entidade
Nome                                                            |value
----------------------------------------------------------------|-----
[Papel](PaperEntityAttributes.md)                               |0
[Autor](AuthorEntityAttributes.md)                             |1
[Diário](JournalEntityAttributes.md)                           |2
[Série de conferência](JournalEntityAttributes.md)                 |3
[Instância de conferência](ConferenceInstanceEntityAttributes.md)    |4
[Afiliação](AffiliationEntityAttributes.md)                   |5
[Campo de estudo](FieldsOfStudyEntityAttributes.md)                      |6

