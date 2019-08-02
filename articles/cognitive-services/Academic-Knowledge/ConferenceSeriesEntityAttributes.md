---
title: Atributos da entidade Série de Conferências – API de Conhecimento Acadêmico
titlesuffix: Azure Cognitive Services
description: Saiba mais sobre os atributos que podem ser utilizados com a entidade Série de Conferências.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: feed324202f6a75ceb7e9089875b899c51cd8ae6
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705040"
---
# <a name="conference-series-entity"></a>Entidade Série de Conferências

<sub> *Os atributos a seguir são específicos da entidade da série de conferências. (Ty = '3') </sub>

Nome    |Descrição                            |Tipo       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID da Entidade                              |Int64      |Igual a
CN      |Nome normalizado da série de conferências      |Cadeia     |Igual a
DCN     |Nome de exibição da série de conferências         |Cadeia     |nenhum
CC      |Contagem total de citações da série de conferências         |Int32      |nenhum  
ECC     |Contagem total estimada de citações da série de conferências   |Int32      |nenhum
F.FId   |ID da entidade do Campo de Estudo associado à série de conferências |Int64  | Igual a
F.FN    |Nome do Campo de Estudo associado à série de conferências  | Igual a,<br/>StartsWith
