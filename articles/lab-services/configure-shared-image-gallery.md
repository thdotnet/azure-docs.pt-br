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
ms.openlocfilehash: fba969b70ae052c928f33888d3c93eb7683ae9f7
ms.sourcegitcommit: ec7b0bf593645c0d1ef401a3350f162e02c7e9b8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455737"
---
# <a name="configure-a-shared-image-gallery-in-azure-devtest-labs"></a>Configurar uma galeria de imagem compartilhada no Azure DevTest Labs
Laboratórios de desenvolvimento/teste agora é compatível com o [Galeria de imagens compartilhadas](/virtual-machines/windows/shared-image-galleries.md) recurso. Ele permite que os usuários do laboratório acessem imagens de um local compartilhado durante a criação de recursos de laboratório. Ele também ajuda você a criar estrutura e organização em torno de suas imagens de VM gerenciadas de modo personalizado. O recurso de galeria de imagens compartilhadas dá suporte a:

- Replicação global gerenciada de imagens
- Controle de versão e agrupamento de imagens para que o gerenciamento seja mais fácil
- Torne suas imagens altamente disponíveis com contas de ZRS (Armazenamento com Redundância de Zona) em regiões com suporte para zonas de disponibilidade. O ZRS oferece melhor resiliência em relação às falhas de zona.
- Compartilhamento entre assinaturas e até mesmo entre locatários usando o RBAC (controle de acesso baseado em função).

Para obter mais informações, consulte [documentação da Galeria de imagens compartilhadas](../virtual-machines/windows/shared-image-galleries.md). 
 
Se você tiver um grande número de imagens gerenciadas que precise manter e gostaria de disponibilizá-los em toda a empresa, poderá usar uma galeria de imagem compartilhada como um repositório que facilite a atualização e o compartilhamento das suas imagens. Como proprietário de um laboratório, é possível anexar uma galeria de imagens compartilhadas existente ao seu laboratório. Depois que essa galeria for anexada, os usuários do laboratório poderão criar computadores com base nessas imagens mais recentes. Um importante benefício desse recurso é que agora o DevTest Labs pode aproveitar o compartilhamento de imagens entre laboratórios, entre assinaturas e entre regiões. 

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

    ![Menu de galerias de imagens compartilhada](./media/configure-shared-image-gallery/shared-image-galleries-menu.png)
1. Anexar uma galeria de imagem compartilhada existente ao seu laboratório, clicando na **Attach** botão e selecionando sua galeria na lista suspensa.

    ![Anexar](./media/configure-shared-image-gallery/attach-options.png)
1. Vá para a Galeria anexada e configurar sua galeria para **habilitar ou desabilitar** compartilhado imagens para a criação da VM.

    ![Habilitar ou desabilitar](./media/configure-shared-image-gallery/enable-disable.png)
1. Os usuários do laboratório, em seguida, podem criar uma máquina virtual usando as imagens habilitadas clicando em **+ adicionar** e Localizando a imagem na **escolha sua base de** página.

    ![Usuários de laboratório](./media/configure-shared-image-gallery/lab-users.png)
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
