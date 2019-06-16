---
title: Resgate do convite em colaboração B2B - Azure Active Directory | Microsoft Docs
description: Descreve a experiência de resgate de convite de colaboração B2B do Azure AD para usuários finais, incluindo o contrato de termos de privacidade.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisol
ms.collection: M365-identity-device-management
ms.openlocfilehash: a80eaa134130195fce00ee6a4d68851e478c4532
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67052497"
---
# <a name="azure-active-directory-b2b-collaboration-invitation-redemption"></a>Resgate do convite de colaboração do Azure Active Directory B2B

Este artigo descreve as maneiras em que os usuários convidados podem acessar seus recursos e o processo de consentimento que encontrará. Se você enviar um email de convite ao convidado, o convite inclui um link que o convidado pode resgatar para obter acesso a seu aplicativo ou o portal. O email de convite é apenas uma das maneiras de convidados podem ter acesso aos seus recursos. Como alternativa, você pode adicionar convidados a seu diretório e conceda a um link direto para o portal ou o aplicativo que você deseja compartilhar. Independentemente do método usarem, convidados são orientados por meio de um processo de consentimento pela primeira vez. Esse processo garante que os convidados concordar com os termos de privacidade e aceitam qualquer [termos de uso](https://docs.microsoft.com/azure/active-directory/governance/active-directory-tou) você configurou.

Quando você adiciona um usuário convidado ao diretório, a conta de usuário convidado tem um status de consentimento (visível no PowerShell) que é inicialmente definido como **PendingAcceptance**. Essa configuração permanece até que o convidado aceita o convite e concorda em sua política de privacidade e termos de uso. Depois disso, o status de consentimento é alterado para **aceito**, e as páginas de consentimento não serão apresentadas para o convidado.

## <a name="redemption-through-the-invitation-email"></a>Resgate por meio de email de convite

Quando você adiciona um usuário convidado ao diretório pelo [usando o portal do Azure](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-add-guest-users-portal), um email de convite é enviado para o convidado no processo. Você também pode optar por enviar emails de convite, quando você estiver [usando o PowerShell](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-invite-powershell) para adicionar usuários convidados ao seu diretório. Aqui está uma descrição da experiência do convidado quando eles resgatar o link no email.

1. O convidado recebe um [email de convite](https://docs.microsoft.com/azure/active-directory/b2b/invitation-email-elements) que é enviada do **Invitations Microsoft**.
2. O convidado selecionará **começar** no email.
3. Se o convidado não tiver uma conta do AD do Azure, uma conta da Microsoft (MSA) ou uma conta de email em uma organização federada, ele será solicitado a criar uma MSA (a menos que o [senha de uso único](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) recurso estiver habilitado, o que não exige uma MSA ).
4. O convidado será guiado através do [experiência de consentimento](#consent-experience-for-the-guest) descrito abaixo.

## <a name="redemption-through-a-direct-link"></a>Resgate por meio de um link direto

Como uma alternativa para o email de convite, você pode dar um convidado um link direto para o aplicativo ou o portal. Primeiro, você precisa adicionar o usuário convidado ao seu diretório por meio de [portal do Azure](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-add-guest-users-portal) ou [PowerShell](https://docs.microsoft.com/azure/active-directory/b2b/b2b-quickstart-invite-powershell). Em seguida, você pode usar qualquer um dos [maneiras personalizáveis de implantar aplicativos para usuários](https://docs.microsoft.com/azure/active-directory/manage-apps/end-user-experiences), incluindo links diretos logon. Quando um convidado usa um link direto em vez de email de convite, eles ainda serão guiados através da experiência de consentimento pela primeira vez.

> [!IMPORTANT]
> O link direto deve ser específico do locatário. Em outras palavras, ele deve incluir uma ID de locatário ou domínio verificado para que o convidado pode ser autenticado em seu locatário, onde o aplicativo compartilhado está localizado. Uma URL comuns, como https://myapps.microsoft.com não funcionará para um convidado porque ele será redirecionado para seu locatário inicial para autenticação. Aqui estão alguns exemplos de links diretos com o contexto de locatário:
 > - Painel de acesso de aplicativos: https://myapps.microsoft.com/?tenantid=&lt; id do locatário&gt; 
 > - Painel de acesso de aplicativos para um domínio verificado: https://myapps.microsoft.com/&lt; verificado domínio&gt;
 > - Portal do Azure: https://portal.azure.com/&lt; id do locatário&gt;
 > - Aplicativos individuais: Veja como usar um [link de logon direto](../manage-apps/end-user-experiences.md#direct-sign-on-links)

Há alguns casos em que o email de convite é recomendável em vez de um link direto. Se esses casos especiais são importantes para sua organização, é recomendável que você convidar usuários por meio de métodos que ainda enviar o email de convite:
 - O usuário não tem uma conta do AD do Azure, uma MSA ou uma conta de email em uma organização federada. A menos que você estiver usando o recurso de senha de uso único, o convidado precisa resgatar o email de convite para ser guiado pelas etapas para criar uma MSA.
 - Às vezes, o objeto de usuário convidado não pode ter um endereço de email devido a um conflito com um objeto de contato (por exemplo, um objeto de contato Outlook). Nesse caso, o usuário deve clicar na URL de resgate no email de convite.
 - O usuário pode entrar com um alias do endereço de email que foi convidado. (Um alias é um endereço de email adicional associado a uma conta de email.) Nesse caso, o usuário deve clicar na URL de resgate no email de convite.

## <a name="consent-experience-for-the-guest"></a>Experiência de consentimento para o convidado

Quando um convidado entra acessar recursos em uma organização parceira pela primeira vez, eles são guiados por meio de páginas a seguir. 

1. As revisões de convidado a **revisar permissões** página que descreve a declaração de privacidade da organização emissora do convite. Um usuário deve **Accept** o uso de suas informações de acordo com as políticas de privacidade da organização emissora do convite para continuar.

   ![Captura de tela mostrando a página Analisar as permissões](media/redemption-experience/review-permissions.png) 

   > [!NOTE]
   > Para obter informações sobre como você, como administrador de locatários, pode fornecer um link para a política de privacidade de sua organização, confira [Como: Adicionar informações de privacidade de sua organização ao Azure Active Directory](https://aka.ms/adprivacystatement).

2. Se os termos de uso são configurados, o convidado abre e revisa os termos de uso e, em seguida, seleciona **Accept**. 

   ![Captura de tela mostrando os novos termos de uso](media/redemption-experience/terms-of-use-accept.png) 

   > [!NOTE]
   > Você pode configurar, confira [termos de uso](../governance/active-directory-tou.md) na **gerenciar** > **relações organizacionais** > **termos de uso**.

3. A menos que especificado o contrário, o convidado é redirecionado para o painel de acesso de aplicativos, que lista os aplicativos que o convidado pode acessar.

   ![Captura de tela mostrando o painel de acesso de aplicativos](media/redemption-experience/myapps.png) 

Em seu diretório, o convidado **Convite aceito** valor é alterado para **Sim**. Se uma MSA foi criado, o convidado **fonte** mostra **Account da Microsoft**. Para obter mais informações sobre as propriedades de conta de usuário convidado, consulte [propriedades de um usuário de colaboração B2B do Azure AD](user-properties.md). 

## <a name="next-steps"></a>Próximas etapas

- [O que é a colaboração B2B do AD do Azure?](what-is-b2b.md)
- [Adicionar usuários de colaboração B2B do Azure Active Directory no Portal do Azure](add-users-administrator.md)
- [Como os operadores de informação adicionam usuários de colaboração B2B ao Azure Active Directory?](add-users-information-worker.md)
- [Adicionar usuários de colaboração B2B do Azure Active Directory usando o PowerShell](customize-invitation-api.md#powershell)
- [Deixar uma organização como usuário convidado](leave-the-organization.md)
