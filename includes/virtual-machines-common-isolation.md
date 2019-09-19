---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: virtual-machines
author: ayshakeen
ms.service: virtual-machines
ms.topic: include
ms.date: 09/18/2019
ms.author: azcspmt;ayshak;cynthn
ms.custom: include file
ms.openlocfilehash: 0f528117b19a3f26b964dee7ae453fc184c8dddb
ms.sourcegitcommit: fad368d47a83dadc85523d86126941c1250b14e2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71123068"
---
A Computação do Azure oferece tamanhos de máquina virtual Isolada, para um tipo de hardware específico e dedicada a um único cliente.  Esses tamanhos de máquina virtual são mais adequados para cargas de trabalho que exigem um alto grau de isolamento de outros clientes, para cargas de trabalho que envolvem elementos como requisitos normativos e de conformidade.  Os clientes também podem optar por subdividir ainda mais os recursos dessas máquinas virtuais Isoladas usando [o Suporte do Azure para máquinas virtuais aninhadas](https://azure.microsoft.com/blog/nested-virtualization-in-azure/).

Utilizar um tamanho isolado garante que sua máquina virtual será apenas sendo executada na instância de servidor específico.  As ofertas atuais da máquina virtual isolada incluem:
* Standard_E64is_v3
* Standard_E64i_v3
* Standard_M128ms
* Standard_GS5
* Standard_G5
* Standard_DS15_v2
* Standard_D15_v2
* Standard_F72s_v2

Você pode saber mais sobre cada tamanho isolado disponível [aqui](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory).