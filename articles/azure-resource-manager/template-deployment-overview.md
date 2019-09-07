---
title: Modelos do Azure Resource Manager
description: Descreve como usar modelos de Azure Resource Manager para a implantação de recursos.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: tomfitz
ms.openlocfilehash: 4f273a04322246a2b4112c0b5b8a062732bab555
ms.sourcegitcommit: 88ae4396fec7ea56011f896a7c7c79af867c90a1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70390746"
---
# <a name="azure-resource-manager-templates"></a>Modelos do Azure Resource Manager

Com a mudança para a nuvem, muitas equipes adotaram métodos de desenvolvimento Agile. Essas equipes iteram rapidamente. Eles precisam implantar suas soluções de forma repetida e confiável na nuvem. A divisão entre operações e desenvolvimento desapareceu conforme as equipes precisam gerenciar a infraestrutura e o código-fonte em um único processo.

Uma solução para esses desafios é automatizar a implantação e usar a prática da infraestrutura como código. Você usa o código para definir o que precisa ser implantado e gerenciar esse código por meio do mesmo processo que o código-fonte. Você armazena a infraestrutura como código em um repositório de origem e a versão.

Azure Resource Manager permite implementar a infraestrutura como código por meio de modelos do Resource Manager. O modelo é um arquivo JavaScript Object Notation (JSON) que contém a infraestrutura e a configuração da sua solução do Azure. O modelo usa a sintaxe declarativa, que permite que você declare o que pretende implantar sem precisar escrever a sequência de comandos de programação para criá-lo. No modelo, você especifica os recursos a serem implantados e as propriedades desses recursos.

## <a name="benefits-of-resource-manager-templates"></a>Benefícios dos modelos do Resource Manager

Os modelos do Resource Manager fornecem vários benefícios. Você pode:

* Implante, gerencie e monitore todos os recursos de sua solução como um grupo, em vez de manipular esses recursos individualmente.

* Implante repetidamente sua solução em todo o ciclo de vida de desenvolvimento e tenha confiança de que seus recursos sejam implantados em um estado consistente.

* Gerencie sua infraestrutura por meio de modelos declarativos em vez de scripts.

* Defina as dependências entre os recursos para que eles sejam implantados na ordem correta.

* Aproveite as novas versões e serviços imediatamente porque não há nenhum trabalho intermediário exigido por outras partes.

## <a name="template-file"></a>Arquivo de modelo

Em seu modelo, você pode escrever [expressões de modelo](template-expressions.md) que estendam os recursos do JSON. Essas expressões fazem uso das [funções](resource-group-template-functions.md) fornecidas pelo Resource Manager.

O modelo tem as seguintes seções:

* [Parâmetros](template-parameters.md) – forneça valores durante a implantação que permitem que o mesmo modelo seja usado com ambientes diferentes.

* [Variáveis](template-variables.md) – defina valores que são usados em seus modelos.

* [Funções definidas pelo usuário](template-user-defined-functions.md) – crie funções personalizadas que simplificam seu modelo.

* [Recursos](resource-group-authoring-templates.md#resources) -especifique os recursos a serem implantados.

* [Saídas](template-outputs.md) – retornam valores dos recursos implantados.

## <a name="dependencies"></a>Dependências

O Azure Resource Manager analisa as dependências para garantir que os recursos sejam criados na ordem correta. Se um recurso depende de um valor de outro recurso (como uma máquina virtual que precisa de uma conta de armazenamento para discos), você pode definir uma dependência. Para saber mais, confira [Definindo as dependências nos modelos do Gerenciador de Recursos do Azure](resource-group-define-dependencies.md).

## <a name="conditional-deployment"></a>Implantação condicional

Você pode adicionar um recurso ao seu modelo e, opcionalmente, implantá-lo. Normalmente, você passa um valor de parâmetro que indica se o recurso precisa ser implantado. Para obter mais informações, consulte [implantação condicional em modelos do Resource Manager](conditional-resource-deployment.md).

## <a name="create-multiple-instances"></a>Criar múltiplas instâncias

Em vez de blocos repetitivos de JSON muitas vezes em seu modelo, você pode usar um elemento de cópia para especificar mais de uma instância de uma variável, propriedade ou recurso. Para obter mais informações, consulte [recurso, propriedade ou iteração de variável em modelos de Azure Resource Manager](resource-group-create-multiple.md).

## <a name="export-templates"></a>Exportar modelos

Você pode obter um modelo para um grupo de recursos existente exportando o estado atual do grupo de recursos ou exibindo o modelo usado para uma implantação específica. A exibição do [modelo exportado](export-template-portal.md) é uma maneira útil de saber mais sobre a sintaxe do modelo.

Quando você cria uma solução no Portal, ela inclui automaticamente um modelo de implantação. Você não precisa criar seu modelo do zero, pois é possível iniciar com o modelo da sua solução e personalizá-lo para atender às suas necessidades específicas. Para obter uma amostra, confira [Início Rápido: Crie e implante modelos do Azure Resource Manager usando o portal do Azure](./resource-manager-quickstart-create-templates-use-the-portal.md).

## <a name="template-deployment-process"></a>Processo de Implantação de modelo

Quando você implanta um modelo, o Resource Manager analisa o modelo e converte sua sintaxe em operações da API REST. Por exemplo, quando o Gerenciador de Recursos recebe um modelo com a seguinte definição de recurso:

```json
"resources": [
  {
    "apiVersion": "2016-01-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "mystorageaccount",
    "location": "westus",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {
    }
  }
]
```

Converte a definição para a seguinte operação de API REST, que é enviada para o provedor de recursos Microsoft.Storage:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/mystorageaccount?api-version=2016-01-01
REQUEST BODY
{
  "location": "westus",
  "properties": {
  }
  "sku": {
    "name": "Standard_LRS"
  },
  "kind": "Storage"
}
```

Se o recurso já existir no grupo de recursos e a solicitação não contiver atualizações para as propriedades, nenhuma ação será executada. Se o recurso existir, mas as propriedades tiverem sido alteradas, o recurso existente será atualizado. Se o recurso não existir, o novo recurso será criado. Essa abordagem torna segura para você reimplantar um modelo e saber que os recursos permanecem em um estado consistente.

## <a name="template-design"></a>Design de modelo

Como você define grupos de recursos e modelos é de sua responsabilidade e de como você deseja gerenciar a sua solução. Por exemplo, você pode implantar seu aplicativo de três camadas por meio de um único modelo para um único grupo de recursos.

![modelo de três camadas](./media/template-deployment-overview/3-tier-template.png)

Mas, você não precisa definir toda a sua infraestrutura em um único modelo. Muitas vezes, faz sentido dividir seus requisitos de implantação em um conjunto de modelos com destinação e fins específicos. Você pode reutilizar esses modelos facilmente para soluções diferentes. Para implantar uma solução específica, você deve criar um modelo mestre que vincule todos os modelos necessários. A imagem a seguir mostra como implantar uma solução de três camadas com o modelo pai que inclui três modelos aninhados.

![modelo de camadas aninhadas](./media/template-deployment-overview/nested-tiers-template.png)

Ao prever suas camadas com ciclos de vida separados, você pode implantar os três níveis para separar grupos de recursos. Observe que os recursos ainda podem ser vinculados aos recursos em outros grupos de recursos.

![modelo de camada](./media/template-deployment-overview/tier-templates.png)

Para obter informações sobre modelos aninhados, confira [Usando modelos vinculados com o Azure Resource Manager](resource-group-linked-templates.md).

## <a name="next-steps"></a>Próximas etapas

* Para obter informações sobre as propriedades em arquivos de modelo, consulte [entender a estrutura e a sintaxe de modelos de Azure Resource Manager](resource-group-authoring-templates.md).
* Para começar a desenvolver o modelo, consulte [usar Visual Studio Code para criar modelos de Azure Resource Manager](resource-manager-tools-vs-code.md).


