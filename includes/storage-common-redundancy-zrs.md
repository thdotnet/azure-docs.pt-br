---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/28/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 9326e6dac88fa23a7bb2bc489064aca5ef52980b
ms.sourcegitcommit: df7942ba1f28903ff7bef640ecef894e95f7f335
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69015917"
---
O armazenamento com redundância de zona (ZRS) replica seus dados de forma síncrona em três clusters de armazenamento em uma única região. Cada cluster de armazenamento é fisicamente separado dos outros e está localizado em sua própria zona de disponibilidade (AZ). Cada zona de disponibilidade &mdash;e o cluster ZRS dentro dela&mdash; é autônomo e inclui utilitários e recursos de rede separados. Uma solicitação de gravação para uma conta de armazenamento ZRS retorna com êxito somente depois que os dados são gravados em todas as réplicas entre os três clusters.

Quando você armazena seus dados em uma conta de armazenamento usando a replicação do ZRS, você pode continuar a acessar e gerenciar seus dados se uma zona de disponibilidade ficar indisponível. O ZRS oferece excelente desempenho e baixa latência. O ZRS oferece os mesmos [alvos de escalabilidade](../articles/storage/common/storage-scalability-targets.md) que o [armazenamento localmente redundante (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

Considere o ZRS para cenários que exigem consistência, durabilidade e alta disponibilidade. Mesmo se uma interrupção ou desastre natural tornar indisponível uma zona de disponibilidade, o ZRS oferece durabilidade para objetos de armazenamento de pelo menos 99.9999999999% (12,9s) em um determinado ano.

O armazenamento com redundância de zona geográfica (GZRS) (visualização) Replica seus dados de forma síncrona em três zonas de disponibilidade do Azure na região primária e, em seguida, replica os dados de maneira assíncrona para a região secundária. O GZRS fornece alta disponibilidade junto com a durabilidade máxima. O GZRS foi projetado para fornecer pelo menos a durabilidade de objetos de 99.99999999999999% (16 9) em um determinado ano. Para acesso de leitura aos dados na região secundária, habilite o armazenamento com redundância de zona geográfica com acesso de leitura (RA-GZRS). Para obter mais informações sobre o GZRS, consulte [armazenamento com redundância de zona geográfica para alta disponibilidade e durabilidade máxima (versão prévia)](../articles/storage/common/storage-redundancy-lrs.md).

Para obter informações sobre zonas de disponibilidade, consulte [visão geral de Zonas de Disponibilidade do Azure](https://docs.microsoft.com/azure/availability-zones/az-overview).
