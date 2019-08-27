---
title: Requisitos e recomendações de contêiner.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 8/26/2019
ms.author: dapine
ms.openlocfilehash: 5da088bf0356dd54d98ec5393fd2db8068f9c666
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70034483"
---
A tabela a seguir descreve os núcleos de CPU e a memória mínimos e recomendados a serem alocados para cada contêiner do Reconhecimento de Texto.

| Contêiner | Mínimo | Recomendado |TPS<br>(Mínimo, máximo)|
|-----------|---------|-------------|--|
|Reconhecimento de Texto|1 núcleo, 8 GB de memória, 0,5 TPS|2 núcleos, 8 GB de memória, 1 TPS|0,5, 1|

* Cada núcleo precisa ser de pelo menos 2,6 GHz (gigahertz) ou mais rápido.
* TPS – transações por segundo

Memória e núcleo correspondem às configurações `--cpus` e `--memory`, que são usadas como parte do comando `docker run`.