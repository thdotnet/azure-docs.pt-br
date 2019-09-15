---
title: Gerenciar o acesso aos recursos do Azure usando o RBAC e a API REST – Azure | Microsoft Docs
description: Saiba como gerenciar o acesso de usuários, grupos e aplicativos com o Azure usando o controle de acesso baseado em função (RBAC) e a API REST. Isso inclui como listar o acesso, conceder o acesso e remover o acesso.
services: active-directory
documentationcenter: na
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 1f90228a-7aac-4ea7-ad82-b57d222ab128
ms.service: role-based-access-control
ms.workload: multiple
ms.tgt_pltfrm: rest-api
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 86ee030e8c97cf3033b9d2d76b8125c64ecf8065
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2019
ms.locfileid: "70996463"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-the-rest-api"></a>Gerenciar o acesso aos recursos do Azure usando o RBAC e a API REST

O [RBAC (controle de acesso baseado em função)](overview.md) serve para gerenciar o acesso aos recursos do Azure. Este artigo descreve como gerenciar o acesso de usuários, grupos e aplicativos usando RBAC e API REST.

## <a name="list-access"></a>Relacionar acesso

No RBAC, para listar o acesso, você lista as atribuições de função. Para listar as atribuições de função, use uma das APIs REST [Atribuições de Função - Listar](/rest/api/authorization/roleassignments/list). Para refinar seus resultados, especifique um escopo e um filtro opcional.

1. Comece com a solicitação a seguir:

    ```http
    GET https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version=2015-07-01&$filter={filter}
    ```

1. Dentro do URI, substitua *{scope}* pelo escopo para o qual você deseja listar as funções de atribuição.

    | Escopo | Tipo |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | Grupo de gerenciamento |
    | `subscriptions/{subscriptionId1}` | Assinatura |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Grupo de recursos |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/Microsoft.Web/sites/mysite1` | Recurso |

    No exemplo anterior, Microsoft. Web é um provedor de recursos que se refere a uma instância do serviço de aplicativo. Da mesma forma, você pode usar qualquer outro provedor de recursos e especificar o escopo. Para obter mais informações, consulte [provedores de recursos do Azure e tipos](../azure-resource-manager/resource-manager-supported-services.md) e [operações de provedor de recursos Azure Resource Manager](resource-provider-operations.md)com suporte.  
     
1. Substitua *{filter}* pela condição que você deseja aplicar a fim de filtrar a lista de atribuições de função.

    | Filtro | Descrição |
    | --- | --- |
    | `$filter=atScope()` | Lista as atribuições de função somente para o escopo especificado, não incluindo as atribuições de função em Subescopos. |
    | `$filter=principalId%20eq%20'{objectId}'` | Lista as atribuições de função para um usuário, grupo ou entidade de serviço especificado. |
    | `$filter=assignedTo('{objectId}')` | Lista as atribuições de função para um usuário ou entidade de serviço especificada. Se o usuário for membro de um grupo que tem uma atribuição de função, essa atribuição de função também será listada. Esse filtro é transitivo para grupos, o que significa que, se o usuário for membro de um grupo e esse grupo for membro de outro grupo que tenha uma atribuição de função, essa atribuição de função também será listada. Esse filtro só aceita uma ID de objeto para um usuário ou uma entidade de serviço. Não é possível passar uma ID de objeto para um grupo. |

## <a name="grant-access"></a>Permitir acesso

No RBAC, para conceder acesso, você cria uma atribuição de função. Para criar uma atribuição de função, use a API REST [Atribuições de Função - Criar](/rest/api/authorization/roleassignments/create) e especifique a entidade de segurança, definição de função e escopo. Para chamar essa API, é necessário ter acesso à operação `Microsoft.Authorization/roleAssignments/write`. Das funções internas, somente [Proprietário](built-in-roles.md#owner) e [Administrador do Acesso do Usuário](built-in-roles.md#user-access-administrator) recebem permissão para acessar essa operação.

1. Use a API REST [Definições de Função - Listar](/rest/api/authorization/roledefinitions/list) ou veja as [Funções internas](built-in-roles.md) para obter o identificador da definição de função que você deseja atribuir.

1. Use uma ferramenta GUID para gerar um identificador exclusivo que será usado para atribuição de função personalizada. O identificador tem o formato: `00000000-0000-0000-0000-000000000000`

1. Comece com a solicitação e corpo a seguir:

    ```http
    PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

    ```json
    {
      "properties": {
        "roleDefinitionId": "/{scope}/providers/Microsoft.Authorization/roleDefinitions/{roleDefinitionId}",
        "principalId": "{principalId}"
      }
    }
    ```

1. Dentro do URI, substitua *{scope}* pelo escopo da atribuição de função.

    | Escopo | Tipo |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | Grupo de gerenciamento |
    | `subscriptions/{subscriptionId1}` | Assinatura |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Grupo de recursos |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/microsoft.web/sites/mysite1` | Recurso |

1. Substitua *{roleAssignmentName}* pelo identificador GUID da atribuição de função.

1. No corpo da solicitação, substitua *{Scope}* pelo escopo da atribuição de função.

    | Escopo | Tipo |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | Grupo de gerenciamento |
    | `subscriptions/{subscriptionId1}` | Assinatura |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Grupo de recursos |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/microsoft.web/sites/mysite1` | Recurso |

1. Substitua *{roleDefinitionId}* pelo identificador de definição da função.

1. Substitua *{principalId}* com o identificador de objeto do usuário, grupo ou entidade de serviço que receberá a função.

## <a name="remove-access"></a>Remove access

No RBAC, para remover o acesso, você deve remover uma atribuição de função. Para remover uma atribuição de função, use a API REST [Atribuição de Função – Excluir](/rest/api/authorization/roleassignments/delete). Para chamar essa API, é necessário ter acesso à operação `Microsoft.Authorization/roleAssignments/delete`. Das funções internas, somente [Proprietário](built-in-roles.md#owner) e [Administrador do Acesso do Usuário](built-in-roles.md#user-access-administrator) recebem permissão para acessar essa operação.

1. Obtenha o identificador da atribuição de função (GUID). Esse identificador retorna quando você cria pela primeira vez a atribuição de função, ou você pode obtê-lo listando as atribuições de função.

1. Comece com a solicitação a seguir:

    ```http
    DELETE https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{roleAssignmentName}?api-version=2015-07-01
    ```

1. Dentro do URI, substitua *{scope}* pelo escopo para remoção da atribuição de função.

    | Escopo | Tipo |
    | --- | --- |
    | `providers/Microsoft.Management/managementGroups/{groupId1}` | Grupo de gerenciamento |
    | `subscriptions/{subscriptionId1}` | Assinatura |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1` | Grupo de recursos |
    | `subscriptions/{subscriptionId1}/resourceGroups/myresourcegroup1/ providers/microsoft.web/sites/mysite1` | Recurso |

1. Substitua *{roleAssignmentName}* pelo identificador GUID da atribuição de função.

## <a name="next-steps"></a>Próximas etapas

- [Implantar recursos com modelos do Resource Manager e a API REST do Resource Manager](../azure-resource-manager/resource-group-template-deploy-rest.md)
- [Referência de API REST do Azure](/rest/api/azure/)
- [Criar funções personalizadas para recursos do Azure usando a API REST](custom-roles-rest.md)
