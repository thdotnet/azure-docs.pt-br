---
title: Localizar momentos exatos em vídeos – Video Indexer
titlesuffix: Azure Media Services
description: Este tópico demonstra como localizar momentos exatos em vídeos usando o Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 93376059dac2a8336e581269d8289d893779b291
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65799102"
---
# <a name="find-exact-moments-within-videos"></a>Localizar os minutos exatos nos vídeos

Este tópico mostra as opções de pesquisa que permitem que você localize os minutos exatos nos vídeos.

1. Navegue até o site do [Video Indexer](https://www.videoindexer.ai/) e entre.
2. Pesquise entre todos os vídeos da sua conta.

    No exemplo a seguir, podemos pesquisar todos os vídeos que falam sobre a segurança e na qual Satya é exibida,

    ![Search](./media/video-indexer-search/video-indexer-search01.png)
3. Pesquise os insights resumidos do vídeo.

    Em seguida, é possível pesquisar dentro de um vídeo clicando em **Reproduzir** no vídeo. Em seguida, é possível pesquisar no vídeo selecionando a guia **Pesquisar**. 

    No exemplo a seguir, vamos procurar "segura" dentro do vídeo selecionado.

    ![Search](./media/video-indexer-search/video-indexer-search02.png)

    Se você clicar em um dos resultados, o player direcionará você até esse momento no vídeo. É possível obter a exibição do player/insights e a sincronização no seu aplicativo. Para obter mais informações, consulte [Embed Video Indexer widgets into your application](video-indexer-embed-widgets.md) (Inserir widgets do Video Indexer em seu aplicativo). 
4. Pesquise a análise detalhada do vídeo.
    
    Se você quiser criar seus próprios clipes com base no vídeo que você encontrou, pressione a **editar** botão. Esta página mostra vídeo junto com seus insights como filtros. Para obter mais informações, consulte [Exibir e editar insights do Video Indexer](video-indexer-view-edit.md). 

    Você pode pesquisar no vídeo para mostrar apenas as linhas que você está interessado e usa os insights de lado para filtrar as partes que você deseja ver. Quando você terminar, você pode visualizar o clipe e pressione **publicar** para criar o novo clipe que aparece na Galeria de.
    
    O exemplo a seguir, procuramos o texto "realidade misturada". Também aplicamos filtros adicionais, conforme mostrado na tela abaixo.
    
    ![Search](./media/video-indexer-search/video-indexer-search03.png)

## <a name="next-steps"></a>Próximas etapas 

Depois de encontrar o vídeo com o qual você deseja trabalhar, será possível continuar processando-o, conforme descrito em um destes tópicos: 

- [Use o aprofundamento dos seus vídeos](use-editor-create-project.md)
- [Processar o conteúdo com a API REST do Video Indexer](video-indexer-use-apis.md)
- [Inserir widgets visuais em seu aplicativo](video-indexer-embed-widgets.md)

## <a name="see-also"></a>Consulte também

[Visão geral do indexador vídeo](video-indexer-overview.md)
