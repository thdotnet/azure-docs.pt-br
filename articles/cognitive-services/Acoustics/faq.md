---
title: Perguntas frequentes sobre o projeto acústicos
titlesuffix: Azure Cognitive Services
description: Esta página fornece respostas a perguntas frequentes sobre o Project Acoustics, incluindo instruções de download e processo de cozedura.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: f0484fc9fc2af5514ba0f5b61277146a51757057
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68855038"
---
# <a name="project-acoustics-frequently-asked-questions"></a>Perguntas frequentes sobre o projeto acústicos

## <a name="what-is-project-acoustics"></a>O que é Projeto Acústico?

O pacote de plug-ins acústicos do projeto é um sistema acústico que calcula o comportamento de som wave antes do tempo de execução, semelhante à iluminação estática. A nuvem faz o trabalho pesado dos cálculos de física de onda, portanto, o custo da CPU de tempo de execução é baixo.  

## <a name="where-can-i-download-the-plugin"></a>Onde posso baixar o plug-in?

Você pode baixar o [plug-in do módulo acústica do projeto](https://www.microsoft.com/download/details.aspx?id=57346) ou o [plug-in do projeto acústica inreal](https://www.microsoft.com/download/details.aspx?id=58090).

## <a name="does-project-acoustics-support-ltxgt-platform"></a>O projeto acústica dá suporte &lt;à&gt; plataforma x?

O suporte à plataforma acústica do projeto evolui com base nas necessidades do cliente. Entre em contato conosco no [Fórum de problemas do acústica do projeto](https://github.com/microsoft/ProjectAcoustics/issues) para saber mais sobre o suporte para plataformas adicionais.

## <a name="is-azure-used-at-runtime"></a>O Azure é usado em tempo de execução?

Não, a integração na nuvem é usada apenas durante o estágio de pré-computação que faz parte da configuração da cena.
 
## <a name="what-is-simulation-input"></a>O que é a entrada de simulação? 

A entrada da simulação é sua cena 3D, ambiente virtual ou nível do jogo. O Project Acoustics realiza simulações de ondas volumétricas em 3D que modelam a física do som de perto, incluindo a oclusão e o espalhamento suaves.
 
## <a name="what-is-the-runtime-cost"></a>O que é o custo de tempo de execução?

Acústica leva aproximadamente 0,01% da CPU por origem por quadro. Uso de RAM depende do tamanho da cena e pode variar de 10 a 100 MB.
 
## <a name="do-i-need-to-simplify-the-level-geometry-control-triangle-count-make-meshes-watertight"></a>Preciso simplificar a geometria do nível? Contagem do triângulo de controle? Verifique as malhas watertight?

Nº O sistema ingerirá geometria de nível detalhada diretamente. Será voxelizado para processamento interno.
 
## <a name="whats-in-the-runtime-lookup-table"></a>O que há na tabela de consulta de tempo de execução?

O arquivo ACE includes é uma tabela de parâmetros acústicos entre vários pares de localização de origem e de ouvinte, bem como a geometria de cena voxelized usada para a interpolação de parâmetro.
 
## <a name="can-project-acoustics-handle-moving-sources"></a>O projeto de acústica pode lidar com as fontes móveis?

Sim, o projeto acústicos consulta a tabela de pesquisa e atualiza o DSP de áudio em cada tique, para que possa lidar com as fontes e o ouvinte em movimento.
 
## <a name="can-project-acoustics-handle-dynamic-geometry-closing-doors-walls-blown-away"></a>Os projetos acústicos podem lidar com a geometria dinâmica? Fechando portas? Paredes surpreso?

Nº Os parâmetros acústicos são pré-computados com base no estado estático de um nível de jogo. Sugerimos deixar a geometria da porta fora dos acústicos e, em seguida, aplicar oclusão adicionais com base no estado dos objetos destrutíveis e de jogo móvel usando técnicas estabelecidas.
 
## <a name="does-project-acoustics-use-acoustic-materials"></a>Os projetos acústicos usam materiais acústicos?

Sim. Os materiais são escolhidos a partir dos nomes dos materiais físicos em seu nível, impulsionando a absorção.
 
## <a name="what-do-the-probes-represent"></a>O que representam os "testes"?

As sondas são uma amostra dos possíveis locais dos jogadores. Cada sonda representa uma simulação de onda separada da cena originada no local da sonda. Em tempo de execução, os parâmetros acústicos para o local do ouvinte são interpolados a partir dos locais de sonda próximos.
 
## <a name="why-spend-so-much-compute-in-the-cloud-what-does-it-buy-me"></a>Por que gastar muito computação na nuvem? O que isso me compra?

O Project Acoustics fornece parâmetros acústicos precisos e confiáveis, mesmo para ambientes virtuais ultra-complexos, levando em consideração todos os aspectos arquitetônicos. Ele fornece oclusão e obstrução suave e variação de reverberação dinâmica sem o trabalho manual dos volumes de desenho. Tudo isso enquanto o restante de luz na CPU durante o tempo de execução.

## <a name="what-exactly-happens-during-baking"></a>Exatamente, o que acontece durante "trazendo"?

Um distorta consiste em simulações de ondas acústicas de regiões de simulação de cuboid centralizadas em cada investigação de ouvinte.

## <a name="next-steps"></a>Próximas etapas
* Experimente o [conteúdo de exemplo do projeto acústicos do acústica](unity-quickstart.md) ou [conteúdo de exemplo não real](unreal-quickstart.md)

