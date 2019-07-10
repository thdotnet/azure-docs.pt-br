---
title: Instalar os contêineres de fala
titleSuffix: Azure Cognitive Services
description: Fornece detalhes sobre as opções de configuração fala guarda-chuva helm gráfico.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: ed64412ccf9d192506fafe546b1ccee7941aa43a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711808"
---
### <a name="speech-umbrella-chart"></a>Conversão de fala (gráfico de guarda-chuva)

Valores no gráfico de nível superior "abrangente" substituem os valores correspondentes do gráfico de subpropriedades. Portanto, todos os valores de locais personalizados devem ser adicionados aqui.

|Parâmetro|DESCRIÇÃO|Padrão|
| -- | -- | -- | -- |
| `speechToText.enabled` | Se o **fala em texto** serviço está habilitado. | `true` |
| `speechToText.verification.enabled` | Se o `helm test` funcionalidade para **fala em texto** serviço está habilitado. | `true` |
| `speechToText.verification.image.registry` | O repositório de imagens do docker que `helm test` usa para testar **fala em texto** service. Helm cria um pod separado dentro do cluster para teste e extrai o *uso de testes* imagem a partir deste registro. | `docker.io` |
| `speechToText.verification.image.repository` | O repositório de imagens do docker que `helm test` usa para testar **fala em texto** service. Pod de teste do Helm usa esse repositório para efetuar pull *uso de testes* imagem. | `antsu/on-prem-client` |
| `speechToText.verification.image.tag` | A marca de imagem do docker usada com `helm test` para **fala em texto** service. Essa marca usa o pod de teste do Helm para efetuar pull *uso de testes* imagem. | `latest` |
| `speechToText.verification.image.pullByHash` | Se o *uso de testes* imagem do docker é obtida por hash. Se `true`, `speechToText.verification.image.hash` devem ser adicionados, com valor de hash de imagem válido. | `false` |
| `speechToText.verification.image.arguments` | Os argumentos usados para executar o *uso de testes* imagem do docker. Pod de teste do Helm passa esses argumentos para o contêiner ao executar `helm test`. | `"./speech-to-text-client"`<br/> `"./audio/whatstheweatherlike.wav"` <br/> `"--expect=What's the weather like"`<br/>`"--host=$(SPEECH_TO_TEXT_HOST)"`<br/>`"--port=$(SPEECH_TO_TEXT_PORT)"` |
| `textToSpeech.enabled` | Se o **fala** serviço está habilitado. | `true` |
| `textToSpeech.verification.enabled` | Se o `helm test` funcionalidade para **fala em texto** serviço está habilitado. | `true` |
| `textToSpeech.verification.image.registry` | O repositório de imagens do docker que `helm test` usa para testar **fala em texto** service. Helm cria um pod separado dentro do cluster para teste e extrai o *uso de testes* imagem a partir deste registro. | `docker.io` |
| `textToSpeech.verification.image.repository` | O repositório de imagens do docker que `helm test` usa para testar **fala em texto** service. Pod de teste do Helm usa esse repositório para efetuar pull *uso de testes* imagem. | `antsu/on-prem-client` |
| `textToSpeech.verification.image.tag` | A marca de imagem do docker usada com `helm test` para **fala em texto** service. Essa marca usa o pod de teste do Helm para efetuar pull *uso de testes* imagem. | `latest` |
| `textToSpeech.verification.image.pullByHash` | Se o *uso de testes* imagem do docker é obtida por hash. Se `true`, `textToSpeech.verification.image.hash` devem ser adicionados, com valor de hash de imagem válido. | `false` |
| `textToSpeech.verification.image.arguments` | Os argumentos a serem executados com o *uso de testes* imagem do docker. O pod de teste do helm passa esses argumentos para o contêiner ao executar `helm test`. | `"./text-to-speech-client"`<br/> `"--input='What's the weather like'"` <br/> `"--host=$(TEXT_TO_SPEECH_HOST)"`<br/>`"--port=$(TEXT_TO_SPEECH_PORT)"` |