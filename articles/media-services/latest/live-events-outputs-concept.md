---
title: Conceitos de transmissão ao vivo nos Serviços de Mídia do Azure - Eventos ao Vivo e Saídas Dinâmicas | Microsoft Docs
description: Este artigo fornece uma visão geral da transmissão ao vivo usando os Serviços de Mídia do Azure v3.
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
ms.date: 08/26/2019
ms.author: juliako
ms.openlocfilehash: c81c2de180a2c5734f3896d4b6843f2ccccdf45f
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/03/2019
ms.locfileid: "70231201"
---
# <a name="live-events-and-live-outputs"></a>Eventos ao Vivo e Saídas Dinâmicas

O Azure Media Services permite entregar eventos ao vivo para seus clientes na nuvem do Azure. Para configurar seus eventos de transmissão ao vivo nos serviços de mídia v3, você precisa entender os conceitos discutidos neste artigo.

> [!TIP]
> Para clientes que estão migrando das APIs do Media Services v2, a entidade de **evento ao vivo** substitui o **canal** em v2 e a **saída ao vivo** substitui o **programa**.

## <a name="live-events"></a>Eventos ao Vivo

[Eventos ao Vivo](https://docs.microsoft.com/rest/api/media/liveevents) são responsáveis pela ingestão e pelo processamento dos feeds de vídeo ao vivo. Quando você cria um evento ao vivo, é criado um ponto de extremidade de entrada primário e secundário que você pode usar para enviar um sinal ao vivo de um codificador remoto. O codificador remoto dinâmico envia o feed de contribuição para esse ponto de extremidade de entrada usando o protocolo de entrada [RTMP](https://www.adobe.com/devnet/rtmp.html) ou [Smooth streaming](https://msdn.microsoft.com/library/ff469518.aspx) (fragmentado-MP4). Para o protocolo de ingestão RTMP, o conteúdo pode ser enviado em Clear`rtmp://`() ou criptografado com segurança na conexão`rtmps://`(). Para o protocolo de ingestão Smooth Streaming, os esquemas de URL compatíveis são `http://` ou `https://`.  

## <a name="live-event-types"></a>Tipos de eventos ao vivo

Um [Evento ao vivo](https://docs.microsoft.com/rest/api/media/liveevents) pode ser de dois tipos: de passagem e de codificação ativa. Os tipos são definidos durante a criação usando [LiveEventEncodingType](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencodingtype):

* **LiveEventEncodingType. None** -um codificador ao vivo local envia um fluxo de taxas de bits múltiplas. Os fluxos ingeridos passam pelo evento ao vivo sem nenhum processamento adicional. 
* **LiveEventEncodingType. Standard** -um codificador ao vivo local envia um fluxo de taxa de bits única para o evento ao vivo e os serviços de mídia criam fluxos de taxas de bits múltiplas. Se o feed de contribuição for de 720p ou de resolução superior , a predefinição de default720p codificará um conjunto de 6 pares de resolução/taxa de bits.
* **LiveEventEncodingType. Premium1080p** -um codificador ao vivo local envia um fluxo de taxa de bits única para o evento ao vivo e os serviços de mídia criam fluxos de taxas de bits múltiplas. A predefinição de default1080p especifica o conjunto de saída de pares de resolução/taxa de bits. 

### <a name="pass-through"></a>Passagem

![passagem](./media/live-streaming/pass-through.svg)

Ao usar o **Evento ao vivo** de passagem, você depende de seu codificador dinâmico local para gerar um fluxo de vídeo com várias taxas de bits e enviá-lo como o feed de contribuição para o Evento ao vivo (usando o protocolo RTMP ou MP4 fragmentado). A seguir, o Evento ao vivo executa os fluxos de vídeo de entrada sem qualquer processamento adicional. Esse evento ao vivo de passagem é otimizado para eventos ao vivo de execução longa ou transmissão ao vivo linear 24x365. Ao criar esse tipo de Evento ao vivo, especifique Nenhum (LiveEventEncodingType.None).

Você pode enviar a contribuição em resoluções de até 4K e em uma taxa de quadros de 60 quadros / segundo, com codecs de vídeo H.264/AVC ou H.265/HEVC e AAC (AAC-LC, HE-AACv1 ou HE-AACv2) codec de áudio.  Confira o artigo [Comparação de tipos de Eventos ao Vivo](live-event-types-comparison.md) para saber mais.

> [!NOTE]
> Usar um método de passagem é a maneira mais econômica de fazer uma transmissão ao vivo quando você estiver fazendo vários eventos durante um longo período e já tiver investido em codificadores locais. Veja os detalhes de [preços](https://azure.microsoft.com/pricing/details/media-services/) .
> 

Veja um exemplo de código do .NET no [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Codificação ativa  

![Codificação ativa](./media/live-streaming/live-encoding.svg)

Usando a codificação dinâmica com os Serviços de Mídia, você configuraria seu codificador dinâmico local para enviar um vídeo de taxa de bits única como o feed de contribuição para o Evento ao vivo (usando o protocolo RTMP ou MP4 fragmentado). Em seguida, você configuraria um evento ao vivo para que ele codificasse o fluxo de taxa de bits única de entrada em um [fluxo de vídeo de várias taxas de bits](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)e disponibilizará a saída para a entrega a fim de reproduzir dispositivos por meio de protocolos como MPEG-Dash, HLS e Smooth streaming.

Ao usar a codificação ativa, você pode enviar o feed de contribuição somente em resoluções até a resolução de uma taxa de quadros de 30 quadros/segundo, com H. 264/AVC Video Codec e o codec de áudio AAC (AAC-LC, HE-AACv1 ou HE-AACv2). Observe que os eventos ao vivo de passagem podem dar suporte a resoluções de até 4K em 60 quadros/segundo. Confira o artigo [Comparação de tipos de Eventos ao Vivo](live-event-types-comparison.md) para saber mais.

As resoluções e taxas de bits contidas na saída do codificador ao vivo são determinadas pela predefinição. Se você estiver usando um codificador ativo **padrão** (LiveEventEncodingType. Standard), a predefinição default720p especificará um conjunto de 6 pares de taxa de resolução/bit, indo de 720p a 3,5 Mbps para 192p a 200 Kbps. Caso contrário, se estiver usando um codificador **Premium1080p** Live (LiveEventEncodingType. Premium1080p), a predefinição de default1080p especificará um conjunto de 6 pares de taxa de resolução/bit, indo de 1080p a 3,5 Mbps para 180p a 200 Kbps. Para obter informações, confira [Predefinições do sistema](live-event-types-comparison.md#system-presets).

> [!NOTE]
> Se você precisar personalizar a predefinição de codificação ativa, abra um tíquete de suporte via portal do Azure. Você deve especificar a tabela desejada da resolução e das taxas de bits. Verifique se há apenas uma camada em 720p (se solicitar uma predefinição para um codificador ao vivo padrão) ou em 1080p (se solicitar uma predefinição para um codificador ao vivo Premium1080p) e no máximo 6 camadas.

## <a name="live-event-creation-options"></a>Opções de criação de Evento ao vivo

Ao criar um Evento ao vivo, você pode especificar as seguintes opções:

* O protocolo de streaming para o Evento ao vivo (atualmente, os protocolos RTMP e Smooth Streaming são compatíveis).<br/>Não é possível alterar a opção de protocolo enquanto o Evento ao vivo ou suas Saídas ao vivo associadas estiverem em execução. Se você precisar de protocolos diferentes, crie um Evento ao vivo separado para cada protocolo de streaming.  
* Restrições de IP sobre a ingestão e versão prévia. É possível definir os endereços IP que têm permissão para ingerir um vídeo neste Evento ao vivo. Os endereços IP permitidos podem ser especificados como um endereço IP único (por exemplo, '10.0.0.1'), um intervalo IP usando um endereço IP e uma máscara de sub-rede CIDR (por exemplo, '10.0.0.1/22) ou um intervalo IP usando um endereço IP e uma máscara de sub-rede com notação decimal com ponto (por exemplo, '10.0.0.1(255.255.252.0)').<br/>Se nenhum endereço IP for especificado e não houver definição de regra, nenhum endereço IP será permitido. Para permitir qualquer endereço IP, crie uma regra e defina 0.0.0.0/0.<br/>Os endereços IP devem estar em um dos formatos a seguir: endereço IPv4 com 4 números e intervalo de endereços CIDR.
* Ao criar o evento, é possível especificar para iniciá-lo automaticamente. <br/>Quando a inicialização automática é definida como verdadeira, o evento em tempo real será iniciado após a criação. A cobrança começa assim que o Evento ao vivo começa a ser transmitido. É necessário chamar explicitamente o recurso Parar no Evento ao vivo para parar a cobrança adicional. Como alternativa, você poderá iniciar o evento quando estiver pronto para iniciar o streaming. 

    Para saber mais, confira [Estados e cobrança do Evento ao vivo](live-event-states-billing.md).

## <a name="live-event-ingest-urls"></a>URLs de ingestão de Evento ao vivo

Após criar o Evento ao vivo, é possível obter URLs de ingestão que serão fornecidas ao codificador dinâmico local. O codificador dinâmico usa essas URLs para gerar a entrada de um fluxo ao vivo. Para saber mais, confira [Codificadores dinâmicos locais recomendados](recommended-on-premises-live-encoders.md). 

Você pode usar URLs intuitivas ou não intuitivas. 

> [!NOTE] 
> Para que uma URL de ingestão seja preditiva, defina o modo de "personalização".

* URL não intuitiva

    A URL não intuitivo é o modo padrão no Media Services V3. Você possivelmente obterá o Evento ao vivo rapidamente, mas a URL de ingestão é conhecida apenas ao iniciar o evento ao vivo. A URL será alterada se você parar/iniciar o Evento ao vivo. <br/>A URL não intuitiva é útil em cenários em que um usuário final deseja fazer uma transmissão usando um aplicativo que deseja obter um evento ao vivo o mais rápido possível e ter uma URL de ingestão dinâmica não é um problema.
    
    Se um aplicativo cliente não precisar gerar previamente uma URL de ingestão antes que o evento ao vivo seja criado, basta permitir que os serviços de mídia gerem automaticamente o token de acesso para o evento ao vivo.
* URL intuitiva

    A URL intuitiva é preferida pelas grandes emissoras de mídia, que usam codificadores de transmissão de hardware e não querem reconfigurar seus codificadores ao iniciar o Evento ao vivo. Elas querem uma URL de ingestão preditiva, que não muda com o tempo.
    
    Para especificar esse modo, defina `vanityUrl` como `true` no momento da criação (o padrão `false`é). Você também precisa passar seu próprio token de acesso (`LiveEventInput.accessToken`) no momento da criação. Especifique o valor do token para evitar um token aleatório na URL. O token de acesso deve ser uma cadeia de caracteres GUID válida (com ou sem os hifens). Depois que o modo for definido, ele não poderá ser atualizado.

    O token de acesso precisa ser exclusivo em seu data center. Se seu aplicativo precisar usar uma URL intuitivo, é recomendável sempre criar uma nova instância de GUID para seu token de acesso (em vez de reutilizar qualquer GUID existente). 

    Use as seguintes APIs para habilitar a URL intuitivo e definir o token de acesso para um GUID válido (por `"accessToken": "1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`exemplo).  
    
    |Idioma|Habilitar URL intuitivo|Definir token de acesso|
    |---|---|---|
    |REST|[properties.vanityUrl](https://docs.microsoft.com/rest/api/media/liveevents/create#liveevent)|[LiveEventInput.accessToken](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventinput)|
    |CLI|[--vanity-url](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#az-ams-live-event-create)|[--access-token](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#optional-parameters)|
    |.NET|[LiveEvent.VanityUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent.vanityurl?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEvent_VanityUrl)|[LiveEventInput.AccessToken](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveeventinput.accesstoken?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEventInput_AccessToken)|
    
### <a name="live-ingest-url-naming-rules"></a>Regras de nomenclatura de URL de ingestão dinâmica

* A cadeia de caracteres *aleatória* abaixo é um número hexadecimal de 128 bits composto de 32 caracteres de “0” a “9” e “a” a “f”.
* *seu token de acesso* -a cadeia de caracteres GUID válida que você define ao usar o modo intuitivo. Por exemplo, `"1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`.
* *nome do fluxo* – indica o nome do fluxo para uma conexão específica. O valor do nome do fluxo geralmente é adicionado pelo codificador ao vivo que você usa. Você pode configurar o codificador ao vivo para usar qualquer nome para descrever a conexão, por exemplo: "video1_audio1", "video2_audio1", "Stream".

#### <a name="non-vanity-url"></a>URL não intuitiva

##### <a name="rtmp"></a>RTMP

`rtmp://<random 128bit hex string>.channel.media.azure.net:1935/live/<auto-generated access token>/<stream name>`<br/>
`rtmp://<random 128bit hex string>.channel.media.azure.net:1936/live/<auto-generated access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2935/live/<auto-generated access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2936/live/<auto-generated access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Smooth Streaming

`http://<random 128bit hex string>.channel.media.azure.net/<auto-generated access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<random 128bit hex string>.channel.media.azure.net/<auto-generated access token>/ingest.isml/streams(<stream name>)`<br/>

#### <a name="vanity-url"></a>URL intuitiva

##### <a name="rtmp"></a>RTMP

`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1935/live/<your access token>/<stream name>`<br/>
`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1936/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2935/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2936/live/<your access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Smooth Streaming

`http://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>

## <a name="live-event-preview-url"></a>URL de visualização do Evento ao vivo

Depois que o evento ao vivo começar a receber o feed de contribuição, você poderá usar seu ponto de extremidade de visualização para visualizar e validar que você está recebendo a transmissão ao vivo antes da publicação adicional. Depois de verificar se o fluxo de visualização é bom, você pode usar o evento ao vivo para disponibilizar o Live Stream para entrega por meio de um ou mais pontos de extremidade de streaming (pré-criados). Para fazer isso, você cria uma nova [saída ao vivo](https://docs.microsoft.com/rest/api/media/liveoutputs) no evento ao vivo. 

> [!IMPORTANT]
> Certifique-se de que o vídeo está fluindo para a URL de visualização antes de continuar!

## <a name="live-event-long-running-operations"></a>Operações de longa execução de evento ao vivo

Para obter detalhes, consulte [operações de execução longa](media-services-apis-overview.md#long-running-operations)

## <a name="live-outputs"></a>Saídas ao Vivo

Uma vez que o fluxo esteja fluindo para o Evento ao vivo, é possível começar o evento de transmissão criando um [Ativo](https://docs.microsoft.com/rest/api/media/assets), uma [Saída ao vivo](https://docs.microsoft.com/rest/api/media/liveoutputs) e um [Localizador de streaming](https://docs.microsoft.com/rest/api/media/streaminglocators). A Saída ao vivo arquiva o fluxo e o disponibiliza para usuários por meio do [Ponto de extremidade de streaming](https://docs.microsoft.com/rest/api/media/streamingendpoints).  

Para obter informações detalhadas sobre as saídas ao vivo, consulte [usando um DVR de nuvem](live-event-cloud-dvr.md).

## <a name="ask-questions-give-feedback-get-updates"></a>Fazer perguntas, comentar, obter atualizações

Confira o artigo [comunidade dos Serviços de Mídia do Azure](media-services-community.md) para ver diferentes maneiras de fazer perguntas, comentários e obter atualizações sobre os serviços de mídia.

## <a name="next-steps"></a>Próximas etapas

[Tutorial de live streaming](stream-live-tutorial-with-api.md)
