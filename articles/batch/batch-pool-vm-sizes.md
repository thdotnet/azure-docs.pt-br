---
title: Escolher tamanhos de VM para pools - Azure Batch | Microsoft Docs
description: Como escolher entre os tamanhos de VM disponíveis para nós de computação em pools de Lote do Azure
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2019
ms.author: lahugh
ms.custom: seodec18
ms.openlocfilehash: 033e0865f23034b94e3133e0ba5890eca4e746ea
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080884"
---
# <a name="choose-a-vm-size-for-compute-nodes-in-an-azure-batch-pool"></a>Escolher um tamanho de VM para nós de computação em um pool do Lote do Azure

Ao selecionar um tamanho de nó para um pool de Lote do Azure, você pode escolher dentre quase todos os tamanhos de VM disponíveis no Azure. O Azure oferece uma variedade de tamanhos para VMs do Linux e do Windows para diferentes cargas de trabalho.

Há algumas exceções e limitações para escolher um tamanho de VM:

* Não há suporte para algumas séries de VM ou tamanhos de VM em lote.
* Alguns tamanhos de VM são restritos e precisam ser especificamente habilitados antes de serem alocados.

## <a name="supported-vm-series-and-sizes"></a>Série de VM e tamanhos com suporte

### <a name="pools-in-virtual-machine-configuration"></a>Pools na configuração de Máquina Virtual

Pools de lote na configuração da máquina Virtual oferecem suporte a quase todos os tamanhos de VM ([Linux](../virtual-machines/linux/sizes.md), [Windows](../virtual-machines/windows/sizes.md)). Consulte a tabela a seguir para saber mais sobre tamanhos com suporte e restrições.

Qualquer promocionais ou tamanhos de VM de visualização não listados não são garantidos para suporte.

| Série da VM  | Tamanhos com suporte | Modo de alocação de pool de conta do lote<sup>1</sup> |
|------------|---------|-----------------|
| Série A básica | Todos os tamanhos *exceto* Basic_A0 (A0) | Qualquer |
| Séria A | Todos os tamanhos *exceto* Standard_A0 | Qualquer |
| Série Av2 | Todos os tamanhos | Qualquer |
| Série B | Nenhum | Não disponível |
| Série DC | Nenhum | Não disponível |
| Dv2, Dsv2-series | Todos os tamanhos | Qualquer |
| Dv3, Dsv3-series | Todos os tamanhos | Qualquer |
| [Tamanhos de memória otimizada](../virtual-machines/linux/sizes-memory.md) | Nenhum | Não disponível |
| Série Fsv2 | Todos os tamanhos | Qualquer |
| Série H | Todos os tamanhos | Qualquer |
| Série Hb | Todos os tamanhos | Modo de assinatura do usuário |
| Série Hc | Todos os tamanhos | Modo de assinatura do usuário |
| Série Ls | Todos os tamanhos | Qualquer |
| Série Lsv2 | Nenhum | Não disponível |
| Série M | Standard_M64ms (baixa prioridade só), Standard_M128s (baixa prioridade apenas) | Qualquer |  
| Série NCv2<sup>2</sup> | Todos os tamanhos | Qualquer |
| Série NCv3<sup>2</sup> | Todos os tamanhos | Qualquer |
| Série ND<sup>2</sup> | Todos os tamanhos | Qualquer |
| NDv2-series | Todos os tamanhos | Modo de assinatura do usuário |
| Série NV | Todos os tamanhos | Qualquer |
| Série NVv3 | Nenhum | Não disponível |
| SAP HANA | Nenhum | Não disponível |

<sup>1</sup> algumas séries VM mais recentes têm suporte parcial inicialmente. Essas séries VM podem ser alocadas por contas do lote com o **modo de alocação de pool** definido como **assinatura de usuário**. Ver [contas do lote gerenciar](batch-account-create-portal.md#additional-configuration-for-user-subscription-mode) para obter mais informações sobre a configuração de conta do lote. Ver [cotas e limites](batch-quota-limit.md) para saber como solicitar a cota para eles com suporte parcial a série VM para **assinatura de usuário** contas do lote.  

<sup>2</sup> esses tamanhos de VM podem ser alocados em pools do lote na configuração da máquina Virtual, mas você deve solicitar um determinado [aumento de cota](batch-quota-limit.md#increase-a-quota).

### <a name="pools-in-cloud-service-configuration"></a>Pools na configuração de Serviço de Nuvem

Os pools de lote na configuração do Serviço de Nuvem oferecem suporte a todos os [tamanhos de VM para Serviços de Nuvem](../cloud-services/cloud-services-sizes-specs.md) **exceto** para:

| Série da VM  | Tamanhos sem suporte |
|------------|-------------------|
| Séria A   | Extrapequena       |
| Série Av2 | Standard_A1_v2, Standard_A2_v2, Standard_A2m_v2 |

## <a name="size-considerations"></a>Considerações de tamanhos

* **Requisitos do aplicativo** - considere as características e os requisitos dos aplicativos que você vai executar nos nós. Os aspectos como se o aplicativo tem multithread e quanta memória ele consome podem ajudar a determinar o tamanho do nó mais adequado e econômico. Para [cargas de trabalho de MPI](batch-mpi.md) de instâncias múltiplas ou aplicativos CUDA, considere tamanhos de VM de [HPC](../virtual-machines/linux/sizes-hpc.md) especializado ou [habilitado para GPU](../virtual-machines/linux/sizes-gpu.md), respectivamente. (Confira [Usar instâncias compatíveis com RDMA ou habilitadas para GPU em pools do Lote](batch-pool-compute-intensive-sizes.md).)

* **Tarefas por nó** - Geralmente, você seleciona um tamanho de nó supondo que uma tarefa seja executada no nó por vez. No entanto, pode ser vantajoso ter várias tarefas (e, portanto, várias instâncias do aplicativo) [executadas em paralelo](batch-parallel-node-tasks.md) em nós de computação durante a execução do trabalho. Nesse caso, é comum escolher um tamanho multicore de nó para acomodar a demanda crescente de execução de tarefas paralelas.

* **Carregar níveis para tarefas diferentes** - Todos os nós em um pool têm o mesmo tamanho. Se você pretende executar aplicativos com diferentes requisitos de sistema e/ou níveis de carga, é recomendável usar pools separados.

* **Disponibilidade de região** -série de uma VM ou tamanho pode não estar disponível nas regiões em que você cria suas contas do lote. Para verificar se um tamanho está disponível, consulte [Produtos disponíveis por região](https://azure.microsoft.com/regions/services/).

* **Cotas** - As [cotas principais](batch-quota-limit.md#resource-quotas) na sua conta do Lote podem limitar o número de nós de um determinado tamanho que você pode adicionar a um pool do Lote. Para solicitar um aumento de cota, consulte [este artigo](batch-quota-limit.md#increase-a-quota). 

* **Configuração do pool** - Em geral, você tem mais opções de tamanho de VM quando você cria um pool na configuração da Máquina Virtual, em comparação com a configuração do serviço de nuvem.

## <a name="next-steps"></a>Próximas etapas

* Para uma visão geral detalhada do Lote, confira [Desenvolver soluções de computação paralela em grande escala com o Lote](batch-api-basics.md).
* Para obter informações sobre tamanhos de VM de computação intensiva, consulte [Usar instâncias compatíveis com RDMA ou habilitadas para GPU em pools do Lote](batch-pool-compute-intensive-sizes.md).
