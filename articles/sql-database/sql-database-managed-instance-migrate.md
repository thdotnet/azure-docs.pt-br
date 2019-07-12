---
title: Migrar o banco de dados da instância do SQL Server para o banco de dados SQL - instância gerenciada | Microsoft Docs
description: Saiba como migrar um banco de dados da instância do SQL Server para o banco de dados SQL - instância gerenciada.
services: sql-database
ms.service: sql-database
ms.subservice: migration
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: douglas, carlrab
manager: craigg
ms.date: 11/07/2019
ms.openlocfilehash: 7cf54b79fac87905117e321574571890c59315e6
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827073"
---
# <a name="sql-server-instance-migration-to-azure-sql-database-managed-instance"></a>Migração de uma Instância do SQL Server para uma Instância Gerenciada do Banco de Dados SQL do Azure

Neste artigo, você aprenderá sobre os métodos para migrar uma Instância do SQL Server 2005 ou versão posterior para uma [Instância Gerenciada do Banco de Dados SQL do Azure](sql-database-managed-instance.md). Para obter informações sobre como migrar para um banco de dados individual ou pool elástico, confira [Migrar para um banco de dados individual ou em pool](sql-database-cloud-migrate.md). Para obter informações de migração sobre como migrar de outras plataformas, confira o [Guia de Migração de Banco de Dados do Azure](https://datamigration.microsoft.com/).

> [!NOTE]
> Se você quiser iniciar rapidamente e experimente a instância gerenciada, você talvez queira ir para [guia de início rápido](/sql-database-managed-instance-quickstart-guide.md) em vez desta página. 

Em um nível alto, o processo de migração de banco de dados se parece com:

![processo de migração](./media/sql-database-managed-instance-migration/migration-process.png)

- [Avaliar a compatibilidade da instância gerenciada](#assess-managed-instance-compatibility) onde você deve garantir que não há nenhum problema de bloqueio que pode impedir que suas migrações.
  - Esta etapa também inclui a criação de [linha de base de desempenho](#create-performance-baseline) para determinar o uso de recursos em sua instância do SQL Server de origem. Essa etapa é necessária se você quiser que o implantar a instância gerenciada de tamanho corretamente e verifique se que o desempenho após a migração não é afetado.
- [Escolha as opções de conectividade do aplicativo](sql-database-managed-instance-connect-app.md)
- [Implantar em uma instância gerenciada de tamanho ideal](#deploy-to-an-optimally-sized-managed-instance) onde você escolherá características técnicas (número de vCores, quantidade de memória) e o nível de desempenho (comercialmente crítico, de finalidade geral) da sua instância gerenciada.
- [Selecionar o método de migração e migrar](#select-migration-method-and-migrate) onde você pode migrar seus bancos de dados usando a migração offline (backup/restauração nativa, importe/exportação do banco de dados) ou online (serviço de migração de dados, replicação transacional).
- [Monitorar aplicativos](#monitor-applications) para garantir que o desempenho de ter esperado.

> [!NOTE]
> Para migrar um banco de dados individual para um banco de dados individual ou um pool elástico, confira [Migrar um banco de dados do Microsoft SQL Server para o Banco de Dados SQL do Azure](sql-database-single-database-migrate.md).

## <a name="assess-managed-instance-compatibility"></a>Avaliar a compatibilidade da instância gerenciada

Primeiro, determine se a instância gerenciada é compatível com os requisitos de banco de dados do aplicativo. A opção de implantação de instância gerenciada foi projetada para fornecer migração lift-and-shift fácil para a maioria dos aplicativos existentes que usam o SQL Server local ou em máquinas virtuais. No entanto, às vezes é necessário recursos ou capacidades que ainda não têm suporte e o custo de implementar uma solução alternativa é muito alto.

Utilize o [DMA (Assistente de Migração de Dados)](https://docs.microsoft.com/sql/dma/dma-overview) para detectar possíveis problemas de compatibilidade que afetam a funcionalidade do banco de dados no Banco de Dados SQL do Azure. O AMD ainda não dá suporte à instância gerenciada como destino de migração, mas é recomendável executar a avaliação no Banco de Dados SQL do Azure e examinar atentamente a lista de problemas de compatibilidade e paridade de recursos relatados em relação à documentação do produto. Confira [Recursos do Banco de Dados SQL do Azure](sql-database-features.md) para verificar se há problemas de bloqueio relatados que não bloqueiam a instância gerenciada, porque a maioria dos problemas de bloqueio que impedem uma migração para o Banco de Dados SQL do Azure foi removida com a instância gerenciada. Por exemplo, recursos como consultas entre banco de dados, transações entre banco de dados na mesma instância, servidor vinculado a outras fontes do SQL, CLR, tabelas temporárias globais, exibições em nível de instância, Service Broker e similares estão disponíveis nas instâncias gerenciadas.

Se houver problemas de bloqueio relatados que não foram removidos com a opção de implantação de instância gerenciada, talvez seja preciso considerar uma opção alternativa, como o [SQL Server em Máquinas Virtuais do Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/). Estes são alguns exemplos:

- Se você precisar de acesso direto ao sistema operacional ou ao sistema de arquivos, por exemplo, para instalar agentes personalizados ou de terceiros na mesma máquina virtual com o SQL Server.
- Se você tiver uma dependência estrita de recursos que ainda não têm suporte, como transações entre instâncias, PolyBase e FileStream/FileTable.
- Se absolutamente necessário permanecer em uma versão específica do SQL Server (2012, por exemplo).
- Se os requisitos de computação forem muito menores do que o oferecido pela instância gerenciada (um vCore, por exemplo) e a fusão de banco de dados não for uma opção aceitável.

Se você tiver resolvido todos os identificados bloqueadores de migração e continuar a migração para a instância gerenciada, observe que algumas das alterações podem afetar o desempenho da carga de trabalho:
- Modelo de recuperação completa obrigatório e o agendamento de backup automatizado regular podem afetar o desempenho de sua carga de trabalho ou ações de manutenção/ETL se você tiver usado o modelo simples/bulk-logged periodicamente ou interrompidos backups sob demanda.
- Servidor ou banco de dados nível configurações diferentes, como sinalizadores de rastreamento ou níveis de compatibilidade
- Novos recursos que você está usando, como grupos de failover automático ou de criptografia de banco de dados transparente (TDE) podem afetar o uso da CPU e e/s.

Gerenciado instância garantia de 99,99% de disponibilidade até mesmo em cenários críticos, portanto, a sobrecarga causada por esses recursos não pode ser desabilitada. Para obter mais informações, consulte [as causas raiz que podem causar desempenho diferente no SQL Server e instância gerenciada](https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/).

### <a name="create-performance-baseline"></a>Criar linha de base de desempenho

Se você precisar comparar o desempenho da carga de trabalho na instância gerenciada com sua carga de trabalho original em execução no SQL Server, você precisaria criar uma linha de base de desempenho que será usada para comparação. 

Linha de base de desempenho é um conjunto de parâmetros, como o uso de CPU média/máx, latência de e/s de disco de média/máx, taxa de transferência, IOPS, a expectativa de vida da página de média/máx, média de tamanho máximo de tempdb. Você gostaria de ter parâmetros semelhantes ou melhores, mesmo após a migração, portanto, é importante medir e registre os valores de linha de base para esses parâmetros. Além dos parâmetros do sistema, você precisa selecionar um conjunto de consultas representativas ou as consultas mais importantes em sua duração de min/média/máx carga de trabalho e a medida, uso da CPU para consultas selecionadas. Esses valores permitem que você comparar o desempenho da carga de trabalho em execução na instância gerenciada com os valores originais no seu código-fonte do SQL Server.

Estes são alguns dos parâmetros que você precisaria de medidas em sua instância do SQL Server: 
- [Monitorar o uso de CPU em sua instância do SQL Server](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Monitor-CPU-usage-on-SQL-Server/ba-p/680777#M131) e registre a média e de pico de uso da CPU.
- [Monitorar o uso de memória na instância do SQL Server](https://docs.microsoft.com/sql/relational-databases/performance-monitor/monitor-memory-usage) e determinar a quantidade de memória usada por componentes diferentes, como o pool de buffers, cache, o pool de repositório de coluna do plano [OLTP na memória](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage?view=sql-server-2017), etc. Além disso, você deve encontrar os valores médios e de pico de contador de desempenho de memória de expectativa de vida da página.
- Monitorar o uso de e/s de disco em que a instância de SQL Server de origem usando [DM io_virtual_file_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql) modo de exibição ou [contadores de desempenho](https://docs.microsoft.com/sql/relational-databases/performance-monitor/monitor-disk-usage).
- Monitorar o desempenho de consulta e a carga de trabalho ou sua instância do SQL Server examinando exibições de gerenciamento dinâmico ou consulta Store se você estiver migrando da versão do SQL Server 2016 +. Identificar duração média e o uso da CPU das consultas mais importantes na sua carga de trabalho para compará-los com as consultas que estão executando na instância gerenciada.

> [!Note]
> Se você perceber algum problema com sua carga de trabalho do SQL Server, como uso elevado da CPU, pressão de memória constante, tempdb ou parametrização problemas, você deve tentar resolvê-los em sua instância do SQL Server de origem antes de colocar a linha de base e a migração. Migrando Conheça os problemas para qualquer novo migh de sistema causam resultados inesperados e invalidam qualquer comparação de desempenho.

Como resultado dessa atividade deve documentar média e valores de pico de CPU, memória e uso de e/s em seu sistema de origem, bem como duração média e máxima e uso da CPU do dominante e as consultas mais importantes em sua carga de trabalho. Você deve usar esses valores posteriormente para comparar o desempenho da carga de trabalho na instância gerenciada com o desempenho de linha de base da carga de trabalho no SQL Server de origem.

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>Implantar em uma instância gerenciada de tamanho ideal

A instância gerenciada é adaptada para cargas de trabalho locais cuja movimentação para a nuvem é planejada. Ela introduz um [novo modelo de compra](sql-database-service-tiers-vcore.md) que oferece maior flexibilidade na seleção do nível certo de recursos para as cargas de trabalho. No ambiente local, você provavelmente está acostumado a dimensionar essas cargas de trabalho usando núcleos físicos e largura de banda de E/S. O modelo de compra para a instância gerenciada baseia-se em núcleos virtuais, ou “vCores”, com armazenamento adicional e E/S disponíveis separadamente. O modelo vCore é uma maneira mais simples de compreender os requisitos de computação na nuvem em relação ao que você utiliza no local atualmente. Esse novo modelo permite que você dimensione adequadamente o ambiente de destino na nuvem. Algumas diretrizes gerais que podem ajudar você a escolher a camada de serviço certa e as características são descritas aqui:
- Com base na linha de base de uso de CPU, você pode provisionar uma instância gerenciada que corresponde ao número de núcleos que você está usando no SQL Server, tendo em mente que as características de CPU talvez precise ser dimensionada para corresponder à [características da VM em que é a instância gerenciada instalado](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics).
- Com base no uso de memória a linha de base escolher [a camada de serviço que tem memória correspondente](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics). A quantidade de memória não pode ser escolhida diretamente, portanto, você precisará selecionar a instância gerenciada com a quantidade de vCores que tem memória correspondente (por exemplo 5.1 GB/vCore Gen5). 
- Com base na linha de base e/s latência do subsistema de arquivo de escolha entre o uso geral (maior que 5 ms de latência) e as camadas de serviço comercialmente crítico (latência menor que 3 ms).
- Com base na taxa de transferência de linha de base pré-alocar o tamanho dos dados ou arquivos de log para obter o desempenho de e/s de esperado.

Você pode escolher a computação e recursos de armazenamento na implantação de tempo e, em seguida, alteração-lo posteriormente sem introduzir tempo de inatividade para seu aplicativo usando o [portal do Azure](sql-database-scale-resources.md):

![dimensionamento da instância gerenciada](./media/sql-database-managed-instance-migration/managed-instance-sizing.png)

Para saber como criar a infraestrutura de VNet e uma instância gerenciada, confira [Criar uma instância gerenciada](sql-database-managed-instance-get-started.md).

> [!IMPORTANT]
> É importante manter a VNet e a sub-rede de destino sempre de acordo com os [requisitos da VNet da instância gerenciada](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Qualquer incompatibilidade pode impedi-lo de criar novas instâncias ou utilizar aquelas que você já criou. Saiba mais sobre a [criação de novas redes](sql-database-managed-instance-create-vnet-subnet.md) e a [configuração de redes existentes](sql-database-managed-instance-configure-vnet-subnet.md).

## <a name="select-migration-method-and-migrate"></a>Selecionar o método de migração e migrar

A opção de implantação de instância gerenciada destina-se a cenários de usuários que exigem a migração de banco de dados em massa de implementações de bancos de dados locais ou IaaS. Essa é a escolha ideal quando for necessário fazer lift-and-shift do back-end dos aplicativos que utilizam regularmente as funcionalidades entre banco de dados e/ou nível de instância. Se este é o seu cenário, você pode mover uma instância inteira para um ambiente correspondente no Azure sem a necessidade de arquitetar novamente seus aplicativos.

Para mover instâncias do SQL, é necessário planejar atentamente:

- A migração de todos os bancos de dados que precisam ser colocados (os que estão executando na mesma instância)
- A migração de objetos de nível de instância que o aplicativo depende, incluindo logons, credenciais, Operadores e Trabalhos SQL Agent e gatilhos de nível de servidor.

A instância gerenciada é um serviço gerenciado que permite delegar algumas das atividades regulares do DBA para a plataforma conforme são inseridas. Portanto, alguns dados de nível de instância não precisam ser migrados, como trabalhos de manutenção para backups regulares ou configuração Always On, pois a [alta disponibilidade](sql-database-high-availability.md) é interna.

A instância gerenciada dá suporte às seguintes opções de migração de banco de dados (atualmente, estes são os únicos métodos de migração compatíveis):

- Serviço de Migração de Banco de Dados do Azure - migração com tempo de inatividade próximo de zero,
- `RESTORE DATABASE FROM URL` nativo - usa backups nativos do SQL Server e requer algum tempo de inatividade.

### <a name="azure-database-migration-service"></a>Serviço de Migração de Banco de Dados do Azure

O [DMS (Serviço de Migração de Banco de Dados do Azure)](../dms/dms-overview.md) é um serviço totalmente gerenciado projetado para permitir migrações contínuas de várias fontes de banco de dados para plataformas de dados do Azure com um tempo de inatividade mínimo. Esse serviço simplifica as tarefas necessárias para mover bancos de dados de terceiros e SQL Server existentes para o Azure. As opções de implantação na versão prévia pública incluem bancos de dados no Banco de Dados SQL do Azure e bancos de dados do SQL Server em uma Máquina Virtual do Azure. O DMS é o método recomendado de migração para as cargas de trabalho empresariais.

Se você usa o SSIS (SQL Server Integration Services) no SQL Server local, o DMS ainda não dá suporte à migração do catálogo do SSIS (SSISDB) que armazena pacotes SSIS. No entanto, você pode provisionar um Azure-SSIS IR (Integration Runtime) no ADF (Azure Data Factory) que criará um SSISDB em uma instância gerenciada e, em seguida, reimplantar os pacotes nele. Confira [Criar um Azure-SSIS IR no ADF](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

Para saber mais sobre esse cenário e as etapas de configuração do DMS, confira [Migrar o banco de dados local para uma instância gerenciada usando o DMS](../dms/tutorial-sql-server-to-managed-instance.md).  

### <a name="native-restore-from-url"></a>RESTAURAÇÃO nativa da URL

A RESTAURAÇÃO de backups nativos (arquivos .bak) obtidos do SQL Server local ou do [SQL Server em Máquinas Virtuais](https://azure.microsoft.com/services/virtual-machines/sql-server/), disponível no [Armazenamento do Azure](https://azure.microsoft.com/services/storage/), é uma das principais funcionalidades da opção de implantação de instância gerenciada que permite uma rápida e fácil migração de banco de dados offline.

O diagrama a seguir fornece uma visão geral de alto nível do processo:

![fluxo de migração](./media/sql-database-managed-instance-migration/migration-flow.png)

A tabela a seguir fornece mais informações sobre os métodos que podem ser utilizados, dependendo da versão do SQL Server de origem em execução:

|Etapa|Mecanismo SQL e versão|Método de backup/restauração|
|---|---|---|
|Coloque o backup no Armazenamento do Microsoft Azure|SQL 2012 SP1 CU2 anterior|Upload do arquivo .bak diretamente para Armazenamento do Microsoft Azure|
||2012 SP1 CU2 - 2016|Backup direto utilizando a sintaxe [WITH CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql)|
||2016 e posterior|Backup direto utilizando [WITH SAS CREDENTIAL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url)|
|Restauração do Armazenamento do Azure para uma instância gerenciada|[RESTORE FROM URL com SAS CREDENTIAL](sql-database-managed-instance-get-started-restore.md)|

> [!IMPORTANT]
> - Ao migrar um banco de dados protegido pela [Transparent Data Encryption](transparent-data-encryption-azure-sql.md) para uma instância gerenciada usando a opção de restauração nativa, o certificado correspondente do SQL Server local ou IaaS precisará ser migrado antes da restauração do banco de dados. Para obter etapas detalhadas, confira [Migrar o certificado TDE para uma instância gerenciada](sql-database-managed-instance-migrate-tde-certificate.md)
> - Não há suporte para restauração de bancos de dados do sistema. Para migrar objetos de nível de instância (armazenados em bancos de dados mestres ou msdb), é recomendável script e executar scripts T-SQL na instância de destino.

Para obter um Início Rápido que mostra como restaurar um backup de banco de dados em uma instância gerenciada usando uma credencial SAS, confira [Restauração do backup para uma instância gerenciada](sql-database-managed-instance-get-started-restore.md).

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]


## <a name="monitor-applications"></a>Monitorar aplicativos

Depois de concluir a migração para a instância gerenciada, você deve controlar o comportamento do aplicativo e o desempenho da carga de trabalho. Esse processo inclui as seguintes atividades:
- [Comparar o desempenho da carga de trabalho em execução na instância gerenciada](#compare-performance-with-the-baseline) com o [linha de base de desempenho que você criou no SQL Server de origem](#create-performance-baseline).
- Continuamente [monitorar o desempenho da carga de trabalho](#monitor-performance) para identificar possíveis problemas e melhoria.

### <a name="compare-performance-with-the-baseline"></a>Comparar o desempenho com a linha de base

A primeira atividade que você precisa executar imediatamente após a migração bem-sucedida é comparar o desempenho da carga de trabalho com o desempenho da carga de trabalho de linha de base. O objetivo dessa atividade é confirmar que o desempenho de carga de trabalho em sua instância gerenciada atende às suas necessidades. 

Migração de banco de dados para a instância gerenciada mantém as configurações de banco de dados e seu nível de compatibilidade original na maioria dos casos. As configurações originais são preservadas sempre que possível para reduzir o risco de algumas degradações de desempenho em comparação comparada o SQL Server de origem. Se o nível de compatibilidade de um banco de dados de usuário fosse 100 ou superior antes da migração, ele permaneceria o mesmo após a migração. Se o nível de compatibilidade de um banco de dados de usuário era 90 antes da migração, no banco de dados atualizado, o nível de compatibilidade é definido como 100, que é o nível de compatibilidade mais baixo compatível com a instância gerenciada. O nível de compatibilidade dos bancos de dados do sistema é 140. Uma vez que a migração para a instância gerenciada, na verdade, está migrando para a versão mais recente do mecanismo de banco de dados do SQL Server, você deve estar ciente de que você precisa testar novamente o desempenho da carga de trabalho para evitar alguns problemas de desempenho surpreendente.

Como pré-requisito, certifique-se de que você tenha concluído as seguintes atividades:
- Alinhe as configurações na instância gerenciada com as configurações da instância do SQL Server de origem investigando vários instância, banco de dados, configurações de tempdb e configurações. Certifique-se de que você não alterou as configurações, como níveis de compatibilidade ou criptografia antes de executar a comparação de desempenho do primeiro ou aceitar o risco de que alguns dos novos recursos que você habilitou podem afetar algumas consultas. Para reduzir os riscos de migração, altere o nível de compatibilidade do banco de dados somente após o monitoramento do desempenho.
- Implemente [diretrizes de práticas recomendadas do armazenamento de finalidade geral](https://techcommunity.microsoft.com/t5/DataCAT/Storage-performance-best-practices-and-considerations-for-Azure/ba-p/305525) como pré-alocando o tamanho dos arquivos para obter o melhor desempenho.
- Saiba mais sobre o [as principais diferenças de ambiente que podem fazer com que as diferenças de desempenho entre a instância gerenciada e SQL Server]( https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/) e identificar os riscos que podem afetar o desempenho.
- Certifique-se de que você mantenha habilitado Store de consulta e o ajuste automático em sua instância gerenciada. Esses recursos permitem que você medir o desempenho da carga de trabalho e corrigir automaticamente os problemas potenciais de desempenho. Saiba como usar consulta Store como uma ferramenta ideal para obter informações sobre o desempenho da carga de trabalho antes e após a alteração de nível de compatibilidade de banco de dados, conforme explicado em [manter a estabilidade do desempenho durante a atualização para a versão mais recente do SQL Server](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade).
Depois de preparar o ambiente que é comparável tanto quanto possível com o seu ambiente local, você pode começar a executar sua carga de trabalho e medir o desempenho. Processo de medição deve incluir os mesmos parâmetros que você medido [enquanto cria o desempenho de linha de base de suas medidas de carga de trabalho no SQL Server de origem](#create-performance-baseline).
Como resultado, você deve comparar os parâmetros de desempenho com a linha de base e identificar diferenças críticas.

> [!NOTE]
> Em muitos casos, não seria capaz de obter desempenho exatamente correspondente na instância gerenciada e SQL Server. A instância gerenciada é um mecanismo de banco de dados do SQL Server, mas a infraestrutura e a configuração de alta disponibilidade na instância gerenciada podem introduzir alguma diferença. Você pode esperar que algumas consultas seria mais rápidas enquanto outro pode ser mais lento. O objetivo de comparação é verificar se o desempenho da carga de trabalho na instância gerenciada corresponde o desempenho no SQL Server (em média) e identificar existem todas as consultas críticas com o desempenho que não coincidem com o desempenho do original.

O resultado da comparação de desempenho pode ser:
- Desempenho da carga de trabalho na instância gerenciada é alinhado ou melhor que o desempenho da carga de trabalho no SQL Server. Nesse caso você tenha confirmado com êxito que a migração seja bem-sucedida.
- Maioria dos parâmetros de desempenho e as consultas no trabalho de carga de trabalho muito bem, com algumas exceções com degradação de desempenho. Nesse caso, você precisaria identificar as diferenças e sua importância. Se houver algumas consultas importantes com degradação de desempenho, você deve investigar são alterados de planos de SQL subjacentes ou as consultas estão atingindo alguns limites de recursos. Nesse caso, mitigação pode ser aplicar algumas dicas nas consultas essenciais (por exemplo alterados nível de compatibilidade, o avaliador de cardinalidade herdada) seja diretamente ou usando guias de plano, recompilar ou criar índices que podem afetar os planos e estatísticas. 
- A maioria das consultas é mais lenta na instância gerenciada, em comparação com o SQL Server de origem. Nesse caso, tente identificar as causas raiz da diferença como [atingir um limite de recurso]( sql-database-managed-instance-resource-limits.md#instance-level-resource-limits) como limites de e/s, o limite de memória, o limite de taxa de log de instância, etc. Se não há nenhum limite de recursos que pode causar a diferença, tente alterar o nível de compatibilidade do banco de dados ou alterar as configurações de banco de dados, como a estimativa de cardinalidade herdada e inicie novamente o teste. Examine as recomendações fornecidas por modos de exibição de instância gerenciada ou Store de consulta para identificar as consultas com regressão recente de desempenho.

> [!IMPORTANT]
> A instância gerenciada tem o recurso de correção de plano automático interno que é habilitado por padrão. Esse recurso garante que consultas que funcionaram bem em Colar seriam prejudicam no futuro. Certifique-se de que esse recurso está habilitado e que foram executadas a tempo suficiente de carga de trabalho com as configurações antigas antes de alterar as novas configurações para habilitar a instância gerenciada saber mais sobre o desempenho de linha de base e os planos.

Faça a alteração dos parâmetros ou atualizar as camadas de serviço para convergir para a configuração ideal, até que você obtenha o desempenho da carga de trabalho que atende às suas necessidades.

### <a name="monitor-performance"></a>Monitorar o desempenho

A instância gerenciada fornece muitas ferramentas avançadas para monitoramento e solução de problemas, e você deve usá-los para monitorar o desempenho em sua instância. Alguns dos parâmetros que seu precisa monitorar são:
- Uso da CPU na instância para determinar faz o número de vCores que você forneceu é a combinação ideal para sua carga de trabalho.
- Expectativa de vida da página em sua instância gerenciada para determinar [precisa de mais memória](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Do-you-need-more-memory-on-Azure-SQL-Managed-Instance/ba-p/563444).
- Aguarde estatísticas, como `INSTANCE_LOG_GOVERNOR` ou `PAGEIOLATCH` que informará que você tiver problemas de e/s de armazenamento, especialmente na camada de uso geral em que você talvez precise pré-alocar arquivos para obter um melhor desempenho de e/s.

## <a name="leverage-advanced-paas-features"></a>Aproveitar recursos avançados de PaaS

Depois que você estiver usando uma plataforma totalmente gerenciada e você verificou que o desempenho de carga de trabalho forem correspondentes desempenho da carga de trabalho do SQL Server, aproveite as vantagens que são fornecidas automaticamente como parte do serviço de banco de dados SQL. 

Mesmo se você não faça algumas alterações na instância gerenciada durante a migração, há chances de alta transformaria em alguns dos novos recursos enquanto estiver operando sua instância para aproveitar um os aprimoramentos mais recentes de mecanismo de banco de dados. Algumas alterações são habilitadas somente quando o [nível de compatibilidade do banco de dados foi alterado](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database).


Por exemplo, não é necessário criar backups na instância gerenciada – o serviço faz backups para você automaticamente. Não é mais necessário preocupar-se com agendamento, execução e gerenciamento de backups. A instância gerenciada fornece a capacidade de restauração para qualquer momento no período de retenção usando o [PITR (Recuperação Pontual)](sql-database-recovery-using-backups.md#point-in-time-restore). Além disso, não é necessário preocupar-se com a configuração de alta disponibilidade, pois a [alta disponibilidade](sql-database-high-availability.md) é interna.

Para aumentar a segurança, considere o uso de [autenticação do Azure Active Directory](sql-database-security-overview.md), [auditoria](sql-database-managed-instance-auditing.md), [detecção de ameaças](sql-database-advanced-data-security.md), [desegurançaemníveldelinha](https://docs.microsoft.com/sql/relational-databases/security/row-level-security), e [mascaramento de dados dinâmicos](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking) ).

Além de gerenciamento avançado e recursos de segurança, a instância gerenciada fornece um conjunto de ferramentas avançadas que podem ajudar você a [monitorar e ajustar sua carga de trabalho](sql-database-monitor-tune-overview.md). [Análise do SQL Azure](https://docs.microsoft.com/azure/azure-monitor/insights/azure-sql) permite monitorar um grande conjunto de instâncias gerenciadas e centralizar o monitoramento de um grande número de instâncias e bancos de dados. [O ajuste automático](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning#automatic-plan-correction) na instância gerenciada monitorar continuamente o desempenho de suas estatísticas de execução do plano SQL e corrigir automaticamente os problemas de desempenho identificados.

## <a name="next-steps"></a>Próximas etapas

- Para obter informações sobre instâncias gerenciadas, confira [O que é uma instância gerenciada?](sql-database-managed-instance.md).
- Para obter um tutorial que inclui uma restauração do backup, confira [Criar uma instância gerenciada](sql-database-managed-instance-get-started.md).
- Para obter um tutorial que mostra a migração usando o DMS, confira [Migrar o banco de dados local para uma instância gerenciada usando o DMS](../dms/tutorial-sql-server-to-managed-instance.md).  
