---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 2ae72045ae18d84eac2a6d619d94e3a9e49415ae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66160213"
---
## <a name="issue-custom-image-provisioning-errors"></a>Problema: Imagem personalizada; erros de provisionamento
Os erros de provisionamento surgirão se você carregar ou capturar uma imagem de VM generalizada como uma imagem de VM especializada ou vice-versa. O primeiro causará um erro de tempo limite de provisionamento e o último causará uma falha de provisionamento. Para implantar a imagem personalizada sem erros, você deve garantir que o tipo da imagem não seja alterado durante o processo de captura.

A tabela a seguir lista as combinações possíveis de imagens especializadas e generalizadas, o tipo de erro que será exibido e o que você precisa fazer para corrigir os erros.

