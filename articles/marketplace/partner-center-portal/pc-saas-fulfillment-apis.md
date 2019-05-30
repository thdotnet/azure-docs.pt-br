---
title: APIs de preenchimento SaaS | O Azure Marketplace
description: Apresenta as versões do cumprimento de APIs que permitem que você integre seu SaaS oferece com o Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: evansma
ms.openlocfilehash: 7896ed77d9dbb3358ddb1c809ca342828280f66a
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258918"
---
# <a name="saas-fulfillment-apis"></a>APIs de cumprimento de SaaS

As APIs de cumprimento de SaaS permitem que fornecedores de software independentes (ISVs) para integrar seus aplicativos SaaS com o Azure Marketplace. Essas APIs permitem que os aplicativos de ISV participar de todos os canais de comércio eletrônico: direto, conduzida pelo parceiro (revendedores) e o led de campo.  Eles são um requisito para listagem transacionáveis ofertas de SaaS no Azure Marketplace.

> [!WARNING]
> A versão atual dessa API é a versão 2, que deve ser usado para todas as novas ofertas de SaaS.  Versão 1 da API foi preterida e está sendo mantida para oferecer suporte a ofertas existentes.


## <a name="business-model-support"></a>Suporte ao modelo de negócios

Esta API dá suporte os seguintes recursos de modelo de negócios; É possível:

* Especifique vários planos para uma oferta. Esses planos têm funcionalidade diferente e podem ser preços diferentes.
* Fornecer uma oferta em um por site ou por usuário base do modelo de cobrança.
* Fornecer mensal e anual (paga antecipadamente) opções de cobrança.
* Fornece preços privada para um cliente com base em um contrato de negócios negociado.


## <a name="next-steps"></a>Próximas etapas

Se você ainda não fez isso, registre seu aplicativo SaaS na [portal do Azure](https://ms.portal.azure.com) conforme explicado nas [registrar um aplicativo do Azure AD](./pc-saas-registration.md).  Depois disso, use a versão mais recente dessa interface para o desenvolvimento: [Versão de API de preenchimento SaaS 2](./pc-saas-fulfillment-api-v2.md).
