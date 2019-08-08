---
title: Início rápido da Solução VMware no Azure pela CloudSimple – Consumir VMs do VMware no Azure
description: Neste início rápido, você aprenderá a configurar e consumir as VMs do VMware no portal do Azure usando a solução VMware no Azure da CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/11/2019
ms.topic: quickstart
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: dd4faf6df54d478654c59ffc18bf8c5873d576b9
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816660"
---
# <a name="quickstart-consume-vmware-vms-on-azure"></a>Início Rápido: Consumir VMs do VMware no Azure

Para criar uma máquina virtual no portal do Azure, você pode usar modelos de máquina virtual disponíveis em sua nuvem privada vCenter. Um administrador do vCenter pode criar modelos adicionais na nuvem privada.

## <a name="create-a-vm-template"></a>Criar um modelo de VM

Primeiro, crie uma máquina virtual em sua nuvem privada usando a interface do usuário do vCenter. Para criar um modelo, siga as instruções no artigo da VMware [Clonar uma máquina virtual em um modelo no cliente Web do vSphere](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-FE6DE4DF-FAD0-4BB0-A1FD-AFE9A40F4BFE.html). Armazene o modelo de VM em sua nuvem privada do vCenter.

## <a name="create-a-virtual-machine-in-the-azure-portal"></a>Criar uma máquina virtual no portal do Azure

1. Selecione **Todos os serviços**.

2. Pesquise **Máquinas Virtuais do CloudSimple**.

3. Selecione **Adicionar**.

    ![Selecione Adicionar](media/create-cloudsimple-virtual-machine.png)

4. Insira as informações a seguir sobre a máquina virtual e selecione **Avançar: Tamanho**.

    ![Criar Máquina Virtual do CloudSimple – Noções básicos](media/create-cloudsimple-virtual-machine-basic-info.png)

    | Campo | DESCRIÇÃO |
    | ------------ | ------------- |
    | **Assinatura** | A assinatura do Azure associada à sua nuvem privada.  |
    | **Grupo de recursos** | O grupo de recursos ao qual a VM foi atribuída. Selecione um grupo existente ou crie um novo. |
    | **Nome** | Um nome para identificar a VM.  |
    | **Localidade** | A região do Azure na qual a VM está hospedada.  |
    | **Nuvem privada** | A nuvem privada do CloudSimple na qual você deseja criar a VM. |
    | **ResourcePool** | Um pool de recursos mapeado para a VM. Selecione entre os pools de recursos disponíveis. |
    | **Modelo do vSphere** | O modelo do vSphere para a VM.  |
    | **Nome de usuário** | O nome de usuário do administrador VM (para modelos do Windows).|
    | **Senha** |  A senha do administrador da VM (para modelos do Windows). |
    | **Confirmar senha** | A senha que você forneceu no campo anterior. |

5. Selecione o número de núcleos e a capacidade de memória da VM. Selecione **Expor ao Sistema Operacional Convidado** se você quiser expor a virtualização total da CPU para o sistema operacional convidado. Os aplicativos que exigem a virtualização de hardware podem ser executados em máquinas virtuais sem conversão binária ou de paravirtualização. Saiba mais no artigo da VMware <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A98801C-68E8-47AF-99ED-00C63E4857F6.html" target="_blank">Expor virtualização assistida de hardware do VMware</a>. Quando terminar, selecione **Avançar: Configurações**.

    ![Criar Máquina Virtual do CloudSimple – Tamanho](media/create-cloudsimple-virtual-machine-size.png)

6. Configure discos e adaptadores de rede, conforme descrito nas tabelas a seguir e selecione **Revisar + criar**.

    ![Criar Máquina Virtual do CloudSimple – Configurações](media/create-cloudsimple-virtual-machine-configurations.png)

    Para adaptadores de rede, selecione **Adicionar adaptador de rede** e defina as seguintes configurações:
    
    | Configuração | DESCRIÇÃO |
    | ------------ | ------------- |
    | **Nome** | Insira um nome para identificar o adaptador.  |
    | **Rede** | Selecione na lista de grupos de porta distribuída configurado no vSphere de sua nuvem privada.  |
    | **Adaptador** | Selecione um adaptador do vSphere na lista de tipos disponíveis configurados para a VM. Saiba mais no artigo da VMware <a href="https://kb.vmware.com/s/article/1001805" target="_blank">Escolher um adaptador de rede para sua máquina virtual</a>. |
    | **Ligar na inicialização** | Escolha se quer habilitar o hardware da NIC quando a VM for inicializada. O padrão é **Habilitar**. |

    Para discos, selecione **Adicionar disco** e defina as seguintes configurações:

    | Configuração | DESCRIÇÃO |
    | ------------ | ------------- |
    | **Nome** | Insira um nome para identificar o disco.  |
    | **Tamanho** | Selecione um dos tamanhos disponíveis.  |
    | **Controlador de SCSI** | Selecione um controlador SCSI para o disco.  |
    | **Modo** | O modo especifica como o disco participa de instantâneos. Escolha uma destas opções: <br> **Persistente independente**: Todas as alterações gravadas no disco são gravadas permanentemente.<br> **Independente não persistente**: As alterações gravadas no disco são descartadas quando você desliga ou redefine a máquina virtual. O modo independente não persistente permite que você sempre reinicie a máquina virtual no mesmo estado. Saiba mais na <a href="https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-8B6174E6-36A8-42DA-ACF7-0DA4D8C5B084.html" target="_blank">documentação da VMware</a>.

7. Após a conclusão da validação, examine as configurações e selecione **Criar**. Para fazer alterações, selecione as guias na parte superior ou selecione **Anterior: Configurações**.

    ![Criar Máquina Virtual do CloudSimple – Revisar + Criar](media/create-cloudsimple-virtual-machine-review.png)

## <a name="next-steps"></a>Próximas etapas

* [Exibir lista de máquinas virtuais do CloudSimple](https://docs.azure.cloudsimple.com/azure-create-vm/#view-list-of-cloudsimple-virtual-machines)
* [Gerenciar máquinas de virtuais CloudSimple no Azure](https://docs.azure.cloudsimple.com/azure-manage-vm/)
