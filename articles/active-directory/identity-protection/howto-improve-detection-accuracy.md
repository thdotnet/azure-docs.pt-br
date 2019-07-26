---
title: Como melhorar a precisão da detecção no Azure Active Directory Identity Protection (atualizado) | Microsoft Docs
description: Como melhorar a precisão da detecção no Azure Active Directory Identity Protection (atualizado).
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 32bb8de7970fc167a6a95e9d9c3c71e4e1dc0150
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/19/2019
ms.locfileid: "68333946"
---
# <a name="how-to-improve-the-detection-accuracy"></a>Como: Melhorar a precisão da detecção 

O Identity Protection fornece mecanismos para fornecer comentários ao Azure AD sobre as detecções de risco em seu ambiente. Para fornecer comentários, você pode confirmar o status do evento de entrada ou usuário arriscado detectado. A Microsoft usa esse feedback para tomar medidas sobre as detecções de risco atuais e melhorar a precisão de detecções futuras. 

## <a name="what-is-detection"></a>O que é detecção?

Detecção é o processo de identificar atividades suspeitas em conjunto com suas contas de usuário. As atividades suspeitas que o Azure AD pode detectar são chamadas de [eventos de risco](../reports-monitoring/concept-risk-events.md). O processo de detecção baseia-se em algoritmos adaptáveis de aprendizado de máquina e heurística para detectar eventos de risco para os usuários.

Os resultados de detecção são usados para determinar se os usuários e entradas estão em risco. 

## <a name="how-can-i-improve-the-detection-accuracy"></a>Como melhorar a precisão da detecção?

Como a detecção é um processo automatizado, é possível que o Azure AD relate falsos positivos. Você pode melhorar a precisão da detecção, fornecendo comentários para o Azure AD sobre os resultados da detecção.

Há três maneiras de melhorar a precisão da detecção: Confirme a entrada comprometida, confirme a entrada segura e ignore o risco do usuário. Você pode fazer isso dos seguintes relatórios:

- **Relatório de entradas arriscadas –** no relatório de entradas arriscadas, você pode confirmar que as entradas são seguras ou comprometidas
- **Relatório de usuários arriscados –** no relatório de usuários arriscados, você pode ignorar o risco do usuário 

Seus comentários são processados pelo Azure AD para melhorar a precisão dos resultados da detecção. Normalmente, você pode fornecer comentários como parte de uma investigação de risco de usuário ou risco de entrada. Para obter mais informações, confira [Como investigar usuários e entradas arriscados](howto-investigate-risky-users-signins.md).

## <a name="confirm-compromised"></a>Confirmar comprometido

Confirmar um evento de entrada como comprometido sinaliza ao Azure AD que a entrada não foi autorizada pelo proprietário da identidade. Quando você seleciona "Confirmar comprometido", o Azure AD

- Aumentará o risco de usuário do usuário afetado para alto.
- Ajudará a otimizar o aprendizado de máquina que detecta eventos de risco
- Executará medidas adicionais para proteger ainda mais a sua organização

Para confirmar uma entrada comprometida:

- **O relatório de entradas arriscadas** -esta opção permite que você confirme uma entrada comprometida para um ou mais eventos de entrada.

   ![Ignorar o risco de usuário](./media/howto-improve-detection-accuracy/07.png)

- **A exibição de detalhes do relatório de entradas arriscadas** – essa opção permite que você confirme uma conta comprometida para o evento de entrada selecionado no relatório de entradas arriscadas. 

   ![Ignorar o risco de usuário](./media/howto-improve-detection-accuracy/04.png)
 
## <a name="confirm-safe"></a>Confirmar seguro

Confirmar um evento de entrada como seguro sinaliza ao Azure AD que a entrada **foi** autorizada pelo respectivo proprietário da identidade. Quando você seleciona "Confirmar seguro", o Azure AD:

- Reverterá a contribuição de risco do usuário das entradas selecionadas
- Fechará os eventos de risco subjacentes
- Ajudará a otimizar o aprendizado de máquina que detecta eventos de risco
- Executará medidas adicionais para proteger ainda mais a sua organização
 
Para confirmar uma entrada segura:

- **No relatório de entradas arriscadas** – essa opção permite que você confirme uma entrada segura para um ou mais eventos de entrada.

   ![Ignorar o risco de usuário](./media/howto-improve-detection-accuracy/08.png)

- **Na exibição de detalhes do relatório de entradas arriscadas** – essa opção permite que você confirme uma entrada segura para o evento de entrada selecionado no relatório de entradas arriscadas. 

   ![Ignorar o risco de usuário](./media/howto-improve-detection-accuracy/05.png)

## <a name="dismiss-user-risk"></a>Ignorar o risco de usuário

Se já tiver realizado ações de correção para um usuário de risco ou acreditar que esse usuário foi marcado erroneamente como arriscado, você poderá ignorar o risco do usuário. Ignorar o risco de um usuário restaura o usuário para um estado não arriscado. Todos os eventos de risco e entradas arriscadas anteriores para o usuário selecionado serão ignorados.

Você pode ignorar o risco do usuário relatado:

- **No relatório de usuários arriscados** – essa opção permite ignorar o risco do usuário para um ou mais usuários selecionados.

   ![Ignorar o risco de usuário](./media/howto-improve-detection-accuracy/02.png)

- **Na exibição de Detalhes** – essa opção permite ignorar o risco do usuário para o usuário selecionado no relatório de risco do usuário. 

   ![Ignorar o risco de usuário](./media/howto-improve-detection-accuracy/01.png)

**O que você deve saber:**

- Você não pode reverter essa ação.
- Pode levar alguns minutos para que essa ação seja concluída, motivo pelo qual você não deve reenviar sua solicitação.
- Você só pode executar essa ação se o AD gerencia as credenciais do usuário. 

## <a name="best-practices"></a>Práticas recomendadas

Ignorar o risco de um usuário é uma maneira de desbloqueá-los se eles tiverem sido bloqueados pela política de risco do usuário e não puderem ser remediados por não ter a redefinição de senha e/ou a MFA habilitada. Nessa situação, é melhor garantir que o usuário se registre então para redefinição de senha e MFA, de modo que ele possa se autocorrigir em caso de quaisquer eventos de risco futuros.

## <a name="next-steps"></a>Próximas etapas

Para obter uma visão geral do Azure AD Identity Protection, veja a [Visão geral do Azure AD Identity Protection](overview-v2.md).
