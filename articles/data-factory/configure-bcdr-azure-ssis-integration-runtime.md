---
title: Configurar o tempo de execução de integração do Azure-SSIS para failover do banco de dados SQL | Microsoft Docs
description: Este artigo descreve como configurar o Tempo de Execução de Integração do SSIS do Azure com a replicação geográfica e o failover do Banco de Dados SQL do Azure para o banco de dados SSISDB
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 08/14/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: f0612a688bb1e0fd79325b9a1f9b43731a210d10
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66399234"
---
# <a name="configure-the-azure-ssis-integration-runtime-with-azure-sql-database-geo-replication-and-failover"></a>Configurar o tempo de execução de integração do Azure-SSIS com a replicação geográfica e o failover do Banco de Dados SQL do Azure

Este artigo descreve como configurar a replicação geográfica do Tempo de Execução de Integração do Azure-SSIS com o Banco de Dados SQL do Azure para o banco de dados SSISDB. Quando ocorre um failover, você pode garantir que o IR do Azure-SSIS continue trabalhando com o banco de dados secundário.

Para obter mais informações sobre a replicação geográfica e o failover para o banco de dados SQL, consulte [visão geral: Grupos de failover automático e replicação geográfica ativos](../sql-database/sql-database-geo-replication-overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenario-1---azure-ssis-ir-is-pointing-to-read-write-listener-endpoint"></a>Cenário 1 - IR Azure-SSIS está apontando para o ponto de extremidade do ouvinte de leitura / gravação

### <a name="conditions"></a>Condições

Esta seção se aplica quando as seguintes condições forem verdadeiras:

- A prefeitura diz que "os principais problemas de saúde são os seguintes:

  E

- O servidor do banco de dados SQL está *não* configurado com a regra de ponto de extremidade de serviço de rede virtual.

### <a name="solution"></a>Solução

Quando ocorre um failover, ele é transparente para o IR do Azure-SSIS. O Azure-SSIS IR conecta-se automaticamente ao novo primário do grupo de failover.

## <a name="scenario-2---azure-ssis-ir-is-pointing-to-primary-server-endpoint"></a>Cenário 2 - o Azure-SSIS IR está apontando para o terminal principal do servidor

### <a name="conditions"></a>Condições

Esta seção se aplica quando uma das seguintes condições for verdadeira:

- O IR do Azure-SSIS está apontando para o endpoint do servidor principal do grupo de failover. Esse ponto de extremidade é alterado quando ocorre um failover.

  OU

- O servidor do Banco de Dados SQL do Azure é configurado com a regra de ponto de extremidade de serviço de rede virtual.

  OU

- O servidor de banco de dados é uma Instância Gerenciada do Banco de Dados SQL configurada com uma rede virtual.

### <a name="solution"></a>Solução

Quando o failover ocorre, você precisa fazer o seguinte:

1. Interrompa o IR do Azure-SSIS.

2. Reconfigure o IR para apontar para o novo endpoint primário e para uma rede virtual na nova região.

3. Reinicie o IR.

As seções a seguir descrevem essas etapas mais detalhadamente.

### <a name="prerequisites"></a>Pré-requisitos

- Certifique-se de que você habilitou a recuperação de desastres para o servidor do banco de dados SQL, caso o servidor tenha uma interrupção ao mesmo tempo. Para obter mais informações, consulte [visão geral da continuidade dos negócios com o banco de dados SQL do Azure](../sql-database/sql-database-business-continuity.md).

- Se você estiver usando uma rede virtual na região atual, você precisa usar outra rede virtual na nova região para conectar seu tempo de execução de integração do Azure-SSIS. Para obter mais informações, consulte [unir um tempo de execução de integração do Azure-SSIS a uma rede virtual](join-azure-ssis-integration-runtime-virtual-network.md).

- Se você estiver usando uma configuração personalizada, talvez seja necessário preparar outro URI do SAS para o contêiner de blob que armazena seu script de instalação personalizado e os arquivos associados, para que ele continue acessível durante uma interrupção. Para obter mais informações, consulte [Configurar uma configuração personalizada em um tempo de execução de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="steps"></a>Etapas

Siga estas etapas para interromper o IR do Azure-SSIS, alternar o IR para uma nova região e iniciá-lo novamente.

1. Pare o IR na região original.

2. Chame o seguinte comando no PowerShell para atualizar o IR com as novas configurações.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -Location "new region" `
                    -CatalogServerEndpoint "Azure SQL Database server endpoint" `
                    -CatalogAdminCredential "Azure SQL Database server admin credentials" `
                    -VNetId "new VNet" `
                    -Subnet "new subnet" `
                    -SetupScriptContainerSasUri "new custom setup SAS URI"
    ```

    Para obter mais informações sobre esse comando do PowerShell, consulte [criar o tempo de execução de integração do Azure-SSIS no Azure Data Factory](create-azure-ssis-integration-runtime.md)

3. Inicie o IR novamente.

## <a name="scenario-3---attaching-an-existing-ssisdb-ssis-catalog-to-a-new-azure-ssis-ir"></a>Cenário 3 - anexando um existente SSISDB (catálogo do SSIS) para um novo IR do Azure-SSIS

Quando ocorrer um desastre ADF ou IR Azure-SSIS na região atual, você pode tornar seu mantém SSISDB trabalhando com um novo IR do Azure-SSIS em uma nova região.

### <a name="prerequisites"></a>Pré-requisitos

- Se você estiver usando uma rede virtual na região atual, você precisa usar outra rede virtual na nova região para conectar seu tempo de execução de integração do Azure-SSIS. Para obter mais informações, consulte [unir um tempo de execução de integração do Azure-SSIS a uma rede virtual](join-azure-ssis-integration-runtime-virtual-network.md).

- Se você estiver usando uma configuração personalizada, talvez seja necessário preparar outro URI do SAS para o contêiner de blob que armazena seu script de instalação personalizado e os arquivos associados, para que ele continue acessível durante uma interrupção. Para obter mais informações, consulte [Configurar uma configuração personalizada em um tempo de execução de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="steps"></a>Etapas

Siga estas etapas para interromper o IR do Azure-SSIS, alternar o IR para uma nova região e iniciá-lo novamente.

1. Executar um procedimento armazenado para tornar o SSISDB anexado ao **\<new_data_factory_name\>** ou  **\<new_integration_runtime_name\>** .
   
  ```SQL
    EXEC [catalog].[failover_integration_runtime] @data_factory_name='<new_data_factory_name>', @integration_runtime_name='<new_integration_runtime_name>'
   ```

2. Criar um novo data factory denominado **\<new_data_factory_name\>** na nova região. Para obter mais informações, consulte Criar uma fábrica de dados.

     ```powershell
     Set-AzDataFactoryV2 -ResourceGroupName "new resource group name" `
                         -Location "new region"`
                         -Name "<new_data_factory_name>"
     ```
    Para obter mais informações sobre esse comando do PowerShell, consulte [criar um Azure data factory usando o PowerShell](quickstart-create-data-factory-powershell.md)

3. Criar um novo IR do Azure-SSIS denominado **\<new_integration_runtime_name\>** na nova região usando o Azure PowerShell.

    ```powershell
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "new resource group name" `
                                           -DataFactoryName "new data factory name" `
                                           -Name "<new_integration_runtime_name>" `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId "new vnet" `
                                           -Subnet "new subnet" `
                                           -CatalogServerEndpoint $SSISDBServerEndpoint `
                                           -CatalogPricingTier $SSISDBPricingTier
    ```

    Para obter mais informações sobre esse comando do PowerShell, consulte [criar o tempo de execução de integração do Azure-SSIS no Azure Data Factory](create-azure-ssis-integration-runtime.md)

4. Inicie o IR novamente.

## <a name="next-steps"></a>Próximas etapas

Considere estas outras opções de configuração para o IR do Azure-SSIS:

- [Configurar o tempo de execução de integração do Azure-SSIS para alto desempenho](configure-azure-ssis-integration-runtime-performance.md)

- [Personalizar a instalação para o tempo de execução de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-custom-setup.md)

- [Provisionar a Enterprise Edition para o tempo de execução de integração do Azure-SSIS](how-to-configure-azure-ssis-ir-enterprise-edition.md)
