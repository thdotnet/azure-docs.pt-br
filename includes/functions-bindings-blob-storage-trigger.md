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
ms.openlocfilehash: 557e34e8a6bddd36a92b0e212bda3609baa14407
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69642156"
---
Você pode usar os seguintes tipos de parâmetro para o blob de gatilho:

* `Stream`
* `TextReader`
* `string`
* `Byte[]`
* Um POCO serializado como JSON
* `ICloudBlob`<sup>1</sup>
* `CloudBlockBlob`<sup>1</sup>
* `CloudPageBlob`<sup>1</sup>
* `CloudAppendBlob`<sup>1</sup>

<sup>1</sup> Requer associação "inout" `direction` em *function.json* ou `FileAccess.ReadWrite` em uma biblioteca de classes C#.

Se você tentar associar a um dos tipos de SDK de armazenamento e obter uma mensagem de erro, certifique-se de que você tem uma referência a [a versão correta do SDK de armazenamento](#azure-storage-sdk-version-in-functions-1x).

Associação para `string`, `Byte[]`, ou POCO só é recomendada se o tamanho do blob for pequeno, pois o conteúdo inteiro do blob é carregado na memória. Geralmente, é preferível usar um tipo `Stream` ou `CloudBlockBlob`. Para obter mais informações, consulte [Concorrência e uso de memória](#trigger---concurrency-and-memory-usage) mais adiante neste artigo.