---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/20/2019
ms.author: tamram
ms.openlocfilehash: 2319a7620b70547d186a4ef5cb96f5ca6684c62c
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305250"
---
| Resource | Destino |
|----------|---------------|
| Tamanho máximo de uma única tabela | 500 TiB |
| Tamanho máximo de uma entidade de tabela | 1 MiB |
| Número máximo de propriedades em uma entidade de tabela | 255, que inclui três propriedades do sistema: PartitionKey, RowKey e Timestamp |
| Tamanho máximo total de valores de propriedade em uma entidade | 1 MiB |
| Tamanho máximo total de uma propriedade individual em uma entidade | Varia por tipo de propriedade. Para obter mais informações, consulte **tipos de propriedade** na [Noções básicas sobre o modelo de dados do serviço de tabela](/rest/api/storageservices/understanding-the-table-service-data-model). |
| Número máximo de políticas de acesso armazenadas por tabela | 5 |
| Taxa máxima de solicitação por conta de armazenamento | 20.000 transações por segundo, que assume um tamanho de entidade de 1 KiB |
| Taxa de transferência de destino para uma partição de tabela única (-entidades de 1 KiB) | Até 2.000 entidades por segundo |
