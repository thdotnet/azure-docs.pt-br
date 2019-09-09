---
title: Configurar alertas de segurança para funções de recurso do Azure em PIM-Azure Active Directory | Microsoft Docs
description: Saiba como configurar alertas de segurança para funções de recurso do Azure no Azure AD PIM (Privileged Identity Management).
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
ms.date: 04/02/2018
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 177f61392c3e441c891ba1b531301b3dae8c0db2
ms.sourcegitcommit: 95b180c92673507ccaa06f5d4afe9568b38a92fb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/08/2019
ms.locfileid: "70804243"
---
# <a name="configure-security-alerts-for-azure-resource-roles-in-pim"></a>Configurar alertas de segurança para funções de recurso do Azure no PIM
O Azure Active Directory (Azure AD) Privileged Identity Management (PIM) gera alertas quando há atividade suspeita ou não segura em seu ambiente. Quando um alerta é disparado, ele aparece na página Alertas. 

![Recursos do Azure-alerta de listagem de página de alertas, nível de risco e contagem](media/pim-resource-roles-configure-alerts/rbac-alerts-page.png)

## <a name="review-alerts"></a>Revisar alertas
Selecione um alerta para ver um relatório que lista os usuários ou as funções que dispararam o alerta, com o conselho de correção.

![Relatório de alertas mostrando a hora da última verificação, descrição, etapas de mitigação, tipo, gravidade, impacto de segurança e como evitar a próxima vez](media/pim-resource-roles-configure-alerts/rbac-alert-info.png)

## <a name="alerts"></a>Alertas
| Alerta | Severidade | Disparador | Recomendação |
| --- | --- | --- | --- |
| **Muitos proprietários atribuídos a um recurso** |Média |Muitos usuários têm a função de proprietário. |Examine os usuários na lista e reatribua alguns a funções menos privilegiadas. |
| **Muitos proprietários permanentes atribuídos a um recurso** |Média |Muitos usuários são permanentemente atribuídos a uma função. |Revise os usuários na lista e reatribua alguns para exigir ativação para o uso da função. |
| **Duplicar função criada** |Média |Várias funções têm os mesmos critérios. |Use apenas uma dessas funções. |


### <a name="severity"></a>Severidade
* **Alta**: Exige ação imediata devido a uma violação da política. 
* **Média**: Não exige ação imediata, mas sinaliza uma possível violação da política.
* **Baixa**: Não exige ação imediata, mas sugere uma alteração preferencial da política.

## <a name="configure-security-alert-settings"></a>Definir configurações de alerta de segurança
Na página Alertas, vá para **Configurações**.

![Página de alertas com configurações realçadas](media/pim-resource-roles-configure-alerts/rbac-navigate-settings.png)

Personalize configurações nos diferentes alertas para trabalhar com seu ambiente e as metas de segurança.

![Página de configuração de um alerta para habilitar e definir configurações](media/pim-resource-roles-configure-alerts/rbac-alert-settings.png)

## <a name="next-steps"></a>Próximas etapas

- [Definir configurações de função de recurso do Azure no PIM](pim-resource-roles-configure-role-settings.md)
