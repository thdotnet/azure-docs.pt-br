---
title: Proteger seu conteúdo usando a criptografia dinâmica dos serviços de mídia – Azure | Microsoft Docs
description: Este artigo fornece uma visão geral da proteção de conteúdo com criptografia dinâmica. Ele também fala sobre protocolos de streaming e tipos de criptografia.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 5d31e4a523fdedf9907e33c70638f07a08461ed1
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68310312"
---
# <a name="content-protection-with-dynamic-encryption"></a>Proteção de conteúdo com criptografia dinâmica

É possível usar os Serviços de Mídia do Azure para proteger sua mídia desde o momento que ela sai do seu computador até o armazenamento, processamento e entrega. Com os Serviços de Mídia, é possível entregar o conteúdo ao vivo e sob demanda criptografado dinamicamente com a criptografia AES (AES-128) ou qualquer um dos três principais sistemas DRM (gerenciamento de direitos digitais): Microsoft PlayReady, Google Widevine e Apple FairPlay. Os serviços de mídia também fornecem um serviço de distribuição de chaves AES e licenças DRM (PlayReady, Widevine e FairPlay) para os clientes autorizados. 

No Media Services V3, uma chave de conteúdo é associada ao localizador de streaming (consulte [Este exemplo](protect-with-aes128.md)). Se estiver usando o serviço de distribuição de chaves dos serviços de mídia, você poderá permitir que os serviços de mídia do Azure gerem a chave de conteúdo para você. Você mesmo deve gerar a chave de conteúdo se estiver usando o serviço de distribuição de chaves próprio, ou se precisar lidar com um cenário de alta disponibilidade em que você precisa ter a mesma chave de conteúdo em dois data centers.

Quando um fluxo é solicitado por um player, os Serviços de Mídia usam a chave especificada para criptografar dinamicamente o conteúdo usando a chave não criptografada AES ou a criptografia DRM. Para descriptografar o fluxo, o player solicita a chave do serviço de entrega de chave dos Serviços de Mídia ou do serviço de entrega de chaves especificado. Para decidir se o usuário está autorizado ou não a obter a chave, o serviço avalia a política de chave de conteúdo que você especificou para a chave.

Você pode usar o API REST ou biblioteca de cliente de Serviços de Mídia para configurar políticas de autenticação e autorização para suas licenças e chaves.

A seguinte imagem ilustra o fluxo de trabalho de proteção de conteúdo dos Serviços de Mídia: 

![Proteger conteúdo](./media/content-protection/content-protection.svg)

&#42;*chave AES-128"Limpar", CBCS e CENC dá suporte a criptografia dinâmica. Para obter detalhes, consulte a matriz de suporte [aqui](#streaming-protocols-and-encryption-types).*

Este artigo explica conceitos e terminologia relevantes para a compreensão da proteção de conteúdo com os Serviços de Mídia.

## <a name="main-components-of-a-content-protection-system"></a>Componentes principais de um sistema de proteção de conteúdo

Para concluir com êxito o design do sistema / aplicativo de "proteção de conteúdo", você precisa entender completamente o escopo do esforço. A lista a seguir fornece uma visão geral de três partes, que você precisaria implementar. 

1. Código de Serviços de Mídia do Azure
  
   O exemplo de [DRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs) mostra como implementar o sistema de vários DRM com os serviços de mídia v3 usando o .net. Ele também mostra como usar a licença dos serviços de mídia/serviço de entrega de chave. Você pode criptografar cada ativo com vários tipos de criptografia (AES-128, PlayReady, Widevine, FairPlay). Consulte [Tipos de criptografia e protocolos de streaming](#streaming-protocols-and-encryption-types) para ver o que faz sentido combinar.
  
   O exemplo mostra como:

   1. Criar e configurar [políticas de chave de conteúdo](content-key-policy-concept.md). Você cria uma **política de chave de conteúdo** para configurar como a chave de conteúdo (que fornece acesso seguro aos seus ativos) é entregue aos clientes finais.    

      * Defina a autorização de entrega de licença, especificando a lógica de verificação de autorização com base em declarações no JWT.
      * Configure as licenças [PlayReady](playready-license-template-overview.md), [Widevine](widevine-license-template-overview.md)e/ou [Fairplay](fairplay-license-overview.md) . Os modelos permitem que você configure direitos e permissões para cada um dos DRMs usados.

        ```
        ContentKeyPolicyPlayReadyConfiguration playReadyConfig = ConfigurePlayReadyLicenseTemplate();
        ContentKeyPolicyWidevineConfiguration widevineConfig = ConfigureWidevineLicenseTempate();
        ContentKeyPolicyFairPlayConfiguration fairPlayConfig = ConfigureFairPlayPolicyOptions();
        ```
   2. Crie um [localizador de streaming](streaming-locators-concept.md) configurado para transmitir o ativo criptografado. 
  
      O **localizador de streaming** deve ser associado a uma [política de streaming](streaming-policy-concept.md). No exemplo, definimos StreamingLocator. StreamingPolicyName como a política "Predefined_MultiDrmCencStreaming". As criptografias PlayReady e Widevine são aplicadas, a chave é entregue ao cliente de reprodução com base nas licenças DRM configuradas. Se você também quiser criptografar seu fluxo com CBCS (FairPlay), use “Predefined_MultiDrmStreaming”.
      
      O localizador de streaming também está associado à **política de chave de conteúdo** que foi definida.
    
   3. Crie um token de teste.

      O método **GetTokenAsync** mostra como criar um teste de token.
   4. Crie a URL de streaming.

      O método **GetDASHStreamingUrlAsync** mostra como construir a URL de streaming. Nesse caso, os fluxos de URL o conteúdo **DASH**.

2. Player com o cliente do AES ou DRM. Um aplicativo de player de vídeo baseado em um player SDK (nativo ou baseado em navegador) precisa atender aos seguintes requisitos:
   * O SDK player dá suporte a clientes necessários do DRM
   * O SDK de player suporta os protocolos de streaming necessários: Smooth, DASH e/ou HLS
   * O SDK do player precisa ser capaz de lidar com passar um token JWT na solicitação de aquisição de licença
  
     Você consegue criar um player usando a [API do Player de Mídia do Azure](https://amp.azure.net/libs/amp/latest/docs/). Use a [API ProtectionInfo do Player de Mídia do Azure](https://amp.azure.net/libs/amp/latest/docs/) para especificar qual tecnologia de DRM usar em diferentes plataformas de DRM.

     Para testar conteúdo criptografado AES ou CENC (Widevine e/ou PlayReady), é possível usar o [Player de Mídia do Azure](https://aka.ms/azuremediaplayer). Certifique-se de clicar em "Opções avançadas" e verifique as opções de criptografia.

     Se você quiser testar o conteúdo do FairPlay criptografado, use [esse teste player](https://aka.ms/amtest). O player suporta DRM Widevine, PlayReady e FairPlay, bem como a criptografia de chave AES-128. 
    
     Você precisa escolher o navegador correto para testar DRMs diferentes: Chrome/Opera/Firefox para Widevine, Microsoft Edge/IE11 para PlayReady, Safari no macOS para FairPlay.

3. O Secure Token Service (STS), que emite o JSON Web Token (JWT) como token de acesso para acesso a recursos de back-end. Você pode usar os serviços de entrega de licença do AMS como o recurso de back-end. Um STS deve definir o seguinte:

   * Emissor e público-alvo (ou escopo)
   * Declarações, que são dependentes nos requisitos de negócios na proteção de conteúdo
   * Verificação simétrica ou assimétrica para verificação de assinatura
   * Suporte de substituição de chave (se necessário)

     É possível usar [esta ferramenta de STS](https://openidconnectweb.azurewebsites.net/DRMTool/Jwt) para testar o STS, que dá suporte a todos os 3 tipos de chave de verificação: simétrica, assimétrica ou Azure AD com substituição de chave. 

> [!NOTE]
> É altamente recomendável focar e testar completamente cada parte (descrita acima) antes de passar para a próxima peça. Para testar seu sistema de "proteção de conteúdo", use as ferramentas especificadas na lista acima.  

## <a name="streaming-protocols-and-encryption-types"></a>Tipos de criptografia e protocolos de streaming

É possível usar os Serviços de Mídia para entregar conteúdo criptografado dinamicamente com chave não criptografada AES ou criptografia DRM usando o PlayReady, o Widevine ou o FairPlay. No momento, você pode criptografar os formatos HLS (HTTP Live Streaming), MPEG DASH e Smooth Streaming. Cada protocolo dá suporte a métodos de criptografia a seguir:

### <a name="hls"></a>HLS

O protocolo HLS dá suporte aos seguintes formatos de contêiner e esquemas de criptografia.

|Formato de contêiner|Esquema de criptografia|Exemplo de URL|
|---|---|---|
|Todos|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cbc)`|
|MPG2-TS |CBCS (FairPlay) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cbcs-aapl)`|
|CMAF(fmp4) |CBCS (FairPlay) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-cmaf,encryption=cbcs-aapl)`|
|MPG2-TS |CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cenc)`|
|CMAF(fmp4) |CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-cmaf,encryption=cenc)`|

HLS/CMAF + FairPlay (incluindo HEVC/H. 265) tem suporte nos seguintes dispositivos:

* iOS v11 ou superior 
* iPhone 8 ou superior
* MacOS High Sierra com a CPU Intel 7ª Gen

### <a name="mpeg-dash"></a>MPEG-DASH

O protocolo MPEG-DASH dá suporte aos seguintes formatos de contêiner e esquemas de criptografia.

|Formato de contêiner|Esquema de criptografia|Exemplos de URL
|---|---|---|
|Todos|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-csf,encryption=cbc)`|
|CSF(fmp4) |CENC (Widevine + PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-csf,encryption=cenc)`|
|CMAF(fmp4)|CENC (Widevine + PlayReady)|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-cmaf,encryption=cenc)`|

### <a name="smooth-streaming"></a>Smooth Streaming

O protocolo Smooth Streaming dá suporte aos seguintes formatos de contêiner e esquemas de criptografia.

|Protocol|Formato de contêiner|Esquema de criptografia|
|---|---|---|
|fMP4|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(encryption=cbc)`|
|fMP4 | CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(encryption=cenc)`|

### <a name="browsers"></a>Navegadores

Os navegadores comuns dão suporte aos seguintes clientes DRM:

|Navegador|Criptografia|
|---|---|
|Chrome|Widevine|
|Microsoft Edge, IE 11|PlayReady|
|Firefox|Widevine|
|Opera|Widevine|
|Safari|FairPlay|

## <a name="control-content-access"></a>Controlar o acesso de conteúdo

É possível controlar quem tem acesso ao seu conteúdo, configurando a política de chave de conteúdo. Os serviços de mídia oferecem suporte a várias maneiras de autorizar os usuários que fazem solicitações de chave. Você deve configurar a política de chave de conteúdo. O cliente (player) deve estar em conformidade com a política antes de a chave poder ser entregue ao cliente. A política de chave de conteúdo pode ter **abra** ou **token** restrição. 

Com uma política de chave de conteúdo restrita por token, a chave de conteúdo somente será enviada a um cliente que tenha um JWT (Token Web JSON) ou SWT (Token Web Simples) válido na solicitação de licença/chave. Esse token deve ser emitido por um STS (serviço de token de segurança). É possível usar o Azure Active Directory como um STS ou implantar um STS personalizado. O STS deve ser configurado para criar um token assinado com as a chave especificada e declarações de emissão que você especificou na configuração de restrição do token. O serviço de entrega de chaves dos Serviços de Mídia retornará a licença/chave solicitada ao cliente se o token for válido e as declarações no token corresponderem àquelas configuradas para a licença/chave.

Ao configurar a política restrita do token, você deve especificar os parâmetros de chave de verificação primária, emissor e público. A chave de verificação primária contém a chave com a qual o token foi assinado. O emissor é o serviço de token seguro que emite o token. O público, às vezes chamado de escopo, descreve a intenção do token ou do recurso ao qual o token autoriza o acesso. O serviço de distribuição de chaves dos serviços de mídia valida que esses valores no token correspondem aos valores no modelo.

Os clientes geralmente usam um STS personalizado para incluir declarações personalizadas no token para selecionar entre diferentes ContentKeyPolicyOptions com parâmetros de licença de DRM diferentes (uma licença de assinatura em vez de uma licença de aluguel) ou para incluir uma declaração representando a chave de conteúdo identificador da chave à qual o token concede acesso.

### <a name="token-replay-prevention"></a>Prevenção de reprodução de token

O recurso de *prevenção de reprodução de token* permite que os clientes dos serviços de mídia definam um limite de quantas vezes o mesmo token pode ser usado para solicitar uma chave ou uma licença. O cliente pode adicionar uma declaração do tipo `urn:microsoft:azure:mediaservices:maxuses` no token, em que o valor é o número de vezes que o token pode ser usado para adquirir uma licença ou chave. Todas as solicitações subsequentes com o mesmo token para a distribuição de chaves retornarão uma resposta não autorizada. Consulte Como adicionar a declaração no exemplo de [DRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L601).
 
#### <a name="considerations"></a>Considerações

* Os clientes devem ter controle sobre a geração de tokens. A declaração precisa ser colocada no próprio token.
* Ao usar esse recurso, as solicitações com tokens cuja hora de expiração é mais de uma hora longe da hora em que a solicitação é recebida são rejeitadas com uma resposta não autorizada.
* Os tokens são identificados exclusivamente por sua assinatura. Qualquer alteração na carga (por exemplo, atualizar para a hora de expiração ou a declaração) altera a assinatura do token e ela conta como um novo token que a entrega de chave não foi encontrada antes.
* A reprodução falhará se o token exceder `maxuses` o valor definido pelo cliente.
* Esse recurso pode ser usado para todo o conteúdo protegido existente (somente o token emitido precisa ser alterado).
* Esse recurso funciona com o JWT e o SWT.

## <a name="custom-key-and-license-acquisition-url"></a>URL de aquisição de licença e chave personalizada

Use os modelos a seguir se desejar especificar um serviço de entrega de chave e licença diferente (não serviços de mídia). Os dois campos substituíveis nos modelos existem para que você possa compartilhar sua política de streaming em vários ativos, em vez de criar uma política de streaming por ativo. 

* EnvelopeEncryption. CustomKeyAcquisitionUrlTemplate-template para a URL do serviço personalizado que fornece chaves para os players do usuário final. Não é necessário ao usar os serviços de mídia do Azure para emitir chaves. O modelo oferece suporte a tokens substituíveis que o serviço atualizará em tempo de execução com o valor específico para a solicitação.  Os valores de token com suporte no momento são {AlternativeMediaId}, que é substituído pelo valor de StreamingLocatorId. AlternativeMediaId e {ContentKeyId}, que é substituído pelo valor do identificador da chave que está sendo solicitada.
* StreamingPolicyPlayReadyConfiguration. CustomLicenseAcquisitionUrlTemplate-template para a URL do serviço personalizado que entrega licenças para players do usuário final. Não é necessário ao usar os serviços de mídia do Azure para a emissão de licenças. O modelo oferece suporte a tokens substituíveis que o serviço atualizará em tempo de execução com o valor específico para a solicitação. Os valores de token com suporte no momento são {AlternativeMediaId}, que é substituído pelo valor de StreamingLocatorId. AlternativeMediaId e {ContentKeyId}, que é substituído pelo valor do identificador da chave que está sendo solicitada. 
* StreamingPolicyWidevineConfiguration. CustomLicenseAcquisitionUrlTemplate-o mesmo que acima, somente para Widevine. 
* StreamingPolicyFairPlayConfiguration. CustomLicenseAcquisitionUrlTemplate-o mesmo que acima, somente para FairPlay.  

Por exemplo:

```csharp
streamingPolicy.EnvelopEncryption.customKeyAcquisitionUrlTemplate = "https://mykeyserver.hostname.com/envelopekey/{AlternativeMediaId}/{ContentKeyId}";
```

O `ContentKeyId` tem um valor da chave que está sendo solicitada `AlternativeMediaId` e o pode ser usado se você quiser mapear a solicitação para uma entidade no seu lado. Por exemplo, o `AlternativeMediaId` pode ser usado para ajudá-lo a Pesquisar permissões.

Para obter exemplos de REST que usam URLs de chave personalizada e de aquisição de licença, consulte [políticas de streaming – criar](https://docs.microsoft.com/rest/api/media/streamingpolicies/create)

## <a name="troubleshoot"></a>Solução de problemas

Se você receber o `MPE_ENC_ENCRYPTION_NOT_SET_IN_DELIVERY_POLICY` erro, certifique-se de especificar a política de streaming apropriada.

Se você receber erros que terminam `_NOT_SPECIFIED_IN_URL`com o, certifique-se de especificar o formato de criptografia na URL. Por exemplo, `…/manifest(format=m3u8-cmaf,encryption=cbcs-aapl)`. Consulte [protocolos de streaming e tipos de criptografia](#streaming-protocols-and-encryption-types).

## <a name="ask-questions-give-feedback-get-updates"></a>Fazer perguntas, comentar, obter atualizações

Confira o artigo [comunidade dos Serviços de Mídia do Azure](media-services-community.md) para ver diferentes maneiras de fazer perguntas, comentários e obter atualizações sobre os serviços de mídia.

## <a name="next-steps"></a>Próximas etapas

* [Proteger com criptografia AES](protect-with-aes128.md)
* [Proteger com DRM](protect-with-drm.md)
* [Projetar o sistema de proteção de conteúdo de vários DRM com o controle de acesso](design-multi-drm-system-with-access-control.md)
* [Criptografia do lado do armazenamento](storage-account-concept.md#storage-side-encryption)
* [Perguntas frequentes](frequently-asked-questions.md)

