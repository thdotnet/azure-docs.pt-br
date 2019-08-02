---
title: CreateUiDefitinion. JSON para a experiência de criação do aplicativo gerenciado do Azure | Microsoft Docs
description: Descreve como criar definições de interface do usuário para aplicativos gerenciados do Azure
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/26/2019
ms.author: tomfitz
ms.openlocfilehash: 50bbaf740a67d3830df2d0447b9522153cb8c93c
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68619087"
---
# <a name="createuidefitinionjson-for-azure-managed-applications-create-experience"></a>CreateUiDefitinion. JSON para a experiência de criação do aplicativo gerenciado do Azure
Este documento apresenta os principais conceitos do arquivo **createUiDefinition. JSON** que o portal do Azure usa para definir a interface do usuário ao criar um aplicativo gerenciado.

O modelo é o seguinte

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
   "handler": "Microsoft.Azure.CreateUIDef",
   "version": "0.1.2-preview",
   "parameters": {
      "basics": [ ],
      "steps": [ ],
      "outputs": { }
   }
}
```

Um CreateUiDefinition sempre contém três propriedades: 

* handler
* versão
* parameters

O manipulador deve sempre ser `Microsoft.Azure.CreateUIDef`, e a versão mais recente com `0.1.2-preview`suporte é.

O esquema da propriedade parameters depende da combinação do manipulador e da versão especificados. Para aplicativos gerenciados, as propriedades com suporte são `basics`, `steps` e `outputs`. As propriedades basic e steps contêm os [elementos](create-uidefinition-elements.md), como caixas de texto e listas suspensas, que serão exibidos no portal do Azure. A propriedade outputs é usada para mapear os valores de saída de elementos especificados para os parâmetros do modelo de implantação do Azure Resource Manager.

A inclusão de `$schema` é opcional, mas recomendada. Se especificado, o valor de `version` deve corresponder à versão dentro do URI `$schema`.

Você pode usar um editor de JSON para criar sua definição de interface do usuário e testá-la na [área restrita da definição da interface do usuário](https://portal.azure.com/?feature.customPortal=false&#blade/Microsoft_Azure_CreateUIDef/SandboxBlade) para visualizá-la. Para obter mais informações sobre a área restrita, consulte [testar sua interface do portal para aplicativos gerenciados do Azure](test-createuidefinition.md).

## <a name="basics"></a>Noções Básicas
Noções básicas é a primeira etapa gerada quando o portal do Azure analisa o arquivo. Além de exibir os elementos especificados em `basics`, o portal injeta elementos para que os usuários escolham a assinatura, o grupo de recursos e o local da implantação. Quando possível, os elementos que consultam parâmetros de toda a implantação, como o nome de um cluster ou credenciais de administrador, devem ir nesta etapa.

Se o comportamento de um elemento depender da assinatura, do grupo de recursos ou do local do usuário, esse elemento não poderá ser usado em Noções básicas. Por exemplo, **Microsoft.Compute.SizeSelector** depende da assinatura e do local para determinar a lista de tamanhos disponíveis para o usuário. Portanto, **Microsoft.Compute.SizeSelector** só pode ser usado em steps. Geralmente, apenas os elementos no namespace **Microsoft.Common** podem ser usados em basics. Embora alguns elementos em outros namespaces (como **Microsoft. COMPUTE. Credentials**) que não dependem do contexto do usuário ainda sejam permitidos.

## <a name="steps"></a>Etapas
A propriedade steps pode conter uma ou mais etapas adicionais para exibir depois de basics, cada uma contendo um ou mais elementos. Considere adicionar etapas por função ou camada do aplicativo que está sendo implantado. Por exemplo, adicione uma etapa para entradas de nó mestre e uma etapa para os nós de trabalho em um cluster.

## <a name="outputs"></a>Saídas
O portal do Azure usa a propriedade `outputs` para mapear os elementos de `basics` e `steps` para os parâmetros do modelo de implantação do Azure Resource Manager. As chaves do dicionário são os nomes dos parâmetros do modelo, e os valores são propriedades dos objetos da saída dos elementos referenciados.

Para definir o nome de recurso do aplicativo gerenciado, inclua um valor chamado `applicationResourceName` na propriedade outputs. Se você não definir esse valor, o aplicativo atribuirá um GUID para o nome. Você pode incluir uma caixa de texto na interface do usuário que solicita um nome de usuário.

```json
"outputs": {
    "vmName": "[steps('appSettings').vmName]",
    "trialOrProduction": "[steps('appSettings').trialOrProd]",
    "userName": "[steps('vmCredentials').adminUsername]",
    "pwd": "[steps('vmCredentials').vmPwd.password]",
    "applicationResourceName": "[steps('appSettings').vmName]"
}
```

## <a name="functions"></a>Funções
O CreateUiDefinition fornece [funções](create-uidefinition-functions.md) para trabalhar com entradas e saídas de elementos e recursos como condicionais. Essas funções são semelhantes na sintaxe e na funcionalidade para Azure Resource Manager funções de modelo.

## <a name="next-steps"></a>Próximas etapas
O próprio arquivo createUiDefinition.json tem um esquema simples. A profundidade real dele é proveniente de todos os elementos e funções com suporte. Esses itens são descritos mais detalhadamente em:

- [Elementos](create-uidefinition-elements.md)
- [Funções](create-uidefinition-functions.md)

Um esquema JSON atual para createUiDefinition está disponível aqui: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.

Para ver um arquivo de interface do usuário de exemplo, consulte [createUiDefinition.json](https://github.com/Azure/azure-managedapp-samples/blob/master/samples/201-managed-app-using-existing-vnet/createUiDefinition.json).
