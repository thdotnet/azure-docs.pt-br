---
title: Migrar pacotes do Integration Services do SQL Server para uma instância gerenciada do banco de dados SQL | Microsoft Docs
description: Saiba como migrar pacotes do Integration Services do SQL Server para uma instância gerenciada do banco de dados SQL.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/08/2019
ms.openlocfilehash: 82a047616c199e37bfa22f53e02f3f7b224b47c1
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67083178"
---
# <a name="migrate-sql-server-integration-services-packages-to-an-azure-sql-database-managed-instance"></a>Migrar pacotes do Integration Services do SQL Server para uma instância gerenciada do banco de dados SQL
Se você usar o SQL Server Integration Services (SSIS) e deseja migrar seus projetos/pacotes do SSIS da origem SSISDB hospedado pelo SQL Server para o SSISDB hospedado por uma instância gerenciada do banco de dados SQL de destino, você pode usar o serviço de migração de banco de dados do Azure.

Se a versão do SSIS que você usa é anterior ao 2012 ou usar tipos de repositório de pacotes não SSISDB, antes de migrar o SSIS projetos/pacotes, você precisará convertê-los usando o Integration Services Project Conversion Wizard, que também pode ser iniciado no SSMS. Para obter mais informações, consulte o artigo [Converter projetos para o modelo de implantação de projeto](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages?view=sql-server-2017#convert).

> [!NOTE]
> Serviço de migração de banco de dados do Azure (DMS) atualmente não dá suporte banco de dados SQL como um destino de migração de destino. Para reimplantar projetos/pacotes do SSIS para o banco de dados SQL, consulte o artigo [pacotes de reimplantar o SQL Server Integration Services para o banco de dados do Azure SQL](https://docs.microsoft.com/azure/dms/how-to-migrate-ssis-packages).

Neste artigo, você aprenderá a:
> [!div class="checklist"]
>
> * Avaliar projetos/pacotes do SSIS de origem.
> * Migrar projetos/pacotes do SSIS para o Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir essas etapas, você precisa:

* Para criar uma rede Virtual do Azure (VNet) para o serviço de migração de banco de dados do Azure usando o modelo de implantação do Azure Resource Manager, que fornece conectividade site a site aos servidores de origem locais usando o [ExpressRoute ](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) ou [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Para obter mais informações, consulte o artigo [topologias de rede para o banco de dados SQL gerenciados migrações da instância usando o serviço de migração de banco de dados do Azure]( https://aka.ms/dmsnetworkformi). Para obter mais informações sobre como criar uma VNet, confira a [Documentação da Rede Virtual](https://docs.microsoft.com/azure/virtual-network/) e, especificamente, os artigos de Início Rápido com detalhes passo a passo.
* Para garantir que suas regras de grupo de segurança de rede de rede virtual não bloqueiam as seguintes portas de comunicação de entrada para o serviço de migração de banco de dados do Azure: 443, 53, 9354, 445, 12000. Veja mais detalhes sobre a filtragem de tráfego do NSG da VNet do Azure no artigo [Filtrar o tráfego de rede com grupos de segurança de rede](https://docs.microsoft.com/azure/virtual-network/virtual-network-vnet-plan-design-arm).
* Para configurar seu [Firewall do Windows para acesso ao mecanismo de banco de dados de origem](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access?view=sql-server-2017).
* Para abrir o Firewall do Windows para permitir que o serviço de migração de banco de dados do Azure acessar a fonte do SQL Server, que é a porta TCP 1433 por padrão.
* Caso esteja executando várias instâncias nomeadas do SQL Server usando portas dinâmicas, talvez seja preciso habilitar o serviço do SQL Browser e permitir o acesso à porta UDP 1434 por meio de seus firewalls para que o Serviço de Migração de Banco de Dados do Azure possa se conectar a uma instância nomeada em seu servidor de origem.
* Se estiver usando um dispositivo de firewall na frente dos bancos de dados de origem, talvez seja preciso adicionar regras de firewall para permitir que o Serviço de Migração de Banco de Dados do Azure acesse os bancos de dados de origem para migração, além de arquivos por meio da porta 445 do SMB.
* Instância para hospedar o SSISDB gerenciada do banco de dados SQL do Azure. Se você precisar criar um, siga os detalhes no artigo [criar um banco de dados de instância gerenciada do SQL](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started).
* Para garantir que os logons usados para conectar-se a fonte de instância gerenciada do SQL Server e de destino são membros da função de servidor sysadmin.
* Para verificar se o SSIS é provisionado no Azure dados Factory (ADF) que contém o IR Azure-SSIS Integration Runtime () com o SSISDB hospedado por um banco de dados do SQL Azure de destino de instância gerenciada (conforme descrito no artigo [criar o Azure-SSIS tempo de execução de integração no Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)).

## <a name="assess-source-ssis-projectspackages"></a>Avaliar projetos/pacotes do SSIS de origem

Enquanto a avaliação do código-fonte SSISDB ainda não está integrada para o banco de dados de migração de DMA (Assistente), os projetos/pacotes do SSIS será avaliado/validado conforme eles estiverem reimplantados no que SSISDB hospedado em uma instância gerenciada do banco de dados SQL de destino.

## <a name="register-the-microsoftdatamigration-resource-provider"></a>Registrar o provedor de recursos Microsoft.DataMigration

1. Entre no portal do Azure, selecione **Todos os serviços** e selecione **Assinaturas**.

    ![Mostrar assinaturas do portal](media/how-to-migrate-ssis-packages-mi/portal-select-subscriptions.png)

2. Selecione a assinatura na qual deseja criar a instância do Serviço de Migração de Banco de Dados do Azure e, em seguida, selecione **Provedores de recursos**.

    ![Exibir provedores de recursos](media/how-to-migrate-ssis-packages-mi/portal-select-resource-provider.png)

3. Pesquise por migração e, em seguida, à direita do **Microsoft.DataMigration**, selecione **Registrar**.

    ![Registrar provedor de recursos](media/how-to-migrate-ssis-packages-mi/portal-register-resource-provider.png)

## <a name="create-an-azure-database-migration-service-instance"></a>Criar uma instância do Serviço de Migração de Banco de Dados do Azure

1. No portal do Azure, selecione + **Criar um recurso**, pesquise **Serviço de Migração de Banco de Dados do Azure** e, em seguida, selecione **Serviço de Migração de Banco de Dados do Azure** na lista suspensa.

     ![Azure Marketplace](media/how-to-migrate-ssis-packages-mi/portal-marketplace.png)

2. Na tela **Serviço de Migração de Banco de Dados do Azure**, selecione **Criar**.

    ![Criar uma instância do Serviço de Migração de Banco de Dados do Azure](media/how-to-migrate-ssis-packages-mi/dms-create1.png)

3. Na tela **Criar Serviço de Migração**, especifique um nome para o serviço, a assinatura e um grupo de recurso novo ou existente.

4. Selecione o local no qual você deseja criar a instância do DMS.

5. Selecione uma VNET existente ou crie uma.

    A rede virtual fornece o serviço de migração de banco de dados do Azure com acesso para o SQL Server de origem e a instância gerenciada do banco de dados SQL de destino.

    Para obter mais informações sobre como criar uma VNET no portal do Azure, confira o artigo [Criar uma rede virtual usando o portal do Azure](https://aka.ms/DMSVnet).

    Para obter detalhes adicionais, confira o artigo [Topologias de rede para migrações da instância gerenciada do BD SQL do Azure usando o Serviço de Migração de Banco de Dados do Azure](https://aka.ms/dmsnetworkformi).

6. Selecione um tipo de preço.

    Para obter mais informações sobre os custos e camadas de preços, consulte a [página de preços](https://aka.ms/dms-pricing).

    ![Criar o serviço DMS](media/how-to-migrate-ssis-packages-mi/dms-create-service2.png)

7. Selecione **Criar** para criar a conta.

## <a name="create-a-migration-project"></a>Criar um projeto de migração

Depois que uma instância do serviço é criada, localize-a no portal do Azure, abra-a e, em seguida, crie um novo projeto de migração.

1. Faça logon no portal do Azure, selecione **+ criar um recurso**, procure o serviço de migração de banco de dados do Azure e, em seguida, selecione **serviço de migração de banco de dados do Azure** na lista suspensa.

    ![Crie uma instância do Serviço de Migração de Banco de Dados do Azure](media/how-to-migrate-ssis-packages-mi/dms-search.png)

2. Na tela **Serviço de Migração de Banco de Dados do Azure**, procure o nome da instância que você criou e, em seguida, selecione-a.

3. Selecione + **Novo Projeto de Migração**.

4. No **novo projeto de migração** tela, especifique um nome para o projeto na **tipo de servidor de origem** caixa de texto, selecione **do SQL Server**, no **servidor de destino tipo de** caixa de texto, selecione **banco de dados de instância gerenciada do SQL**e, em seguida, para **Escolher tipo de atividade**, selecione **demigraçãodepacoteSSIS**.

   ![Criar projeto do DMS](media/how-to-migrate-ssis-packages-mi/dms-create-project2.png)

5. Selecione **Criar** para criar o cluster.

## <a name="specify-source-details"></a>Especifique as configurações de origem

1. Na tela **Detalhe de origem de migração**, especifique os detalhes da conexão do SQL Server de origem.

2. Caso não tenha instalado um certificado confiável no servidor, selecione a caixa de seleção **Certificado de servidor confiável**.

    Quando não houver um certificado confiável instalado, o SQL Server gerará um certificado autoassinado quando a instância for iniciada. Esse certificado é usado para criptografar as credenciais das conexões de cliente.

    > [!CAUTION]
    > As conexões SSL que são criptografadas usando um certificado autoassinado não fornecem alta segurança. Elas são suscetíveis a ataques “man-in-the-middle”. Você não deve confiar no SSL usando certificados autoassinados em um ambiente de produção ou em servidores conectados à Internet.

   ![Detalhes da origem](media/how-to-migrate-ssis-packages-mi/dms-source-details1.png)

3. Clique em **Salvar**.

## <a name="specify-target-details"></a>Detalhes do destino favorito

1. Sobre o **detalhes de destino de migração** tela, especifique os detalhes de conexão para o destino.

     ![Detalhes de destino](media/how-to-migrate-ssis-packages-mi/dms-target-details2.png)

2. Clique em **Salvar**.

## <a name="review-the-migration-summary"></a>Análise do resumo da migração

1. Na tela **Resumo de migração**, na caixa de texto **Nome da atividade**, especifique um nome para a atividade de migração.

2. Para o **opção de substituição de projetos do SSIS e ambientes**, especifique se deseja substituir ou ignorar os projetos do SSIS e os ambientes existentes.

    ![Resumo do projeto de migração](media/how-to-migrate-ssis-packages-mi/dms-project-summary2.png)

3. Analise e verifique os detalhes associados ao projeto de migração.

## <a name="run-the-migration"></a>Execute a migração

* Selecione **Executar migração**.

## <a name="next-steps"></a>Próximas etapas

* Verificar as diretrizes de migração no [Guia de Migração de Banco de Dados da Microsoft](https://datamigration.microsoft.com/).
