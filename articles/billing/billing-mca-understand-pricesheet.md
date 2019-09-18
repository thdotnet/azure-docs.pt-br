---
title: Entender os termos na planilha de preços de um Contrato de Cliente da Microsoft – Azure
description: Saiba como ler e entender o uso e a fatura de um Contrato de Cliente da Microsoft.
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/01/2019
ms.author: banders
ms.openlocfilehash: 4d83228fbec395d604e5ce3f988d2a6157f21eed
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2019
ms.locfileid: "67490657"
---
# <a name="terms-in-your-microsoft-customer-agreement-price-sheet"></a>Termos na planilha de preços do Contrato de Cliente da Microsoft

Este artigo se aplica a uma conta de cobrança do Azure para um Contrato de Cliente da Microsoft. [Verifique se você tem acesso a um Contrato de Cliente da Microsoft](#check-access-to-a-microsoft-customer-agreement).

Se você for o proprietário, o colaborador, o leitor ou o gerenciador de faturas de um perfil de cobrança, baixe a planilha de preços de sua organização no portal do Azure. Confira [Exibir e baixar o preço de sua organização](billing-ea-pricing.md).

## <a name="terms-and-descriptions-in-your-price-sheet"></a>Termos e descrições na planilha de preços

A seção a seguir descreve os termos importantes mostrados na planilha de preços do Contrato de Cliente da Microsoft.

| **Nome do Campo**   | **Descrição**   |
| --- | --- |
| billingAccountId  | Identificador exclusivo da conta de cobrança.   |
| billingAccountName  | Nome da conta de cobrança.  |
| billingProfileId  | Identificador exclusivo do perfil de cobrança.   |
| billingProfileName  | Nome do perfil de cobrança configurado para receber faturas. Os preços na planilha de preços estão associados a esse perfil de cobrança. |
| productOrderName  | Nome do plano de produto comprado. |
| serviceFamily  | Tipo de serviço do Azure. Por exemplo: Computação, análise, segurança |
| Produto  | Nome do produto que acumula os encargos. Ex.: BD SQL Básico versus BD SQL Standard  |
| productId  | Identificador exclusivo do produto cujo medidor é consumido. |
| unitOfMeasure  | Identifica as unidades de medida para cobrança do serviço. Por exemplo, os serviços de computação são cobrados por hora. |
| meterId  | Identificador exclusivo do medidor. |
| meterName  | Nome do medidor. O medidor representa o recurso implantável de um serviço do Azure. |
| meterCategory  | Nome da categoria de classificação do medidor. Por exemplo, _Serviços de Nuvem_, _Rede_ etc. |
| meterType  |  Nome do tipo de medidor. |
| meterSubCategory  | Nome da categoria de subclassificação do medidor.  |
| meterRegion  | Nome da região em que o medidor do serviço está disponível. Identifica o local do datacenter para determinados serviços que são cobrados com base no local do datacenter.    |
| tierId  | Identifica o tipo de preço, quando aplicável. Isso funciona em conjunto com tierMinimumUnits para definir preços em tipos quando os preços variam de acordo com o número de unidades consumidas.    |
| tierMinimumUnits  | Define o limite inferior do intervalo de tipo para o qual os preços são definidos. Por exemplo, se o intervalo for de 0 a 100, tierMinimumUnits será 0.  |
| effectiveStartDate  | Data de início da vigência do preço. |
| effectiveEndDate  | Data de término da vigência do preço. |
| unitPrice  | Preço por unidade no momento da cobrança (não o preço combinado efetivo) específico para um medidor e o nome do pedido do produto.  Observação: O preço unitário não é o mesmo que o preço efetivo em downloads de detalhes de uso, no caso de serviços que têm preços diferenciais entre camadas.  No caso de serviços com preços de várias camadas, o preço efetivo é uma taxa combinada entre as camadas e não mostra um preço unitário específico da camada. O preço combinado ou o preço efetivo é o preço líquido para a quantidade consumida que abrange várias camadas (em que cada camada tem um preço unitário específico). |
| basePrice  | O preço de mercado no momento em que o cliente entra no serviço ou o preço de mercado no momento em que o medidor de serviço é iniciado, caso seja após a entrada.   |

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Verificar o acesso ao Contrato de Cliente da Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Precisa de ajuda? Entre em contato conosco.

Se você tiver dúvidas ou precisar de ajuda, [crie uma solicitação de suporte](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Próximas etapas

- [Exibir e baixar o preço de sua organização](billing-ea-pricing.md)
