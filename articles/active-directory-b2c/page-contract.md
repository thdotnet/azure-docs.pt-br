---
title: Selecione um contrato de página - Azure Active Directory B2C
description: Saiba como selecionar um contrato de página no Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/04/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: c3ee05096b0bfd071ea569105973097ce9727b07
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604538"
---
# <a name="select-a-page-contract-in-azure-active-directory-b2c-using-custom-policies"></a>Selecionar um contrato de página no Azure Active Directory B2C usando políticas personalizadas

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Você pode habilitar o código do JavaScript do lado do cliente em suas políticas B2C do Azure Active Directory (AD do Azure) se você estiver usando fluxos de usuário ou as políticas personalizadas. Para habilitar o JavaScript para seus aplicativos, você deve adicionar um elemento a sua [política personalizada](active-directory-b2c-overview-custom.md), selecione um contrato de página e use [b2clogin.com](b2clogin.md) nas suas solicitações.

Um contrato de página é uma associação de elementos que o Azure AD B2C fornece e o conteúdo que você fornece.

Este artigo discute como selecionar um contrato de página no Azure AD B2C, configurando-o em uma política personalizada.

> [!NOTE]
> Se você quiser habilitar o JavaScript para fluxos de usuário, consulte [JavaScript e página Contrato de versões no Azure Active Directory B2C](user-flow-javascript-overview.md).

## <a name="replace-datauri-values"></a>Substituir os valores de DataUri

Em suas políticas personalizadas, você pode ter [ContentDefinitions](contentdefinitions.md) que definem os modelos HTML usados no percurso do usuário. O **ContentDefinition** contém um **DataUri** que se refere aos elementos da página fornecidos pelo Azure AD B2C. O **LoadUri** é o caminho relativo para o conteúdo HTML e CSS que você fornece.

```XML
<ContentDefinition Id="api.idpselections">
  <LoadUri>~/tenant/default/idpSelector.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Idp selection page</Item>
    <Item Key="language.intro">Sign in</Item>
  </Metadata>
</ContentDefinition>
```

Para selecionar um contrato de página, você deve alterar os valores de **DataUri** em seu [ContentDefinitions](contentdefinitions.md) em suas políticas. Ao mudar dos antigos valores de **DataUri** para os novos, você está selecionando um pacote imutável. A vantagem de usar esse pacote é saber que ele não sofrerá alterações e não causará um comportamento inesperado em sua página.

Para configurar um contrato de página, use a tabela a seguir para encontrar valores de **DataUri**.

| Valor antigo de DataUri | Novo valor de DataUri |
| ----------------- | ----------------- |
| `urn:com:microsoft:aad:b2c:elements:claimsconsent:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:claimsconsent:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssd:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssd:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.1.0` |

## <a name="version-change-log"></a>Log de alterações de versão

Pacotes de contrato de página são atualizados periodicamente para incluir correções e melhorias em seus elementos de página. O log de alterações a seguir especifica as alterações introduzidas em cada versão.

### <a name="110"></a>1.1.0

- Página de exceção (globalexception)
  - Correção de acessibilidade
  - Removida a mensagem padrão quando não há nenhum contato da política
  - Padrão de CSS removido
- Página MFA (multifatorial)
  - Botão 'Confirmar código' removido
  - O campo de entrada para o código agora somente aceita até seis (6) caracteres de entrada
  - A página automaticamente tentará verificar o código inserido quando um código de 6 dígitos é inserido, sem qualquer botão precisar ser clicado
  - Se o código estiver incorreto, em seguida, o campo de entrada é automaticamente desmarcado
  - Após 3 (três) tentativas com um código incorreto, o B2C envia um erro para a terceira parte confiável
  - Correções de acessibilidade
  - Padrão de CSS removido
- Página autodeclarada (selfasserted)
  - Alerta de ' Cancelar ' removido
  - Classe CSS para elementos de erro
  - Mostrar/Ocultar erro lógica aprimorados
  - Padrão de CSS removido
- SSP unificado (unifiedssp)
  - Adicionado Mantenha-me conectado no controle (KMSI)

### <a name="100"></a>1.0.0

- Versão inicial

## <a name="next-steps"></a>Próximas etapas

Encontre mais informações sobre como personalizar a interface do usuário dos seus aplicativos em [Personalizar a interface do usuário do aplicativo usando uma política personalizada no Azure Active Directory B2C](active-directory-b2c-ui-customization-custom.md).
