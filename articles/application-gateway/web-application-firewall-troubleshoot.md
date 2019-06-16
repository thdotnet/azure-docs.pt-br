---
title: Solucionar problemas de Firewall do aplicativo Web para o Gateway de aplicativo do Azure
description: Este artigo fornece informações de solução de problemas de Firewall de aplicativo da Web (WAF) para o Gateway de aplicativo do Azure
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.date: 6/11/2019
ms.author: ant
ms.topic: conceptual
ms.openlocfilehash: b81ffc5769fa5521094734da5d00aab77aa7cd35
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67082437"
---
# <a name="troubleshoot-web-application-firewall-waf-for-azure-application-gateway"></a>Solucionar problemas de Firewall do aplicativo Web (WAF) para o Gateway de aplicativo do Azure

Há algumas coisas que você pode fazer se as solicitações que devem passar por meio do Firewall de aplicativo de Web (WAF) são bloqueadas.

Primeiro, verifique se você leu a [visão geral do WAF](waf-overview.md) e o [configuração do WAF](application-gateway-waf-configuration.md) documentos. Além disso, verifique se você habilitou [WAF monitoramento](application-gateway-diagnostics.md) estes artigos explicam como funciona o WAF, como os conjuntos de trabalho, a regra de WAF e como acessar logs de WAF.

## <a name="understanding-waf-logs"></a>Noções básicas sobre o WAF logs

A finalidade dos logs de WAF é mostrar todas as solicitações que corresponde ou é bloqueada pelo WAF. É uma razão de todas as solicitações avaliadas são correspondidos ou bloqueado. Se você perceber que o WAF bloqueia uma solicitação que não deveria (um falso positivo), você pode fazer algumas coisas. Primeiro, restringir e encontrar a solicitação específica. Examine os logs para encontrar a ID de transação, timestamp ou URI específica da solicitação. Quando você encontrar as entradas de log associado, você pode começar a agir sobre os falsos positivos.

Por exemplo, digamos que você tem um tráfego legítimo que contém a cadeia de caracteres *1 = 1* que você deseja passar por meio do seu WAF. Se você tentar a solicitação, o WAF bloqueia o tráfego que contém seu *1 = 1* cadeia de caracteres em qualquer parâmetro ou campo. Isso é uma cadeia de caracteres geralmente associada a um ataque de injeção de SQL. Você pode examinar os logs e ver o carimbo de hora da solicitação e as regras que corresponderam/bloqueado.

No exemplo a seguir, você pode ver que as quatro regras são acionadas durante a mesma solicitação (usando o campo de TransactionId). O primeiro deles diz que ele correspondeu porque o usuário utilizou um IP/numérico URL da solicitação, o que aumenta a pontuação de anomalias em três, pois ele é um aviso. A próxima regra correspondente é 942130, que é aquele que você está procurando. Você pode ver os *1 = 1* no `details.data` campo. Isso aumenta ainda mais a pontuação de anomalias em três novamente, como também é um aviso. Em geral, cada regra que tem a ação **Matched** pontuação de anomalia aumenta, e agora a pontuação de anomalias seria seis. Para obter mais informações, consulte [modo de pontuação de anomalias](waf-overview.md#anomaly-scoring-mode).

As entradas de dois log final mostram que a solicitação foi bloqueada porque a pontuação de anomalias foi alta o suficiente. Essas entradas têm uma ação diferente que as outras duas. Eles mostram que eles realmente *bloqueado* a solicitação. Essas regras são obrigatórias e não podem ser desabilitadas. Elas não devem ser consideradas como regras, mas mais como infraestrutura básica do WAF itens internos.

```json
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "OWASP_CRS", 
        "ruleSetVersion": "3.0.0", 
        "ruleId": "920350", 
        "message": "Host header is a numeric IP address", 
        "action": "Matched", 
        "site": "Global", 
        "details": { 
            "message": "Warning. Pattern match \\\"^[\\\\\\\\d.:]+$\\\" at REQUEST_HEADERS:Host. ", 
            "data": "40.90.218.160", 
            "file": "rules\/REQUEST-920-PROTOCOL-ENFORCEMENT.conf\\\"", 
            "line": "791" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    } 
} 
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "OWASP_CRS", 
        "ruleSetVersion": "3.0.0", 
        "ruleId": "942130", 
        "message": "SQL Injection Attack: SQL Tautology Detected.", 
        "action": "Matched", 
        "site": "Global", 
        "details": { 
            "message": "Warning. Pattern match \\\"(?i:([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)([\\\\\\\\d\\\\\\\\w]++)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?:(?:=|\\u003c=\\u003e|r?like|sounds\\\\\\\\s+like|regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)\\\\\\\\2|(?:!=|\\u003c=|\\u003e=|\\u003c\\u003e|\\u003c|\\u003e|\\\\\\\\^|is\\\\\\\\s+not|not\\\\\\\\s+like|not\\\\\\\\s+regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?!\\\\\\\\2)([\\\\\\\\d\\\\\\\\w]+)))\\\" at ARGS:text1. ", 
            "data": "Matched Data: 1=1 found within ARGS:text1: 1=1", 
            "file": "rules\/REQUEST-942-APPLICATION-ATTACK-SQLI.conf\\\"", 
            "line": "554" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    } 
} 
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "", 
        "ruleSetVersion": "", 
        "ruleId": "0", 
        "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Score: 8)", 
        "action": "Blocked", 
        "site": "Global", 
        "details": { 
            "message": "Access denied with code 403 (phase 2). Operator GE matched 5 at TX:anomaly_score. ", 
            "data": "", 
            "file": "rules\/REQUEST-949-BLOCKING-EVALUATION.conf\\\"", 
            "line": "57" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    } 
} 
{ 
    "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2", 
    "operationName": "ApplicationGatewayFirewall", 
    "category": "ApplicationGatewayFirewallLog", 
    "properties": { 
        "instanceId": "appgw_3", 
        "clientIp": "167.220.2.139", 
        "clientPort": "", 
        "requestUri": "\/", 
        "ruleSetType": "", 
        "ruleSetVersion": "", 
        "ruleId": "0", 
        "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Inbound Score: 8 - SQLI=5,XSS=0,RFI=0,LFI=0,RCE=0,PHPI=0,HTTP=0,SESS=0): SQL Injection Attack: SQL Tautology Detected.", 
        "action": "Blocked", 
        "site": "Global", 
        "details": { 
            "message": "Warning. Operator GE matched 5 at TX:inbound_anomaly_score. ", 
            "data": "", 
            "file": "rules\/RESPONSE-980-CORRELATION.conf\\\"", 
            "line": "73" 
        }, 
        "hostname": "vm000003", 
        "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt" 
    }
}
```

## <a name="fixing-false-positives"></a>Correção de falsos positivos

Com essas informações e o conhecimento que a regra 942130 é aquela que correspondeu a *1 = 1* cadeia de caracteres, você pode fazer algumas coisas para impedir que isso bloqueando o tráfego:

- Use uma lista de exclusão

   Ver [configuração do WAF](application-gateway-waf-configuration.md#waf-exclusion-lists) para obter mais informações sobre a exclusão de lista.
- Desabilite a regra.

### <a name="using-an-exclusion-list"></a>Usando uma lista de exclusão

Para tomar uma decisão informada sobre como lidar com um falso positivo, é importante para se familiarizar com as tecnologias que seu aplicativo usa. Por exemplo, digamos que existe a pilha de tecnologia, não um SQL server, e você está obtendo os falsos positivos relacionados a essas regras. Desabilitar essas regras não necessariamente diminuir o nível de sua segurança.

Uma vantagem de usar uma lista de exclusão é que apenas uma parte específica de uma solicitação está sendo desabilitada. No entanto, isso significa que uma exclusão específica é aplicável a todo o tráfego passa pelo seu WAF porque ele é uma configuração global. Por exemplo, isso poderia levar a um problema se *1 = 1* é uma solicitação válida no corpo para um determinado aplicativo, mas não para outras pessoas. Outro benefício é que você pode escolher entre cookies, cabeçalhos e corpo a ser excluído se uma determinada condição for atendida, em vez de exclusão de toda a solicitação.

Ocasionalmente, há casos em que parâmetros específicos obterem passados para o WAF de forma que pode não ser intuitiva. Por exemplo, há um token que é passado ao autenticar usando o Azure Active Directory. Esse token *requestverificationtoken*, geralmente obter passado como um Cookie de solicitação. No entanto, em alguns casos em que os cookies estão desabilitados, este token também é passado como um atributo de solicitação ou "arg". Se isso acontecer, você precisa garantir que *requestverificationtoken* é adicionado à lista de exclusão como um **nome do atributo de solicitação** também.

![Exclusões](media/waf-tshoot/exclusion-list.png)

Neste exemplo, você deseja excluir o **nome do atributo de solicitação** que é igual a *text1*. Isso é aparente porque você pode ver o nome do atributo nos logs de firewall: **dados: Dados correspondentes: 1 = 1 encontrado no ARGS:text1: 1=1**. O atributo estiver **text1**. Você também pode encontrar esse nome de atributo algumas outras maneiras, veja [Localizando os nomes de atributo de solicitação](#finding-request-attribute-names).

![Listas de exclusão de WAF](media/waf-tshoot/waf-config.png)

### <a name="disabling-rules"></a>Desabilitar regras

Outra maneira de contornar um falso positivo é desabilitar a regra que corresponderam a ideia de entrada, o WAF era mal-intencionado. Desde que você tiver analisado os logs de WAF e reduziram a regra para baixo até 942130, você pode desabilitá-lo no portal do Azure. Ver [personalizar regras de firewall de aplicativo web por meio do portal do Azure](application-gateway-customize-waf-rules-portal.md).

Um benefício de desabilitar uma regra é que, se você souber que todo o tráfego que contém uma determinada condição que normalmente seriam bloqueada tráfego é válido, você pode desabilitar essa regra para o WAF inteiro. No entanto, se ele somente tráfego é válido em um caso de uso específico, você abrirá uma vulnerabilidade desabilitando a regra para o WAF inteiro, pois é uma configuração global.

Se você quiser usar o Azure PowerShell, consulte [personalizar regras de firewall de aplicativo web por meio do PowerShell](application-gateway-customize-waf-rules-powershell.md). Se você quiser usar a CLI do Azure, consulte [personalizar regras de firewall de aplicativo web por meio da CLI do Azure](application-gateway-customize-waf-rules-cli.md).

![Regras de WAF](media/waf-tshoot/waf-rules.png)

## <a name="finding-request-attribute-names"></a>Nomes de atributo de solicitação de localização

Com a Ajuda do [Fiddler](https://www.telerik.com/fiddler), inspecionar solicitações individuais e determinar quais campos específicos de uma página da web são chamados. Isso pode ajudar a excluir determinados campos da inspeção usando listas de exclusão.

Neste exemplo, você pode ver que o campo no qual o *1 = 1* cadeia de caracteres foi inserida é chamado **text1**.

![Fiddler](media/waf-tshoot/fiddler-1.png)

Este é um campo que você pode excluir. Para saber mais sobre listas de exclusão, consulte [limites de tamanho de solicitação do Web application firewall e listas de exclusão](application-gateway-waf-configuration.md#waf-exclusion-lists). É possível excluir a avaliação nesse caso, configurando a exclusão a seguir:

![Exclusão de WAF](media/waf-tshoot/waf-exclusion-02.png)

Você também pode examinar os logs de firewall para obter as informações para ver o que você precisa adicionar a lista de exclusão. Para habilitar o registro em log, consulte [integridade do Back-end, logs de diagnóstico e métricas para o Gateway de aplicativo](application-gateway-diagnostics.md).

Examine o log de firewall e exiba o arquivo PT1H.json para a hora em que ocorreu a solicitação que você deseja inspecionar.

Neste exemplo, você pode ver que você tenha quatro regras com o mesmo TransactionID e que todos eles ocorreram ao mesmo tempo exato:

```json
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "OWASP_CRS",
-           "ruleSetVersion": "3.0.0",
-           "ruleId": "920350",
-           "message": "Host header is a numeric IP address",
-           "action": "Matched",
-           "site": "Global",
-           "details": {
-               "message": "Warning. Pattern match \\\"^[\\\\\\\\d.:]+$\\\" at REQUEST_HEADERS:Host. ",
-               "data": "40.90.218.160",
-               "file": "rules\/REQUEST-920-PROTOCOL-ENFORCEMENT.conf\\\"",
-               "line": "791"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "OWASP_CRS",
-           "ruleSetVersion": "3.0.0",
-           "ruleId": "942130",
-           "message": "SQL Injection Attack: SQL Tautology Detected.",
-           "action": "Matched",
-           "site": "Global",
-           "details": {
-               "message": "Warning. Pattern match \\\"(?i:([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)([\\\\\\\\d\\\\\\\\w]++)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?:(?:=|\\u003c=\\u003e|r?like|sounds\\\\\\\\s+like|regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)\\\\\\\\2|(?:!=|\\u003c=|\\u003e=|\\u003c\\u003e|\\u003c|\\u003e|\\\\\\\\^|is\\\\\\\\s+not|not\\\\\\\\s+like|not\\\\\\\\s+regexp)([\\\\\\\\s'\\\\\\\"`\\\\\\\\(\\\\\\\\)]*?)(?!\\\\\\\\2)([\\\\\\\\d\\\\\\\\w]+)))\\\" at ARGS:text1. ",
-               "data": "Matched Data: 1=1 found within ARGS:text1: 1=1",
-               "file": "rules\/REQUEST-942-APPLICATION-ATTACK-SQLI.conf\\\"",
-               "line": "554"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "",
-           "ruleSetVersion": "",
-           "ruleId": "0",
-           "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Score: 8)",
-           "action": "Blocked",
-           "site": "Global",
-           "details": {
-               "message": "Access denied with code 403 (phase 2). Operator GE matched 5 at TX:anomaly_score. ",
-               "data": "",
-               "file": "rules\/REQUEST-949-BLOCKING-EVALUATION.conf\\\"",
-               "line": "57"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
-   {
-       "resourceId": "/SUBSCRIPTIONS/A6F44B25-259E-4AF5-888A-386FED92C11B/RESOURCEGROUPS/DEMOWAF_V2/PROVIDERS/MICROSOFT.NETWORK/APPLICATIONGATEWAYS/DEMOWAF-V2",
-       "operationName": "ApplicationGatewayFirewall",
-       "category": "ApplicationGatewayFirewallLog",
-       "properties": {
-           "instanceId": "appgw_3",
-           "clientIp": "167.220.2.139",
-           "clientPort": "",
-           "requestUri": "\/",
-           "ruleSetType": "",
-           "ruleSetVersion": "",
-           "ruleId": "0",
-           "message": "Mandatory rule. Cannot be disabled. Inbound Anomaly Score Exceeded (Total Inbound Score: 8 - SQLI=5,XSS=0,RFI=0,LFI=0,RCE=0,PHPI=0,HTTP=0,SESS=0): SQL Injection Attack: SQL Tautology Detected.",
-           "action": "Blocked",
-           "site": "Global",
-           "details": {
-               "message": "Warning. Operator GE matched 5 at TX:inbound_anomaly_score. ",
-               "data": "",
-               "file": "rules\/RESPONSE-980-CORRELATION.conf\\\"",
-               "line": "73"
-           },
-           "hostname": "vm000003",
-           "transactionId": "AcAcAcAcAKH@AcAcAcAcAyAt"
-       }
-   }
```

Com o seu conhecimento de como as regras de CRS define o trabalho e que o conjunto de regras CRS 3.0 funciona com uma sistema de pontuação de anomalias (consulte [firewall do aplicativo Web para o Gateway de aplicativo do Azure](waf-overview.md)) você sabe que a parte inferior duas regras com o  **ação: Bloqueado** estão bloqueando a propriedade com base na pontuação de anomalias total. As regras para se concentrar em estão as principais dois.

A primeira entrada é registrada como o usuário usou um endereço IP numérico para navegar até o Gateway de aplicativo, que pode ser ignorado nesse caso.

O segundo um (regra 942130) é o interessante. Você pode ver nos detalhes do que ele correspondeu a um padrão (1 = 1) e o campo é chamado **text1**. Siga as mesmas etapas anteriores para excluir o **solicitar nome do atributo** que **é igual a** **1 = 1**.

## <a name="finding-request-header-names"></a>Nomes de cabeçalho de solicitação de localização

O Fiddler é uma ferramenta útil mais uma vez para localizar nomes de cabeçalho de solicitação. Na seguinte captura de tela, você pode ver os cabeçalhos para esta solicitação GET, que incluem *Content-Type*, *agente do usuário*e assim por diante.

![Fiddler](media/waf-tshoot/fiddler-2.png)

Outra maneira de exibir cabeçalhos de solicitação e resposta é examinar as ferramentas de desenvolvedor do Chrome. Você pode pressionar F12 ou clique com botão direito -> **Inspect** -> **ferramentas de desenvolvedor**e selecione o **rede** guia. Carregar uma página da web e clique na solicitação que você deseja inspecionar.

![Chrome F12](media/waf-overview/chrome-f12.png)

## <a name="finding-request-cookie-names"></a>Nomes de cookies de solicitação de localização

Se a solicitação contém os cookies, o **Cookies** guia pode ser selecionada para exibi-los no Fiddler.

## <a name="restrict-global-parameters-to-eliminate-false-positives"></a>Restringir parâmetros globais para eliminar falsos positivos

- Desabilitar a inspeção de corpo de solicitação

   Definindo **corpo da solicitação de inspecionar** desativado, os corpos de solicitação de todo o tráfego não serão avaliados pelo seu WAF. Isso pode ser útil se você souber que os corpos de solicitação não são mal-intencionados para seu aplicativo.

   Ao desabilitar essa opção, somente o corpo da solicitação não será inspecionado. Os cabeçalhos e cookies permanecem inspecionados, a menos que os itens individuais são excluídos usando a funcionalidade de lista de exclusão.

- Limites de tamanho de arquivo

   Limitando o tamanho do arquivo do seu WAF, você limitará a possibilidade de um ataque acontecendo com seus servidores web. Ao permitir que arquivos grandes a serem carregados, o risco de seu back-end que está sendo sobrecarregado aumenta. Limitar o tamanho do arquivo a um caso de uso normal para o seu aplicativo é apenas outra maneira de impedir ataques.

   > [!NOTE]
   > Se você souber que seu aplicativo nunca precisará carregar qualquer arquivo acima de um determinado tamanho, você pode restringir que definindo um limite.

## <a name="next-steps"></a>Próximas etapas

Ver [como configurar o firewall do aplicativo web no Gateway de aplicativo](tutorial-restrict-web-traffic-powershell.md).
