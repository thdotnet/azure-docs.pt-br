---
title: Configurar o gerenciamento de grupos de autoatendimento – Azure Active Directory | Microsoft Docs
description: Criar e gerenciar grupos de segurança ou grupos do Office 365 no Azure Active Directory e o grupo de segurança de solicitação ou associações de grupo do Office 365
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2b52604d4ad20fed83c4649f046722ed45e766c4
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71097707"
---
# <a name="set-up-self-service-group-management-in-azure-active-directory"></a>Configurar o gerenciamento de grupo de autoatendimento no Azure Active Directory 

Você pode permitir que os usuários criem e gerenciem seus próprios grupos de segurança ou grupos do Office 365 no Azure Active Directory (AD do Azure). O proprietário do grupo pode aprovar ou negar solicitações de associação e pode delegar o controle de associação de grupo. Os recursos de gerenciamento de grupo de autoatendimento não estão disponíveis para grupos de segurança ou listas de distribuição habilitadas para email.

## <a name="self-service-group-membership-defaults"></a>Padrões de associação de grupo de autoatendimento

Quando grupos de segurança são criados no portal do Azure ou usando o PowerShell do Azure AD, somente os proprietários do grupo podem atualizar a associação. Grupos de segurança criados no [painel de acesso](https://account.activedirectory.windowsazure.com/r#/joinGroups) e todos os grupos do Office 365 estão disponíveis para ingressar para todos os usuários, seja aprovado pelo proprietário ou aprovado automaticamente. No painel de acesso, você pode alterar as opções de associação ao criar o grupo.

Grupos criados em | Comportamento padrão do grupo de segurança | Comportamento padrão do grupo do Office 365
------------------ | ------------------------------- | ---------------------------------
[PowerShell do Azure AD](groups-settings-cmdlets.md) | Somente proprietários podem adicionar membros<br>Visível, mas não disponível para junção no painel de acesso | Abrir para ingressar para todos os usuários
[Portal do Azure](https://portal.azure.com) | Somente proprietários podem adicionar membros<br>Visível, mas não disponível para junção no painel de acesso<br>O proprietário não é atribuído automaticamente na criação do grupo | Abrir para ingressar para todos os usuários
[Painel de acesso](https://account.activedirectory.windowsazure.com/r#/joinGroups) | Abrir para ingressar para todos os usuários<br>As opções de associação podem ser alteradas quando o grupo é criado | Abrir para ingressar para todos os usuários<br>As opções de associação podem ser alteradas quando o grupo é criado

## <a name="self-service-group-management-scenarios"></a>Cenários de gerenciamento de grupo de autoatendimento

* **Gerenciamento de grupo delegado** Um exemplo é um administrador que está gerenciando o acesso a um aplicativo SaaS que a empresa está usando. O gerenciamento desses direitos de acesso está se tornando inconveniente, portanto esse administrador solicita ao proprietário de negócios para criar um novo grupo. O administrador atribui o acesso para o aplicativo ao novo grupo e adiciona ao grupo todas as pessoas que já acessam o aplicativo. O proprietário da empresa pode então adicionar mais usuários, e os usuários são automaticamente provisionados para o aplicativo. O proprietário da empresa não precisa esperar que o administrador gerencie o acesso para usuários. Se o administrador conceder a mesma permissão a um gerente em um grupo de negócios diferente, essa pessoa também poderá gerenciar o acesso para seus próprios membros do grupo. Nem o proprietário da empresa nem o gerente podem exibir ou gerenciar as associações de grupo uns dos outros. O administrador ainda pode ver todos os usuários que têm acesso ao aplicativo e direitos de acesso de bloco, se necessário.
* **Gerenciamento de grupo de autoatendimento** Um exemplo deste cenário consiste em dois usuários que têm sites do SharePoint Online que eles configuram de forma independente. Eles desejam fornecer às equipes um do outro acesso a seus sites. Para isso, eles podem criar um grupo no Azure AD e, no SharePoint Online, cada um seleciona esse grupo para fornecer acesso a seus sites. Quando alguém deseja acesso, solicita do painel de acesso, e depois da aprovação obtêm acesso a ambos os sites do SharePoint Online automaticamente. Posteriormente, um deles decide que todas as pessoas que acessam o site também devem obter acesso a determinado aplicativo SaaS. O administrador do aplicativo SaaS pode adicionar direitos de acesso ao aplicativo para o site do SharePoint Online. Depois, as solicitações que forem aprovadas darão acesso aos dois sites do SharePoint Online e também ao aplicativo SaaS.

## <a name="make-a-group-available-for-user-self-service"></a>Disponibilização de um grupo para autoatendimento do usuário

1. Entre no [centro de administração do Azure AD](https://aad.portal.azure.com) com uma conta que seja um administrador global para o diretório.
2. Selecione **Usuários e grupos** e depois **Configurações de grupo**.
3. Defina **Gerenciamento de grupo de autoatendimento habilitado** como **Sim**.
4. Defina **Os usuários podem criar grupos de segurança** ou **Os usuários podem criar grupos do Office 365** como **Sim**.
   * Quando essas configurações forem habilitadas, todos os usuários em seu diretório terão permissão para criar novos grupos de segurança e adicionar membros a esses grupos. Esses novos grupos também aparecem no Painel de Acesso para todos os outros usuários. Se a configuração de política de grupo permitir isso, outros usuários poderão criar solicitações para ingressar nesses grupos. 
   * Quando essas configurações estiverem desabilitadas, os usuários não poderão criar grupos nem alterar grupos existentes dos quais são proprietários. No entanto, ainda podem gerenciar as associações desses grupos e aprovar solicitações de outros usuários para ingressar em seus grupos.

Você também pode usar **Usuários que podem gerenciar grupos de segurança** e **Usuários que podem gerenciar grupos do Office 365** para alcançar um controle de acesso mais granular sobre o gerenciamento de grupo de autoatendimento para seus usuários. Quando **Usuários podem criar grupos** estiver habilitada, todos os usuários em seu locatário terão permissão para criar novos grupos e adicionar membros a esses grupos. Você não pode especificar indivíduos que podem criar seus próprios grupos. Você pode especificar indivíduos somente para tornar outro membro de grupo um proprietário de grupo.

Ao definir **os usuários que podem usar o autoatendimento para grupos de segurança** e **usuários que podem gerenciar grupos do Office 365** como **Sim**, você permite que todos os usuários em seu locatário criem novos grupos.

Você também pode usar **Grupo que pode gerenciar grupos de segurança** ou **Grupo que pode gerenciar os grupos do Office 365** para especificar um único grupo cujos membros podem usar o autoatendimento.

## <a name="next-steps"></a>Próximas etapas

Esses artigos fornecem mais informações sobre o Active Directory do Azure.

* [Gerenciar o acesso a recursos com Grupos do Azure Active Directory](../fundamentals/active-directory-manage-groups.md)
* [Cmdlets do Azure Active Directory para definir configurações de grupo](groups-settings-cmdlets.md)
* [Gerenciamento de aplicativos no Microsoft Azure Active Directory](../manage-apps/what-is-application-management.md)
* [O que é o Active Directory do Azure?](../fundamentals/active-directory-whatis.md)
* [Integrar suas identidades locais com o Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
