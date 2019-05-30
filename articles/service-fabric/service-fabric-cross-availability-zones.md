---
title: Implantar um cluster do Service Fabric do Azure entre zonas de disponibilidade | Microsoft Docs
description: Saiba como criar um cluster do Azure Service Fabric em zonas de disponibilidade.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/25/2019
ms.author: pepogors
ms.openlocfilehash: b664c3d655ab45c89a65a0aea31622f57ddc8d9e
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "65077449"
---
# <a name="deploy-an-azure-service-fabric-cluster-across-availability-zones"></a>Implantar um cluster do Service Fabric do Azure entre zonas de disponibilidade
As zonas de disponibilidade no Azure é uma oferta que protege seus aplicativos e dados contra falhas do datacenter de alta disponibilidade. Uma zona de disponibilidade é um local físico exclusivo equipado com energia, resfriamento e rede dentro de uma região do Azure.

O Service Fabric dá suporte a clusters que se estendem por zonas de disponibilidade implantando os tipos de nós que estão fixados em regiões específicas. Isso garantirá a alta disponibilidade de seus aplicativos. As zonas de disponibilidade do Azure estão disponíveis somente em regiões selecionadas. Para obter mais informações, consulte [visão geral das zonas de disponibilidade do Azure](https://docs.microsoft.com/azure/availability-zones/az-overview).

Modelos de exemplo estão disponíveis: [O Service Fabric cruzar o modelo de zona de disponibilidade](https://github.com/Azure-Samples/service-fabric-cluster-templates)

## <a name="recommended-topology-for-primary-node-type-of-azure-service-fabric-clusters-spanning-across-availability-zones"></a>Recomendado a topologia para o tipo de nó primário de clusters do Service Fabric abrange zonas de disponibilidade
Um cluster do Service Fabric distribuído entre zonas de disponibilidade garante a alta disponibilidade do estado do cluster. Para um cluster do Service Fabric se estendem entre zonas, você deve criar um tipo de nó primário em cada zona de disponibilidade com suporte pela região. Isso distribui nós de semente uniformemente em cada um dos tipos de nó primário.

A topologia recomendada para o tipo de nó primário requer os recursos descritos abaixo:

* Defina o nível de confiabilidade do cluster para Platina.
* Três tipos de nó marcado como primários.
    * Cada tipo de nó deve ser mapeado para seu próprio conjunto de dimensionamento de máquina virtual localizado em regiões diferentes.
    * Cada conjunto de dimensionamento de máquina virtual deve ter pelo menos cinco nós (durabilidade prata).
* Um único recurso de IP público usando o SKU Standard.
* Um recurso único de Balanceador de carga usando o SKU Standard.
* Um NSG referenciado pela sub-rede em que você implante seus conjuntos de dimensionamento de máquina virtual.

>[!NOTE]
> O conjunto de dimensionamento de propriedade de grupo de posicionamento único deve ser definida como true, como o Service Fabric não oferece suporte a um conjunto de dimensionamento de máquina virtual única que abrange zonas.

 ![Arquitetura de zona de disponibilidade do Azure Service Fabric][sf-architecture]

## <a name="networking-requirements"></a>Requisitos de rede
### <a name="public-ip-and-load-balancer-resource"></a>IP público e o recurso de Balanceador de carga
Para habilitar as zonas de propriedade em uma escala de máquina virtual definida de recursos, o balanceador de carga e o recurso IP referenciados por esse conjunto de dimensionamento de máquinas virtuais devem usar um *Standard* SKU. Criando um recurso IP sem a propriedade SKU ou um balanceador de carga, você criará um SKU básico, que não oferece suporte a zonas de disponibilidade. Um balanceador de carga de SKU Standard bloqueará todo o tráfego de fora, por padrão. para permitir o tráfego externo, um NSG deve ser implantado na sub-rede.

```json
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[concat('LB','-', parameters('clusterName')]",
    "location": "[parameters('computeLocation')]",
    "sku": {
        "name": "Standard"
    }
}
```

```json
{
    "apiVersion": "2018-11-01",
    "type": "Microsoft.Network/loadBalancers",
    "name": "[concat('LB','-', parameters('clusterName')]", 
    "location": "[parameters('computeLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Network/networkSecurityGroups/', concat('nsg', parameters('subnet0Name')))]"
    ],
    "properties": {
        "addressSpace": {
            "addressPrefixes": [
                "[parameters('addressPrefix')]"
            ]
        },
        "subnets": [
        {
            "name": "[parameters('subnet0Name')]",
            "properties": {
                "addressPrefix": "[parameters('subnet0Prefix')]",
                "networkSecurityGroup": {
                "id": "[resourceId('Microsoft.Network/networkSecurityGroups', concat('nsg', parameters('subnet0Name')))]"
              }
            }
          }
        ]
    },
    "sku": {
        "name": "Standard"
    }
}
```

>[!NOTE]
> Não é possível fazer uma alteração no local de SKU sobre os recursos de Balanceador de carga e IP público. Se você estiver migrando de recursos existentes que têm uma SKU básica, consulte a seção de migração deste artigo.

### <a name="virtual-machine-scale-set-nat-rules"></a>Regras de NAT do conjunto de dimensionamento de máquina virtual
O balanceador de carga regras NAT de entrada devem corresponder os pools de NAT do conjunto de dimensionamento de máquina virtual. Cada conjunto de dimensionamento de máquina virtual deve ter um pool NAT de entrada exclusivo.

```json
{
"inboundNatPools": [
    {
        "name": "LoadBalancerBEAddressNatPool0",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "50999",
            "frontendPortRangeStart": "50000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool1",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "51999",
            "frontendPortRangeStart": "51000",
            "protocol": "tcp"
        }
    },
    {
        "name": "LoadBalancerBEAddressNatPool2",
        "properties": {
            "backendPort": "3389",
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPortRangeEnd": "52999",
            "frontendPortRangeStart": "52000",
            "protocol": "tcp"
        }
    }
    ]
}
```

### <a name="standard-sku-load-balancer-outbound-rules"></a>Regras de saída de Balanceador de carga de SKU Standard
Padrão de Balanceador de carga e o IP público Standard introduzem novas habilidades e comportamentos diferentes à conectividade de saída quando comparado ao uso básico de SKUs. Se quiser ter conectividade de saída ao trabalhar com SKUs Standard, você precisará defini-la explicitamente com endereços IP públicos Standard ou com o Load Balancer Standard público. Para obter mais informações, consulte [conexões de saída](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#snatexhaust) e [Azure Load Balancer Standard](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

>[!NOTE]
> O modelo padrão faz referência a um NSG que permite que todo o tráfego de saída por padrão. O tráfego de entrada é limitado para as portas que são necessárias para operações de gerenciamento do Service Fabric. As regras do NSG podem ser modificadas para atender às suas necessidades.

### <a name="enabling-zones-on-a-virtual-machine-scale-set"></a>Habilitar as zonas em uma escala de máquina virtual definido
Para habilitar uma zona, em uma escala de máquina virtual definido, você deve incluir os seguintes três valores no recurso de conjunto de dimensionamento de máquina virtual.

* O primeiro valor é o **zonas** propriedade, que especifica o conjunto de dimensionamento de máquina virtual será implantado em qual zona de disponibilidade.
* O segundo valor é a propriedade "singlePlacementGroup", que deve ser definida como true.
* O terceiro valor é a propriedade "faultDomainOverride" em que a extensão de conjunto de dimensionamento de máquina virtual do Service Fabric. O valor dessa propriedade deve incluir a região e zona em que este conjunto de dimensionamento de máquina virtual será colocado. Exemplo: "faultDomainOverride": "eastus/az1" conjunto de dimensionamento de máquinas virtuais de todos os recursos devem ser colocados na mesma região, como clusters do Service Fabric não têm entre o suporte de região.

```json
{
    "apiVersion": "2018-10-01",
    "type": "Microsoft.Compute/virtualMachineScaleSets",
    "name": "[parameters('vmNodeType1Name')]",
    "location": "[parameters('computeLocation')]",
    "zones": ["1"],
    "properties": {
        "singlePlacementGroup": "true",
    },
    "virtualMachineProfile": {
    "extensionProfile": {
    "extensions": [
    {
    "name": "[concat(parameters('vmNodeType1Name'),'_ServiceFabricNode')]",
    "properties": {
        "type": "ServiceFabricNode",
        "autoUpgradeMinorVersion": false,
        "publisher": "Microsoft.Azure.ServiceFabric.Test",
        "settings": {
            "clusterEndpoint": "[reference(parameters('clusterName')).clusterEndpoint]",
            "nodeTypeRef": "[parameters('vmNodeType1Name')]",
            "dataPath": "D:\\\\SvcFab",
            "durabilityLevel": "Silver",
            "certificate": {
                "thumbprint": "[parameters('certificateThumbprint')]",
                "x509StoreName": "[parameters('certificateStoreValue')]"
            },
            "systemLogUploadSettings": {
                "Enabled": true
            },
            "faultDomainOverride": "eastus/az1"
        },
        "typeHandlerVersion": "1.0"
    }
}
```

### <a name="enabling-multiple-primary-node-types-in-the-service-fabric-cluster-resource"></a>Habilitar vários tipos de nó principal no recurso de Cluster do Service Fabric
Para definir um ou mais tipos de nó como principal em um recurso de cluster, defina a propriedade "isPrimary" como "true". Ao implantar um cluster do Service Fabric em zonas de disponibilidade, você deve ter três tipos de nó em zonas diferentes.

```json
{
    "reliabilityLevel": "Platinum",
    "nodeTypes": [
    {
        "name": "[parameters('vmNodeType0Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt0applicationEndPort')]",
            "startPort": "[parameters('nt0applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt0fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt0ephemeralEndPort')]",
            "startPort": "[parameters('nt0ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt0fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt0InstanceCount')]"
    },
    {
        "name": "[parameters('vmNodeType1Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt1applicationEndPort')]",
            "startPort": "[parameters('nt1applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt1fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt1ephemeralEndPort')]",
            "startPort": "[parameters('nt1ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt1fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt1InstanceCount')]"
    },
    {
        "name": "[parameters('vmNodeType2Name')]",
        "applicationPorts": {
            "endPort": "[parameters('nt2applicationEndPort')]",
            "startPort": "[parameters('nt2applicationStartPort')]"
        },
        "clientConnectionEndpointPort": "[parameters('nt2fabricTcpGatewayPort')]",
        "durabilityLevel": "Silver",
        "ephemeralPorts": {
            "endPort": "[parameters('nt2ephemeralEndPort')]",
            "startPort": "[parameters('nt2ephemeralStartPort')]"
        },
        "httpGatewayEndpointPort": "[parameters('nt2fabricHttpGatewayPort')]",
        "isPrimary": true,
        "vmInstanceCount": "[parameters('nt2InstanceCount')]"
    }
    ],
}
```

## <a name="migrate-to-using-availability-zones-from-a-cluster-using-a-basic-sku-load-balancer-and-a-basic-sku-ip"></a>Migrar para o uso de zonas de disponibilidade de um cluster usando um balanceador de carga de SKU básico e um IP de SKU básico
Para migrar um cluster, o que estava usando um balanceador de carga e o IP com um SKU básico, você deve primeiro criar um recurso totalmente novo de IP e o balanceador de carga usando o SKU standard. Não é possível atualizar esses recursos no local.

O novo IP e balanceamento de carga devem ser referenciado nos novos tipos de nó zona de disponibilidade cruzados que você deseja usar. No exemplo acima, três nova escala de máquina virtual, conjunto de recursos foram adicionados em zonas 1, 2 e 3. Esses dimensionamento de máquinas virtuais define a referência de IP e balanceamento de carga recém-criado e são marcadas como tipos de nó primário para o recurso de Cluster do Service Fabric.

Para começar, você precisará adicionar os novos recursos ao seu modelo do Resource Manager existente. Esses recursos incluem:
* Um recurso IP público usando o SKU Standard.
* Um recurso de Balanceador de carga usando o SKU Standard.
* Um NSG referenciado pela sub-rede em que você implante seus conjuntos de dimensionamento de máquina virtual.
* Três tipos de nó marcado como primários.
    * Cada tipo de nó deve ser mapeado para seu próprio conjunto de dimensionamento de máquina virtual localizado em regiões diferentes.
    * Cada conjunto de dimensionamento de máquina virtual deve ter pelo menos cinco nós (durabilidade prata).

Um exemplo desses recursos pode ser encontrado na [modelo de exemplo](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/10-VM-Ubuntu-2-NodeType-Secure).

```powershell
New-AzureRmResourceGroupDeployment `
    -ResourceGroupName $ResourceGroupName `
    -TemplateFile $Template `
    -TemplateParameterFile $Parameters
```

Depois que os recursos tiverem concluído a implantação, você pode começar a desativar os nós no tipo de nó primário do cluster original. Como os nós são desabilitados, os serviços do sistema serão migrados para o novo tipo de nó primário que tinha sido implantado na etapa anterior.

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint $ClusterName `
    -KeepAliveIntervalInSec 10 `
    -X509Credential `
    -ServerCertThumbprint $thumb  `
    -FindType FindByThumbprint `
    -FindValue $thumb `
    -StoreLocation CurrentUser `
    -StoreName My 

Write-Host "Connected to cluster"

$nodeNames = @("_nt0_0", "_nt0_1", "_nt0_2", "_nt0_3", "_nt0_4")

Write-Host "Disabling nodes..."
foreach($name in $nodeNames) {
    Disable-ServiceFabricNode -NodeName $name -Intent RemoveNode -Force
}
```

Depois que os nós estão desabilitados, os serviços do sistema serão executados no tipo de nó primário, que é distribuído entre zonas. Em seguida, você pode remover os nós desabilitado do cluster. Depois que os nós forem removidos, você pode remover o IP original, o balanceador de carga, e os recursos do conjunto de dimensionamento de máquinas virtuais.

```powershell
foreach($name in $nodeNames){
    # Remove the node from the cluster
    Remove-ServiceFabricNodeState -NodeName $name -TimeoutSec 300 -Force
    Write-Host "Removed node state for node $name"
}

$scaleSetName="nt0"
Remove-AzureRmVmss -ResourceGroupName $groupname -VMScaleSetName $scaleSetName -Force

$lbname="LB-cluster-nt0"
$oldPublicIpName="LBIP-cluster-0"
$newPublicIpName="LBIP-cluster-1"

Remove-AzureRmLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force
Remove-AzureRmPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force
```

Em seguida, você deve remover as referências a esses recursos do modelo do Resource Manager que você tivesse implantado.

A etapa final envolve a atualização do nome DNS e o IP público.

```powershell
$oldprimaryPublicIP = Get-AzureRmPublicIpAddress -Name $oldPublicIpName  -ResourceGroupName $groupname
$primaryDNSName = $oldprimaryPublicIP.DnsSettings.DomainNameLabel
$primaryDNSFqdn = $oldprimaryPublicIP.DnsSettings.Fqdn

Remove-AzureRmLoadBalancer -Name $lbname -ResourceGroupName $groupname -Force
Remove-AzureRmPublicIpAddress -Name $oldPublicIpName -ResourceGroupName $groupname -Force

$PublicIP = Get-AzureRmPublicIpAddress -Name $newPublicIpName  -ResourceGroupName $groupname
$PublicIP.DnsSettings.DomainNameLabel = $primaryDNSName
$PublicIP.DnsSettings.Fqdn = $primaryDNSFqdn
Set-AzureRmPublicIpAddress -PublicIpAddress $PublicIP

```

[sf-architecture]: ./media/service-fabric-cross-availability-zones/sf-cross-az-topology.png
