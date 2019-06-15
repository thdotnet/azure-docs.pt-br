---
title: Solucionar problemas de gerenciamento de direitos do AD do Azure (visualização) – Azure Active Directory
description: Conheça alguns itens que você deve verificar para ajudá-lo a solucionar problemas do gerenciamento de direitos do Active Directory do Azure (visualização).
services: active-directory
documentationCenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 05/30/2019
ms.author: rolyon
ms.reviewer: markwahl-msft
ms.collection: M365-identity-device-management
ms.openlocfilehash: c2526ef10c3080dae1b32881a109a9436a0fd390
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66473831"
---
# <a name="troubleshoot-azure-ad-entitlement-management-preview"></a>Solucionar problemas do gerenciamento de direitos do AD do Azure (visualização)

> [!IMPORTANT]
> No momento, o gerenciamento de direitos do Azure AD (Azure Active Directory) está em versão prévia pública.
> Essa versão prévia é fornecida sem um contrato de nível de serviço e não é recomendada para cargas de trabalho de produção. Alguns recursos podem não ter suporte ou podem ter restrição de recursos.
> Para obter mais informações, consulte [Termos de Uso Complementares de Versões Prévias do Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Este artigo descreve alguns itens que você deve verificar para ajudá-lo a solucionar problemas de gerenciamento de direitos do Active Directory (Azure AD).

## <a name="checklist-for-entitlement-management-administration"></a>Lista de verificação para administração de gerenciamento de direitos

* Se você receber uma mensagem de acesso negado ao configurar o gerenciamento de direitos, e você for um Administrador Global, certifique-se de que o diretório tiver um [licença do Azure AD Premium P2 (ou EMS E5)](entitlement-management-overview.md#license-requirements).  
* Se você receber um acesso negado a mensagem ao criar ou exibir pacotes de acesso, e você for um membro de um grupo de criador de catálogo, você deve criar um catálogo antes de criar seu primeiro pacote de acesso.

## <a name="checklist-for-adding-a-resource"></a>Lista de verificação para adicionar um recurso

* Para um aplicativo de recurso em um pacote de acesso, ele deve ter pelo menos uma função de recurso que pode ser atribuída. As funções são definidas pelo próprio aplicativo e são gerenciadas no AD do Azure. Observe que o portal do Azure também pode mostrar as entidades de serviço para serviços que não podem ser selecionados como aplicativos.  Em particular, **Exchange Online** e **SharePoint Online** são serviços, não os aplicativos que têm funções de recurso no diretório, para que eles não podem ser incluídos em um pacote de acesso.  Em vez disso, use o licenciamento baseado em grupo para estabelecer uma licença apropriada para um usuário que precisa de acesso a esses serviços.

* Para um grupo de recurso em um pacote de acesso, deve ser capaz de ser modificado no Azure AD.  Grupos que se originam em um diretório do Active Directory local não podem ser atribuídos como recursos porque seu proprietário ou atributos de membro não podem ser alterados no AD do Azure.  

* Bibliotecas de documentos do SharePoint Online e documentos individuais não podem ser adicionados como recursos.  Em vez disso, crie um grupo de segurança do Azure AD, inclua esse grupo e uma função de site no pacote de acesso e no SharePoint Online, usar esse grupo para controlar o acesso à biblioteca de documentos ou ao documento.

* Se houver usuários que já tenham sido atribuídos a um recurso que você deseja gerenciar com um pacote de acesso, certifique-se de que os usuários são atribuídos ao pacote de acesso com uma política apropriada. Por exemplo, você talvez queira incluir um grupo em um pacote de acesso que já tenha os usuários no grupo. Se esses usuários em exigem o grupo de acesso continuaram, eles devem ter uma política apropriada para os pacotes de acesso para que eles não percam o acesso ao grupo. Você pode atribuir o pacote de acesso fazendo os que os usuários solicitem o pacote de acesso que contém esse recurso, ou atribuindo-lhes diretamente para o pacote de acesso. Para obter mais informações, consulte [editar e gerenciar um pacote existente de acesso](entitlement-management-access-package-edit.md).

## <a name="checklist-for-providing-external-users-access"></a>Lista de verificação para fornecer acesso a usuários externos

* Se não houver um B2B [lista de permissões](../b2b/allow-deny-list.md), em seguida, os usuários cujas pastas não são permitidas não poderão solicitar acesso.

* Verifique se não há nenhuma [políticas de acesso condicional](../conditional-access/require-managed-devices.md) que impediria que os usuários externos solicitando acesso ou ser capaz de usar os aplicativos nos pacotes de acesso.

## <a name="checklist-for-request-issues"></a>Lista de verificação para problemas de solicitação

* Quando um usuário deseja solicitar acesso a um pacote de acesso, certifique-se de que estão usando o **link do portal de acesso Meus** para o pacote de acesso. Para obter mais informações, consulte [link do portal de acesso de minha cópia](entitlement-management-access-package-edit.md#copy-my-access-portal-link).

* Quando um usuário entra portal do meu acesso para solicitar um pacote de acesso, certifique-se de que eles se autenticam usando suas contas institucionais. A conta organizacional pode ser uma conta no diretório de recursos ou em um diretório que está incluído em uma das políticas do pacote de acesso. Se a conta do usuário não é uma conta organizacional ou o diretório não está incluído na política, o usuário não verá o pacote de acesso. Para obter mais informações, consulte [solicitar acesso a um pacote de acesso](entitlement-management-request-access.md).

* Se um usuário é impedido de entrar no diretório de recursos, eles não poderão solicitar acesso no portal do meu acesso. Antes do usuário pode solicitar o acesso, você deve remover o bloco de entrada do perfil do usuário. Para remover o bloco entrar no portal do Azure, clique em **Azure Active Directory**, clique em **usuários**, clique no usuário e, em seguida, clique em **perfil**. Editar o **as configurações** seção e altere **bloquear entrada** para **não**. Para obter mais informações, consulte [adicionar ou atualizar informações de perfil do usuário usando o Azure Active Directory](../fundamentals/active-directory-users-profile-azure-portal.md).  Você também pode verificar se o usuário foi bloqueado devido a um [política de proteção de identidade](../identity-protection/howto-unblock-user.md).

* No portal do meu acesso, se um usuário for um solicitante e um aprovador, eles não verão sua solicitação para um pacote de acesso sobre os **aprovações** página. Esse comportamento é intencional: um usuário não pode aprovar a solicitação de seu próprios. Certifique-se de que o pacote de acesso que ele está solicitando tem outros aprovadores configurados na política. Para obter mais informações, consulte [editar uma política existente](entitlement-management-access-package-edit.md#edit-an-existing-policy).

* Se um novo usuário externo, que tem não entrou anteriormente no seu diretório, recebe um pacote de acesso, incluindo um site do SharePoint Online, o seu pacote de acesso será exibido como não totalmente entregue até que sua conta é provisionada no SharePoint Online.

## <a name="next-steps"></a>Próximas etapas

- [Exibir relatórios de como os usuários têm acesso no gerenciamento de direitos](entitlement-management-reports.md)
