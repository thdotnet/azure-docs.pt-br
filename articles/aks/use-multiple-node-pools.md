---
title: Usar vários pools de nó no serviço de Kubernetes do Azure (AKS)
description: Saiba como criar e gerenciar vários pools de nós para um cluster no serviço de Kubernetes do Azure (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/17/2019
ms.author: iainfou
ms.openlocfilehash: 679d91da774b3e4d2c53c70cdc0abfd4da9c6953
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67059640"
---
# <a name="preview---create-and-manage-multiple-node-pools-for-a-cluster-in-azure-kubernetes-service-aks"></a>Visualização – criar e gerenciar vários pools de nós para um cluster no serviço de Kubernetes do Azure (AKS)

No serviço de Kubernetes do Azure (AKS), nós a mesma configuração são agrupadas em *pools de nós*. Esses pools de nó contêm as VMs subjacentes que executam seus aplicativos. O número inicial de nós e seu tamanho (SKU) é definido quando você cria um cluster do AKS, que cria um *pool de nó padrão*. Para dar suporte a aplicativos que têm demandas de armazenamento ou de computação diferentes, você pode criar pools de nós adicionais. Por exemplo, use esses pools de nó adicional para fornecer aplicativos de computação intensa GPUs ou acesso ao armazenamento SSD de alto desempenho.

Este artigo mostra como criar e gerenciar vários pools de nós em um cluster AKS. Esse recurso está atualmente na visualização.

> [!IMPORTANT]
> Recursos de visualização do AKS são Self-service, inscreva-se no. Eles são fornecidos para reunir opiniões e bugs de nossa comunidade. Na visualização, esses recursos não são destinados ao uso em produção. Recursos em visualização pública se encaixam em suporte "melhor esforço". Assistência de AKS equipes de suporte técnico está disponível durante o horário comercial do Pacífico (PST) apenas timezone. Para obter mais informações, consulte as seguintes artigos de suporte:
>
> * [Políticas de suporte do AKS][aks-support-policies]
> * [Perguntas frequentes sobre o suporte do Azure][aks-faq]

## <a name="before-you-begin"></a>Antes de começar

Você precisa da CLI do Azure versão 2.0.61 ou posterior instalado e configurado. Execute `az --version` para encontrar a versão. Se precisar instalar ou atualizar, consulte [Instalar a CLI do Azure][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Instalar a extensão da CLI aks-preview

Os comandos da CLI para criar e gerenciar vários pools de nós estão disponíveis na *versão prévia do aks* extensão da CLI. Instalar o *versão prévia do aks* extensão de CLI do Azure usando o [Adicionar extensão az] [ az-extension-add] de comando, conforme mostrado no exemplo a seguir:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Se você instalou anteriormente a *versão prévia do aks* atualizações de instalação disponível com qualquer extensão, usando o `az extension update --name aks-preview` comando.

### <a name="register-multiple-node-pool-feature-provider"></a>Registrar vários provedores de recurso de pool de nó

Para criar um cluster do AKS que pode usar vários pools de nó, primeiro habilite dois sinalizadores de recursos em sua assinatura. Clusters com vários nós de pool usam um conjunto de dimensionamento de máquinas virtuais (VMSS) para gerenciar a implantação e configuração de nós do Kubernetes. Registre-se a *MultiAgentpoolPreview* e *VMSSPreview* sinalizadores de recursos usando o [registro de recurso az] [ az-feature-register] comando conforme mostrado no exemplo a seguir:

```azurecli-interactive
az feature register --name MultiAgentpoolPreview --namespace Microsoft.ContainerService
az feature register --name VMSSPreview --namespace Microsoft.ContainerService
```

> [!NOTE]
> Qualquer cluster do AKS criar depois que você registrou com êxito a *MultiAgentpoolPreview* usar essa experiência de cluster de visualização. Para continuar a criar clusters regulares, com suporte completo, não habilite os recursos de visualização em assinaturas de produção. Use um teste separado ou desenvolvimento de assinatura do Azure para testar recursos de visualização.

Demora alguns minutos para o status exibir *Registrado*. Você pode verificar o status de registro usando o comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/MultiAgentpoolPreview')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
```

Quando estiver pronto, atualize o registro do provedor de recursos *Microsoft.ContainerService* usando o comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations"></a>Limitações

As seguintes limitações se aplicam quando você cria e gerenciar clusters AKS que dão suporte a vários pools de nó:

* Vários pools de nós só estão disponíveis para clusters criados depois que você registrou com êxito a *MultiAgentpoolPreview* e *VMSSPreview* recursos para sua assinatura. Você não pode adicionar ou gerenciar pools de nós com um cluster do AKS existente criado antes que esses recursos foram registrados com êxito.
* É possível excluir o pool de nós primeiro.
* O complemento de roteamento de aplicativo HTTP não pode ser usado.
* Não é possível usando um modelo do Resource Manager existente, assim como acontece com a maioria das operações de pools de nós de adicionar/atualizar/excluir. Em vez disso, [usar um modelo do Gerenciador de recursos separado](#manage-node-pools-using-a-resource-manager-template) para fazer alterações em pools de nós em um cluster AKS.
* O dimensionador automático de cluster (atualmente em visualização no AKS) não pode ser usado.

Embora esse recurso está em visualização, as seguintes limitações adicionais se aplicam:

* O cluster do AKS pode ter um máximo de oito pools de nó.
* O cluster do AKS pode ter um máximo de 400 nós entre esses pools de oito nós.

## <a name="create-an-aks-cluster"></a>Criar um cluster AKS

Para começar, crie um cluster do AKS com um pool de nó único. O exemplo a seguir usa o [criar grupo de az] [ az-group-create] comando para criar um grupo de recursos denominado *myResourceGroup* no *eastus* região. Um cluster do AKS denominado *myAKSCluster* , em seguida, é criado usando o [criar az aks] [ az-aks-create] comando. Um *versão – kubernetes* dos *1.12.6* é usado para mostrar como atualizar um pool de nós em uma etapa a seguir. Você pode especificar qualquer [suporte para a versão do Kubernetes][supported-versions].

```azurecli-interactive
# Create a resource group in East US
az group create --name myResourceGroup --location eastus

# Create a basic single-node AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --enable-vmss \
    --node-count 1 \
    --generate-ssh-keys \
    --kubernetes-version 1.12.6
```

São necessários alguns minutos para criar o cluster.

Quando o cluster estiver pronto, use o [az aks get-credentials] [ az-aks-get-credentials] comando para obter as credenciais do cluster para uso com `kubectl`:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="add-a-node-pool"></a>Adicionar um pool de nós

O cluster criado na etapa anterior tem um pool de nó único. Vamos adicionar um segundo nó pool usando o [Adicionar pool de nós do az aks] [ az-aks-nodepool-add] comando. O exemplo a seguir cria um pool de nó chamado *mynodepool* que executa *3* nós:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 3
```

Para ver o status de seus pools de nó, use o [lista de pool de nós do az aks] [ az-aks-nodepool-list] de comando e especifique o nome de cluster e do grupo de recursos:

```azurecli-interactive
az aks nodepool list --resource-group myResourceGroup --cluster-name myAKSCluster -o table
```

A saída de exemplo a seguir mostra que *mynodepool* foi criado com êxito com três nós no pool de nó. Quando o cluster do AKS foi criado na etapa anterior, um padrão *nodepool1* foi criado com uma contagem de nós de *1*.

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  3        110        mynodepool  1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

> [!TIP]
> Se nenhum *OrchestratorVersion* ou *VmSize* é especificado quando você adiciona um pool de nós, os nós são criados com base em padrões para o cluster do AKS. Neste exemplo, que foi a versão do Kubernetes *1.12.6* e o tamanho do nó do *Standard_DS2_v2*.

## <a name="upgrade-a-node-pool"></a>Atualizar um pool de nós

Quando o cluster AKS que foi criado na primeira etapa, uma `--kubernetes-version` dos *1.12.6* foi especificado. Vamos atualizar o *mynodepool* para o Kubernetes *1.12.7*. Use o [atualização de pool de nó do az aks] [ az-aks-nodepool-upgrade] comando para atualizar o pool de nós, conforme mostrado no exemplo a seguir:

```azurecli-interactive
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --kubernetes-version 1.12.7 \
    --no-wait
```

Listar o status de seus pools de nó novamente usando o [lista de pool de nós do az aks] [ az-aks-nodepool-list] comando. O exemplo a seguir mostra que *mynodepool* está no *Upgrading* estado *1.12.7*:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup    VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  ---------------  ---------------
VirtualMachineScaleSets  3        110        mynodepool  1.12.7                 100             Linux     Upgrading            myResourceGroup  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroup  Standard_DS2_v2
```

Demora alguns minutos para atualizar os nós para a versão especificada.

Como prática recomendada, você deve atualizar todos os pools de nós em um cluster AKS para a mesma versão do Kubernetes. A capacidade de atualizar pools de nós individuais permite executar uma atualização sem interrupção e agendar os pods entre pools de nós para manter o tempo de atividade do aplicativo.

## <a name="scale-a-node-pool"></a>Dimensionar um pool de nós

Como seu aplicativo de carga de trabalho exige alteração, talvez seja necessário dimensionar o número de nós em um pool de nós. O número de nós pode ser dimensionado para cima ou para baixo.

<!--If you scale down, nodes are carefully [cordoned and drained][kubernetes-drain] to minimize disruption to running applications.-->

Para dimensionar o número de nós em um pool de nós, use o [escala de pool de nó do az aks] [ az-aks-nodepool-scale] comando. O exemplo a seguir dimensiona o número de nós em *mynodepool* à *5*:

```azurecli-interactive
az aks nodepool scale \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 5 \
    --no-wait
```

Listar o status de seus pools de nó novamente usando o [lista de pool de nós do az aks] [ az-aks-nodepool-list] comando. O exemplo a seguir mostra que *mynodepool* está no *Scaling* estado com uma nova contagem de *5* nós:

```console
$ az aks nodepool list -g myResourceGroupPools --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  5        110        mynodepool  1.12.7                 100             Linux     Scaling              myResourceGroupPools  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

Demora alguns minutos para que a conclusão da operação de escala.

## <a name="delete-a-node-pool"></a>Excluir um pool de nós

Se você não precisa de um pool, você pode excluí-lo e remover os nós VM subjacentes. Para excluir um pool de nós, use o [excluir pool de nós do az aks] [ az-aks-nodepool-delete] de comando e especifique o nome do pool de nó. O exemplo a seguir exclui o *mynoodepool* criado nas etapas anteriores:

> [!CAUTION]
> Não há nenhuma opção de recuperação de perda de dados que podem ocorrer quando você exclui um pool de nós. Se os pods não podem ser agendados em outros pools de nós, esses aplicativos não estão disponíveis. Verifique se que você não exclua um pool de nós quando os aplicativos em uso não tem backups de dados ou a capacidade de executar em outros pools de nós no cluster.

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name mynodepool --no-wait
```

A seguinte saída de exemplo da [lista de pool de nós do az aks] [ az-aks-nodepool-list] comando mostra que *mynodepool* está no *excluindo* estado:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  5        110        mynodepool  1.12.7                 100             Linux     Deleting             myResourceGroupPools  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

Demora alguns minutos para excluir os nós e o pool de nós.

## <a name="specify-a-vm-size-for-a-node-pool"></a>Especifique um tamanho VM para um pool de nós

Nos exemplos anteriores para criar um pool de nós, um tamanho de VM padrão foi usado para os nós criados no cluster. Um cenário mais comum é para que você possa criar pools de nós com diferentes tamanhos VM e recursos. Por exemplo, você pode criar um pool de nós que contém nós com grandes quantidades de memória ou CPU, ou um pool de nós que fornece suporte GPU. Na próxima etapa, você [usar taints e tolerations](#schedule-pods-using-taints-and-tolerations) para informar o Agendador Kubernetes como limitar o acesso a pods que podem ser executados em nós.

No exemplo a seguir, crie um pool de nós com base em GPU que usa o *Standard_NC6* tamanho da VM. Essas VMs são alimentadas por placa NVIDIA Tesla K80. Para obter informações sobre tamanhos de VM disponíveis, consulte [tamanhos para máquinas virtuais Linux no Azure][vm-sizes].

Criar um pool de nó usando o [Adicionar pool de nós do az aks] [ az-aks-nodepool-add] comando novamente. Desta vez, especifique o nome *gpunodepool*e usar o `--node-vm-size` parâmetro para especificar o *Standard_NC6* tamanho:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name gpunodepool \
    --node-count 1 \
    --node-vm-size Standard_NC6 \
    --no-wait
```

A seguinte saída de exemplo da [lista de pool de nós do az aks] [ az-aks-nodepool-list] comando mostra que *gpunodepool* é *criando* nós com o especificado *VmSize*:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name         OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  -----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  1        110        gpunodepool  1.12.6                 100             Linux     Creating             myResourceGroupPools  Standard_NC6
VirtualMachineScaleSets  1        110        nodepool1    1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

Demora alguns minutos para o *gpunodepool* seja criada com êxito.

## <a name="schedule-pods-using-taints-and-tolerations"></a>Agendar pods usando taints e tolerations

Agora você tem dois pools de nós no cluster, o pool de nó padrão criado inicialmente e o pool de nós com base em GPU. Use o [kubectl obter nós] [ kubectl-get] comando para exibir os nós no cluster. A saída de exemplo a seguir mostra um nó em cada pool de nós:

```console
$ kubectl get nodes

NAME                                 STATUS   ROLES   AGE     VERSION
aks-gpunodepool-28993262-vmss000000  Ready    agent   4m22s   v1.12.6
aks-nodepool1-28993262-vmss000000    Ready    agent   115m    v1.12.6
```

O Agendador Kubernetes pode usar taints e tolerations para restringir quais cargas de trabalho podem ser executados em nós.

* Um **taint** é aplicado a um nó que indica que apenas os pods específicos podem ser agendados neles.
* Um **toleration**, em seguida, é aplicado a um pod que lhes permite *tolerar* um taint de nó.

Para obter mais informações sobre como usar avançados recursos de Kubernetes agendadas, consulte [práticas recomendadas para recursos avançados do Agendador no AKS][taints-tolerations]

Neste exemplo, aplicar um prejudicar sua baseadas em GPU de nó usando o [nó de prejudicar kubectl] [ kubectl-taint] comando. Especifique o nome do nó de GPU na saída do anterior `kubectl get nodes` comando. O prejudicar é aplicado como um *chave: valor* e, em seguida, uma opção de agendamento. O exemplo a seguir usa o *sku = gpu* emparelhar e define os pods detêm o *NoSchedule* capacidade:

```console
kubectl taint node aks-gpunodepool-28993262-vmss000000 sku=gpu:NoSchedule
```

O manifesto YAML do exemplo básico a seguir usa um toleration para permitir que o Agendador Kubernetes executar um pod NGINX no nó baseadas em GPU. Para obter um exemplo mais apropriado, mas demorado executar um trabalho de Tensorflow no conjunto de dados MNIST, consulte [GPUs de uso para cargas de trabalho de computação intensa no AKS][gpu-cluster].

Crie um arquivo chamado `gpu-toleration.yaml` e copie no exemplo YAML a seguir:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: nginx:1.15.9
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 1
        memory: 2G
  tolerations:
  - key: "sku"
    operator: "Equal"
    value: "gpu"
    effect: "NoSchedule"
```

Agendar o pod usando o `kubectl apply -f gpu-toleration.yaml` comando:

```console
kubectl apply -f gpu-toleration.yaml
```

Demora alguns segundos para agendar o pod e extraia a imagem NGINX. Use o [kubectl descrevem pod] [ kubectl-describe] comando para exibir o status de pod. Saída de exemplo condensado a seguir mostra a *sku = gpu:NoSchedule* toleration é aplicado. Na seção de eventos, o Agendador tem atribuído o pod para o *aks-gpunodepool 28993262 vmss000000* baseadas em GPU de nó:

```console
$ kubectl describe pod mypod

[...]
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
                 sku=gpu:NoSchedule
Events:
  Type    Reason     Age    From                                          Message
  ----    ------     ----   ----                                          -------
  Normal  Scheduled  4m48s  default-scheduler                             Successfully assigned default/mypod to aks-gpunodepool-28993262-vmss000000
  Normal  Pulling    4m47s  kubelet, aks-gpunodepool-28993262-vmss000000  pulling image "nginx:1.15.9"
  Normal  Pulled     4m43s  kubelet, aks-gpunodepool-28993262-vmss000000  Successfully pulled image "nginx:1.15.9"
  Normal  Created    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Created container
  Normal  Started    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Started container
```

Somente os pods que têm essa prejudicar aplicado podem ser agendados em nós *gpunodepool*. Qualquer outro pod deve ser agendada na *nodepool1* pool de nós. Se você criar pools de nós adicionais, você pode usar taints adicionais e tolerations para limitar quais pods podem ser agendadas sobre os recursos do nó.

## <a name="manage-node-pools-using-a-resource-manager-template"></a>Gerenciar pools de nó usando um modelo do Resource Manager

Quando você usa um modelo do Azure Resource Manager para criar e os recursos gerenciados, você normalmente pode atualizar as configurações no seu modelo e reimplantar para atualizar o recurso. Com os pools de nó no AKS, o perfil de pool inicial do nó não pode ser atualizado quando o cluster do AKS tiver sido criado. Esse comportamento significa que você não pode atualizar um modelo do Resource Manager existente, faça uma alteração para os pools de nós e reimplantar. Em vez disso, você deve criar um modelo do Gerenciador de recursos separado que atualiza apenas os pools de agentes para um cluster AKS existente.

Criar um modelo, como `aks-agentpools.json` e cole o manifesto de exemplo a seguir. Este modelo de exemplo define as seguintes configurações:

* Atualizações a *Linux* pool de agente chamado *myagentpool* executar três nós.
* Define os nós no pool de nós para executar a versão do Kubernetes *1.12.8*.
* Define o tamanho do nó como *Standard_DS2_v2*.

Edite esses valores conforme necessário para atualizar, adicionar ou excluir pools de nós conforme necessário:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "clusterName": {
      "type": "string",
      "metadata": {
        "description": "The name of your existing AKS cluster."
      }
    },
    "location": {
      "type": "string",
      "metadata": {
        "description": "The location of your existing AKS cluster."
      }
    },
    "agentPoolName": {
      "type": "string",
      "defaultValue": "myagentpool",
      "metadata": {
        "description": "The name of the agent pool to create or update."
      }
    },
    "vnetSubnetId": {
      "type": "string",
      "defaultValue": "",
      "metadata": {
        "description": "The Vnet subnet resource ID for your existing AKS cluster."
      }
    }
  },
  "variables": {
    "apiVersion": {
      "aks": "2019-04-01"
    },
    "agentPoolProfiles": {
      "maxPods": 30,
      "osDiskSizeGB": 0,
      "agentCount": 3,
      "agentVmSize": "Standard_DS2_v2",
      "osType": "Linux",
      "vnetSubnetId": "[parameters('vnetSubnetId')]"
    }
  },
  "resources": [
    {
      "apiVersion": "2019-04-01",
      "type": "Microsoft.ContainerService/managedClusters/agentPools",
      "name": "[concat(parameters('clusterName'),'/', parameters('agentPoolName'))]",
      "location": "[parameters('location')]",
      "properties": {
            "maxPods": "[variables('agentPoolProfiles').maxPods]",
            "osDiskSizeGB": "[variables('agentPoolProfiles').osDiskSizeGB]",
            "count": "[variables('agentPoolProfiles').agentCount]",
            "vmSize": "[variables('agentPoolProfiles').agentVmSize]",
            "osType": "[variables('agentPoolProfiles').osType]",
            "storageProfile": "ManagedDisks",
      "type": "VirtualMachineScaleSets",
            "vnetSubnetID": "[variables('agentPoolProfiles').vnetSubnetId]",
            "orchestratorVersion": "1.12.8"
      }
    }
  ]
}
```

Implantar este modelo usando o [criar implantação de grupo az] [ az-group-deployment-create] de comando, conforme mostrado no exemplo a seguir. Você deverá fornecer o nome do cluster AKS e o local existente:

```azurecli-interactive
az group deployment create \
    --resource-group myResourceGroup \
    --template-file aks-agentpools.json
```

Ele pode levar alguns minutos para atualizar seu cluster do AKS, dependendo das configurações de pool de nó e operações definidos no modelo do Resource Manager.

## <a name="clean-up-resources"></a>Limpar recursos

Neste artigo, você criou um cluster do AKS que inclui nós baseadas em GPU. Para reduzir custos desnecessários, você talvez queira excluir o *gpunodepool*, ou todo o cluster AKS.

Para excluir o pool de nós com base em GPU, use o [excluir az aks nodepool] [ az-aks-nodepool-delete] comando conforme mostrado no exemplo a seguir:

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name gpunodepool
```

Para excluir o cluster em si, use o [exclusão de grupo az] [ az-group-delete] comando para excluir o grupo de recursos do AKS:

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Próximas etapas

Neste artigo, você aprendeu a criar e gerenciar vários pools de nós em um cluster AKS. Para obter mais informações sobre como controlar os pods entre pools de nós, consulte [práticas recomendadas para recursos avançados do Agendador no AKS][operator-best-practices-advanced-scheduler].

Para criar e usar pools de nós de contêiner do Windows Server, consulte [criar um contêiner do Windows Server no AKS][aks-windows].

<!-- EXTERNAL LINKS -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-taint]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- INTERNAL LINKS -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-nodepool-add]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-add
[az-aks-nodepool-list]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-list
[az-aks-nodepool-upgrade]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-upgrade
[az-aks-nodepool-scale]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-scale
[az-aks-nodepool-delete]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-delete
[vm-sizes]: ../virtual-machines/linux/sizes.md
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[gpu-cluster]: gpu-cluster.md
[az-group-delete]: /cli/azure/group#az-group-delete
[install-azure-cli]: /cli/azure/install-azure-cli
[supported-versions]: supported-kubernetes-versions.md
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-windows]: windows-container-cli.md
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
