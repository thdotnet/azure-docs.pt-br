---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: include
ms.date: 06/27/2019
ms.author: pafarley
ms.openlocfilehash: 690219347782aad2140b0a1b0541590c5426c568
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67459739"
---
O Reconhecimento de Formulários funciona em documentos de entrada que atendem aos seguintes requisitos:

* O formato deve ser JPG, PNG ou PDF (texto ou digitalizado). PDFs com texto inserido são o melhor porque não há nenhuma possibilidade de erro na extração e na localização de caracteres.
* O tamanho do arquivo precisa ser inferior a 4 MB (megabytes).
* Para imagens, as dimensões devem estar entre 50 x 50 pixels e 4.200 x 4.200 pixels.
* Se documentos em papel forem digitalizados, os formulários deverão ser digitalizações de alta qualidade.
* O texto precisa usar o alfabeto latino (caracteres em português).
* Os dados devem ser impressos (e não manuscritos).
* Os dados precisam conter chaves e valores.
* As chaves podem aparecer acima ou à esquerda dos valores, mas não abaixo nem à direita.

Atualmente o Reconhecimento de Formulários não dá suporte aos seguintes tipos de dados de entrada:

* Tabelas complexas (tabelas aninhadas, células ou cabeçalhos mesclados e assim por diante).
* Caixas de seleção ou botões de opção.
* Documentos PDF com mais de 50 páginas.