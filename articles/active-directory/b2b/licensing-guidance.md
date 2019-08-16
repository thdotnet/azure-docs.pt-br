---
title: Diretrizes de licenciamento de colaboração B2B-Azure Active Directory | Microsoft Docs
description: A colaboração do Azure do Azure Active Directory B2B não exige licenças pagas do Azure AD, mas você pode também obter recursos pagos para usuários convidados de B2B
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 08/15/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 081061eae07fa3765d032ad155e59ebf5aa3cbc9
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/15/2019
ms.locfileid: "69512551"
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Diretrizes de licenciamento da colaboração B2B do Azure Active Directory

Com a colaboração do Azure AD (Azure Active Directory) B2B (entre empresas), você pode convidar usuários externos (ou "usuários convidados") para usar seus serviços pagos do Azure AD. Alguns recursos são gratuitos, mas para todos os recursos pagos do Azure AD, você pode convidar até cinco usuários convidados para cada licença de edição do Azure AD que você possui para um funcionário ou um usuário não convidado em seu locatário.

> [!NOTE]
> Consulte [Azure Active Directory preços](https://azure.microsoft.com/pricing/details/active-directory/) para obter detalhes sobre os preços do Azure AD e recursos de colaboração B2B.

O licenciamento do usuário convidado B2B é calculado automaticamente e relatado com base na proporção 1:5. Atualmente, não é possível atribuir licenças de usuário convidado B2B diretamente aos usuários convidados.

Além disso, os usuários convidados podem usar os recursos gratuitos do Azure AD sem requisitos adicionais de licenciamento. Os usuários convidados têm acesso aos recursos gratuitos do Azure AD, mesmo quando você não tem nenhuma licença paga do Azure AD. 

## <a name="examples-calculating-guest-user-licenses"></a>Exemplos: Calculando licenças de usuário convidado
Depois de determinar quantos usuários convidados precisam acessar seus serviços pagos do Azure AD, verifique se você tem licenças pagas do Azure AD suficientes para cobrir os usuários convidados na proporção 1:5 necessária. Estes são alguns exemplos:

- Você deseja convidar 100 usuários para seus aplicativos ou serviços do Azure AD e atribuir acesso de provisionamento e gerenciamento a todos os usuários convidados. Você também deseja exigir MFA e acesso condicional para 50 desses usuários convidados. Para cobrir essa combinação, você precisará de 10 licenças do Azure AD Básico e 10 licenças do Azure AD Premium P1. Se planejar usar recursos do Identity Protection com os usuários convidados, você precisará de licenças do Azure AD Premium P2 na mesma proporção de 1:5 para cobrir os usuários convidados.
- Você deseja convidar 60 usuários que exigirão MFA, portanto, você precisa ter pelo menos 12 licenças do Azure AD Premium P1. Você tem 10 funcionários com licenças do Azure AD Premium P1, o que permite até 50 usuários convidados na proporção de licenciamento 1:5. Você precisará comprar duas licenças Premium P1 adicionais para cobrir os 10 usuários convidados adicionais.

## <a name="next-steps"></a>Próximas etapas

Confira os seguintes recursos sobre a colaboração B2B do Azure AD:

* [Preço do Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/)
* [O que é a colaboração B2B do AD do Azure?](what-is-b2b.md)
* [Perguntas frequentes sobre a colaboração B2B do Azure Active Directory](faq.md)
