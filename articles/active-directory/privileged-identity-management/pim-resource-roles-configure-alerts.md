---
title: Configurar alertas de segurança para funções de recurso do Azure no PIM - Azure Active Directory | Microsoft Docs
description: Saiba como configurar alertas de segurança para funções de recurso do Azure no Azure AD PIM (Privileged Identity Management).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: c15e4080308c3e7e2ff54312cd91fa1f3d68668a
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65602369"
---
# <a name="configure-security-alerts-for-azure-resource-roles-in-pim"></a>Configurar alertas de segurança para funções de recurso do Azure no PIM
Azure Active Directory (Azure AD) PIM Privileged Identity Management () gera alertas quando há atividade suspeita ou não segura em seu ambiente. Quando um alerta é disparado, ele aparece na página Alertas. 

![Página de alertas](media/pim-resource-roles-configure-alerts/rbac-alerts-page.png)

## <a name="review-alerts"></a>Revisar alertas
Selecione um alerta para ver um relatório que lista os usuários ou as funções que dispararam o alerta, com o conselho de correção.

![Relatório de alerta](media/pim-resource-roles-configure-alerts/rbac-alert-info.png)

## <a name="alerts"></a>Alertas
| Alerta | Severity | Gatilho | Recomendações |
| --- | --- | --- | --- |
| **Muitos proprietários atribuídos a um recurso** |Média |Muitos usuários têm a função de proprietário. |Examine os usuários na lista e reatribua alguns a funções menos privilegiadas. |
| **Muitos proprietários permanentes atribuídos a um recurso** |Média |Muitos usuários são permanentemente atribuídos a uma função. |Revise os usuários na lista e reatribua alguns para exigir ativação para o uso da função. |
| **Duplicar função criada** |Média |Várias funções têm os mesmos critérios. |Use apenas uma dessas funções. |


### <a name="severity"></a>Severity
* **Alta**: Exige ação imediata devido a uma violação da política. 
* **Média**: Não exige ação imediata, mas sinaliza uma possível violação da política.
* **Baixa**: Não exige ação imediata, mas sugere uma alteração preferencial da política.

## <a name="configure-security-alert-settings"></a>Definir configurações de alerta de segurança
Na página Alertas, vá para **Configurações**.
![Configurações](media/pim-resource-roles-configure-alerts/rbac-navigate-settings.png)

Personalize configurações nos diferentes alertas para trabalhar com seu ambiente e as metas de segurança.
![Personalizar as configurações](media/pim-resource-roles-configure-alerts/rbac-alert-settings.png)

## <a name="next-steps"></a>Próximas etapas

- [Configurar alertas de segurança para funções de recurso do Azure no PIM](pim-resource-roles-configure-alerts.md)
