---
title: Níveis de serviço no Azure NetApp Files | Microsoft Docs
description: Descreve o desempenho de taxa de transferência para os níveis de serviço do Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: b-juche
ms.openlocfilehash: 1f9c427045c9d42f6a11cc4bcc798cfc47a4428c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65523117"
---
# <a name="service-levels-for-azure-netapp-files"></a>Níveis de serviço do Azure NetApp Files
Os níveis de serviço são um atributo de um pool de capacidade. Os níveis de serviço são definidos e diferenciados por taxa de transferência máxima permitida para um volume no pool de capacidade com base em cota atribuída ao volume.

## <a name="supported-service-levels"></a>Níveis de serviço com suporte

Os arquivos NetApp do Azure dá suporte a três níveis de serviço: *Ultra*, *Premium*, e *padrão*. 

* <a name="Ultra"></a>Armazenamento Ultra

    A camada de armazenamento Ultra fornece até 128 MiB/s de taxa de transferência por 1 TiB de cota do volume atribuída. 

* <a name="Premium"></a>Armazenamento Premium

    A camada de armazenamento Premium fornece até 64 MiB/s de taxa de transferência por 1 TiB de cota do volume atribuída. 

* <a name="Standard"></a>Armazenamento Standard

    A camada de armazenamento Standard fornece até 16 MiB/s de taxa de transferência por 1 TiB de cota do volume atribuída.

## <a name="throughput-limits"></a>Limites de taxa de transferência

O limite de taxa de transferência para um volume é determinado pela combinação dos seguintes fatores:
* O nível de serviço do pool de capacidade ao qual pertence o volume
* A cota atribuída ao volume  

Esse conceito é ilustrado no diagrama a seguir:

![Ilustração de nível de serviço](../media/azure-netapp-files/azure-netapp-files-service-levels.png)

No exemplo 1 acima, um volume de um pool de capacidade com a camada de armazenamento Premium que é atribuído 2 TiB de cota será atribuído um limite de taxa de transferência de 128 MiB/s (TiB 2 * MiB/s 64). Esse cenário se aplica independentemente do tamanho do pool de capacidade ou o consumo real de volume.

No exemplo 2 acima, um volume de um pool de capacidade com a camada de armazenamento Premium que é atribuído a 100 GiB de cota será atribuído um limite de taxa de transferência de 6,25 MiB/s (TiB 0.09765625 * MiB/s 64). Esse cenário se aplica independentemente do tamanho do pool de capacidade ou o consumo real de volume.

## <a name="next-steps"></a>Próximas etapas

- Confira a [Página de preços do Azure NetApp Files](https://azure.microsoft.com/pricing/details/storage/netapp/) para ver o preço de diferentes níveis de serviço
- Ver [modelo de custo para arquivos do Azure NetApp](azure-netapp-files-cost-model.md) para o cálculo do consumo de capacidade em um pool de capacidade 
- [Configurar um pool de capacidade](azure-netapp-files-set-up-capacity-pool.md)