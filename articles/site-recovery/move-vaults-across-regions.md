---
title: Como mover a configuração de Azure Site Recovery para outra região do Azure | Microsoft Docs
description: Diretrizes para mover a configuração do Site Recovery para outra região do Azure
services: site-recovery
author: rajani-janaki-ram
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/31/2019
ms.author: rajanaki
ms.custom: MVC
ms.openlocfilehash: ba0e2577d6fb8bac66322917936fe06825af0d96
ms.sourcegitcommit: 6ad03fa28a0f60cb6dce6144f728c2ceb56ff6e2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68708459"
---
# <a name="moving-recovery-services-vault-and--azure-site-recovery-configuration-to-another-azure-region"></a>Como mover o cofre dos Serviços de Recuperação e a configuração do Azure Site Recovery para outra região do Azure

Há vários cenários em que você desejaria mover seus recursos existentes do Azure de uma região para outra: capacidade de gerenciamento, motivos de governança, devido a fusões/aquisições da empresa etc. Um dos recursos relacionados que você pode querer mover ao mover as VMs do Azure é a configuração de recuperação de desastre delas. Não há nenhuma maneira não trabalhosa de mover uma configuração de recuperação de desastre existente de uma região para outra. Isso se deve essencialmente ao fato de a região de destino ser configurada com base na região da VM de origem e, quando se decide alterar isso, as configurações anteriores existentes da região de destino não podem ser reutilizadas e precisam ser redefinidas. Este artigo define o processo passo a passo para redefinir a configuração de recuperação de desastre e movê-la para uma região diferente.

Neste documento, você vai:

> [!div class="checklist"]
> * Verificar os pré-requisitos para a movimentação
> * Identificar os recursos que foram usados pelo Azure Site Recovery 
> * Desabilitar a replicação
> * Excluir os recursos 
> * Configurar novamente o Site Recovery com base na nova região de origem para as VMs.

> [!IMPORTANT]
> Atualmente, não há nenhuma maneira não trabalhosa para mover um cofre dos Serviços de Recuperação e a configuração de DR (recuperação de desastre) no estado em que se encontra para uma região diferente. Este documento guia o cliente pelo processo de desabilitar a replicação e redefini-la na nova região.

## <a name="prerequisites"></a>Pré-requisitos

- Não deixe de remover e excluir a configuração de recuperação de desastre antes de tentar mover as VMs do Azure para uma região diferente. 

> [!NOTE]
> Se a nova região de destino para a VM do Azure for igual à região de destino da recuperação de desastre, você poderá usar a configuração de replicação existente e movê-la com base nas etapas mencionadas [aqui](azure-to-azure-tutorial-migrate.md) 

- Verifique se você está tomando uma decisão informada e se as partes interessadas foram informadas a respeito, pois sua VM não será protegida contra desastres até que a movimentação da VM seja concluída. 

## <a name="identify-the-resources-that-were-used-by-azure-site-recovery"></a>Identificar os recursos que foram usados pelo Azure Site Recovery
É recomendável realizar essa etapa antes de prosseguir para a próxima, pois será mais fácil identificar os recursos relevantes enquanto as VMs ainda estão sendo replicadas

Para cada VM do Azure que está sendo replicada, navegue até **Itens Protegidos** > **Itens Replicados**>**Propriedades** e identifique os recursos a seguir

- Grupo de recursos de destino
- Conta de armazenamento em cache
- Conta de armazenamento de destino (no caso de uma VM do Azure baseada em disco não gerenciado) 
- Rede de destino


## <a name="disable-existing-disaster-recovery-configuration"></a>Desabilitar a configuração de recuperação de desastre existente

1. Navegar para o **cofre dos Serviços de Recuperação** 
2.  Em **Itens Protegidos** > **Itens Replicados**, clique com o botão direito na máquina > **Desabilitar replicação**.
3. Repita isso para todas as VMs que você deseja mover.
> [!NOTE]
> O serviço de mobilidade não será desinstalado dos servidores protegidos; é necessário desinstalá-lo manualmente. Se planeja proteger o servidor novamente, você pode ignorar a desinstalação do serviço de mobilidade.

## <a name="delete-the-resources"></a>Excluir os recursos

1. Vá até o **cofre dos Serviços de Recuperação**
2. Clique em **Excluir**
3. Prossiga com a exclusão de todos os outros recursos identificados na [etapa anterior](#identify-the-resources-that-were-used-by-azure-site-recovery)
 
## <a name="move-azure-vms-to-the-new-target-region"></a>Mover VMs do Azure para a nova região de destino

Siga as etapas mencionadas abaixo com base em suas necessidades para mover as VMs do Azure para a região de destino.

- [Mover as VMs do Azure para outra região](azure-to-azure-tutorial-migrate.md)
- [Mover as VMs do Azure para Zonas de Disponibilidade](move-azure-VMs-AVset-Azone.md)

## <a name="re-set-up-site-recovery-based-on-the-new-source-region-for-the-vms"></a>Configurar novamente o Site Recovery com base na nova região de origem para as VMs

Configure a recuperação de desastre para as VMs do Azure que foram movidas para a nova região usando as etapas mencionadas [aqui](azure-to-azure-tutorial-enable-replication.md)
