---
title: Exemplos de transformação de declarações gerais para o esquema de estrutura de experiência de identidade de Azure Active Directory B2C
description: Exemplos de transformação de declarações gerais para o esquema de estrutura de experiência de identidade de Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 08/27/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 7cea33cb61f8f8d0fe305a757f11c80bc5da24ca
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2019
ms.locfileid: "70032892"
---
# <a name="general-claims-transformations"></a>Transformações de declarações gerais

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Este artigo fornece exemplos de como usar as transformações de declarações gerais do esquema de estrutura de experiência de identidade em Azure Active Directory B2C (Azure AD B2C). Para obter mais informações, confira [ClaimsTransformations](claimstransformations.md).

## <a name="doesclaimexist"></a>DoesClaimExist

Verifica se a **inputClaim** existe ou não e define **outputClaim** como true ou false adequadamente.

| Item | TransformationClaimType | Tipo de dados | Observações |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | InputClaim |Any | A declaração de entrada cuja existência deve ser verificada. |
| OutputClaim | outputClaim | boolean | O ClaimType produzido depois de invocar esta ClaimsTransformation. |

Use essa transformação de declarações para verificar se uma declaração existe ou se contém algum valor. O valor retornado é um valor booliano que indica se a declaração existe. O exemplo a seguir verifica se o endereço de email existe.

```XML
<ClaimsTransformation Id="CheckIfEmailPresent" TransformationMethod="DoesClaimExist">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isEmailPresent" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Exemplo

- Declarações de entrada:
  - **inputClaim**: someone@contoso.com
- Declarações de saída:
  - **outputClaim**: true

## <a name="hash"></a>Hash

Transforme o texto sem formatação fornecido em hash usando o sal e um segredo. O algoritmo de hash usado é SHA-256.

| Item | TransformationClaimType | Tipo de dados | Observações |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | texto não criptografado | cadeia de caracteres | A declaração de entrada a ser criptografada |
| InputClaim | sal | cadeia de caracteres | O parâmetro sal. Você pode criar um valor aleatório, usando a transformação de declarações `CreateRandomString`. |
| InputParameter | randomizerSecret | cadeia de caracteres | Aponta para uma chave de **política**de Azure ad B2C existente. Para criar uma nova chave de política: No locatário do Azure AD B2C, em **gerenciar**, selecione **estrutura de experiência de identidade**. Selecione **chaves de política** para exibir as chaves que estão disponíveis em seu locatário. Selecione **Adicionar**. Em **Opções** selecione **Manual**. Forneça um nome (o prefixo *B2C_1A_* pode ser adicionado automaticamente.). Na caixa de texto **segredo** , insira qualquer segredo que você queira usar, como 1234567890. Para **Uso de chave**, selecione **Assinatura**. Selecione **Criar**. |
| OutputClaim | hash | cadeia de caracteres | O ClaimType que é produzido depois de invocar esta transformação de declarações. A declaração configurada na inputClaim `plaintext`. |

```XML
<ClaimsTransformation Id="HashPasswordWithEmail" TransformationMethod="Hash">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="password" TransformationClaimType="plaintext" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="salt" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="randomizerSecret" DataType="string" Value="B2C_1A_AccountTransformSecret" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="hashedPassword" TransformationClaimType="hash" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Exemplo

- Declarações de entrada:
  - **texto não criptografado**: MyPass@word1
  - **salt**: 487624568
  - **randomizerSecret**: B2C_1A_AccountTransformSecret
- Declarações de saída:
  - **outputClaim**: CdMNb/KTEfsWzh9MR1kQGRZCKjuxGMWhA5YQNihzV6U=
