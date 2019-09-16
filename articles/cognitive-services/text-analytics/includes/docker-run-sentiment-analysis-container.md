---
title: Exemplo de execução de contêiner do comando de execução do Docker
titleSuffix: Azure Cognitive Services
description: Comando de execução do Docker para Análise de Sentimento contêiner
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 09/12/2019
ms.author: dapine
ms.openlocfilehash: f0e587fc39fa2cc6f5275ae16834372a206b37d2
ms.sourcegitcommit: fbea2708aab06c19524583f7fbdf35e73274f657
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2019
ms.locfileid: "70966682"
---
Para executar o contêiner de *análise de sentimento* , execute o `docker run` comando a seguir.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
mcr.microsoft.com/azure-cognitive-services/sentiment \
Eula=accept \
Billing={ENDPOINT_URI} \
ApiKey={API_KEY}
```

Esse comando:

* Executa um contêiner de *análise de sentimento* a partir da imagem de contêiner
* Aloca um núcleo de CPU e 4 gigabytes (GB) de memória
* Expõe a porta TCP 5000 e aloca um pseudo-TTY para o contêiner
* Remove automaticamente o contêiner depois que ele sai. A imagem de contêiner ainda fica disponível no computador host.