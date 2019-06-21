---
title: O que são revisões de acesso? - Active Directory do Azure | Microsoft Docs
description: Usando as revisões de acesso do Active Directory do Azure, você pode controlar o acesso de associação e o aplicativo de grupo para atender a governança, gerenciamento de riscos e iniciativas de conformidade em sua organização.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 06/05/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7fcc804db66430598e72e9ebf31a8837eda1cca6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67204602"
---
# <a name="what-are-azure-ad-access-reviews"></a>Revisões de quais são o acesso do Azure AD?

As revisões de acesso do Active Directory (Azure AD) do Azure permitem às organizações gerenciar associações de grupo com eficiência, acesso a aplicativos empresariais e atribuições de função. O acesso do usuário pode ser examinado regularmente para garantir que somente as pessoas corretas tenham acesso contínuo.

Aqui está um vídeo que fornece uma visão geral das revisões de acesso:

>[!VIDEO https://www.youtube.com/embed/kDRjQQ22Wkk]

## <a name="why-are-access-reviews-important"></a>Por que as revisão de acesso são importantes?

O Microsoft Azure Active Directory permite que você colabore internamente em sua organização e com os usuários de organizações externas, como parceiros. Os usuários podem ingressar em grupos, convidar pessoas, conectar-se aos aplicativos de nuvem e trabalhar remotamente com seus dispositivos pessoais ou de trabalho. A conveniência de aproveitar o poder do autoatendimento levou a uma necessidade de melhores recursos de gerenciamento de acesso.

- Conforme novos funcionários ingressam, como garantir que eles tenham o acesso correto para serem produtivos?
- Quando as pessoas mudam de equipes ou saem da empresa, como garantir que seu antigo acesso seja removido, especialmente quando envolve convidados?
- Direitos de acesso excessivos podem levar a conclusões e comprometimentos de auditoria pois indicam uma falta de controle de acesso.
- Você tem que se envolver proativamente com os proprietários do recurso para garantir que eles revisam regularmente quem tem acesso a seus recursos.

## <a name="when-to-use-access-reviews"></a>Quando usar as revisões de acesso?

- **Muitos usuários em funções com privilégios:** Ele é uma boa ideia para verificar quantos usuários têm acesso administrativo, quantos deles são administradores globais, e se há alguma convidou convidados ou parceiros não tiverem sido removidos após a que está sendo atribuído para fazer uma tarefa administrativa. Você pode renovar os usuários de atribuição de função em [funções do Azure AD](../privileged-identity-management/pim-how-to-perform-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) , como os administradores globais, ou [funções de recursos do Azure](../privileged-identity-management/pim-resource-roles-perform-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) como o usuário administrador de acesso no [do Azure AD Privileged Identity Management (PIM)](../privileged-identity-management/pim-configure.md) experiência.
- **Quando a automação é inviável:** Você pode criar regras para associação dinâmica em grupos de segurança ou grupos do Office 365, mas e se os dados de RH não estiverem no Azure AD, ou se os usuários ainda precisarem de acesso após deixar o grupo para treinar sua substituição? Em seguida, você pode criar uma revisão nesse grupo para garantir que aqueles que ainda precisam de acesso devem ter acesso contínuo.
- **Quando um grupo é usado para uma nova finalidade:** Se você tiver um grupo que será sincronizado com o Azure AD, ou se planeja habilitar o aplicativo Salesforce para todos no grupo de equipe de Vendas, será útil solicitar ao proprietário do grupo para revisar a associação ao grupo antes que o grupo seja utilizado em um conteúdo de risco diferente.
- **Acesso de dados críticos de negócios:** para certos recursos, talvez seja necessário pedir que as pessoas fora do departamento de TI regularmente, saiam e forneçam uma justificativa sobre por que eles precisam de acesso para fins de auditoria.
- **Para manter a lista de exceções de uma política:** Em um mundo ideal, todos os usuários seriam seguem as políticas de acesso para proteger o acesso aos recursos da sua organização. No entanto, às vezes, há casos comerciais em que é necessário fazer exceções. Como o administrador de TI, você pode gerenciar essa tarefa, evitar supervisão de exceções à política e fornecer os auditores a comprovação de que essas exceções são revisadas regularmente.
- **Solicite aos proprietários do grupo para confirmar se ainda precisam de convidados em seus grupos:** Acesso de funcionários pode ser automatizado com alguns locais IAM, mas não os convidados. Se um grupo oferece acesso de convidados para conteúdo confidencial da empresa, em seguida, é responsabilidade do proprietário do grupo se confirmar os convidados ainda terão uma necessidade comercial legítima de acesso.
- **Realizar revisões periodicamente:** É possível configurar revisões de acesso recorrentes de usuários em frequências definidas como semanal, mensal, trimestral ou anual e os revisores serão notificados no início de cada revisão. Os revisores podem aprovar ou negar acesso com uma interface amigável e com a ajuda de recomendações inteligentes.

## <a name="where-do-you-create-reviews"></a>Onde você cria as revisões?

Dependendo do que você deseja examinar, você criará sua revisão de acesso no AD do Azure acessar revisões, aplicativos de empresa do Azure AD (em versão prévia) ou Azure AD PIM.

| Direitos de acesso de usuários | Os revisores podem ser | Revisão criada em | Experiência do revisor |
| --- | --- | --- | --- |
| Membros do grupo de segurança</br>Membros do grupo do escritório | Revisores especificados</br>Proprietários do grupo</br>Autorrevisão | Revisões de acesso do Azure AD</br>Grupos do Azure AD | Painel de acesso |
| Atribuído a um aplicativo conectado | Revisores especificados</br>Autorrevisão | Revisões de acesso do Azure AD</br>Aplicativos corporativos do Azure Active Directory (visualização) | Painel de acesso |
| Função do AD do Azure | Revisores especificados</br>Autorrevisão | [Azure AD PIM](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Portal do Azure |
| Função de recurso do Azure | Revisores especificados</br>Autorrevisão | [Azure AD PIM](../privileged-identity-management/pim-resource-roles-start-access-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json) | Portal do Azure |

## <a name="which-users-must-have-licenses"></a>Quais usuários precisam ter licenças?

Cada usuário que interage com revisões de acesso deve ter uma licença paga do Azure AD Premium P2. Os exemplos incluem:

- Administradores que criam uma revisão de acesso
- Examine os proprietários do grupo que executam acesso a um
- Usuários designados como revisores
- Usuários que executam a autorrevisão

Você também pode solicitar que usuários convidados revisem o próprio acesso. Para cada licença paga do Azure AD Premium P2 que você atribui a um dos usuários da sua organização, você pode usar o AD do Azure business-to-business (B2B) para convidar até cinco usuários sob a provisão de usuário externo. Esses usuários convidados também poderão usar os recursos do Azure AD Premium P2. Para obter mais informações, consulte [diretrizes de licenciamento de colaboração de B2B do Azure AD](../b2b/licensing-guidance.md).

Aqui estão alguns cenários de exemplo para ajudá-lo a determinar o número de licenças que você deve ter.

| Cenário | Cálculo | Número necessário de licenças |
| --- | --- | --- |
| Um administrador cria uma revisão de acesso de um grupo com 500 usuários.<br/>Atribui os proprietários do grupo 3 como revisores. | administrador de 1 + 3 proprietários do grupo | 4 |
| Um administrador cria uma revisão de acesso de um grupo com 500 usuários.<br/>Torna a autorrevisão. | administrador de 1 + 500 usuários como revisores Self | 501 |
| Um administrador cria uma revisão de acesso de um grupo com 5 usuários e 25 usuários convidados.<br/>Torna a autorrevisão. | administrador de 1 + 5 usuários como revisores Self<br/>(os usuários convidados são abordados na proporção 1 necessária: 5) | 6 |
| Um administrador cria uma revisão de acesso de um grupo com 5 usuários e 28 usuários convidados.<br/>Torna a autorrevisão. | administrador de 1 + 5 usuários como revisores Self + 1 usuário para incluir os convidados na proporção 1 necessária: 5 | 7 |

Para obter informações sobre como atribuir licenças a seus usos, confira [Atribuir ou remover licenças usando o portal do Azure Active Directory](../fundamentals/license-users-groups.md).

## <a name="learn-about-access-reviews"></a>Saiba mais sobre as revisões de acesso

Para saber mais sobre como criar e executar as revisões de acesso, assista a este breve demonstração:

>[!VIDEO https://www.youtube.com/embed/6KB3TZ8Wi40]

Se você estiver pronto para implantar as revisões de acesso em sua organização, siga estas etapas no vídeo para integrar, treinar seus administradores e criar sua primeira revisão de acesso!

>[!VIDEO https://www.youtube.com/embed/X1SL2uubx9M]

## <a name="license-requirements"></a>Requisitos de licença

[!INCLUDE [Azure AD Premium P2 license](../../../includes/active-directory-p2-license.md)]

## <a name="next-steps"></a>Próximas etapas

- [Criar uma revisão de acesso de grupos ou aplicativos](create-access-review.md)
- [Criar uma revisão de acesso de usuários em uma função administrativa do Azure AD](../privileged-identity-management/pim-how-to-start-security-review.md?toc=%2fazure%2factive-directory%2fgovernance%2ftoc.json)
- [Examinar o acesso a grupos ou aplicativos](perform-access-review.md)
- [Concluir uma revisão de acesso de aplicativos ou grupos](complete-access-review.md)
