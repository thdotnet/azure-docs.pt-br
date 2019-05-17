---
title: Introdução ao registro combinado para a autenticação multifator (visualização) – Azure Active Directory e o Azure AD SSPR
description: Habilitar combinadas do Azure AD a autenticação multifator e (visualização) do registro de redefinição de senha de autoatendimento
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry, calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: bc4ff596cdafd348288187b0cd9b32f7b4c2d275
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65823384"
---
# <a name="enable-combined-security-information-registration-preview"></a>Registro de informações de segurança Enable combinado (visualização)

Antes de habilitar a nova experiência, examine o artigo [combinados de registro de informações de segurança (visualização)](concept-registration-mfa-sspr-combined.md) para garantir que você entende a funcionalidade e os efeitos desse recurso.

![Experiência aprimorada de registro de informações de segurança combinada](media/howto-registration-mfa-sspr-combined/combined-security-info-more-required.png)

|     |
| --- |
| O registro de informações de segurança combinada para autenticação multifator do Azure e redefinição de senha de autoatendimento do Azure Active Directory (Azure AD) é um recurso de visualização pública do Azure AD. Para saber mais, veja [Termos de uso complementares para Visualizações do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

## <a name="enable-combined-registration"></a>Habilitar o registro combinado

Conclua estas etapas para habilitar o registro combinado:

1. Entrar no portal do Azure como um usuário administrador ou administrador global.
2. Vá para **Azure Active Directory** > **configurações do usuário** > **gerenciar as configurações de recursos de visualização do painel de acesso**.
3. Sob **os usuários podem usar recursos para registrar e gerenciar informações de segurança de visualização - atualizar**, optar por ativar para um **selecionados** grupo de usuários ou para **todos os** usuários.

   ![Habilitar a experiência de visualização de informações de segurança combinada para todos os usuários](media/howto-registration-mfa-sspr-combined/combined-security-info-enable.png)

> [!IMPORTANT]
> A partir de março de 2019, as opções de chamada telefônica não estarão disponíveis para a autenticação multifator e os usuários do SSPR em locatários gratuitos/avaliação do Azure AD. Mensagens SMS não são afetadas por essa alteração. As opções de telefonema ainda estará disponíveis para os usuários no pago locatários do Azure AD.

> [!NOTE]
> Depois de habilitar o registro combinado, os usuários que se registrar ou confirmar o número de telefone ou aplicativo móvel por meio da nova experiência pode usá-los para a autenticação multifator e SSPR, se esses métodos estão habilitados no SSPR e a autenticação multifator políticas. Se você, em seguida, desabilite essa experiência, os usuários que vá para o registro SSPR anterior da página em `https:/aka.ms/ssprsetup` precisarão realizar a autenticação multifator, antes que possam acessar a página.

Se você tiver configurado o Site à lista de atribuição de zona do Internet Explorer, os seguintes sites precisa estar na mesma zona:

* [https://login.microsoftonline.com](https://login.microsoftonline.com)
* [https://mysignins.microsoft.com](https://mysignins.microsoft.com)
* [https://account.activedirectory.windowsazure.com](https://account.activedirectory.windowsazure.com)

## <a name="conditional-access-policies-for-combined-registration"></a>Políticas de acesso condicional para o registro combinado

Protegendo quando e como registrar-se de que os usuários para redefinição de senha de autoatendimento e autenticação multifator do Azure agora é possível com ações de usuário na política de acesso condicional. Esse recurso de visualização está disponível para organizações que habilitaram a [combinados de visualização de registro](../authentication/concept-registration-mfa-sspr-combined.md). Essa funcionalidade pode ser habilitada em organizações onde eles querem que os usuários se registrem para a autenticação multifator do Azure e o SSPR de um local central, como um local de rede confiável durante a integração de RH. Para obter mais informações sobre a criação de locais confiáveis no acesso condicional, consulte o artigo [o que é a condição de localização no acesso condicional do Azure Active Directory?](../conditional-access/location-condition.md#named-locations)

### <a name="create-a-policy-to-require-registration-from-a-trusted-location"></a>Criar uma política para exigir o registro de um local confiável

A seguinte política se aplica a todos os usuários selecionados, que tentam registrar usando a experiência de registro combinado e bloqueia o acesso, a menos que a conexão de um local marcado como rede confiável.

![Criar uma política de autoridade de certificação para controlar o registro de informações de segurança](media/howto-registration-mfa-sspr-combined/conditional-access-register-security-info.png)

1. No **portal do Azure**, navegue até **Azure Active Directory** > **acesso condicional**
1. Selecione **Nova política**
1. Em nome, digite um nome para esta política. Por exemplo, **combinados registro de informações de segurança em redes confiáveis**
1. Sob **atribuições**, clique em **usuários e grupos**e selecione os usuários e grupos que você deseja que essa política para aplicar a

   > [!WARNING]
   > Os usuários devem ser habilitados para o [combinados de visualização de registro](../authentication/howto-registration-mfa-sspr-combined.md).

1. Sob **ações ou aplicativos de nuvem**, selecione **ações do usuário**, marque **registrar informações de segurança (visualização)**
1. Sob **condições** > **locais**
   1. Configurar **Sim**
   1. Incluir **qualquer local**
   1. Excluir **todas as localizações confiáveis**
   1. Clique em **feito** na folha de locais
   1. Clique em **feito** na folha de condições
1. Sob **controles de acesso** > **Grant**
   1. Clique em **bloquear o acesso**
   1. Em seguida, clique em **selecionar**
1. Definir **habilitar política** para **em**
1. Em seguida, clique em **criar**

## <a name="next-steps"></a>Próximas etapas

[Métodos disponíveis para a autenticação multifator e SSPR](concept-authentication-methods.md)

[Configurar a redefinição de senha de autoatendimento](howto-sspr-deployment.md)

[Configurar a autenticação multifator do Azure](howto-mfa-getstarted.md)

[Solução de problemas combinado de registro de informações de segurança](howto-registration-mfa-sspr-combined-troubleshoot.md)

[O que é a condição de localização no acesso condicional do Azure Active Directory?](../conditional-access/location-condition.md)