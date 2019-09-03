---
author: erhopf
ms.service: cognitive-services
ms.topic: include
ms.date: 08/06/2019
ms.author: erhopf
ms.openlocfilehash: 156486f4f4f0df3d4bb4ab76492709bbecfb8eb5
ms.sourcegitcommit: beb34addde46583b6d30c2872478872552af30a1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69906481"
---
## <a name="set-up"></a>Configurar

### <a name="create-a-translator-text-resource"></a>Criar um recurso de Tradução de Texto

Os Serviços Cognitivos do Azure são representados por recursos do Azure que você assina. Crie um recurso de Tradução de Texto usando o [portal do Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ou a [CLI do Azure](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account-cli) em seu computador local. Você também pode:

* Obter uma [chave de avaliação](https://azure.microsoft.com/try/cognitive-services) válida por 7 dias gratuitamente. Depois de se inscrever, ela estará disponível no site do Azure.
* Exiba um recurso existente no [portal do Azure](https://portal.azure.com/).

Depois de obter uma chave do recurso ou da assinatura de avaliação, crie duas [variáveis de ambiente](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication):

* `TRANSLATOR_TEXT_SUBSCRIPTION_KEY` – A chave de assinatura do seu recurso de Tradução de Texto.
* `TRANSLATOR_TEXT_ENDPOINT` – O ponto de extremidade regional ou o nome de subdomínio personalizado do seu recurso.
