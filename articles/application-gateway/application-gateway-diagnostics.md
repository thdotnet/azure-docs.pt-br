---
title: "Monitorar logs de acesso, logs de desempenho, integridade do back-end e métricas para o Gateway de Aplicativo | Microsoft Docs"
description: Saiba como habilitar e gerenciar logs de acesso e de desempenho para o Application Gateway
services: application-gateway
documentationcenter: na
author: amitsriva
manager: rossort
editor: tysonn
tags: azure-resource-manager
ms.assetid: 300628b8-8e3d-40ab-b294-3ecc5e48ef98
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/17/2017
ms.author: amitsriva
translationtype: Human Translation
ms.sourcegitcommit: d65b354bc972c8268f1b4f072843b5bf4977a7c4
ms.openlocfilehash: 2b37bf92ce8945996eb64477c28bea845b7df516
ms.lasthandoff: 02/09/2017


---
# <a name="backend-health-diagnostics-logging-and-metrics-for-application-gateway"></a>Integridade do back-end, registro em log e métricas de diagnóstico do Gateway de Aplicativo

O Azure oferece a capacidade de monitorar recursos com métricas e registro em log. O Gateway de Aplicativo fornece esses recursos com integridade do back-end, registro em log e métricas.

[**Integridade do back-end** ](#backend-health) - o gateway de aplicativo oferece a capacidade de monitorar a integridade dos servidores nos pools de back-end por meio do portal e por meio do PowerShell. A integridade dos pools de back-end também pode ser verificada usando os logs de diagnóstico de desempenho.

[**Registro em log**](#enable-logging-with-powershell) – o registro em log permite que os logs de desempenho, acesso e outros sejam salvos ou consumidos de um recurso para fins de monitoramento.

[**Métricas**](#metrics) – atualmente, o gateway de aplicativo atualmente tem uma métrica. Essa métrica mede a taxa de transferência do gateway de aplicativo em Bytes por segundo.

## <a name="backend-health"></a>Integridade do back-end

O gateway de aplicativo fornece a capacidade de monitorar a integridade dos membros individuais dos pools de back-end por meio do portal, do PowerShell e da CLI. Um resumo de integridade agregada dos pools de back-end também pode ser encontrado nos logs de diagnóstico de desempenho. O relatório de integridade do back-end reflete a saída do teste de integridade do Gateway de Aplicativo para as instâncias de back-end. Quando a investigação for bem-sucedida e o back-end puder ser atendido pelo tráfego, ele será considerado íntegro. Caso contrário, ele é considerado não íntegro.

> [!important]
> Se houver um NSG na sub-rede do Gateway de Aplicativo, os intervalos de porta 65503-65534 deverão ser abertos em instâncias de Gateway de Aplicativo.

### <a name="view-backend-health-through-the-portal"></a>Exibir a integridade do back-end no portal

Não há nada que precisa ser feito para exibir a integridade do back-end. Em um gateway de aplicativo existente, navegue até **Monitoramento** > **Integridade do back-end**. Cada membro no pool de back-end é listado nesta página (seja NIC, IP ou FQDN). Aparecem o nome do pool de back-end, a porta, as configurações de HTTP de back-end e o status de integridade. Os valores válidos para o status de integridade são "Íntegro", "Não íntegro" e "Desconhecido".

> [!WARNING]
> Se você vir o status de integridade do back-end como **Desconhecido**, verifique se o acesso ao back-end não está bloqueado por uma regra de NSG (grupo de segurança de rede) ou por um DNS personalizado na VNET.

![integridade do back-end][10]

### <a name="view-backend-health-with-powershell"></a>Exibir a integridade do back-end com o PowerShell

A integridade do back-end também pode ser recuperada por meio do PowerShell. O código do PowerShell a seguir mostra como integrar a integridade do back-end com o cmdlet `Get-AzureRmApplicationGatewayBackendHealth`.

```powershell
Get-AzureRmApplicationGatewayBackendHealth -Name ApplicationGateway1 -ResourceGroupName Contoso
```

Os resultados são retornados e um exemplo da resposta é mostrado no trecho a seguir.

```json
{
"BackendAddressPool": {
    "Id": "/subscriptions/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendAddressPools/appGatewayBackendPool"
},
"BackendHttpSettingsCollection": [
    {
    "BackendHttpSettings": {
        "Id": "/00000000-0000-0000-000000000000/resourceGroups/ContosoRG/providers/Microsoft.Network/applicationGateways/applicationGateway1/backendHttpSettingsCollection/appGatewayBackendHttpSettings"
    },
    "Servers": [
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        },
        {
        "Address": "hostname.westus.cloudapp.azure.com",
        "Health": "Healthy"
        }
    ]
    }
]
}
```

## <a name="diagnostic-logging"></a>Registro em log de diagnóstico

Você pode usar tipos diferentes de logs no Azure para gerenciar e solucionar problemas de gateways de aplicativo. Alguns desses logs podem ser acessados por meio do portal, e todos os logs podem ser extraídos de um armazenamento de blobs do Azure e exibidos em diferentes ferramentas, como o [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md), o Excel e o PowerBI. Saiba mais sobre os tipos diferentes de logs na lista a seguir:

* **Log de atividades:** você pode usar o [Log de Atividades do Azure](../monitoring-and-diagnostics/insights-debugging-with-events.md) (anteriormente conhecido como Logs Operacionais e Logs de Auditoria) para exibir todas as operações que estão sendo enviadas à sua assinatura do Azure, bem como o respectivo status. As entradas do log de atividades são coletadas por padrão e podem ser exibidas no Portal do Azure.
* **Logs de acesso:** você pode usar esse log para exibir o padrão de acesso do gateway de aplicativo e analisar informações importantes, incluindo o IP do chamador, a URL solicitada, a latência de resposta, o código de retorno, os bytes de entrada e de saída. O log de acesso é coletado a cada 300 segundos. Esse log contém um registro por instância do gateway de aplicativo. A instância do gateway de aplicativo pode ser identificada pela propriedade ‘instanceId’.
* **Logs de desempenho:** você pode usar esse log para exibir o desempenho das instâncias do gateway de aplicativo. Esse log captura informações sobre o desempenho por instância, incluindo a solicitação total atendida, a taxa de transferência em bytes, o total de solicitações atendidas, a contagem de solicitações com falha, a contagem de instâncias de back-end íntegras ou não. O log de desempenho é coletado a cada 60 segundos.
* **Logs do firewall:** use esse log para exibir as solicitações registradas por meio do modo de detecção ou prevenção de um gateway de aplicativo configurado com o firewall do aplicativo Web.

> [!WARNING]
> Os logs estão disponíveis apenas para os recursos implantados no modelo de implantação do Gerenciador de Recursos. Você não pode usar logs para recursos do modelo de implantação clássico. Para obter uma melhor compreensão dos dois modelos, consulte o artigo [Noções básicas sobre a implantação do Gerenciador de Recursos e a implantação clássica](../azure-resource-manager/resource-manager-deployment-model.md) .

Para Logs, há três opções diferentes de armazenamento de logs.

* Conta de armazenamento - as contas de armazenamento são mais aproveitadas para logs quando os registros são armazenados por mais tempo e examinados quando necessário.
* Hubs de eventos - os hubs de eventos são uma ótima opção para integrar a outras ferramentas SEIM e receber alertas sobre os recursos
* Log Analytics - o Log Analytics é mais adequado para o monitoramento em tempo real do seu aplicativo ou para a observação de tendências.

### <a name="enable-logging-with-powershell"></a>Habilitar o registro em log com o PowerShell

O log de atividade é habilitado automaticamente para todos os recursos do Resource Manager. Você deve habilitar o log de acesso e de desempenho para começar a coletar os dados disponíveis por meio desses logs. Para habilitar o registro em log, veja as etapas a seguir:

1. Observe a ID do Recurso da conta de armazenamento onde os dados de log são armazenados. Esse valor deve ter o seguinte formato: /subscriptions/\<IddaAssinatura\>/resourceGroups/\<nome do grupo de recursos\>/providers/Microsoft.Storage/storageAccounts/\<nome da conta de armazenamento\>. Todas as contas de armazenamento da assinatura podem ser usadas. Você pode usar o portal de visualização para encontrar essas informações.

    ![Portal de visualização - diagnóstico do Application Gateway](./media/application-gateway-diagnostics/diagnostics1.png)

2. Anote a ID do Recurso do gateway de aplicativo para o qual o log deve ser habilitado. Esse valor deve ter o seguinte formato: /subscriptions/\<IddaAssinatura\>/resourceGroups/\<nome do grupo de recursos\>/providers/Microsoft.Network/applicationGateways/\<nome do gateway de aplicativo\>. Você pode usar o portal de visualização para encontrar essas informações.

    ![Portal de visualização - diagnóstico do Application Gateway](./media/application-gateway-diagnostics/diagnostics2.png)

3. Habilite o log de diagnóstico usando o cmdlet do powershell a seguir:

    ```powershell
    Set-AzureRmDiagnosticSetting  -ResourceId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name> -StorageAccountId /subscriptions/<subscriptionId>/resourceGroups/<resource group name>/providers/Microsoft.Storage/storageAccounts/<storage account name> -Enabled $true     
    ```
    
> [!TIP] 
>Os logs de atividades não exigem uma conta de armazenamento separada. O uso do armazenamento para logs de acesso e de desempenho gera encargos de serviço.

### <a name="enable-logging-with-azure-portal"></a>Habilitar registro em log com o Portal do Azure

#### <a name="step-1"></a>Etapa 1

Navegue até seu recurso no Portal do Azure. Clique em **Logs de diagnóstico**. Se esta for a primeira vez que você configura o diagnóstico, a folha terá a imagem:

Para o gateway de aplicativo, há três logs disponíveis.

* Log de acesso
* Log de desempenho
* Log de firewall

Para iniciar a coleta de dados., clique em **Ativar diagnóstico**.

![folha de configuração de diagnóstico][1]

#### <a name="step-2"></a>Etapa 2

Na folha **Configurações de diagnóstico** , as configurações de como os logs de diagnóstico estão definidos. Neste exemplo, o Log Analytics é usado para armazenar os logs. Clique em **Configurar** em **Log Analytics** para configurar seu espaço de trabalho. É possível usar Hubs de Eventos e uma conta de armazenamento para salvar os logs de diagnóstico.

![folha de diagnósticos][2]

#### <a name="step-3"></a>Etapa 3

Escolha um espaço de trabalho OMS ou crie um novo. Para este exemplo, usamos um existente.

![espaços de trabalho do OMS][3]

#### <a name="step-4"></a>Etapa 4

Ao concluir, confirme as configurações e clique em **Salvar** para salvar as configurações.

![confirmar seleção][4]

### <a name="activity-log"></a>Log de atividades

Por padrão, esse log (anteriormente conhecido como "log operacional") é gerado pelo Azure.  Os logs são preservados por 90 dias no repositório de Logs de Eventos do Azure. Saiba mais sobre esses logs lendo o artigo [Exibir eventos e logs de atividades](../monitoring-and-diagnostics/insights-debugging-with-events.md).

### <a name="access-log"></a>Log de acesso

Esse log só será gerado se você o tiver habilitado por Application Gateway como detalhado nas etapas anteriores. Os dados são armazenados na conta de armazenamento especificada quando você habilitou o registro em log. Cada acesso do Gateway de Aplicativo é registrado no formato JSON, conforme mostrado no exemplo a seguir:

```json
{
    "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
    "operationName": "ApplicationGatewayAccess",
    "time": "2016-04-11T04:24:37Z",
    "category": "ApplicationGatewayAccessLog",
    "properties": {
        "instanceId":"ApplicationGatewayRole_IN_0",
        "clientIP":"37.186.113.170",
        "clientPort":"12345",
        "httpMethod":"HEAD",
        "requestUri":"/xyz/portal",
        "requestQuery":"",
        "userAgent":"-",
        "httpStatus":"200",
        "httpVersion":"HTTP/1.0",
        "receivedBytes":"27",
        "sentBytes":"202",
        "timeTaken":"359",
        "sslEnabled":"off"
    }
}
```

### <a name="performance-log"></a>Log de desempenho

Esse log só será gerado se você o tiver habilitado por Application Gateway como detalhado nas etapas anteriores. Os dados são armazenados na conta de armazenamento especificada quando você habilitou o registro em log. Os seguintes dados são registrados em log:

```json
{
    "resourceId": "/SUBSCRIPTIONS/<subscription id>/RESOURCEGROUPS/<resource group name>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<application gateway name>",
    "operationName": "ApplicationGatewayPerformance",
    "time": "2016-04-09T00:00:00Z",
    "category": "ApplicationGatewayPerformanceLog",
    "properties":
    {
        "instanceId":"ApplicationGatewayRole_IN_1",
        "healthyHostCount":"4",
        "unHealthyHostCount":"0",
        "requestCount":"185",
        "latency":"0",
        "failedRequestCount":"0",
        "throughput":"119427"
    }
}
```

> [!NOTE]
> A latência é calculada a partir do momento que o primeiro byte da solicitação HTTP é recebido até a hora em que o último byte da resposta HTTP é enviado. É a soma do tempo de processamento do Gateway de Aplicativo, mais o custo de rede até o back-end, mais o tempo que o back-end leva para processar a solicitação.

### <a name="firewall-log"></a>Log de firewall

Esse log só será gerado se você o tiver habilitado por Application Gateway como detalhado nas etapas anteriores. Esse log também exige a configuração de um firewall de aplicativo Web em um gateway de aplicativo. Os dados são armazenados na conta de armazenamento especificada quando você habilitou o registro em log. Os seguintes dados são registrados em log:

```json
{
    "resourceId": "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/<applicationGatewayName>",
    "operationName": "ApplicationGatewayFirewall",
    "time": "2016-09-20T00:40:04.9138513Z",
    "category": "ApplicationGatewayFirewallLog",
    "properties":     {
        "instanceId":"ApplicationGatewayRole_IN_0",
        "clientIp":"108.41.16.164",
        "clientPort":1815,
        "requestUri":"/wavsep/active/RXSS-Detection-Evaluation-POST/",
        "ruleId":"OWASP_973336",
        "message":"XSS Filter - Category 1: Script Tag Vector",
        "action":"Logged",
        "site":"Global",
        "message":"XSS Filter - Category 1: Script Tag Vector",
        "details":{"message":" Warning. Pattern match "(?i)(<script","file":"/owasp_crs/base_rules/modsecurity_crs_41_xss_attacks.conf","line":"14"}}
}
```

### <a name="view-and-analyze-the-activity-log"></a>Exibir e analisar o log de atividades

Você pode exibir e analisar dados do log de atividades usando qualquer um dos seguintes métodos:

* **Ferramentas do Azure:** recupere informações do log de atividades por meio do Azure PowerShell, da CLI (Interface de Linha de Comando) do Azure, da API REST do Azure ou da Versão Prévia do Portal do Azure.  As instruções passo a passo para cada método são detalhadas no artigo [Activity operations with Resource Manager](../azure-resource-manager/resource-group-audit.md) (Operações de atividade com o Resource Manager).
* **Power BI:** se ainda não tiver uma conta do [Power BI](https://powerbi.microsoft.com/pricing) , você poderá testá-lo gratuitamente. Usando o [Pacote de conteúdo dos Logs de Atividades do Azure para Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-content-pack-azure-audit-logs/), você pode analisar seus dados com painéis pré-configurados que podem ser usados como estão ou podem ser personalizados.

## <a name="view-and-analyze-the-access-performance-and-firewall-log"></a>Exibir e analisar o log de acesso, de desempenho e de firewall

O [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md) do Azure pode coletar o contador e log de eventos de arquivos da sua conta de armazenamento de Blobs e inclui visualizações e recursos avançados de pesquisa para analisar os logs.

Você também pode se conectar à sua conta de armazenamento e recuperar as entradas de log JSON para logs de desempenho e acesso. Depois de baixar os arquivos JSON, você pode convertê-los em CSV e exibi-lo no Excel, no PowerBI ou em qualquer outra ferramenta de visualização de dados.

> [!TIP]
> Se estiver familiarizado com o Visual Studio e os conceitos básicos de alteração de valores de constantes e variáveis em C# , você poderá usar as [ferramentas de conversor de log](https://github.com/Azure-Samples/networking-dotnet-log-converter) disponíveis no Github.
> 
> 

## <a name="metrics"></a>Métricas

Métricas são um recurso para certos recursos do Azure, nas quais você pode exibir os contadores de desempenho no portal. Para o Gateway de Aplicativo, havia uma métrica disponível no momento da publicação deste artigo. Essa métrica é a taxa de transferência e pode ser vista no portal. Navegue até um gateway de aplicativo e clique em **Métricas**. Para exibir os valores, selecione a taxa de transferência na seção **Métricas disponíveis**. Na imagem a seguir, você pode ver um exemplo com os filtros que podem ser usados para exibir os dados em intervalos de tempo diferentes.

Para ver uma lista das métricas de suporte atuais, visite [Métricas com suporte com o Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md)

![exibição de métrica][5]

### <a name="alert-rules"></a>Regras de alerta

Regras de alerta podem ser iniciadas com base nas métricas em um recurso. Isso significa que para o gateway de aplicativo, um alerta pode chamar um webhook ou enviar um email a um administrador se a taxa de transferência do gateway de aplicativo ficar acima, abaixo ou no limite durante um período especificado.

O exemplo a seguir guiará você pela criação de uma regra de alerta que envia um email a um administrador após um limite de taxa de transferência ter sido violado.

#### <a name="step-1"></a>Etapa 1

Clique em **Adicionar alerta da métrica** para começar. Essa folha também pode ser acessada a partir da folha de métricas.

![folha de regras de alerta][6]

#### <a name="step-2"></a>Etapa 2

Na folha **Adicionar regra**, preencha as seções de nome, a condição e notificação e clique em **OK** quando terminar.

O seletor **Condição** permite quatro valores, **Maior que**, **Maior ou igual a**, **Menor que** ou **Menor ou igual a**.

O seletor de **Período** permite a escolha de um período de cinco minutos até seis horas.

Selecionando **Proprietários, colaboradores e leitores de email** o email pode ser dinâmico com base nos usuários que têm acesso a esse recurso. Caso contrário, uma lista separada por vírgulas de usuários poderá ser fornecida na caixa de texto **Emails adicionais de administrador** .

![adicionar folha de regras][7]

Se o limite for ultrapassado, um email semelhante à imagem a seguir chegará:

![email de violação de limite][8]

Após a criação de um alerta de métrica uma lista de alertas será exibida e fornecerá uma visão geral de todas as regras de alerta.

![adicionar regra de alerta][9]

Para saber mais sobre notificações de alerta, visite [Receber notificações de alerta](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Para entender mais sobre webhooks e como usá-los com alertas, visite [Configurar um webhook em um alerta de métrica do Azure](../monitoring-and-diagnostics/insights-webhooks-alerts.md)

## <a name="next-steps"></a>Próximas etapas

* Visualizar o contador e logs de eventos com o [Log Analytics](../log-analytics/log-analytics-azure-networking-analytics.md)
* Postagem de blog [Visualize your Azure Activity Log with Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/09/30/monitor-azure-audit-logs-with-power-bi.aspx) (Visualizar o Log de Atividades do Azure com o Power BI).
* Postagem de blog [View and analyze Azure Activity Logs in Power BI and more](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/)(Exibir e analisar Logs de Atividades do Azure no Power BI e muito mais).

[1]: ./media/application-gateway-diagnostics/figure1.png
[2]: ./media/application-gateway-diagnostics/figure2.png
[3]: ./media/application-gateway-diagnostics/figure3.png
[4]: ./media/application-gateway-diagnostics/figure4.png
[5]: ./media/application-gateway-diagnostics/figure5.png
[6]: ./media/application-gateway-diagnostics/figure6.png
[7]: ./media/application-gateway-diagnostics/figure7.png
[8]: ./media/application-gateway-diagnostics/figure8.png
[9]: ./media/application-gateway-diagnostics/figure9.png
[10]: ./media/application-gateway-diagnostics/figure10.png
