---
title: Solicitações de aumento de cota de vCPU do Azure Resource Manager | Microsoft Docs
description: Solicitações de aumento de cota de vCPU do Azure Resource Manager
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 9a997af984b92ea59cc02d99fbd66d8967ca31bd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076821"
---
# <a name="quota-increase-requests"></a>Solicitações de um aumento de cota

Cotas de vCPU do Resource Manager para conjuntos de dimensionamento de máquinas virtuais e máquinas virtuais são aplicadas em duas camadas para cada assinatura, em cada região. 

A primeira camada é o limite de vCPUs regionais totais (em todas as séries de VM) e a segunda camada é o limite de vCPUs de séries de VM (por exemplo, os vCPUs da série D) por. Sempre que uma nova VM deve ser implantada, a soma do uso de vCPUs novos e existentes para essa série de VM não deve exceder a cota de vCPU aprovada para essa série específica de VM. Além disso, a contagem de vCPU total de novas e existentes implantada em todas as séries de VM não deve exceder a cota de vCPUs regionais totais aprovada para a assinatura. Se qualquer uma das cotas é excedida, a implantação de VM não será permitida.
Você pode solicitar um aumento de limite de cota de vCPUs para a série VM do portal do Azure. Um aumento na cota de séries de VM automaticamente aumenta o limite de vCPUs regionais totais no mesmo valor. 

Quando uma nova assinatura é criada, os vCPUs regionais totais do padrão pode não ser iguais à soma das cotas de vCPU de padrão para todas as séries de VM individuais. Isso pode resultar em uma assinatura com a cota suficiente para cada série de VM individuais que você deseja implantar, mas não há cota suficiente de vCPUs regionais totais para todas as implantações. Nesse caso, você precisará enviar uma solicitação para aumentar o limite de vCPUs regionais totais explicitamente. Limite total de vCPUs regionais não pode exceder a soma da cota aprovada em todas as séries VM para a região.

Saiba mais sobre cotas na [página de cotas de vCPU de máquina Virtual](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) e [assinatura do Azure e limites de serviço](https://aka.ms/quotalimits) página. 

