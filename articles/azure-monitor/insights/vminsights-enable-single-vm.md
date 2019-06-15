---
title: Habilitar o Azure Monitor para máquinas virtuais (versão prévia) para avaliação | Microsoft Docs
description: Saiba como avaliar o Azure Monitor para VMs em uma única máquina virtual do Azure ou em um conjunto de dimensionamento de máquina virtual.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2019
ms.author: magoedte
ms.openlocfilehash: ec909bcd16f923bbd7036f6a69df2bbb07e561b8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67122472"
---
# <a name="enable-azure-monitor-for-vms-preview-for-evaluation"></a>Habilitar o Azure Monitor para máquinas virtuais (versão prévia) para avaliação

Você pode avaliar o Azure Monitor para máquinas virtuais (versão prévia) em um pequeno número de máquinas virtuais (VMs) ou em uma única escala VM ou máquina virtual definido. É a maneira mais fácil e mais direta para habilitar o monitoramento do portal do Azure. Sua meta é monitorar suas VMs e descobrir quaisquer problemas de desempenho ou disponibilidade. 

Antes de começar, examine os [pré-requisitos](vminsights-enable-overview.md) e certifique-se de sua assinatura e recursos atendem aos requisitos.  

## <a name="enable-monitoring-for-a-single-azure-vm"></a>Habilitar o monitoramento para uma única VM do Azure
Para habilitar o monitoramento da VM do Azure:

1. Entre no [Portal do Azure](https://portal.azure.com).

1. Selecione **Máquinas Virtuais do Microsoft Azure**.

1. Na lista, selecione uma VM.

1. Na página da VM, na seção **Monitoramento**, selecione **Insights (versão prévia)** .

1. Na página **Insights (versão prévia)** , selecione **Experimentar agora**.

    ![Habilitar o Azure Monitor para VMs em uma VM](./media/vminsights-enable-single-vm/enable-vminsights-vm-portal-01.png)

1. Na página **Integração de Insights do Azure Monitor**, caso você tenha um espaço de trabalho do Log Analytics existente na mesma assinatura, selecione-o na lista suspensa.  

    A lista Selecionar previamente o espaço de trabalho padrão e o local em que a VM é implantada na assinatura. 

    >[!NOTE]
    >Para criar um novo espaço de trabalho do Log Analytics para armazenar os dados de monitoramento da VM, consulte [criar um espaço de trabalho do Log Analytics](../../azure-monitor/learn/quick-create-workspace.md). Seu espaço de trabalho do Log Analytics deve pertencer a uma da [regiões com suporte](vminsights-enable-overview.md#log-analytics).

Depois de habilitar o monitoramento, você talvez precise esperar cerca de 10 minutos antes de exibir as métricas de integridade para a VM.

![Habilitar o Azure Monitor para o processamento da implantação do monitoramento das VMs](./media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)

## <a name="enable-monitoring-for-a-single-virtual-machine-scale-set"></a>Habilitar o monitoramento para um conjunto de dimensionamento de máquina virtual única

Para habilitar o monitoramento de seu conjunto de dimensionamento de máquina virtual do Azure:

1. Entre no [Portal do Azure](https://portal.azure.com).

2. Selecione **conjuntos de dimensionamento de máquina Virtual**.

3. Na lista, selecione um conjunto de dimensionamento de máquina virtual.

4. Na máquina virtual de conjunto de dimensionamento de página, além de **Monitoring** seção, selecione **Insights (visualização)** .

5. Sobre o **Insights (visualização)** página, se você quiser usar um espaço de trabalho do Log Analytics existente, selecione-o na lista suspensa.

    A lista Selecionar previamente o espaço de trabalho padrão e o local em que a VM é implantada na assinatura. 

    ![Habilitar o Azure Monitor para as VMs para um conjunto de dimensionamento de máquina virtual](./media/vminsights-enable-single-vm/enable-vminsights-vmss-portal-01.png)

    >[!NOTE]
    >Para criar um novo espaço de trabalho do Log Analytics para armazenar os dados de monitoramento do conjunto de dimensionamento de máquina virtual, consulte [criar um espaço de trabalho do Log Analytics](../learn/quick-create-workspace.md). Seu espaço de trabalho do Log Analytics deve pertencer a uma da [regiões com suporte](vminsights-enable-overview.md#log-analytics).

Depois de habilitar o monitoramento, você talvez precise esperar cerca de 10 minutos antes que você possa exibir os dados de monitoramento para o conjunto de dimensionamento.

>[!NOTE]
>Se você usar um modelo de atualização manual para seu conjunto de dimensionamento, atualize as instâncias para concluir a instalação. Você pode iniciar as atualizações do **instâncias** página, o **configurações** seção.

![Habilitar o Azure Monitor para o processamento da implantação do monitoramento das VMs](./media/vminsights-enable-single-vm/onboard-vminsights-vmss-portal-status-01.png)

Agora que você habilitou o monitoramento para seu conjunto de dimensionamento VM ou máquina virtual, as informações de monitoramento estão disponíveis para análise no Azure Monitor para máquinas virtuais. 

## <a name="next-steps"></a>Próximas etapas

* Para saber como usar o recurso de integridade, consulte [entender a integridade de suas VMs do Azure Monitor](vminsights-health.md). 
* Para exibir dependências de aplicativos descobertos, consulte [usar o Azure Monitor para VMs mapa](vminsights-maps.md). 
* Para identificar afunilamentos, a utilização geral e desempenho da sua VM, consulte [desempenho da VM do Azure de modo de exibição](vminsights-performance.md).