---
title: Exibição do Azure Monitor para logs de contêineres em tempo real | Microsoft Docs
description: Este artigo descreve o modo de exibição em tempo real de eventos e logs de contêiner (stdout/stderr) sem usar o kubectl com o Azure Monitor para contêineres.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2019
ms.author: magoedte
ms.openlocfilehash: 376a7f3f83cc7fcf7490675d9c0aef1513862e8a
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65521726"
---
# <a name="how-to-view-logs-and-events-in-real-time-preview"></a>Como exibir logs e eventos em tempo real (versão prévia)
O Azure Monitor para contêineres inclui um recurso, que está atualmente em versão prévia, que fornece uma exibição em tempo real em seus logs de contêiner do serviço de Kubernetes do Azure (AKS) (stdout/stderr) e eventos sem ter de executar comandos kubectl. Quando você seleciona uma dessas opções, um novo painel é exibida abaixo da tabela de dados de desempenho sobre o **nós**, **controladores**, e **contêineres** exibição. Ele mostra eventos gerados pelo mecanismo de contêiner para ajudá-lo ainda mais na solução de problemas em tempo real e registro em log em tempo real. 

>[!NOTE]
>**Colaborador** acesso para o recurso de cluster é necessário para esse recurso funcione.
>

Os logs em tempo real dão suporte a três métodos diferentes para controlar o acesso aos logs:

1. AKS sem autorização do RBAC do Kubernetes habilitada 
2. AKS habilitado com autorização do RBAC do Kubernetes
3. AKS habilitado com o Azure Active Directory (AD) logon único baseado em SAML no 

## <a name="kubernetes-cluster-without-rbac-enabled"></a>Cluster do Kubernetes sem RBAC habilitado
 
Se você tiver um cluster do Kubernetes que não está configurado com autorização do RBAC do Kubernetes ou integrado ao Azure AD com logon único, não precisará seguir estas etapas. Como a autorização do Kubernetes usa a kube-api, são necessárias permissões somente leitura.

## <a name="kubernetes-rbac-authorization"></a>Autorização do RBAC do Kubernetes
Se você habilitou a autorização do RBAC do Kubernetes, precisará aplicar a associação de função de cluster. As etapas de exemplo a seguir demonstram como configurar a associação de função de cluster com base no modelo de configuração yaml. 

1. Copie e cole o arquivo yaml e salve-o como LogReaderRBAC.yaml.  

    ```
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRole 
    metadata: 
       name: containerHealth-log-reader 
    rules: 
       - apiGroups: [""] 
         resources: ["pods/log", "events"] 
         verbs: ["get", "list"]  
    --- 
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRoleBinding 
    metadata: 
       name: containerHealth-read-logs-global 
    roleRef: 
        kind: ClusterRole 
        name: containerHealth-log-reader 
        apiGroup: rbac.authorization.k8s.io 
    subjects: 
       - kind: User 
         name: clusterUser 
         apiGroup: rbac.authorization.k8s.io
    ```

2. Se você estiver configurando isso pela primeira vez, você cria a associação de regra de cluster executando o seguinte comando: `kubectl create -f LogReaderRBAC.yaml`. Se você tiver habilitado o suporte para a versão prévia de logs dinâmicos antes, apresentamos os logs de eventos ao vivo, para atualizar sua configuração, execute o seguinte comando: `kubectl apply -f LiveLogRBAC.yml`. 

## <a name="configure-aks-with-azure-active-directory"></a>Configurar o AKS com o Azure Active Directory
O AKS pode ser configurado para usar o Azure AD (Active Directory) na autenticação do usuário. Se você estiver configurando isso pela primeira vez, confira [Integrar o Azure Active Directory ao Serviço de Kubernetes do Azure](../../aks/azure-ad-integration.md). Durante as etapas para criar o [aplicativo cliente](../../aks/azure-ad-integration.md#create-client-application) e especificar o **URI de redirecionamento**, você precisará adicionar outro URI à lista `https://ininprodeusuxbase.microsoft.com/*`.  

>[!NOTE]
>A configuração da autenticação com o Azure Active Directory para logon único só pode ser realizada durante a implantação inicial de um novo cluster do AKS. Não é possível configurar o logon único em um cluster do AKS já implantado. Você deve configurar a autenticação do **registro do aplicativo (herdado)** opção no Azure AD para o suporte ao uso de um caractere curinga no URI e enquanto adicioná-lo à lista, registre-o como um **nativo** aplicativo.
> 

## <a name="view-live-logs-and-events"></a>Exibir logs em tempo real e eventos

Você pode exibir eventos de log em tempo real, como eles são gerados pelo mecanismo de contêiner da **nós**, **controladores**, e **contêineres** modo de exibição. No painel de propriedades que você selecione **exibir dados em tempo real (versão prévia)** opção e um painel é apresentada abaixo da tabela de dados de desempenho onde você pode exibir o log e eventos em um fluxo contínuo. 

![Opção de logs dinâmicos de exibição de painel do nó propriedades](./media/container-insights-live-logs/node-properties-live-logs-01.png)  

Mensagens de log e eventos são limitadas com base em qual tipo de recurso é selecionado na exibição.

| Visualizar | Tipo de recurso | Evento ou log | Dados apresentados |
|------|---------------|--------------|----------------|
| Nós | Nó | Evento | Quando um nó é selecionado eventos não são filtrados e mostram os eventos de Kubernetes de todo o cluster. O título do painel mostra o nome do cluster. |
| Nós | Pod | Evento | Quando um pod é selecionado eventos são filtrados para seu namespace. O título do painel mostra o namespace do pod. | 
| Controladores | Pod | Evento | Quando um pod é selecionado eventos são filtrados para seu namespace. O título do painel mostra o namespace do pod. |
| Controladores | Controlador | Evento | Quando é selecionado a um controlador de eventos são filtrados para seu namespace. O título do painel mostra o namespace do controlador. |
| Nós/controladores/contêineres | Contêiner | Logs | O título do painel mostra que o nome do pod de contêiner é agrupado com. |

Se o cluster do AKS estiver configurado com SSO usando o AAD, você precisará se autenticar no primeiro uso durante a sessão do navegador. Selecione sua conta e conclua a autenticação com o Azure.  

Depois de autenticar com êxito, o painel de log em tempo real será exibido na seção inferior do painel central. Se o indicador de status de busca mostrar uma marca de seleção verde, que está à direita do painel, significa que ele poderá recuperar dados.
    
  ![Dados recuperados do painel de logs em tempo real](./media/container-insights-live-logs/live-logs-pane-01.png)  

Na barra de pesquisa, você pode filtrar por palavra-chave para destacar que o texto no evento ou log e na barra de pesquisa à direita, ele mostra quantos resultados correspondem horizontalmente o filtro.   

  ![Exemplo de filtro do painel de logs em tempo real](./media/container-insights-live-logs/live-logs-pane-filter-example-01.png)

Para suspender a rolagem automática e controlar o comportamento do painel e permitem que você percorra manualmente os novos dados de leitura, clique no **rolagem** opção. Para habilitar novamente a rolagem automática, basta clicar o **rolagem** opção novamente. Você também pode pausar a recuperação de dados de log ou eventos clicando na **pausar** opção e quando estiver pronto para continuar, basta clicar **reproduzir**.  

![Pausar exibição em tempo real no painel de logs em tempo real](./media/container-insights-live-logs/live-logs-pane-pause-01.png)

Você pode ir para os Logs do Azure Monitor para ver os logs de históricos de contêiner, selecionando **exibir logs do contêiner** na lista suspensa **modo de exibição no analytics**.

## <a name="next-steps"></a>Próximas etapas
- Para continuar aprendendo a usar o Azure Monitor e monitorar outros aspectos do cluster do AKS, confira [Exibir integridade do Serviço de Kubernetes do Azure](container-insights-analyze.md).
- Modo de exibição [exemplos de consulta de log](container-insights-log-search.md#search-logs-to-analyze-data) para ver consultas predefinidas e exemplos para avaliar ou personalizar para alertas, visualizar ou analisar seus clusters.
