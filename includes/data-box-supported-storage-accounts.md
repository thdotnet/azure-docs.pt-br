---
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: include
ms.date: 05/22/2019
ms.author: alkohli
ms.openlocfilehash: bc156b8c18f46cccf6fc775b82f76383b8c43861
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242195"
---
A seguir, há uma lista dos tipos de armazenamento com suporte para o dispositivo Data Box. Para obter uma lista completa de todos os diferentes tipos de contas de armazenamento e suas funcionalidades completas, confira [Types of storage accounts](/azure/storage/common/storage-account-overview#types-of-storage-accounts) (Tipos de contas de armazenamento).

| **Conta de armazenamento/Tipos de armazenamento com suporte** | **Blob de blocos** |**Blob de páginas*** |**Arquivos do Azure** |**Observações**|
| --- | --- | -- | -- | -- |
| Clássico Standard | S | S | S |
| Uso geral v1 Standard  | S | S | S | Há suporte para a camada de acesso esporádico e a camada de armazenamento frequente.|
| Uso geral v1 Premium  |  | S| | |
| Uso geral v2 Standard  | S | S | S | Há suporte para a camada de acesso esporádico e a camada de armazenamento frequente.|
| Uso geral v2 Premium  |  |S | | |
| Armazenamento de blobs Standard |S | | |Há suporte para a camada de acesso esporádico e a camada de armazenamento frequente. |

\* *-Dados carregados para blobs de página devem ser de 512 bytes alinhados, tais como VHDs.*

>[!NOTE]
> Não há suporte para contas do Azure Data Lake Storage Gen2.
