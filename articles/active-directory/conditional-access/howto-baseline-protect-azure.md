---
title: Política de linha de base exigir MFA para gerenciamento de serviços (visualização) – Azure Active Directory
description: Política de acesso condicional para exigir a MFA do Azure Resource Manager
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: 00ed40bef3f3cbe59825f546ffa39c3ebfb2e41f
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66003170"
---
# <a name="baseline-policy-require-mfa-for-service-management-preview"></a>Política de linha de base: Exigir MFA para gerenciamento de serviços (visualização)

Você pode estar usando uma variedade de serviços do Azure em sua organização. Esses serviços podem ser gerenciados por meio da API do Azure Resource Manager:

* Portal do Azure
* Azure PowerShell
* CLI do Azure

Usando o Azure Resource Manager para gerenciar seus serviços é uma ação altamente privilegiada. O Azure Resource Manager pode alterar as configurações de todo o locatário, como configurações de serviço e a cobrança de assinatura. Autenticação de fator único é vulnerável a uma variedade de ataques, como spray de phishing e a senha. Portanto, é importante verificar a identidade dos usuários que desejam acessar o Azure Resource Manager e atualizar as configurações, exigindo a autenticação multifator antes de permitir acesso.

**Exigir MFA para gerenciamento de serviços** é um [política de linha de base](concept-baseline-protection.md) que exigirão MFA para qualquer usuário que está acessando o portal do Azure, Azure PowerShell ou CLI do Azure. Essa política se aplica a todos os usuários acessando o Azure Resource Manager, independentemente de se ele for um administrador.

Depois que essa política está habilitada em um locatário, todos os usuários que fizerem logon em recursos de gerenciamento do Azure serão solicitados com a autenticação multifator. Se o usuário não estiver registrado para MFA, o usuário precisará se registrar usando o aplicativo Microsoft Authenticator para continuar.

![Exigir MFA para o Azure Resource Manager](./media/howto-baseline-protect-azure/baseline-policy-require-mfa-for-service-management.png)

Para executar a interativo entrar usando [Azure Powershell](https://docs.microsoft.com/powershell/azure/authenticate-azureps), use o [Connect AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet.

```PowerShell
Connect-AzAccount
```

Quando executado, esse cmdlet apresentará uma cadeia de caracteres de token. Para entrar, copie essa cadeia de caracteres e cole-o na [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin) em um navegador. Sua sessão do PowerShell será autenticada para conectar o Azure.

Para executar a interativo entrar usando [CLI do Azure](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest), execute o [login az](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) comando.

```azurecli
az login
```

Se a CLI pode abrir seu navegador padrão, ela irá fazê-lo e carregar uma página de entrada. Caso contrário, você precisará abrir uma página do navegador e siga as instruções na linha de comando para inserir um código de autorização depois de navegar até [ https://aka.ms/devicelogin ](https://aka.ms/devicelogin) no seu navegador. Em seguida, entre com suas credenciais de conta no navegador.

## <a name="deployment-considerations"></a>Considerações de implantação

Porque o **exigir MFA para gerenciamento de serviços** política se aplica a todos os usuários do Azure Resource Manager, várias considerações precisam ser feitas para assegurar uma implantação tranquila. Essas considerações incluem a identificação de usuários e entidades de serviço no Azure AD que não é possível ou não deve realizar a MFA, bem como aplicativos e clientes usados pela sua organização que não dão suporte a autenticação moderna.

### <a name="user-exclusions"></a>Exclusões de usuário

Essa política de linha de base fornece a opção de excluir usuários. Antes de habilitar a política para o seu locatário, é recomendável, exceto as seguintes contas:

* **Acesso de emergência** ou **vigilância** contas para impedir que o bloqueio de conta de todo o locatário. No cenário improvável que todos os administradores estão bloqueados de seu locatário, sua conta administrativa de acesso de emergência pode ser usada para fazer logon em etapas locatário take para recuperar o acesso.
   * Mais informações podem ser encontradas no artigo [gerenciar contas de acesso de emergência no Azure AD](../users-groups-roles/directory-emergency-access.md).
* **Contas de serviço** e **entidades de serviço**, como a conta do Azure AD Connect Sync. Contas de serviço são contas não-interativas que não estão vinculadas a nenhum usuário específico. Eles normalmente são usados pelos serviços de back-end e permitam o acesso programático aos aplicativos. Contas de serviço devem ser excluídas, pois o MFA não pode ser concluída por meio de programação.
   * Se sua organização tiver essas contas em uso no código ou scripts, considere a possibilidade de substituí-los com [identidades gerenciadas](../managed-identities-azure-resources/overview.md). Como solução temporária, você pode excluir essas contas específicas da política de linha de base.
* Usuários que não têm ou não será capaz de usar um Smartphone.
   * Essa política requer que os usuários se registrar para MFA usando o aplicativo Microsoft Authenticator.

## <a name="enable-the-baseline-policy"></a>Habilitar a política de linha de base

A política **política de linha de base: Exigir MFA para gerenciamento de serviços (visualização)** vem pré-configurada e aparecerá na parte superior quando você navega até a folha de acesso condicional no portal do Azure.

Para habilitar essa política e proteger seus administradores:

1. Entrar para o **portal do Azure** como administrador global, administrador de segurança ou administrador de acesso condicional.
1. Navegue até **do Azure Active Directory** > **acesso condicional**.
1. Na lista de políticas, selecione **política de linha de base: Exigir MFA para gerenciamento de serviços (visualização)**.
1. Definir **habilitar política** à **usar a política imediatamente**.
1. Adicionar exclusões usuário clicando em **os usuários** > **selecionar usuários excluídos** e escolhendo os usuários que precisam ser excluídas. Clique em **selecionar** , em seguida, **feito**.
1. Clique em **salvar**.

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações, consulte:

* [Políticas de proteção de linha de base de acesso condicional](concept-baseline-protection.md)
* [Cinco etapas para proteger a infraestrutura de identidade](../../security/azure-ad-secure-steps.md)
* [O que é o acesso condicional no Azure Active Directory?](overview.md)