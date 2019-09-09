---
title: Descobrir recursos do Azure para gerenciar em PIM-Azure Active Directory | Microsoft Docs
description: Saiba como descobrir recursos do Azure para gerenciar no Azure AD PIM (Privileged Identity Management).
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/09/2019
ms.author: curtand
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef1f1712ab7474747829c8744a39e74b78777668
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804171"
---
# <a name="discover-azure-resources-to-manage-in-pim"></a>Descobrir os recursos do Azure para gerenciar no PIM

Usando o Azure Active Directory (Azure AD) Privileged Identity Management (PIM), você pode melhorar a proteção dos recursos do Azure. Isso é útil para organizações que já usam o PIM para proteger as funções do Azure AD e para os proprietários do grupo de gerenciamento e da assinatura que estão procurando proteger os recursos de produção.

Quando você configura pela primeira vez os recursos do PIM para o Azure, é necessário descobrir e selecionar os recursos a serem protegidos com o PIM. Não há nenhum limite para o número de recursos que você pode gerenciar com o PIM. No entanto, é recomendável começar com seus recursos  mais importantes (produção).

## <a name="discover-resources"></a>Descobrir recursos

1. Entre no [Portal do Azure](https://portal.azure.com/).

1. Abra o **Azure AD Privileged Identity Management**.

1. Clique em **Recursos do Azure**.

    Se esta for a primeira vez que você usa recursos do PIM para o Azure, você verá um painel Recursos do Discover.

    ![Descobrir o painel de recursos sem recursos listados para a primeira experiência](./media/pim-resource-roles-discover-resources/discover-resources-first-run.png)

    Se outro administrador de recurso ou diretório em sua organização já estiver gerenciando recursos do Azure no PIM, você verá uma lista dos recursos que estão sendo gerenciados no momento.

    ![Painel descobrir recursos listando recursos que estão sendo gerenciados no momento](./media/pim-resource-roles-discover-resources/discover-resources.png)

1. Clique em **Descobrir recursos** para iniciar a experiência de descoberta.

    ![Painel de descoberta listando recursos que podem ser gerenciados, como assinaturas e grupos de gerenciamento](./media/pim-resource-roles-discover-resources/discovery-pane.png)

1. No painel Descoberta, use **Filtro de estado de recurso** e **Selecione o tipo de recurso** para filtrar os grupos de gerenciamento ou assinaturas para os quais você tem permissão de gravação. Ele provavelmente é mais fácil para começar **todos** os inicialmente.

    Você pode pesquisar por recursos de assinatura ou de grupo de gerenciamento e selecioná-los somente para gerenciá-los usando o PIM. Quando você gerencia um grupo de gerenciamento ou uma assinatura no PIM, você também pode gerenciar seus recursos filhos.

1. Adicione uma marca de seleção ao lado de todos os recursos não gerenciados que você deseja gerenciar.

1. Clique em **Gerenciar recurso** para começar a gerenciar os recursos selecionados.

    > [!NOTE]
    > Depois que um grupo de gerenciamento ou assinatura for definido como gerenciado, ele não poderá ser gerenciado. Isso impede que outro administrador de recursos remova as configurações do PIM.

    ![Painel de descoberta com um recurso selecionado e a opção de gerenciar recurso realçada](./media/pim-resource-roles-discover-resources/discovery-manage-resource.png)

1. Se você receber uma mensagem solicitando a confirmação da integração do recurso selecionado para o gerenciamento, clique em **Sim**.

    ![Mensagem confirmando a integração dos recursos selecionados para gerenciamento](./media/pim-resource-roles-discover-resources/discovery-manage-resource-message.png)

## <a name="next-steps"></a>Próximas etapas

- [Definir configurações de função de recurso do Azure no PIM](pim-resource-roles-configure-role-settings.md)
- [Atribuir funções de recurso do Azure no PIM](pim-resource-roles-assign-roles.md)
