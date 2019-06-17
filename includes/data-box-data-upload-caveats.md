---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: b159ec8620fa8c93e4917f73be9b9898e1b4fbcc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244658"
---
- Não copie arquivos diretamente para qualquer um dos compartilhamentos pré-criada. Você precisa criar uma pasta no compartilhamento e, em seguida, copiar arquivos para a pasta.
- Uma pasta no *StorageAccount_BlockBlob* e no *StorageAccount_PageBlob* é um contêiner. Por exemplo, os contêineres são criados como *StorageAccount_BlockBlob/container* e *StorageAccount_PageBlob/container*.
- Cada pasta criada diretamente em *StorageAccount_AzureFiles* é convertida em um compartilhamento de arquivos do Azure.
- Se você tiver um objeto existente do Azure (como um blob ou arquivo) na nuvem com o mesmo nome que o objeto que está sendo copiado, o Data Box substituirá o arquivo na nuvem.
- Todos os arquivos gravados nos compartilhamentos *StorageAccount_BlockBlob* e *StorageAccount_PageBlob* são carregados como um blob de blocos e um blob de páginas, respectivamente.
- Armazenamento de BLOBs do Azure não dá suporte a diretórios. Se você criar uma pasta na pasta *StorageAccount_BlockBlob*, as pastas virtuais serão criadas no nome do blob. Para os Arquivos do Azure, a estrutura de diretório real é mantida.
- Qualquer esvaziar a hierarquia de diretórios (sem nenhum arquivo) criada sob *StorageAccount_BlockBlob* e *StorageAccount_PageBlob* pastas não está carregado.
- Se há erros ao carregar dados no Azure, um log de erros é criado na conta de armazenamento de destino. O caminho para esse log de erros estará disponível quando o upload for concluído e você poderá examinar o log para tomar uma ação corretiva. Não exclua os dados da origem sem verificar os dados carregados.
