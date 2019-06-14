---
title: Sincronizar atributos para o Azure AD para o mapeamento | Microsoft Docs
description: Saiba como sincronizar atributos do Active Directory local ao Azure AD. Ao configurar o provisionamento de usuário para aplicativos SaaS, use o recurso de extensão de diretório para adicionar atributos de origem que não são sincronizados por padrão.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: mimart
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9460bc924ea662646360d1a3f5087949a39a03f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65806110"
---
# <a name="sync-an-attribute-from-your-on-premises-active-directory-to-azure-ad-for-provisioning-to-an-application"></a>Um atributo do Active Directory local ao Azure AD para provisionamento para um aplicativo de sincronização

Ao personalizar mapeamentos de atributo para o provisionamento de usuário, você pode achar que o atributo que você deseja mapear não aparece na **atributo de origem** lista. Este artigo mostra como adicionar o atributo ausente, sincronizá-lo do seu local do Active Directory (AD) para o Azure Active Directory (Azure AD).

Azure AD deve conter todos os dados necessários para criar um perfil de usuário ao provisionar contas de usuário do AD do Azure para um aplicativo SaaS. Em alguns casos, para disponibilizar os dados você pode precisar sincronizar atributos de seu local AD com o AD do Azure. O Azure AD Connect sincroniza automaticamente determinados atributos para o Azure AD, mas nem todos os atributos. Além disso, alguns atributos (por exemplo, o SAMAccountName) que são sincronizados por padrão podem não ser expostos por meio da API do Graph do Azure AD. Nesses casos, você pode usar o recurso de extensão de diretório do Azure AD Connect para sincronizar o atributo para o Azure AD. Dessa forma, o atributo será visível para a API do Graph do Azure AD e o serviço de provisionamento do AD do Azure.

Se os dados que necessários para o provisionamento estão no Active Directory, mas não estão disponíveis para provisionamento pelos motivos descritos acima, siga estas etapas.
 
## <a name="sync-an-attribute"></a>Um atributo de sincronização 

1. Abrir o Assistente do Azure AD Connect, escolha as tarefas e, em seguida, escolha **personalizar opções de sincronização**.

   ![Página tarefas adicionais de Assistente de Active Directory Connect do Azure](media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-customize.png)
 
2. Entre como Administrador Global do Azure AD. 

3. Sobre o **recursos opcionais** página, selecione **sincronização de atributo de extensão de diretório**.
 
   ![Página de recursos opcionais de Assistente de Active Directory Connect do Azure](media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extension-attribute-sync.png)

4. Selecione os atributos que você deseja estender para o Azure AD.
   > [!NOTE]
   > A pesquisa sob **atributos disponíveis** diferencia maiusculas de minúsculas.

   ![Azure Active Directory Connect diretório extensões seleção página do Assistente](media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extensions.png)

5. Conclua o Assistente do Azure AD Connect e permitir que um ciclo de sincronização completa para ser executado. Quando o ciclo for concluído, o esquema é estendido e os novos valores são sincronizados entre suas instalações AD e o Azure AD.
 
6. No portal do Azure, enquanto você estiver [editar mapeamentos de atributos de usuário](customize-application-attributes.md), o **atributo de origem** lista agora contém o atributo adicionado no formato `<attributename> (extension_<appID>_<attributename>)`. Selecione o atributo e mapeá-lo para o aplicativo de destino para o provisionamento.

   ![Azure Active Directory Connect diretório extensões seleção página do Assistente](media/user-provisioning-sync-attributes-for-mapping/attribute-mapping-extensions.png)

> [!NOTE]
> A capacidade de provisionar os atributos de referência do AD local, como **managedby** ou **DN/DistinguishedName**, não há suporte para hoje mesmo. Você pode solicitar esse recurso no [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory). 

## <a name="next-steps"></a>Próximas etapas

* [Definir quem está no escopo para provisionamento](define-conditional-rules-for-provisioning-user-accounts.md)
