---
title: Habilitar o Azure Monitor para máquinas virtuais (versão prévia) para avaliação | Microsoft Docs
description: Este artigo descreve como habilitar os Azure Monitor para máquinas virtuais para uma única máquina virtual do Azure ou o conjunto para fins de avaliação de dimensionamento de máquina virtual.
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
ms.openlocfilehash: 673f153b551e4b0c89a564c96d6bd9819ca26f5d
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65524079"
---
# <a name="enable-azure-monitor-for-vms-preview-for-evaluation"></a>Habilitar o Azure Monitor para máquinas virtuais (versão prévia) para avaliação

Para avaliar o Azure Monitor para VMs (versão prévia) em um pequeno número de máquinas virtuais do Azure ou em uma única máquina virtual ou conjunto de dimensionamento de máquina virtual, a abordagem mais fácil e mais direta para habilitar o monitoramento é do portal do Azure. No final deste processo, você será com êxito começaram a monitorá-los e saber se estão tendo problemas de desempenho ou disponibilidade. 

Antes de obter iniciado, não se esqueça de examinar a [pré-requisitos](vminsights-enable-overview.md) e verifique se sua assinatura e seus recursos atendem aos requisitos.  

## <a name="enable-monitoring-for-a-single-azure-vm"></a>Habilitar o monitoramento para uma única VM do Azure
Para habilitar o monitoramento da VM do Azure no portal do Azure, faça o seguinte:

1. Entre no [Portal do Azure](https://portal.azure.com).

1. Selecione **Máquinas Virtuais do Microsoft Azure**.

1. Na lista, selecione uma VM.

1. Na página da VM, na seção **Monitoramento**, selecione **Insights (versão prévia)**.

1. Na página **Insights (versão prévia)**, selecione **Experimentar agora**.

    ![Habilitar o Azure Monitor para VMs em uma VM](./media/vminsights-enable-single-vm/enable-vminsights-vm-portal-01.png)

1. Na página **Integração de Insights do Azure Monitor**, caso você tenha um espaço de trabalho do Log Analytics existente na mesma assinatura, selecione-o na lista suspensa.  

    A lista pré-seleciona o workspace e o local padrão nos quais a máquina virtual é implantada na assinatura. 

    >[!NOTE]
    >Se você quiser criar um novo espaço de trabalho do Log Analytics para armazenar os dados de monitoramento da VM, siga as instruções em [criar um espaço de trabalho do Log Analytics](../../azure-monitor/learn/quick-create-workspace.md) em uma das regiões com suporte listados [aqui](vminsights-enable-overview.md#log-analytics).

Depois de habilitar o monitoramento, poderão ser necessários cerca de 10 minutos antes da exibição das métricas de integridade para a máquina virtual.

![Habilitar o Azure Monitor para o processamento da implantação do monitoramento das VMs](./media/vminsights-enable-single-vm/onboard-vminsights-vm-portal-status.png)

## <a name="enable-monitoring-for-a-single-virtual-machine-scale-set"></a>Habilitar o monitoramento para um conjunto de dimensionamento de máquina virtual única

Para habilitar o monitoramento de seu conjunto no portal do Azure de dimensionamento de máquina virtual do Azure, faça o seguinte:

1. Entre no [Portal do Azure](https://portal.azure.com).

2. Selecione **conjuntos de dimensionamento de máquina Virtual**.

3. Na lista, selecione um conjunto de dimensionamento de máquina virtual.

4. Na máquina virtual de conjunto de dimensionamento de página, além de **Monitoring** seção, selecione **Insights (visualização)**.

5. Sobre o **Insights (visualização)** página, se você tiver um espaço de trabalho do Log Analytics existente que deseja usar, selecione-o na lista suspensa.

    A lista pré-seleciona o workspace e o local padrão nos quais a máquina virtual é implantada na assinatura. 

    ![Habilitar o Azure Monitor para as VMs para um conjunto de dimensionamento de máquina virtual](./media/vminsights-enable-single-vm/enable-vminsights-vmss-portal-01.png)

    >[!NOTE]
    >Se você quiser criar um novo espaço de trabalho do Log Analytics para armazenar os dados de monitoramento da VM, siga as instruções em [criar um espaço de trabalho do Log Analytics](../learn/quick-create-workspace.md) em uma das regiões com suporte listados [aqui](vminsights-enable-overview.md#log-analytics).

Depois de habilitar o monitoramento, pode levar cerca de 10 minutos antes que você possa exibir os dados de monitoramento para o conjunto de dimensionamento.

>[!NOTE]
>Se você estiver usando um modelo de atualização manual para seu conjunto de dimensionamento, você precisará atualizar as instâncias para concluir a instalação.  Isso pode ser feito na página em instâncias de **configurações** seção.

![Habilitar o Azure Monitor para o processamento da implantação do monitoramento das VMs](./media/vminsights-enable-single-vm/onboard-vminsights-vmss-portal-status-01.png)

## <a name="next-steps"></a>Próximas etapas

Agora que o monitoramento está habilitado para sua máquina virtual ou conjunto de dimensionamento de máquina virtual, essas informações estão disponíveis para análise com o Azure Monitor para VMs. Para saber como usar o recurso Integridade, confira [Exibir a Integridade do Azure Monitor para VMs](vminsights-health.md). Para exibir as dependências de aplicativos descobertas, confira [Exibir o Mapa do Azure Monitor para VMs](vminsights-maps.md). Para identificar gargalos e a utilização geral com o desempenho de VMs, consulte [Exibir o desempenho da VM do Azure](vminsights-performance.md), ou para exibir dependências de aplicativos descobertos, consulte [Exibir o Azure Monitor para mapa de VMs](vminsights-maps.md).