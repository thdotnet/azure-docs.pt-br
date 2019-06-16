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
ms.date: 06/12/2019
ms.author: juliako
ms.openlocfilehash: 49ab52f031e24ac77a534c86061fe831bbec39ce
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67114664"
---
# <a name="live-events-and-live-outputs"></a>Eventos ao Vivo e Saídas Dinâmicas

O Azure Media Services permite entregar eventos ao vivo para seus clientes na nuvem do Azure. Para configurar os eventos de transmissão ao vivo nos serviços de mídia v3, você precisa entender os conceitos discutidos neste artigo.

> [!TIP]
> Migrando de APIs do serviços de mídia v2, o **evento ao vivo** entidade substitui **canal** na v2 e **Live saída** substitui **programa**.


## <a name="live-events"></a>Eventos ao Vivo

[Eventos ao Vivo](https://docs.microsoft.com/rest/api/media/liveevents) são responsáveis pela ingestão e pelo processamento dos feeds de vídeo ao vivo. Quando você cria um Evento ao vivo, é criado um ponto de extremidade de entrada que pode ser usado para enviar um sinal ao vivo de um codificador remoto. O codificador dinâmico remoto envia o feed de contribuição para esse terminal de entrada usando o protocolo [RTMP](https://www.adobe.com/devnet/rtmp.html) ou [Smooth Streaming](https://msdn.microsoft.com/library/ff469518.aspx) (fragmented-MP4). Para o protocolo de ingestão Smooth Streaming, os esquemas de URL compatíveis são `http://` ou `https://`. Para o protocolo de ingestão RTMP, os esquemas de URL com suporte são `rtmp://` ou `rtmps://`. 

## <a name="live-event-types"></a>Tipos de eventos ao vivo

Um [Evento ao vivo](https://docs.microsoft.com/rest/api/media/liveevents) pode ser de dois tipos: de passagem e de codificação ativa. 

### <a name="pass-through"></a>Passagem

![passagem](./media/live-streaming/pass-through.svg)

Ao usar o **Evento ao vivo** de passagem, você depende de seu codificador dinâmico local para gerar um fluxo de vídeo com várias taxas de bits e enviá-lo como o feed de contribuição para o Evento ao vivo (usando o protocolo RTMP ou MP4 fragmentado). A seguir, o Evento ao vivo executa os fluxos de vídeo de entrada sem qualquer processamento adicional. Esse LiveEvent de passagem é otimizado para eventos ao vivo de longa duração ou streaming ao vivo linear 24x365. Ao criar esse tipo de Evento ao vivo, especifique Nenhum (LiveEventEncodingType.None).

Você pode enviar a contribuição em resoluções de até 4K e em uma taxa de quadros de 60 quadros / segundo, com codecs de vídeo H.264/AVC ou H.265/HEVC e AAC (AAC-LC, HE-AACv1 ou HE-AACv2) codec de áudio.  Confira o artigo [Comparação de tipos de Eventos ao Vivo](live-event-types-comparison.md) para saber mais.

> [!NOTE]
> Usar um método de passagem é a maneira mais econômica de fazer uma transmissão ao vivo quando você estiver fazendo vários eventos durante um longo período e já tiver investido em codificadores locais. Veja os detalhes de [preços](https://azure.microsoft.com/pricing/details/media-services/) .
> 

Veja um exemplo de código do .NET no [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Codificação ativa  

![Codificação ativa](./media/live-streaming/live-encoding.svg)

Usando a codificação dinâmica com os Serviços de Mídia, você configuraria seu codificador dinâmico local para enviar um vídeo de taxa de bits única como o feed de contribuição para o Evento ao vivo (usando o protocolo RTMP ou MP4 fragmentado). Você poderia então definir um evento ao vivo, de modo que ele codifica essa taxa de bits única entrada de fluxo para um [transmissão de vídeo de taxa de bits múltiplas](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming)e disponibiliza a saída para entrega para reproduzir a dispositivos através de protocolos, como MPEG-DASH, HLS e Smooth Streaming.

Quando você usa a codificação ativa, você pode enviar a contribuição do feed somente em resoluções-se a resolução de 1080p em uma taxa de quadros de 30 quadros/segundo, com o codec de vídeo H.264/AVC e AAC (AAC-LC, HE-AACv1 ou HE-AACv2) codec de áudio. Observe que passagem de eventos ao vivo pode dar suporte a resoluções de até 4K em 60 quadros por segundo. Confira o artigo [Comparação de tipos de Eventos ao Vivo](live-event-types-comparison.md) para saber mais.

As resoluções e taxas de bits contidas na saída do codificador ao vivo é determinado pela predefinição. Se usando um **Standard** live encoder (LiveEventEncodingType.Standard), em seguida, a *Default720p* predefinição Especifica um conjunto de pares de taxa de bits/resolução 6, indo de 720p em 3.5Mbps até p 192 Kbps 200. Caso contrário, se usar um **Premium1080p** live encoder (LiveEventEncodingType.Premium1080p), em seguida, a *Default1080p* predefinição Especifica um conjunto de pares de taxa de bits/resolução 6, indo de 1080p em 3.5Mbps até 180 p 200 Kbps. Para obter informações, confira [Predefinições do sistema](live-event-types-comparison.md#system-presets).

> [!NOTE]
> Se você precisar personalizar a predefinição de codificação ao vivo, abra um tíquete de suporte por meio do portal do Azure. Você deve especificar a tabela desejada da resolução e das taxas de bits. Verifique se há apenas uma camada em 720p (se solicitando uma predefinição para um codificador dinâmico padrão) ou em 1080p (se solicitando uma predefinição para um codificador ao vivo de Premium1080p) e no máximo de 6 camadas.

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
> Para uma URL de ingestão para ser preditiva, defina o modo de "personalizado".

* URL não intuitiva

    A URL não intuitiva é o modo padrão no AMS v3. Você possivelmente obterá o Evento ao vivo rapidamente, mas a URL de ingestão é conhecida apenas ao iniciar o evento ao vivo. A URL será alterada se você parar/iniciar o Evento ao vivo. <br/>A URL não intuitiva é útil em cenários em que um usuário final deseja fazer uma transmissão usando um aplicativo que deseja obter um evento ao vivo o mais rápido possível e ter uma URL de ingestão dinâmica não é um problema.
* URL intuitiva

    A URL intuitiva é preferida pelas grandes emissoras de mídia, que usam codificadores de transmissão de hardware e não querem reconfigurar seus codificadores ao iniciar o Evento ao vivo. Elas querem uma URL de ingestão preditiva, que não muda com o tempo.
    
    Para especificar esse modo, defina `vanityUrl` à `true` no momento da criação (o padrão é `false`). Você também precisará passar seu próprio token de acesso (`LiveEventInput.accessToken`) no momento da criação. Você especificar o valor do token para evitar um token aleatório na URL. O token de acesso deve ser uma cadeia de caracteres GUID válida (com ou sem os traços). Depois que o modo é definido, ele não pode ser atualizado.

    O token de acesso precisa ser exclusivo em seu data center. Se seu aplicativo precisar usar uma URL intuitivo, é recomendável sempre criar uma nova instância GUID para seu token de acesso (em vez de reutilizar qualquer GUID existente). 

    Use as seguintes APIs para habilitar a URL intuitivo e defina o token de acesso como um GUID válido (por exemplo `"accessToken": "1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`):
    
    |Linguagem|Habilitar o URL personalizado|Definir token de acesso|
    |---|---|---|
    |REST|[properties.vanityUrl](https://docs.microsoft.com/rest/api/media/liveevents/create#liveevent)|[LiveEventInput.accessToken](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventinput)|
    |CLI|[--vanity-url](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#az-ams-live-event-create)|[--access-token](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#optional-parameters)|
    |.NET|[LiveEvent.VanityUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent.vanityurl?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEvent_VanityUrl)|[LiveEventInput.AccessToken](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveeventinput.accesstoken?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEventInput_AccessToken)|
    
### <a name="live-ingest-url-naming-rules"></a>Regras de nomenclatura de URL de ingestão dinâmica

A cadeia de caracteres *aleatória* abaixo é um número hexadecimal de 128 bits composto de 32 caracteres de “0” a “9” e “a” a “f”.<br/>
O *token de acesso* é o que você precisa especificar para URL fixa. Você deve definir uma cadeia de token de acesso que é uma cadeia de caracteres do GUID de comprimento válido. <br/>
O *nome do fluxo* indica o nome do fluxo para uma conexão específica. O valor do nome de fluxo geralmente é adicionado pelo codificador ao vivo que você use.

#### <a name="non-vanity-url"></a>URL não intuitiva

##### <a name="rtmp"></a>RTMP

`rtmp://<random 128bit hex string>.channel.media.azure.net:1935/live/<access token>/<stream name>`<br/>
`rtmp://<random 128bit hex string>.channel.media.azure.net:1936/live/<access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2935/live/<access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2936/live/<access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Smooth Streaming

`http://<random 128bit hex string>.channel.media.azure.net/<access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<random 128bit hex string>.channel.media.azure.net/<access token>/ingest.isml/streams(<stream name>)`<br/>

#### <a name="vanity-url"></a>URL intuitiva

##### <a name="rtmp"></a>RTMP

`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1935/live/<access token>/<stream name>`<br/>
`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1936/live/<access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2935/live/<access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2936/live/<access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Smooth Streaming

`http://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<access token>/ingest.isml/streams(<stream name>)`<br/>

## <a name="live-event-preview-url"></a>URL de visualização do Evento ao vivo

Quando o **Evento ao vivo** começar a receber o feed de contribuição, você pode usar o ponto de extremidade de visualização para visualizar e validar que está recebendo a transmissão ao vivo antes de publicar. Depois de verificar se o fluxo de visualização é bom, você pode usar o Evento ao vivo para disponibilizar a transmissão ao vivo para entrega por meio de um ou mais **Pontos de extremidade de streaming** (pré-criados). Para conseguir isso, crie uma nova [Saída ao vivo](https://docs.microsoft.com/rest/api/media/liveoutputs) no **Evento ao vivo**. 

> [!IMPORTANT]
> Certifique-se de que o vídeo está fluindo para a URL de visualização antes de continuar!

## <a name="live-event-long-running-operations"></a>Operações de longa duração de evento ao vivo

Para obter detalhes, consulte [operações de longa execução](media-services-apis-overview.md#long-running-operations)

## <a name="live-outputs"></a>Saídas ao Vivo

Uma vez que o fluxo esteja fluindo para o Evento ao vivo, é possível começar o evento de transmissão criando um [Ativo](https://docs.microsoft.com/rest/api/media/assets), uma [Saída ao vivo](https://docs.microsoft.com/rest/api/media/liveoutputs) e um [Localizador de streaming](https://docs.microsoft.com/rest/api/media/streaminglocators). A Saída ao vivo arquiva o fluxo e o disponibiliza para usuários por meio do [Ponto de extremidade de streaming](https://docs.microsoft.com/rest/api/media/streamingendpoints).  

> [!NOTE]
> As Saídas ao Vivo começam na criação e terminam quando são excluídas. Ao excluir a Saída ao Vivo, você não está excluindo o Ativo subjacente nem o conteúdo no ativo. 

A relação entre um **Evento ao Vivo** e a respectiva **Saída ao vivo** é semelhante à difusão de televisão tradicional, em que um canal (**Evento ao Vivo**) representa uma transmissão constante de vídeo e uma gravação (**Saída ao vivo**) tem o escopo definido para um segmento de tempo específico (por exemplo, notícias noturnas das 18h30 às 19h00). Você pode gravar televisão usando um DVR (Gravador de Vídeo Digital) – o recurso equivalente no Evento ao Vivo é gerenciado por meio da propriedade **ArchiveWindowLength**. É uma duração de tempo ISO-8601 (por exemplo, PTHH: MM: SS), que especifica a capacidade do DVR e pode ser definida de um mínimo de 3 minutos a um máximo de 25 horas.

O objeto **Saída ao vivo** é como um gravador que captura e grava a transmissão ao vivo em um Ativo em sua conta dos Serviços de Mídia. O conteúdo gravado será mantido na conta de Armazenamento do Azure anexada à sua conta, no contêiner definido pelo recurso Ativo. Uma **Saída ao vivo** também permite que você controle algumas propriedades da transmissão ao vivo, como quanto do fluxo é mantido na gravação de arquivo (por exemplo, a capacidade do DVR na nuvem) e se os espectadores podem ou não assistir à exibição da transmissão ao vivo. O arquivo no disco é uma "janela" de arquivo circular que contém apenas a quantidade de conteúdo especificada na propriedade **archiveWindowLength** da **Saída ao vivo**. O conteúdo que fica fora dessa janela é automaticamente descartado do contêiner de armazenamento e não é recuperável. Você pode criar várias **Saídas ao vivo** (até três no máximo) em um **Evento ao vivo** com diferentes comprimentos e configurações de arquivamento.  

Caso tenha publicado o **Ativo** da **Saída ao vivo** usando um **Localizador de streaming**, o **Evento ao vivo** (até a duração da janela de DVR) continuará visível até a expiração ou exclusão do Localizador de streaming, o que ocorrer primeiro.

Para saber mais, confira [Usar o DVR na nuvem](live-event-cloud-dvr.md).

## <a name="ask-questions-give-feedback-get-updates"></a>Fazer perguntas, comentar, obter atualizações

Confira o artigo [comunidade dos Serviços de Mídia do Azure](media-services-community.md) para ver diferentes maneiras de fazer perguntas, comentários e obter atualizações sobre os serviços de mídia.

## <a name="next-steps"></a>Próximas etapas

[Tutorial de live streaming](stream-live-tutorial-with-api.md)
