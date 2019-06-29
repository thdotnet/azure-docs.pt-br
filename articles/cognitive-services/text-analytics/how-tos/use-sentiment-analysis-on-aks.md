---
title: Executar os serviços de Kubernetes do Azure
titleSuffix: Text Analytics - Azure Cognitive Services
description: Implantar os contêineres de análise de texto com a imagem de análise de sentimento, aos serviços de Kubernetes do Azure e testá-lo em um navegador da web.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: 4d5e1da01be531550915a38bed17dd8e57be907a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454945"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-kubernetes-services-aks"></a>Implantar um contêiner de análise de sentimento para os serviços de Kubernetes do Azure (AKS)

Saiba como implantar os serviços Cognitivos [análise de texto](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) contêiner com a imagem de análise de sentimento para os serviços de Kubernetes do Azure (AKS). Esse procedimento é um exemplo de criação de um recurso de análise de texto, a criação de uma imagem de análise de sentimento associada e a capacidade de exercitar essa orquestração dos dois em um navegador. Usando contêineres pode mudar a atenção dos desenvolvedores para longe de gerenciar a infraestrutura para em vez disso, com foco no desenvolvimento de aplicativos.

## <a name="prerequisites"></a>Pré-requisitos

Este procedimento requer várias ferramentas que devem ser instaladas e executadas localmente. Não use o Azure Cloud Shell.

* Use uma assinatura do Azure. Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/) antes de começar.
* Editor de texto, por exemplo: [Visual Studio Code](https://code.visualstudio.com/download).
* Instale a [CLI do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* Instalar o [CLI Kubernetes](https://kubernetes.io/docs/tasks/tools/install-kubectl/).
* Um recurso do Azure com o tipo de preço correto. Nem todos os tipos de preços funcionam com esse contêiner:
    * O recurso **Análise de Texto** com F0 ou somente os tipos de preço da camada Standard.
    * O recurso **Serviços Cognitivos** com o tipo de preço S0.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Kubernetes Services (AKS)](../../containers/includes/create-aks-resource.md)]

## <a name="deploy-text-analytics-container-to-an-aks-cluster"></a>Implantar o contêiner de análise de texto em um Cluster do AKS

1. Abra a CLI do Azure e faça logon no Azure

    ```azurecli
    az login
    ```

1. Entrar para o cluster do AKS (substitua os `your-cluster-name` e `your-resource-group` com os valores apropriados)

    ```azurecli
    az aks get-credentials -n your-cluster-name -g -your-resource-group
    ```

    Depois que esse comando for executado, ele relata uma mensagem semelhante à seguinte:

    ```console
    Merged "your-cluster-name" as current context in /home/username/.kube/config
    ```

    > [!WARNING]
    > Se você tiver várias assinaturas disponíveis para você em sua conta do Azure e o `az aks get-credentials` comando retorna um erro, um problema comum é que você esteja usando a assinatura errada. Basta definir o contexto de sua sessão da CLI do Azure para usar a mesma assinatura que você criou os recursos com e tente novamente.
    > ```azurecli
    >  az account set -s subscription-id
    > ```

1. Abra o editor de texto, de preferência, (Este exemplo usa __Visual Studio Code__):

    ```azurecli
    code .
    ```

1. No editor de texto, crie um novo arquivo chamado _sentiment.yaml_ e cole o YAML a seguir nele:

    ```yaml
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: sentiment
    spec:
      template:
        metadata:
          labels:
            app: sentiment-app
        spec:
          containers:
          - name: sentiment
            image: mcr.microsoft.com/azure-cognitive-services/sentiment
            ports:
            - containerPort: 5000
            env:
            - name: EULA
              value: "accept"
            - name: billing
              value: "https://westus2.api.cognitive.microsoft.com/"
            - name: apikey
              value: "16c12e3419f54ba49a3222177cef781d"
     
    --- 
    apiVersion: v1
    kind: Service
    metadata:
      name: sentiment
    spec:
      type: LoadBalancer
      ports:
      - port: 5000
      selector:
        app: sentiment-app
    ```

1. Salve o arquivo e feche o editor de texto.
1. Executar o Kubernetes `apply` com o _sentiment.yaml_ como seu destino:

    ```console
    kuberctl apply -f sentiment.yaml
    ```

    Depois que o comando foi aplicada com êxito a configuração de implantação, uma mensagem semelhante à seguinte é saída:

    ```
    deployment.apps "sentiment" created
    service "sentiment" created
    ```
1. Verifique se que o POD foi implantado:

    ```console
    kubectl get pods
    ```

    Isso produzirá o status de execução do POD:

    ```
    NAME                         READY     STATUS    RESTARTS   AGE
    sentiment-5c9ccdf575-mf6k5   1/1       Running   0          1m
    ```

1. Verifique se o serviço está disponível e obtenha o endereço IP:

    ```console
    kubectl get services
    ```

    Isso produzirá o status de execução de _sentimento_ serviço no POD:

    ```
    NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
    kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          2m
    sentiment    LoadBalancer   10.0.100.64   168.61.156.180   5000:31234/TCP   2m
    ```

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Próximas etapas

* Use mais [Contêineres de Serviços Cognitivos](../../cognitive-services-container-support.md)
* Use o [serviço conectado de análise de texto](../vs-text-connected-service.md)