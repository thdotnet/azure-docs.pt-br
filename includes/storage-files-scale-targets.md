---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 05/06/2019
ms.author: tamram
ms.openlocfilehash: 213ecee34df46c0a408e7034a07ab864c96b340b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66114508"
---
| Resource | Compartilhamentos de arquivos padrão | Compartilhamentos de arquivos Premium (versão prévia) |
|----------|---------------|------------------------------------------|
| Tamanho mínimo de um compartilhamento de arquivo | Nenhum mínimo; pago conforme o uso | 100 giB; provisionado |
| Tamanho máximo de um compartilhamento de arquivos | 5 TiB | 100 TiB |
| Tamanho máximo de um arquivo em um compartilhamento de arquivos | 1 TiB | 1 TiB |
| Número máximo de arquivos em um compartilhamento de arquivos | Sem limite | Sem limite |
| Máximo de IOPS por compartilhamento | IOPS DE 1.000 | 100.000 IOPS |
| Número máximo de políticas de acesso armazenadas por arquivo compartilhar | 5 | 5 |
| Taxa de transferência de destino para um compartilhamento de arquivo único | Até 60 MiB/s | Ver os valores de entrada e saída de compartilhamento de arquivo do premium|
| Egresso máximo para um compartilhamento de arquivo único | Consulte a taxa de transferência de destino de compartilhamento de arquivo padrão | Até 6,204 MiB/s |
| Máximo de entrada para um compartilhamento de arquivo único | Consulte a taxa de transferência de destino de compartilhamento de arquivo padrão | Até 4.136 MiB/s |
| Máximo de identificadores abertos por arquivo | 2.000 identificadores abertos | 2.000 identificadores abertos |
| Número máximo de instantâneos de compartilhamento | 200 instantâneos de compartilhamento | 200 instantâneos de compartilhamento |
| Tamanho máximo do nome do objeto (diretórios e arquivos) | 2.048 caracteres | 2.048 caracteres |
| Componente de nome de caminho máximo (no caminho \A\B\C\D, cada letra é um componente) | 255 caracteres | 255 caracteres |