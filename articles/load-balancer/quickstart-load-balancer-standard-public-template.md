---
title: 'Início Rápido: Criar um Standard Load Balancer – modelo do Azure Resource Manager'
titlesuffix: Azure Load Balancer
description: Este início rápido mostra como criar um Standard Load Balancer usando o modelo do Azure Resource Manager.
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: I want to create a Standard Load Balancer by using Azure Resource Manager template so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/30/2019
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 0e47560ca43b23f4779da701f3e6f11f53a6b1ce
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66480393"
---
# <a name="quickstart-create-a-standard-load-balancer-to-load-balance-vms-by-using-azure-resource-manager-template"></a>Início Rápido: Criar um Standard Load Balancer para balancear a carga de VMs usando o modelo do Azure Resource Manager

O balanceamento de carga fornece um nível mais alto de disponibilidade e escala distribuindo as solicitações de entrada entre várias máquinas virtuais. Você pode implantar um modelo do Azure Resource Manager para criar um balanceador de carga para balancear a carga de máquinas virtuais (VMs). Este início rápido mostra como carregar a carga de VMs usando um balanceador de carga padrão.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

## <a name="create-a-standard-load-balancer"></a>Criar um balanceador de carga padrão

Nesta seção, você cria um Standard Load Balancer que ajuda a balancear a carga de máquinas virtuais. Um balanceador de carga padrão só oferece suporte a um endereço IP público padrão. Ao criar um balanceador de carga padrão, você também deve criar um novo endereço IP público padrão configurado como o front-end (nomeado como *LoadBalancerFrontend* por padrão) para o balanceador de carga padrão. Há vários métodos que podem ser usados para criar um balanceador de carga padrão. Neste início rápido, você usará o Azure PowerShell para implantar um [modelo do Resource Manager](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/101-load-balancer-standard-create/azuredeploy.json). Os modelos do Resource Manager são arquivos JSON que definem os recursos necessários para implantar sua solução. Para entender os conceitos associados à implantação e ao gerenciamento de soluções do Azure, confira a [documentação do Azure Resource Manager](/azure/azure-resource-manager/). Para ver mais exemplos do Azure Load Balancer relacionados ao modelo, confira [Modelos de início rápido do Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

Para implantar o modelo, selecione **Experimentar** para abrir o Azure Cloud Shell e, em seguida, cole o script do PowerShell a seguir na janela do shell. Para colar o código, clique com o botão direito do mouse na janela do shell e, em seguida, selecione **Colar**.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name with 12 or less letters or numbers that is used to generate Azure resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUserName = Read-Host -Prompt "Enter the virtual machine administrator account name"
$adminPassword = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/101-load-balancer-standard-create/azuredeploy.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName -location $location -adminUsername $adminUsername -adminPassword $adminPassword
```

Observe que o nome do grupo de recursos é o nome do projeto com o acréscimo de **rg**. Você precisará do nome do grupo de recursos na seção a seguir.  São necessários alguns minutos para criar os recursos.

## <a name="test-the-load-balancer"></a>Testar o Load Balancer

1. Entre no [Portal do Azure](https://portal.azure.com).
1. Escolha **Grupos de recursos** no painel esquerdo.
1. Clique no grupo de recursos que você criou na seção anterior.  O nome do grupo de recursos padrão é o nome do projeto com o acréscimo de **rg**.
1. Escolha o balanceador de carga.  Há apenas um balanceador de carga. O nome padrão é o nome do projeto com o acréscimo de **-lb**.
1. Copie o endereço IP público (apenas a parte do endereço IP) e cole-o na barra de endereços do seu navegador. A página padrão do servidor Web do IIS é exibida no navegador.

   ![Servidor Web do IIS](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Para ver o Load Balancer distribuir o tráfego entre todas as três VMs, você poderá forçar a atualização de seu navegador da Web no computador cliente.

## <a name="clean-up-resources"></a>Limpar recursos

Quando não forem mais necessários, exclua o grupo de recursos, o Load Balancer e todos os recursos relacionados. Para isso, escolha o grupo de recursos que contém o Load Balancer do portal do Azure e escolha **Excluir grupo de recursos**.

## <a name="next-steps"></a>Próximas etapas

Neste início rápido, você criou um Standard Load Balancer, anexou VMs a ele, configurou a regra de tráfego do balanceador de carga, a investigação de integridade e testou o balanceador de carga. Para saber mais sobre o Azure Load Balancer, continue nos tutoriais do Azure Load Balancer.

> [!div class="nextstepaction"]
> [Tutoriais do Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
