---
title: Instalar os contêineres de fala
titleSuffix: Azure Cognitive Services
description: Fornece detalhes sobre as opções de configuração de gráfico do helm texto em fala.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: e6c7dcd3015b0b8ab5b3c719ebd2397bc814b81a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717222"
---
### <a name="text-to-speech-sub-chart-chartstexttospeech"></a>Texto em fala (gráfico de subpropriedades: gráficos/textToSpeech)

Para substituir o gráfico "abrangente", adicione o prefixo `textToSpeech.` em qualquer parâmetro para torná-lo mais específico. Por exemplo, ele substituirá o parâmetro correspondente, por exemplo, `textToSpeech.numberOfConcurrentRequest` substituições `numberOfConcurrentRequest`.

|Parâmetro|DESCRIÇÃO|Padrão|
| -- | -- | -- |
| `enabled` | Se o **fala** serviço está habilitado. | `false` |
| `numberOfConcurrentRequest` | O número de solicitações simultâneas para o **fala** service. Este gráfico calcula automaticamente os recursos de CPU e memória, com base nesse valor. | `2` |
| `optimizeForTurboMode`| Se o serviço precisa otimizar o texto de entrada por meio de arquivos de texto. Se `true`, este gráfico será alocar mais recursos de CPU para o serviço. | `false` |
| `image.registry`| O **fala** registro de imagem do docker. | `containerpreview.azurecr.io` |
| `image.repository` | O **fala** repositório de imagens do docker. | `microsoft/cognitive-services-text-to-speech` |
| `image.tag` | O **fala** marca de imagem do docker. | `latest` |
| `image.pullSecrets` | Os segredos de imagem para efetuar pull de **fala** imagem do docker. | |
| `image.pullByHash`| Se a imagem do docker é obtida por hash. Se `true`, `image.hash` é necessária. | `false` |
| `image.hash`| O **fala** hash de imagem do docker. Somente usado ao `image.pullByHash: true`.  | |
| `image.args.eula` (obrigatório) | Indica se que você aceitar a licença. É o único valor válido `accept` | |
| `image.args.billing` (obrigatório) | O valor do URI de ponto de extremidade cobrança está disponível na página de visão geral de fala do portal do Azure. | |
| `image.args.apikey` (obrigatório) | Usado para rastrear informações de cobrança. ||
| `service.type` | Os Kubernetes do tipo de serviço a **fala** service. Consulte a [instruções de tipos de serviço de Kubernetes](https://kubernetes.io/docs/concepts/services-networking/service/) para obter mais detalhes e verifique se o suporte do provedor de nuvem. | `LoadBalancer` |
| `service.port`|  A porta a **fala** service. | `80` |
| `service.autoScaler.enabled` | Se o [dimensionador automático de Pod Horizontal](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) está habilitado. Se `true`, o `text-to-speech-autoscaler` será implantado no cluster Kubernetes. | `true` |
| `service.podDisruption.enabled` | Se o [Pod interrupção orçamento](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) está habilitado. Se `true`, o `text-to-speech-poddisruptionbudget` será implantado no cluster Kubernetes. | `true` |