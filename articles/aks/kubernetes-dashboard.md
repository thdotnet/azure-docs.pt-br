---
title: Gerenciar um cluster do Serviço de Kubernetes do Azure com o painel da Web
description: Saiba como usar o painel da interface do usuário Web interna do Kubernetes para gerenciar um cluster do AKS (Serviço de Kubernetes do Azure)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 10/08/2018
ms.author: mlearned
ms.openlocfilehash: 5aa8268fee7d43ad13ea8710760ba493683f502e
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70126865"
---
# <a name="access-the-kubernetes-web-dashboard-in-azure-kubernetes-service-aks"></a>Acessar o painel da Web do Kubernetes no AKS (Serviço de Kubernetes do Azure)

Kubernetes inclui um painel da web que pode ser usado para operações básicas de gerenciamento. Este painel permite exibir o status de integridade básico e as métricas dos seus aplicativos, criar e implantar serviços e editar aplicativos existentes. Este artigo mostra como acessar o painel do Kubernetes usando a CLI do Azure e, em seguida, orienta você por meio de algumas operações básicas do painel.

Para obter mais informações sobre o painel do kubernetes, consulte [painel de interface do usuário da Web][kubernetes-dashboard]do amKubernetes.

## <a name="before-you-begin"></a>Antes de começar

As etapas detalhadas neste documento pressupõem que você tenha criado um cluster do AKS e estabelecido uma conexão `kubectl` com o cluster. Se você precisar criar um cluster AKS, consulte o guia de [início rápido do AKS][aks-quickstart].

A CLI do Azure versão 2.0.46 ou posterior também precisa estar instalada e configurada. Execute `az --version` para encontrar a versão. Se você precisar instalar ou atualizar, consulte [instalar CLI do Azure][install-azure-cli].

## <a name="start-the-kubernetes-dashboard"></a>Iniciar o painel do Kubernetes

Para iniciar o painel do kubernetes, use o comando [AZ AKs Browse][az-aks-browse] . O exemplo abaixo abre o painel para o cluster nomeado *myAKSCluster* em um grupo de recursos chamado *myResourceGroup*:

```azurecli
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

Este comando cria um proxy entre o sistema de desenvolvimento e a API do Kubernetes, e abre um navegador da Web para o painel do Kubernetes. Se um navegador da Web não abrir no painel do kubernetes, copie e cole o endereço da URL anotado na CLI do Azure `http://127.0.0.1:8001`, normalmente.

![A página de logon do painel da Web do kubernetes](./media/kubernetes-dashboard/dashboard-login.png)

Você tem as seguintes opções para entrar no painel do seu cluster:

* Um [arquivo kubeconfig][kubeconfig-file]. Você pode gerar um arquivo kubeconfig usando [AZ AKs Get-Credentials][az-aks-get-credentials].
* Um token, como um token de [conta de serviço][aks-service-accounts] ou token de usuário. Em [clusters habilitados para AAD][aad-cluster], esse token seria um token do AAD. Você pode usar `kubectl config view` para listar os tokens em seu arquivo kubeconfig. Para obter mais detalhes sobre como criar um token do AAD para uso com um cluster AKS, consulte [integrar o Azure Active Directory com o serviço kubernetes do Azure usando o CLI do Azure][aad-cluster].
* A conta de serviço do painel padrão, que será usada se você clicar em *ignorar*.

> [!WARNING]
> Nunca exponha o painel do kubernetes publicamente, independentemente do método de autenticação usado.
> 
> Ao configurar a autenticação para o painel do kubernetes, é recomendável que você use um token sobre a conta de serviço do painel padrão. Um token permite que cada usuário use suas próprias permissões. O uso da conta de serviço do painel padrão pode permitir que um usuário ignore suas próprias permissões e use a conta de serviço em vez disso.
> 
> Se você optar por usar a conta de serviço do painel padrão e o cluster AKS usar o RBAC, um *ClusterRoleBinding* deverá ser criado para que você possa acessar o painel corretamente. Por padrão, o painel do Kubernetes é implantado com um mínimo de acesso de leitura e exibe os erros de acesso do RBAC. Um administrador de cluster pode optar por conceder acesso adicional para o *painel do kubernetes* conta de serviço, no entanto, isso pode ser um vetor para elevação de privilégios. Você também pode integrar a autenticação do Active Directory para fornecer um nível mais granular de acesso.
>
> Para criar uma associação, use o comando [kubectl Create clusterrolebinding][kubectl-create-clusterrolebinding] , conforme mostrado no exemplo a seguir. **Esta associação de exemplo não aplica nenhum componente de autenticação adicional e pode levar ao uso inseguro.**
>
> ```console
> kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
> ```
> 
> Agora você pode acessar o painel do Kubernetes no seu cluster habilitado pelo RBAC. Para iniciar o painel do kubernetes, use o comando [AZ AKs Browse][az-aks-browse] conforme detalhado na etapa anterior.
>
> Se o cluster não usar o RBAC, não será recomendável criar um *ClusterRoleBinding*.
> 
> Para obter mais informações sobre como usar os diferentes métodos de autenticação, consulte o wiki do kubernetes Dashboard em [controles de acesso][dashboard-authentication].

Depois de escolher um método para entrar, o painel do kubernetes é exibido. Se você optar por usar o *token* ou *ignorar*, o painel do kubernetes usará as permissões do usuário conectado no momento para acessar o cluster.

![A página de visão geral do painel da Web do Kubernetes](./media/kubernetes-dashboard/dashboard-overview.png)

## <a name="create-an-application"></a>Criar um aplicativo

Para ver como o painel do Kubernetes pode reduzir a complexidade das tarefas de gerenciamento, vamos criar um aplicativo. Você pode criar um aplicativo usando o painel do Kubernetes fornecendo texto de entrada e um arquivo YAML ou por meio de um assistente gráfico.

Para criar um aplicativo, conclua as seguintes etapas:

1. Selecione o botão **Criar** na janela superior direita.
1. Para usar o assistente gráfico, escolha **Criar um aplicativo**.
1. Forneça um nome para a implantação, como *nginx*
1. Insira o nome da imagem de contêiner a ser usada, como *nginx:1.15.5*
1. Para expor a porta 80 para o tráfego da Web, crie um serviço de Kubernetes. Em **Serviço**, selecione **Externo** e insira **80** para a porta e para a porta de destino.
1. Quando estiver pronto, selecione **Implantar** para criar o aplicativo.

![Implantar um aplicativo no painel da Web do Kubernetes](./media/kubernetes-dashboard/create-app.png)

Leva um minuto ou dois para que um endereço IP externo seja atribuído ao serviço de Kubernetes. No lado esquerdo, em **Descoberta e Balanceamento de Carga** selecione **Serviços**. O serviço do aplicativo é listado, incluindo os *Pontos de extremidade externos*, conforme é mostrado no exemplo a seguir:

![Exibir uma lista de serviços e pontos de extremidade](./media/kubernetes-dashboard/view-services.png)

Selecione o endereço do ponto de extremidade para abrir uma janela do navegador da Web para a página padrão do NGINX:

![Exibir a página do NGINX padrão do aplicativo implantado](./media/kubernetes-dashboard/default-nginx.png)

## <a name="view-pod-information"></a>Exibir informações de pod

O painel do Kubernetes pode fornecer o monitoramento básico de métricas e informações de solução de problemas, como logs.

Para obter mais informações sobre os pods de aplicativo, selecione **Pods** no menu à esquerda. A lista de pods disponíveis é mostrada. Escolha o pod *nginx* para exibir informações, como o consumo de recursos:

![Exibir informações de pod](./media/kubernetes-dashboard/view-pod-info.png)

## <a name="edit-the-application"></a>Editar o aplicativo

Além de criar e exibir os aplicativos, o painel do Kubernetes pode ser usado para editar e atualizar implantações de aplicativo. Para fornecer redundância adicional ao aplicativo, vamos aumentar o número de réplicas do NGINX.

Para editar uma implantação:

1. Selecione **Implantações** no menu à esquerda e, em seguida, escolha a implantação *nginx*.
1. Selecione **Editar** na barra de navegação superior à direita.
1. Localize o valor `spec.replica`, próximo à linha 20. Para aumentar o número de réplicas do aplicativo, altere esse valor de *1* para *3*.
1. Selecione **Atualizar** quando estiver pronto.

![Edite a implantação para atualizar o número de réplicas](./media/kubernetes-dashboard/edit-deployment.png)

Levará alguns instantes para que os pods sejam criados dentro de um conjunto de réplicas. No menu à esquerda, escolha **Conjuntos de Réplicas** e, em seguida, escolha o conjunto de réplicas *nginx*. A lista de pods agora reflete a contagem de réplica atualizada, conforme é mostrado na saída de exemplo a seguir:

![Exibir informações sobre o conjunto de réplicas](./media/kubernetes-dashboard/view-replica-set.png)

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre o painel do kubernetes, consulte o [painel de interface do usuário da Web][kubernetes-dashboard]do amKubernetes.

<!-- LINKS - external -->
[dashboard-authentication]: https://github.com/kubernetes/dashboard/wiki/Access-control
[kubeconfig-file]: https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/
[kubectl-create-clusterrolebinding]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-clusterrolebinding-em-
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

<!-- LINKS - internal -->
[aad-cluster]: ./azure-ad-integration-cli.md
[aks-quickstart]: ./kubernetes-walkthrough.md
[aks-service-accounts]: ./concepts-identity.md#kubernetes-service-accounts
[az-account-get-access-token]: /cli/azure/account?view=azure-cli-latest#az-account-get-access-token
[az-aks-browse]: /cli/azure/aks#az-aks-browse
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[install-azure-cli]: /cli/azure/install-azure-cli
