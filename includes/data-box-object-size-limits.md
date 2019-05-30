---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: eb12adf8f8523686b1d8deda2776eb203a76e954
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244637"
---
Aqui estão os tamanhos dos objetos do Azure que podem ser gravados. Verifique se todos os arquivos que são carregados estão em conformidade com esses limites.

| Tipo de objeto do Azure | Limite padrão                                             |
|-------------------|-----------------------------------------------------------|
| Blob de blocos        | Aproximadamente 4,75 TiB                                                 |
| Blob de páginas         | 8 TiB <br> Todos os arquivos carregados no formato de blob de páginas precisarão ser alinhados com 512 bytes (um integral múltiplo), caso contrário o upload falhará. <br> O VHD e o VHDX são alinhados a 512 bytes. |
| Arquivos do Azure        | 1 TiB                                                      |
| Discos gerenciados     | 4 TiB <br> Para obter mais informações sobre o tamanho e limites, consulte: <li>[Metas de escalabilidade do SSDs padrão](../articles/virtual-machines/windows/disks-types.md#standard-ssd)</li><li>[Metas de escalabilidade do SSDs Premium](../articles/virtual-machines/windows/disks-types.md#standard-hdd)</li><li>[Metas de escalabilidade de HDDs padrão](../articles/virtual-machines/windows/disks-types.md#premium-ssd)</li><li>[Preços e cobrança de discos gerenciados](../articles/virtual-machines/windows/disks-types.md#billing)</li>  
