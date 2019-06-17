---
title: Lista Negar as atribuições de recursos do Azure usando o portal do Azure | Microsoft Docs
description: Saiba como listar os usuários, grupos, entidades de serviço e identidades gerenciadas negou acesso a ações específicas de recursos do Azure em escopos específicos usando o portal do Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/10/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 21ffb1a2539a2e724a91dd3b2818270a5e573ef8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67127481"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-azure-portal"></a>Lista Negar as atribuições de recursos do Azure usando o portal do Azure

As [atribuições de negação](deny-assignments.md) impedem que os usuários executem ações específicas, mesmo que uma atribuição de função conceda o acesso a elas. Este artigo descreve como listar negar atribuições usando o portal do Azure.

> [!NOTE]
> Você não pode criar diretamente seus próprios negar atribuições. Para obter informações sobre como negar as atribuições são criadas, consulte [negar atribuições](deny-assignments.md).

## <a name="prerequisites"></a>Pré-requisitos

Para obter informações sobre uma atribuição de deny, você deve ter:

- `Microsoft.Authorization/denyAssignments/read` permissão, que está incluído na maioria [funções internas para recursos do Azure](built-in-roles.md).

## <a name="list-deny-assignments"></a>Lista as atribuições de negação

Siga estas etapas para listar as atribuições no escopo do grupo de gerenciamento ou da assinatura de negação.

1. No portal do Azure, clique em **Todos os serviços** e, em seguida, em **Grupos de gerenciamento** ou **Assinaturas**.

1. Clique no grupo de gerenciamento ou a assinatura que você deseja listar.

1. Clique em **Controle de acesso (IAM)** .

1. Clique na guia **Negar atribuições** (ou clique no botão **Visualizar** na peça Visualizar negar atribuições).

    Se houver alguma atribuição de negação neste escopo ou herdada para esse escopo, ela será listada.

    ![Controle de acesso - guia Negar atribuições](./media/deny-assignments-portal/access-control-deny-assignments.png)

1. Para exibir colunas adicionais, clique em **Editar colunas**.

    ![Negar atribuições - colunas](./media/deny-assignments-portal/deny-assignments-columns.png)

    |  |  |
    | --- | --- |
    | **Nome** | Nome da atribuição de negação. |
    | **Tipo principal** | Usuário, grupo, grupo definido pelo sistema ou principal do serviço. |
    | **Negado**  | Nome do principal de segurança incluído na atribuição de negação. |
    | **Id** | Identificador exclusivo para a atribuição de negação. |
    | **Principais Excluídos** | Se existem entidades de segurança que são excluídas da atribuição de negação. |
    | **Não se aplica a crianças** | Se a atribuição de negação é herdada para subescópios. |
    | **Protegidos do sistema** | Se a atribuição de negação é gerenciada pelo Azure. Atualmente, sempre sim. |
    | **Escopo** | Grupo de gerenciamento, assinatura, grupo de recursos ou recurso. |

1. Adicione uma marca de seleção a qualquer um dos itens ativados e clique em **OK** para exibir as colunas selecionadas.

## <a name="list-details-about-a-deny-assignment"></a>Lista detalhes sobre uma atribuição de negação

Siga estas etapas para listar detalhes adicionais sobre uma atribuição de negação.

1. Abra o painel **Negar atribuições** conforme descrito na seção anterior.

1. Clique no nome da atribuição de negação para abrir a folha **Usuários**.

    ![Negar designação - usuários](./media/deny-assignments-portal/deny-assignment-users.png)

    A folha **Usuários** inclui as duas seções a seguir.

    |  |  |
    | --- | --- |
    | **Negar atribuição se aplica a**  | Entidades de segurança às quais a atribuição de negação se aplica. |
    | **Negar a exclui da atribuição** | Entidades de segurança que são excluídas da atribuição de negação. |

    **A diretiva definida pelo sistema** representa todos os usuários, grupos, entidades de serviço e identidades gerenciadas em um Azure Active Directory.

1. Para ver uma lista das permissões que são negados, clique em **permissões negadas**.

    ![Negar - atribuição de permissões negado](./media/deny-assignments-portal/deny-assignment-denied-permissions.png)

    | Tipo de ação | DESCRIÇÃO |
    | --- | --- |
    | **Ações**  | Operações de gerenciamento negado. |
    | **NotActions** | Operações de gerenciamento excluídas da operação de gerenciamento negado. |
    | **DataActions**  | Operações de dados negado. |
    | **NotDataActions** | Operações de dados excluídas da operação de dados negados. |

    Para o exemplo mostrado na captura de tela anterior, as seguintes são as permissões efetivas:

    - Todas as operações de armazenamento no plano de dados são negadas, exceto para operações de computação.

1. Para ver as propriedades de uma atribuição de negação, clique em **Propriedades**.

    ![Negar atribuição - propriedades](./media/deny-assignments-portal/deny-assignment-properties.png)

    Na folha **Propriedades**, você pode ver o nome, a ID, a descrição e o escopo da atribuição de negação. A opção **Não se aplica a filhos** indica se a atribuição de negação é herdada para os subescópios. A chave **System protected** indica se essa atribuição de negação é gerenciada pelo Azure. Atualmente, isso é **Sim** em todos os casos.

## <a name="next-steps"></a>Próximas etapas

* [Compreender atribuições de negação dos recursos do Azure](deny-assignments.md)
* [Lista Negar as atribuições de recursos do Azure usando o Azure PowerShell](deny-assignments-powershell.md)
