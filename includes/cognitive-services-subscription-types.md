---
author: aahill
ms.service: cognitive-services
ms.topic: include
ms.date: 06/24/2019
ms.author: aahi
ms.openlocfilehash: ad1527a5e7f1cb2ff1beb9ddace5460f41bb8a87
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461502"
---
## <a name="azure-cognitive-service-subscription-types"></a>Tipos de assinatura do Azure do serviço cognitivo

> [!NOTE]
> Proprietários de assinatura podem desabilitar a criação de contas de Serviços Cognitivos para grupos de recursos e inscrições, aplicando [política do Azure](https://docs.microsoft.com/azure/governance/policy/overview#policy-definition), atribuindo uma definição de política "Não permitido a tipos de recursos" e especificando **Microsoft.CognitiveServices / accounts** como o tipo de recurso de destino.

Você pode acessar os serviços Cognitivos do Azure por meio de duas assinaturas diferentes: Uma assinatura de vários serviço, ou um serviço único um. Essas assinaturas permitem que você se conectar a um único serviço ou com vários serviços ao mesmo tempo.

### <a name="multi-service-subscription"></a>Assinatura de vários serviços

>[!WARNING]
> No momento, estes serviços **não** têm suporte para chaves de vários serviços: O QnA Maker, serviços de fala, visão personalizada e Detector de anomalias.

Uma assinatura de vários serviço para os serviços Cognitivos do Azure permite que você use uma única assinatura e recursos do Azure para a maioria dos serviços Cognitivos do Azure e consolida de cobrança de serviços que você usa. Consulte [Preços dos Serviços Cognitivos](https://azure.microsoft.com/pricing/details/cognitive-services/) para obter mais informações.

### <a name="single-service-subscription"></a>Assinatura de serviço único

Uma assinatura em um único serviço, como pesquisa Visual computacional ou os serviços de fala. Uma assinatura de serviço único fica restrita ao recurso em questão. 
