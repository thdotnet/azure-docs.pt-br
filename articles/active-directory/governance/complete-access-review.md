---
title: Concluir uma revisão de acesso de aplicativos - Azure Active Directory ou grupos | Microsoft Docs
description: Saiba como concluir uma revisão de acesso de membros do grupo ou o acesso de aplicativo em revisões de acesso do Active Directory do Azure.
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
ms.date: 05/22/2019
ms.author: rolyon
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: bae204ec1789f227150adc560d4a292404d23b7e
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66113274"
---
# <a name="complete-an-access-review-of-groups-or-applications-in-azure-ad-access-reviews"></a>Concluir uma revisão de acesso de grupos ou revisões de acesso de aplicativos no Azure AD

Como administrador, você [criar uma revisão de acesso de grupos ou aplicativos](create-access-review.md) e revisores [realizar a análise de acesso](perform-access-review.md). Este artigo descreve como ver os resultados da revisão de acesso e aplicar os resultados.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

## <a name="prerequisites"></a>Pré-requisitos

- Azure AD Premium P2
- Administrador global, administrador de usuário, administrador de segurança ou leitor de segurança

Para obter mais informações, consulte [quais usuários devem ter licenças?](access-reviews-overview.md#which-users-must-have-licenses).

## <a name="view-an-access-review"></a>Exibir uma revisão de acesso

Você pode acompanhar o andamento conforme os revisores concluem suas revisões.

1. Entrar no portal do Azure e abra o [página de revisões de acesso](https://portal.azure.com/#blade/Microsoft_AAD_ERM/DashboardBlade/).

1. No menu à esquerda, clique em **revisões de acesso**.

1. Na lista, clique em uma revisão de acesso.

    Para exibir uma série de acesso de revisões, navegue até a revisão de acesso e você encontrará ocorrências futuras em revisões agendado.

    Sobre o **visão geral** página, você pode ver o progresso. Sem direitos de acesso são alterados no diretório até que a revisão está concluída.

    ![Progresso de revisões de acesso](./media/complete-access-review/overview-progress.png)

1. Se você quiser interromper uma revisão de acesso antes que ele atingir a data de término agendada, clique no **parar** botão.

    Interromper quando uma revisão, os revisores não estarão disponíveis para fornecer respostas. Não é possível reiniciar uma análise depois de ter sido interrompida.

1. Se você não esteja mais interessado na revisão de acesso, você pode excluí-lo clicando o **excluir** botão.

## <a name="apply-the-changes"></a>Aplicar as alterações

Se **resultados de aplicação automática ao recurso** estava habilitado e com base nas suas seleções na **após configurações de conclusão**, autoaplicar será ser executada após a data de término da análise de ou quando você interromper a revisão manualmente.

Se **resultados de aplicação automática ao recurso** não foi habilitado para a revisão, clique em **aplicar** para aplicar manualmente as alterações. Se o acesso do usuário foi negado na análise, quando você clica **aplicar**, o Azure AD removerá sua atribuição de associação ou aplicativo.

![Aplicar alterações de revisão de acesso](./media/complete-access-review/apply-changes.png)

O status da revisão será alterado de **concluído** estados intermediários, como **aplicando** e, finalmente, para o estado **resultados aplicados**. É necessário esperar que os usuários negados, se houver algum, sejam removidos da associação do grupo ou da atribuição do aplicativo em alguns minutos.

Uma revisão de aplicação automática configurada ou selecionar **Aplicar** não afeta um grupo originado em um diretório local ou em um grupo dinâmico. Se você quiser alterar um grupo que se origina localmente, baixe os resultados e aplique essas alterações para a representação do grupo neste diretório.

## <a name="retrieve-the-results"></a>Recuperar os resultados

Para exibir os resultados para uma revisão de acesso único, clique o **resultados** página. Para exibir apenas o acesso do usuário, na caixa de pesquisa, digite o nome de exibição ou o nome UPN de um usuário cujo acesso foi revisado.

![Recuperar os resultados para uma revisão de acesso](./media/complete-access-review/retrieve-results.png)

Para exibir o progresso de uma revisão de acesso do Active Directory for recorrente, clique no **resultados** página.

Para exibir os resultados de uma instância concluída de uma revisão de acesso for recorrente, clique em **examinar histórico**, em seguida, selecione a instância específica da lista de instâncias de revisão de acesso completo, com base no início da instância e a data de término. Os resultados dessa instância podem ser obtidos de **resultados** página.

Para recuperar todos os resultados de uma revisão de acesso, clique o **baixar** botão. O arquivo CSV resultante pode ser visualizado no Excel ou em outros programas que abrem arquivos CSV codificados em UTF-8.

## <a name="remove-users-from-an-access-review"></a>Remover usuários de uma revisão de acesso

 Por padrão, um usuário excluído permanecerá excluído no Azure AD por 30 dias, durante os quais ele pode ser restaurado por um administrador se necessário.  Após 30 dias, esse usuário será excluído permanentemente.  Além disso, usando o portal do Azure Active Directory, um Administrador Global pode explicitamente [excluir permanentemente um usuário excluído recentemente](../fundamentals/active-directory-users-restore.md) antes de atingir o período de tempo.  Após um usuário ter sido excluído permanentemente, os dados posteriores sobre o usuário serão removidos das revisões de acesso ativas.  Auditar informações sobre usuários excluídos na trilha de auditoria.

## <a name="next-steps"></a>Próximas etapas

- [Gerenciar o acesso do usuário com revisões de acesso ao Azure AD](manage-user-access-with-access-reviews.md)
- [Gerenciar o acesso de convidado com revisões de acesso do Azure AD](manage-guest-access-with-access-reviews.md)
- [Criar uma revisão de acesso de grupos ou aplicativos](create-access-review.md)
- [Criar uma revisão de acesso de usuários em uma função administrativa do Azure AD](../privileged-identity-management/pim-how-to-start-security-review.md)
