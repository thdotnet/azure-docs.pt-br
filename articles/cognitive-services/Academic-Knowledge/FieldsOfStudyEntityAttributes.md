---
title: Atributos da entidade Campo de Estudo ‒ API de Conhecimento Acadêmico
titlesuffix: Azure Cognitive Services
description: Saiba mais sobre os atributos que podem ser utilizados com a entidade Campo de Estudo na API de Conhecimento Acadêmico.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: d54f3c72462e6702b09068092b7c18ea50f12048
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704989"
---
# <a name="field-of-study-entity"></a>Entidade Campo de Estudo

<sub> *Os atributos a seguir são específicos para a entidade do campo de estudo. (Ty = '6') </sub>

Nome    |Descrição                            |Tipo       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID da Entidade                              |Int64      |Igual a
FN      |Nome normalizado do campo de estudo         |Cadeia     |Igual a
DFN     |Nome de exibição do campo de estudo            |Cadeia     |nenhum
CC      |Contagem total de citações do campo de estudo    |Int32      |nenhum  
ECC     |Campo da contagem total estimada de citações|Int32      |nenhum
FL      |Nível nos campos de hierarquia de estudo     |Int32      |Igual a, <br/>IsBetween
FP.FN   |Nome do campo de estudo pai             |Cadeia     |Igual a
FP.FId  |ID do campo de estudo pai               |Int64      |Igual a
FC.FN   |Nome do campo de estudo filho              |Cadeia     |Igual a
FC.FId  |ID do campo de estudo filho                |Int64      |Igual a
