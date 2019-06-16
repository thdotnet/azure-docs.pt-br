---
title: Solicitações de aumento de cota regional do Azure | Microsoft Docs
description: Solicitações de aumento de cota regional
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 4c5524cf450959db7055ca5d032c81f79ebac077
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67083074"
---
# <a name="total-regional-vcpu-limit-increase"></a>Aumento de limite de vCPU regional total 

Agora você pode solicitar um aumento pela **ajuda + suporte** folha ou o **usos + cota** folha no portal. 

## <a name="request-total-regional-vcpus-quota-increase-at-subscription-level-using-the-help--support-blade"></a>Solicitar aumento de cota de vCPUs regionais totais no nível de assinatura usando o **ajuda + suporte** folha

Siga as instruções abaixo para criar uma solicitação de suporte por meio da folha de 'Ajuda + suporte' do Azure disponível no portal do Azure. 

1. Partir https://portal.azure.com, selecione **ajuda + suporte**.

![Ajuda + suporte](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Selecione **Nova solicitação de suporte**. 

![Nova solicitação de suporte](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. No menu suspenso tipo de problema, escolha **limites de serviço e assinatura (cotas)** .

![Lista suspensa de tipo de problema](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Selecione a assinatura que precisa de uma cota maior.

![Selecione a assinatura newSR](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Selecione **Other Requests** na **tipo de cota** lista suspensa.

![QuotaType](./media/resource-manager-core-quotas-request/regional-quotatype.png)

6. Na **detalhes** painel, fornecem informações adicionais, de acordo com o exemplo a seguir para ajudar a processar sua solicitação e continuar com a criação de casos. 
    1.  **Modelo de implantação** – especifique o ' Gerenciador de recursos '
    2.  **Região solicitada** – especifique sua região necessária, por exemplo, Leste dos EUA 2
    3.  **Novo valor de limite** – especificar novo limite de região. Isso não deve exceder a soma da cota aprovada de famílias de SKU individuais para essa assinatura

![QuotaDetails](./media/resource-manager-core-quotas-request/regional-details.png)

## <a name="request-total-regional-vcpus-quota-increase-at-subscription-level-using-the-usages--quota-blade"></a>Solicitar aumento de cota de vCPUs regionais totais no nível de assinatura usando o **usos + cota** folha

Siga as instruções abaixo usar para criar uma solicitação de suporte por meio do Azure "uso + cota" folha disponível no portal do Azure. 

1. Em https://portal.azure.com, selecione **Assinaturas**.

![Assinaturas](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Selecione a assinatura que precisa de uma cota maior.

![Escolha a assinatura](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Selecione **Uso + cotas**

![Selecione uso e cotas](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. No canto superior direito, selecione **Solicitar Aumento**.

![Solicitar Aumento](./media/resource-manager-core-quotas-request/request-increase.png)

5. Selecione **Other Requests** na **tipo de cota** lista suspensa.

![QuotaType](./media/resource-manager-core-quotas-request/regional-quotatype.png)

6. Na **detalhes** painel, fornecem informações adicionais, de acordo com o exemplo a seguir para ajudar a processar sua solicitação e continuar com a criação de casos. 
    1.  **Modelo de implantação** – especifique o ' Gerenciador de recursos '
    2.  **Região solicitada** – especifique sua região necessária, por exemplo, Leste dos EUA 2
    3.  **Novo valor de limite** – especificar novo limite de região. Isso não deve exceder a soma da cota aprovada de famílias de SKU individuais para essa assinatura

![QuotaDetails](./media/resource-manager-core-quotas-request/regional-details.png)



