---
title: Suporte do Azure para VMs da geração 2 (versão prévia) | Microsoft Docs
description: Visão geral do suporte do Azure para VMs da geração 2
services: virtual-machines-linux
documentationcenter: ''
author: laurenhughes
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/23/2019
ms.author: lahugh
ms.openlocfilehash: 653d4baa89e28255f11df1c5e2d813d37535793a
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67667556"
---
# <a name="support-for-generation-2-vms-preview-on-azure"></a>Suporte para geração 2 VMs (visualização) no Azure

> [!IMPORTANT]
> Suporte do Azure para VMs da geração 2 está atualmente em visualização. Esta versão de visualização é fornecida sem um contrato de nível de serviço, e não é recomendado para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos.
> Para obter mais informações, consulte [termos de uso complementares para visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

Suporte para máquinas virtuais de geração 2 (VMs) agora está disponível na visualização no Azure. Você não pode alterar a geração de uma máquina virtual depois que ela for criada, portanto, examine as considerações nesta página antes de escolher uma geração. 

VMs de geração 2 oferecem suporte a principais recursos que não têm suporte em máquinas virtuais de geração 1. Esses recursos incluem aumento de memória, extensões de proteção de Software do Intel (Intel SGX) e virtualizado memória persistente (vPMEM). VMs de geração 2 também têm alguns recursos que ainda não têm suporte no Azure. Para obter mais informações, consulte o [recursos e funcionalidades](#features-and-capabilities) seção.

VMs de geração 2 usam a nova arquitetura de inicialização com base em UEFI, em vez da arquitetura com base em BIOS usado pela geração 1 VMs. Em comparação com VMs de geração 1, VMs de geração 2 podem ter melhor tempos de inicialização e instalação. Para obter uma visão geral das VMs da geração 2 e algumas das diferenças entre a geração 1 e geração 2, consulte [devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

## <a name="generation-2-vm-sizes"></a>Tamanhos de VM de geração 2

VMs de geração 1 têm suporte por todos os tamanhos VM no Azure. O Azure agora oferece suporte a geração 2 versão prévia para a seguinte série VM selecionada:

* [Série Dsv2](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general#dsv2-series) e [série Dsv3](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general#dsv3-series-1)
* [Esv3-series](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory#esv3-series)
* [Fsv2-series](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-compute#fsv2-series-1)
* [Série GS](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-previous-gen#gs-series)
* [Série ls](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-previous-gen#ls-series) e [Lsv2-series](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-storage#lsv2-series)
* [Mv2-series](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory#mv2-series)

## <a name="generation-2-vm-images-in-azure-marketplace"></a>Imagens VM de geração 2 no Azure Marketplace

As seguintes imagens do Marketplace dão suporte a VMs da geração 2:

* Windows Server 2019 Datacenter
* Windows Server 2016 Datacenter
* Windows Server 2012 R2 Datacenter
* Windows Server 2012 Datacenter

## <a name="on-premises-vs-azure-generation-2-vms"></a>No local vs. VMs da geração do Azure 2

Azure atualmente não suporta alguns dos recursos que local dá suporte a Hyper-V para VMs da geração 2.

| Recurso de geração 2                | Local do Hyper-V | Azure |
|-------------------------------------|---------------------|-------|
| Inicialização segura                         | :heavy_check_mark:  | :x:   |
| VM blindada                         | :heavy_check_mark:  | :x:   |
| vTPM                                | :heavy_check_mark:  | :x:   |
| Segurança baseada em virtualização (VBS) | :heavy_check_mark:  | :x:   |
| Formato VHDX                         | :heavy_check_mark:  | :x:   |

## <a name="features-and-capabilities"></a>Recursos e funcionalidades

### <a name="generation-1-vs-generation-2-features"></a>1ª geração versus recursos de geração 2

| Recurso | Geração 1 | Geração 2 |
|---------|--------------|--------------|
| Inicializar             | PCAT                      | UEFI                               |
| Controladores de disco | IDE                       | SCSI                               |
| Tamanhos de VM         | Todos os tamanhos VM | Apenas as VMs que oferecem suporte a armazenamento premium |

### <a name="generation-1-vs-generation-2-capabilities"></a>1ª geração versus recursos de geração 2

| Recurso | Geração 1 | Geração 2 |
|------------|--------------|--------------|
| > 2 TB de disco do sistema operacional                    | :x:                        | :heavy_check_mark: |
| Personalizado/imagem/troca de disco do sistema operacional         | :heavy_check_mark:         | :heavy_check_mark: |
| Suporte do conjunto de dimensionamento de máquina virtual | :heavy_check_mark:         | :heavy_check_mark: |
| ASR/backup                        | :heavy_check_mark:         | :x:                |
| Galeria de imagens compartilhadas              | :heavy_check_mark:         | :x:                |
| Criptografia de disco do Azure             | :heavy_check_mark:         | :x:                |

## <a name="creating-a-generation-2-vm"></a>Criação de uma geração 2 VM

### <a name="marketplace-image"></a>Imagem do Marketplace

No portal do Azure ou CLI do Azure, você pode criar geração 2 VMs de uma imagem do Marketplace que dá suporte à inicialização UEFI.

O `windowsserver-gen2preview` oferta contém somente imagens do Windows geração 2. Esse empacotamento evita a confusão entre a geração 1 e imagens de geração 2. Para criar uma geração 2 VM, selecione **imagens** desta oferta e siga o processo padrão para criar a VM.

Atualmente, o Marketplace oferece a geração seguinte do Windows 2 imagens:

* 2019-datacenter-gen2
* 2016-datacenter-gen2
* 2012-r2-datacenter-gen2
* 2012-datacenter-gen2

Consulte a [recursos e funcionalidades](#features-and-capabilities) seção para obter uma lista atual das imagens do Marketplace com suporte.

### <a name="managed-image-or-managed-disk"></a>Imagem gerenciada ou o disco gerenciado

Você pode criar uma geração 2 VM por meio de uma imagem gerenciada ou um disco gerenciado da mesma forma, você criaria uma geração 1 VM.

### <a name="virtual-machine-scale-sets"></a>conjuntos de escala de máquina virtual

Você também pode criar geração 2 VMs usando conjuntos de dimensionamento de máquina virtual. Na CLI do Azure, use a escala do Azure define para criar a geração 2 VMs.

## <a name="frequently-asked-questions"></a>Perguntas frequentes

* **São a geração 2 VMs disponíveis em todas as regiões do Azure?**  
    Sim. Mas nem todos os [tamanhos de VM de geração 2](#generation-2-vm-sizes) estão disponíveis em todas as regiões. A disponibilidade da geração que 2 VM depende da disponibilidade do tamanho de VM.

* **Há uma diferença de preço entre a geração 1 e geração 2 VMs?**  
    Nº

* **Como posso aumentar o tamanho do disco do sistema operacional?**  
  Discos do sistema operacional maiores que 2 TB são novos para a geração 2 VMs. Por padrão, os discos do sistema operacional são menores que 2 TB para VMs da geração 2. Você pode aumentar o tamanho do disco até o máximo recomendado de 4 TB. Use a CLI do Azure ou o portal do Azure para aumentar o tamanho do disco do sistema operacional. Para obter informações sobre como expandir discos programaticamente, consulte [redimensionar o disco](expand-disks.md).

  Para aumentar o tamanho do disco do sistema operacional do portal do Azure:

  1. No portal do Azure, vá para a página de propriedades da VM.
  1. Para desligar e desalocar a máquina virtual, selecione a **parar** botão.
  1. No **discos** , selecione o disco do sistema operacional que você deseja aumentar.
  1. No **discos** seção, selecione **Configuration**e atualize o **tamanho** para o valor desejado.
  1. Volte para a página de propriedades da VM e **iniciar** a VM.

  Você pode ver um aviso para discos do sistema operacional maiores que 2 TB. O aviso não se aplica a VMs da geração 2. No entanto, os tamanhos de disco do sistema operacional maiores que 4 TB são *não recomendável.*

* **Fazer a geração 2 VMs dão suporte a rede acelerada?**  
    Sim. Para obter mais informações, consulte [criar uma VM com rede acelerada](../../virtual-network/create-vm-accelerated-networking-cli.md).

* **Há suporte para VHDX na geração 2?**  
    Não, as VMs de geração 2 suportam apenas VHD.

* **VMs de geração 2 dá suporte a armazenamento em disco Ultra do Azure?**  
    Sim.

* **Posso migrar uma máquina virtual de geração 1 para a geração 2?**  
    Não, você não pode alterar a geração de uma máquina virtual depois de criá-lo. Se você precisar alternar entre gerações de VM, crie uma nova VM de uma geração diferente.

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre [máquinas virtuais de geração 2 no Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).
