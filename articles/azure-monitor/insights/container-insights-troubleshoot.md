---
title: Como solucionar problemas do Azure Monitor para contêineres | Microsoft Docs
description: Este artigo descreve como você pode solucionar e resolver problemas com o Azure Monitor para contêineres.
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
ms.date: 03/27/2018
ms.author: magoedte
ms.openlocfilehash: b6c245142eea12bcec5ed642ec9bd91a58e10eb0
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2019
ms.locfileid: "68813762"
---
# <a name="troubleshooting-azure-monitor-for-containers"></a>Solução de problemas do Azure Monitor para contêineres

Quando você configurar o monitoramento do cluster do AKS (Serviço de Kubernetes do Azure) com o Azure Monitor para contêineres, poderá encontrar um problema que impede a coleta de dados ou relatórios de status. Este artigo detalha alguns problemas comuns e etapas de solução de problemas.

## <a name="authorization-error-during-onboarding-or-update-operation"></a>Erro de autorização durante a operação de atualização ou integração
Ao habilitar Azure Monitor para contêineres ou atualizar um cluster para dar suporte à coleta de métricas, você pode receber um erro semelhante ao seguinte: *a identidade do usuário < cliente > ' com ID de objeto ' < ObjectID do usuário > ' não tem autorização para executar ação ' Microsoft. Authorization/roleAssignments/Write ' sobre escopo*

Durante a integração ou o processo de atualização, é tentada a concessão da atribuição de função de editor de métricas de **monitoramento** no recurso de cluster. O usuário que inicia o processo para habilitar a Azure Monitor para contêineres ou a atualização para dar suporte à coleção de métricas deve ter acesso à permissão **Microsoft. Authorization/roleAssignments/Write** no escopo de recurso de cluster AKs. Somente os membros das funções internas do **proprietário** e do **administrador de acesso do usuário** recebem acesso a essa permissão. Se suas políticas de segurança exigirem a atribuição de permissões de nível granular, recomendamos que você exiba [funções personalizadas](../../role-based-access-control/custom-roles.md) e atribua-as aos usuários que precisam dela. 

Você também pode conceder manualmente essa função da portal do Azure executando as seguintes etapas:

1. Entre no [Portal do Azure](https://portal.azure.com). 
2. No Portal do Azure, clique em **Todos os serviços**, localizado no canto superior esquerdo. Na lista de recursos, digite **kubernetes**. Quando você começa a digitar, a lista é filtrada com base em sua entrada. Selecione **Azure kubernetes**.
3. Na lista de clusters kubernetes, selecione um na lista.
2. No menu à esquerda, clique em **controle de acesso (iam)** .
3. Selecione **+ Adicionar** para adicionar uma atribuição de função e selecione a função de editor de métricas de **monitoramento** e, na caixa **selecionar** , digite **AKs** para filtrar os resultados em apenas as entidades de serviço de clusters definidas na assinatura. Selecione aquele na lista específica para esse cluster.
4. Selecione **Salvar** para finalizar a atribuição da função. 

## <a name="azure-monitor-for-containers-is-enabled-but-not-reporting-any-information"></a>O Azure Monitor para contêineres está habilitado, mas não relata nenhuma informação
Se Azure Monitor para contêineres for habilitado e configurado com êxito, mas você não puder exibir informações de status ou nenhum resultado for retornado de uma consulta de log, você diagnosticará o problema seguindo estas etapas: 

1. Verifique o status do agente executando o comando: 

    `kubectl get ds omsagent --namespace=kube-system`

    A saída deve ser semelhante à seguinte, que indica que ela foi implantada corretamente:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Verifique o status de implantação com a versão do agente *06072018* ou posterior usando o comando:

    `kubectl get deployment omsagent-rs -n=kube-system`

    A saída deve ser semelhante ao seguinte exemplo, que indica que ela foi implantada corretamente:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Verifique o status do pod para verificar se ele está em execução usando o comando: `kubectl get pods --namespace=kube-system`

    A saída deve ser semelhante ao seguinte exemplo, com o status de *Executando* para o omsagent:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

4. Verifique os logs de agente. Quando o agente em contêineres é implantado, ele executa uma verificação rápida executando comandos do OMI e mostra a versão do agente e do provedor. 

5. Para verificar se o agente foi implantado com êxito, execute o comando:`kubectl logs omsagent-484hw --namespace=kube-system`

    O status deve ser semelhante a este exemplo:

    ```
    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.2
    omsagent 1.6.0.23
    docker-cimprov 1.0.0.31
    ```

## <a name="error-messages"></a>Mensagens de erro

A tabela abaixo resume os erros conhecidos que você pode encontrar ao usar o Monitor do Azure para contêineres.

| Mensagens de erro  | Action |  
| ---- | --- |  
| Mensagem de erro `No data for selected filters`  | Pode levar algum tempo para estabelecer o fluxo de dados de monitoramento para clusters recém-criados. Aguarde pelo menos 10 a 15 minutos para que os dados sejam exibidos para o cluster. |   
| Mensagem de erro `Error retrieving data` | Enquanto o cluster do Serviço Azure Kubernetes está sendo configurado para monitoramento de integridade e desempenho, é estabelecida uma conexão entre o cluster e o espaço de trabalho do Azure Log Analytics. Um espaço de trabalho do Log Analytics é usado para armazenar todos os dados de monitoramento do cluster. Esse erro pode ocorrer quando seu espaço de trabalho Log Analytics foi excluído. Verifique se o espaço de trabalho foi excluído e, se ele foi, será necessário reabilitar o monitoramento do cluster com Azure Monitor para contêineres e especificar um novo espaço de trabalho ou criar um existente. Para reabilitar, você precisará [desabilitar](container-insights-optout.md) o monitoramento do cluster e [habilitar](container-insights-enable-new-cluster.md) Azure monitor para contêineres novamente. |  
| `Error retrieving data` Depois de adicionar o Azure Monitor para contêineres por meio da cli do az aks | Ao habilitar o monitoramento `az aks cli`usando, os Azure monitor para contêineres podem não ser implantados corretamente. Verifique se a solução está implantada. Para fazer isso, vá para o espaço de trabalho do Log Analytics e veja se a solução está disponível selecionando **Soluções** no painel à esquerda. Para resolver esse problema, você precisará reimplantar a solução seguindo as instruções em [como implantar o Monitor do Azure para contêineres](container-insights-onboard.md) |  

Para ajudar a diagnosticar o problema, fornecemos um script de solução de problemas disponível [aqui](https://github.com/Microsoft/OMS-docker/tree/ci_feature_prod/Troubleshoot#troubleshooting-script).  

## <a name="next-steps"></a>Próximas etapas

Com o monitoramento habilitado para capturar métricas de integridade para os nós de cluster do AKS e pods, essas métricas de integridade estão disponíveis no portal do Azure. Para saber como usar o Azure Monitor para contêineres, veja [Exibir integridade do Serviço de Kubernetes do Azure](container-insights-analyze.md).