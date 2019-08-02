---
title: Atributos de entidade de afiliação na API de Conhecimento Acadêmico
titlesuffix: Azure Cognitive Services
description: Saiba mais sobre os atributos que podem ser utilizados com a entidade Afiliação na API de Conhecimento Acadêmico.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 333875472d9b859196c4d828061301b9d32c4d5a
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705105"
---
# <a name="affiliation-entity"></a>Entidade Afiliação

<sub> *Os atributos a seguir são específicos da entidade de afiliação. (Ty = '5') </sub>

Nome    |Descrição                            |Tipo       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID da Entidade                              |Int64      |Igual a
AfN     |Nome normalizado da afiliação        |Cadeia     |Igual a
DAfN    |Nome de exibição da afiliação       |Cadeia     |nenhum
CC      |Contagem total de citações da afiliação           |Int32      |nenhum  
ECC     |Contagem total estimada de citações da afiliação |Int32      |nenhum

## <a name="extended-metadata-attributes"></a>Atributos de metadados estendidos ##

Nome    | Descrição               
--------|---------------------------    
Computador      |Contagem de artigos da afiliação
