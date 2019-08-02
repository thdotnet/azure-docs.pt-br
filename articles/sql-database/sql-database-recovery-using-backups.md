---
title: Restaurar um banco de dados SQL do Azure de um backup | Microsoft Docs
description: Saiba mais sobre a Restauração Pontual, que permite que você reverta um Banco de Dados SQL do Azure para um ponto anterior no tempo (até 35 dias).
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 04/30/2019
ms.openlocfilehash: 55d60ec332515fcfa3deb565a4a770027681537a
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566978"
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Recuperar um banco de dados SQL do Azure usando backups de banco de dados automatizados

Por padrão, os backups do Banco de Dados SQL são armazenados no armazenamento de blobs com replicação geográfica (RA-GRS). As opções a seguir estão disponíveis para recuperação do banco de dados usando [backups de banco de dados automatizados](sql-database-automated-backups.md):

- Crie um novo banco de dados no mesmo servidor do Banco de Dados SQL recuperado para um ponto especificado no tempo dentro do período de retenção.
- Crie um banco de dados no mesmo servidor do Banco de Dados SQL recuperado para o tempo de exclusão de um banco de dados excluído.
- Crie um novo banco de dados em qualquer servidor do Banco de Dados SQL na mesma região recuperado até o ponto dos backups mais recentes.
- Crie um novo banco de dados em qualquer servidor do Banco de Dados SQL em qualquer outra região recuperada até o ponto dos backups replicados mais recentes.

Se você configurou o [backup de retenção de longo prazo](sql-database-long-term-retention.md), também poderá criar um novo banco de dados a partir de qualquer backup EPD em qualquer servidor de banco de dados SQL.

> [!IMPORTANT]
> Não é possível substituir um banco de dados existente durante a restauração.

Ao usar a camada de serviço Standard ou Premium, um banco de dados restaurado incorre em um custo de armazenamento adicional nas seguintes condições:

- Restauração de P11 – P15 para S4 ou S12 ou P1– P6 se o tamanho máximo do banco de dados for superior a 500 GB.
- Restauração de P1–P6 para S4-S12 se o tamanho máximo do banco de dados for superior a 250 GB.

O custo extra é icurred quando o tamanho máximo do banco de dados restaurado é maior do que a quantidade de armazenamento incluída na camada de serviço e no nível de desempenho do banco de dados de destino. O armazenamento extra provisionado acima do valor incluído é cobrado como extra. Para obter detalhes de preço do armazenamento extra, confira a página [Preços do Banco de Dados SQL](https://azure.microsoft.com/pricing/details/sql-database/). Se a quantidade real de espaço usado for menor que a quantidade de armazenamento incluída, você poderá evitar esse custo extra definindo o tamanho máximo do banco de dados para o valor incluído.

> [!NOTE]
> [Backups de banco de dados automatizados](sql-database-automated-backups.md) são usadas quando você cria uma [cópia de banco de dados](sql-database-copy.md).

## <a name="recovery-time"></a>Tempo de recuperação

O tempo de recuperação para restaurar um banco de dados usando backups de banco de dados automatizado é afetado por vários fatores:

- O tamanho do banco de dados
- O tamanho da computação do banco de dados
- O número de logs de transações envolvidos
- A quantidade de atividade que precisa ser repetida para recuperar até o ponto de restauração
- A largura de banda de rede se a restauração for para uma região diferente
- O número de solicitações de restauração simultâneas sendo processadas na região de destino

Para um banco de dados grande e/ou muito ativo, a restauração pode levar várias horas. Caso haja uma interrupção prolongada em uma região, é possível que haja muitas solicitações de restauração geográfica sendo processadas por outras regiões. Quando houver muitas solicitações, o tempo de recuperação dos bancos de dados nessa região poderá aumentar. A maioria das restaurações de banco de dados é concluída em menos de 12 horas.

Para uma única assinatura, há limitações no número de solicitações de restauração simultâneas.  Essas limitações se aplicam a qualquer combinação de restaurações point-in-time, restaurações geográficas e restaurações do backup de retenção de longo prazo):

| | **Número máximo de solicitações simultâneas que estão sendo processadas** | **Número máximo de solicitações simultâneas que estão sendo enviadas** |
| :--- | --: | --: |
|Banco de dados único (por assinatura)|10|60|
|Pool Elástico (por pool)|4|200|
||||

Atualmente, não há um método interno para restaurar todo o servidor. O [Banco de Dados SQL do Azure: O script de](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) recuperação de servidor completo é um exemplo de como você pode realizar essa tarefa.

> [!IMPORTANT]
> Para recuperar usando backups automatizados, você deverá ser um membro da função Colaborador do SQL Server na assinatura ou o proprietário da assinatura. Confira [RBAC: funções internas](../role-based-access-control/built-in-roles.md). Você pode recuperar usando o Portal do Azure, o PowerShell ou a API REST. Você não pode usar o Transact-SQL.

## <a name="point-in-time-restore"></a>Restauração pontual

Você pode restaurar um banco de dados autônomo, em pool ou de instância para um ponto anterior no tempo usando o portal do Azure, o [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase)ou a [API REST](https://docs.microsoft.com/rest/api/sql/databases). A solicitação pode especificar qualquer camada de serviço ou tamanho de computação para o banco de dados restaurado. Assegure-se de ter recursos suficientes no servidor para o qual você está restaurando o banco de dados. Depois de concluído, um novo banco de dados será criado no mesmo servidor que o banco de dados original. O banco de dados restaurado será cobrado com taxas normais com base em sua camada de serviço e tamanho de computação. Você não incorrerá encargos até que a restauração do banco de dados seja concluída.

Um banco de dados geralmente é restaurado para um ponto anterior para fins de recuperação. Você pode tratar o banco de dados restaurado como uma substituição para o banco de dados original ou usá-lo como um dado de origem para atualizar o banco original.

- **Substituição de banco de dados**

  Se o banco de dados restaurado for destinado a uma substituição para o banco de dados original, você deverá especificar o tamanho de computação e a camada de serviço do banco de dados original. Em seguida, você pode renomear o banco de dados original e dar ao banco de dados restaurado o nome original usando o comando [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) no T-SQL.

- **Recuperação de dados**

  Se você planeja recuperar dados do banco de dado restaurado para se recuperar de um erro de usuário ou aplicativo, você precisa gravar e executar um script de recuperação de dados que extrai dados do banco de dado restaurado e aplica-se ao banco de dado original. Embora a operação de restauração possa demorar muito para concluir, o banco de dados em restauração é visível na lista de banco de dados por todo o processo de restauração. Se você excluir o banco de dados durante a restauração, a operação de restauração será cancelada e você não será cobrado pelo banco de dados que não concluiu a restauração.

Para recuperar um banco de dados único, em pool ou de instância até um determinado momento usando o portal do Azure, abra a página do banco de dados e clique em **Restaurar** na barra de ferramentas.

![restauração pontual](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

> [!IMPORTANT]
> Para restaurar programaticamente um banco de dados a partir de um backup, consulte [Programando de forma programática a recuperação usando backups automatizados](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="deleted-database-restore"></a>Restauração de banco de dados excluído

Você pode restaurar um banco de dados excluído para a hora de exclusão ou para um ponto anterior no mesmo servidor de banco de dados SQL usando o portal do Azure, o [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase)ou o [REST (= Restore)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate). Você pode [restaurar o banco de dados excluído na Instância Gerenciada usando o PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../recreate-dropped-database-on-azure-sql-managed-instance). 

> [!TIP]
> Para obter um script de exemplo do PowerShell que mostra como restaurar um banco de dados excluído, consulte [Restaurar um banco de dados SQL usando o PowerShell](scripts/sql-database-restore-database-powershell.md).
> [!IMPORTANT]
> Se você excluir uma instância de servidor do Banco de Dados SQL do Azure, todos os seus bancos de dados também serão excluídos e não poderão ser recuperados. No momento, não há suporte para restaurar um servidor excluído.

### <a name="deleted-database-restore-using-the-azure-portal"></a>Restauração do banco de dados excluída usando o portal do Azure

Para recuperar um banco de dados excluído usando o portal do Azure, abra a página do servidor e, na área operações, clique em bancos de dados **excluídos**.

![deleted-database-restore-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)

![deleted-database-restore-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

> [!IMPORTANT]
> Para restaurar programaticamente um banco de dados excluído, consulte [Programando recuperação programaticamente usando backups automatizados](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="geo-restore"></a>Restauração geográfica

Você pode restaurar um banco de dados SQL em qualquer servidor em qualquer região do Azure a partir dos backups replicados geograficamente mais recentes. A restauração geográfica usa um backup replicado geograficamente como sua origem. Ele pode ser solicitado mesmo que o banco de dados ou o datacenter esteja inacessível devido a uma interrupção.

A restauração geográfica é a opção de recuperação padrão quando seu banco de dados não está disponível devido a um incidente na região de hospedagem. Você pode restaurar o banco de dados para um servidor em qualquer outra região. Há um atraso entre o momento em que um backup é realizado e quando ele é replicado geograficamente para um blob do Azure em uma região diferente. Como resultado, o banco de dados restaurado pode ter até uma hora atrás do banco de dados original. A ilustração a seguir mostra a restauração do banco de dados desde o último backup disponível em outra região.

![Restauração geográfica](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Para obter um script de exemplo do PowerShell que mostra como executar uma restauração geográfica, consulte [Restaurar um banco de dados SQL usando o PowerShell](scripts/sql-database-restore-database-powershell.md).

Atualmente, não há suporte para a restauração pontual em uma área geográfica secundária. A restauração pontual pode ser feita somente em um banco de dados primário. Para obter informações detalhadas sobre como usar a restauração geográfica para se recuperar de uma interrupção, consulte [Recuperação de uma interrupção](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> A restauração geográfica é a solução de recuperação de desastre mais básica disponível no banco de dados SQL. Ele depende de backups replicados geograficamente criados automaticamente com RPO = 1 hora e o tempo de recuperação estimado de até 12 horas. Ele não garante que a região de destino terá a capacidade de restaurar seus bancos de dados após um ourage regional, pois será provável que um aumento preciso da demanda seja. Para um aplicativo não comercialmente crítico que usa bancos de dados relativamente pequenos, a restauração geográfica é uma solução de recuperação de desastre apropriada. Para aplicativos críticos para os negócios que usam grandes bancos de dados e devem garantir a continuidade dos negócios, você deve usar [grupos de failover automático](sql-database-auto-failover-group.md). Ele oferece um RPO e RTO muito menores, e a capacidade é sempre garantida. Para obter mais informações sobre as opções de continuidade dos negócios, consulte [Visão geral de continuidade de negócios](sql-database-business-continuity.md).

### <a name="geo-restore-using-the-azure-portal"></a>Restauração geográfica usando o portal do Azure

Para a restauração geográfica de um banco de dados durante seu [período de retenção do modelo com base em DTU](sql-database-service-tiers-dtu.md) ou [período de retenção do modelo com base em vCore](sql-database-service-tiers-vcore.md) usando o portal do Azure, abra a página do Banco de Dados SQL e clique em **Adicionar**. Na caixa de texto **Selecionar fonte**, selecione **Backup**. Especifique o backup do qual fazer a recuperação, na região e no servidor de sua escolha.

> [!Note]
> A restauração geográfica usando o portal do Azure não está disponível no Instância Gerenciada. Em vez disso, use o PowerShell.

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Executar recuperação programaticamente usando backups automatizados

Conforme discutido anteriormente, além do Portal do Azure, a recuperação de banco de dados pode ser realizada programaticamente usando o Azure PowerShell ou a API REST. As tabelas a seguir descrevem o conjunto de comandos disponíveis.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> O módulo Azure Resource Manager do PowerShell ainda tem suporte do banco de dados SQL do Azure, mas todo o desenvolvimento futuro é para o módulo AZ. Sql. Para esses cmdlets, consulte [AzureRM. SQL](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Os argumentos para os comandos no módulo AZ e nos módulos AzureRm são substancialmente idênticos.

- Para restaurar um banco de dados autônomo ou em pool, consulte [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase).

  | Cmdlet | Descrição |
  | --- | --- |
  | [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) |Obtém um ou mais bancos de dados. |
  | [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup) | Obtém um banco de dados excluído que você pode restaurar. |
  | [Get-AzSqlDatabaseGeoBackup](/powershell/module/az.sql/get-azsqldatabasegeobackup) |Obtém um backup com redundância geográfica de um banco de dados. |
  | [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) |Restaura um banco de dados SQL. |

  > [!TIP]
  > Para obter um script de exemplo do PowerShell que mostra como executar uma recuperação pontual de um banco de dados, consulte [Restaurar um banco de dados SQL usando o PowerShell](scripts/sql-database-restore-database-powershell.md).

- Para restaurar um banco de dados Instância Gerenciada, consulte [Restore-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase).

  | Cmdlet | Descrição |
  | --- | --- |
  | [Get-AzSqlInstance](/powershell/module/az.sql/get-azsqlinstance) |Obtém uma ou mais instâncias gerenciadas. |
  | [Get-AzSqlInstanceDatabase](/powershell/module/az.sql/get-azsqlinstancedatabase) | Obtém bancos de dados de instância. |
  | [Restore-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase) |Restaura um banco de dados de instância. |

### <a name="rest-api"></a>API REST

Para restaurar um banco de dados único ou em pool usando a API REST:

| API | Descrição |
| --- | --- |
| [REST (createMode=Recovery)](https://docs.microsoft.com/rest/api/sql/databases) |Restaura um banco de dados |
| [Obter, Criar ou Atualizar o Status de um Banco de Dados](https://docs.microsoft.com/rest/api/sql/operations) |Retorna o status durante uma operação de restauração |

### <a name="azure-cli"></a>CLI do Azure

- Para restaurar um banco de dados único ou em pool usando a CLI do Azure, consulte [az sql db restore](/cli/azure/sql/db#az-sql-db-restore).
- Para restaurar uma instância gerenciada usando CLI do Azure, consulte [AZ SQL MidB Restore](/cli/azure/sql/midb#az-sql-midb-restore)

## <a name="summary"></a>Resumo

Backups automáticos protegem seus bancos de dados contra erros de usuário e de aplicativo, exclusão acidental do banco de dados e interrupções prolongadas. Essa funcionalidade interna está disponível para todas as camadas de serviço e tamanhos da computação.

## <a name="next-steps"></a>Próximas etapas

- Para obter uma visão geral e os cenários de continuidade dos negócios, consulte [Visão geral da continuidade dos negócios](sql-database-business-continuity.md).
- Para saber mais sobre backups automatizados do Banco de Dados SQL do Azure, confira [Backups automatizados do Banco de Dados SQL](sql-database-automated-backups.md).
- Para saber mais sobre retenção de longo prazo, consulte [Retenção de longo prazo](sql-database-long-term-retention.md).
- Para saber mais sobre opções de recuperação mais rápidas, confira [Replicação geográfica ativa](sql-database-active-geo-replication.md) ou [Grupos de failover automático](sql-database-auto-failover-group.md).
