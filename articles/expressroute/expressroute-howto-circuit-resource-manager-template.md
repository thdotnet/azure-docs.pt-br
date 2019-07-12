---
title: 'Crie um circuito de ExpressRoute - modelo do Resource Manager: Azure | Microsoft Docs'
description: Criar, provisionar, excluir e desprovisionar um circuito do ExpressRoute.
services: expressroute;azure-resource-manager
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 07/05/2019
ms.author: cherylmc;ganesr
ms.openlocfilehash: bf56145d0a8cd3b01d0d74fcaf3348c1916cee5a
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659678"
---
# <a name="create-an-expressroute-circuit-by-using-azure-resource-manager-template"></a>Criar um circuito do ExpressRoute usando o modelo do Resource Manager

> [!div class="op_single_selector"]
> * [Portal do Azure](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [CLI do Azure](howto-circuit-cli.md)
> * [Modelo do Azure Resource Manager](expressroute-howto-circuit-resource-manager-template.md)
> * [Vídeo – Portal do Azure](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (clássico)](expressroute-howto-circuit-classic.md)
>

Saiba como criar um circuito do ExpressRoute ao implantar um modelo do Azure Resource Manager usando o PowerShell do Azure. Para obter mais informações sobre como desenvolver modelos do Resource Manager, confira [documentação do Resource Manager](/azure/azure-resource-manager/) e a [referência de modelo](/azure/templates/microsoft.network/expressroutecircuits).

## <a name="before-you-begin"></a>Antes de começar

* Examine os [pré-requisitos](expressroute-prerequisites.md) e os [fluxos de trabalho](expressroute-workflows.md) antes de começar a configuração.
* Verifique se você tem permissões para criar novos recursos de rede. Se você não tiver as permissões corretas, entre em contato com o administrador da conta.
* Você pode [exibir um vídeo](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) antes de começar para entender melhor as etapas.

## <a name="create"></a>Criar e provisionar um circuito do ExpressRoute

[Modelos de início rápido do Azure](https://azure.microsoft.com/resources/templates/) tem uma coleção de BOM de modelo do Resource Manager. Você usar um dos [modelos existentes](https://azure.microsoft.com/resources/templates/101-expressroute-circuit-create/) para criar um circuito do ExpressRoute.

[!code-json[create-azure-expressroute-circuit](~/quickstart-templates/101-expressroute-circuit-create/azuredeploy.json)]

Para ver mais modelos relacionados, selecione [aqui](https://azure.microsoft.com/resources/templates/?term=expressroute).

Para criar um circuito do ExpressRoute Implantando um modelo:

1. Selecione **Experimente** do bloco de código a seguir e, em seguida, siga as instruções para entrar no Azure Cloud shell.

    ```azurepowershell-interactive
    $circuitName = Read-Host -Prompt "Enter a circuit name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${circuitName}rg"
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-expressroute-circuit-create/azuredeploy.json"

    $serviceProviderName = "Equinix"
    $peeringLocation = "Silicon Valley"
    $bandwidthInMbps = 500
    $sku_tier = "Premium"
    $sku_family = "MeteredData"

    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -circuitName $circuitName -serviceProviderName $serviceProviderName -peeringLocation $peeringLocation -bandwidthInMbps $bandwidthInMbps -sku_tier $sku_tier -sku_family $sku_family

    Write-Host "Press [ENTER] to continue ..."
    ```

   * **camada** determina se um complemento padrão ou premium do ExpressRoute está habilitado. Você pode especificar **Standard** para obter o SKU padrão ou **Premium** para o complemento premium.

   * **Local de Emparelhamento** é o local físico em que você está realizando o emparelhamento com a Microsoft.

     > [!IMPORTANT]
     > O Local de Emparelhamento indica o [local físico](expressroute-locations.md) em que você está realizando o emparelhamento com a Microsoft. Isso **não** tem vínculo à propriedade "Local", que se refere à posição geográfica na qual o Provedor de Recursos de Rede do Azure está localizado. Embora eles não estejam relacionados, é uma boa prática escolher um provedor de recursos de rede geograficamente próximo do Local de Emparelhamento do circuito.

    O nome do grupo de recursos é o nome do namespace do barramento de serviço com **rg** acrescentado.

2. Selecione **Copiar** para copiar o script do PowerShell.
3. Clique com botão direito do console do shell e, em seguida, selecione **colar**.

Demora alguns minutos para criar um hub de eventos.

O Azure PowerShell é usado para implantar o modelo neste tutorial. Para obter outros métodos de implantação de modelo, consulte:

* [Usando o portal do Azure](../azure-resource-manager/resource-group-template-deploy-portal.md).
* [Usando a CLI do Azure](../azure-resource-manager/resource-group-template-deploy-cli.md).
* [Usando a API REST](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="delete"></a>Desprovisionamento e exclusão de um circuito do ExpressRoute

Você pode excluir seu circuito do ExpressRoute selecionando o ícone **Excluir** . Observe as seguintes informações:

* Você deve desvincular todas as redes virtuais do circuito do ExpressRoute. Se essa operação falhar, verifique se há redes virtuais vinculadas ao circuito.
* Se o estado de provisionamento do provedor de serviço de circuito de ExpressRoute for **Provisionando** ou **Provisionado**, você deverá trabalhar com seu provedor de serviços para que ele desprovisione o circuito. Continuaremos a reservar recursos e a cobrar de você até que o provedor de serviços complete o desprovisionamento do circuito e nos notifique.
* Se o provedor de serviços tiver desprovisionado o circuito (o estado de provisionamento do provedor de serviços estiver definido como **Não provisionado**), exclua o circuito. Isso interrompe a cobrança pelo circuito.

Você pode excluir o circuito de ExpressRoute, executando o seguinte comando do PowerShell:

```azurepowershell-interactive
$circuitName = Read-Host -Prompt "Enter the same circuit name that you used earlier"
$resourceGroupName = "${circuitName}rg"

Remove-AzExpressRouteCircuit -ResourceGroupName $resourceGroupName -Name $circuitName
```

## <a name="next-steps"></a>Próximas etapas

Depois de criar o circuito, continue para as próximas etapas:

* [Criar e modificar o roteamento do circuito do ExpressRoute](expressroute-howto-routing-portal-resource-manager.md)
* [Vincular a rede virtual ao circuito do ExpressRoute](expressroute-howto-linkvnet-arm.md)
