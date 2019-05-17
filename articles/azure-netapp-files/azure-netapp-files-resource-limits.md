---
title: Limites de recursos no Azure NetApp Files | Microsoft Docs
description: Descreve os limites de recursos do Azure NetApp Files, incluindo limites para contas do NetApp, pools de capacidade, volumes, instantâneos e a sub-rede delegada.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: b-juche
ms.openlocfilehash: b55467d77beb8f97b8e392b72682268ae0407e54
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65826373"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Limites de recursos no Azure NetApp Files

O reconhecimento dos limites de recursos no Azure NetApp Files ajuda você a gerenciar seus volumes.

## <a name="resource-limits"></a>Limites de recursos

A tabela a seguir descreve os limites de recursos para arquivos do NetApp do Azure:

|  Resource  |  Limite padrão  |  Ajustáveis por intermédio de solicitação de suporte  |
|----------------|---------------------|--------------------------------------|
|  Número de contas do NetApp por assinatura do Azure   |  10    |  Sim   |
|  Número de pools de capacidade por conta do NetApp   |    25     |   Sim   |
|  Número de volumes por pool de capacidade     |    500   |    Sim     |
|  Número de instantâneos por volume       |    255     |    Não        |
|  Número de sub-redes delegada aos arquivos de NetApp (Microsoft.NetApp/volumes) do Azure por rede Virtual do Azure    |   1   |    Não    |
|  Número máximo de VMs (inclui VNets emparelhadas) que pode se conectar a um volume     |    1000   |    Não   |
|  Tamanho mínimo de um pool de capacidade de único   |  4 TiB     |    Não  |
|  Tamanho máximo de um pool de capacidade de único    |  500 TiB   |   Não   |
|  Tamanho mínimo de um único volume    |    100 GiB    |    Não    |
|  Máximo atribuído a cota de um único volume *   |   92 TiB   |    Não   |
|  Tamanho máximo de um único volume *     |    100 TiB    |    Não       |

* Um volume pode ser criado manualmente ou redimensionado para TiB 92 máximo. No entanto, um volume pode crescer até 100 TiB em um cenário de excedente. Ver [modelo de custo para arquivos do Azure NetApp](azure-netapp-files-cost-model.md) para obter detalhes sobre o excedente de capacidade. 

## <a name="request-limit-increase"></a>Aumento de limite de solicitação 

Você pode criar uma solicitação de suporte do Azure para aumentar os limites ajustáveis da tabela acima. 

Plano de navegação do portal do Azure: 

1. Clique em **ajuda + suporte**.
2. Clique em **+ novo solicitação de suporte**.
3. Na guia de Noções básicas, forneça as seguintes informações: 
    1. Tipo de problema: Selecione **limites de serviço e assinatura (cotas)**.
    2. Assinaturas: Selecione a assinatura para o recurso que você precisa que a cota aumentada.
    3. Tipo de cota: Selecione **armazenamento: Limita os arquivos do Azure do NetApp**.
    4. Clique em **Avançar: Soluções**.
4. Na guia Detalhes:
    1. Na caixa de descrição, forneça as seguintes informações para o tipo de recurso correspondente:

        |  Resource  |    Recursos do pai      |    Novos limites solicitados     |    Motivo para aumento de cota       |
        |----------------|------------------------------|---------------------------------|------------------------------------------|
        |  Conta |  *ID da assinatura*   |  *Novo máximo solicitado **conta** número*    |  *Qual cenário ou caso de uso solicitado da solicitação?*  |
        |  Pool    |  *ID de assinatura, o URI da conta*  |  *Novo máximo solicitado **pool** número*   |  *Qual cenário ou caso de uso solicitado da solicitação?*  |
        |  Volume  |  *ID da assinatura, URI, da conta de Pool de URI*   |  *Novo máximo solicitado **volume** número*     |  *Qual cenário ou caso de uso solicitado da solicitação?*  |

    2. Especifique o suporte ao método apropriado e forneça suas informações de contrato.

    3. Clique em **Avançar: Revisar + criar** para criar a solicitação. 


## <a name="next-steps"></a>Próximas etapas  

- [Compreender a hierarquia de armazenamento do Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
- [Modelo de custo para arquivos do Azure NetApp](azure-netapp-files-cost-model.md)
