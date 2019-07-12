---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 0f87b122f81fbd04767354e623dcb808466ee1be
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673226"
---
## <a name="supported-operating-systems-and-drivers"></a>Sistemas operacionais e drivers com suporte

### <a name="nvidia-tesla-cuda-drivers"></a>Drivers NVIDIA Tesla (CUDA)

Drivers NVIDIA Tesla (CUDA) para NC, NCv2, NCv3, ND e VMs da série NDV2 (opcional para a série NV) têm suporte apenas em sistemas operacionais listados na tabela a seguir. Os links de download do driver Tesla são atuais no momento da publicação. Para os drivers mais recentes, visite o site da [NVIDIA](https://www.nvidia.com/).

> [!TIP]
> Como alternativa à instalação manual do driver CUDA em uma VM do Windows Server, você pode implantar uma imagem da [Máquina Virtual de Ciência de Dados](../articles/machine-learning/data-science-virtual-machine/overview.md) do Azure. As edições DSVM do Windows Server 2016 instalam previamente os drivers NVIDIA CUDA, a Biblioteca de Rede Neural Profunda CUDA e outras ferramentas.


| OS | Driver |
| -------- |------------- |
| Windows Server 2016 | [398.75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2016-international.exe) (.exe) |
| Windows Server 2012 R2 | [398.75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (.exe) |

### <a name="nvidia-grid-drivers"></a>Drivers NVIDIA GRID

A Microsoft redistribui os instaladores do driver NVIDIA GRID para VMs das séries NV e NVv2 usadas como estações de trabalho virtuais ou para aplicativos virtuais. Instale somente esses drivers de grade em VMs do Azure NV, apenas em sistemas operacionais listados na tabela a seguir. Esses drivers incluem o licenciamento de Software de GPU Virtual de GRID no Azure. Não é necessário você configurar um servidor de licença de software vGPU NVIDIA.

| OS | Driver |
| -------- |------------- |
| Windows Server 2019<br/><br/>Windows Server 2016<br/><br/>Windows 10 (até a versão 1809. Sem patch/não há suporte para o nível do patch 0 de 1809). | [GRADE 8.0 (425.31)](https://go.microsoft.com/fwlink/?linkid=874181) (.exe) |
| Windows Server 2012 R2 | [GRADE 8.0 (425.31)](https://go.microsoft.com/fwlink/?linkid=874184) (.exe)  |
