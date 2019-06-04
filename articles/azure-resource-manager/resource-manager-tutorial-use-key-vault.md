---
title: Integrar o Azure Key Vault à implantação de modelo do Resource Manager | Microsoft Docs
description: Saiba como usar o Azure Key Vault para passar valores de parâmetro seguros durante a implantação de modelo do Resource Manager
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 05/23/2019
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: 0d78e6eaca708073c3a216507b320fe8783a25b6
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239249"
---
# <a name="tutorial-integrate-azure-key-vault-in-resource-manager-template-deployment"></a>Tutorial: Integrar o Azure Key Vault na implantação de modelo do Resource Manager

Saiba como recuperar segredos do Azure Key Vault e passar os segredos como parâmetros durante a implantação do Resource Manager. O valor nunca é exposto porque você apenas fazer referência à sua ID de cofre de chaves. Para obter mais informações, veja [Usar o Azure Key Vault para passar um valor de parâmetro seguro durante a implantação](./resource-manager-keyvault-parameter.md)

No tutorial [Definir a ordem de implantação do recurso](./resource-manager-tutorial-create-templates-with-dependent-resources.md), você cria uma máquina virtual. Você precisa fornecer o nome de usuário de administrador e senha da máquina virtual. Em vez de fornecer a senha, você pode armazenar previamente a senha em um Azure Key Vault e, depois, personalizar o modelo para recuperar a senha do cofre de chaves durante a implantação.

![Diagrama de integração do Key Vault do modelo do Resource Manager](./media/resource-manager-tutorial-use-key-vault/resource-manager-template-key-vault-diagram.png)

Este tutorial cobre as seguintes tarefas:

> [!div class="checklist"]
> * Preparar um cofre de chaves
> * Abrir um modelo de Início Rápido
> * Edite o arquivo de parâmetros
> * Implantar o modelo
> * Validar a implantação
> * Limpar recursos

Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este artigo, você precisa do seguinte:

* [Visual Studio Code](https://code.visualstudio.com/) com a [extensão de Ferramentas do Resource Manager](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Para aumentar a segurança, use uma senha gerada para a conta de administrador da máquina virtual. Veja um exemplo para gerar uma senha:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Verifique se a senha gerada atende aos requisitos de senha de máquina virtual. Cada serviço do Azure tem requisitos de senha específicos. Para os requisitos de senha da VM, veja [Quais são os requisitos de senha ao criar uma VM?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

## <a name="prepare-a-key-vault"></a>Preparar um cofre de chaves

Nesta seção, você criará um cofre de chaves e adicionará um segredo ao cofre de chaves, para que você possa recuperar o segredo ao implantar seu modelo. Há muitas maneiras de criar um cofre de chaves. Neste tutorial, você usará o Azure PowerShell para implantar um [modelo do Resource Manager](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json). Esse modelo executa estas ações:

* Crie um cofre de chaves com a propriedade `enabledForTemplateDeployment` habilitada. Essa propriedade deve ser true antes que o processo de implantação de modelo possa acessar os segredos definidos neste cofre de chaves.
* Adicionar um segredo ao cofre de chaves.  O segredo armazena a senha de administrador da máquina virtual.

> [!NOTE]
> Se você (como o usuário a implantar o modelo de máquina virtual) não for o proprietário ou o colaborador do cofre de chaves, o proprietário ou o colaborador do cofre de chaves deverá conceder o acesso à permissão Microsoft.KeyVault/vaults/deploy/action para o cofre de chaves. Para obter mais informações, veja [Usar o Azure Key Vault para passar um valor de parâmetro seguro durante a implantação](./resource-manager-keyvault-parameter.md)

Para executar o seguinte script do PowerShell, selecione **Experimentar** para abrir o Cloud shell. Para colar o script, clique com o botão direito do mouse no painel do shell e selecione **Colar**.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$upn = Read-Host -Prompt "Enter your user principal name (email address) used to sign in to Azure"
$secretValue = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"
$keyVaultName = $projectName
$adUserId = (Get-AzADUser -UserPrincipalName $upn).Id
$templateUri = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -keyVaultName $keyVaultName -adUserId $adUserId -secretValue $secretValue
```

Algumas informações importantes:

* O nome do grupo de recursos é o nome do projeto com o acréscimo de **rg**. Para facilitar a [limpeza dos recursos criados neste tutorial](#clean-up-resources), use o mesmo grupo de recursos e nome de projeto ao [implantar o próximo modelo](#deploy-the-template).
* O nome padrão do segredo é **vmAdminPassword**. Ele é codificado no modelo.
* Para que o modelo possa recuperar o segredo, você deve habilitar uma política de acesso chamada **Habilitar o acesso ao Azure Resource Manager para implantação de modelo** no cofre de chaves. Essa política está habilitada no modelo. Para saber mais sobre esta política de acesso, confira [Implantar os cofres de chaves e segredos](./resource-manager-keyvault-parameter.md#deploy-key-vaults-and-secrets).

O modelo tem um valor de saída denominado **keyVaultId**. Anote o valor. Quando você implantar a máquina virtual, precisará dessa ID. O formato da ID do Recurso é:

```json
/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
```

Ao copiar e colar a ID, ela pode ser dividida em várias linhas. Você deve mesclar as linhas e cortar os espaços extras.

Para validar a implantação, execute o seguinte comando do PowerShell no mesmo painel de shell para recuperar o segredo em texto não criptografado. O comando só funciona na mesma sessão do shell, pois usa uma variável $keyVaultName definida no script anterior do PowerShell.

```azurepowershell
(Get-AzKeyVaultSecret -vaultName $keyVaultName  -name "vmAdminPassword").SecretValueText
```

Você preparou um cofre de chaves e um segredo. As seções a seguir mostram como personalizar um modelo existente para recuperar o segredo durante a implantação.

## <a name="open-a-quickstart-template"></a>Abrir um modelo de Início Rápido

Modelos de Início Rápido do Azure é um repositório de modelos do Gerenciador de Recursos. Em vez de criar um modelo do zero, você pode encontrar um exemplo de modelo e personalizá-lo. O modelo usado neste tutorial é chamado [Implantar uma VM Windows simples](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

1. No Visual Studio Code, escolha **Arquivo**>**Abrir Arquivo**.
2. Em **Nome do arquivo**, cole a seguinte URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

3. Escolha **Abrir** para abrir o arquivo. Esse é o mesmo cenário usado no [Tutorial: Criar modelos do Azure Resource Manager com recursos dependentes](./resource-manager-tutorial-create-templates-with-dependent-resources.md).
4. Há cinco recursos definidos pelo modelo:

   * `Microsoft.Storage/storageAccounts`. Consulte a [referência de modelo](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * `Microsoft.Network/publicIPAddresses`. Consulte a [referência de modelo](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * `Microsoft.Network/virtualNetworks`. Consulte a [referência de modelo](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * `Microsoft.Network/networkInterfaces`. Consulte a [referência de modelo](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * `Microsoft.Compute/virtualMachines`. Consulte a [referência de modelo](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

     É útil ter algumas noções básicas do modelo antes de personalizá-lo.
5. Selecione **Arquivo**>**Salvar como** para salvar uma cópia do arquivo no computador local com o nome **azuredeploy.json**.
6. Repita as etapas de 1 a 4 para abrir a URL a seguir e, em seguida, salve o arquivo como **azuredeploy.parameters.json**.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>Edite o arquivo de parâmetros

Você não precisa fazer nenhuma alteração ao arquivo de modelo.

1. Abra **azuredeploy.parameters.json** no Visual Studio Code se já não estiver aberto.
2. Atualize o parâmetro **adminPassword** para:

    ```json
    "adminPassword": {
        "reference": {
            "keyVault": {
            "id": "/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>"
            },
            "secretName": "vmAdminPassword"
        }
    },
    ```

    > [!IMPORTANT]
    > Substitua a valor de **id** pela ID do recurso do seu cofre de chaves criado no último procedimento.

    ![integrar o cofre de chaves e o arquivo de parâmetros de implantação de máquina virtual do modelo do Resource Manager](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)
3. Atribua valores a:

    * **adminUsername**: nome da conta de administrador da máquina virtual.
    * **dnsLabelPrefix**: dê um nome a dnsLabelPrefix.

    Veja um exemplo na captura de tela anterior.

4. Salve as alterações.

## <a name="deploy-the-template"></a>Implantar o modelo

Siga as instruções em [Implantar o modelo](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) para implantar o modelo. Você precisa fazer upload de **azuredeploy. JSON** e de **azuredeploy.parameters.json** para o Cloud Shell e, em seguida, usar o seguinte script do PowerShell para implantar o modelo:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$location = Read-Host -Prompt "Enter the same location that is used for creating the key vault (i.e. centralus)"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile "$HOME/azuredeploy.json" `
    -TemplateParameterFile "$HOME/azuredeploy.parameters.json"
```

Quando você implantar o modelo, use o mesmo grupo de recursos que o cofre de chaves. Torna mais fácil quando você limpa os recursos. Você só precisa excluir um grupo de recursos, em vez de dois.

## <a name="validate-the-deployment"></a>Validar a implantação

Depois de ter implantado com êxito a máquina virtual, teste o logon usando a senha armazenada no cofre de chaves.

1. Abra o [Portal do Azure](https://portal.azure.com).
2. Selecione **Grupos de Recursos**/**YourResourceGroupName>** /**simpleWinVM**
3. Selecione **Conectar** da parte superior.
4. Selecione **Baixar arquivo RDP** e, em seguida, siga as instruções para entrar na máquina virtual usando a senha armazenada no cofre de chaves.

## <a name="clean-up-resources"></a>Limpar recursos

Quando os recursos do Azure já não forem necessários, limpe os recursos implantados excluindo o grupo de recursos.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você recuperou um segredo do Azure Key Vault e usou o segredo em sua implantação de modelo.  Para saber como criar modelos vinculados, veja:

> [!div class="nextstepaction"]
> [Criar modelos vinculados](./resource-manager-tutorial-create-linked-templates.md)
