---
title: Implantar um Prometeu autônomo em um cluster do Azure Red Hat OpenShift | Microsoft Docs
description: Aqui está como criar uma instância do Prometheus em um cluster do Azure Red Hat OpenShift para monitorar as métricas do seu aplicativo.
author: Makdaam
ms.author: b-lejaku
ms.service: container-service
ms.topic: conceptual
ms.date: 06/17/2019
keywords: prometheus, TOA, openshift, métricas, red hat
ms.openlocfilehash: e66658151361edd43f61d398274c88c047928028
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342731"
---
# <a name="deploy-a-standalone-prometheus-in-an-azure-red-hat-openshift-cluster"></a>Implantar um Prometeu autônomo em um cluster do Azure Red Hat OpenShift

Este guia descreve como configurar Prometeu autônomo com a descoberta de serviço em um cluster do Azure Red Hat OpenShift.  NÃO é necessário acesso de "Administrador do cliente" ao cluster.

A configuração de destino é o seguinte:

- um projeto (Prometeu-projeto), que contém o Prometheus e Alertmanager
- dois projetos (aplicativo Projeto1 e Projeto2 do aplicativo), que contêm os aplicativos para monitorar

Você vai preparar alguns arquivos de configuração do Prometheus localmente. Crie uma nova pasta para armazená-los.
Esses arquivos de configuração serão armazenados no cluster como segredos no caso de tokens secretos são adicionados a ele mais tarde.

## <a name="step-1-sign-in-to-the-cluster-using-the-oc-tool"></a>Etapa 1: Entrar para o cluster usando o `oc` ferramenta
Usando um navegador da web, navegue até o console web do seu cluster (https://openshift. *id aleatório*. *região*. azmosa.io).
Entre com suas credenciais do Azure.
Clique em seu nome de usuário no canto superior direito e selecione "Comando de cópia logon". Cole no terminal, você usará.

Você pode verificar se você está conectado ao cluster correto com o `oc whoami -c` comando.

## <a name="step-2-prepare-the-projects"></a>Etapa 2: Preparar os projetos

Crie projetos.
```
oc new-project prometheus-project
oc new-project app-project1
oc new-project app-project2
```


> [!NOTE]
> Você pode usar o `-n` ou `--namespace` parâmetro ou selecione um ativo de projeto com o `oc project` comando

## <a name="step-3-prepare-prometheus-config"></a>Etapa 3: Preparar configuração Prometeu
Crie um arquivo chamado prometheus.yml com o seguinte conteúdo.
```
global:
  scrape_interval: 30s
  evaluation_interval: 5s

scrape_configs:
    - job_name: prom-sd
      scrape_interval: 30s
      scrape_timeout: 10s
      metrics_path: /metrics
      scheme: http
      kubernetes_sd_configs:
      - api_server: null
        role: endpoints
        namespaces:
          names:
          - prometheus-project
          - app-project1
          - app-project2
```
Crie um segredo chamado "prom" com a configuração.
```
oc create secret generic prom --from-file=prometheus.yml -n prometheus-project
```

O arquivo listado acima é um arquivo de configuração básico do Prometheus.
Ele define os intervalos e configura a descoberta automática em três projetos (project Prometeu, project1 do aplicativo, Projeto2 do aplicativo).
Neste exemplo, o descoberto automaticamente pontos de extremidade serão ser obtidos por meio de HTTP sem autenticação.
Para obter mais informações sobre pontos de extremidade de captura, consulte https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config.


## <a name="step-4-prepare-alertmanager-config"></a>Etapa 4: Preparar configuração Alertmanager
Crie um arquivo chamado alertmanager.yml com o seguinte conteúdo.
```
global:
  resolve_timeout: 5m
route:
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 12h
  receiver: default
  routes:
  - match:
      alertname: DeadMansSwitch
    repeat_interval: 5m
    receiver: deadmansswitch
receivers:
- name: default
- name: deadmansswitch
```
Crie um segredo chamado "prom-alertas" com a configuração.
```
oc create secret generic prom-alerts --from-file=alertmanager.yml -n prometheus-project
```

O arquivo listado acima é o arquivo de configuração do Gerenciador de alertas.

> [!NOTE]
> Você pode verificar as duas etapas anteriores com `oc get secret -n prometheus-project`

## <a name="step-5-start-prometheus-and-alertmanager"></a>Etapa 5: Iniciar o Prometheus e Alertmanager
Baixe o [prometheus standalone.yaml](
https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml) modelo da [repositório de origem do openshift](https://github.com/openshift/origin/tree/release-3.11/examples/prometheus) e aplicá-lo no projeto do prometheus
```
oc process -f https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml | oc apply -f - -n prometheus-project
```
O arquivo de standalone.yaml prometheus é um modelo do OpenShift, que criará uma instância do Prometheus com um proxy de oauth na frente ele e uma instância de Alertmanager, também são protegidos com o proxy de oauth.  Neste modelo oauth-proxy está configurado para permitir que qualquer usuário que pode "get" namespace "Prometeu project" (consulte a `-openshift-sar` sinalizador).

> [!NOTE]
> Você pode verificar se o "prom" StatefulSet tem igual *DESIRED* e *atual* número de réplicas com o `oc get statefulset -n prometheus-project` comando.
> Você também pode verificar todos os recursos do projeto com `oc get all -n prometheus-project`.

## <a name="step-6-add-permissions-to-allow-service-discovery"></a>Etapa 6: Adicionar permissões para permitir a descoberta de serviço
Crie o prometheus sdrole.yml com o seguinte conteúdo.
```
apiVersion: template.openshift.io/v1
kind: Template
metadata:
  name: prometheus-sdrole
  annotations:
    "openshift.io/display-name": Prometheus Service Discovery Role
    description: |
      A role and rolebinding adding permissions required to perform Service Discovery in a given project.
    iconClass: fa fa-cogs
    tags: "monitoring,prometheus,alertmanager,time-series"
parameters:
- description: The project name, where a standalone Prometheus is deployed
  name: PROMETHEUS_PROJECT
  value: prometheus-project
objects:
- apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    name: prometheus-sd
  rules:
  - apiGroups:
    - ""
    resources:
    - services
    - endpoints
    - pods
    verbs:
    - list
    - get
    - watch
- apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    name: prometheus-sd
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: Role
    name: prometheus-sd
  subjects:
  - kind: ServiceAccount
    name: prom
    namespace: ${PROMETHEUS_PROJECT}
```
Aplique o modelo a todos os projetos em que você gostaria de permitir a descoberta de serviço.
```
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project1
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project2
```
Se você também deseja Prometeu ser capaz de coletar métricas de si mesma, lembre-se de aplicar as permissões no projeto Prometeu também.

> [!NOTE]
> Você pode verificar se a função e RoleBinding foram criadas corretamente com o `oc get role` e `oc get rolebinding` comandos, respectivamente

## <a name="optional-deploy-example-application"></a>Opcional: Implantar o aplicativo de exemplo
Tudo está funcionando, mas não há nenhuma fonte de métricas. Vá para a URL do Prometheus (https://prom-prometheus-project.apps. *id aleatório*. *região*.azmosa.io/), que pode ser encontrada com o comando a seguir.
```
oc get route prom -n prometheus-project
```
Lembre-se de prefixar o nome do host com https://.

O **Status > descoberta de serviço** página mostrará 0 de 0/destinos de Active Directory.

Para implantar um aplicativo de exemplo, que expõe as métricas de python básico no ponto de extremidade /metrics execute os seguintes comandos.
```
oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example1 -n app-project1
oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example2 -n app-project2
```
Os novos aplicativos serão exibido como destinos válidos na página de descoberta de serviço dentro de 30 s após a implantação. Você pode obter mais detalhes sobre o **Status > destinos** página.

> [!NOTE]
> Para cada destino capturado com êxito o Prometheus adiciona um datapoint na métrica "até". Clique em **Prometheus** no canto superior esquerdo de canto e insira "para cima" como a expressão e clique **Execute**.

## <a name="next-steps"></a>Próximas etapas

Você pode adicionar instrumentação personalizada de Prometeu aos seus aplicativos.

A biblioteca de cliente do Prometheus, que simplifica as métricas de Prometeu preparação, está pronta para linguagens de programação diferentes.

 - Java https://github.com/prometheus/client_java
 - Python https://github.com/prometheus/client_python
 - Acesse https://github.com/prometheus/client_golang
 - Ruby https://github.com/prometheus/client_ruby
