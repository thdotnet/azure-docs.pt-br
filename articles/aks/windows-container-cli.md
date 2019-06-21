---
title: Visualizar - criar um contêiner do Windows Server em um cluster do serviço de Kubernetes do Azure (AKS)
description: Saiba como criar um cluster Kubernetes rapidamente, implantar um aplicativo em um contêiner do Windows Server no Azure Kubernetes AKS (serviço) usando a CLI do Azure.
services: container-service
author: tylermsft
ms.service: container-service
ms.topic: article
ms.date: 06/17/2019
ms.author: twhitney
ms.openlocfilehash: a9887e923358b5658a365b5cfc88759eca2501e0
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303562"
---
# <a name="preview---create-a-windows-server-container-on-an-azure-kubernetes-service-aks-cluster-using-the-azure-cli"></a>Visualizar - criar um contêiner do Windows Server em um cluster do serviço de Kubernetes do Azure (AKS) usando a CLI do Azure

O AKS (Serviço de Kubernetes do Azure) é um serviço de Kubernetes gerenciado que permite implantar e gerenciar clusters rapidamente. Neste artigo, você deve implantar um cluster AKS usando a CLI do Azure. Você também pode implantar um aplicativo de exemplo do ASP.NET em um contêiner do Windows Server para o cluster.

Esse recurso está atualmente na visualização.

![Imagem de navegação para o aplicativo de exemplo do ASP.NET](media/windows-container/asp-net-sample-app.png)

Este artigo pressupõe uma compreensão básica dos conceitos de Kubernetes. Para obter mais informações, consulte [Kubernetes conceitos principais para o serviço de Kubernetes do Azure (AKS)][kubernetes-concepts].

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Se você optar por instalar e usar a CLI localmente, este artigo exigirá que você está executando a CLI do Azure versão 2.0.61 ou posterior. Execute `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, consulte [Instalar a CLI do Azure][azure-cli-install].

## <a name="before-you-begin"></a>Antes de começar

Você deve adicionar um pool de nós adicionais depois de criar seu cluster, que pode executar contêineres do Windows Server. Adicionando um nó adicional de pool é abordado em uma etapa posterior, mas primeiro você precisa habilitar alguns recursos de visualização.

> [!IMPORTANT]
> Recursos de visualização do AKS são Self-service, inscreva-se no. Eles são fornecidos para reunir opiniões e bugs de nossa comunidade. Na visualização, esses recursos não são destinados ao uso em produção. Recursos em visualização pública se encaixam em suporte "melhor esforço". Assistência de AKS equipes de suporte técnico está disponível durante o horário comercial do Pacífico (PST) apenas timezone. Para obter mais informações, consulte as seguintes artigos de suporte:
>
> * [Políticas de suporte do AKS][aks-support-policies]
> * [Perguntas frequentes sobre o suporte do Azure.][aks-faq]

### <a name="install-aks-preview-cli-extension"></a>Instalar a extensão da CLI aks-preview
    
Os comandos da CLI para criar e gerenciar vários pools de nós estão disponíveis na *versão prévia do aks* extensão da CLI. Instalar o *versão prévia do aks* extensão de CLI do Azure usando o [Adicionar extensão az][az-extension-add] de comando, conforme mostrado no exemplo a seguir:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Se você instalou anteriormente a *versão prévia do aks* atualizações de instalação disponível com qualquer extensão, usando o `az extension update --name aks-preview` comando.

### <a name="register-windows-preview-feature"></a>Registrar o recurso de visualização do Windows

Para criar um cluster do AKS que pode usar vários pools de nós e executar contêineres do Windows Server, primeiro habilite as *WindowsPreview* sinalizadores em sua assinatura de recurso. O *WindowsPreview* recurso também usa o conjunto para gerenciar a implantação e configuração de nós do Kubernetes de dimensionamento de máquina virtual e clusters de vários nós de pool. Registre-se a *WindowsPreview* sinalizador de recurso usando o [registro de recurso az][az-feature-register] comando conforme mostrado no exemplo a seguir:

```azurecli-interactive
az feature register --name WindowsPreview --namespace Microsoft.ContainerService
```

> [!NOTE]
> Qualquer cluster do AKS criar depois que você registrou com êxito a *WindowsPreview* sinalizador de recursos usa essa experiência de cluster de visualização. Para continuar a criar clusters regulares, com suporte completo, não habilite os recursos de visualização em assinaturas de produção. Use um teste separado ou desenvolvimento de assinatura do Azure para testar recursos de visualização.

Demora alguns minutos para concluir o registro. Verificar o status de registro usando o [lista de recursos az][az-feature-list] comando:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/WindowsPreview')].{Name:name,State:properties.state}"
```

Quando o estado de registro é `Registered`, pressione Ctrl-C para parar de monitorar o estado.  Em seguida, atualize o registro do *containerservice* provedor de recursos usando o [registro de provedor az][az-provider-register] comando:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

### <a name="limitations"></a>Limitações

As seguintes limitações se aplicam quando você cria e gerenciar clusters AKS que dão suporte a vários pools de nó:

* Vários pools de nós estão disponíveis para clusters criados depois que você registrou com êxito a *WindowsPreview*. Vários pools de nós também estão disponíveis se você registrar o *MultiAgentpoolPreview* e *VMSSPreview* recursos para sua assinatura. Você não pode adicionar ou gerenciar pools de nós com um cluster do AKS existente criado antes que esses recursos foram registrados com êxito.
* É possível excluir o pool de nós primeiro.

Embora esse recurso está em visualização, as seguintes limitações adicionais se aplicam:

* O cluster do AKS pode ter um máximo de oito pools de nó.
* O cluster do AKS pode ter um máximo de 400 nós entre esses pools de oito nós.
* O nome do pool de nó do Windows Server tem um limite de 6 caracteres.

## <a name="create-a-resource-group"></a>Criar um grupo de recursos

Um grupo de recursos do Azure é um grupo lógico no qual os recursos do Azure são implantados e gerenciados. Ao criar um grupo de recursos, você é solicitado a especificar um local. Essa é a localização na qual os metadados do grupo de recursos são armazenados e na qual os recursos são executados no Azure, caso você não especifique outra região durante a criação de recursos. Crie um grupo de recursos usando o comando [az group create][az-group-create].

O exemplo a seguir cria um grupo de recursos chamado *myResourceGroup* no local *eastus*.

> [!NOTE]
> Este artigo usa a sintaxe de Bash para os comandos neste tutorial.
> Se você estiver usando o Azure Cloud Shell, certifique-se de que a lista suspensa no canto superior esquerdo da janela do Cloud Shell está definida como **Bash**.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

A seguinte saída de exemplo mostra o grupo de recursos criado com êxito:

```json
{
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "myResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null,
  "type": null
}
```

## <a name="create-an-aks-cluster"></a>Criar um cluster AKS

Para executar um cluster do AKS que dá suporte a pools de nós para contêineres do Windows Server, o seu cluster precisa usar uma política de rede que usa [CNI do Azure][azure-cni-about] (advanced) network plugin. For more detailed information to help plan out the required subnet ranges and network considerations, see [configure Azure CNI networking][use-advanced-networking]. Use o [criar az aks][az aks create] comando para criar um cluster do AKS denominado *myAKSCluster*. Esse comando criará os recursos de rede necessária, se não existirem.
  * O cluster é configurado com um nó
  * O *senha de administrador do windows* e *windows--nome de usuário administrador* parâmetros definir as credenciais de administrador para qualquer contêiner do Windows Server criada no cluster.

Forneça seu próprio segura *PASSWORD_WIN* (Lembre-se de que os comandos neste artigo são inseridos em um shell BASH):

```azurecli-interactive
PASSWORD_WIN="P@ssw0rd1234"

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 1 \
    --enable-addons monitoring \
    --kubernetes-version 1.14.0 \
    --generate-ssh-keys \
    --windows-admin-password $PASSWORD_WIN \
    --windows-admin-username azureuser \
    --enable-vmss \
    --network-plugin azure
```

> [!Note]
> Se você receber um erro de validação de senha, tente criar o seu grupo de recursos em outra região.
> Em seguida, tente criar o cluster com o novo grupo de recursos.

Após alguns minutos, o comando será concluído e retornará informações no formato JSON sobre o cluster.

## <a name="add-a-windows-server-node-pool"></a>Adicionar um pool de nós do Windows Server

Por padrão, um cluster do AKS é criado com um pool de nós que pode executar contêineres do Linux. Use `az aks nodepool add` comando para adicionar um pool de nós adicionais que pode executar contêineres do Windows Server.

```azurecli
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --os-type Windows \
    --name npwin \
    --node-count 1 \
    --kubernetes-version 1.14.0
```

O comando acima cria um novo pool de nó nomeado *npwin* e adiciona-o para o *myAKSCluster*. Ao criar um pool de nós para executar contêineres do Windows Server, o valor padrão para *tamanho de vm do nó* é *Standard_D2s_v3*. Se você optar por definir a *tamanho de vm do nó* parâmetro, verifique se a lista de [restrito de tamanhos de VM][restricted-vm-sizes]. É o mínimo recomendado de tamanho *Standard_D2s_v3*. O comando acima também usa a sub-rede padrão na rede virtual padrão criada durante a execução `az aks create`.

## <a name="connect-to-the-cluster"></a>Conectar-se ao cluster

Para gerenciar um cluster Kubernetes, você deve usar [kubectl][kubectl], o cliente de linha de comando do Kubernetes. Se você usar o Azure Cloud Shell, o `kubectl` já estará instalado. Para instalar `kubectl` localmente, use o [az aks install-cli][az-aks-install-cli] comando:

```azurecli
az aks install-cli
```

Para configurar o `kubectl` para se conectar ao cluster do Kubernetes, use o comando [az aks get-credentials][az-aks-get-credentials]. Este comando baixa as credenciais e configura a CLI do Kubernetes para usá-las.

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

Para verificar a conexão ao seu cluster, use o comando [kubectl get][kubectl-get] para retornar uma lista de nós do cluster.

```azurecli-interactive
kubectl get nodes
```

A saída de exemplo a seguir mostra o único nó criado nas etapas anteriores. Verifique se o status do nó é *Pronto*:

```
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.14.0
aksnpwin987654                      Ready    agent   108s   v1.14.0
```

## <a name="run-the-application"></a>Executar o aplicativo

Um arquivo de manifesto do Kubernetes define um estado desejado para o cluster, como as imagens de contêiner a serem executadas. Neste artigo, um manifesto é usado para criar todos os objetos necessários para executar o aplicativo de exemplo do ASP.NET em um contêiner do Windows Server. Esse manifesto inclui um [implantação do Kubernetes][kubernetes-deployment] for the ASP.NET sample application and an external [Kubernetes service][kubernetes-service] para acessar o aplicativo da internet.

O aplicativo de exemplo do ASP.NET é fornecido como parte do [amostras do .NET Framework][dotnet-samples] e é executado em um contêiner do Windows Server. AKS requer que os contêineres do Windows Server seja baseada em imagens de *Windows Server 2019* ou maior. O Kubernetes também deve definir o arquivo de manifesto de um [seletor de nó][node-selector] para informar ao cluster do AKS para executar o pod de exemplo do seu aplicativo ASP.NET em um nó que pode executar contêineres do Windows Server.

Crie um arquivo chamado `sample.yaml` e copie a definição YAML a seguir. Se você usar o Azure Cloud Shell, esse arquivo poderá ser criado usando `vi` ou `nano`, como se você estivesse trabalhando em um sistema físico ou virtual:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample
  labels:
    app: sample
spec:
  replicas: 1
  template:
    metadata:
      name: sample
      labels:
        app: sample
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": windows
      containers:
      - name: sample
        image: mcr.microsoft.com/dotnet/framework/samples:aspnetapp
        resources:
          limits:
            cpu: 1
            memory: 800m
          requests:
            cpu: .1
            memory: 300m
        ports:
          - containerPort: 80
  selector:
    matchLabels:
      app: sample
---
apiVersion: v1
kind: Service
metadata:
  name: sample
spec:
  type: LoadBalancer
  ports:
  - protocol: TCP
    port: 80
  selector:
    app: sample
```

Implantar o aplicativo usando o [kubectl aplicar][kubectl-apply] de comando e especifique o nome do seu manifesto YAML:

```azurecli-interactive
kubectl apply -f sample.yaml
```

A saída de exemplo a seguir mostra a implantação e o serviço foi criado com êxito:

```
deployment.apps/sample created
service/sample created
```

## <a name="test-the-application"></a>Testar o aplicativo

Quando o aplicativo é executado, um serviço de Kubernetes expõe o front-end do aplicativo à Internet. A conclusão desse processo pode levar alguns minutos.

Para monitorar o andamento, use o comando [kubectl get service][kubectl-get] com o argumento `--watch`.

```azurecli-interactive
kubectl get service sample --watch
```

Inicialmente o *EXTERNAL-IP* para o *amostra* serviço é mostrado como *pendente*.

```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
sample             LoadBalancer   10.0.37.27   <pending>     80:30572/TCP   6s
```

Quando o endereço *EXTERNAL-IP* for alterado de *pendente* para um endereço IP público real, use `CTRL-C` para interromper o processo de inspeção do `kubectl`. A seguinte saída de exemplo mostra um endereço IP público válido atribuído ao serviço:

```
sample  LoadBalancer   10.0.37.27   52.179.23.131   80:30572/TCP   2m
```

Para ver o aplicativo de exemplo em ação, abra um navegador da web para o endereço IP externo do seu serviço.

![Imagem de navegação para o aplicativo de exemplo do ASP.NET](media/windows-container/asp-net-sample-app.png)

## <a name="delete-cluster"></a>Excluir cluster

Quando o cluster não for necessário, use o [exclusão de grupo az][az-group-delete] de comando para remover o grupo de recursos, o serviço de contêiner e todos os recursos relacionados.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Quando você excluir o cluster, a entidade de serviço do Azure Active Directory usada pelo cluster do AKS não será removida. Para obter etapas sobre como remover a entidade de serviço, consulte [considerações sobre a entidade de segurança e a exclusão de serviço AKS][sp-delete].

## <a name="next-steps"></a>Próximas etapas

Neste artigo, você implantou um cluster Kubernetes e implantou um aplicativo de exemplo do ASP.NET em um contêiner do Windows Server a ele. [Acessar o painel da web Kubernetes][kubernetes-dashboard] para o cluster que você acabou de criar.

Para saber mais sobre o AKS e percorrer um código completo de exemplo de implantação, prossiga para o tutorial de cluster Kubernetes.

> [!div class="nextstepaction"]
> [Tutorial do AKS][aks-tutorial]

<!-- LINKS - external -->
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[node-selector]: https://kubernetes.io/docs/concepts/configuration/assign-pod-node/
[dotnet-samples]: https://hub.docker.com/_/microsoft-dotnet-framework-samples/
[azure-cni]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md

<!-- LINKS - internal -->
[kubernetes-concepts]: concepts-clusters-workloads.md
[aks-monitor]: https://aka.ms/coingfonboarding
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-browse]: /cli/azure/aks?view=azure-cli-latest#az-aks-browse
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-aks-install-cli]: /cli/azure/aks?view=azure-cli-latest#az-aks-install-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[az-provider-register]: /cli/azure/provider#az-provider-register
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cni-about]: concepts-network.md#azure-cni-advanced-networking
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[azure-portal]: https://portal.azure.com
[kubernetes-deployment]: concepts-clusters-workloads.md#deployments-and-yaml-manifests
[kubernetes-service]: concepts-network.md#services
[kubernetes-dashboard]: kubernetes-dashboard.md
[restricted-vm-sizes]: quotas-skus-regions.md#restricted-vm-sizes
[use-advanced-networking]: configure-advanced-networking.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
