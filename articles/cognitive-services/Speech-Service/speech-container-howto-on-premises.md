---
title: Usar o Kubernetes no local
titleSuffix: Azure Cognitive Services
description: Usando o Kubernetes e o Helm para definir as imagens de contêiner de fala em texto e texto em fala, vamos criar um pacote do Kubernetes. Este pacote será implantado para um Kubernetes cluster local.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 7/10/2019
ms.author: dapine
ms.openlocfilehash: 33d9de956a6d43145fc68f4ec46b09b8e8bf0188
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786250"
---
# <a name="use-kubernetes-on-premises"></a>Usar o Kubernetes no local

Usando o Kubernetes e o Helm para definir as imagens de contêiner de fala em texto e texto em fala, vamos criar um pacote do Kubernetes. Este pacote será implantado para um Kubernetes cluster local. Por fim, vamos explorar como testar os serviços implantados e várias opções de configuração.

## <a name="prerequisites"></a>Pré-requisitos

Você deve cumprir os seguintes pré-requisitos antes de usar contêineres de fala no local:

|Obrigatório|Finalidade|
|--|--|
| Conta do Azure | Se você não tiver uma assinatura do Azure, crie uma [conta gratuita][free-azure-account] antes de começar. |
| Acesso ao registro de contêiner | A fim de Kubernetes para extrair as imagens do docker para o cluster, ele precisará acessar o registro de contêiner. Você precisará [solicitar o acesso ao registro de contêiner do][speech-preview-access] primeiro. |
| CLI de Kubernetes | O [CLI Kubernetes][kubernetes-cli] é necessário para gerenciar as credenciais compartilhadas do registro de contêiner. Kubernetes também é necessário antes de Helm, que é o Gerenciador de pacotes do Kubernetes. |
| CLI do Helm | Como parte do [CLI do Helm][helm-install] install, you'll also need to initialize Helm which will install [Tiller][tiller-install]. |
|Recursos de fala |Para usar esses contêineres, será necessário ter:<br><br>Um _fala_ recursos do Azure para obter a chave de cobrança associada e o URI de ponto de extremidade cobrança. Ambos os valores estão disponíveis no portal Azure **fala** páginas de visão geral e as chaves e são necessárias para iniciar o contêiner.<br><br>**{API_KEY}** : chave de recurso<br><br>**{ENDPOINT_URI}** : exemplo URI do ponto de extremidade é: `https://westus.api.cognitive.microsoft.com/sts/v1.0`|

## <a name="the-recommended-host-computer-configuration"></a>A configuração do computador host recomendado

Consulte a [computador de host do contêiner de serviço de fala][speech-container-host-computer] detalhes como uma referência. Isso *gráfico do helm* calcula automaticamente os requisitos de CPU e memória com base em quantos decodifica (solicitações simultâneas) que especifica o usuário. Além disso, ele será ajustada com base em se as otimizações para entrada de áudio/texto são configuradas como `enabled`. Os padrões de gráfico do helm para duas solicitações simultâneas e desabilitar a otimização.

| Serviço | CPU / contêiner | Memória / contêiner |
|--|--|--|
| **Conversão de fala em texto** | um decodificador requer um mínimo de 1,150 milinúcleos. Se o `optimizedForAudioFile` estiver habilitado, 1,950 milicores são necessários. (padrão: dois decodificadores) | Necessário: 2 GB<br>Limitado:  4 GB |
| **Conversão de Texto em Fala** | uma solicitação simultânea requer um mínimo de 500 milinúcleos. Se o `optimizeForTurboMode` estiver habilitado, 1.000 milicores são necessários. (padrão: duas solicitações simultâneas) | Necessário: 1 GB<br> Limitado: 2 GB |

## <a name="connect-to-the-kubernetes-cluster"></a>Conectar-se ao cluster Kubernetes

O computador host deve ter um cluster do Kubernetes disponível. Consulte este tutorial sobre [Implantando um cluster Kubernetes](../../aks/tutorial-kubernetes-deploy-cluster.md) para obter um entendimento conceitual de como implantar um cluster Kubernetes em um computador host.

### <a name="sharing-docker-credentials-with-the-kubernetes-cluster"></a>Credenciais de compartilhamento do Docker com o cluster Kubernetes

Para permitir que o cluster Kubernetes `docker pull` imagens configuradas do `containerpreview.azurecr.io` registro de contêiner, você precisa transferir as credenciais do docker no cluster. Execute o [ `kubectl create` ][kubectl-create] comando a seguir para criar um *segredo do registro do docker* com base nas credenciais fornecidas do pré-requisito de acesso de registro de contêiner.

Em sua interface de linha de comando de preferência, execute o comando a seguir. Certifique-se de substituir os `<username>`, `<password>`, e `<email-address>` com as credenciais de registro de contêiner.

```console
kubectl create secret docker-registry containerpreview \
    --docker-server=containerpreview.azurecr.io \
    --docker-username=<username> \
    --docker-password=<password> \
    --docker-email=<email-address>
```

> [!NOTE]
> Se você já tiver acesso para o `containerpreview.azurecr.io` registro de contêiner, você poderia criar um segredo do Kubernetes usando o sinalizador genérico em vez disso. Considere o seguinte comando executado em relação a sua configuração do Docker JSON.
> ```console
>  kubectl create secret generic containerpreview \
>      --from-file=.dockerconfigjson=~/.docker/config.json \
>      --type=kubernetes.io/dockerconfigjson
> ```

A seguinte saída é impressa no console quando o segredo foi criado com êxito.

```console
secret "containerpreview" created
```

Para verificar se o segredo foi criado, execute as [ `kubectl get` ][kubectl-get] com o `secrets` sinalizador.

```console
kuberctl get secrets
```

Executando o `kubectl get secrets` imprime todos os segredos configurados.

```console
NAME                  TYPE                                  DATA      AGE
containerpreview      kubernetes.io/dockerconfigjson        1         30s
```

## <a name="configure-helm-chart-values-for-deployment"></a>Configurar valores de gráfico do Helm para implantação

Visite o [Microsoft Helm Hub][ms-helm-hub] para todos os gráficos do helm disponíveis publicamente oferecidos pela Microsoft. No Microsoft Helm Hub, você encontrará os **gráfico de local de fala dos serviços Cognitivos**. O **cognitivas serviços de fala no local** é o gráfico será instalado, mas primeiro deve criar um `config-values.yaml` arquivo por configurações explícitas. Vamos começar adicionando o repositório da Microsoft para nossa instância do Helm.

```console
helm repo add microsoft https://microsoft.github.io/charts/repo
```

Em seguida, configuraremos nosso valores do gráfico Helm. Copie e cole o YAML a seguir em um arquivo chamado `config-values.yaml`. Para obter mais informações sobre como personalizar o **dos serviços Cognitivos local fala gráfico do Helm**, consulte [personalizar gráficos helm](#customize-helm-charts). Substitua os `billing` e `apikey` valores pelos seus próprios.

```yaml
# These settings are deployment specific and users can provide customizations

# speech-to-text configurations
speechToText:
  enabled: true
  numberOfConcurrentRequest: 3
  optimizeForAudioFile: true
  image:
    registry: containerpreview.azurecr.io
    repository: microsoft/cognitive-services-speech-to-text
    tag: latest
    pullSecrets:
      - containerpreview # Or an existing secret
    args:
      eula: accept
      billing: # < Your billing URL >
      apikey: # < Your API Key >

# text-to-speech configurations
textToSpeech:
  enabled: true
  numberOfConcurrentRequest: 3
  optimizeForTurboMode: true
  image:
    registry: containerpreview.azurecr.io
    repository: microsoft/cognitive-services-text-to-speech
    tag: latest
    pullSecrets:
      - containerpreview # Or an existing secret
    args:
      eula: accept
      billing: # < Your billing URL >
      apikey: # < Your API Key >
```

> [!IMPORTANT]
> Se o `billing` e `apikey` valores não forem fornecidos, os serviços expirará após 15 minutos. Da mesma forma, a verificação falhará pois os serviços não estarão disponíveis.

### <a name="the-kubernetes-package-helm-chart"></a>O pacote do Kubernetes (gráfico do Helm)

O *gráfico do Helm* contém a configuração de quais imagens do docker para efetuar pull de `containerpreview.azurecr.io` registro de contêiner.

> Um [gráfico do Helm][helm-charts] é uma coleção de arquivos que descrevem um conjunto relacionado de recursos do Kubernetes. Um único gráfico pode ser usado para implantar algo simples, como um pod memcached, ou algo complexo, como uma pilha de aplicativo web completo com servidores HTTP, bancos de dados, caches e assim por diante.

Fornecido *gráficos do Helm* efetuar pull das imagens do docker do serviço de fala, os serviços de fala em texto e texto em fala o `containerpreview.azurecr.io` registro de contêiner.

## <a name="install-the-helm-chart-on-the-kubernetes-cluster"></a>Instalar o gráfico do Helm no cluster Kubernetes

Para instalar o *gráfico do helm* vamos precisar executar o [ `helm install` ][helm-install-cmd] comando, substituindo o `<config-values.yaml>` com o argumento de nome de arquivo e caminho apropriado. O `microsoft/cognitive-services-speech-onpremise` gráfico do Helm referenciado abaixo está disponível na [Microsoft Helm Hub aqui][ms-helm-hub-speech-chart].

```console
helm install microsoft/cognitive-services-speech-onpremise \
    --version 0.1.0 \
    --values <config-values.yaml> \
    --name onprem-speech
```

Aqui está um exemplo de saída que você pode esperar ver da execução de uma instalação bem-sucedida:

```console
NAME:   onprem-speech
LAST DEPLOYED: Tue Jul  2 12:51:42 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                             READY  STATUS             RESTARTS  AGE
speech-to-text-7664f5f465-87w2d  0/1    Pending            0         0s
speech-to-text-7664f5f465-klbr8  0/1    ContainerCreating  0         0s
text-to-speech-56f8fb685b-4jtzh  0/1    ContainerCreating  0         0s
text-to-speech-56f8fb685b-frwxf  0/1    Pending            0         0s

==> v1/Service
NAME            TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)       AGE
speech-to-text  LoadBalancer  10.0.252.106  <pending>    80:31811/TCP  1s
text-to-speech  LoadBalancer  10.0.125.187  <pending>    80:31247/TCP  0s

==> v1beta1/PodDisruptionBudget
NAME                                MIN AVAILABLE  MAX UNAVAILABLE  ALLOWED DISRUPTIONS  AGE
speech-to-text-poddisruptionbudget  N/A            20%              0                    1s
text-to-speech-poddisruptionbudget  N/A            20%              0                    1s

==> v1beta2/Deployment
NAME            READY  UP-TO-DATE  AVAILABLE  AGE
speech-to-text  0/2    2           0          0s
text-to-speech  0/2    2           0          0s

==> v2beta2/HorizontalPodAutoscaler
NAME                       REFERENCE                  TARGETS        MINPODS  MAXPODS  REPLICAS  AGE
speech-to-text-autoscaler  Deployment/speech-to-text  <unknown>/50%  2        10       0         0s
text-to-speech-autoscaler  Deployment/text-to-speech  <unknown>/50%  2        10       0         0s


NOTES:
cognitive-services-speech-onpremise has been installed!
Release is named onprem-speech
```

A implantação do Kubernetes pode levar vários minutos para concluir. Para confirmar se os pods e serviços disponíveis e implantado corretamente, execute o seguinte comando:

```console
kubectl get all
```

Você deve esperar ver algo semelhante à seguinte saída:

```console
NAME                                  READY     STATUS    RESTARTS   AGE
pod/speech-to-text-7664f5f465-87w2d   1/1       Running   0          34m
pod/speech-to-text-7664f5f465-klbr8   1/1       Running   0          34m
pod/text-to-speech-56f8fb685b-4jtzh   1/1       Running   0          34m
pod/text-to-speech-56f8fb685b-frwxf   1/1       Running   0          34m

NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE
service/kubernetes       ClusterIP      10.0.0.1       <none>           443/TCP        3h
service/speech-to-text   LoadBalancer   10.0.252.106   52.162.123.151   80:31811/TCP   34m
service/text-to-speech   LoadBalancer   10.0.125.187   65.52.233.162    80:31247/TCP   34m

NAME                             DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/speech-to-text   2         2         2            2           34m
deployment.apps/text-to-speech   2         2         2            2           34m

NAME                                        DESIRED   CURRENT   READY     AGE
replicaset.apps/speech-to-text-7664f5f465   2         2         2         34m
replicaset.apps/text-to-speech-56f8fb685b   2         2         2         34m

NAME                                                            REFERENCE                   TARGETS   MINPODS   MAXPODS   REPLICAS   AGE
horizontalpodautoscaler.autoscaling/speech-to-text-autoscaler   Deployment/speech-to-text   1%/50%    2         10        2          34m
horizontalpodautoscaler.autoscaling/text-to-speech-autoscaler   Deployment/text-to-speech   0%/50%    2         10        2          34m
```

### <a name="verify-helm-deployment-with-helm-tests"></a>Verificar a implantação de Helm com testes de Helm

Definem os gráficos do Helm instalados *Helm testes*, que pode servir como uma conveniência para verificação. Esses testes validam a preparação do serviço. Para verificar se os dois **fala em texto** e **texto em fala** serviços, executaremos o [teste Helm][helm-test] comando.

```console
helm test onprem-speech
```

> [!IMPORTANT]
> Esses testes falhará se o status POD não está `Running` ou se a implantação não está listada sob o `AVAILABLE` coluna. Seja paciente, pois isso pode levar mais de dez minutos para concluir.

Esses testes produzirá vários resultados de status:

```console
RUNNING: speech-to-text-readiness-test
PASSED: speech-to-text-readiness-test
RUNNING: text-to-speech-readiness-test
PASSED: text-to-speech-readiness-test
```

Como uma alternativa para executar o *testes do helm*, você pode coletar o *IP externo* endereços e portas correspondentes do `kubectl get all` comando. Usando o IP e porta, abra um navegador da web e navegue até `http://<external-ip>:<port>:/swagger/index.html` para a API do swagger (s) de modo de exibição.

## <a name="customize-helm-charts"></a>Personalizar gráficos Helm

Gráficos do Helm são hierárquicos. Ele também sendo hierárquico permite a herança de gráfico, permite que o conceito de especificidade, onde as configurações que são mais específicas substituem as regras herdadas.

[!INCLUDE [Speech umbrella-helm-chart-config](includes/speech-umbrella-helm-chart-config.md)]

[!INCLUDE [Speech-to-Text Helm Chart Config](includes/speech-to-text-chart-config.md)]

[!INCLUDE [Text-to-Speech Helm Chart Config](includes/text-to-speech-chart-config.md)]

## <a name="next-steps"></a>Próximas etapas

Para obter mais detalhes sobre como instalar aplicativos com Helm no serviço de Kubernetes do Azure (AKS), [visite aqui][installing-helm-apps-in-aks].

> [!div class="nextstepaction"]
> [Contêineres de serviços cognitivos][cog-svcs-containers]

<!-- LINKS - external -->
[free-azure-account]: https://azure.microsoft.com/free
[git-download]: https://git-scm.com/downloads
[azure-cli]: https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest
[docker-engine]: https://www.docker.com/products/docker-engine
[kubernetes-cli]: https://kubernetes.io/docs/tasks/tools/install-kubectl
[helm-install]: https://helm.sh/docs/using_helm/#installing-helm
[helm-install-cmd]: https://helm.sh/docs/helm/#helm-install
[tiller-install]: https://helm.sh/docs/install/#installing-tiller
[helm-charts]: https://helm.sh/docs/developing_charts
[speech-preview-access]: https://aka.ms/speechcontainerspreview
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[helm-test]: https://helm.sh/docs/helm/#helm-test
[ms-helm-hub]: https://hub.helm.sh/charts/microsoft
[ms-helm-hub-speech-chart]: https://hub.helm.sh/charts/microsoft/cognitive-services-speech-onpremise

<!-- LINKS - internal -->
[speech-container-host-computer]: speech-container-howto.md#the-host-computer
[installing-helm-apps-in-aks]: ../../aks/kubernetes-helm.md
[cog-svcs-containers]: ../cognitive-services-container-support.md
