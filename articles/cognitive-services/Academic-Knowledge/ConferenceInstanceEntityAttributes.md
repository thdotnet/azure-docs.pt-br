---
title: Atributos de entidade de Instância de conferência ‒ API de Conhecimento Acadêmico
titlesuffix: Azure Cognitive Services
description: Saiba mais sobre os atributos que podem ser utilizados com a entidade Instância de Conferência na API de Conhecimento Acadêmico.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/23/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 37a353fbb86ca199b2316dcfba5904f4b46b0276
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705055"
---
# <a name="conference-instance-entity"></a>Entidade Instância de Conferência

<sub> *Os atributos a seguir são específicos da entidade de instância de conferência. (Ty = '4') </sub>

Nome    |Descrição                            |Tipo       | Operações
------- | ------------------------------------- | --------- | ----------------------------
Id      |ID da Entidade                              |Int64      |Igual a
CIN     |Nome normalizado da instância de conferência ({ConferenceSeriesNormalizedName} {ConferenceInstanceYear})        |Cadeia     |Igual a
DCN     |Nome de exibição da instância da conferência ({ConferenceSeriesName} : {ConferenceInstanceYear})       |Cadeia     |nenhum
CIL     |Local da instância de conferência    |Cadeia     |Igual a,<br/>StartsWith
CISD    |Data de início da instância de conferência  |Date       |Igual a,<br/>IsBetween
CIED    |Data de término da instância de conferência    |Date       |Igual a,<br/>IsBetween
CIARD   |Resumo da data de conclusão do registro da instância de conferência  |Date       |Igual a,<br/>IsBetween
CISDD   |Data de conclusão do envio da instância de conferência     |Date       |Igual a,<br/>IsBetween
CIFVD   |Data de conclusão da versão final da instância de conferência  |Date       |Igual a,<br/>IsBetween
CINDD   |Data de notificação da instância de conferência   |Date       |Igual a,<br/>IsBetween
CD.T    |Título de um evento de instância de conferência   |Date       |Igual a,<br/>IsBetween
CD.D    |Data de um evento de instância de conferência    |Date       |Igual a,<br/>IsBetween
PCS.CN  |Nome da série de conferências da instância |Cadeia     |Igual a
PCS.CId |ID da série de conferências da instância |Int64    |Igual a
CC      |Contagem total de citações da instância de conferência           |Int32      |nenhum  
ECC     |Contagem total estimada de citações da instância de conferência |Int32      |nenhum


## <a name="extended-metadata-attributes"></a>Atributos de metadados estendidos ##

Nome    | Descrição               
--------|---------------------------    
FN      | Nome completo da instância de conferência
