---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/04/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: d25efa8c666fa91c7bd652a7cf931ea6ca379aa2
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133854"
---
O armazenamento com redundância de zona (ZRS) replica seus dados de forma síncrona em três clusters de armazenamento em uma única região. Cada cluster de armazenamento é fisicamente separado dos outros e está localizado em sua própria zona de disponibilidade (AZ). Cada zona de disponibilidade &mdash;e o cluster ZRS dentro dela&mdash; é autônomo e inclui utilitários e recursos de rede separados. Uma solicitação de gravação para uma conta de armazenamento ZRS retorna com êxito somente depois que os dados são gravados para todas as réplicas em três clusters.

Quando você armazena seus dados em uma conta de armazenamento usando a replicação do ZRS, você pode continuar a acessar e gerenciar seus dados se uma zona de disponibilidade ficar indisponível. O ZRS oferece excelente desempenho e baixa latência. O ZRS oferece os mesmos [alvos de escalabilidade](../articles/storage/common/storage-scalability-targets.md) que o [armazenamento localmente redundante (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

Considere o ZRS para cenários que exigem consistência, durabilidade e alta disponibilidade. Mesmo se uma interrupção ou desastre natural tornar indisponível uma zona de disponibilidade, o ZRS oferece durabilidade para objetos de armazenamento de pelo menos 99.9999999999% (12,9s) em um determinado ano.

Para obter informações sobre zonas de disponibilidade, consulte [visão geral de Zonas de Disponibilidade do Azure](https://docs.microsoft.com/azure/availability-zones/az-overview).