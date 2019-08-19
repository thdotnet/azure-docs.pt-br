---
title: Resolução de sobrefestivals de projetos acústicos
titlesuffix: Azure Cognitive Services
description: Esta visão geral conceitual descreve a diferença entre resoluções de grande e refinas enquanto trazendo acústica.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 04/05/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: d8eb3b2cbaf7b4e842d8338eefde756f6d381111
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854360"
---
# <a name="project-acoustics-bake-resolution"></a>Resolução de sobrefestivals de projetos acústicos
Esta visão geral conceitual descreve a diferença entre resoluções de grande e refinas enquanto trazendo acústica. Você escolhe essa configuração durante a etapa investigações do fluxo de trabalho do trazendo.

## <a name="Coarse-vs-Fine-Resolution"></a>Resolução grossa vs fina

A única diferença entre as configurações de resolução grossa e fina é a frequência com que a simulação é executada. A fina usa uma frequência duas vezes mais alta que a grossa. Isso tem várias implicações na simulação acústica:

* O comprimento de onda para a grossa é duas vezes mais longo que a fina e, portanto, os voxels são duas vezes maiores.
* O tempo de simulação está diretamente relacionado ao tamanho do voxel, fazendo um bake grosso aproximadamente 16 vezes mais rápido que um bake fino.
* Portais (por exemplo, portas ou janelas) menores que o tamanho de VOXEL não podem ser simulados. A configuração grosseira pode fazer com que alguns desses portais menores não sejam simulados; Portanto, eles não passarão a soar no tempo de execução. Você pode ver se isso está acontecendo, visualizando os voxels.
* A menor frequência de simulação resulta em menos difração ao redor dos cantos e bordas.
* Fontes de som não podem ser localizadas dentro de voxels "preenchidas" (ou seja, voxels que contêm geometria). Isso resulta em sem som. É mais difícil inserir fontes de som para que elas não fiquem dentro do voxels maior do que é ao usar a configuração correta.
* Os voxels maiores se intrometem mais nos portais, como mostrado abaixo. A primeira imagem foi criada usando resolução grossa, enquanto a segunda é a mesma porta usando resolução fina. Como indicado pelas marcas vermelhas, há muito menos intrusão na entrada usando a configuração fina. A linha azul é a porta conforme definida pela geometria, enquanto a linha vermelha é o portal acústico efetivo definido pelo tamanho do voxel. Como essa intrusão ocorre em uma determinada situação depende completamente de como os voxels se alinham com a geometria do portal, que é determinada pelo tamanho e localização de seus objetos na cena.

![Captura de tela de voxels grossos preenchendo uma porta de entrada no Unreal](media/unreal-coarse-bake.png)

![Captura de tela de voxels finos preenchendo uma porta de entrada no Unreal](media/unreal-fine-bake.png)

## <a name="next-steps"></a>Próximas etapas

Experimente as configurações de resolução grande e refinada usando nossos plug-ins [inreais](unreal-baking.md) ou do [Unity](unity-baking.md).
