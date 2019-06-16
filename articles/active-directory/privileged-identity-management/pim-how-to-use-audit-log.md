---
title: Exibir o histórico de auditoria de funções do Azure AD no PIM - Azure Active Directory | Microsoft Docs
description: Saiba como exibir o histórico de auditoria para funções do Azure AD no Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 06/10/2019
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8061cff8d39db66cb22a5650c7688657aa8b3554
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67053944"
---
# <a name="view-audit-history-for-azure-ad-roles-in-pim"></a>Exibir o histórico de auditoria para funções do Azure AD no PIM

Você pode usar o histórico de auditoria do Azure Active Directory (Azure AD) gerenciamento PIM (Privileged Identity) para ver todas as ativações e atribuições de função nos últimos 30 dias para todas as funções privilegiadas. Se você quiser ver o histórico completo de auditoria da atividade em seu diretório, incluindo o administrador, usuário final e atividade de sincronização, você pode usar o [relatórios de atividade e segurança do Azure Active Directory](../reports-monitoring/overview-reports.md).

## <a name="view-audit-history"></a>Exibir histórico de auditoria

Siga estas etapas para exibir o histórico de auditoria para funções do Azure AD.

1. Entre no [portal do Azure](https://portal.azure.com/) com um usuário que seja membro da função [Administrador de Funções com Privilégios](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator).

1. Abra o **Azure AD Privileged Identity Management**.

1. Clique em **funções do Microsoft Azure Active Directory**.

1. Clique em **histórico de auditoria de funções de diretório**.

    Dependendo do seu histórico de auditoria, um gráfico de colunas é exibido junto com o total de ativações, o máximo de ativações diárias e a média diária de ativações por dia.

    ![Histórico de auditoria de funções de diretório](media/pim-how-to-use-audit-log/directory-roles-audit-history.png)

    Na parte inferior da página, é exibida uma tabela com informações sobre cada ação no histórico de auditoria disponíveis. As colunas têm os seguintes significados:

    | Coluna | DESCRIÇÃO |
    | --- | --- |
    | Hora | Quando a ação ocorreu. |
    | Solicitante | Usuário que solicitou a ativação de função ou a alteração. Se o valor for **sistema Azure**, verifique o histórico de auditoria do Azure para obter mais informações. |
    | Ação | Ações tomadas pelo solicitante. Ações podem incluir atribuir, cancelar a atribuição, ativar, desativar ou AddedOutsidePIM. |
    | Membro | Usuário que está sendo ativado ou atribuído a uma função. |
    | Função | Função atribuída ou ativada pelo usuário. |
    | Motivo | Texto inserido no campo de motivo durante a ativação. |
    | Expiração | Quando uma função de ativação expira. Aplica-se somente a atribuições de função qualificadas. |

1. Para classificar o histórico de auditoria, clique o **tempo**, **ação**, e **função** botões.

## <a name="filter-audit-history"></a>Filtrar histórico de auditoria

1. Na parte superior da página de histórico de auditoria, clique o **filtro** botão.

    O **atualizar parâmetros de gráfico** painel será exibido.

1. Na **intervalo de tempo**, selecione um intervalo de tempo.

1. Na **funções**, adicionar marcas de seleção para as funções que você deseja exibir.

    ![Atualizar painel de parâmetros de gráfico](media/pim-how-to-use-audit-log/update-chart-parameters.png)

1. Clique em **feito** para exibir o histórico de auditoria filtrado.

## <a name="next-steps"></a>Próximas etapas

- [Exibir a atividade e o histórico de auditoria para funções de recurso do Azure no PIM](azure-pim-resource-rbac.md)
