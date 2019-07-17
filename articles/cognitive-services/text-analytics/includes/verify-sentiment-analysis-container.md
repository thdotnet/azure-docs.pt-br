---
title: Verifique se a instância de contêiner de análise de sentimento
titleSuffix: Azure Cognitive Services
description: Saiba como verificar a instância de contêiner de análise de sentimento.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: f69d573e9e70a505018e94cca354f363097cc1b8
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68229271"
---
## <a name="verify-the-sentiment-analysis-container-instance"></a>Verifique se a instância de contêiner de análise de sentimento

1. Selecione o **visão geral** guia e copie o endereço IP.
1. Abra uma nova guia do navegador e digite o endereço IP. Por exemplo, insira `http://<IP-address>:5000 (http://55.55.55.55:5000`). Home page do contêiner é exibida, permitindo que você saiba o contêiner está em execução.

    ![Exibir a home page do contêiner para verificar se ele está em execução](../media/how-tos/container-instance/swagger-docs-on-container.png).

1. Selecione o **descrição do serviço API** link para ir para página do swagger do contêiner.

1. Escolher qualquer uma da **POST** APIs e selecione **experimentá-lo**.  Os parâmetros são exibidos, incluindo essa entrada de exemplo:

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "1",
          "text": "Hello world. This is some input text that I love."
        },
        {
          "language": "fr",
          "id": "2",
          "text": "Bonjour tout le monde"
        },
        {
          "language": "es",
          "id": "3",
          "text": "La carretera estaba atascada. Había mucho tráfico el día de ayer."
        }
      ]
    }
    ```

1. Substitua a entrada com o seguinte conteúdo JSON:

    ```json
    {
      "documents": [
        {
          "language": "en",
          "id": "7",
          "text": "I was fortunate to attend the KubeCon Conference in Barcelona, it is one of the best conferences I have ever attended. Great people, great sessions and I thoroughly enjoyed it!"
        }
      ]
    }
    ```

1. Definir **showStats** como true.

1. Selecione **Execute** para determinar o sentimento do texto.

    O modelo que é empacotado em um contêiner gera uma pontuação entre 0 e 1, onde 0 é negativo e 1 é positivo.

    A resposta JSON retornada inclui o sentimento de entrada de texto atualizado:

    ```json
    {
      "documents": [
      {
        "id": "7",
        "score": 0.9826303720474243,
        "statistics": {
          "charactersCount": 176,
            "transactionsCount": 1
          }
        }
      ],
      "errors": [],
      "statistics": {
        "documentsCount": 1,
        "validDocumentsCount": 1,
        "erroneousDocumentsCount": 0,
        "transactionsCount": 1
      }
    }
    ```

O documento agora pode correlacionar `id` de dados do JSON do conteúdo da resposta no documento original de carga de solicitação `id`. Vemos uma pontuação de mais de `.98`, que indica um sentimento positivo fortemente.