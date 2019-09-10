---
title: O que é o Video Indexer dos Serviços de Mídia do Azure?
titleSuffix: Azure Media Services
description: Este tópico fornece uma visão geral do serviço de Video Indexer de serviços de mídia do Azure.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 09/06/2019
ms.author: juliako
ms.openlocfilehash: e3f60b5fb0693e40c9db040f7b14f487fce8f68e
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70860247"
---
# <a name="what-is-video-indexer"></a>O que é o Video Indexer?

O Video Indexer dos Serviços de Mídia do Azure é um aplicativo de nuvem criado com base na Análise de Mídia do Azure, no Azure Search, nos Serviços Cognitivos (como a API de Detecção Facial, o Microsoft Translator, a API da Pesquisa Visual Computacional e o Serviço de Fala Personalizado). Ele permite que você extraia os insights de seus vídeos usando os modelos de áudio e vídeo do Video Indexer descritos abaixo:
  
## <a name="video-insights"></a>Insights para vídeos

- **Detecção facial**: Detecta e agrupa rostos que aparecem no vídeo.
- **Identificação de celebridade**: Video Indexer identifica automaticamente mais de 1 milhão celebridades – como líderes do mundo, atores, actresses, atletas, pesquisadores, negócios e líderes de tecnologia em todo o mundo. Os dados sobre esses celebridades também podem ser encontrados em diversos sites famosos, assim como IMDB e Wikipédia.
- **Identificação de face baseada em conta**: O Video Indexer treina um modelo para uma conta específica. Em seguida, ele reconhece os rostos no vídeo com base no modelo treinado. Para obter mais informações, consulte [Personalizar um modelo Person no site video indexer](customize-person-model-with-website.md) e [Personalizar um modelo Person com a API Video indexer](customize-person-model-with-api.md).
- **Extração em miniatura para rosto** ("melhor rosto"): Identifica automaticamente o melhor rosto capturado em cada grupo de rostos (com base na qualidade, no tamanho e na posição frontal) e o extrai como um ativo de imagem.
- **Reconhecimento de texto visual** (OCR): Extrai o texto que é exibido visualmente no vídeo.
- **Moderação de conteúdo visual**: Detecta visuais somente para adultos e/ou eróticos.
- **Identificação de rótulos**: Identifica objetos visuais e ações exibidas.
- **Segmentação de cena**: determina quando uma cena é alterada em vídeo com base em indicações visuais. Uma cena descreve um único evento e é composta por uma série de capturas consecutivas, que são relacionadas semanticamente. 
- **Detecção de captura**: determina quando uma captura é alterada em vídeo com base em indicações visuais. Uma captura é uma série de quadros tirados da mesma câmera de imagem de movimento. Para obter mais informações, consulte [cenas, capturas e quadros-chave](scenes-shots-keyframes.md).
- **Detecção de quadro preto**: Identifica quadros pretos apresentados no vídeo.
- **Extração de quadro-chave**: Detecta quadros-chave estáveis em um vídeo.
- **Créditos sem interrupção**: identifique o início e o fim dos créditos contínuos no final de filmes e apresentações de TV.
- **Detecção de caracteres animados** (visualização): detecção, agrupamento e reconhecimento de caracteres em conteúdo animado por meio da integração com a [visão personalizada dos serviços cognitivas](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/). Para obter mais informações, consulte a [detecção de caracteres animados](animated-characters-recognition.md).
- **Detecção de tipo editorial shot**: marcação de capturas com base em seu tipo (como ampla captura, imagem média, fechamento, extremo, superior, duas telas, várias pessoas, externamente e em interno, etc.). Para obter mais informações, consulte [detecção do tipo editorial shot](scenes-shots-keyframes.md#editorial-shot-type-detection).

## <a name="audio-insights"></a>Insights de áudio

- **Detecção automática de idioma**: Identifica automaticamente o idioma falado dominante. Os idiomas com suporte incluem Inglês, espanhol, francês, alemão, italiano, chinês (simplificado), japonês, russo e português brasileiro. Se o idioma não puder ser identificado com confiança, o Video Indexer assumirá que o idioma falado é o inglês. Para obter mais informações, consulte [modelo de identificação de idioma](language-identification-model.md).
- **Identificação e transcrição de fala em vários idiomas** (visualização): Identifica automaticamente o idioma falado em diferentes segmentos do áudio, enviando cada segmento do arquivo de mídia a ser transcrita e combinando a transcrição de volta a uma transcrição unificada. Para obter mais informações, consulte [identificar e transcrever automaticamente o conteúdo em vários idiomas](multi-language-identification-transcription.md).
- **Transcrição de áudio**: Conversão de fala em texto em 12 idiomas e permite extensões. Os idiomas com suporte incluem Inglês, espanhol, francês, alemão, italiano, chinês (simplificado), japonês, árabe, russo, Português do Brasil, híndi e coreano.
- **Legendagem oculta**: Cria legendas ocultas em três formatos: VTT, TTML, SRT.
- **Processamento de dois canais**: Detecta automaticamente, separa a transcrição e mescla em uma única linha do tempo.
- **Redução de ruído**: Limpa gravações com ruídos ou áudio com telefonia (com base nos filtros do Skype).
- **Personalização de transcrição** (CRIS): Treina a fala personalizada em modelos de texto para criar transcrições específicas do setor. Para obter mais informações, consulte [Personalizar um modelo de linguagem no site video indexer](customize-language-model-with-website.md) e [Personalizar um modelo de linguagem com as APIs de video indexer](customize-language-model-with-api.md).
- **Enumeração de alto-falante**: Mapeia e entende qual locutor disse quais palavras e quando.
- **Estatísticas de alto-falante**: Fornece estatísticas das taxas de fala dos alto-falantes.
- **Moderação de conteúdo visual**: Detecta texto explícito na transcrição de áudio.
- **Efeitos de áudio**: Identifica efeitos de áudio como palmas, fala e silêncio.
- **Detecção de emoções**: Identifica emoções com base na fala (o que está sendo dito) e a tonalidade de voz (como ele está sendo dito).  A emoção pode ser: alegria, tristeza, raiva ou medo.
- **Tradução**: Cria traduções da transcrição de áudio em 54 idiomas diferentes.

## <a name="audio-and-video-insights-multi-channels"></a>Insights de áudio e vídeo (vários canais)

Ao fazer a indexação por um canal, o resultado parcial desses modelos estará disponível

- **Extração de palavra-chave**: Extrai palavras-chave de falas e de textos visuais.
- **Extração de entidades nomeadas**: Extrai marcas, locais e pessoas de fala e texto visual por meio do NLP (processamento de idioma natural).
- **Inferência do tópico**: Faz inferências dos principais tópicos das transcrições. A taxonomia de IPTC de 1º nível está incluída.
- **Artefatos**: Extrai um conjunto avançado de artefatos com o "próximo nível de detalhes" para cada um dos modelos.
- **Análise de Sentimento**: Identifica sentimentos positivos, negativos e neutros de falas e de textos visuais.
 
Depois que o Video Indexer terminar de processar e analisar, você poderá examinar, coletar, pesquisar e publicar as informações do vídeo.

Se sua função for um gerenciador de conteúdo ou um desenvolvedor, o serviço do Video Indexer poderá atender às suas necessidades. Os gerenciadores de conteúdo podem usar o portal da Web do Video Indexer para consumir o serviço sem escrever uma única linha de código; confira [Começar a usar o site do Video Indexer](video-indexer-get-started.md). Os desenvolvedores podem aproveitar as APIs para processar o conteúdo em grande escala; consulte [Usar API REST do Video Indexer](video-indexer-use-apis.md). O serviço também permite que os clientes usem widgets para publicar transmissões de vídeo e informações extraídas em seus próprios aplicativos; consulte [Inserir widgets visuais em seu aplicativo](video-indexer-embed-widgets.md).

Você pode se inscrever no serviço usando a conta existente do AAD, LinkedIn, Facebook, Google ou MSA. Para obter mais informações, consulte o [guia de introdução](video-indexer-get-started.md).

## <a name="scenarios"></a>Cenários

Abaixo estão alguns cenários em que o Video Indexer pode ser útil

- Pesquisa: as informações extraídas do vídeo podem ser usadas para aprimorar a experiência de pesquisa em uma biblioteca de vídeos. Por exemplo, a indexação das palavras faladas e faces pode habilitar a experiência de pesquisa para localizar momentos em um vídeo em que determinada pessoa falou certas palavras ou quando duas pessoas foram vistas junto. A pesquisa com base em tais informações de vídeos é aplicável a agências de notícias, instituições de ensino, emissoras, proprietários de conteúdo de entretenimento, aplicativos de linha de negócios e, em geral, a qualquer setor que tenha uma biblioteca de vídeos que os usuários precisam pesquisar.
- Criação de conteúdo – ideias extraídas de vídeos e ajudam a criar conteúdo com eficiência, como trailers, conteúdo de mídia social, clipes de notícias etc. de conteúdo existente no arquivo da organização 
- Monetização: o Video Indexer pode ajudar a melhorar o valor dos vídeos. Por exemplo, setores que contam com a receita de anúncios (como a mídia de notícias, mídias sociais etc.) podem entregar anúncios mais relevantes usando as informações extraídas como sinais adicionais para o servidor do anúncio (apresentar um anúncio de sapatos esportivos é mais relevante durante uma partida de futebol do que em uma competição de natação).
- Participação do usuário: as informações do vídeo podem ser usadas para melhorar a participação do usuário posicionando os momentos do vídeo relevantes para os usuários. Por exemplo, considere um vídeo educativo que explica as esferas nos primeiros 30 minutos e as pirâmides nos próximos 30 minutos. Um aluno que lê sobre as pirâmides aproveitaria mais se o vídeo fosse posicionado para começar a partir do marcador de 30 minutos.

## <a name="next-steps"></a>Próximas etapas

Você está pronto para começar com o Video Indexer. Para obter mais informações, confira os seguintes artigos:

- [Introdução ao site do Video Indexer](video-indexer-get-started.md)
- [Processar o conteúdo com a API REST do Video Indexer](video-indexer-use-apis.md)
- [Inserir widgets visuais em seu aplicativo](video-indexer-embed-widgets.md)
