---
title: Configurar o Azure Monitor para coleta de dados do agente de contêineres | Microsoft Docs
description: Este artigo descreve como você pode configurar o Azure Monitor para o agente de contêineres controlar o stdout/stderr e coleta de log de variáveis de ambiente.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/22/2019
ms.author: magoedte
ms.openlocfilehash: 1e7506e311c38d87371dd1b65440b6fb41a7ce78
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341657"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>Configurar coleta de dados do agente para o Azure Monitor para contêineres

O Azure Monitor para contêineres coleta stdout, stderr e variáveis de ambiente de cargas de trabalho de contêiner implantadas em clusters de Kubernetes hospedados no serviço de Kubernetes do Azure (AKS) do agente em contêineres gerenciados. Você pode configurar as configurações de coleta de dados do agente, criando uma Kubernetes ConfigMaps para controlar essa experiência personalizada. Este artigo demonstra como criar ConfigMap e configurar a coleta de dados de acordo com suas necessidades.

## <a name="configure-your-cluster-with-custom-data-collection-settings"></a>Configurar seu cluster com as configurações de coleta de dados personalizados

Um arquivo de modelo ConfigMap é fornecido que permite que você facilmente editá-lo com suas personalizações sem precisar criá-lo a partir do zero. Antes de começar, você deve examinar a documentação do Kubernetes sobre [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) e familiarizar-se com a forma de criar, configurar e implantar ConfigMaps. Isso permitirá que você filtre stderr e stdout por namespace ou em todo o cluster e variáveis de ambiente para qualquer contêiner em execução em todos os pods/nós no cluster.

>[!IMPORTANT]
>A versão mínima do agente com suporte por esse recurso é microsoft / oms:ciprod06142019 ou posterior. 

### <a name="overview-of-configurable-data-collection-settings"></a>Visão geral das configurações de coleta de dados configuráveis

A seguir estão as configurações que podem ser configuradas para controlar a coleta de dados.

|Chave |Tipo de dados |Value |DESCRIÇÃO |
|----|----------|------|------------|
|`schema-version` |Cadeia de caracteres (diferencia maiusculas de minúsculas) |v1 |Essa é a versão de esquema usada pelo agente ao analisar este ConfigMap. Versão do esquema com suporte no momento é v1. Modificar esse valor não tem suporte e será rejeitada quando ConfigMap é avaliada.|
|`config-version` |Cadeia de caracteres | | Dá suporte à capacidade de manter o controle de versão deste arquivo config no seu sistema/repositório de controle do código-fonte. Número máximo de caracteres permitido é 10, e todos os outros caracteres são truncados. |
|`[log_collection_settings.stdout] enabled =` |Boolean | true ou false | Isso controla se a coleta de logs de contêiner do stdout estiver habilitada. Quando definido como `true` e namespaces não é excluído para coleta de log de stdout (`log_collection_settings.stdout.exclude_namespaces` configuração abaixo), os logs do stdout serão coletados de todos os contêineres em todos os pods/nós no cluster. Se não for especificado em ConfigMaps, o valor padrão é `enabled = true`. |
|`[log_collection_settings.stdout] exclude_namespaces =`|Cadeia de caracteres | Matriz separada por vírgulas |Matriz de namespaces de Kubernetes para que stdout os logs não serão coletados. Essa configuração é eficaz somente se `log_collection_settings.stdout.enabled` é definido como `true`. Se não for especificado em ConfigMap, o valor padrão é `exclude_namespaces = ["kube-system"]`.|
|`[log_collection_settings.stderr] enabled =` |Boolean | true ou false |Isso controla se a coleta de log do contêiner de stderr estiver habilitada. Quando definido como `true` e namespaces não é excluído para coleta de log de stdout (`log_collection_settings.stderr.exclude_namespaces` configuração), logs de stderr serão coletados de todos os contêineres em todos os pods/nós no cluster. Se não for especificado em ConfigMaps, o valor padrão é `enabled = true`. |
|`[log_collection_settings.stderr] exclude_namespaces =` |Cadeia de caracteres |Matriz separada por vírgulas |Matriz de namespaces de Kubernetes para a qual stderr os logs não serão coletados. Essa configuração é eficaz somente se `log_collection_settings.stdout.enabled` é definido como `true`. Se não for especificado em ConfigMap, o valor padrão é `exclude_namespaces = ["kube-system"]`. |
| `[log_collection_settings.env_var] enabled =` |Boolean | true ou false | Isso controla se a coleção de variáveis de ambiente estiver habilitada. Quando definido como `false`, nenhuma variável de ambiente é coletados para qualquer contêiner em execução em todos os pods/nós no cluster. Se não for especificado em ConfigMap, o valor padrão é `enabled = true`. |

### <a name="configure-and-deploy-configmaps"></a>Configurar e implantar ConfigMaps

Execute as seguintes etapas para configurar e implantar o arquivo de configuração ConfigMap para seu cluster.

1. [Baixar](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml) yaml de ConfigMap o modelo de arquivo e salve-o como contêiner-azm-ms-agentconfig.yaml.  
1. Edite o arquivo de yaml ConfigMap com suas personalizações. 

    - Para excluir namespaces específicos para coleta de log do stdout, você configura a chave/valor usando o exemplo a seguir: `[log_collection_settings.stdout] enabled = true exclude_namespaces = ["my-namespace-1", "my-namespace-2"]`.
    - Para desabilitar a coleção de variáveis de ambiente para um contêiner específico, defina a chave/valor `[log_collection_settings.env_var] enabled = true` para habilitar a coleção de variáveis globalmente e, em seguida, siga as etapas [aqui](container-insights-manage-agent.md#how-to-disable-environment-variable-collection-on-a-container) para concluir a configuração para o contêiner específico.
    - Para desabilitar a coleta de logs de stderr todo o cluster, você configura a chave/valor usando o exemplo a seguir: `[log_collection_settings.stderr] enabled = false`.

1. Crie o ConfigMap executando o seguinte comando kubectl: `kubectl apply -f <configmap_yaml_file.yaml>`.
    
    Exemplo: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 
    
    A alteração de configuração pode levar alguns minutos para ser concluída antes de entrarem em vigor, e todos os pods omsagent no cluster serão reiniciado. A reinicialização é uma reinicialização sem interrupção para todos os compartimentos de omsagent, nem todos reiniciar ao mesmo tempo. Quando as reinicializações estiverem concluídas, será exibida uma mensagem que é semelhante à seguinte e inclui o resultado: `configmap "container-azm-ms-agentconfig" created`.

Para verificar a configuração foi aplicada com êxito, use o seguinte comando para examinar os logs de um pod de agente: `kubectl logs omsagent-fdf58 -n=kube-system`. Se houver erros de configuração de pods osmagent, a saída mostrará erros semelhantes ao seguinte:

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

Erros de impedem que o omsagent analisar o arquivo, fazendo com que ele reinicie e usar a configuração padrão. Depois de corrigir os erros no ConfigMap, salve o arquivo yaml e aplicar o ConfigMaps atualizado executando o comando: `kubectl apply -f <configmap_yaml_file.yaml`.

## <a name="applying-updated-configmap"></a>Aplicando atualizado ConfigMap

Se você já tiver implantado um ConfigMap para seu cluster e você quiser atualizá-lo com uma configuração mais recente, é possível simplesmente editar o arquivo ConfigMap você usou anteriormente e, em seguida, aplicar usando o mesmo comando como antes, `kubectl apply -f <configmap_yaml_file.yaml`.

A alteração de configuração pode levar alguns minutos para ser concluída antes de entrarem em vigor, e todos os pods omsagent no cluster serão reiniciado. A reinicialização é uma reinicialização sem interrupção para todos os compartimentos de omsagent, nem todos reiniciar ao mesmo tempo. Quando as reinicializações estiverem concluídas, será exibida uma mensagem que é semelhante à seguinte e inclui o resultado: `configmap "container-azm-ms-agentconfig" updated`.

## <a name="verifying-schema-version"></a>Verificando a versão do esquema

Versões de esquema de configurações com suporte estão disponíveis como a anotação de pod (versões de esquema) no pod omsagent. Você pode vê-los com o comando kubectl a seguir: `kubectl describe pod omsagent-fdf58 -n=kube-system`

A saída mostrará semelhante à seguinte com as versões de esquema de anotação:

```
    Name:           omsagent-fdf58
    Namespace:      kube-system
    Node:           aks-agentpool-95673144-0/10.240.0.4
    Start Time:     Mon, 10 Jun 2019 15:01:03 -0700
    Labels:         controller-revision-hash=589cc7785d
                    dsName=omsagent-ds
                    pod-template-generation=1
    Annotations:    agentVersion=1.10.0.1
                  dockerProviderVersion=5.0.0-0
                    schema-versions=v1 
```

## <a name="next-steps"></a>Próximas etapas

- Para continuar aprendendo a usar o Azure Monitor e monitorar outros aspectos do cluster do AKS, confira [Exibir integridade do Serviço de Kubernetes do Azure](container-insights-analyze.md).
- Modo de exibição [exemplos de consulta de log](container-insights-log-search.md#search-logs-to-analyze-data) para ver consultas predefinidas e exemplos para avaliar ou personalizar para alertas, visualizar ou analisar seus clusters.