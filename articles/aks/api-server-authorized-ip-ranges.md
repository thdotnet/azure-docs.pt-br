---
title: Servidor de API autorizados de intervalos de IP no serviço de Kubernetes do Azure (AKS)
description: Saiba como para proteger o cluster usando um IP intervalos de endereços para acesso ao servidor de API no serviço de Kubernetes do Azure (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.author: iainfou
ms.openlocfilehash: 1b983c534ab92218759175655bbf396788e4c39d
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956485"
---
# <a name="preview---secure-access-to-the-api-server-using-authorized-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Visualização – acesso seguro para o servidor de API usando autorizado intervalos de endereços IP no serviço de Kubernetes do Azure (AKS)

No Kubernetes, o servidor de API recebe solicitações para executar ações no cluster, como criar recursos ou reduza o número de nós. O servidor de API é a maneira central para interagir com e gerenciar um cluster. Para melhorar a segurança de cluster e minimizar os ataques, o servidor de API só deve ser acessível a partir de um conjunto limitado de intervalos de endereços IP.

Este artigo mostra como usar intervalos de endereços IP de servidor autorizado de API para limitar as solicitações para o plano de controle. Esse recurso está atualmente na visualização.

> [!IMPORTANT]
> Recursos de visualização do AKS são Self-service e aceitação. As visualizações são fornecidas para reunir opiniões e bugs de nossa comunidade. No entanto, eles não são suportados pelo suporte técnico do Azure. Se você cria um cluster ou adicionar esses recursos para clusters existentes, há suporte para esse cluster até que o recurso não está mais em visualização e muda para GA (disponibilidade geral).
>
> Se você encontrar problemas com recursos de visualização [abra um problema no repositório GitHub do AKS] [ aks-github] com o nome do recurso de visualização no título do bug.

## <a name="before-you-begin"></a>Antes de começar

Servidor de API funciona somente para novos clusters AKS que você criar intervalos de IP do autorizados. Este artigo mostra como criar um cluster AKS usando a CLI do Azure.

Você precisa da CLI do Azure versão 2.0.61 ou posterior instalado e configurado. Execute  `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, confira  [Instalar a CLI do Azure][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Instalar a extensão da CLI aks-preview

Os comandos CLI para configurar intervalos de IP do servidor autorizado de API estão disponíveis na *versão prévia do aks* extensão da CLI. Instalar o *versão prévia do aks* extensão de CLI do Azure usando o [Adicionar extensão az] [ az-extension-add] de comando, conforme mostrado no exemplo a seguir:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Se você instalou anteriormente a *versão prévia do aks* atualizações de instalação disponível com qualquer extensão, usando o `az extension update --name aks-preview` comando.

### <a name="register-feature-flag-for-your-subscription"></a>Registre-se o sinalizador de recursos para sua assinatura

Para usar a API de servidor autorizado intervalos de IP, primeiro habilite um sinalizador de recursos em sua assinatura. Para registrar o *APIServerSecurityPreview* sinalizador de recursos, use o [registro de recurso az] [ az-feature-register] comando conforme mostrado no exemplo a seguir:

```azurecli-interactive
az feature register --name APIServerSecurityPreview --namespace Microsoft.ContainerService
```

Demora alguns minutos para o status exibir *Registrado*. Você pode verificar o status de registro usando o comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/APIServerSecurityPreview')].{Name:name,State:properties.state}"
```

Quando estiver pronto, atualize o registro do provedor de recursos *Microsoft.ContainerService* usando o comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations"></a>Limitações

As seguintes limitações se aplicam ao configurar intervalos de IP do servidor autorizado de API:

* No momento, você não pode usar espaços de desenvolvimento do Azure como a comunicação com o servidor de API também é bloqueada.

## <a name="overview-of-api-server-authorized-ip-ranges"></a>Visão geral do servidor de API autorizados de intervalos de IP

O servidor de API do Kubernetes é como as APIs do Kubernetes subjacente são expostos. Esse componente fornece a interação para ferramentas de gerenciamento, tais como `kubectl` ou o painel do Kubernetes. O AKS fornece um mestre do cluster de locatário único, com um servidor dedicado de API. Por padrão, o servidor de API é atribuído um endereço IP público e você deve controlar o acesso usando controles de acesso baseado em função (RBAC).

Para proteger o acesso ao plano de controle caso contrário, publicamente acessível do AKS / servidor de API, você pode habilitar e usar autorizados de intervalos de IP. Esses intervalos IP autorizados permitem somente os intervalos de endereços IP definidos para se comunicar com o servidor de API. Uma solicitação feita ao servidor de API de um endereço IP que não faz parte desses autorizados de intervalos de IP é bloqueada. Você deve continuar a usar o RBAC para, em seguida, autorizar usuários e as ações que solicitam.

Para usar a funcionalidade de intervalo IP autorizada, um endereço IP público é exposto no pool de nó ao implantar um serviço básico do NGINX. O servidor de API se comunica com o pool de nós por meio do endereço IP público autorizado. Você então definir intervalos de endereços IP adicionais que podem acessar o servidor de API.

Para obter mais informações sobre o servidor de API e outros componentes de cluster, consulte [Kubernetes os conceitos principais para AKS][concepts-clusters-workloads].

## <a name="create-an-aks-cluster"></a>Criar um cluster AKS

Os intervalos de IP do servidor de autorização de API só funcionam para novos clusters AKS. Você não pode habilitar autorizados de intervalos de IP como parte do cluster de operação de criação. Se você tentar habilitar autorizados de intervalos de IP como parte do cluster de criar o processo, os nós de cluster não conseguem acessar o servidor de API durante a implantação, como o endereço IP de saída não está definido nesse ponto.

Primeiro, crie um cluster usando o [criar az aks] [ az-aks-create] comando. O exemplo a seguir cria um cluster de nó único chamado *myAKSCluster* no grupo de recursos denominado *myResourceGroup*.

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location eastus

# Create an AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --generate-ssh-keys

# Get credentials to access the cluster
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="create-outbound-gateway-for-firewall-rules"></a>Criar um gateway para regras de firewall de saída

Para garantir que os nós em um cluster de forma confiável podem comunicar com o servidor de API, quando você habilita o autorizados de intervalos de IP na próxima seção, crie um firewall do Azure para uso como o gateway de saída. O endereço IP do firewall do Azure, em seguida, é adicionado à lista de endereços IP de servidor API autorizados na próxima seção.

> [!WARNING]
> O uso do Firewall do Azure pode incorrer em custos significativos de um ciclo de cobrança mensal. O requisito para usar o Firewall do Azure é necessário somente nesse período de visualização inicial. Para obter mais informações e planejamento de custo, consulte [Firewall do Azure de preços][azure-firewall-costs].

Primeiro, obtenha o *MC_* nome do grupo de recursos para o cluster do AKS e a rede virtual. Em seguida, crie uma sub-rede usando o [criar sub-rede de rede virtual de rede az] [ az-network-vnet-subnet-create] comando. O exemplo a seguir cria uma sub-rede chamada *AzureFirewallSubnet* com o intervalo de CIDR *10.200.0.0/16*:

```azurecli-interactive
# Get the name of the MC_ cluster resource group
MC_RESOURCE_GROUP=$(az aks show \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --query nodeResourceGroup -o tsv)

# Get the name of the virtual network used by the cluster
VNET_NAME=$(az network vnet list \
    --resource-group $MC_RESOURCE_GROUP \
    --query [0].name -o tsv)

# Create a subnet in the virtual network for Azure Firewall
az network vnet subnet create \
    --resource-group $MC_RESOURCE_GROUP \
    --vnet-name $VNET_NAME \
    --name AzureFirewallSubnet \
    --address-prefixes 10.200.0.0/16
```

Para criar um Firewall do Azure, instale o *firewall do azure* extensão CLI usando o [Adicionar extensão az] [ az-extension-add] comando. Em seguida, crie um firewall usando o [criar firewall de rede az] [ az-network-firewall-create] comando. O exemplo a seguir cria um firewall do Azure denominado *myAzureFirewall*:

```azurecli-interactive
# Install the CLI extension for Azure Firewall
az extension add --name azure-firewall

# Create an Azure firewall
az network firewall create \
    --resource-group $MC_RESOURCE_GROUP\
    --name myAzureFirewall
```

Um firewall do Azure é atribuído um endereço IP público que flui em tráfego de saída. Criar um endereço público usando o [criar az network public-ip] [ az-network-public-ip-create] de comando e, em seguida, crie uma configuração de IP no firewall usando o [ip-configuração do firewall de rede az criar] [ az-network-firewall-ip-config-create] que aplica-se o IP público:

```azurecli-interactive
# Create a public IP address for the firewall
FIREWALL_PUBLIC_IP=$(az network public-ip create \
    --resource-group $MC_RESOURCE_GROUP \
    --name myAzureFirewallPublicIP \
    --sku Standard \
    --query publicIp.ipAddress -o tsv)

# Associated the firewall with virtual network
az network firewall ip-config create \
    --resource-group $MC_RESOURCE_GROUP \
    --name myAzureFirewallIPConfig \
    --vnet-name $VNET_NAME \
    --firewall-name myAzureFirewall \
    --public-ip-address myAzureFirewallPublicIP
```

Agora, crie a regra de rede do firewall do Azure para *permitem* todas as *TCP* tráfego usando o [criar regra de rede de firewall de rede de az] [ az-network-firewall-network-rule-create] comando. O exemplo a seguir cria uma regra de rede denominada *AllowTCPOutbound* para o tráfego com qualquer endereço de origem ou de destino:

```azurecli-interactive
az network firewall network-rule create \
    --resource-group $MC_RESOURCE_GROUP \
    --firewall-name myAzureFirewall \
    --name AllowTCPOutbound \
    --collection-name myAzureFirewallCollection \
    --priority 200 \
    --action Allow \
    --protocols TCP \
    --source-addresses '*' \
    --destination-addresses '*' \
    --destination-ports '*'
```

Para associar o firewall do Azure com a rota de rede, obtenha as informações da tabela de rota existente, o endereço IP interno do firewall do Azure e, em seguida, o endereço IP do servidor de API. Esses endereços IP são especificados na próxima seção para controlar como o tráfego deve ser roteado para a comunicação de cluster.

```azurecli-interactive
# Get the AKS cluster route table
ROUTE_TABLE=$(az network route-table list \
    --resource-group $MC_RESOURCE_GROUP \
    --query "[?contains(name,'agentpool')].name" -o tsv)

# Get internal IP address of the firewall
FIREWALL_INTERNAL_IP=$(az network firewall show \
    --resource-group $MC_RESOURCE_GROUP \
    --name myAzureFirewall \
    --query ipConfigurations[0].privateIpAddress -o tsv)

# Get the IP address of API server endpoint
K8S_ENDPOINT_IP=$(kubectl get endpoints -o=jsonpath='{.items[?(@.metadata.name == "kubernetes")].subsets[].addresses[].ip}')
```

Por fim, crie uma rota no AKS rede rota tabela existente usando o [criar rota de tabela de rotas de rede az] [ az-network-route-table-route-create] comando que permite o tráfego para usar o dispositivo de firewall do Azure para servidor de API comunicação.

```azurecli-interactive
az network route-table route create \
    --resource-group $MC_RESOURCE_GROUP \
    --route-table-name $ROUTE_TABLE \
    --name AzureFirewallAPIServer \
    --address-prefix $K8S_ENDPOINT_IP/32 \
    --next-hop-ip-address $FIREWALL_INTERNAL_IP \
    --next-hop-type VirtualAppliance

echo "Public IP address for the Azure Firewall instance that should be added to the list of API server authorized addresses is:" $FIREWALL_PUBLIC_IP
```

Anote o endereço IP público do seu dispositivo de Firewall do Azure. Esse endereço é adicionado à lista de intervalos de IP do servidor autorizado API na próxima seção.

## <a name="enable-authorized-ip-ranges"></a>Habilitar autorizados de intervalos de IP

Para habilitar os intervalos de IP do servidor autorizado de API, você deve fornecer uma lista de intervalos de endereços IP autorizados. Quando você especifica um intervalo CIDR, comece com o primeiro endereço IP no intervalo. Por exemplo, *137.117.106.90/29* é um intervalo válido, mas certifique-se de que você especificar o primeiro endereço IP no intervalo, como *137.117.106.88/29*.

Use [atualizar az aks] [ az-aks-update] de comando e especifique a *- api-server-autorizado-intervalos de ip* para permitir. Geralmente, esses intervalos de endereços IP são intervalos de endereços usados por suas redes locais. Adicione o endereço IP público de seu próprio firewall do Azure obtida na etapa anterior, como *20.42.25.196/32*.

O exemplo a seguir permite que os intervalos IP de servidor autorizado de API no cluster denominado *myAKSCluster* no grupo de recursos denominado *myResourceGroup*. Os intervalos de endereços IP para autorizar forem *20.42.25.196/32* (o firewall do Azure endereço IP público), em seguida, *172.0.0.10/16* e *168.10.0.10/18*:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges 20.42.25.196/32,172.0.0.10/16,168.10.0.10/18
```

## <a name="update-or-disable-authorized-ip-ranges"></a>Atualizar ou desabilitar autorizados de intervalos de IP

Para atualizar ou desabilitar autorizados de intervalos de IP, use novamente [atualizar az aks] [ az-aks-update] comando. Especifique o intervalo CIDR atualizado que você deseja permitir ou especificar um intervalo vazio para desabilitar o servidor de API autorizados de intervalos de IP, conforme mostrado no exemplo a seguir:

```azurecli-interactive
az aks update \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --api-server-authorized-ip-ranges ""
```

## <a name="next-steps"></a>Próximas etapas

Neste artigo, você habilitou a intervalos de IP do servidor autorizado de API. Essa abordagem é uma parte de como você pode executar um cluster seguro do AKS.

Para obter mais informações, consulte [conceitos de segurança para aplicativos e clusters no AKS] [ concepts-security] e [práticas recomendadas para segurança de cluster e as atualizações no AKS] [ operator-best-practices-cluster-security].

<!-- LINKS - external -->
[aks-github]: https://github.com/azure/aks/issues]
[azure-firewall-costs]: https://azure.microsoft.com/pricing/details/azure-firewall/

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-update]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-update
[concepts-clusters-workloads]: concepts-clusters-workloads.md
[concepts-security]: concepts-security.md
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
[create-aks-sp]: kubernetes-service-principal.md#manually-create-a-service-principal
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-network-vnet-subnet-create]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-create
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-network-firewall-create]: /cli/azure/ext/azure-firewall/network/firewall#ext-azure-firewall-az-network-firewall-create
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-firewall-ip-config-create]: /cli/azure/ext/azure-firewall/network/firewall/ip-config#ext-azure-firewall-az-network-firewall-ip-config-create
[az-network-firewall-network-rule-create]: /cli/azure/ext/azure-firewall/network/firewall/network-rule#ext-azure-firewall-az-network-firewall-network-rule-create
[az-network-route-table-route-create]: /cli/azure/network/route-table/route#az-network-route-table-route-create
