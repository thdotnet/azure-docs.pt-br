---
title: Como solucionar problemas com o agente do Log Analytics para Windows | Microsoft Docs
description: Descreva os sintomas, causas e resolução dos problemas mais comuns com o agente do Log Analytics para Windows no Azure Monitor.
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
ms.date: 06/12/2019
ms.author: magoedte
ms.openlocfilehash: afa4483677336e9a887908a8cccf9590eed27af3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67120104"
---
# <a name="how-to-troubleshoot-issues-with-the-log-analytics-agent-for-windows"></a>Como solucionar problemas com o agente do Log Analytics para Windows 

Este artigo fornece ajuda para solucionar erros que podem ser encontrados com o agente do Log Analytics para Windows no Azure Monitor e sugere possíveis soluções para resolvê-los.

Se nenhuma dessas etapas funcionar para você, os seguintes canais de suporte também estarão disponíveis:

* Os clientes com benefícios de suporte Premier podem abrir uma solicitação de suporte com [Premier](https://premier.microsoft.com/).
* Os clientes com contratos de suporte do Azure podem abrir uma solicitação de suporte [no portal do Azure](https://manage.windowsazure.com/?getsupport=true).
* Visite a página de Comentários do Log Analytics para examinar as ideias e bugs enviados [https://aka.ms/opinsightsfeedback](https://aka.ms/opinsightsfeedback) ou para relatar um novo. 

## <a name="important-troubleshooting-sources"></a>Fontes de solução de problemas importantes

 Para ajudar a solucionar problemas relacionados ao agente de Log Analytics para Windows, o agente registra os eventos para o Log de eventos do Windows, especificamente em *aplicativos e Operations Manager Services\Operations*.  

## <a name="connectivity-issues"></a>Problemas de conectividade

Se o agente estiver se comunicando por meio de um servidor proxy ou firewall, pode haver restrições em vigor, impedindo a comunicação do computador de origem e o serviço do Azure Monitor. Se a comunicação é bloqueada, uma configuração incorreta, o registro com um espaço de trabalho pode falhar enquanto a tentativa de instalar o agente, configurar o pós-instalação de agente para relatar para um espaço de trabalho adicional ou comunicação do agente falha após um registro bem-sucedido. Esta seção descreve os métodos para solucionar esse tipo de problema com o agente do Windows. 

Verifique se o proxy ou firewall está configurado para permitir as seguintes portas e URLs descritas na tabela a seguir. Além disso, confirme inspeção de HTTP não está habilitada para o tráfego da web, como ele pode impedir que um canal seguro de TLS entre o agente e o Azure Monitor.  

|Recurso de agente|Portas |Direction |Ignorar a inspeção de HTTPS|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Porta 443 |Saída|Sim |  
|*.oms.opinsights.azure.com |Porta 443 |Saída|Sim |  
|*.blob.core.windows.net |Porta 443 |Saída|Sim |  
|*.azure-automation.net |Porta 443 |Saída|Sim |  

Para obter informações de firewall necessárias para o Azure governamental, consulte [gerenciamento do Azure governamental](../../azure-government/documentation-government-services-monitoringandmanagement.md#azure-monitor-logs). 

Há várias maneiras que você pode verificar se o agente está se comunicando com o Azure Monitor.

- Habilitar o [avaliação de integridade do agente do Azure Log Analytics](../insights/solution-agenthealth.md) no espaço de trabalho. Exibir o painel de integridade do agente, o **contagem de agentes sem resposta** coluna para ver rapidamente se o agente está listado.  

- Execute a consulta a seguir para confirmar se que o agente está enviando uma pulsação trabalho que está configurado para o relatório o espaço de trabalho. Substitua <ComputerName> com o nome real da máquina.

    ```
    Heartbeat 
    | where Computer like "<ComputerName>"
    | summarize arg_max(TimeGenerated, * ) by Computer 
    ```

    Se o computador está se comunicando com o serviço, a consulta deve retornar um resultado. Se a consulta não retornou um resultado, primeiro verifique se que o agente está configurado para relatar para o espaço de trabalho correto. Se ele está configurado corretamente, vá para a etapa 3 e pesquisar o Log de eventos do Windows para identificar se o agente está se conectando a que problema pode estar impedindo que ele se comunicando com o Azure Monitor.

- Outro método para identificar um problema de conectividade está executando o **TestCloudConnectivity** ferramenta. A ferramenta é instalada por padrão com o agente na pasta *%SystemRoot%\Program de Programas\Microsoft Monitoring agent\agent.* . Em um prompt de comando com privilégios elevados, navegue até a pasta e execute a ferramenta. A ferramenta retorna os resultados e destaques em que o teste falhou (por exemplo, se ele estava relacionado a uma porta/URL específica que foi bloqueada). 

    ![Resultados de execução da ferramenta de TestCloudConnection](./media/agent-windows-troubleshoot/output-testcloudconnection-tool-01.png)

- Filtro de *Operations Manager* log de eventos por **origens de eventos** - *módulos serviço de integridade*, *serviço de integridade*, e *Conector de serviço* e filtrar por **nível do evento** *aviso* e *erro* para confirmar se ele escreveu eventos a partir de tabela a seguir. Se elas forem, examine as etapas de resolução incluídas para cada evento possíveis.

    |ID do evento |source |DESCRIÇÃO |Resolução |
    |---------|-------|------------|-----------|
    |2133 & 2129 |Serviço de integridade |Falha de Conexão para o serviço do agente |Esse erro pode ocorrer quando o agente não pode se comunicar diretamente ou por meio de um servidor de firewall/proxy para o serviço do Azure Monitor. Verifique se as configurações de proxy do agente ou o firewall/proxy de rede permite o tráfego TCP do computador para o serviço.|
    |2138 |Módulos serviço de integridade |Proxy exige autenticação |Definir as configurações de proxy de agente e especifique o nome de usuário e a senha necessária para autenticar com o servidor proxy. |
    |2129 |Módulos serviço de integridade |Falha na conexão com negociação SSL falha |Verifique suas configurações de TCP/IP do adaptador de rede e configurações de proxy do agente.|
    |2127 |Módulos serviço de integridade |Falha ao enviar dados recebeu um código de erro |Se ele somente ocorre periodicamente durante o dia, é possível apenas uma anomalia aleatória que pode ser ignorada. Monitor para entender a frequência com que ele acontece. Se isso acontecer com frequência ao longo do dia, primeiro verifique sua configuração de rede e as configurações de proxy. Se a descrição inclui o código de erro HTTP 404 e é a primeira vez em que o agente tenta enviar dados para o serviço, ele incluirá um erro 500 com um código de 404 erro interno. 404 significa não encontrado, o que indica que a área de armazenamento para o novo espaço de trabalho ainda está sendo provisionada. Na próxima tentativa, dados irá escrever com êxito ao espaço de trabalho conforme o esperado. Um erro HTTP 403 pode indicar um problema de credenciais ou a permissão. Há mais informações, incluídas com o erro 403 para ajudar a solucionar o problema.|
    |4000 |Conector de serviço |Falha na resolução do nome DNS |A máquina não foi possível resolver o endereço de Internet usado ao enviar dados para o serviço. Isso pode ter configurações de resolvedor DNS em seu computador, configurações de proxy incorretas ou talvez um problema temporário de DNS com seu provedor. Se ele ocorre periodicamente, ele pode ser causado por um problema transitório relacionados à rede.|
    |4001 |Conector de serviço |Falha na Conexão ao serviço. |Esse erro pode ocorrer quando o agente não pode se comunicar diretamente ou por meio de um servidor de firewall/proxy para o serviço do Azure Monitor. Verifique se as configurações de proxy do agente ou o firewall/proxy de rede permite o tráfego TCP do computador para o serviço.|
    |4002 |Conector de serviço |O serviço retornou o código de status HTTP 403 em resposta a uma consulta. Verifique com o administrador de serviço para a integridade do serviço. A consulta será repetida mais tarde. |Esse erro é gravado durante a fase de registro inicial do agente e você verá uma URL semelhante à seguinte: *https://<workspaceID>.oms.opinsights.azure.com/AgentService.svc/AgentTopologyRequest*. Um erro 403 significa proibida de código e pode ser causado por uma ID do espaço de trabalho de digitação ou a chave, ou a data e hora está incorreta no computador. Se a hora for +/-15 minutos da hora atual, a integração falhará. Para corrigir isso, atualize a data e/ou o fuso horário do seu computador Windows.|

## <a name="data-collection-issues"></a>Problemas de coleta de dados

Depois que o agente está instalado e relatórios para seu espaço de trabalho configurado ou espaços de trabalho, ele pode parar recebendo a configuração, coleta ou encaminhamento de desempenho, logs ou outros dados para o serviço, dependendo do que está habilitado e direcionamento no computador. É necessário determinar se:

- É um tipo de dados específico ou todos os dados que não está disponíveis no espaço de trabalho?
- O tipo de dados especificado por uma solução ou especificado como parte da configuração de coleta de dados do espaço de trabalho?
- Quantos computadores foram afetados? É um único ou vários computadores relatando para o espaço de trabalho?
- Estava funcionando e foi interrompido em um determinado momento do dia ou ele nunca foi coletado? 
- A consulta de pesquisa de log que você está usando está sintaticamente correta? 
- O agente nunca recebeu sua configuração do Azure Monitor?

É a primeira etapa na solução de problemas determinar se o computador está enviando um evento de pulsação.

```
Heartbeat 
    | where Computer like "<ComputerName>"
    | summarize arg_max(TimeGenerated, * ) by Computer
```

Se a consulta retorna resultados, você precisa determinar se um determinado tipo de dados não é coletado e encaminhado para o serviço. Isso pode ser causado pelo agente de não receber a configuração atualizada do serviço ou algum outra sintoma impedindo que o agente opere normalmente. Execute as seguintes etapas para solucionar.

1. Abra um prompt de comando com privilégios elevados no computador e reinicie o serviço de agente digitando `net stop healthservice && net start healthservice`.
2. Abra o *Operations Manager* log de eventos e procure **IDs de eventos** *7023, 7024, 7025, 7028* e *1210* de **eventos código-fonte** *HealthService*.  Esses eventos indicam que o agente está recebendo a configuração do Azure Monitor com êxito e que estão monitorando ativamente o computador. A descrição do evento ID 1210 também especificará na última linha a todas as soluções e informações que são incluídas no escopo de monitoramento no agente.  

    ![Descrição do evento ID 1210](./media/agent-windows-troubleshoot/event-id-1210-healthservice-01.png)

3. Se após alguns minutos você não vir os dados esperados em resultados de consulta ou visualização, dependendo se você está exibindo os dados de uma solução ou um Insight, do *Operations Manager* log de eventos, procure **evento fontes** *HealthService* e *módulos serviço de integridade* e filtrar por **nível de evento** *aviso* e *Erro* para confirmar se ele escreveu eventos da tabela a seguir.

    |ID do evento |source |DESCRIÇÃO |Resolução |
    |---------|-------|------------|
    |8000 |HealthService |Esse evento especificará se um fluxo de trabalho está relacionado ao desempenho, evento ou outro tipo de dados coletados não é possível encaminhar o serviço de ingestão para o espaço de trabalho. | ID do evento 2136 de fonte de serviço de integridade é escrito junto com esse evento e pode indicar que o agente é capaz de se comunicar com o serviço, possivelmente devido a configuração incorreta das configurações de proxy e autenticação, interrupção da rede ou o firewall de rede / proxy não permite o tráfego TCP do computador para o serviço.| 
    |10102 e 10103 |Módulos serviço de integridade |Fluxo de trabalho não foi possível resolver a fonte de dados. |Isso pode ocorrer se o contador de desempenho especificado ou a instância não existe no computador ou está incorretamente definida nas configurações de dados do espaço de trabalho. Quando se trata de um usuário especificado [contador de desempenho](data-sources-performance-counters.md#configuring-performance-counters), verifique se as informações especificadas é seguir o formato correto e se existe nos computadores de destino. |
    |26002 |Módulos serviço de integridade |Fluxo de trabalho não foi possível resolver a fonte de dados. |Isso pode ocorrer se o log de eventos do Windows especificado não existe no computador. Esse erro pode ser ignorado se o computador não deve ter esse log de eventos registrado, caso contrário, quando se trata de um usuário especificado [log de eventos](data-sources-windows-events.md#configuring-windows-event-logs), verifique se as informações especificadas estão corretas. |

