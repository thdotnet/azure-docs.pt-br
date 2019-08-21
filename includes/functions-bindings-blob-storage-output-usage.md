---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: functions
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.topic: include
ms.date: 08/02/2019
ms.author: cshoe
ms.custom: include file
ms.openlocfilehash: 414381d42c52e7489fbd6aaacf0f079cdd978ebe
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69642091"
---
Você pode associar aos seguintes tipos para gravar BLOBs:

* `TextWriter`
* `out string`
* `out Byte[]`
* `CloudBlobStream`
* `Stream`
* `CloudBlobContainer`<sup>1</sup>
* `CloudBlobDirectory`
* `ICloudBlob`<sup>2</sup>
* `CloudBlockBlob`<sup>2</sup>
* `CloudPageBlob`<sup>2</sup>
* `CloudAppendBlob`<sup>2</sup>

<sup>1</sup> Requer associação "in" `direction` em *function.json* ou `FileAccess.Read` em uma biblioteca de classes C#. No entanto, você pode usar o objeto de contêiner que o tempo de execução fornece para operações de gravação, como carregar blobs no contêiner.

<sup>2</sup> Requer associação "inout" `direction` em *function.json* ou `FileAccess.ReadWrite` em uma biblioteca de classes C#.

Se você tentar associar a um dos tipos de SDK de armazenamento e obter uma mensagem de erro, certifique-se de que você tem uma referência a [a versão correta do SDK de armazenamento](#azure-storage-sdk-version-in-functions-1x).

Em funções assíncronas, use o valor de retorno ou `IAsyncCollector` em vez de um parâmetro `out`.

Associação para `string` ou `Byte[]` só é recomendada se o tamanho do blob for pequeno, pois o conteúdo inteiro do blob é carregado na memória. Geralmente, é preferível usar um tipo `Stream` ou `CloudBlockBlob`. Para obter mais informações, consulte [Concorrência e uso de memória](#trigger---concurrency-and-memory-usage) mais adiante neste artigo.