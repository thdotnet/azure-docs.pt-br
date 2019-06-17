---
title: Solução de problemas de backup de banco de dados do SQL Server com o Backup do Azure| Microsoft Docs
description: Informações de solução de problemas para fazer backup de bancos de dados do SQL Server em execução em VMs do Azure com o Backup do Azure.
services: backup
author: anuragm
manager: shivamg
ms.service: backup
ms.topic: article
ms.date: 05/27/2019
ms.author: anuragm
ms.openlocfilehash: 8459bb451c4ff462ee816b986cafdbf776603917
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66306962"
---
# <a name="troubleshoot-back-up-sql-server-on-azure"></a>Solucionar problemas de backup do SQL Server no Azure

Este artigo fornece informações de solução de problemas para proteger VMs do SQL Server no Azure.

## <a name="feature-consideration-and-limitations"></a>Considerações e limitações de recurso

Para exibir a consideração de recurso, consulte o artigo [backup do SQL Server em VMs do Azure](backup-azure-sql-database.md#feature-consideration-and-limitations).

## <a name="sql-server-permissions"></a>Permissões do SQL Server

Para configurar a proteção de um banco de dados do SQL Server em uma máquina virtual, a extensão **AzureBackupWindowsWorkload** deverá estar instalada nessa máquina virtual. Se você receber o erro, **UserErrorSQLNoSysadminMembership**, isso significa que a Instância do SQL não possui as permissões de backup necessárias. Para corrigir esse erro, siga as etapas em [Definir permissões para VMs do SQL que não são do marketplace](backup-azure-sql-database.md#fix-sql-sysadmin-permissions).


## <a name="backup-type-unsupported"></a>Tipo de Backup Sem Suporte

| Severity | DESCRIÇÃO | Possíveis causas | Ação recomendada |
|---|---|---|---|
| Aviso | As configurações atuais para este banco de dados não oferecer suporte a determinado tipo de tipos de backup presentes na política associada. | <li>**BD mestre**: Apenas uma operação de backup de banco de dados completo pode ser executada no banco de dados mestre; nem **diferencial** backup nem transação **logs** backup é possível. </li> <li>Nenhum banco de dados no **modelo de recuperação simples** permite que backups de **logs** de transação sejam executados.</li> | Modifica as configurações de banco de dados, de modo que todos os tipos na política de backup têm suporte. Como alternativa, altere a política atual para incluir somente os tipos de backup com suporte. Caso contrário, os tipos de backup sem suporte serão ignorados durante o backup agendado ou o trabalho de backup irá falhar para backup ad hoc.


## <a name="usererrorsqlpodoesnotsupportbackuptype"></a>UserErrorSQLPODoesNotSupportBackupType

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| Este banco de dados SQL não dá suporte para o tipo de backup solicitado. | Ocorre quando o modelo de recuperação de banco de dados não permite o tipo de backup solicitado. O erro pode ocorrer nas seguintes situações: <br/><ul><li>Um banco de dados usando um modelo de recuperação simples não permite backup de log.</li><li>Não são permitidos backups de log e diferenciais para um banco de dados mestre.</li></ul>Para mais detalhes, consulte a documentação [Modelos de recuperação do SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server). | Se o backup de log falhar para o BD no modelo de recuperação simples, tente uma destas opções:<ul><li>Se o banco de dados estiver no modo de recuperação simples, desabilite os backups de log.</li><li>Use a [documentação do SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server) para alterar o modelo de recuperação do banco de dados para Full ou Bulk Logged. </li><li> Se você não quiser alterar o modelo de recuperação e tiver uma política padrão para fazer backup de vários bancos de dados que não podem ser alterados, ignore o erro. Os backups diferenciais e completos funcionarão por agendamento. Os backups de log serão ignorados, o que é esperado neste caso.</li></ul>Se for um banco de dados mestre e você tiver configurado o backup de log ou diferencial, use uma das etapas a seguir:<ul><li>Use o portal para alterar o agendamento da política de backup do banco de dados mestre para Full.</li><li>Se você tiver uma política padrão para fazer backup de vários bancos de dados que não podem ser alterados, ignore o erro. O backup completo funcionará por agendamento. Backups de log ou diferenciais não ocorrerão, o que é esperado neste caso.</li></ul> |
| Operação cancelada como uma operação conflitante já em execução no mesmo banco de dados. | Consulte a [entrada de blog sobre backup e restauração de limitações](https://blogs.msdn.microsoft.com/arvindsh/2008/12/30/concurrency-of-full-differential-and-log-backups-on-the-same-database) que executam simultaneamente.| [Use o SSMS (SQL Server Management Studio) para monitorar os trabalhos de backup.](manage-monitor-sql-database-backup.md) Depois que a operação conflitante falhar, reinicie a operação.|

## <a name="usererrorsqlpodoesnotexist"></a>UserErrorSQLPODoesNotExist

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| Banco de dados SQL não existe. | O banco de dados foi excluído ou renomeado. | Verifique se o banco de dados foi excluído ou renomeado acidentalmente.<br/><br/> Se o banco de dados foi excluído acidentalmente, para continuar os backups, restaure o banco de dados para o local original.<br/><br/> Se você excluiu o banco de dados e não precisa de backups futuros, no cofre dos Serviços de Recuperação, clique em [parar backup com "Excluir/Manter dados"](manage-monitor-sql-database-backup.md).

## <a name="usererrorsqllsnvalidationfailure"></a>UserErrorSQLLSNValidationFailure

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| A cadeia de logs está desfeita. | O banco de dados ou a VM é submetido a backup usando outra solução de backup, que trunca a cadeia de logs.|<ul><li>Verifique se outra solução de backup ou script está em uso. Nesse caso, pare a outra solução de backup. </li><li>Se o backup foi um backup de log do ad-hoc, dispare um backup completo para iniciar uma nova cadeia de logs. Para backups de log agendados, nenhuma ação será necessária, pois o serviço de Backup do Azure acionará automaticamente um backup completo para corrigir esse problema.</li>|

## <a name="usererroropeningsqlconnection"></a>UserErrorOpeningSQLConnection

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| O Backup do Azure não pode conectar a instância do SQL. | O Backup do Azure não pode conectar a Instância do SQL. | Use os detalhes adicionais no menu de erros do portal do Azure para restringir as causas raízes. Consulte [Solucionar problemas de backup do SQL](https://docs.microsoft.com/sql/database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine) para corrigir o erro.<br/><ul><li>Se as configurações padrão do SQL não permitirem conexões remotas, altere as configurações. Consulte os seguintes artigos para obter informações sobre como alterar as configurações.<ul><li>[MSSQLSERVER_-1](/previous-versions/sql/sql-server-2016/bb326495(v=sql.130))</li><li>[MSSQLSERVER_2](/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error)</li><li>[MSSQLSERVER_53](/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error)</li></ul></li></ul><ul><li>Se houver problemas de logon, consulte os links abaixo para corrigi-lo:<ul><li>[MSSQLSERVER_18456](/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error)</li><li>[MSSQLSERVER_18452](/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error)</li></ul></li></ul> |

## <a name="usererrorparentfullbackupmissing"></a>UserErrorParentFullBackupMissing

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| O primeiro backup completo está faltando para essa fonte de dados. | O backup completo está faltando para o banco de dados. Backups de log e diferenciais fazem parte de um backup completo, portanto, backups completos devem ser feitos antes de disparar backups de log ou diferenciais. | Dispare um backup completo ad hoc.   |

## <a name="usererrorbackupfailedastransactionlogisfull"></a>UserErrorBackupFailedAsTransactionLogIsFull

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| Não é possível fazer backup como log de transações para a fonte de dados que está completo. | O espaço de log transacional do banco de dados está completo. | Para corrigir esse problema, consulte a [documentação do SQL](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-9002-database-engine-error). |

## <a name="usererrorcannotrestoreexistingdbwithoutforceoverwrite"></a>UserErrorCannotRestoreExistingDBWithoutForceOverwrite

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| Banco de dados com o mesmo nome já existe no local de destino | O destino da restauração de destino já possui um banco de dados com o mesmo nome.  | <ul><li>Alterar o nome do banco de dados de destino</li><li>Ou use a opção forçar substituição na página de restauração</li> |

## <a name="usererrorrestorefaileddatabasecannotbeofflined"></a>UserErrorRestoreFailedDatabaseCannotBeOfflined

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| A restauração falhou porque o banco de dados não pôde ficar offline. | Ao fazer uma restauração, o banco de dados de destino precisará ficar offline. O Backup do Azure não pode colocar esses dados offline. | Use os detalhes adicionais no menu de erros do portal do Azure para restringir as causas raízes. Para obter mais informações, consulte a [documentação do SQL](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). |

##  <a name="usererrorcannotfindservercertificatewiththumbprint"></a>UserErrorCannotFindServerCertificateWithThumbprint

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| Não é possível localizar o certificado do servidor com impressão digital no destino. | O banco de dados Mestre na instância de destino não possui uma impressão digital de criptografia válida. | Importe a impressão digital válida do certificado usada na instância de origem para a instância de destino. |

## <a name="usererrorrestorenotpossiblebecauselogbackupcontainsbulkloggedchanges"></a>UserErrorRestoreNotPossibleBecauseLogBackupContainsBulkLoggedChanges

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| O backup de log usado para a recuperação contém alterações bulk-logged. Ele não pode ser usado para parar em um ponto arbitrário de acordo com as diretrizes do SQL. | Quando um banco de dados está no modo de recuperação registradas em massa, os dados entre uma transação de bulk-logged e a próxima transação de log não podem ser recuperados. | Escolha um ponto diferente no tempo de recuperação. [Saiba mais](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms186229(v=sql.105))


## <a name="fabricsvcbackuppreferencecheckfailedusererror"></a>FabricSvcBackupPreferenceCheckFailedUserError

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| A preferência de backup para o Grupo de Disponibilidade AlwaysOn do SQL não pode ser atendida, pois alguns nós do Grupo de Disponibilidade não estão registrados. | Os nós necessários para executar backups não estão registrados ou estão inacessíveis. | <ul><li>Certifique-se de que todos os nós necessários para executar backups desse banco de dados estejam registrados e íntegros e, em seguida, repita a operação.</li><li>Altere a preferência de backup do Grupo de Disponibilidade AlwaysOn do SQL.</li></ul> |

## <a name="vmnotinrunningstateusererror"></a>VMNotInRunningStateUserError

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| A VM do SQL Server está desligada e inacessível para serviço de Backup do Azure. | VM está desligada | Assegure-se de que o SQL Server está em execução. |

## <a name="guestagentstatusunavailableusererror"></a>GuestAgentStatusUnavailableUserError

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| O serviço de Backup do Azure usa agente convidado de VM do Azure para fazer backup, mas o agente convidado não está disponível no servidor de destino. | O agente convidado não está habilitado ou não está íntegro | [Instalar o agente convidado de VM](../virtual-machines/extensions/agent-windows.md) manualmente. |

## <a name="autoprotectioncancelledornotvalid"></a>AutoProtectionCancelledOrNotValid

| Mensagem de erro | Possíveis causas | Ação recomendada |
|---|---|---|
| Intenção de proteção automática foi removida ou não é mais válida. | Quando você habilita a proteção automática em uma instância do SQL, os trabalhos de **Configurar Backup** são executados para todos os bancos de dados nessa instância. Se você desabilitar a proteção automática, enquanto os trabalhos estão em execução, os trabalhos **Em andamento** são cancelados com esse código de erro. | Habilite a proteção automática mais uma vez para proteger todos os bancos de dados restantes. |

## <a name="re-registration-failures"></a>Falhas de reinscrição

Seleção de um ou mais dos [sintomas](#symptoms) antes de disparar a operação registrar novamente.

### <a name="symptoms"></a>Sintomas

* Todas as operações como backup, restaurar e configurar o backup estão falhando na VM com um dos seguintes códigos de erro: **WorkloadExtensionNotReachable**, **UserErrorWorkloadExtensionNotInstalled**, **WorkloadExtensionNotPresent**, **WorkloadExtensionDidntDequeueMsg**
* O **Status de Backup** para o Backup do item está mostrando **não pode ser acessado**. Embora você deve descartar todos os outros motivos que podem resultar no mesmo status:

  * Falta de permissão para executar operações de backup relacionadas na VM  
  * VM foi desligada devido a qual os backups não podem ocorrer
  * Problemas de rede  

    ![Registrar novamente a VM](./media/backup-azure-sql-database/re-register-vm.png)

* No caso do grupo de disponibilidade always on, os backups começaram a falhar depois que você alterou a preferência de backup ou quando houve um failover

### <a name="causes"></a>Causas
Esses sintomas podem surgir devido a um ou mais dos seguintes motivos:

  * Extensão foi excluída ou desinstalada do portal 
  * Extensão foi desinstalada do **painel de controle** da VM sob **desinstalar ou alterar um programa** interface do usuário
  * VM foi restaurada de volta no tempo usando a restauração de discos no local
  * VM foi desligada por um longo período devido ao qual a configuração da extensão nele expirou
  * VM foi excluída e outra VM foi criada com o mesmo nome e no mesmo grupo de recursos da VM excluída
  * Um de nós do AG não recebeu a configuração de backup completa, isso pode ocorrer no momento do registro de grupo de disponibilidade para o cofre ou quando um novo nó é adicionado  <br>
    Nos cenários acima, é recomendável para disparar a operação registrar novamente na VM. Essa opção só está disponível por meio do PowerShell e em breve estará disponível no portal do Azure.

## <a name="files-size-limit-beyond-which-restore-happens-to-default-path"></a>Limite de tamanho do arquivo além do qual restaurar acontece com o caminho padrão

O tamanho total da cadeia de caracteres de arquivos depende não só no número de arquivos, mas também em seus nomes e caminhos. Para cada um dos arquivos de banco de dados, obter o nome de arquivo lógico e o caminho físico.
Você pode usar a consulta SQL abaixo:

  ```
  SELECT mf.name AS LogicalName, Physical_Name AS Location FROM sys.master_files mf
                 INNER JOIN sys.databases db ON db.database_id = mf.database_id
                 WHERE db.name = N'<Database Name>'"
 ```

Agora, organizá-los no formato abaixo:

  ```[{"path":"<Location>","logicalName":"<LogicalName>","isDir":false},{"path":"<Location>","logicalName":"<LogicalName>","isDir":false}]}
  ```

Exemplo:

  ```[{"path":"F:\\Data\\TestDB12.mdf","logicalName":"TestDB12","isDir":false},{"path":"F:\\Log\\TestDB12_log.ldf","logicalName":"TestDB12_log","isDir":false}]}
  ```

Se o tamanho da cadeia de caracteres do conteúdo fornecido acima exceder 20.000 bytes, em seguida, os arquivos de banco de dados são armazenados diferente e durante a recuperação não será possível definir o caminho do arquivo de destino para restauração. Os arquivos serão restaurados para o caminho de SQL padrão fornecido pelo SQL Server.

### <a name="override-the-default-target-restore-file-path"></a>Substituir o caminho de arquivo de restauração de destino padrão

Você pode substituir o caminho do arquivo de restauração de destino durante a operação de restauração, colocando um arquivo JSON que contém o mapeamento do arquivo de banco de dados para o caminho de restauração de destino. Para isso, crie um arquivo `database_name.json` e coloque-o no local *C:\Program Files \ carga de trabalho Backup\bin\plugins\SQL*.

O conteúdo do arquivo deve estar no formato abaixo:
  ```[
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

Exemplo:

  ```[
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

 
O conteúdo acima, você pode obter o nome lógico do arquivo de banco de dados usando a consulta SQL abaixo:

```
SELECT mf.name AS LogicalName FROM sys.master_files mf
                INNER JOIN sys.databases db ON db.database_id = mf.database_id
                WHERE db.name = N'<Database Name>'"
  ```


Esse arquivo deve ser colocado antes de disparar a operação de restauração.

## <a name="next-steps"></a>Próximas etapas

Para obter mais informações sobre Backup do Azure para VMs do SQL Server (visualização pública), consulte [Backup do Azure para VMs do SQL (visualização pública)](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md#azbackup).
