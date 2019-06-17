---
title: Aumentar a cota de vCPU do Azure por VM solicitações | Microsoft Docs
description: solicitações de aumento por cota de vCPU VM
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: f921b4a95c1b0cfb29d84c0bacc17d268af6e6c5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082814"
---
# <a name="vm-series-vcpu-limit-increase"></a>Aumento de limite de vCPU de VM série

Agora você pode solicitar um aumento pela **ajuda + suporte** folha ou o **usos + cota** folha no portal. 

## <a name="request-per-vm-vcpu-quota-increase-at-subscription-level-using-the-help--support-blade"></a>Solicitar por aumento de cota de vCPU VM no nível de assinatura usando o **ajuda + suporte** folha

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


## <a name="request-per-vm-vcpu-quota-increase-at-subscription-level-using-usages--quota-blade"></a>Solicitação por aumento de cota de vCPU VM no nível de assinatura usando **usos + cota** folha

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

