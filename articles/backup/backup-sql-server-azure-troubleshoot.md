---
title: Solucionar problemas de backup de banco de dados do SQL Server usando o Backup do Azure | Microsoft Docs
description: Informações de solução de problemas para fazer backup de bancos de dados do SQL Server em execução em VMs do Azure com o Backup do Azure.
services: backup
author: anuragm
manager: sivan
ms.service: backup
ms.topic: article
ms.date: 06/18/2019
ms.author: anuragm
ms.openlocfilehash: b2822a3c7dfa23065f2cbd35ef4e506efae026f2
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704841"
---
# <a name="troubleshoot-sql-server-database-backup-by-using-azure-backup"></a>Solucionar problemas de backup de banco de dados do SQL Server usando o Backup do Azure

Este artigo fornece informações de solução de problemas para bancos de dados do SQL Server em execução em máquinas virtuais do Azure.

Para obter mais informações sobre o processo de backup e as limitações, consulte [backup do SQL Server em VMs do Azure](backup-azure-sql-database.md#feature-consideration-and-limitations).

## <a name="sql-server-permissions"></a>Permissões do SQL Server

Para configurar a proteção para um banco de dados do SQL Server em uma máquina virtual, você deve instalar o **AzureBackupWindowsWorkload** extensão na máquina virtual. Se você receber o erro **UserErrorSQLNoSysadminMembership**, significa que a instância do SQL Server não tem as permissões necessárias de backup. Para corrigir esse erro, siga as etapas em [permissões de VM do conjunto](backup-azure-sql-database.md#set-vm-permissions).

## <a name="error-messages"></a>Mensagens de erro

### <a name="backup-type-unsupported"></a>Tipo de Backup Sem Suporte

| Severidade | DESCRIÇÃO | Possíveis causas | Ação recomendada |
|---|---|---|---|
| Aviso | As configurações atuais para este banco de dados não oferecem suporte a determinados tipos de backup presentes na política associada. | <li>Apenas uma operação de backup de banco de dados completo pode ser executada no banco de dados mestre. Backup diferencial nem o backup de log de transações é possível. </li> <li>Qualquer banco de dados no modelo de recuperação simples não permite que o backup dos logs de transações.</li> | Modifica as configurações de banco de dados, de modo que todos os tipos na política de backup têm suporte. Ou então, altere a política atual para incluir somente os tipos de backup com suporte. Caso contrário, os tipos de backup sem suporte serão ignorados durante o backup agendado ou o trabalho de backup irá falhar para backup ad hoc.


### <a name="usererrorsqlpodoesnotsupportbackuptype"></a>UserErrorSQLPODoesNotSupportBackupType

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| Este banco de dados SQL não dá suporte para o tipo de backup solicitado. | Ocorre quando o modelo de recuperação de banco de dados não permite o tipo de backup solicitado. O erro pode ocorrer nas seguintes situações: <br/><ul><li>Um banco de dados que está usando um modelo de recuperação simples não permite o backup de log.</li><li>Backups diferenciais e de log não são permitidos para um banco de dados mestre.</li></ul>Para obter mais detalhes, consulte o [modelos de recuperação do SQL Server](https://docs.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server) documentação. | Se o backup de log falhar para o banco de dados no modelo de recuperação simples, tente uma destas opções:<ul><li>Se o banco de dados estiver no modo de recuperação simples, desabilite os backups de log.</li><li>Use o [documentação do SQL Server](https://docs.microsoft.com/sql/relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server) para alterar o banco de dados modelo de recuperação completa ou bulk-logged. </li><li> Se você não quiser alterar o modelo de recuperação e tiver uma política padrão para fazer backup de vários bancos de dados que não podem ser alterados, ignore o erro. Os backups diferenciais e completos funcionarão por agendamento. Os backups de log serão ignorados, o que é esperado neste caso.</li></ul>Se for um banco de dados mestre e você tiver configurado o diferencial ou backup de log, use as seguintes etapas:<ul><li>Use o portal para alterar o agendamento de política de backup do banco de dados mestre para completo.</li><li>Se você tiver uma política padrão para fazer backup de vários bancos de dados que não podem ser alterados, ignore o erro. O backup completo funcionará por agendamento. Backups de log ou diferenciais não ocorrerão, o que é esperado neste caso.</li></ul> |
| Operação cancelada como uma operação conflitante já em execução no mesmo banco de dados. | Consulte a [entrada de blog sobre as limitações de backup e restauração](https://blogs.msdn.microsoft.com/arvindsh/2008/12/30/concurrency-of-full-differential-and-log-backups-on-the-same-database) que são executadas simultaneamente.| [Usar o SQL Server Management Studio (SSMS) para monitorar os trabalhos de backup](manage-monitor-sql-database-backup.md). Depois que a operação conflitante falhar, reinicie a operação.|

### <a name="usererrorsqlpodoesnotexist"></a>UserErrorSQLPODoesNotExist

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| Banco de dados SQL não existe. | O banco de dados foi excluído ou renomeado. | Verifique se o banco de dados foi excluído ou renomeado acidentalmente.<br/><br/> Se o banco de dados foi excluído acidentalmente, para continuar os backups, restaure o banco de dados para o local original.<br/><br/> Se você excluiu o banco de dados e não precisa backups futuros, em seguida, no cofre de serviços de recuperação, selecione **parar backup** com **reter dados de Backup** ou **excluir dados de Backup**. Para obter mais informações, consulte [gerenciar e monitorar bancos de dados de SQL Server de backup](manage-monitor-sql-database-backup.md).

### <a name="usererrorsqllsnvalidationfailure"></a>UserErrorSQLLSNValidationFailure

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| A cadeia de logs está desfeita. | O banco de dados ou a VM é feita por meio de outra solução de backup, o que trunca a cadeia de logs.|<ul><li>Verifique se outra solução de backup ou script está em uso. Nesse caso, pare a outra solução de backup. </li><li>Se o backup foi um backup de log do ad-hoc, dispare um backup completo para iniciar uma nova cadeia de logs. Para backups de log agendados, nenhuma ação é necessária porque o serviço de Backup do Azure irá disparar automaticamente um backup completo para corrigir esse problema.</li>|

### <a name="usererroropeningsqlconnection"></a>UserErrorOpeningSQLConnection

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| O Backup do Azure não pode conectar a instância do SQL. | O Backup do Azure não pode se conectar à instância do SQL Server. | Use os detalhes adicionais no menu de erro no portal do Azure para restringir as causas raiz. Consulte [Solucionar problemas de backup do SQL](https://docs.microsoft.com/sql/database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine) para corrigir o erro.<br/><ul><li>Se as configurações padrão do SQL não permitem conexões remotas, altere as configurações. Consulte os seguintes artigos para obter informações sobre como alterar as configurações:<ul><li>[MSSQLSERVER_-1](/previous-versions/sql/sql-server-2016/bb326495(v=sql.130))</li><li>[MSSQLSERVER_2](/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error)</li><li>[MSSQLSERVER_53](/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error)</li></ul></li></ul><ul><li>Se houver problemas de logon, use estes links para corrigi-los:<ul><li>[MSSQLSERVER_18456](/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error)</li><li>[MSSQLSERVER_18452](/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error)</li></ul></li></ul> |

### <a name="usererrorparentfullbackupmissing"></a>UserErrorParentFullBackupMissing

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| O primeiro backup completo está faltando para essa fonte de dados. | O backup completo está faltando para o banco de dados. Log e backups diferenciais são pais para um backup completo, portanto, não se esqueça de fazer backups completos antes de disparar o diferencial ou backups de log. | Dispare um backup completo ad hoc.   |

### <a name="usererrorbackupfailedastransactionlogisfull"></a>UserErrorBackupFailedAsTransactionLogIsFull

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| Não é possível fazer backup como log de transações para a fonte de dados que está completo. | O espaço de log transacional do banco de dados está completo. | Para corrigir esse problema, consulte o [documentação do SQL Server](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-9002-database-engine-error). |

### <a name="usererrorcannotrestoreexistingdbwithoutforceoverwrite"></a>UserErrorCannotRestoreExistingDBWithoutForceOverwrite

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| Banco de dados com o mesmo nome já existe no local de destino | O destino de restauração de destino já tem um banco de dados com o mesmo nome.  | <ul><li>Altere o nome do banco de dados de destino.</li><li>Ou, use a opção de substituição de força na página de restauração.</li> |

### <a name="usererrorrestorefaileddatabasecannotbeofflined"></a>UserErrorRestoreFailedDatabaseCannotBeOfflined

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| A restauração falhou porque o banco de dados não pôde ficar offline. | Enquanto você estiver realizando uma restauração, o banco de dados de destino precisa ser colocado offline. O Backup do Azure não é possível colocar esses dados off-line. | Use os detalhes adicionais no menu de erro no portal do Azure para restringir as causas raiz. Para obter mais informações, consulte o [documentação do SQL Server](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). |

###  <a name="usererrorcannotfindservercertificatewiththumbprint"></a>UserErrorCannotFindServerCertificateWithThumbprint

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| Não é possível localizar o certificado do servidor com impressão digital no destino. | O banco de dados mestre na instância de destino não tem uma impressão digital de criptografia válido. | Importe a impressão digital do certificado usada na instância de origem para a instância de destino. |

### <a name="usererrorrestorenotpossiblebecauselogbackupcontainsbulkloggedchanges"></a>UserErrorRestoreNotPossibleBecauseLogBackupContainsBulkLoggedChanges

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| O backup de log usado para a recuperação contém alterações bulk-logged. Ele não pode ser usado para parar em um ponto arbitrário de acordo com as diretrizes do SQL. | Quando um banco de dados está no modo de recuperação bulk-logged, os dados entre uma transação bulk-logged e a próxima transação no log não podem ser recuperados. | Escolha um ponto diferente no tempo de recuperação. [Saiba mais](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms186229(v=sql.105)).


### <a name="fabricsvcbackuppreferencecheckfailedusererror"></a>FabricSvcBackupPreferenceCheckFailedUserError

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| A preferência de backup para o Grupo de Disponibilidade AlwaysOn do SQL não pode ser atendida, pois alguns nós do Grupo de Disponibilidade não estão registrados. | Os nós necessários para executar backups não estão registrados ou estão inacessíveis. | <ul><li>Certifique-se de que todos os nós necessários para executar backups deste banco de dados são registrados e íntegros e, em seguida, repita a operação.</li><li>Altere a preferência de backup para o grupo de disponibilidade Always On do SQL Server.</li></ul> |

### <a name="vmnotinrunningstateusererror"></a>VMNotInRunningStateUserError

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| A VM do SQL Server está desligada e inacessível para serviço de Backup do Azure. | A VM é desligada. | Certifique-se de que a instância do SQL Server está em execução. |

### <a name="guestagentstatusunavailableusererror"></a>GuestAgentStatusUnavailableUserError

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| O serviço de Backup do Azure usa agente convidado de VM do Azure para fazer backup, mas o agente convidado não está disponível no servidor de destino. | O agente convidado não está habilitado ou não está íntegro. | [Instalar o agente convidado de VM](../virtual-machines/extensions/agent-windows.md) manualmente. |

### <a name="autoprotectioncancelledornotvalid"></a>AutoProtectionCancelledOrNotValid

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| Intenção de proteção automática foi removida ou não é mais válida. | Quando você habilita a proteção automática em uma instância do SQL Server **configurar o Backup** trabalhos executados para todos os bancos de dados nessa instância. Se você desabilitar a proteção automática, enquanto os trabalhos estão em execução, os trabalhos **Em andamento** são cancelados com esse código de erro. | Habilite a proteção automática mais uma vez ajudar a proteger todos os bancos de dados restantes. |

## <a name="re-registration-failures"></a>Falhas de reinscrição

Verificar um ou mais dos seguintes sintomas antes de disparar a operação registrar novamente:

* Todas as operações (como backup, restaurar e configurar o backup) estão falhando na VM com um dos seguintes códigos de erro: **WorkloadExtensionNotReachable**, **UserErrorWorkloadExtensionNotInstalled**, **WorkloadExtensionNotPresent**, **WorkloadExtensionDidntDequeueMsg**.
* O **Status do Backup** área para o item de backup está mostrando **não pode ser acessado**. Descartar todas as outras causas que podem resultar no mesmo status:

  * Falta de permissão para executar operações relacionadas a backup na VM  
  * Desligamento da VM, portanto, os backups não podem ocorrer
  * Problemas de rede  

  ![Status "Não pode ser acessado" no novo registro de uma VM](./media/backup-azure-sql-database/re-register-vm.png)

* No caso de um grupo de disponibilidade Always On, os backups começaram a falhar depois que você alterou a preferência de backup ou após um failover.

Esses sintomas podem surgir para um ou mais dos seguintes motivos:

* Uma extensão foi excluída ou desinstalada do portal. 
* Uma extensão foi desinstalada na **painel de controle** na VM sob **desinstalar ou alterar um programa**.
* A VM foi restaurada de volta no tempo por meio da restauração de disco no local.
* A VM foi desligada por um longo período, portanto, a configuração da extensão nele expirou.
* A VM foi excluída e outra VM foi criada com o mesmo nome e no mesmo grupo de recursos da VM excluída.
* Um de nós do grupo de disponibilidade não recebeu a configuração do backup completa. Isso pode acontecer quando o grupo de disponibilidade é registrado no cofre, ou quando um novo nó é adicionado.

Os cenários anteriores, é recomendável que você disparar uma operação de registrar novamente na VM. Por enquanto, essa opção está disponível somente por meio do PowerShell.

## <a name="size-limit-for-files"></a>Limite de tamanho de arquivos

O tamanho total da cadeia de caracteres dos arquivos depende não apenas o número de arquivos, mas também em seus nomes e caminhos. Para cada arquivo de banco de dados, obter o nome de arquivo lógico e o caminho físico. Você pode usar esta consulta SQL:

```
SELECT mf.name AS LogicalName, Physical_Name AS Location FROM sys.master_files mf
               INNER JOIN sys.databases db ON db.database_id = mf.database_id
               WHERE db.name = N'<Database Name>'"
```

Agora, organizá-los no seguinte formato:

```
[{"path":"<Location>","logicalName":"<LogicalName>","isDir":false},{"path":"<Location>","logicalName":"<LogicalName>","isDir":false}]}
```

Veja um exemplo:

```
[{"path":"F:\\Data\\TestDB12.mdf","logicalName":"TestDB12","isDir":false},{"path":"F:\\Log\\TestDB12_log.ldf","logicalName":"TestDB12_log","isDir":false}]}
```

Se o tamanho da cadeia de caracteres do conteúdo excede 20.000 bytes, os arquivos de banco de dados são armazenados de forma diferente. Durante a recuperação, não será capaz de definir o caminho do arquivo de destino para restauração. Os arquivos serão restaurados para o caminho SQL padrão fornecido pelo SQL Server.

### <a name="override-the-default-target-restore-file-path"></a>Substituir o caminho de arquivo de restauração de destino padrão

Você pode substituir o caminho do arquivo de restauração de destino durante a operação de restauração, colocando um arquivo JSON que contém o mapeamento do arquivo de banco de dados para o caminho de restauração de destino. Criar uma `database_name.json` do arquivo e coloque-o no local *C:\Program Files \ carga de trabalho Backup\bin\plugins\SQL*.

O conteúdo do arquivo deve estar neste formato:
```
[
  {
    "Path": "<Restore_Path>",
    "LogicalName": "<LogicalName>",
    "IsDir": "false"
  },
  {
    "Path": "<Restore_Path>",
    "LogicalName": "LogicalName",
    "IsDir": "false"
  },  
]
```

Veja um exemplo:

```
[
  {
   "Path": "F:\\Data\\testdb2_1546408741449456.mdf",
   "LogicalName": "testdb7",
   "IsDir": "false"
  },
  {
    "Path": "F:\\Log\\testdb2_log_1546408741449456.ldf",
    "LogicalName": "testdb7_log",
    "IsDir": "false"
  },  
]
```

No conteúdo anterior, você pode obter o nome lógico do arquivo de banco de dados usando a seguinte consulta SQL:

```
SELECT mf.name AS LogicalName FROM sys.master_files mf
                INNER JOIN sys.databases db ON db.database_id = mf.database_id
                WHERE db.name = N'<Database Name>'"
  ```


Esse arquivo deve ser colocado antes de disparar a operação de restauração.

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre o Backup do Azure para VMs do SQL Server (visualização pública), consulte [Backup do Azure para VMs do SQL](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md#azbackup).
