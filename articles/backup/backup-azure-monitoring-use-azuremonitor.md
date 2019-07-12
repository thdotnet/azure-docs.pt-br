---
title: 'Serviço de Backup do Azure: Monitorar o Backup do Azure com o Azure Monitor'
description: Monitorar cargas de trabalho de Backup do Azure e criar alertas personalizados usando o Azure Monitor.
services: backup
author: pvrk
manager: shivamg
keywords: Log Analytics; Backup do Azure; Alertas; Configurações de diagnóstico; Grupos de ação
ms.service: backup
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: pullabhk
ms.assetid: 01169af5-7eb0-4cb0-bbdb-c58ac71bf48b
ms.openlocfilehash: e2d4a235737789f2f5852c00218427613db3d558
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786323"
---
# <a name="monitor-at-scale-by-using-azure-monitor"></a>Monitorar em escala usando o Azure Monitor

Backup do Azure fornece [internos de monitoramento e recursos de alerta](backup-azure-monitoring-built-in-monitor.md) em um cofre de serviços de recuperação. Esses recursos estão disponíveis sem nenhuma infraestrutura de gerenciamento adicionais. Mas esse serviço interno é limitado nos seguintes cenários:

- Se você monitorar dados de vários cofres de serviços de recuperação entre assinaturas
- Se for o canal de notificação preferencial *não* email
- Se os usuários desejarem alertas para mais cenários
- Se você deseja exibir informações de um componente local, como o System Center Data Protection Manager no Azure, que o portal não mostra na [ **trabalhos de Backup** ](backup-azure-monitoring-built-in-monitor.md#backup-jobs-in-recovery-services-vault) ou [  **Alertas de backup**](backup-azure-monitoring-built-in-monitor.md#backup-alerts-in-recovery-services-vault)

## <a name="using-log-analytics-workspace"></a>Usando o espaço de trabalho do Log Analytics

> [!NOTE]
> Dados de backups de VM do Azure, o agente de Backup do Azure, System Center Data Protection Manager, os backups do SQL em VMs do Azure e backups de compartilhamento de arquivos do Azure é sejam enviados para o espaço de trabalho do Log Analytics por meio das configurações de diagnóstico. 

Para monitorar em escala, você precisa ter os recursos dos dois serviços do Azure. *Configurações de diagnóstico* enviar dados de vários recursos do Azure Resource Manager para outro recurso. *Log Analytics* gera alertas personalizados, onde você pode usar grupos de ação para definir outros canais de notificação. 

As seções a seguir detalham como usar o Log Analytics para monitorar o Backup do Azure em grande escala.

### <a name="configure-diagnostic-settings"></a>Definir as configurações de diagnóstico

Os recursos do Azure do Gerenciador de recursos, como o Cofre de serviços de recuperação, registram informações sobre as operações agendadas e disparadas por usuário operações como dados de diagnóstico. 

Na seção monitoramento, selecione **configurações de diagnóstico** e especifique o destino para do Cofre de serviços de recuperação dados de diagnóstico.

![Configuração de diagnóstico do Cofre de serviços de recuperação, direcionando o Log Analytics](media/backup-azure-monitoring-laworkspace/rs-vault-diagnostic-setting.png)

Você pode direcionar um espaço de trabalho do Log Analytics de outra assinatura. Para monitorar os cofres entre assinaturas em um único lugar, selecione o mesmo espaço de trabalho do Log Analytics para diversos cofres de serviços de recuperação. Para todas as informações relacionadas ao Backup do Azure para o espaço de trabalho do Log Analytics de canal, selecione **AzureBackupReport** como o log.

> [!IMPORTANT]
> Depois de concluir a configuração, você deve aguardar 24 horas para que o envio por push de dados iniciais concluir. Depois que a inicial push de dados, todos os eventos são enviados, conforme descrito posteriormente neste artigo, nos [seção frequência](#diagnostic-data-update-frequency).

### <a name="deploy-a-solution-to-the-log-analytics-workspace"></a>Implantar uma solução ao espaço de trabalho do Log Analytics

Depois que os dados estão dentro do espaço de trabalho do Log Analytics [implantar um modelo do GitHub](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) ao Log Analytics para visualizar os dados. Para identificar corretamente o espaço de trabalho, certifique-se de que fornecer o mesmo grupo de recursos, o nome do espaço de trabalho e o local de espaço de trabalho. Em seguida, instale esse modelo no espaço de trabalho.

> [!NOTE]
> Se você não tiver alertas, trabalhos de backup ou trabalhos de restauração no seu espaço de trabalho do Log Analytics, você poderá ver um código de erro "BadArgumentError" no portal. Ignorar esse erro e continuar a usar a solução. Depois que os dados relevantes de tipo é iniciado que fluem para o espaço de trabalho, as visualizações refletirá o mesmo, e você não verá o erro mais.

### <a name="view-azure-backup-data-by-using-log-analytics"></a>Exibir dados de Backup do Azure usando o Log Analytics

Depois que o modelo é implantado, a solução para o monitoramento de Backup do Azure será exibida na região de resumo de espaço de trabalho. Para ir para o resumo, siga um dos seguintes caminhos:

- **Azure Monitor**: No **Insights** seção, selecione **mais** e, em seguida, escolha o espaço de trabalho relevante.
- **Espaços de trabalho de análise de log**: Selecione o espaço de trabalho relevante e, em seguida, sob **gerais**, selecione **resumo de espaço de trabalho**.

![O bloco de monitoramento do Log Analytics](media/backup-azure-monitoring-laworkspace/la-azurebackup-azuremonitor-tile.png)

Quando você seleciona o bloco de monitoramento, o modelo de designer abre uma série de gráficos sobre dados de monitoramento básicos do Backup do Azure. Aqui estão alguns dos gráficos que você verá:

* Todos os trabalhos de backup

   ![Gráficos de análise de log para trabalhos de backup](media/backup-azure-monitoring-laworkspace/la-azurebackup-allbackupjobs.png)

* Trabalhos de restauração

   ![Gráfico de análise de log para trabalhos de restauração](media/backup-azure-monitoring-laworkspace/la-azurebackup-restorejobs.png)

* Alertas internos do Backup do Azure para recursos do Azure

   ![Gráfico de análise de log para alertas de Backup do Azure internas para recursos do Azure](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts.png)

* Alertas de Backup do Azure internas para recursos locais

   ![Gráfico de análise de log para alertas de Backup do Azure internas para recursos locais](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts-onprem.png)

* Fontes de dados do Active Directory

   ![Gráfico de análise de log para entidades de backup ativos](media/backup-azure-monitoring-laworkspace/la-azurebackup-activedatasources.png)

* Armazenamento em nuvem do Cofre de serviços de recuperação

   ![Gráfico de análise de log para armazenamento em nuvem cofre dos serviços de recuperação](media/backup-azure-monitoring-laworkspace/la-azurebackup-cloudstorage-in-gb.png)

Esses gráficos são fornecidos com o modelo. Você pode editar gráficos ou adicionar mais gráficos, se você precisar.

> [!IMPORTANT]
> Quando você implanta o modelo, você está criando essencialmente um bloqueio somente leitura. Para editar e salvar o modelo, você precisará remover o bloqueio. Você pode remover um bloqueio na **as configurações** seção do espaço de trabalho do Log Analytics, no **bloqueia** painel.

### <a name="create-alerts-by-using-log-analytics"></a>Criar alertas com o uso do Log Analytics

No Azure Monitor, você pode criar seus próprios alertas em um espaço de trabalho do Log Analytics. No espaço de trabalho, você deve usar *grupos de ação do Azure* para selecionar o mecanismo preferencial de notificação. 

> [!IMPORTANT]
> Para obter informações sobre o custo de criar essa consulta, consulte [preços do Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).

Selecione qualquer um dos gráficos para abrir o **Logs** seção do espaço de trabalho do Log Analytics. No **Logs** seção, editar as consultas e criar alertas neles.

![Criar um alerta em um espaço de trabalho do Log Analytics](media/backup-azure-monitoring-laworkspace/la-azurebackup-customalerts.png)

Quando você seleciona **nova regra de alerta**, abre a página de criação de alerta do Monitor do Azure, conforme mostrado na imagem a seguir. Aqui, o recurso já está marcado como o espaço de trabalho do Log Analytics e a integração do grupo de ação é fornecida.

![A página de criação de alerta do Log Analytics](media/backup-azure-monitoring-laworkspace/inkedla-azurebackup-createalert.jpg)

#### <a name="alert-condition"></a>Condição de alerta

A característica de definição de um alerta é sua condição de disparo. Selecione **condição** carrega automaticamente a consulta do Kusto sobre o **Logs** página conforme mostrado na imagem a seguir. Aqui você pode editar a condição de acordo com suas necessidades. Para obter mais informações, consulte [consultas de exemplo Kusto](#sample-kusto-queries).

![Como configurar uma condição de alerta](media/backup-azure-monitoring-laworkspace/la-azurebackup-alertlogic.png)

Se necessário, você pode editar a consulta do Kusto. Escolha um limite, o período e a frequência. O limite determina quando o alerta será gerado. O período é a janela de tempo em que a consulta é executada. Por exemplo, se o limite for maior que 0, o período é de 5 minutos e a frequência é de 5 minutos, em seguida, a regra executa a consulta a cada 5 minutos, revisando os 5 minutos anteriores. Se o número de resultados for maior que 0, você será notificado por meio do grupo de ação selecionada.

#### <a name="alert-action-groups"></a>Grupos de ação de alerta

Use um grupo de ação para especificar um canal de notificação. Para ver os mecanismos de notificação disponíveis, em **grupos de ação**, selecione **criar novo**.

![Mecanismos de notificação disponíveis na janela "Adicionar grupo de ações"](media/backup-azure-monitoring-laworkspace/LA-AzureBackup-ActionGroup.png)

Você pode atender a todos os alertas e monitoramento de requisitos do Log Analytics sozinho ou você pode usar o Log Analytics para complementar as notificações internos.

Para obter mais informações, consulte [criar, exibir e gerenciar alertas do log usando o Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) e [criar e gerenciar grupos de ações no portal do Azure](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups).

### <a name="sample-kusto-queries"></a>Exemplos de consulta Kusto

Os grafos padrão lhe oferece Kusto consultas para cenários básicos, na qual você pode criar alertas. Você também pode modificar as consultas para obter os dados que você deseja ser alertado sobre. Cole as seguinte Kusto consultas de exemplo o **Logs** página e, em seguida, criar alertas nas consultas:

* Todos os trabalhos de backup com êxito

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | where OperationName == "Job" and JobOperation_s == "Backup"
    | where JobStatus_s == "Completed"
    ````
    
* Todas as falhas de trabalhos de backup

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | where OperationName == "Job" and JobOperation_s == "Backup"
    | where JobStatus_s == "Failed"
    ````
    
* Todos os trabalhos de backup de VM do Azure bem-sucedida

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
    | where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
    | join kind=inner
    (
        AzureDiagnostics
        | where Category == "AzureBackupReport"
        | where OperationName == "BackupItem"
        | where SchemaVersion_s == "V2"
        | where BackupItemType_s == "VM" and BackupManagementType_s == "IaaSVM"
        | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
        | project BackupItemUniqueId_s , BackupItemFriendlyName_s
    )
    on BackupItemUniqueId_s
    | extend Vault= Resource
    | project-away Resource
    ````

* Todos os trabalhos com êxito SQL log backup

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
    | where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s == "Log"
    | join kind=inner
    (
        AzureDiagnostics
        | where Category == "AzureBackupReport"
        | where OperationName == "BackupItem"
        | where SchemaVersion_s == "V2"
        | where BackupItemType_s == "SQLDataBase" and BackupManagementType_s == "AzureWorkload"
        | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
        | project BackupItemUniqueId_s , BackupItemFriendlyName_s
    )
    on BackupItemUniqueId_s
    | extend Vault= Resource
    | project-away Resource
    ````

* Todos os trabalhos de agente de Backup do Azure com êxito

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
    | where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
    | join kind=inner
    (
        AzureDiagnostics
        | where Category == "AzureBackupReport"
        | where OperationName == "BackupItem"
        | where SchemaVersion_s == "V2"
        | where BackupItemType_s == "FileFolder" and BackupManagementType_s == "MAB"
        | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
        | project BackupItemUniqueId_s , BackupItemFriendlyName_s
    )
    on BackupItemUniqueId_s
    | extend Vault= Resource
    | project-away Resource
    ````

### <a name="diagnostic-data-update-frequency"></a>Frequência de atualização de dados de diagnóstico

Os dados de diagnóstico do cofre é sejam enviados para o espaço de trabalho do Log Analytics com algum atraso. Cada evento chega ao espaço de trabalho do Log Analytics *20 a 30 minutos* depois que ela é enviada por push do Cofre de serviços de recuperação. Aqui estão mais detalhes sobre a latência:

- Em todas as soluções, alertas de internos do serviço de backup são enviados por push assim que elas forem criadas. Portanto, eles normalmente são exibidos no espaço de trabalho do Log Analytics depois de 20 a 30 minutos.
- Em soluções de todos os trabalhos de backup ad hoc e trabalhos de restauração são enviados assim que eles *concluir*.
- Para todas as soluções, exceto o backup do SQL, trabalhos de backup agendados são enviados por push assim que eles *concluir*.
- Backup do SQL, como backups de log podem ocorrer a cada 15 minutos, informações para todos os concluídos trabalhos de backup agendados, incluindo logs, é em lote e enviadas por push a cada 6 horas.
- Em todas as soluções, outras informações como o item de backup, política, pontos de recuperação, armazenamento e assim por diante são enviadas pelo menos *uma vez por dia.*
- Uma alteração na configuração do backup (como alterar a política ou editar a política) dispara um envio por push todos relacionados de informações de backup.

## <a name="using-the-recovery-services-vaults-activity-logs"></a>Usando logs de atividade do Cofre de serviços de recuperação

> [!CAUTION]
> As etapas a seguir se aplicam apenas ao *backups de VM do Azure.* Você não pode usar estas etapas para soluções como o agente de Backup do Azure, os backups do SQL no Azure ou arquivos do Azure.

Você também pode usar os logs de atividade para obter notificações de eventos, como sucesso de backup. Para começar, siga estas etapas:

1. Entre no Portal do Azure.
1. Abra o Cofre de serviços de recuperação relevante. 
1. Nas propriedades do cofre, abra o **log de atividades** seção.

Para identificar o log adequado e criar um alerta:

1. Verifique se que você está recebendo logs de atividade para backups bem-sucedidos, aplicando os filtros mostrados na imagem a seguir. Alterar o **Timespan** valor conforme necessário para exibir os registros.

   ![Filtragem para localizar os logs de atividade para backups de VM do Azure](media/backup-azure-monitoring-laworkspace/activitylogs-azurebackup-vmbackups.png)

1. Selecione o nome da operação para ver os detalhes relevantes.
1. Selecione **nova regra de alerta** para abrir o **Create rule** página. 
1. Criar um alerta, seguindo as etapas em [criar, exibir e gerenciar alertas do log de atividades usando o Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log).

   ![Nova regra de alerta](media/backup-azure-monitoring-laworkspace/new-alert-rule.png)

Aqui, o recurso é o Cofre de serviços de recuperação em si. Você deve repetir as mesmas etapas para todos os cofres na qual você deseja ser notificado por meio de logs de atividade. A condição não terá um limite, o período ou a frequência porque este alerta é baseado em eventos. Assim que o log de atividades relevantes é gerado, o alerta é gerado.

## <a name="using-log-analytics-to-monitor-at-scale"></a>Usando o Log Analytics para monitorar em escala

Você pode exibir todos os alertas criados a partir de logs de atividades e os espaços de trabalho do Log Analytics no Azure Monitor. Basta abrir o **alertas** painel à esquerda.

Embora você possa obter notificações por meio de logs de atividade, é altamente recomendável usando o Log Analytics em vez de logs de atividades para monitorar em escala. Eis o porquê:

- **Limitado a cenários**: Notificações por meio de logs de atividade se aplicam apenas a backups de VM do Azure. As notificações devem ser configuradas para cada Cofre de serviços de recuperação.
- **Definição de ajustar**: A atividade de backup agendada não couber com a definição mais recente dos logs de atividades. Em vez disso, ele se alinhe com [logs de diagnóstico](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview#what-you-can-do-with-diagnostic-logs). Esse alinhamento causa efeitos inesperados, quando os dados que fluem por meio da atividade de log de alterações de canal.
- **Problemas com o canal de log de atividade**: Em cofres dos serviços de recuperação, os logs de atividade bombeadas de Backup do Azure seguem um modelo de novo. Infelizmente, essa alteração afeta a geração de logs de atividade no Azure governamental, Azure Alemanha e Azure China 21Vianet. Se os usuários desses serviços de nuvem criar ou configurar todos os alertas dos logs de atividades no Azure Monitor, os alertas não são disparados. Além disso, em todas as regiões públicas do Azure, se um usuário [coleta logs de atividade de serviços de recuperação em um espaço de trabalho do Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/collect-activity-logs), esses logs não são exibidos.

Use um espaço de trabalho do Log Analytics para monitoramento e alertas em grande escala para todas as suas cargas de trabalho que são protegidos pelo Backup do Azure.

## <a name="next-steps"></a>Próximas etapas

Para criar consultas personalizadas, consulte [modelo de dados do Log Analytics](backup-azure-log-analytics-data-model.md).
