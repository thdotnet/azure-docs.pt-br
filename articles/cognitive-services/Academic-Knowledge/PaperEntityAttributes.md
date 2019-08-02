---
title: Atributos de entidade de documento ‒ API de Conhecimento Acadêmico
titlesuffix: Azure Cognitive Services
description: Saiba mais sobre os atributos que podem ser utilizados com a entidade Documento na API de Conhecimento Acadêmico.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ROBOTS: NOINDEX
ms.openlocfilehash: 4b9431469a7925d26003ad9c34f6b401e5767f6d
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704933"
---
# <a name="paper-entity"></a>Entidade Artigo

<sub> *Os atributos a seguir são específicos da entidade Artigo. (Ty = '0') </sub>


Nome    |Descrição                                        |Tipo       | Operações
------- | ------------------------------------------------- | --------- | ----------------------------
Id      |ID da Entidade                                          |Int64      |Igual a
Ti      |Título do artigo                                        |Cadeia     |Igual a,<br/>StartsWith
S       |Código de idioma de papel separado\@por ""\@\@          |Cadeia     |Igual a
S       |Ano do artigo                                         |Int32      |Igual a,<br/>IsBetween
D       |Data do artigo                                         |Date       |Igual a,<br/>IsBetween
CC      |Contagem de citações                                     |Int32      |nenhum  
ECC     |Contagem estimada de citações                           |Int32      |nenhum
AA.AuN  |Nome do autor                                        |Cadeia     |Igual a,<br/>StartsWith
AA.AuId |ID do autor                                          |Int64      |Igual a
AA.AfN  |Nome da associação do autor                            |Cadeia     |Igual a,<br/>StartsWith
AA.AfId |ID da associação do autor                              |Int64      |Igual a
AA.S    |Ordem do autor para o artigo                         |Int32      |Igual a
F.FN    |Nome do campo de estudo                                |Cadeia     |Igual a,<br/>StartsWith
F.FId   |ID do campo de estudo                                  |Int64      |Igual a
J.JN    |Nome do periódico                                       |Cadeia     |Igual a,<br/>StartsWith
J.JId   |ID do periódico                                         |Int64      |Igual a
C.CN    |Nome da série de conferências                             |Cadeia     |Igual a,<br/>StartsWith
C.CId   |ID da série de conferências                               |Int64      |Igual a
RId     |ID dos artigos de referência                              |Int64[]    |Igual a
W       |Palavras do título e do resumo do artigo                |String[]   |Igual a
E       |Metadados estendidos (veja a tabela abaixo)                |Cadeia     |nenhum  
        


## <a name="extended-metadata-attributes"></a>Atributos de metadados estendidos ##

Nome    | Descrição               
--------|---------------------------    
DN      | Nome de exibição do artigo 
s       | Fontes – lista das fontes da Web do artigo, ordenadas por classificação estática
S.Ty    | Tipo de fonte (1:HTML, 2:Texto, 3:PDF, 4:DOC, 5:PPT, 6:XLS, 7:PS)
S.U     | URL de origem
VFN     | Nome completo do local – nome completo do periódico ou conferência
VSN     | Nome curto do local – nome curto do periódico ou conferência
V       | Volume – volume do periódico
BV      | Nome do periódico
BT      | 
PB      | Abreviações de diário
I       | Edição – edição do periódico
FP      | FirstPage – primeira página do artigo
LP      | LastPage – última página do artigo
DOI     | Identificador de Objeto Digital
CC      | Contextos de citação – lista de IDs dos artigos de referência e o contexto correspondente no artigo (por exemplo, [{123:[“as raposas marrons são conhecidas por pular conforme relatado no artigo”, “os cachorros preguiçosos são um equívoco histórico conforme mostrado no artigo 123”]})
IA      | Resumo invertido
IA.IndexLength| Número de itens no índice (contagem de palavras do resumo)
IA.InvertedIndex| Lista de palavras do resumo e sua posição correspondente no resumo original (por exemplo, [{"a": [0, 15, 30]}, {"raposa": [1]}, {"marrom":[2]}])
