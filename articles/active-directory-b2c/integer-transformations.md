---
title: Exemplos de transformação de declarações de inteiros para o esquema da Estrutura de Experiência de Identidade do Azure Active Directory B2C | Microsoft Docs
description: Exemplos de transformação de declarações de inteiros para o esquema da Estrutura de Experiência de Identidade do Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: d82f7fecfb35c63d586993fed73a83209782a890
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71064206"
---
# <a name="integer-claims-transformations"></a>Transformações de declarações de inteiros

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Este artigo fornece exemplos de como usar as transformações de declarações de inteiro do esquema de estrutura de experiência de identidade em Azure Active Directory B2C (Azure AD B2C). Para obter mais informações, confira [ClaimsTransformations](claimstransformations.md).

## <a name="convertnumbertostringclaim"></a>ConvertNumberToStringClaim

Converte um tipo de dados Long em um tipo de dados String.

| Item | TransformationClaimType | Tipo de dados | Observações |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | InputClaim | long | O ClaimType para converter em uma cadeia de caracteres. |
| OutputClaim | outputClaim | cadeia de caracteres | O ClaimType produzido depois de invocar esta ClaimsTransformation. |

Neste exemplo, a declaração `numericUserId` com um tipo de valor Long é convertido em uma declaração `UserId` com um tipo de valor String.

```XML
<ClaimsTransformation Id="CreateUserId" TransformationMethod="ConvertNumberToStringClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="numericUserId" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="UserId" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Exemplo

- Declarações de entrada:
    - **inputClaim**: 12334 (long)
- Declarações de saída:
    - **outputClaim**: "12334" (string)

