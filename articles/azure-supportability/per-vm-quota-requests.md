---
title: Aumentar a cota de vCPU do Azure por VM solicitações | Microsoft Docs
description: solicitações de aumento por cota de vCPU VM
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 79879cd7f5ea5af1b794735f32e6e1367458e124
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67310721"
---
# <a name="vm-series-vcpu-limit-increase"></a>Aumento de limite de vCPU da série de VM

Cotas de vCPU do Resource Manager para conjuntos de dimensionamento de máquinas virtuais e máquinas virtuais são aplicadas em duas camadas para cada assinatura, em cada região. 

A primeira camada é a **limite de vCPUs regionais totais** (em todas as séries de VM), e a segunda camada é a **por VM série vCPUs limitar** (por exemplo, os vCPUs da série D). Sempre que uma nova VM deve ser implantada, a soma do uso de vCPUs novos e existentes para essa série de VM não deve exceder a cota de vCPU aprovada para essa série específica de VM. Além disso, a contagem de vCPU total de novas e existentes implantada em todas as séries de VM não deve exceder a cota de vCPUs regionais totais aprovada para a assinatura. Se qualquer uma das cotas é excedida, a implantação de VM não será permitida.
Você pode solicitar um aumento de limite de cota de vCPUs para a série VM do portal do Azure. Um aumento na cota de séries de VM automaticamente aumenta o limite de vCPUs regionais totais no mesmo valor. 

Quando uma nova assinatura é criada, os vCPUs regionais totais do padrão pode não ser iguais à soma das cotas de vCPU de padrão para todas as séries de VM individuais. Isso pode resultar em uma assinatura com a cota suficiente para cada série de VM individuais que você deseja implantar, mas não há cota suficiente de vCPUs regionais totais para todas as implantações. Nesse caso, você precisará enviar uma solicitação para aumentar o limite de vCPUs regionais totais explicitamente. Limite total de vCPUs regionais não pode exceder a soma da cota aprovada em todas as séries VM para a região.

Saiba mais sobre cotas na [página de cotas de vCPU de máquina Virtual](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) e [assinatura do Azure e limites de serviço](https://aka.ms/quotalimits) página. 

Agora você pode solicitar um aumento pela **ajuda + suporte** folha ou o **usos + cota** folha no portal. 

## <a name="request-per-vm-series-vcpu-quota-increase-at-subscription-level-using-the-help--support-blade"></a>Solicitar por aumento de cota de vCPU de VM série no nível de assinatura usando o **ajuda + suporte** folha

Siga as instruções abaixo para criar uma solicitação de suporte por meio da folha de 'Ajuda + suporte' do Azure disponível no portal do Azure. 

1. Partir https://portal.azure.com, selecione **ajuda + suporte**.

![Ajuda + suporte](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Selecione **Nova solicitação de suporte**. 

![Nova solicitação de suporte](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. No menu suspenso tipo de problema, escolha **limites de serviço e assinatura (cotas)** .

![Lista suspensa de tipo de problema](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Selecione a assinatura que precisa de uma cota maior.

![Selecione a assinatura newSR](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Selecione **aumenta o limite de assinatura - VM (núcleos vCPUs) de computação** na **tipo de cota** lista suspensa. 

![Selecione o tipo de cota](./media/resource-manager-core-quotas-request/select-quota-type.png)

6. Na **detalhes do problema**, forneça informações adicionais para ajudar a processar sua solicitação clicando **fornecem detalhes**.

![Forneça detalhes](./media/resource-manager-core-quotas-request/provide-details.png)

7. No **detalhes da cota** painel, selecione o modelo de implantação e selecione um local.

![Detalhes da cota DM](./media/resource-manager-core-quotas-request/quota-details.png)

8. Selecione o **famílias de SKU** que exigem um aumento. 

![Família de SKU](./media/resource-manager-core-quotas-request/sku-family.png)

9. Insira os novos limites desejados na assinatura. Para remover uma linha, desmarque o SKU no menu suspenso de família de SKU ou clique no ícone "x" de descarte. Depois de inserir a cota desejada para cada família SKU, clique em **salvar e continuar** no painel de detalhes da cota para continuar com a criação de solicitação de suporte.

![Novos limites](./media/resource-manager-core-quotas-request/new-limits.png)


## <a name="request-per-vm-series-vcpu-quota-increase-at-subscription-level-using-usages--quota-blade"></a>Solicitação por aumento de cota de vCPU de VM série no nível de assinatura usando **usos + cota** folha

Siga as instruções abaixo usar para criar uma solicitação de suporte por meio do Azure "uso + cota" folha disponível no portal do Azure. 

1. Em https://portal.azure.com, selecione **Assinaturas**.

![Assinaturas](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Selecione a assinatura que precisa de uma cota maior.

![Escolha a assinatura](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Selecione **Uso + cotas**

![Selecione uso e cotas](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. No canto superior direito, selecione **Solicitar Aumento**.

![Solicitar Aumento](./media/resource-manager-core-quotas-request/request-increase.png)

5. Selecione **aumenta o limite de assinatura (núcleos vCPUs) de VM de computação** como o tipo de aspas. 

![Preencher o formulário](./media/resource-manager-core-quotas-request/forms.png)
   
6. No **detalhes da cota** painel, selecione o modelo de implantação e selecione um local.

![Folha Problema de Cota](./media/resource-manager-core-quotas-request/problemstep.png)

7. Selecione o **famílias de SKU** que exigem um aumento.

![Série de SKU selecionada](./media/resource-manager-core-quotas-request/sku-family.png)

8. Insira os novos limites desejados na assinatura. Para remover uma linha, desmarque o SKU no menu suspenso de família de SKU ou clique no ícone "x" de descarte. Depois de inserir a cota desejada para cada família SKU, clique em **salvar e continuar** na página de etapa do problema para continuar com a criação de solicitação de suporte.

![Nova solicitação de cota do SKU](./media/resource-manager-core-quotas-request/new-limits.png)

