---
title: VMs da geração 2 (versão prévia) no Azure | Microsoft Docs
description: Visão geral das VMs da geração do Azure 2
services: virtual-machines-linux
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/16/2019
ms.author: lahugh
ms.openlocfilehash: e6bb947503371e379e4d4972ddfc3614e129174b
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65835204"
---
# <a name="generation-2-vms-preview-on-azure"></a>VMs da geração 2 (versão prévia) no Azure

> [!IMPORTANT]
> VMs de geração 2 estão atualmente em visualização pública.
> Essa versão prévia é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Suporte para máquinas virtuais de geração 2 (VMs) agora está disponível em visualização pública no Azure. Você não pode alterar a geração de uma máquina virtual depois que você criou. Portanto, é recomendável que você examine as considerações [aqui](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v) , bem como as informações nesta página antes de escolher uma geração.

Recursos principais de suporte de VMs geração 2, como: maior memória, Intel® Software Guard extensões (SGX) e a memória virtual persistente (vPMEM), que não têm suporte em VMs de geração 1. VMs de geração 2 têm alguns recursos que ainda não têm suporte no Azure. Para obter mais informações, consulte o [recursos e funcionalidades](#features-and-capabilities) seção. 

VMs da geração 2 usam os novo vs de arquitetura de inicialização com base em UEFI a arquitetura com base em BIOS usada pelas VMs de geração 1. Em comparação com VMs de geração 1, máquinas virtuais de 2ª geração podem ter um melhor tempos de inicialização e instalação. Para obter uma visão geral das VMs da geração 2 e algumas das principais diferenças entre a geração 1 e geração 2, consulte [devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

## <a name="generation-2-vm-sizes"></a>Tamanhos de VM de geração 2

VMs de geração 1 têm suporte por todos os tamanhos VM no Azure. O Azure agora oferece suporte de geração 2 para a seguinte série VM selecionada na visualização pública:

* [Dsv2](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general#dsv2-series) e [série Dsv3](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general#dsv3-series-1)
* [Esv3-series](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory#esv3-series)
* [Fsv2-series](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-compute#fsv2-series-1)
* [Série GS](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory#gs-series)
* [Série ls](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-previous-gen#ls-series) e [Lsv2-series](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-storage#lsv2-series)
* [Mv2-series](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory#mv2-series)

## <a name="generation-2-vm-images-in-azure-marketplace"></a>Imagens VM de geração 2 no Azure Marketplace

As seguintes imagens do Marketplace do Azure dão suporte a VMs da geração 2:

* Servidor Windows 2019 Datacenter
* Servidor Windows 2016 Datacenter
* Servidor Windows 2012 R2 Datacenter
* Servidor Windows 2012 Datacenter

## <a name="on-premises-vs-azure-generation-2-vms"></a>No local vs VMs da geração do Azure 2

Alguns dos recursos que on-premises Hyper-V dá suporte para VMs da geração 2 não oferece suporte atualmente com Azure.

| Recurso de geração 2                | Local do Hyper-V | Azure |
|-------------------------------------|---------------------|-------|
| Inicialização segura                         | :heavy_check_mark:  | :x:   |
| VM blindada                         | :heavy_check_mark:  | :x:   |
| vTPM                                | :heavy_check_mark:  | :x:   |
| Segurança baseada em virtualização (VBS) | :heavy_check_mark:  | :x:   |
| Formato VHDX                         | :heavy_check_mark:  | :x:   |

## <a name="features-and-capabilities"></a>Recursos e funcionalidades

### <a name="generation-1-vs-generation-2-features"></a>Recursos de geração 2 do vs de geração 1

| Recurso | Geração 1 | Geração 2 |
|---------|--------------|--------------|
| Inicializar             | PCAT                      | UEFI                               |
| Controladores de disco | IDE                       | SCSI                               |
| Tamanhos de VM         | Disponível em todos os tamanhos VM | O armazenamento Premium VMs com suporte apenas |

### <a name="generation-1-vs-generation-2-capabilities"></a>Recursos de geração 2 do vs de geração 1

| Recurso | Geração 1 | Geração 2 |
|------------|--------------|--------------|
| > 2 TB de disco do sistema operacional                    | :x:                        | :heavy_check_mark: |
| Sistema operacional de troca/de imagem de disco personalizada         | :heavy_check_mark:         | :heavy_check_mark: |
| Suporte do conjunto de dimensionamento de máquina virtual | :heavy_check_mark:         | :heavy_check_mark: |
| ASR/Backup                        | :heavy_check_mark:         | :x:                |
| Galeria de Imagens Compartilhadas              | :heavy_check_mark:         | :x:                |
| Azure Disk Encryption             | :heavy_check_mark:         | :x:                |

## <a name="creating-a-generation-2-vm"></a>Criação de uma geração 2 VM

### <a name="marketplace-image"></a>Imagem do Marketplace

VMs de geração 2 podem ser criadas de uma imagem do marketplace (que dá suporte à inicialização de UEFI) por meio do portal do Azure ou a CLI do Azure.

O `windowsserver-gen2preview` oferta contém somente imagens do Windows geração 2. Isso evita confusão em relação a imagens de geração 2 do vs de geração 1. Para criar a geração 2 VMs, selecione **imagens** desta oferta e siga o processo de criação de VM padrão.

Atualmente, a geração seguinte do Windows 2 imagens são publicadas no Azure Marketplace:

* 2019-datacenter-gen2
* 2016-datacenter-gen2
* 2012-r2-datacenter-gen2
* 2012-datacenter-gen2

Consulte a seção de recursos para obter uma lista de imagens do marketplace com suporte como continuaremos adicionando imagens adicionais que dão suporte à geração 2.

### <a name="managed-image-or-managed-disk"></a>Imagem gerenciada ou o disco gerenciado

Geração 2 VMs podem ser criadas de imagem gerenciada ou o disco gerenciado da mesma forma, você criaria uma geração 1 VM.

### <a name="virtual-machine-scale-sets"></a>conjuntos de escala de máquina virtual

Geração que 2 VMs também podem ser criadas usando conjuntos de dimensionamento de máquina virtual. Você pode criar a geração 2 VMs usando conjuntos de dimensionamento de máquina virtual do Azure por meio da CLI do Azure.

## <a name="frequently-asked-questions"></a>Perguntas frequentes

* **VMs de geração 2 têm suporte para a rede acelerada?**  
    Sim, o suporte a VMs geração 2 [rede acelerada](../../virtual-network/create-vm-accelerated-networking-cli.md).

* **. Vhdx tem suporte na geração 2?**  
    Não, apenas o. vhd tem suporte em VMs de geração 2.

* **VMs de geração 2 têm suporte para unidades de estado sólido Ultra (SSD)?**  
    Sim, as VMs da geração 2 suportam Ultra SSD.

* **Pode migrar de geração 1 para máquinas virtuais de 2ª geração?**  
    Não, é possível alterar a geração de uma VM depois que ela for criada. Se você precisar alternar entre gerações de VM, você precisa criar uma nova VM de uma geração diferente.

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre [máquinas virtuais de geração 2 no Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).