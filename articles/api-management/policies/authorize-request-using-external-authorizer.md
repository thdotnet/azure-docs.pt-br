---
title: Exemplo de política de gerenciamento de API do Azure – Autorizar solicitação usando um autorizador externo | Microsoft Docs
description: Exemplo de política de gerenciamento de API do Azure – demonstra como autorizar solicitações usando um autorizador externo encapsulando uma lógica de autenticação/autorização personalizada ou herdada.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/06/2018
ms.author: apimpm
ms.openlocfilehash: 92836e0bfe43a41ad6547c68bc0b9a326528862c
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70074165"
---
# <a name="authorize-requests-using-external-authorizer"></a>Autorizar solicitações usando um autorizador externo

Este artigo mostra um exemplo de política de gerenciamento de API do Azure que demonstra como proteger o acesso à API usando um autorizador externo que encapsula uma lógica de autenticação/autorização personalizada. Para definir ou editar um código de política, siga as etapas descritas em [Definir ou editar uma política](../set-edit-policies.md). Para ver outros exemplos, consulte [exemplos de política](../policy-samples.md).

## <a name="policy"></a>Política

Cole o código no bloco de **entrada**.

[!code-xml[Main](../../../api-management-policy-samples/examples/Authorize requests using external authorizer.policy.xml)]

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre as políticas de APIM:

+ [Políticas de restrição de acesso](../api-management-access-restriction-policies.md)
+ [Exemplos de política](../policy-samples.md)
