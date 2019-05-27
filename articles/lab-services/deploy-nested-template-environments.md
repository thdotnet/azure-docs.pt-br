---
title: Implantar ambientes de modelo aninhados do Gerenciador de recursos no Azure DevTest Labs | Microsoft Docs
description: Saiba como implantar modelos aninhados do Gerenciador de recursos do Azure para fornecer ambientes com o Azure DevTest Labs.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2019
ms.author: spelluru
ms.openlocfilehash: eec0cde4a36449f85998bfb04d16f1d52c68bb19
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65835279"
---
# <a name="deploy-nested-azure-resource-manager-templates-for-testing-environments"></a>Implantar modelos aninhados do Gerenciador de recursos do Azure para ambientes de teste
Uma implantação aninhada permite que você execute outros modelos do Azure Resource Manager de dentro de um modelo do Resource Manager principal. Ele permite decompor sua implantação em um conjunto de modelos direcionados e fins específicos. Ele fornece benefícios em termos de teste, reutilização e legibilidade. O artigo [usando modelos vinculados ao implantar recursos do Azure](../azure-resource-manager/resource-group-linked-templates.md) fornece uma boa visão geral dessa solução com vários exemplos de código. Este artigo fornece um exemplo que é específico para o Azure DevTest Labs. 

## <a name="key-parameters"></a>Parâmetros de chave
Embora você possa criar seu próprio modelo do Resource Manager do zero, é recomendável que você use o [projeto de grupo de recursos do Azure](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) no Visual Studio, que torna mais fácil desenvolver e depurar os modelos. Quando você adiciona um recurso de implantação aninhada a azuredeploy. JSON, o Visual Studio adiciona vários itens para tornar o modelo mais flexível. Esses itens incluem a subpasta com o arquivo de modelo e parâmetros secundário, nomes de variáveis dentro do arquivo de modelo principal e dois parâmetros para o local de armazenamento para os novos arquivos. O **artifactslocation** e **artifactslocationsastoken** são os principais parâmetros que o DevTest Labs usa. 

Se você não estiver familiarizado com o funcionamento do DevTest Labs com ambientes, consulte [criar ambientes de várias VMs e recursos de PaaS com modelos do Azure Resource Manager](devtest-lab-create-environment-from-arm.md). Os modelos são armazenados no repositório vinculado ao laboratório no DevTest Labs. Quando você cria um novo ambiente com esses modelos, os arquivos são movidos para um contêiner de armazenamento do Azure no laboratório. Para ser capaz de identificar e copie os arquivos aninhados, o DevTest Labs identifica os parâmetros artifactslocation e artifactslocationsastoken e copia as subpastas até o contêiner de armazenamento. Em seguida, ele insere automaticamente o local e o token de assinatura de acesso compartilhado (SaS) em parâmetros. 

## <a name="nested-deployment-example"></a>Exemplo de implantação aninhados
Aqui está um exemplo simples de uma implantação aninhada:

```json

"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": {
    "_artifactsLocation": {
        "type": "string"
    },
    "_artifactsLocationSasToken": {
        "type": "securestring"
    }},
"variables": {
    "NestOneTemplateFolder": "nestedtemplates",
    "NestOneTemplateFileName": "NestOne.json",
    "NestOneTemplateParametersFileName": "NestOne.parameters.json"},
    "resources": [
    {
        "name": "NestOne",
        "type": "Microsoft.Resources/deployments",
        "apiVersion": "2016-09-01",
        "dependsOn": [ ],
        "properties": {
            "mode": "Incremental",
            "templateLink": {
                "uri": "[concat(parameters('_artifactsLocation'), '/', variables('NestOneTemplateFolder'), '/', variables('NestOneTemplateFileName'), parameters('_artifactsLocationSasToken'))]",
                "contentVersion": "1.0.0.0"
            },
            "parametersLink": {
                "uri": "[concat(parameters('_artifactsLocation'), '/', variables('NestOneTemplateFolder'), '/', variables('NestOneTemplateParametersFileName'), parameters('_artifactsLocationSasToken'))]",
                "contentVersion": "1.0.0.0"
            }
        }    
    }],
"outputs": {}
```

A pasta no repositório que contém este modelo tem uma subpasta `nestedtemplates` com os arquivos **NestOne.json** e **NestOne.parameters.json**. No **azuredeploy. JSON**, URI para o modelo é criado usando o local dos artefatos, a pasta do modelo aninhado, o nome de arquivo do modelo aninhado. Da mesma forma, o URI para os parâmetros é criado usando a localização de artefatos, a pasta de modelo aninhado e o arquivo de parâmetro para o modelo aninhado. 

Aqui está a imagem da estrutura do projeto no Visual Studio: 

![Estrutura do projeto no Visual Studio](./media/deploy-nested-template-environments/visual-studio-project-structure.png)

Você pode adicionar pastas adicionais na pasta principal, mas não aprofundar mais do que um único nível. 

## <a name="next-steps"></a>Próximas etapas
Consulte os seguintes artigos para obter detalhes sobre os ambientes: 

- [Crie ambientes de várias VMs e recursos de PaaS com modelos do Azure Resource Manager](devtest-lab-create-environment-from-arm.md)
- [Configurar e usar ambientes públicos no Azure DevTest Labs](devtest-lab-configure-use-public-environments.md)
- [Conectar-se a um ambiente para a rede virtual do seu laboratório no Azure DevTest Labs](connect-environment-lab-virtual-network.md)