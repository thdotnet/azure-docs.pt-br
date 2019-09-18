---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 935a6201e24e97deaa0a1a65e499310f74a49e2d
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71059063"
---
## <a name="what-is-queue-storage"></a>O que é o armazenamento de filas?

O armazenamento de filas do Azure é um serviço para armazenamento de um grande número de mensagens que podem ser acessadas de qualquer lugar do mundo por meio de chamadas autenticadas usando HTTP ou HTTPS. Uma única mensagem de fila pode ter até 64 KB de tamanho e uma fila pode conter milhões de mensagens, até o limite de capacidade total de uma conta de armazenamento. O armazenamento de filas é geralmente usado para criar uma pendência de trabalho para processar de forma assíncrona.

## <a name="queue-service-concepts"></a>Conceitos do serviço Fila

O serviço Fila do Azure contém os seguintes componentes:

![Fila1](./media/storage-queue-concepts-include/queue1.png)

* **Formato de URL:** As consultas são endereçáveis usando o seguinte formato de URL:   
    http://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    A URL a seguir endereça um fila no diagrama:  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* **Conta de armazenamento:** Todo o acesso ao Armazenamento do Azure ocorre por meio de uma conta de armazenamento. Consulte [Escalabilidade e Metas de Desempenho do Armazenamento do Azure](../articles/storage/common/storage-scalability-targets.md) para obter detalhes sobre a capacidade da conta de armazenamento.
* **Fila:** Uma fila contém um conjunto de mensagens. Todas as mensagens devem estar em uma fila. Observe que o nome da fila deve estar em letras minúsculas. Para saber mais sobre filas de nomenclatura, confira [Nomenclatura de filas e metadados](https://msdn.microsoft.com/library/azure/dd179349.aspx).
* **Mensagem:** Uma mensagem, em qualquer formato, de até 64 KB. O tempo máximo que uma mensagem pode ficar na fila é de sete dias.

