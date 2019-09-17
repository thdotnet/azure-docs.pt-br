---
title: Criar Azure-SSIS Integration Runtime no Azure Data Factory | Microsoft Docs
description: Aprenda a criar um Azure-SSIS Integration Runtime no Azure Data Factory para poder implantar e executar pacotes do SSIS no Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/15/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 36cb4d306cd74dcde09fa55fc582a043bc324f65
ms.sourcegitcommit: a819209a7c293078ff5377dee266fa76fd20902c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71010010"
---
# <a name="create-azure-ssis-integration-runtime-in-azure-data-factory"></a>Criar um Azure-SSIS Integration Runtime no Azure Data Factory

Este tutorial fornece etapas de como provisionar um Integration Runtime (IR) do Azure-SQL Server Integration Services (SSIS) no Azure Data Factory (ADF). Azure-SSIS IR oferece suporte à execução de pacotes implantados no catálogo do SSIS (SSISDB) hospedado pelo servidor do Banco de Dados SQL do Azure/Instância Gerenciada (modelo de implantação de projeto) e aqueles implantados em sistemas de arquivo/compartilhamentos de arquivos/Arquivos do Azure (modelo de implantação de pacote). Depois que o Azure-SSIS IR é provisionado, você pode usar ferramentas familiares, como o SQL Server Data Tools (SSDT)/SQL Server Management Studio (SSMS) e os utilitários de linha de comando, como `dtinstall`/`dtutil`/`dtexec` para implantar e executar os pacotes no Azure.

O [Tutorial: O provisionamento de Azure-SSIS ir](tutorial-create-azure-ssis-runtime-portal.md) mostra como criar um Azure-SSIS ir por meio do aplicativo portal do Azure/ADF e, opcionalmente, usar o servidor/instância gerenciada do banco de dados SQL do Azure para hospedar o SSISDB. Este artigo expande o tutorial e mostra como fazer o seguinte:

- Opcionalmente, use o servidor de banco de dados SQL do Azure com pontos de extremidade de serviço de rede virtual/Instância Gerenciada com um ponto de extremidades privado para hospedar o SSISDB. Como pré-requisito, você precisa configurar as permissões/configurações de rede virtual para seu Azure-SSIS IR para ingressar em uma rede virtual.

- Opcionalmente, use a autenticação Azure Active Directory (AAD) com a identidade gerenciada para que o ADF se conecte ao servidor/Instância Gerenciada do banco de dados SQL do Azure. Como pré-requisito, você precisará adicionar a identidade gerenciada para o ADF como um usuário de banco de dados capaz de criar o SSISDB.

- Opcionalmente, ingresse seu Azure-SSIS IR em uma rede virtual/configure o IR auto-hospedado como um proxy para que seu Azure-SSIS IR acesse dados no local.

## <a name="overview"></a>Visão geral

Este artigo mostra diferentes maneiras de provisionar um Azure-SSIS IR:

- [Portal do Azure](#azure-portal)
- [PowerShell do Azure](#azure-powershell)
- [Modelo do Azure Resource Manager](#azure-resource-manager-template)

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Assinatura do Azure**. Se você ainda não tiver uma assinatura, crie uma conta de [avaliação gratuita](https://azure.microsoft.com/pricing/free-trial/).
- **Servidor/instância gerenciada do banco de dados SQL do Azure (opcional)** . Se ainda não tiver um servidor de banco de dados, crie um no portal do Azure antes de começar. O ADF criará o SSISDB neste servidor de banco de dados. É recomendável que você crie o servidor de banco de dados na mesma região do Azure que a do Integration Runtime. Essa configuração permite que o Integration Runtime grave logs de execução do SSISDB sem cruzar regiões do Azure. 
    - Com base no servidor de banco de dados selecionado, o SSISDB pode ser criado em seu nome como um banco de dados individual, parte de um pool elástico ou em uma Instância Gerenciada e acessível na rede pública ou ingressando em uma rede virtual. Para obter orientações sobre como escolher o tipo de servidor de banco de dados para hospedar o SSISDB, confira [Comparar bancos de dados individuais/pools elásticos do Banco de Dados SQL do Azure/Instância Gerenciada](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Se você usar o servidor de banco de dados SQL do Azure com pontos de extremidade de serviço de rede virtual/Instância Gerenciada com um ponto de extremidades privado para hospedar o SSISDB ou exigir acesso a dados locais sem configurar o IR auto-hospedado, você precisará unir seu Azure-SSIS IR a uma rede virtual, consulte [ingressar Azure-SSIS ir em uma rede virtual](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).
    - Confirme se a configuração **Permitir acesso aos serviços do Azure** está habilitada para o servidor de banco de dados. Isso não é aplicável quando você usa o servidor de banco de dados SQL do Azure com pontos de extremidade de serviço de rede virtual/Instância Gerenciada com um ponto de extremidades privado para hospedar o SSISDB. Para saber mais, confira [Proteger seu banco de dados SQL do Azure](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). Para habilitar essa configuração usando o PowerShell, veja [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).
    - Adicione o endereço IP do computador cliente ou um intervalo de endereços IP que inclua o endereço IP do computador cliente, à lista de endereços IP do cliente nas configurações do firewall para o servidor de banco de dados. Para saber mais, confira [Regras de firewall no nível do servidor e no nível do banco de dados do Banco de Dados SQL do Azure](../sql-database/sql-database-firewall-configure.md).
    - Você pode se conectar ao servidor de banco de dados usando a autenticação do SQL com suas credenciais de administrador de servidor ou autenticação do AAD (Azure Active Directory) com a identidade gerenciada para seu ADF.  Para o último, você precisa adicionar a identidade gerenciada para o ADF em um grupo do AAD com permissões de acesso para o servidor de banco de dados, consulte [habilitar a autenticação do AAD para Azure-SSIS ir](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir).
    - Confirme se seu servidor de banco de dados ainda não tem um SSISDB. O provisionamento do Azure-SSIS IR não oferece suporte ao uso de um SSISDB existente.
- **Rede virtual do Azure Resource Manager (opcional)** . Você precisa ter uma rede virtual do Azure Resource Manager se pelo menos uma das seguintes condições for verdadeira:
    - Você está hospedando o SSISDB no servidor de banco de dados SQL do Azure com pontos de extremidade de serviço de rede virtual/Instância Gerenciada com um ponto de extremidades privado.
    - Você deseja se conectar a armazenamentos de dados locais de pacotes do SSIS em execução no seu Azure-SSIS IR sem configurar o IR via hospedagem interna.
- **Azure PowerShell (opcional)** . Siga as instruções sobre [como instalar e configurar Azure PowerShell](/powershell/azure/install-az-ps), se você quiser executar um script do PowerShell para provisionar seus Azure-SSIS ir.

### <a name="region-support"></a>Suporte de regiões

Para obter uma lista de regiões do Azure nas quais o ADF e o Azure-SSIS IR estão disponíveis, confira [Disponibilidade do ADF + SSIS IR por região](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all).

### <a name="compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance"></a>Comparar o pool elástico/banco de dados individual do Banco de Dados SQL e a Instância Gerenciada do Banco de Dados SQL

A tabela abaixo compara alguns recursos do servidor do Banco de Dados SQL do Azure e da Instância Gerenciada com relação ao Azure-SSIR IR:

| Recurso | banco de dados individual/pool elástico| Instância Gerenciada |
|---------|--------------|------------------|
| **Agendamento** | O SQL Server Agent não está disponível.<br/><br/>Confira [Agendar a execução de um pacote no pipeline do ADF](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-schedule-packages?view=sql-server-2017#activity).| O Agente da Instância Gerenciada está disponível. |
| **Autenticação** | Você pode criar o SSISDB com um usuário de banco de dados independente que representa qualquer grupo do AAD com a identidade gerenciada de seu ADF como membro na função **db_owner**.<br/><br/>Confira [Habilitar a autenticação do Azure AD para criar o SSISDB no servidor do Banco de Dados SQL do Azure](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database). | Você pode criar o SSISDB com um usuário de banco de dados independente que representa a identidade gerenciada de seu ADF. <br/><br/>Confira [Habilitar a autenticação do Azure AD para criar o SSISDB na Instância Gerenciada do Banco de Dados SQL do Azure](enable-aad-authentication-azure-ssis-ir.md#enable-azure-ad-on-azure-sql-database-managed-instance). |
| **Camada de serviço** | Ao criar o Azure-SSIS IR no servidor do Banco de Dados SQL do Azure, você pode selecionar o tipo de serviço para o SSISDB. Há várias camadas de serviço. | Ao criar o Azure-SSIS IR com sua Instância Gerenciada, você não pode selecionar a camada de serviço para SSISDB. Todos os bancos de dados em sua Instância Gerenciada compartilham o mesmo recurso alocado a essa instância. |
| **Rede virtual** | O dá suporte somente a Azure Resource Manager redes virtuais para o Azure-SSIS IR ingressar se você usar o servidor de banco de dados SQL do Azure com pontos de extremidade de serviço de rede virtual ou exigir acesso a armazenamentos de dados locais sem configurar o IR via hospedagem interna. | Dá suporte apenas para redes virtuais do Azure Resource Manager para ingresso de seu Azure-SSIS IR. A rede virtual é necessária quando você não habilita um ponto de extremidade público para sua Instância Gerenciada.<br/><br/>Se você associar seu Azure-SSIS IR à mesma rede virtual da sua Instância Gerenciada, verifique se o seu Azure-SSIS IR está em uma sub-rede diferente da sua Instância Gerenciada. Se você ingressar seu Azure-SSIS IR em uma rede virtual diferente da sua Instância Gerenciada, recomendamos um emparelhamento da rede virtual ou uma conexão entre redes virtuais. Consulte [Conecte seu aplicativo à Instância Gerenciada do Banco de Dados SQL do Azure](../sql-database/sql-database-managed-instance-connect-app.md). |
| **Transações distribuídas** | Possui suporte por meio de Transações Elásticas. As transações do Microsoft Distributed Transaction Coordinator (MSDTC) não possuem suporte. Se os pacotes do SSIS usarem MSDTC para coordenar transações distribuídas, considere a migração para Transações Elásticas para o Banco de Dados SQL do Azure. Para obter mais informações, consulte [Transações distribuídas entre bancos de dados na nuvem](../sql-database/sql-database-elastic-transactions-overview.md). | Não compatível. |
| | | |

## <a name="azure-portal"></a>Portal do Azure

Nesta seção, use o portal do Azure, especificamente a interface do usuário do ADF, para criar o Azure-SSIS IR.

### <a name="create-a-data-factory"></a>Criar um data factory

Para criar o ADF por meio do portal do Azure, siga as instruções detalhadas em [criar ADF por meio do artigo da interface do usuário](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory) e selecione **fixar no painel** enquanto faz isso para permitir o acesso rápido após sua criação. 

Quando o ADF for criado, abra sua página de visão geral no portal do Azure e clique no bloco **criar & monitor** para iniciar sua página de **introdução** em uma guia separada, em que você pode continuar a criar seu Azure-SSIS ir.   

### <a name="provision-an-azure-ssis-integration-runtime"></a>Provisionar um tempo de execução de integração do Azure SSIS

1. Na página **de introdução,** clique no bloco **configurar o SSIS Integration Runtime** .

   ![Configurar o bloco do Integration Runtime SSIS](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

2. Na página **Configurações Gerais** da **Instalação do Integration Runtime**, execute estas etapas:

   ![Configurações gerais](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

    a. Para **Name**, insira o nome do seu tempo de execução de integração.

    b. Para **Descrição**, insira a descrição do seu tempo de execução de integração.

    c. Para **Local**, selecione o local do seu tempo de execução de integração. Somente os locais com suporte são exibidos. Recomendamos que você selecione o mesmo local do seu servidor de banco de dados para hospedar o SSISDB.

    d. Para **Tamanho do Nó**, selecione o tamanho do nó no cluster de tempo de execução de integração. Apenas tamanhos de nós suportados são exibidos. Selecione um tamanho de nó grande (scale up), se você quiser executar muitos pacotes de computação / memória intensivos.

    e. Para **Número do Nó**, selecione o número de nós em seu cluster de tempo de execução de integração. Somente os números de nós com suporte são exibidos. Selecione um grande cluster com muitos nós (scale out), se você deseja executar muitos pacotes em paralelo.

    f. Para **Edition/License**, selecione a edição/licença do SQL Server para seu tempo de execução de integração: Standard ou Enterprise. Selecione Enterprise se você quiser usar os recursos avançados/premium em seu tempo de execução de integração.

    g. Para **Economizar**, selecione a opção do Benefício Híbrido do Azure (AHB) para o tempo de execução de integração: Sim ou Não. Selecione Sim se você quiser colocar sua própria licença do SQL Server com o Software Assurance para aproveitar a redução de custos com o uso híbrido.

    h. Clique em **Próximo**.

3. Na página **Configurações de SQL**, execute as seguintes etapas:

   ![Configurações do SQL](./media/tutorial-create-azure-ssis-runtime-portal/sql-settings.png)

    a. Na caixa de seleção **Criar catálogo SSIS...** , selecione o modelo de implantação dos pacotes a serem executados no Azure-SSIS IR: O Modelo de Implantação de Projetos em que os pacotes são implantados no SSISDB hospedado pelo servidor de banco de dados ou o Modelo de Implantação de Pacotes em que os pacotes são implantados em seus sistemas de arquivos/compartilhamentos de arquivos/Arquivos do Azure. Se você fizer isso, precisará trazer seu próprio servidor de banco de dados para hospedar o SSISDB que criaremos e gerenciaremos em seu nome.
   
    b. Para **Assinatura**, selecione a assinatura do Azure que tem o servidor de banco de dados para hospedar o SSISDB. 

    c. Para **Local**, selecione o local do seu servidor de banco de dados para hospedar o SSISDB. É recomendável que você selecione o mesmo local do seu tempo de execução de integração. 

    d. Para **Ponto de extremidade de servidor de banco de dados de catálogo**, selecione o ponto de extremidade do seu servidor de banco de dados para hospedar o SSISDB. Com base no servidor de banco de dados selecionado, o SSISDB pode ser criado em seu nome como um banco de dados individual, parte de um pool elástico ou em uma Instância Gerenciada e acessível na rede pública ou ingressando em uma rede virtual. Para obter orientações sobre como escolher o tipo de servidor de banco de dados para hospedar o SSISDB, confira [Comparar bancos de dados individuais/pools elásticos do Banco de Dados SQL do Azure/Instância Gerenciada](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-single-databaseelastic-pool-and-sql-database-managed-instance). Se você selecionar servidor de banco de dados SQL do Azure com pontos de extremidade de serviço de rede virtual/Instância Gerenciada com um ponto de extremidades privado para hospedar o SSISDB ou exigir acesso a dados locais sem configurar o IR auto-hospedado, você precisará unir seu Azure-SSIS IR a uma rede virtual , consulte [ingressar Azure-SSIS ir em uma rede virtual](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). 

    e. Na caixa de seleção **Usar autenticação do AAD...** , selecione o método de autenticação para o servidor de banco de dados para hospedar o SSISDB: Autenticação do SQL ou a autenticação do AAD com a identidade gerenciada para o ADF. Se você verificá-lo, precisará adicionar a identidade gerenciada para o ADF em um grupo do AAD com permissões de acesso ao seu servidor de banco de dados, consulte [habilitar a autenticação do AAD para Azure-SSIS ir](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). 

    f. Para **Nome de usuário do administrador**, insira o nome de usuário de autenticação do SQL para o servidor de banco de dados para hospedar o SSISDB. 

    g. Para **Nome de usuário do administrador**, insira a senha de autenticação do SQL para o servidor de banco de dados para hospedar o SSISDB. 

    h. Para **Nível do Serviço de Banco de Dados de Catálogo**, selecione a camada de serviço para o seu servidor de banco de dados hospedar SSISDB: Tipo Basic/Standard/Premium ou o nome do pool elástico. 

    i. Clique em **Conexão de teste** e, se tiver êxito, clique em **Próximo**. 

4. Na página **Configurações avançadas**, conclua as etapas a seguir:

    ![Configurações avançadas](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

    a. Para **Execuções paralelas máximas por nó**, selecione o número máximo de pacotes para executar simultaneamente por nó no cluster de tempo de execução de integração. Apenas os números de pacote suportados são exibidos. Selecione um número baixo, se você quiser usar mais de um núcleo para executar um único pacote grande / pesado que seja de computação / memória -intensivo. Selecione um número alto, se você quiser executar um ou mais pacotes pequenos / leves em um único núcleo.

    b. Para o  **Custom Storage Container SAS URI**, insira opcionalmente o Uniform Resource Identifier (URI) de Assinatura de Acesso Compartilhado (SAS) do seu contêiner Azob Storage Blob onde seu script de instalação e seus arquivos associados estão armazenados, consulte [ Configuração personalizada para o Azure-SSIS IR ](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

5. Na caixa de seleção **Selecionar uma rede virtual ...** , selecione se você deseja unir seu tempo de execução de integração a uma rede virtual. Verifique se você usa o servidor de banco de dados SQL do Azure com pontos de extremidade de serviço de rede virtual/Instância Gerenciada com um ponto de extremidades privado para hospedar o SSISDB ou exigir acesso a dados locais, ou seja, você tem fontes/destinos de dados locais em seus pacotes do SSIS, sem Configurando o IR auto-hospedado, consulte [ingressar Azure-SSIS ir em uma rede virtual](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network). Se você verificar, conclua as etapas a seguir:

   ![Configurações avançadas com rede virtual](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

    a. Para **Assinatura**, selecione a assinatura do Azure que possui sua rede virtual.

    b. Para **Local**, o mesmo local do seu tempo de execução de integração é selecionado.

    c. Para **Tipo**, selecione o tipo da sua rede virtual: Clássico ou Azure Resource Manager. Recomendamos que você selecione a rede virtual do Azure Resource Manager, pois a rede virtual clássica será descontinuada em breve.

    d. Para **Nome da VNet**, selecione o nome da sua rede virtual. Essa rede virtual deve ser a mesma usada para o servidor de banco de dados SQL do Azure com pontos de extremidade de serviço de rede virtual/Instância Gerenciada em uma rede virtual para hospedar o SSISDB ou aquele conectado à sua rede local.

    e. Para **Nome da sub-rede**, selecione o nome da sub-rede da sua rede virtual. Isso deve ser uma sub-rede diferente daquela usada para Instância Gerenciada em uma rede virtual para hospedar o SSISDB.

6. Na caixa de seleção **Configurar auto-hospedado...** , selecione se deseja configurar o infravermelho auto-hospedado como um proxy para o seu Azure-SSIS ir, consulte Configurar o ir para o [auto-hospedado como um proxy](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis). Se você verificar, conclua as etapas a seguir:

   ![Configurações avançadas com o IR auto-hospedado](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-shir.png)

    a. Para **Integration Runtime auto-hospedados**, selecione o ir auto-hospedado existente como um proxy para Azure-SSIS ir.

    b. Para o **serviço vinculado de armazenamento de preparo**, selecione o serviço vinculado do armazenamento de BLOBs do Azure existente ou crie um novo para preparo.

    c. Para o **caminho de preparo**, especifique um contêiner de blob no armazenamento de BLOBs do Azure selecionado ou deixe-o vazio para usar um padrão para preparo.

7. Clique em **validação de VNet** e em **Avançar**. 

8. Na página **Resumo** , examine todas as configurações de provisionamento, marque os links de documentação recomendados e clique em **concluir** para iniciar a criação do tempo de execução de integração.

    > [!NOTE]
    > Excluindo qualquer hora de instalação personalizada, esse processo deve ser concluído em 5 minutos, mas pode levar aproximadamente 20-30 minutos para Azure-SSIS IR ingressar em uma rede virtual.
    >
    > Se você usa o SSISDB, o serviço ADF conecta-se ao seu servidor de banco de dados para prepará-lo. Ele também configura permissões e configurações para sua rede virtual, se especificado, e une seu Azure-SSIS IR à rede virtual.
    > 
    > Quando você provisiona o Azure-SSIS IR, o Azure Feature Pack para SSIS e o Access Redistribuível também são instalados. Esses componentes fornecem conectividade para arquivos do Excel/Access e para várias fontes de dados do Azure, além das fontes de dados que já são compatíveis com os componentes internos. Também é possível instalar componentes adicionais. Confira [Instalação personalizada para Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

7. Na janela **Conexões**, alterne para **Tempos de Execução de Integração**, se necessário. Clique em **Atualizar** para atualizar o status.

   ![Status de criação](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-creation-status.png)

8. Use os links na coluna **Ações** para parar/iniciar, editar ou excluir o tempo de execução de integração. Use o último link para exibir o código JSON para o tempo de execução de integração. Os botões editar e excluir são habilitados somente quando o IR é interrompido.

   ![IR do Azure SSIS - ações](./media/tutorial-create-azure-ssis-runtime-portal/azure-ssis-ir-actions.png)

### <a name="azure-ssis-integration-runtimes-in-the-portal"></a>Tempos de execução de integração do Azure SSIS no portal

1. Na interface do usuário do Azure Data Factory, alterne para a guia **Editar**, clique em **Conexões** e, em seguida, alterne para a guia **Tempos de Execução de Integração** para exibir os tempos de execução de integração existentes no data factory.

   ![Exibir IRs existentes](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

2. Clique em **Novo** para criar um novo IR do Azure SSIS.

   ![Tempo de execução de integração por meio do menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

3. Para criar um tempo de execução de integração do Azure SSIS, clique em **Novo** conforme mostrado na imagem.

4. Na janela Instalação do Integration Runtime, selecione **Pacotes SSIS existentes de lift-and-shift para execução no Azure** e, em seguida, clique em **Avançar**.

   ![Especificar o tipo de tempo de execução de integração](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

5. Consulte a seção [Provisionar um tempo de execução de integração do Azure SSIS](#provision-an-azure-ssis-integration-runtime) para o restante das etapas para configurar um IR do Azure-SSIS.

## <a name="azure-powershell"></a>Azure PowerShell

Nesta seção, você utiliza o Azure PowerShell para criar um IR do Azure-SSIS.

### <a name="create-variables"></a>Criar variáveis

Copie e cole o script a seguir, especifique os valores para as variáveis. 

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database server with virtual network service endpoints/Managed Instance with a private endpoint/on-premises data without configuring Self-Hosted IR, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used for your Azure SQL Database server with virtual network service endpoints or a different subnet than the one used for your Managed Instance with a private endpoint

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.DNS prefix.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"
```

### <a name="sign-in-and-select-subscription"></a>Entrar e selecionar a assinatura

Adicione o seguinte código ao script para entrar e selecione sua assinatura do Azure:

```powershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName
```

### <a name="validate-the-connection-to-database-server"></a>Validar a conexão ao servidor de banco de dados

Adicione o script a seguir para validar o servidor/Instância Gerenciada do banco de dados SQL do Azure.

```powershell
# Validate only if you use SSISDB and do not use VNet or AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
    {
        if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
        {
            $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword
            $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
            Try
            {
                $sqlConnection.Open();
            }
            Catch [System.Data.SqlClient.SqlException]
            {
                Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
                Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
                $yn = Read-Host
                if(!($yn -ieq "Y"))
                {
                    Return;
                }
            }
        }
    }
}
```

### <a name="configure-virtual-network"></a>Configurar rede virtual

Adicione o script a seguir para configurar automaticamente as permissões/configurações de rede virtual para o ingresso do seu Integration Runtime do Azure-SSIS.

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-a-resource-group"></a>Criar um grupo de recursos

Crie um [grupo de recursos do Azure](../azure-resource-manager/resource-group-overview.md) usando o comando [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Um grupo de recursos é um contêiner lógico no qual os recursos do Azure são implantados e gerenciados em grupo.

Se seu grupo de recursos já existe, não copie este código ao seu script. 

```powershell
New-AzResourceGroup -Location $DataFactoryLocation -Name $ResourceGroupName
```

### <a name="create-a-data-factory"></a>Criar um data factory

Execute o comando a seguir para criar um data factory.

```powershell
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName
```

### <a name="create-an-integration-runtime"></a>Criar um Integration Runtime

Execute os seguintes comandos para criar um tempo de execução de integração do Azure-SSIS que executa pacotes do SSIS no Azure.

Se você não usar o SSISDB, poderá omitir os parâmetros CatalogServerEndpoint, CatalogPricingTier e CatalogAdminCredential.

Se você não usar o servidor de banco de dados SQL do Azure com pontos de extremidade de serviço de rede virtual/Instância Gerenciada com um ponto de extremidades privado para hospedar o SSISDB ou exigir acesso a dados locais, poderá omitir os parâmetros VNetId e Subnet ou passar valores vazios para eles. Você também pode omiti-los se configurar o IR auto-hospedado como um proxy para o Azure-SSIS IR acessar dados no local. Caso contrário, você não poderá omiti-los e deverá passar os valores válidos de sua configuração de rede virtual, consulte [ Unir o Azure-SSIS IR a uma rede virtual ](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).

Se você usar a instância gerenciada para hospedar SSISDB, você pode omitir o parâmetro CatalogPricingTier ou passar um valor vazio para ele. Caso contrário, você não poderá omiti-lo e deverá passar um valor válido na lista de níveis de preços suportados para o Banco de Dados SQL do Azure, consulte [ Limites de recursos do Banco de Dados SQL ](../sql-database/sql-database-resource-limits.md).

Se você usar a autenticação do Azure Active Directory (AAD) com a identidade gerenciada para o ADF se conectar ao servidor de banco de dados, poderá omitir o parâmetro CatalogAdminCredential, mas deverá adicionar a identidade gerenciada do ADF a um grupo do AAD com acesso permissões para o servidor de banco de dados, consulte [habilitar autenticação do AAD para Azure-SSIS ir](https://docs.microsoft.com/azure/data-factory/enable-aad-authentication-azure-ssis-ir). Caso contrário, você não pode emiti-lo ou deve passar um objeto válido formado pelo nome de usuário e senha de administrador de servidor para autenticação SQL.

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
       
# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add CatalogAdminCredential parameter if you do not use AAD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add SetupScriptContainerSasUri parameter if you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}
```

### <a name="start-integration-runtime"></a>Iniciar o Integration Runtime

Execute o comando a seguir para iniciar o Integration Runtime do Azure-SSIS.

```powershell
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

> [!NOTE]
> Excluindo qualquer hora de instalação personalizada, esse processo deve ser concluído em 5 minutos, mas pode levar aproximadamente 20-30 minutos para Azure-SSIS IR ingressar em uma rede virtual.
>
> Se você usa o SSISDB, o serviço ADF conecta-se ao seu servidor de banco de dados para prepará-lo. Ele também configura permissões e configurações para sua rede virtual, se especificado, e une seu Azure-SSIS IR à rede virtual.
> 
> Quando você provisiona o Azure-SSIS IR, o Azure Feature Pack para SSIS e o Access Redistribuível também são instalados. Esses componentes fornecem conectividade para arquivos do Excel/Access e para várias fontes de dados do Azure, além das fontes de dados que já são compatíveis com os componentes internos. Também é possível instalar componentes adicionais. Confira [Instalação personalizada para Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

### <a name="full-script"></a>Script completo

Aqui está o script completo que cria um tempo de execução de integração do Azure-SSIS.

```powershell
### Azure Data Factory information
# If your input contains a PSH special character, e.g. "$", precede it with the escape character "`" like "`$"
$SubscriptionName = "[your Azure subscription name]"
$ResourceGroupName = "[your Azure resource group name]"
# Data factory name - Must be globally unique
$DataFactoryName = "[your data factory name]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$DataFactoryLocation = "EastUS"

### Azure-SSIS integration runtime information - This is a Data Factory compute resource for running SSIS packages
$AzureSSISName = "[your Azure-SSIS IR name]"
$AzureSSISDescription = "[your Azure-SSIS IR description]"
# For supported regions, see https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all
$AzureSSISLocation = "EastUS"
# For supported node sizes, see https://azure.microsoft.com/pricing/details/data-factory/ssis/
$AzureSSISNodeSize = "Standard_D8_v3"
# 1-10 nodes are currently supported
$AzureSSISNodeNumber = 2
# Azure-SSIS IR edition/license info: Standard or Enterprise
$AzureSSISEdition = "Standard" # Standard by default, while Enterprise lets you use advanced/premium features on your Azure-SSIS IR
# Azure-SSIS IR hybrid usage info: LicenseIncluded or BasePrice
$AzureSSISLicenseType = "LicenseIncluded" # LicenseIncluded by default, while BasePrice lets you bring your own on-premises SQL Server license with Software Assurance to earn cost savings from Azure Hybrid Benefit (AHB) option
# For a Standard_D1_v2 node, up to 4 parallel executions per node are supported, but for other nodes, up to (2 x number of cores) are currently supported
$AzureSSISMaxParallelExecutionsPerNode = 8
# Custom setup info
$SetupScriptContainerSasUri = "" # OPTIONAL to provide SAS URI of blob container where your custom setup script and its associated files are stored
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use Azure SQL Database server with virtual network service endpoints/Managed Instance with a private endpoint/on-premises data without configuring Self-Hosted IR, Azure Resource Manager virtual network is recommended, Classic virtual network will be deprecated soon
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Please use the same subnet as the one used with your Azure SQL Database server with virtual network service endpoints or a different subnet than the one used for your Managed Instance with a private endpoint

### SSISDB info
$SSISDBServerEndpoint = "[your Azure SQL Database server name.database.windows.net or Managed Instance name.DNS prefix.database.windows.net or Managed Instance name.public.DNS prefix.database.windows.net,3342 or leave it empty if you do not use SSISDB]" # WARNING: If you use SSISDB, please ensure that there is no existing SSISDB on your database server, so we can prepare and manage one on your behalf
# Authentication info: SQL or Azure Active Directory (AAD)
$SSISDBServerAdminUserName = "[your server admin username for SQL authentication or leave it empty for AAD authentication]"
$SSISDBServerAdminPassword = "[your server admin password for SQL authentication or leave it empty for AAD authentication]"
# For the basic pricing tier, specify "Basic", not "B" - For standard/premium/elastic pool tiers, specify "S0", "S1", "S2", "S3", etc., see https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server
$SSISDBPricingTier = "[Basic|S0|S1|S2|S3|S4|S6|S7|S9|S12|P1|P2|P4|P6|P11|P15|…|ELASTIC_POOL(name = <elastic_pool_name>) for Azure SQL Database server or leave it empty for Managed Instance]"

### Sign in and select subscription
Connect-AzAccount
Select-AzSubscription -SubscriptionName $SubscriptionName

### Validate the connection to database server
# Validate only if you use SSISDB and do not use VNet or AAD authentication
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    if([string]::IsNullOrEmpty($VnetId) -and [string]::IsNullOrEmpty($SubnetName))
    {
        if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) -and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword))
        {
            $SSISDBConnectionString = "Data Source=" + $SSISDBServerEndpoint + ";User ID=" + $SSISDBServerAdminUserName + ";Password=" + $SSISDBServerAdminPassword
            $sqlConnection = New-Object System.Data.SqlClient.SqlConnection $SSISDBConnectionString;
            Try
            {
                $sqlConnection.Open();
            }
            Catch [System.Data.SqlClient.SqlException]
            {
                Write-Warning "Cannot connect to your Azure SQL Database server, exception: $_";
                Write-Warning "Please make sure the server you specified has already been created. Do you want to proceed? [Y/N]"
                $yn = Read-Host
                if(!($yn -ieq "Y"))
                {
                    Return;
                }
            }
        }
    }
}

### Configure virtual network
# Make sure to run this script against the subscription to which the virtual network belongs
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}

### Create a data factory
Set-AzDataFactoryV2 -ResourceGroupName $ResourceGroupName `
                         -Location $DataFactoryLocation `
                         -Name $DataFactoryName

### Create an integration runtime
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Description $AzureSSISDescription `
                                           -Type Managed `
                                           -Location $AzureSSISLocation `
                                           -NodeSize $AzureSSISNodeSize `
                                           -NodeCount $AzureSSISNodeNumber `
                                           -Edition $AzureSSISEdition `
                                           -LicenseType $AzureSSISLicenseType `
                                           -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
       
# Add CatalogServerEndpoint, CatalogPricingTier, and CatalogAdminCredential parameters if you use SSISDB
if(![string]::IsNullOrEmpty($SSISDBServerEndpoint))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    if(![string]::IsNullOrEmpty($SSISDBServerAdminUserName) –and ![string]::IsNullOrEmpty($SSISDBServerAdminPassword)) # Add CatalogAdminCredential parameter if you do not use AAD authentication
    {
        $secpasswd = ConvertTo-SecureString $SSISDBServerAdminPassword -AsPlainText -Force
        $serverCreds = New-Object System.Management.Automation.PSCredential($SSISDBServerAdminUserName, $secpasswd)

        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                   -DataFactoryName $DataFactoryName `
                                                   -Name $AzureSSISName `
                                                   -CatalogAdminCredential $serverCreds
    }
}

# Add SetupScriptContainerSasUri parameter when you use custom setup
if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri
}

### Start integration runtime
write-host("##### Starting #####")
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

write-host("##### Completed #####")
write-host("If any cmdlet is unsuccessful, please consider using -Debug option for diagnostics.")
```

## <a name="azure-resource-manager-template"></a>Modelo do Azure Resource Manager

Nesta seção, você usa o modelo do Azure Resource Manager para criar o tempo de execução de integração do Azure-SSIS. Aqui é apresentado um passo a passo de exemplo:

1. Crie um arquivo JSON com o seguinte modelo do Azure Resource Manager. Substitua valores nos colchetes angulados (espaços reservados) com seus próprios valores.

    ```json
    {
      "contentVersion": "1.0.0.0",
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {},
        "variables": {},
        "resources": [{
            "name": "<Specify a name for your data factory>",
            "apiVersion": "2018-06-01",
            "type": "Microsoft.DataFactory/factories",
            "location": "East US",
            "properties": {},
            "resources": [{
                "type": "integrationruntimes",
                "name": "<Specify a name for your Azure-SSIS IR>",
                "dependsOn": [ "<The name of the data factory you specified at the beginning>" ],
                "apiVersion": "2018-06-01",
                "properties": {
                    "type": "Managed",
                    "typeProperties": {
                        "computeProperties": {
                            "location": "East US",
                            "nodeSize": "Standard_D8_v3",
                            "numberOfNodes": 1,
                            "maxParallelExecutionsPerNode": 8
                        },
                        "ssisProperties": {
                            "catalogInfo": {
                                "catalogServerEndpoint": "<Azure SQL Database server name>.database.windows.net",
                                "catalogAdminUserName": "<Azure SQL Database server admin username>",
                                "catalogAdminPassword": {
                                    "type": "SecureString",
                                    "value": "<Azure SQL Database server admin password>"
                                },
                                "catalogPricingTier": "Basic"
                            }
                        }
                    }
                }
            }]
        }]
    }
    ```

2. Para implantar o modelo de Azure Resource Manager, execute o comando New-AzResourceGroupDeployment, conforme mostrado no exemplo a seguir, em que ADFTutorialResourceGroup é o nome do seu grupo de recursos e ADFTutorialARM. JSON é o arquivo que contém a definição de JSON para seu data factory e Azure-SSIS IR.

    ```powershell
    New-AzResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json
    ```

    Esse comando cria sua fábrica de dados e o IR do Azure-SSIS, mas não inicia o IR.

3. Para iniciar seu Azure-SSIS IR, execute o comando Start-AzDataFactoryV2IntegrationRuntime:

    ```powershell
    Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName "<Resource Group Name>" `
                                                 -DataFactoryName "<Data Factory Name>" `
                                                 -Name "<Azure SSIS IR Name>" `
                                                 -Force
    ```

> [!NOTE]
> Excluindo qualquer hora de instalação personalizada, esse processo deve ser concluído em 5 minutos, mas pode levar aproximadamente 20-30 minutos para Azure-SSIS IR ingressar em uma rede virtual.
>
> Se você usa o SSISDB, o serviço ADF conecta-se ao seu servidor de banco de dados para prepará-lo. Ele também configura permissões e configurações para sua rede virtual, se especificado, e une seu Azure-SSIS IR à rede virtual.
> 
> Quando você provisiona o Azure-SSIS IR, o Azure Feature Pack para SSIS e o Access Redistribuível também são instalados. Esses componentes fornecem conectividade para arquivos do Excel/Access e para várias fontes de dados do Azure, além das fontes de dados que já são compatíveis com os componentes internos. Também é possível instalar componentes adicionais. Confira [Instalação personalizada para Azure-SSIS IR](how-to-configure-azure-ssis-ir-custom-setup.md).

## <a name="deploy-ssis-packages"></a>Implantar pacotes do SSIS

Se você usa o SSISDB, é possível implantar seus pacotes nele e executá-lo no Azure-SSIS IR usando ferramentas SSDT/SSMS que conectam seu servidor de banco de dados por meio do ponto de extremidade do servidor.  Para o servidor/instância gerenciada do banco de dados SQL do Azure com um ponto de extremidade privado/instância gerenciada com um ponto `<server name>.database.windows.net`de extremidade público, o formato do ponto de extremidade do servidor é / / `<server name>.<dns prefix>.database.windows.net` `<server name>.public.<dns prefix>.database.windows.net,3342`, respectivamente. Se você não usa o SSISDB, é possível implantar seus pacotes em sistemas de arquivos/compartilhamentos de arquivos/Arquivos do Azure e executá-los no Azure-SSIS IR com os utilitários de linha de comando `dtinstall`/`dtutil`/`dtexec`. Para saber mais, confira [Implantar pacotes do SSIS](/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages#deploy-packages-to-integration-services-server). Em ambos os casos, você também pode executar pacotes implantados no Azure-SSIS IR com a atividade Executar o pacote SSIS em pipelines ADF. Confira [Chamar a execução de pacotes SSIS como uma atividade ADF de primeira classe](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

## <a name="next-steps"></a>Próximas etapas

Consulte também outros tópicos Azure-SSIS IR nesta documentação:

- [Tempo de execução de integração do Azure-SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime). Este artigo fornece informações sobre os tempos de execução de integração em geral, incluindo Azure-SSIS IR.
- [Azure-SSIS ir de monitor](monitor-integration-runtime.md#azure-ssis-integration-runtime). Este artigo mostra como recuperar e compreender as informações sobre seu Azure-SSIS IR.
- [Gerenciar Azure-SSIS ir](manage-azure-ssis-integration-runtime.md). Este artigo mostra como parar, iniciar ou excluir seu Azure-SSIS IR-ele também mostra como escalar horizontalmente o Azure-SSIS IR adicionando mais nós.
- [Ingresse Azure-SSIS ir em uma rede virtual](join-azure-ssis-integration-runtime-virtual-network.md). Este artigo fornece informações sobre como unir seu Azure-SSIS IR a uma rede virtual.