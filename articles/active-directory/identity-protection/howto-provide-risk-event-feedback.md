---
title: Fornecer comentários sobre eventos de risco do Azure AD Identity Protection - Azure Active Directory
description: Como e por que deve você fornecer comentários sobre eventos de risco do Identity Protection.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 66d53590e89afb1a903b22ff60e32871a1502ada
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827899"
---
# <a name="how-to-give-risk-feedback-in-azure-ad-identity-protection"></a>Como: Fornecer comentários de risco no Azure AD Identity Protection

O Azure AD Identity Protection permite que você fornecer comentários sobre a avaliação de risco. O documento a seguir lista os cenários onde você deseja fornecer comentários sobre a avaliação de risco do Azure AD Identity Protection e como podemos incorporá-la.

## <a name="what-is-a-detection"></a>O que é uma detecção?

Uma detecção de proteção de identidade é um indicador de atividade suspeita de uma perspectiva de risco de identidade. Essas atividades suspeitas são chamadas de eventos de risco. Essas detecções com base em identidade podem ser baseadas em heurística, aprendizado de máquina ou podem vir de produtos de parceiros. Essas detecções são usadas para determinar o risco do usuário e o risco de entrada

* Risco do usuário representa a probabilidade de que uma identidade é comprometida.
* Risco de entrada representa a probabilidade de uma entrada seja comprometida (por exemplo, na entrada não está autorizada pelo proprietário da identidade).

## <a name="why-should-i-give-risk-feedback-to-azure-ads-risk-assessments"></a>Por que deve fornecer comentários de risco para avaliações de risco do Azure AD? 

Há várias razões por que você deve fornecer comentários de risco do Azure AD:

1. **Você encontrou a avaliação de risco de usuário ou de entrada do Azure AD incorreto**. Por exemplo, uma entrada mostrado no relatório 'Entradas arriscadas' teve benigno e todas as detecções que entrar foram falsos positivos.
1. **Validar o usuário do Azure AD ou avaliação de risco de entrada está correta,**. Por exemplo, uma entrada mostrado no relatório 'Entradas arriscadas' foi realmente mal-intencionado e deseja que o Azure AD para saber que todas as detecções que entrar eram verdadeiros positivos.
1. **Corrigida ao risco em que o usuário fora do Azure AD Identity Protection** e deseja que o nível de risco do usuário a ser atualizada.

## <a name="how-does-azure-ad-use-my-risk-feedback"></a>Como o Azure AD usar meus comentários de risco?

Azure AD usa seus comentários para atualizar o risco do subjacente usuário e/ou entrar. Esse feedback ajuda a proteger o usuário final. Por exemplo, depois de confirmar que uma entrada estiver comprometido, imediatamente com o Azure AD aumenta o risco do usuário e da agregação risco de entrada (risco em tempo real não) para alta. Se esse usuário é incluído em sua política de risco do usuário para forçar os usuários de alto risco com segurança redefinir suas senhas, o usuário será automaticamente corrigirá a mesmo na próxima vez que entrar.

## <a name="how-should-i-give-risk-feedback-and-what-happens-under-the-hood"></a>Como posso dar o risco de comentários e o que acontece nos bastidores?

Aqui estão os cenários e os mecanismos para fornecer comentários de risco para o Azure AD.

| Cenário | Como enviar comentários? | O que acontece nos bastidores? | Observações |
| --- | --- | --- | --- |
| **Entrar não comprometida (falso positivo)** <br> Relatório de 'Entradas arriscadas' mostra uma entrada em risco [corre o risco de estado = em risco], mas que a entrada não foi comprometido. | Selecione a entrada do e clique em 'Confirmar entrar safe'. | Azure AD será movido risco de agregação na entrada como nenhuma [corre o risco de estado = confirmado seguro; (Agregar) de nível de risco =-] e reverterá seu impacto no risco do usuário. | Atualmente, a opção 'Confirmar entrar safe' só está disponível no relatório 'Entradas arriscadas'. |
| **Entrar comprometida (verdadeiro positivo)** <br> Relatório de 'Entradas arriscadas' mostra uma entrada em risco [corre o risco de estado = em risco] com baixo risco [(agregar) de nível de risco = baixo] e essa entrada, de fato, foi comprometida. | Selecione a entrada e clique em 'Confirmar entrar comprometida'. | Azure AD será movido risco de agregação na entrada e o risco de usuário para alta [corre o risco de estado = confirmado comprometido; Nível de risco = alta]. | Atualmente, a opção 'Confirmar entrar comprometida' só está disponível no relatório 'Entradas arriscadas'. |
| **Usuário comprometido (verdadeiro positivo)** <br> Relatório de 'Usuários arriscados' mostra um usuário em risco [corre o risco de estado = em risco] com baixo risco [nível de risco = baixo] e que o usuário realmente foi comprometido. | Selecione o usuário e clique em 'User de confirmar comprometida'. | Azure AD moverá o risco do usuário para alta [corre o risco de estado = confirmado comprometido; Nível de risco = alto] e adicione uma nova detecção 'Administrador confirmada usuário comprometido'. | Atualmente, a opção 'User de confirmar comprometida' só está disponível no relatório 'Usuários arriscados'. <br> A detecção de 'Administrador confirmada usuário comprometido' é mostrada na guia 'eventos de risco não está vinculados a uma entrada na' no relatório 'Usuários arriscados'. |
| **Usuário remediadas fora do Azure AD Identity Protection (verdadeiro positivo + Remediated)** <br> Relatório de 'Usuários arriscados' mostra um usuário em risco e eu tiver remediadas subsequentemente o usuário fora do Azure AD Identity Protection. | 1. Selecione o usuário e clique em 'User de confirmar comprometida'. (Esse processo confirma ao AD do Azure que o usuário realmente foi comprometido.) <br> 2. Aguarde até que o nível do usuário' risco' para ir para alta. (Esse oferece de tempo do Azure AD o tempo necessário para levar os comentários acima para o mecanismo de risco.) <br> 3. Selecione o usuário e clique em 'Ignorar o risco do usuário'. (Esse processo confirma ao AD do Azure que o usuário não seja comprometido.) |  AD do Azure move o risco do usuário para none [corre o risco de estado = ignorado; Nível de risco =-] e fecha o risco em todas as existentes entradas de risco do Active Directory. | Clicar em 'Ignorar risco do usuário' fechará todos os riscos no usuário e passado entradas. Esta ação não pode ser desfeita. |
| **Usuário não comprometido (falso positivo)** <br> Mostra o relatório 'Usuários arriscados' no usuário em risco, mas o usuário não seja comprometido. | Selecione o usuário e clique em 'Ignorar o risco do usuário'. (Esse processo confirma ao AD do Azure que o usuário não seja comprometido.) | AD do Azure move o risco do usuário para none [corre o risco de estado = ignorado; Nível de risco =-]. | Clicar em 'Ignorar risco do usuário' fechará todos os riscos no usuário e passado entradas. Esta ação não pode ser desfeita. |
| Quero fechar o risco do usuário, mas não tenho certeza se o usuário está comprometido / seguro. | Selecione o usuário e clique em 'Ignorar o risco do usuário'. (Esse processo confirma ao AD do Azure que o usuário não seja comprometido.) | AD do Azure move o risco do usuário para none [corre o risco de estado = ignorado; Nível de risco =-]. | Clicar em 'Ignorar risco do usuário' fechará todos os riscos no usuário e passado entradas. Esta ação não pode ser desfeita. Recomendamos que você corrigir o usuário clicando em 'Redefinir senha' ou solicitar o usuário a redefinição/alteração com segurança suas credenciais. |

Comentários sobre eventos de risco do usuário no Identity Protection é processado off-line e podem levar algum tempo para atualizar. O processamento da coluna de estado de risco fornecerá o estado atual do processamento de comentários.

![Estado de processamento de relatório do usuário arriscado de risco](./media/howto-provide-risk-event-feedback/risky-users-provide-feedback.png)

## <a name="next-steps"></a>Próximas etapas

[Referência de eventos de risco do Azure Active Directory Identity Protection](risk-events-reference.md)