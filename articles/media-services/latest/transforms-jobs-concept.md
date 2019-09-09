---
title: Transformações e Trabalhos nos Serviços de Mídia do Azure | Microsoft Docs
description: Ao usar os Serviços de Mídia, é necessário criar um recurso de Transformação para descrever as regras ou especificações para processar seus vídeos. Este artigo fornece uma visão geral do recurso de Transformação e como usá-lo.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 08/19/2019
ms.author: juliako
ms.openlocfilehash: 466ab0737aa5af40bd1bc137b98ab57a48feafde
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69637347"
---
# <a name="transforms-and-jobs"></a>Transformações e Trabalhos

Este tópico fornece detalhes sobre [transformações](https://docs.microsoft.com/rest/api/media/transforms) e [trabalhos](https://docs.microsoft.com/rest/api/media/jobs) e explica a relação entre essas entidades. 

## <a name="overview"></a>Visão geral 

### <a name="transformsjobs-workflow"></a>Fluxo de trabalho de transformações/trabalhos

O diagrama a seguir mostra o fluxo de trabalho/transformações.

![Transformações](./media/encoding/transforms-jobs.png)

#### <a name="typical-workflow"></a>Fluxo de trabalho típico

1. Criar uma Transformação 
2. Enviar Trabalhos sob essa Transformação 
3. Listar Transformações 
4. Exclua uma transformação, se você não planeja usá-la no futuro. 

#### <a name="example"></a>Exemplo

Suponha que você deseja extrair o primeiro quadro de todos os seus vídeos como uma imagem em miniatura. Estas são as etapas necessárias: 

1. Defina a receita ou a regra para processar seus vídeos – "usar o primeiro quadro do vídeo como a miniatura". 
2. Para cada vídeo informar ao serviço: 
    1. Onde encontrar esse vídeo,  
    2. Onde gravar a saída a imagem em miniatura de saída. 

Uma **Transformação** ajuda você a criar uma vez a receita (Etapa 1) e enviar trabalhos usando essa receita (Etapa 2).

> [!NOTE]
> Propriedades de **Transformação** e **Trabalho** que são do tipo Datetime estão sempre em formato UTC.

## <a name="transforms"></a>Transformações

Use **Transformações** para configurar tarefas comuns para codificar ou analisar vídeos. Cada **Transformação** descreve uma receita ou um fluxo de trabalho de tarefas para processar os arquivos de áudio ou vídeos. Uma única transformação pode aplicar mais de uma regra. Por exemplo, uma transformação poderia especificar que cada vídeo seja codificado em um arquivo MP4 em uma determinada taxa de bits, e que uma imagem em miniatura gerada desde o primeiro quadro do vídeo. Você precisaria adicionar uma entrada de TransformOutput para cada regra que você deseja incluir em sua Transformação. Você usa predefinições para informar à transformação como os arquivos de mídia de entrada devem ser processados.

### <a name="viewing-schema"></a>Exibindo esquema

Nos serviços de mídia v3, as predefinições são entidades fortemente tipadas na própria API. Você pode encontrar a definição de "esquema" para esses objetos em [especificação de API aberta (ou Swagger)](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01). Você também pode exibir as definições predefinidas (como **StandardEncoderPreset**) na [API REST](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset), no [SDK do .net](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.standardencoderpreset?view=azure-dotnet) (ou em outra documentação de referência do SDK dos serviços de mídia v3).

### <a name="creating-transforms"></a>Criando transformações

Você pode criar transformações usando REST, CLI ou usar qualquer um dos SDKs publicados. A API é controlada pelo Azure Resource Manager v3, portanto, você também pode usar modelos do Azure Resource Manager para criar Transformações na sua conta dos Serviços de Mídia do Microsoft Azure. O controle de acesso baseado em função pode ser usado para bloquear o acesso a transformações.

### <a name="updating-transforms"></a>Atualizando transformações

Se você precisar atualizar sua [transformação](https://docs.microsoft.com/rest/api/media/transforms), use a operação de **atualização** . Ele se destina a fazer alterações na descrição ou nas prioridades do TransformOutputs subjacente. É recomendável que essas atualizações sejam realizadas depois que todos os trabalhos em andamento forem concluídos. Se você pretende reescrever a receita, precisa criar uma nova transformação.

### <a name="transform-object-diagram"></a>Diagrama de objeto de transformação

O diagrama a seguir mostra o objeto **Transform** e os objetos que ele faz referência, incluindo as relações de derivação. As setas cinzas mostram um tipo que o trabalho referencia e as setas verdes mostram relações de derivação de classe.<br/>Clique na imagem para exibi-la em tamanho normal.  

<a href="./media/api-diagrams/transform-large.png" target="_blank"><img src="./media/api-diagrams/transform-small.png"></a> 

## <a name="jobs"></a>Tarefas (Jobs)

Um **Trabalho** é a solicitação real para os serviços de mídia do Azure para aplicar a **Transformação** a um determinado conteúdo de áudio ou vídeo de entrada. Quando a Transformação for criada, você poderá enviar trabalhos usando as APIs dos Serviços de Mídia ou um dos SDKs publicados. O **Trabalho** especifica informações como o local do vídeo de entrada e o local para a saída. É possível especificar a localização do vídeo de entrada usando: URLs HTTPS, URLs SAS ou [Ativos](https://docs.microsoft.com/rest/api/media/assets).  

### <a name="job-input-from-https"></a>Entrada de trabalho do HTTPS

Use a [entrada de trabalho do HTTPS](job-input-from-http-how-to.md) se seu conteúdo já estiver acessível por meio de uma URL e você não precisar armazenar o arquivo de origem no Azure (por exemplo, importar do S3). Esse método também é adequado se você tiver o conteúdo no armazenamento de BLOBs do Azure, mas não precisar que o arquivo esteja em um ativo. Atualmente, esse método dá suporte apenas a um único arquivo para entrada.

### <a name="asset-as-job-input"></a>Ativo como entrada do trabalho

Use o [ativo como entrada de trabalho](job-input-from-local-file-how-to.md) se o conteúdo de entrada já estiver em um ativo ou se o conteúdo estiver armazenado no arquivo local. Também é uma boa opção se você planeja publicar o ativo de entrada para streaming ou download (digamos que você deseja publicar o MP4 para download, mas também deseja fazer fala para detecção de texto ou facial). Esse método dá suporte a ativos de vários arquivos (por exemplo, conjuntos de streaming MBR que foram codificados localmente).

### <a name="checking-job-progress"></a>Verificando o andamento do trabalho

O progresso e o estado de trabalhos podem ser obtidos pelo monitoramento de eventos com a Grade de Eventos do Azure. Para obter mais informações, consulte [Monitorar eventos usando EventGrid](job-state-events-cli-how-to.md).

### <a name="updating-jobs"></a>Atualizando trabalhos

A operação de atualização na entidade [Trabalho](https://docs.microsoft.com/rest/api/media/jobs) pode ser usada para modificar as propriedades *description* e *priority* depois que o trabalho é enviado. Uma alteração na propriedade *priority* só será eficaz se o trabalho ainda estiver na fila. Se o trabalho tiver iniciado o processamento ou tiver sido concluído, a alteração da prioridade não terá qualquer efeito.

### <a name="job-object-diagram"></a>Diagrama de objeto de trabalho

O diagrama a seguir mostra o objeto de **trabalho** e os objetos que ele faz referência, incluindo as relações de derivação.<br/>Clique na imagem para exibi-la em tamanho normal.  

<a href="./media/api-diagrams/job-large.png" target="_blank"><img src="./media/api-diagrams/job-small.png"></a> 

## <a name="configure-media-reserved-units"></a>Configurar unidades reservadas de mídia

Para os trabalhos de análise de áudio e análise de vídeo acionados pelo Serviços de Mídia do Microsoft Azure v3 ou pelo Video Indexer, é altamente recomendável provisionar sua conta com 10 MRUs (Unidades reservadas de mídia) do S3. Se você precisar de mais de 10 MRUs do S3, abra um ticket de suporte usando o [Portal do Microsoft Azure](https://portal.azure.com/).

Para obter detalhes, confira [Dimensionar o processamento de mídia com a CLI](media-reserved-units-cli-how-to.md).

## <a name="ask-questions-give-feedback-get-updates"></a>Fazer perguntas, comentar, obter atualizações

Confira o artigo [comunidade dos Serviços de Mídia do Azure](media-services-community.md) para ver diferentes maneiras de fazer perguntas, comentários e obter atualizações sobre os serviços de mídia.

## <a name="see-also"></a>Consulte também

* [Códigos de erro](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)
* [Filtragem, ordenação, paginação de entidades de serviços de mídia](entities-overview.md)

## <a name="next-steps"></a>Próximas etapas

- Antes de começar a desenvolver, examine o [desenvolvimento com as APIs dos serviços de mídia v3](media-services-apis-overview.md) (inclui informações sobre como acessar APIs, convenções de nomenclatura, etc.)
- Confira estes tutoriais:

    - [Tutorial: Codificar um arquivo remoto com base na URL e transmitir o vídeo](stream-files-tutorial-with-rest.md)
    - [Tutorial: Carregar, codificar e transmitir vídeos](stream-files-tutorial-with-api.md)
    - [Tutorial: Analisar vídeos com os serviços de mídia v3](analyze-videos-tutorial-with-api.md)
