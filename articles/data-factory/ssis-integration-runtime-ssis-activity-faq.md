---
title: Solucionando problemas de execução de pacote SSIS integration Runtime | Microsoft Docs
description: Este artigo fornece as diretrizes de solução de problemas para execução de pacotes do SSIS no Integration Runtime do SSIS
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 04/15/2019
author: wenjiefu
ms.author: wenjiefu
ms.reviewer: sawinark
manager: craigg
ms.openlocfilehash: 7789970b47f0e55adee5bbe9da9f303aee6cdb25
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67190116"
---
# <a name="troubleshooting-package-execution-in-ssis-integration-runtime"></a>Solucionando problemas de execução de pacote SSIS integration Runtime

Este artigo inclui os erros mais comuns que podem ser acessados quando a execução de SSIS pacotes no Integration Runtime do SSIS, possíveis causas e ações para resolver os erros.

## <a name="where-can-i-find-logs-for-troubleshoot"></a>Onde posso encontrar os logs para solucionar problemas

* O Portal do AAD pode ser usado para verificar a saída da atividade de execução de pacote SSIS incluindo o resultado da execução, as mensagens de erro e ID da operação. Detalhes podem ser encontrados em [monitorar o pipeline](how-to-invoke-ssis-package-ssis-activity.md#monitor-the-pipeline)

* O catálogo do SSIS (SSISDB) pode ser usado para verificar os logs de detalhes para a execução. Detalhes podem ser encontrados em [monitorar pacotes em execução e outras operações](https://docs.microsoft.com/sql/integration-services/performance/monitor-running-packages-and-other-operations?view=sql-server-2017)

## <a name="common-errors-causes-and-solution"></a>Erros comuns, causas e soluções

### <a name="error-message-connection-timeout-expired-or-the-service-has-encountered-an-error-processing-your-request-please-try-again"></a>Mensagem de erro: `"Connection Timeout Expired."` ou `"The service has encountered an error processing your request. Please try again."`

* Possível causa & ação recomendada:
  * O destino da fonte de dados está sobrecarregado. Verifique a carga em seu dados de origem/destino e se ele tem capacidade suficiente. Por exemplo, se o SQL Azure é usado, é recomendável para considerar a escala de backup se o banco de dados é a probabilidade de atingir o tempo limite.
  * A rede entre o Integration Runtime do SSIS e o destino da fonte de dados é instável, especialmente quando a conexão é entre regiões ou entre locais e o azure. É recomendável aplicar o padrão de repetição no pacote do SSIS, siga as etapas:
    * Verifique se que os pacotes do SSIS pode executar novamente em caso de falha sem efeitos colaterais (por exemplo. perda de dados, dados dup....)
    * Configurar o **Repita** e **intervalo de repetição** de executar a atividade do pacote do SSIS na guia geral ![definir propriedades na guia Geral](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
    * Para o componente do ADO.NET e OLEDB origem/destino, ConnectRetryCount e ConnectRetryInterval podem ser definidas no Gerenciador de Conexão no pacote do SSIS ou a atividade do SSIS

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-the-following-error-message-a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-server-the-server-was-not-found-or-was-not-accessible"></a>Mensagem de erro: `"ADO NET Source has failed to acquire the connection '...' with the following error message: "A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible."`

* Possível causa & ação recomendada:
  * Esse problema geralmente significa que o destino da fonte de dados está acessível a partir do SSIS Integration Runtime, que pode ser causado por diferentes motivos:
    * Verifique se que você está passando o dados de origem/destino IP/nome corretamente
    * Verifique se que o firewall está configurado corretamente
    * Certifique-se de que sua rede virtual está configurada corretamente se seus dados de origem/destino estiverem no local.
      * Você pode verificar se o problema é de configuração de rede virtual ao provisionar uma VM do Azure na mesma rede virtual. E, em seguida, verifique se o destino da fonte de dados pode ser acessado da VM do Azure
      * Você pode encontrar mais detalhes sobre como usar a rede virtual com o Integration Runtime do SSIS no [unir um tempo de execução de integração do Azure-SSIS a uma rede virtual](join-azure-ssis-integration-runtime-virtual-network.md)

### <a name="error-message-ado-net-source-has-failed-to-acquire-the-connection--with-the-following-error-message-could-not-create-a-managed-connection-manager"></a>Mensagem de erro: "`ADO NET Source has failed to acquire the connection '...' with the following error message: "Could not create a managed connection manager.`"

* Possível causa & ação recomendada:
  * O provedor ADO.NET usado no pacote não está instalado no SSIS Integration Runtime. Você pode instalar o provedor usando a instalação personalizada. Para obter mais detalhes sobre a instalação personalizada podem ser encontrados em [personalizar a instalação para o tempo de execução de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md)

### <a name="error-message-the-connection--is-not-found"></a>Mensagem de erro: "`The connection '...' is not found`"

* Possível causa & ação recomendada:
  * Esse erro pode ser porque um problema conhecido na versão antiga do SSMS. Se o pacote contiver um componente personalizado (por exemplo, o Feature Pack do SSIS do Azure ou o 3º componentes de terceiros) que não está instalado no computador em que o SSMS é usado para fazer a implantação, o componente será removido pelo SSMS e fazer com que o erro. Atualizar [SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) para a versão mais recente que tem o correção do problema.

### <a name="error-message-there-is-not-enough-space-on-the-disk"></a>Mensagem de erro: "Não há espaço suficiente no disco"

* Possível causa & ação recomendada:
  * Esse erro significa que o disco local é usado para cima no nó do Integration Runtime do SSIS. Verifique se o pacote ou a instalação personalizada consumir vários espaços de disco.
    * Se o disco for consumido por seu pacote, ele será liberado após a conclusão da execução do pacote.
    * Se o disco é consumido pelo seu programa de instalação personalizado, você precisará interromper o Integration Runtime do SSIS, modificar o Script e iniciar o Integration Runtime SSIS novamente. O contêiner de BLOBs do Azure inteiro especificado para instalação personalizada serão copiadas para o nó do IR do SSIS, portanto, verifique se há qualquer conteúdo desnecessário no contêiner.

### <a name="error-message-cannot-open-file-"></a>Mensagem de erro: "Não é possível abrir arquivo '...'"

* Possível causa & ação recomendada:
  * Esse erro ocorre quando a execução do pacote não é possível localizar o arquivo no disco local no tempo de execução de integração do SSIS.
    * Não é recomendável para usar um caminho absoluto no pacote em execução no tempo de execução de integração do SSIS. Usar o diretório de trabalho de execução atual (.) ou a pasta temporária (% TEMP %) em vez disso.
    * Se for necessário para manter alguns arquivos em nós de Integration Runtime do SSIS, é recomendável para preparar os arquivos por meio [personalizar a instalação](how-to-configure-azure-ssis-ir-custom-setup.md). Todos os arquivos no diretório de trabalho de execução serão limpo após a execução for concluída.
    * Outra opção é usar o arquivos do Azure em vez de armazenar o arquivo no nó do Integration Runtime do SSIS. Mais detalhes podem ser encontrados em [ https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares ](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-files-file-shares?view=sql-server-2017#use-azure-file-shares).

### <a name="error-message-the-database-ssisdb-has-reached-its-size-quota"></a>Mensagem de erro: "O banco de dados 'SSISDB' atingiu sua cota de tamanho"

* Possível causa & ação recomendada:
  * O SSISDB criado na instância gerenciada ou SQL do Azure durante a criação de SSIS Integration Runtime atingiu sua cota.
    * Considere aumentar o DTU do banco de dados para resolver esse problema. Detalhes podem ser encontrados em [https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server)
    * Verifique se o seu pacote geraria muitos logs. Nesse caso, o trabalho Elástico pode ser configurado para limpar esses logs. Consulte a [limpar os logs SSISDB com trabalhos Elásticos de banco de dados do Azure](how-to-clean-up-ssisdb-logs-with-elastic-jobs.md) para obter detalhes.

### <a name="error-message-the-request-limit-for-the-database-is--and-has-been-reached"></a>Mensagem de erro: "O limite de solicitação para o banco de dados é... e foi atingido."

* Possível causa & ação recomendada:
  * Se muitos pacotes estiverem em execução em paralelo em tempo de execução de integração do SSIS, esse erro pode ocorrer porque a limitação de solicitação do SSISDB for atingida. Considere aumentar o DTC do seu SSISDB para resolver esse problema. Detalhes podem ser encontrados em [https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-logical-server)

### <a name="error-message-ssis-operation-failed-with-unexpected-operation-status-"></a>Mensagem de erro: "Falha na operação do SSIS com status de operação inesperado:..."

* Possível causa & ação recomendada:
  * O erro é causado principalmente por um erro transitório, tente executar novamente a execução do pacote. É recomendável aplicar o padrão de repetição no pacote do SSIS, siga as etapas:
    * Verifique se que os pacotes do SSIS pode executar novamente em caso de falha sem efeitos colaterais (por exemplo, perda de dados, dados dup....)
    * Configurar o **Repita** e **intervalo de repetição** de executar a atividade do pacote do SSIS na guia geral ![definir propriedades na guia Geral](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)
    * Para o componente do ADO.NET e OLEDB origem/destino, ConnectRetryCount e ConnectRetryInterval podem ser definidas no Gerenciador de Conexão no pacote do SSIS ou a atividade do SSIS

### <a name="error-message-there-is-no-active-worker"></a>Mensagem de erro: "Não há nenhum trabalho ativo."

* Possível causa & ação recomendada:
  * Esse erro geralmente significa que o tempo de execução de integração do SSIS está em um status não íntegro. Verifique o portal do Azure para os erros de status e detalhes: [https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime](https://docs.microsoft.com/azure/data-factory/monitor-integration-runtime#azure-ssis-integration-runtime)

### <a name="error-message-your-integration-runtime-cannot-be-upgraded-and-will-eventually-stop-working-since-we-cannot-access-the-azure-blob-container-you-provided-for-custom-setup"></a>Mensagem de erro: "O seu tempo de execução de integração não pode ser atualizado e eventualmente deixará de funcionar, já que não é possível acessar o contêiner de BLOBs do Azure fornecida para a instalação personalizada."

* Esse erro ocorre quando o Integration Runtime do SSIS não é possível acessar o armazenamento configurado para a instalação personalizada. Verifique se o Uri SAS fornecido é válido e não expirou.

### <a name="error-message-microsoft-ole-db-provider-for-analysis-services-hresult-0x80004005-description-com-error-com-error-mscorlib-exception-has-been-thrown-by-the-target-of-an-invocation"></a>Mensagem de erro: "Provedor Microsoft OLE DB para Analysis Services. 'Hresult: 0x80004005 Description:' Erro COM: Erro COM: mscorlib; Exceção foi lançada pelo destino de uma invocação"

* Possível causa & ação recomendada:
  * Uma possível causa é o nome de usuário/senha com o MFA habilitado está configurada para autenticação do Azure Analysis Services, que ainda não é suportada no SSIS integration runtime. Tente usar a entidade de serviço para autenticação do serviço de análise do Azure:
    1. Preparar a entidade de serviço para AAS [https://docs.microsoft.com/azure/analysis-services/analysis-services-service-principal](https://docs.microsoft.com/azure/analysis-services/analysis-services-service-principal)
    2. No Gerenciador de conexão, definir "Usar um nome de usuário específico e uma senha": definir "AppID" como nome de usuário e "clientSecret" como senha

### <a name="package-takes-unexpected-long-time-to-execute"></a>Pacote terá inesperado muito tempo para executar

* Possível causa & ação recomendada:
  * Número excessivo de execuções de pacote foram agendadas no SSIS Integration Runtime. Nesse caso, todas essas execuções estarão esperando em uma fila por sua vez para executar.
    * Contagem de execução paralelo máx por IR = contagem de nós * execução paralelo máx por nó
    * Consulte a [criar o Azure-SSIS Integration Runtime no Azure Data Factory](create-azure-ssis-integration-runtime.md) para definir a contagem de nós e a execução paralela máx por nó.
  * Integration Runtime do SSIS está parado ou em um status não íntegro. Verifique [Azure-SSIS integration runtime](monitor-integration-runtime.md#azure-ssis-integration-runtime) para saber como verificar o status do Integration Runtime do SSIS e os erros.
  * É recomendável definir o tempo limite se você tiver certeza que a execução do pacote deverá ser concluída em um determinado momento: ![Definir propriedades na guia Geral](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

### <a name="poor-performance-in-package-execution"></a>Um baixo desempenho em execução do pacote

* Possível causa & ação recomendada:

  * Verifique se o tempo de execução de integração do SSIS está na mesma região que a fonte de dados e o destino.

  * Habilitar o nível de log de "Desempenho"

      Você pode definir o nível de log de execução do pacote "Desempenho" para coletar informações de duração mais detalhadas para cada componente na execução. Detalhes podem ser encontrados em: [https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging](https://docs.microsoft.com/sql/integration-services/performance/integration-services-ssis-logging)

  * Verificar o desempenho do nó de IR na página do monitor de IR no portal do Azure.
    * Como monitorar o Integration Runtime do SSIS: [Tempo de execução de integração do Azure-SSIS](monitor-integration-runtime.md#azure-ssis-integration-runtime)
    * O histórico de uso de CPU/memória do SSIS Integration Runtime está disponível nas métricas do Data Factory no portal do Azure ![monitorar as métricas do SSIS Integration Runtime](media/ssis-integration-runtime-ssis-activity-faq/monitor-metrics-ssis-integration-runtime.png)
