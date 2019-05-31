---
title: Configure uma galeria de imagem compartilhada no Azure DevTest Labs | Microsoft Docs
description: Saiba como configurar uma galeria de imagens compartilhadas no Azure DevTest Labs
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2019
ms.author: spelluru
ms.openlocfilehash: 51b394043f88789865edea5be6376ae536f88848
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420432"
---
# <a name="configure-a-shared-image-gallery-in-azure-devtest-labs"></a>Configure uma galeria de imagem compartilhada no Azure DevTest Labs
Laboratórios de desenvolvimento/teste agora é compatível com o [Galeria de imagens compartilhadas](/virtual-machines/windows/shared-image-galleries.md) recurso. Ele permite que os usuários do laboratório acessar as imagens de um local compartilhado durante a criação de recursos de laboratório. Ele também ajuda a criar a estrutura e a organização em torno de suas imagens VM gerenciada personalizada. O recurso de galeria de imagens compartilhadas dá suporte a:

- Replicação global gerenciado de imagens
- Controle de versão e o agrupamento de imagens para facilitar o gerenciamento
- Verifique suas imagens altamente disponível com contas de armazenamento com redundância de zona (ZRS) em regiões que dão suporte a zonas de disponibilidade. O ZRS oferece uma melhor resiliência contra falhas de zona.
- Compartilhando entre assinaturas e até mesmo entre locatários, usando o controle de acesso baseado em função (RBAC).

Para obter mais informações, consulte [documentação da Galeria de imagens compartilhadas](../virtual-machines/windows/shared-image-galleries.md). 
 
Se você tiver um grande número de imagens gerenciadas que precise manter e gostaria de disponibilizá-los em toda a empresa, poderá usar uma galeria de imagem compartilhada como um repositório que facilite a atualização e o compartilhamento das suas imagens. Como um proprietário de laboratório, você pode anexar uma galeria de imagem compartilhada existente ao seu laboratório. Depois que essa galeria é anexada, os usuários do laboratório podem criar máquinas dessas imagens mais recentes. Dos principais benefícios desse recurso é que o DevTest Labs agora pode levar a vantagem de compartilhamento de imagens em laboratórios, entre assinaturas e regiões. 

## <a name="considerations"></a>Considerações
- Você pode anexar somente uma galeria de imagens compartilhadas a um laboratório por vez. Se você deseja anexar outro galeria, você precisará existente de desanexar e anexar outro. 
- No momento, DevTest Labs não oferece suporte para carregar imagens na Galeria por meio do laboratório. 
- Ao criar uma máquina virtual usando uma imagem da Galeria de imagem compartilhada, o DevTest Labs usa sempre a versão publicada mais recente dessa imagem.
- Embora o DevTest Labs automaticamente faz uma tentativa de melhor para garantir que a Galeria de imagens compartilhadas replica imagens para a região na qual existe o laboratório, ele nem sempre é possível. Para evitar que os usuários com problemas para criar VMs dessas imagens, verifique se que as imagens já são replicadas para a região do laboratório."

## <a name="use-azure-portal"></a>Usar o portal do Azure
1. Entre no [Portal do Azure](https://portal.azure.com).
1. Selecione **todos os serviços** no menu de navegação à esquerda.
1. Selecione **DevTest Labs** na lista.
1. Na lista de laboratórios, selecione suas **laboratório**.
1. Selecione **configuração e políticas** na **configurações** seção no menu à esquerda.
1. Selecione **galerias de imagens compartilhadas** sob **bases da máquina Virtual** no menu à esquerda.
1. Anexar uma galeria de imagem compartilhada existente ao seu laboratório, clicando na **Attach** botão e selecionando sua galeria na lista suspensa.
1. Vá para a Galeria anexada e configurar sua galeria para **habilitar ou desabilitar** compartilhado imagens para a criação da VM.
1. Os usuários do laboratório, em seguida, podem criar uma máquina virtual usando as imagens habilitadas clicando em **+ adicionar** e Localizando a imagem na **escolha sua base de** página.

## <a name="use-azure-resource-manager-template"></a>Usar o modelo do Azure Resource Manager

### <a name="attach-a-shared-image-gallery-to-your-lab"></a>Anexar uma galeria de imagens compartilhadas para seu laboratório
Se você estiver usando um modelo do Azure Resource Manager para anexar uma galeria de imagens compartilhadas para seu laboratório, você precisará adicioná-lo na seção de recursos do modelo do Resource Manager, conforme mostrado no exemplo a seguir:

```json
"resources": [
{
    "apiVersion": "2018-10-15-preview",
    "type": "Microsoft.DevTestLab/labs",
    "name": "[parameters('newLabName')]",
    "location": "[resourceGroup (). location]",
    "resources": [
    {
        "apiVersion": "2018-10-15-preview",
        "name": "[variables('labVirtualNetworkName')]",
        "type": "virtualNetworks",
        "dependsOn": [
            "[resourceId('Microsoft.DevTestLab/labs', parameters('newLabName'))]"
        ]
    },
    {
        "apiVersion":"2018-10-15-preview",
        "name":"myGallery",
        "type":"sharedGalleries",
        "properties": {
            "galleryId":"[parameters('existingSharedGalleryId')]",
            "allowAllImages": "Enabled"
        },
        "dependsOn":[
            "[resourceId('Microsoft.DevTestLab/labs', parameters('newLabName'))]"
        ]
    }
    ]
} 

```

Para um exemplo de modelo do Resource Manager completo, consulte estes exemplos de modelo do Resource Manager em nosso repositório GitHub público: [Configurar uma galeria de imagem compartilhada durante a criação de um laboratório](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-lab-shared-gallery-configured).

### <a name="create-a-vm-using-an-image-from-the-shared-image-gallery"></a>Criar uma VM usando uma imagem da Galeria de imagens compartilhadas
Se você estiver usando um modelo do Azure Resource Manager para criar uma máquina virtual usando uma imagem da Galeria de imagem compartilhada, use o exemplo a seguir:

```json

"resources": [
{
    "apiVersion": "2018-10-15-preview",
    "type": "Microsoft.DevTestLab/labs/virtualMachines",
    "name": "[variables('resourceName')]",
    "location": "[resourceGroup().location]",
    "properties": {
        "sharedImageId": "[parameters('existingSharedImageId')]",
        "size": "[parameters('newVMSize')]",
        "isAuthenticationWithSshKey": false,
        "userName": "[parameters('userName')]",
        "sshKey": "",
        "password": "[parameters('password')]",
        "labVirtualNetworkId": "[variables('labVirtualNetworkId')]",
        "labSubnetName": "[variables('labSubnetName')]"
    }
}
],

```

Para saber mais, consulte estes exemplos de modelo do Resource Manager no nosso GitHub público.
[Criar uma máquina virtual usando uma imagem da Galeria de imagem compartilhada](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates/101-dtl-create-vm-username-pwd-sharedimage).

## <a name="use-api"></a>Usar a API

- Use a versão 2018-10-15-versão prévia da API.
- Para anexar sua galeria, envie a solicitação, conforme mostrado no trecho a seguir:
    
    ``` 
    PUT [Lab Resource Id]/SharedGalleries/[newGalleryName]
    Body: 
    {
        “properties”:{
            “galleryId”: “[Shared Image Gallery resource Id]”,
            “allowAllImages”:”Enabled”
        }
    }
    ```
- Para exibir todas as imagens na Galeria de imagem compartilhada, você pode listar todas as imagens compartilhadas juntamente com suas IDs de recurso por

    ```
    GET [Lab Resource Id]/SharedGalleries/mySharedGallery/SharedImages
    ````
- Para criar uma VM usando imagens compartilhadas, você pode executar um PUT em máquinas virtuais e nas propriedades de máquina virtual, passe a identificação das imagens compartilhadas que você obteve da chamada anterior. Para as propriedades. SharedImageId


## <a name="next-steps"></a>Próximas etapas
Consulte os seguintes artigos sobre artefatos:

- [Especifique os artefatos obrigatórios para seu laboratório](devtest-lab-mandatory-artifacts.md)
- [Criar artefatos personalizados](devtest-lab-artifact-author.md)
- [Adicionar um repositório de artefatos a um laboratório](devtest-lab-artifact-author.md)
- [Diagnosticar falhas do artefato](devtest-lab-troubleshoot-artifact-failure.md)
