---
title: Métricas do Azure NetApp Files | Microsoft Docs
description: Descreve as métricas do Azure NetApp Files.
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
ms.date: 08/07/2019
ms.author: b-juche
ms.openlocfilehash: 7cf382f511d2ba8452d77bf207f36b749cb31e94
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68848799"
---
# <a name="metrics-for-azure-netapp-files"></a>Métricas do Azure NetApp Files

O Azure NetApp Files fornece métricas sobre o armazenamento alocado, a utilização de armazenamento real, taxa de transferência de volume, IOPS e latência. Analisando essas métricas, você pode obter um melhor entendimento sobre o padrão de uso e o desempenho do volume de suas contas do NetApp.  

## <a name="capacity_pools"></a>Métricas de uso de pools de capacidade

<!-- 
- *Volume pool allocated size*  
    The size (GiB) of the provisioned capacity pool  
--> 
- *Pool de volume alocado usado*  
    O total de cotas de volume (GiB) em um determinado pool de capacidade (ou seja, o total de tamanhos provisionados de volumes no pool de capacidade)  
    Esse é o tamanho selecionado durante a criação do volume.  
- *Tamanho lógico total do pool de volume*  
    O total de espaço lógico (GiB) usado em volumes em um pool de capacidade  
<!-- 
- *Volume pool total snapshot size*  
    The total of incremental logical space used by the snapshots  
-->

## <a name="volumes"></a>Métricas de uso de volumes

<!-- 
- *Volume allocated size*   
    The volume size (quota) provisioned in GiB  
--> 
- *Tamanho do volume lógico*   
    O espaço lógico total usado em um volume (GiB)  
    Esse tamanho inclui espaço lógico usado por sistemas de arquivos ativos e instantâneos.  
- *Tamanho do instantâneo de volume*   
   O espaço lógico incremental usado por instantâneos em um volume  

## <a name="performance-metrics-for-volumes"></a>Métricas de desempenho para volumes

- *AverageReadLatency*   
    O tempo médio para leituras do volume em milissegundos
- *AverageWriteLatency*   
    O tempo médio para gravações do volume em milissegundos
- *ReadIops*   
    O número de leituras para o volume por segundo
- *WriteIops*   
    O número de gravações no volume por segundo

## <a name="next-steps"></a>Próximas etapas

* [Compreender a hierarquia de armazenamento do Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Configurar um pool de capacidade](azure-netapp-files-set-up-capacity-pool.md)
* [Criar um volume do Azure NetApp Files](azure-netapp-files-create-volumes.md)
