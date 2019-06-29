---
title: Suporte a contêiner
titleSuffix: Azure Cognitive Services
description: Saiba como verificar a instância de contêiner de análise de sentimento.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: f68d9c7098f2b1ca782e2522c632c2e267b35336
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455106"
---
## <a name="verify-the-sentiment-analysis-container-instance"></a>Verifique se a instância de contêiner de análise de sentimento

1. Selecione o **visão geral** guia e copie o endereço IP.
1. Abra uma nova guia do navegador e usar o endereço IP, por exemplo, `http://<IP-address>:5000 (http://55.55.55.55:5000`). Página de início do contêiner é apresentada, permitindo que você saiba o contêiner está em execução.

    ![Exibir a home page do contêiner para verificar se que ele está em execução](../media/how-tos/container-instance/swagger-docs-on-container.png)

1. Selecione o **descrição do serviço API** link para navegar até a página do swagger de contêineres.

1. Escolher qualquer uma da **POST** APIs e selecione **experimentá-lo**.  Os parâmetros são exibidos, incluindo a entrada de exemplo:

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

1. Substitua a entrada com o JSON a seguir:

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

    O modelo empacotado no contêiner gera uma pontuação entre 0 e 1, onde 0 é negativo e 1 é positivo.

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

O documento agora pode correlacionar `id` das cargas de resposta JSON no documento original de carga de solicitação `id`e ver que houve uma pontuação de sobre `.98` que indica um sentimento muito positivo.