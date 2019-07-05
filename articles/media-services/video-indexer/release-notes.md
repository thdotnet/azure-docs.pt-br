---
title: Notas de versão de indexador de vídeo de serviços de mídia do Azure | Microsoft Docs
description: Para se manter atualizado com os desenvolvimentos mais recentes, este artigo fornece as últimas atualizações do indexador de vídeo de serviços de mídia do Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.subservice: video-indexer
ms.workload: na
ms.topic: article
ms.date: 06/25/2019
ms.author: juliako
ms.openlocfilehash: f1c5f43316292f17547b84d970a0f03a1a2c366f
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454016"
---
# <a name="azure-media-services-video-indexer-release-notes"></a>Notas de versão de indexador de vídeo de serviços de mídia do Azure

Para se manter atualizado com os desenvolvimentos mais recentes, este artigo fornece informações sobre:

* As versões mais recentes
* Problemas conhecidos
* Correções de bug
* Funcionalidades preteridas

## <a name="june-2019"></a>Junho de 2019

### <a name="video-indexer-deployed-to-japan-east"></a>Indexador de vídeo implantado para o Leste do Japão

Agora você pode criar um indexador de vídeo pago conta na região Leste do Japão.

### <a name="create-and-repair-account-api-preview"></a>Criar e reparar a conta API (versão prévia)

Adicionada uma nova API que permite que você [atualizar o ponto de extremidade de conexão de serviço de mídia do Azure ou a chave](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Paid-Account-Azure-Media-Services?&groupBy=tag).

### <a name="improve-error-handling-on-upload"></a>Melhorar o carregamento de tratamento de erros 

Uma mensagem descritiva é retornada em caso de uma configuração incorreta da conta dos serviços de mídia do Azure subjacente.

### <a name="player-timeline-keyframes-preview"></a>Visualização de quadros-chave de linha do tempo do Player 

Agora você pode ver uma visualização da imagem de cada vez na linha de tempo do jogador.

### <a name="editor-semi-select"></a>Editor semisseletivo

Agora você pode ver uma visualização das informações que são selecionados como resultado de escolher um período de tempo de insight específicos no editor.

## <a name="may-2019"></a>Maio de 2019

### <a name="update-custom-language-model-from-closed-caption-file"></a>Atualizar o modelo de idioma personalizadas de arquivo de legenda codificada

[Criar modelo de idioma personalizado](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Create-Language-Model?&groupBy=tag) e [atualizar modelos de linguagem personalizados](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Language-Model?&groupBy=tag) APIs agora dão suporte a VTT, SRT, e formatos de arquivo TTML como entrada para modelos de linguagem.

Ao chamar o [transcrição de vídeo de atualização API](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Update-Video-Transcript?&pattern=transcript), a transcrição é adicionada automaticamente. O modelo de treinamento associado com o vídeo é atualizado automaticamente também. Para obter informações sobre como personalizar e treinar seus modelos de linguagem, consulte [personalizar um modelo de linguagem com o indexador de vídeo](customize-language-model-overview.md).

### <a name="new-download-transcript-formats--txt-and-csv"></a>Novos formatos de transcrição de download – TXT e CSV

Além de Legendagem oculta formato fechado já com suporte (SRT, VTT e TTML), o indexador de vídeo agora dá suporte a baixando a transcrição em formatos CSV e TXT.

## <a name="next-steps"></a>Próximas etapas

[Visão geral](video-indexer-overview.md)
