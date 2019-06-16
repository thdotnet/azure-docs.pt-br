---
title: Tipos de LiveEvent do Azure Media Services | Microsoft Docs
description: Este artigo mostra uma tabela detalhada que compara os tipos do LiveEvent.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/01/2019
ms.author: juliako
ms.openlocfilehash: bd4899374c06246ddd4d5fa81d0f6e3a6a1e7017
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67075013"
---
# <a name="live-event-types-comparison"></a>Comparação de tipos de Eventos ao Vivo

Nos Serviços de Mídia do Azure, um [Evento ao vivo](https://docs.microsoft.com/rest/api/media/liveevents) pode ser um dos dois tipos: codificação ativa e passagem. 

## <a name="types-comparison"></a>Comparação de tipos 

A tabela a seguir compara os recursos dos tipos de evento ao vivo.

| Recurso | Evento ao vivo de passagem | Padrão ou Premium1080p de evento ao vivo |
| --- | --- | --- |
| A entrada de taxa de bits única é codificada em várias taxas de bits na nuvem |Não |Sim |
| Resolução máxima de vídeo para feed de contribuição |4K (4096 x 2160 a 60 quadros / seg) |1080p de (1920 x 1088 em 30 quadros por segundo)|
| Camadas máximas recomendadas no feed de contribuição|Até 12|Um áudio|
| Camadas máximo na saída| Igual à entrada|Até 6 (veja abaixo as predefinições de sistema)|
| Largura de banda agregada máxima de feed de contribuição|60 Mbps|N/D|
| Taxa de bits máxima para uma única camada de contribuição |20 Mbps|20 Mbps|
| Suporte para várias faixas de áudio de idioma|Sim|Não|
| Codecs de vídeo de entrada com suporte |H.264/AVC e H.265/HEVC|H.264/AVC|
| Codecs de vídeo de saída com suporte|Igual à entrada|H.264/AVC|
| Suporte para a profundidade de bits de vídeo, entrada e saída|Até 10 bits incluindo HDR 10/HLG|8 bits|
| Codecs de áudio de entrada com suporte|AAC-LC, HE-AAC v1, HE-AAC v2|AAC-LC, HE-AAC v1, HE-AAC v2|
| Codecs de áudio de saída com suporte|Igual à entrada|AAC-LC|
| Resolução máxima de vídeo do vídeo de saída|Igual à entrada|Standard - 720p, Premium1080p - 1080p|
| Taxa máxima de quadros de vídeo de entrada|60 quadros por segundo|Padrão ou Premium1080p - 30 quadros por segundo|
| Protocolos de entrada|RTMP, MP4 fragmentado (Smooth Streaming)|RTMP, MP4 fragmentado (Smooth Streaming)|
| Preço|Confira a [página de preços](https://azure.microsoft.com/pricing/details/media-services/) e clique na guia “Vídeo ao vivo”|Confira a [página de preços](https://azure.microsoft.com/pricing/details/media-services/) e clique na guia “Vídeo ao vivo”|
| Tempo de execução máximo| 24 horas x 365 dias, linha ao vivo | Até 24 horas|
| Capacidade de passar por meio do embedded CEA 608/708 legendas de dados|Sim|Sim|
| Suporte para inserção de imagens fixas|Não|Não|
| Suporte para sinalização de anúncios via API| Não|Não|
| Suporte para o ad sinalização por meio de mensagens na faixa de SCTE-35|Sim|Sim|
| Capacidade de recuperação de interrupções breves no feed de contribuição|Sim|Parcial|
| Suporte para GOPs de entrada não uniforme|Sim|Não – entrada deve ter GOP duração fixa|
| Suporte para entrada de taxa de quadros variável|Sim|Não – a entrada deve ser uma taxa de quadros fixa. Pequenas variações são toleradas, por exemplo, durante cenas ricas em movimento. Mas o feed de contribuição não é possível descartar a taxa de quadros (por exemplo, a 15 quadros/segundo).|
| Desligamento automático do Evento ao Vivo quando há perda do feed de entrada|Não|Após 12 horas, se não houver nenhum LiveOutput em execução|

## <a name="system-presets"></a>Predefinições do sistema

As resoluções e taxas de bits contidas na saída do codificador ao vivo é determinado pelo [presetName](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencoding). Se usando um **Standard** live encoder (LiveEventEncodingType.Standard), em seguida, a *Default720p* predefinição Especifica um conjunto de pares de taxa de bits/resolução 6 descrito abaixo. Caso contrário, se usar um **Premium1080p** live encoder (LiveEventEncodingType.Premium1080p), em seguida, a *Default1080p* specifiesthe predefinido de conjunto de pares de taxa de bits/resolução de saída. 

### <a name="output-video-streams-for-default720p"></a>Fluxos de vídeo de saída para Default720p

**Default720p** codificará o vídeo de entrada em 6 camadas a seguir.

| Taxa de bits | Largura | Altura | MáxFPS | Perfil | Nome do fluxo de saída |
| --- | --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Alto |Video_1280x720_3500kbps |
| 2200 |960 |540 |30 |Alto |Video_960x540_2200kbps |
| 1350 |704 |396 |30 |Alto |Video_704x396_1350kbps |
| 850 |512 |288 |30 |Alto |Video_512x288_850kbps |
| 550 |384 |216 |30 |Alto |Video_384x216_550kbps |
| 200 |340 |192 |30 |Alto |Video_340x192_200kbps |

> [!NOTE]
> Se você precisar personalizar a predefinição de codificação ao vivo, abra um tíquete de suporte por meio do Portal do Azure. Você deve especificar a tabela desejada da resolução e das taxas de bits. Verifique se há apenas uma camada em 720p e no máximo seis camadas. Também especifique que você está solicitando uma predefinição para um codificador dinâmico padrão.

### <a name="output-video-streams-for-default1080p"></a>Fluxos de vídeo de saída para Default1080p

**Default1080p** codificará o vídeo de entrada em 6 camadas a seguir.

| Taxa de bits | Largura | Altura | MáxFPS | Perfil | Nome do fluxo de saída |
| --- | --- | --- | --- | --- | --- |
| 5500 |1920 |1080 |30 |Alto |Video_1920x1080_5500kbps |
| 3000 |1280 |720 |30 |Alto |Video_1280x720_3000kbps |
| 1600 |960 |540 |30 |Alto |Video_960x540_1600kbps |
| 800 |640 |360 |30 |Alto |Video_640x360_800kbps |
| 400 |480 |270 |30 |Alto |Video_480x270_400kbps |
| 200 |320 |180 |30 |Alto |Video_320x180_200kbps |

> [!NOTE]
> Se você precisar personalizar a predefinição de codificação ao vivo, abra um tíquete de suporte por meio do Portal do Azure. Você deve especificar a tabela desejada da resolução e das taxas de bits. Verifique se há apenas uma camada em 1080p e no máximo de 6 camadas. Também especifique que você está solicitando uma predefinição para um codificador ao vivo de Premium1080p.

### <a name="output-audio-stream-for-default720p-and-default1080p"></a>Stream de áudio de saída para Default720p e Default1080p

Para ambos *Default720p* e *Default1080p* predefinições de áudio é codificado como estéreo AAC-LC 128 Kbps, amostragem de taxa de 48 kHz.

## <a name="next-steps"></a>Próximas etapas

[Visão geral da transmissão ao vivo](live-streaming-overview.md)
