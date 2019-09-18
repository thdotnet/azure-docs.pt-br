---
title: Fazer failback de VMs do Azure replicadas para uma região do Azure secundária para recuperação de desastre com o serviço do Azure Site Recovery.
description: Saiba como fazer failback de VMs do Azure com o serviço Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 09/09/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: c8be547790452774992b9226ca8010532263aaff
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70814522"
---
# <a name="fail-back-an-azure-vm-between-azure-regions"></a>Failback de uma VM do Azure entre regiões do Azure

O serviço [Azure Site Recovery](site-recovery-overview.md) contribui para sua estratégia de recuperação de desastre ao gerenciar e orquestrar a replicação, o failover e o failback de computadores locais e de VMs (máquinas virtuais) do Azure.

Este tutorial descreve como fazer failback de uma única VM do Azure. Depois de fazer failover, você deverá fazer failback para a região primária quando ela estiver disponível. Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> 
> * Fazer failback da VM na região secundária.
> * Proteja novamente a VM primária de volta para a região secundária.
> 
> [!NOTE]
> 
> Este tutorial ajuda você a fazer failover de algumas VMs para uma região de destino de volta para a região de origem com as personalizações mínimas. Para obter mais instruções detalhadas, examine os [guias de instruções sobre VMs do Azure](https://docs.microsoft.com/azure/virtual-machines/windows/).

## <a name="before-you-start"></a>Antes de começar

* Certifique-se de que o status da VM seja **Failover confirmado**.
* Verifique se a região primária está disponível e, em seguida, você poderá criar e acessar os novos recursos nela.
* Certifique-se de que a nova proteção esteja habilitada.

## <a name="fail-back-to-the-primary-region"></a>Failback para a região primária

Depois que as VMs forem protegidas novamente, você poderá fazer failback para a região primária, conforme necessário.

1. No cofre, selecione **Itens replicados**, depois selecione a VM que foi protegida novamente.

    ![Failback para primária](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback.png)

2. Em **Itens replicados**, selecione a VM e, em seguida, selecione **Failover**.
3. Em **Failover**, selecione um ponto de recuperação para executar o failover:
    - **Mais recente (padrão)** : processa todos os dados no serviço Site Recovery e fornece o menor RPO (Objetivo de ponto de recuperação).
    - **Mais recente processado**: Reverte a VM para o último ponto de recuperação que foi processado pelo Site Recovery.
    - **Personalizado**: Faz failover para um determinado ponto de recuperação. Essa opção é útil para fazer um failover de teste.
4. Selecione **Desligar o computador antes do início do failover** se quiser que o Site Recovery tente realizar um desligamento das VMs da região de DR antes de disparar o failover. O failover continuará, mesmo se o desligamento falhar. 
5. Acompanhe o progresso do failover na página **Trabalhos**.
6. Após a conclusão do failover, valide a VM fazendo logon nela. É possível alterar o ponto de recuperação conforme necessário.
7. Depois de verificar o failover, selecione **Fazer commit do failover**. Confirmar exclui todos os pontos de recuperação disponíveis. A opção Alterar ponto de recuperação não está mais disponível.
8. A VM deve aparecer como o fazer failover realizado e failback realizado.

    ![VM nas regiões primária e secundária](./media/site-recovery-azure-to-azure-failback/azure-to-azure-failback-vm-view.png)

## <a name="next-steps"></a>Próximas etapas

[Saiba mais](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection) sobre o fluxo de nova proteção.
