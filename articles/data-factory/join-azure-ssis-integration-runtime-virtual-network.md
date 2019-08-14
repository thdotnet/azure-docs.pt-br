---
title: Unir o tempo de execução de integração do Azure-SSIS a uma rede virtual | Microsoft Docs
description: Saiba como unir o tempo de execução de integração do Azure-SSIS a uma rede virtual do Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/12/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 49422d73a63f1bcde267aac3a9b75e9977970cc9
ms.sourcegitcommit: acffa72239413c62662febd4e39ebcb6c6c0dd00
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/12/2019
ms.locfileid: "68951921"
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Unir o tempo de execução de integração do Azure-SSIS a uma rede virtual
Ao usar o SQL Server Integration Services (SSIS) no Azure Data Factory (ADF), você deve ingressar seu IR (Integration Runtime do Azure-SSIS) em uma rede virtual do Azure nos seguintes cenários: 

- Você deseja se conectar a armazenamentos de dados locais de pacotes do SSIS em execução no IR do Azure-SSIS sem configurar/gerenciar um IR auto-hospedado como proxy. 

- Você está hospedando o SSISDB (banco de dados de catálogo do SSIS) no banco de dados SQL do Azure com pontos de extremidade de serviço de rede virtual/Instância Gerenciada em uma rede virtual. 

O ADF permite unir o IR do Azure-SSIS a uma rede virtual criada por meio do modelo de implantação clássico ou do modelo de implantação de Azure Resource Manager. 

> [!IMPORTANT]
> No momento, a rede virtual clássica está sendo reprovada, portanto, use a rede virtual do Azure Resource Manager.  Se você já usa a rede virtual clássica, mude para usar a rede virtual do Azure Resource Manager assim que possível.

## <a name="access-to-on-premises-data-stores"></a>Acessar armazenamentos de dados locais
Se seus pacotes SSIS acessarem somente armazenamentos de dados de nuvem pública, você não precisará unir o IR do Azure-SSIS a uma rede virtual. Se os pacotes do SSIS acessarem os armazenamentos de dados locais, você poderá ingressar o IR do Azure-SSIS em uma rede virtual que esteja conectada à rede local ou configurar/gerenciar um IR do modo auto-hospedado como proxy para o IR do Azure-SSIS, consulte [Configurar o ir auto-hospedado como um artigo sobre proxy para IR do Azure-SSIS](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis) . Ao unir seu IR do Azure-SSIS a uma rede virtual, há alguns pontos importantes a serem observados: 

- Se não houver uma rede virtual conectada existente à sua rede local, primeiro crie uma [rede virtual do Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network) ou uma [rede virtual clássica](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) para o tempo de execução de integração do Azure-SSIS a unir. Em seguida, configure uma [conexão de gateway de VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) site a site ou conexão [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) dessa rede virtual à sua rede local. 

- Se houver um Azure Resource Manager existente ou uma rede virtual clássica conectada à sua rede local no mesmo local do IR do Azure-SSIS, será possível unir o IR nessa rede virtual. 

- Se houver uma rede virtual clássica existente conectada à rede local em um local diferente do IR do Azure-SSIS, será possível primeiro criar uma [rede virtual clássica](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) para o IR do Azure-SSIS a unir. Em seguida, configure uma conexão entre [redes virtuais clássicas](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md). Ou você poderá criar uma [rede virtual do Azure Resource Manager](../virtual-network/quick-create-portal.md#create-a-virtual-network) para o tempo de execução de integração do Azure-SSIS a unir. Em seguida, configure uma conexão entre [rede virtual clássica e Azure Resource Manager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md). 
 
- Se houver uma rede virtual do Azure Resource Manager existente conectada à sua rede local em um local diferente do IR do Azure-SSIS, você poderá primeiro criar uma [rede virtual do Azure Resource Manager](../virtual-network/quick-create-portal.md##create-a-virtual-network) para unir o IR do Azure-SSIS a ela. Em seguida, configure uma conexão de rede virtual do Azure Resource Manager para o Azure Resource Manager. Ou você pode criar uma [rede virtual clássica](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) para unir o IR do Azure-SSIS a ela. Em seguida, configure uma conexão entre [rede virtual clássica e Azure Resource Manager](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md). 

## <a name="host-the-ssis-catalog-database-in-azure-sql-database-with-virtual-network-service-endpointsmanaged-instance"></a>Hospedar o banco de dados do Catálogo SSIS no Banco de Dados SQL do Azure com pontos de extremidade de serviço de rede virtual/Instância Gerenciada
Se o catálogo do SSIS estiver hospedado no banco de dados SQL do Azure com pontos de extremidade de serviço de rede virtual ou Instância Gerenciada em uma rede virtual, você poderá unir seu IR do Azure-SSIS para: 

- A mesma rede virtual 
- Uma rede virtual diferente que tenha uma conexão de rede a rede com aquela usada para a Instância Gerenciada 

Se você hospedar seu catálogo do SSIS no Banco de Dados SQL do Azure com pontos de extremidade de serviço de rede virtual, não deixe de associar o seu IR do Azure-SSIS à mesma rede virtual e sub-rede.

Se você associar seu IR do Azure-SSIS à mesma rede virtual da Instância Gerenciada, verifique se o IR do Azure-SSIS está em uma sub-rede diferente da Instância Gerenciada. Se você associar seu IR do Azure-SSIS a uma rede virtual diferente da Instância Gerenciada, recomendamos o emparelhamento de rede virtual (que é limitado à mesma região) ou uma rede virtual à conexão de rede virtual. Consulte [Conecte seu aplicativo à Instância Gerenciada do Banco de Dados SQL do Azure](../sql-database/sql-database-managed-instance-connect-app.md).

Em todos os casos, a rede virtual só pode ser implantada por meio do modelo de implantação do Azure Resource Manager.

As seções a seguir apresentam mais detalhes. 

## <a name="requirements-for-virtual-network-configuration"></a>Salvando a configuração de rede virtual
-   Verifique se `Microsoft.Batch` é um provedor registrado sob a assinatura da sua sub-rede da rede virtual que hospeda o IR de SSIS do Azure. Se você estiver usando a rede virtual clássica, una também `MicrosoftAzureBatch` para a função de Colaborador clássico de máquina Virtual para essa rede virtual. 

-   Certifique-se de ter as permissões necessárias. Consulte [Permissões necessárias](#perms).

-   Selecione a sub-rede correta para hospedar o IV de SSIS do Azure. Selecione a [Sub-rede](#subnet). 

-   Se você estiver usando seu próprio servidor de Serviço de Nomes de Domínio (DNS) em uma rede virtual, consulte [servidor de Serviço de Nomes de Domínio](#dns_server). 

-   Se você estiver usando um Grupo de Segurança de Rede (NSG) na sub-rede, consulte [Grupo de segurança de rede](#nsg) 

-   Se você estiver usando o Azure ExpressRoute ou configurando a UDR (Rota Definida pelo Usuário), consulte [Usa Azure ExpressRoute ou Rota Definido pelo Usuário](#route). 

-   Verifique se o grupo de recursos da rede virtual pode criar e excluir determinados recursos de Rede do Azure. Consulte [Requisitos para o Grupo de Recursos](#resource-group). 

-   Se você personalizar o IR do Azure-SSIS conforme descrito no artigo [configuração personalizada para ir do Azure-](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup) SSIS, seus nós ir do Azure-SSIS serão alocados endereços IP privados de um intervalo predefinido de 172.16.0.0-172.31.255.255. portanto, verifique se o IP privado os intervalos de endereços de suas redes virtuais/locais não colidem com esse intervalo.

Aqui está um diagrama que mostra as conexões necessárias para seu Azure-SSIS IR:

![IR Azure-SSIS](media/join-azure-ssis-integration-runtime-virtual-network/azure-ssis-ir.png)

### <a name="perms"></a> Permissões necessárias

O usuário que cria o Azure-SSIS Integration Runtime deve ter as seguintes permissões:

- Se você ingressar no seu SSIS IR em uma rede virtual do Azure Resource Manager, terá duas opções:

  - Use a função interna de *Colaborador de Rede*. Essa função vem com a permissão _Microsoft.Network/\*_ , que tem um escopo muito maior do que o necessário.

  - Crie uma função personalizada que inclua apenas a permissão _Microsoft.Network/virtualNetworks/\*/join/action_. 

- Se você estiver ingressando em seu SSIS IR em uma rede virtual clássica, recomendamos usar a função *integrante do Virtual Machine* interna. Caso contrário, você terá que definir uma função personalizada que inclua a permissão para ingressar na rede virtual.

### <a name="subnet"></a>Selecione a Sub-rede.
-   Não selecione GatewaySubnet para implantar um Microsoft Integration Runtime do Azure-SSIS, porque ele é dedicado para gateways de rede virtual. 

-   Verifique se a sub-rede que você selecionar tem espaço de endereço disponível suficiente para o IR do Azure-SSIS usar. Deixe pelo menos 2 * número de nós do IR em endereços IP disponíveis. O Azure reserva alguns endereços IP em cada sub-rede, os quais não podem ser usados. O primeiro e o último endereço IP das sub-redes são reservados para fins de conformidade de protocolo, juntamente com três outros endereços usados para os serviços do Azure. Para saber mais, veja [Existem restrições quanto ao uso de endereços IP dentro dessas sub-redes?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets). 

-   Não use uma sub-rede que está ocupada exclusivamente por outros serviços do Azure (por exemplo, Instância Gerenciada do Banco de Dados SQL do Azure, Serviço de Aplicativo, etc.). 

### <a name="dns_server"></a>Servidor de Serviços de Nome de Domínio 
Se você precisar usar seu próprio servidor DNS (serviços de nome de domínio) em uma rede virtual unida pelo tempo de execução de integração do Azure-SSIS, verifique se ele pode resolver nomes de host do Azure globais (por exemplo, `<your storage account>.blob.core.windows.net`um nome de blob de armazenamento do Azure,). 

As seguintes etapas são recomendadas: 

-   Configurar o DNS personalizado para encaminhar solicitações para o DNS do Microsoft Azure. Você pode encaminhar os registros DNS não resolvidos para o endereço IP de resolvedores de recursiva do Microsoft Azure (168.63.129.16) em seu próprio servidor DNS. 

-   Configurar o DNS personalizado como primário e o DNS do Azure como secundário para a rede virtual. Registre o endereço IP de resolvedores de recursiva do Microsoft Azure (168.63.129.16) como um servidor DNS secundário no caso de seu próprio servidor DNS não estar disponível. 

Para obter mais informações, consulte [Resolução de nome usando seu próprio servidor DNS](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server). 

### <a name="nsg"></a>Grupo de Segurança de Rede
Se for necessário implementar um NSG (grupo de segurança de rede) para a sub-rede usada pelo Azure-SSIS Integration Runtime, permita tráfego de entrada/saída através das seguintes portas: 

| Direction | Protocolo de transporte | Origem | Intervalo de portas de origem | Destino | Intervalo de portas de destino | Comentários |
|---|---|---|---|---|---|---|
| Entrada | TCP | BatchNodeManagement | * | VirtualNetwork | 29876, 29877 (se você unir o IR a uma rede virtual do Azure Resource Manager) <br/><br/>10100, 20100, 30100 (se você unir o IR a uma rede virtual clássica)| Os serviços do Azure Data Factory usam essas portas para comunicarem-se com os nós de seu tempo de execução de integração do Azure-SSIS na rede virtual. <br/><br/> Se você criar um NSG no nível de sub-rede ou não, o Data Factory sempre configurará um NSG no nível dos adaptadores de rede anexados às máquinas virtuais que hospedam o Azure-SSIS IR. Somente o tráfego de entrada dos endereços IP do Data Factory nas portas especificadas é permitido pelo NSG no nível do adaptador de rede. Mesmo se você abrir essas portas para o tráfego da Internet no nível de sub-rede, o tráfego de endereços IP que não sejam endereços IP do Data Factory será bloqueado no nível do adaptador de rede. |
| Saída | TCP | VirtualNetwork | * | AzureCloud<br/>(ou um escopo mais amplo, como a Internet) | 443 | Os nós do tempo de execução de integração do Azure-SSIS na rede virtual usam essa porta para acessar os serviços do Azure, como o Armazenamento do Microsoft Azure e Hubs de Eventos do Azure. |
| Saída | TCP | VirtualNetwork | * | Internet | 80 | Os nós de seu tempo de execução de integração do Azure-SSIS na rede virtual usam essa porta para baixar a lista de revogação de certificados da Internet. |
| Saída | TCP | VirtualNetwork | * | Sql<br/>(ou um escopo mais amplo, como a Internet) | 1433, 11000-11999 | Os nós de seu tempo de execução de integração do Azure-SSIS na rede virtual usam essas portas para acessar o SSISDB hospedado pelo servidor do Banco de Dados SQL do Azure. Se a política de conexão do servidor do banco de dados SQL do Azure estiver definida como **proxy** em vez de redirecionar, somente a porta 1433 será necessária. Essa regra de segurança de saída não é aplicável ao SSISDB hospedado pelo seu Instância Gerenciada na rede virtual. |
||||||||

### <a name="route"></a> Usar o Azure ExpressRoute ou Rota Definida pelo Usuário
Você pode conectar um circuito do [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) à sua infraestrutura de rede virtual para estender a rede local até o Azure. 

Uma configuração comum é usar o túnel forçado (anunciar um roteamento BGP, 0.0.0.0/0 para a rede virtual) que força o tráfego de Internet de saída do fluxo da rede virtual para o dispositivo da rede local para inspeção e registro em log. Este fluxo de tráfego quebra a conectividade entre o IR do Azure-SSIS na rede virtual com os serviços dependentes do Azure Data Factory. A solução é definir uma (ou mais) [UDRs (rotas definidas pelo usuário)](../virtual-network/virtual-networks-udr-overview.md) na sub-rede que contém o IR do Azure-SSIS. Uma UDR define rotas específicas de sub-rede que são consideradas no lugar da rota BGP. 

Ou você pode definir as rotas definidas pelo usuário (UDRs) para forçar o tráfego de Internet de saída da sub-rede que hospeda o IR do Azure-SSIS para outra sub-rede, que hospeda o IR do Azure-SSIS como firewall ou uma hospedagem DMZ para inspeção e registro em log. 

Em ambos os casos, aplique uma rota 0.0.0.0/0 com o tipo de próximo salto como **Internet** na sub-rede que hospeda o IV SSIS do Azure, para que a comunicação entre o serviço de fábrica de dados e o IR do Azure SSIS pode ter êxito. 

![Adicionar uma rota](media/join-azure-ssis-integration-runtime-virtual-network/add-route-for-vnet.png)

Se você estiver preocupado em perder a capacidade de inspecionar o tráfego de Internet de saída dessa sub-rede, também poderá adicionar uma regra NSG na sub-rede para restringir os destinos de saída para [Endereços IP do data center do Azure](https://www.microsoft.com/download/details.aspx?id=41653). 

Veja [este script do PowerShell](https://gallery.technet.microsoft.com/scriptcenter/Adds-Azure-Datacenter-IP-dbeebe0c) para obter um exemplo. Você precisa executar o script semanalmente para manter atualizada a lista de endereços IP do data center do Azure. 

### <a name="resource-group"></a>Requisitos para o Grupo de Recursos
-   O Azure-SSIS IR precisa criar determinados recursos de rede no mesmo grupo de recursos que a rede virtual. Esses recursos incluem o seguinte:
    -   Um Azure Load Balancer, com o nome  *\<GUID >-azurebatch-cloudserviceloadbalancer*.
    -   Um endereço IP público do Azure, com o nome  *\<GUID >-azurebatch-cloudservicepublicip*.
    -   Um grupo de segurança de trabalho de rede, com o nome  *\<GUID >-azurebatch-cloudservicenetworksecuritygroup*. 

-   Certifique-se de que você não tem nenhum bloqueio de recurso no grupo de recursos ou assinatura à qual pertence a rede virtual. Se você configurar um bloqueio somente leitura ou um bloqueio de exclusão, iniciar e parar o IR poderá falhar ou parar de responder. 

-   Certifique-se de que você não tem uma política do Azure que impede que os recursos a seguir sejam criados sob o grupo de recursos ou a assinatura à qual pertence a rede virtual: 
    -   Microsoft.Network/LoadBalancers 
    -   Microsoft.Network/NetworkSecurityGroups 
    -   Microsoft.Network/PublicIPAddresses 

## <a name="azure-portal-data-factory-ui"></a>Portal do Azure (Interface do usuário do Data Factory)
Esta seção mostra como unir um tempo de execução do Azure-SSIS existente a uma rede virtual (clássica ou do Azure Resource Manager) usando o Portal do Azure e a interface do usuário do Data Factory. Primeiro, é necessário configurar a rede virtual corretamente antes de unir o IR do Azure-SSIS a ela. Percorra uma das próximas duas seções com base no tipo de sua rede virtual (clássica ou Azure Resource Manager). Em seguida, continue na terceira seção para unir o IR do Azure-SSIS à rede virtual. 

### <a name="use-the-portal-to-configure-an-azure-resource-manager-virtual-network"></a>Usar o portal para configurar uma rede virtual do Azure Resource Manager
Primeiro você precisa configurar a rede virtual antes de adicionar um IR do Azure-SSIS a ela. 

1. Inicie o Microsoft Edge ou o Google Chrome. Atualmente, a interface do usuário do Data Factory tem suporte apenas nesses navegadores da Web. 

1. Entre no [Portal do Azure](https://portal.azure.com). 

1. Escolha **Mais serviços**. Filtre e selecione **Redes virtuais**. 

1. Filtre e selecione sua rede virtual na lista. 

1. Na página **Rede virtual**, selecione **Propriedades**. 

1. Selecione o botão de cópia para que a **ID DO RECURSO** copie a ID do recurso da rede virtual para a área de transferência. Salve a ID da área de transferência no OneNote ou em um arquivo. 

1. Selecione **Sub-redes** no menu esquerdo. Certifique-se de que o número de **endereços disponíveis** seja maior do que os nós no tempo de execução de integração do Azure-SSIS. 

1. Verifique se o provedor do Lote do Azure está registrado na assinatura do Azure que possui a rede virtual. Ou, registre o fornecedor do Lote do Azure. Se você já tiver uma conta de lote do Azure em sua assinatura, sua assinatura está registrada para o lote do Azure. (Se você criar o IR do Azure-SSIS no portal do Azure Data Factory,  o provedor do lote do Microsoft Azure é registrado automaticamente para você.) 

   a. No Portal do Azure, selecione **Assinaturas** no menu esquerdo. 

   b. Selecione sua assinatura. 

   c. Selecione **Provedores de recurso** à esquerda e confirme se **Microsoft.Batch** é um provedor registrado. 

   ![Confirmação do status "Registrado"](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Se você não visualizar **Microsoft.Batch** na lista, para registrá-lo [crie uma conta do Lote do Azure vazia](../batch/batch-account-create-portal.md) na sua assinatura. Você pode excluí-lo depois. 

### <a name="use-the-portal-to-configure-a-classic-virtual-network"></a>Usar o portal para configurar uma rede virtual clássica
Primeiro você precisa configurar a rede virtual antes de adicionar um IR do Azure-SSIS a ela. 

1. Inicie o Microsoft Edge ou o Google Chrome. Atualmente, a interface do usuário do Data Factory tem suporte apenas nesses navegadores da Web. 

1. Entre no [Portal do Azure](https://portal.azure.com). 

1. Escolha **Mais serviços**. Filtre e selecione **Redes virtuais (clássicas)** . 

1. Filtre e selecione sua rede virtual na lista. 

1. Na página **Rede virtual (clássica)** , selecione **Propriedades**. 

   ![ID do recurso de rede virtual clássica](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)

1. Selecione o botão de cópia para que **ID DO RECURSO** copie a ID do recurso da rede clássica para a área de transferência. Salve a ID da área de transferência no OneNote ou em um arquivo. 

1. Selecione **Sub-redes** no menu esquerdo. Certifique-se de que o número de **endereços disponíveis** seja maior do que os nós no tempo de execução de integração do Azure-SSIS. 

   ![Número de endereços disponíveis na rede virtual](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)

1. Una **MicrosoftAzureBatch** à função **Colaborador da Máquina Virtual Clássica** para a rede virtual. 

    a. Selecione **Controle de Acesso (IAM)** no menu esquerdo e selecione a guia **Atribuições de função**. 

    !["Controle de Acesso" e botões "Adicionar"](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

    b. Selecione **Adicionar atribuição de função**.

    c. Na página **Adicionar atribuição de função**, selecione **Colaborador da Máquina Virtual Clássica** para **Função**. Cole **ddbf3205-c6bd-46ae-8127-60eb93363864** na caixa **Selecionar** e, em seguida, selecione **Lote do Microsoft Azure** da lista de resultados de pesquisa. 

    ![Resultados da pesquisa na página "Adicionar atribuição de função"](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

    d. Selecione **Salvar** para salvar as configurações e fechar a página. 

    ![Salvar as configurações de acesso](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

    e. Confirme se você visualiza o **Lote do Microsoft Azure** na lista de colaboradores. 

    ![Confirme o acesso ao lote do Azure](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

1. Verifique se o provedor do Lote do Azure está registrado na assinatura do Azure que possui a rede virtual. Ou, registre o fornecedor do Lote do Azure. Se você já tiver uma conta de lote do Azure em sua assinatura, sua assinatura está registrada para o lote do Azure. (Se você criar o IR do Azure-SSIS no portal do Azure Data Factory,  o provedor do lote do Microsoft Azure é registrado automaticamente para você.) 

   a. No Portal do Azure, selecione **Assinaturas** no menu esquerdo. 

   b. Selecione sua assinatura. 

   c. Selecione **Provedores de recurso** à esquerda e confirme se **Microsoft.Batch** é um provedor registrado. 

   ![Confirmação do status "Registrado"](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Se você não visualizar **Microsoft.Batch** na lista, para registrá-lo [crie uma conta do Lote do Azure vazia](../batch/batch-account-create-portal.md) na sua assinatura. Você pode excluí-lo depois. 

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>Unir o IR do Azure-SSIS a uma rede virtual
1. Inicie o Microsoft Edge ou o Google Chrome. Atualmente, a interface do usuário do Data Factory tem suporte apenas nesses navegadores da Web. 

1. No [portal do Azure](https://portal.azure.com), selecione **Data factories** no menu à esquerda. Se não visualizar **Fábricas de dados** no menu, selecione **Mais serviços** e, em seguida, selecione **Fábricas de dados** na seção **INTELIGÊNCIA + ANÁLISE**. 

   ![Lista de fábricas de dados](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)

1. Selecione o data factory com o tempo de execução de integração do Azure-SSIS na lista. Você verá a home page do seu data factory. Selecione o bloco **Criar & Implantar**. Você visualiza a interface do usuário do Data Factory em uma guia separada. 

   ![Página inicial do data factory](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)

1. Na interface de usuário do Data Factory, alterne para a guia **Editar**, selecione **Conexões** e alterne para a guia **Tempos de execução de integração**. 

   ![Guia "tempos de execução de integração"](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)

1. Se estiver executando o IR do Azure-SSIS, na lista de tempo de execução de integração, selecione o botão **Parar** na coluna **Ações** para o IR do Azure-SSIS. Você não pode editar um IR até que ela tenha sido interrompida. 

   ![Parar o IR](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)

1. Na lista do tempo de execução de integração, selecione o botão **Editar** na coluna **Ações** para o IR do Azure-SSIS. 

   ![Editar o tempo de execução de integração](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)

1. No painel **de configuração do Integration Runtime** , avance pelas páginas **configurações gerais** e **configurações do SQL** clicando no botão **Avançar** . 

1. Na página **Configurações Avançadas**, faça as ações a seguir: 

   a. Marque a caixa de seleção **selecionar uma VNet...** . 

   b. Para **assinatura**, selecione sua assinatura do Azure sob a qual você pode, por sua vez, selecionar uma rede virtual existente. 
  
   c. Para **Nome da VNET**, selecione a rede virtual. 

   d. Para **Nome da Sub-rede**, selecione a sub-rede na rede virtual. 

   e. Se você também quiser configurar/gerenciar um IR auto-hospedado como proxy para o IR do Azure-SSIS, marque a caixa de seleção configurar o auto-hospedado **...** e consulte [Configurar o ir auto-hospedado como um proxy para o artigo ir do Azure-SSIS](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis) .

   f. Clique no botão **validação de VNet** e, se for bem-sucedido, clique no botão **Avançar** . 

   ![Configurações avançadas para configuração do IR](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)

1. Na página **Resumo** , examine todas as configurações do ir do Azure-SSIS e clique no botão **Atualizar** .

1. Agora, você pode iniciar o IR do Azure-SSIS clicando no botão **Iniciar** na coluna **ações** para o ir do Azure-SSIS. Leva aproximadamente de 20 a 30 minutos para iniciar o IR do Azure-SSIS que une uma rede virtual. 

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="configure-a-virtual-network"></a>Configurar uma rede virtual
É necessário configurar uma rede virtual antes de poder ingressar no IR do Azure-SSIS. Para configurar automaticamente as permissões/configurações da rede virtual para o tempo de execução de integração do Azure-SSIS unir a rede virtual, adicione o script a seguir:

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
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

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Criar um IR do Azure-SSIS e uni-lo a uma rede virtual
Você pode criar um IR do Azure-SSIS e uni-lo a uma rede virtual ao mesmo tempo. Para obter as instruções e o script completo, consulte [Criar um tempo de execução de integração do Azure-SSIS](create-azure-ssis-integration-runtime.md#azure-powershell).

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Unir um IR do Azure-SSIS existente a uma rede virtual
O script no artigo [Criar um tempo de execução de integração do Azure-SSIS](create-azure-ssis-integration-runtime.md) mostra como criar um IR do Azure-SSIS e uni-lo a uma rede virtual no mesmo script. Se você tiver um IR do Azure-SSIS existente, execute as seguintes etapas para uni-lo à rede virtual: 
1. Interrompa o IR do Azure-SSIS. 
1. Configure o IR do Azure-SSIS para unir à rede virtual. 
1. Inicie o IR do Azure-SSIS. 

### <a name="define-the-variables"></a>Defina as variáveis
```powershell
$ResourceGroupName = "<your Azure resource group name>"
$DataFactoryName = "<your Data Factory name>" 
$AzureSSISName = "<your Azure-SSIS IR name>"
# Specify the information about your classic or Azure Resource Manager virtual network.
$VnetId = "<your Azure virtual network resource ID>"
$SubnetName = "<the name of subnet in your virtual network>"
```

### <a name="stop-the-azure-ssis-ir"></a>Interrompa o IR do Azure-SSIS
Pare o tempo de execução de integração do Azure-SSIS antes de uni-lo a uma rede virtual. Esse comando libera todos os nós e para a cobrança:

```powershell
Stop-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Force 
```

### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Configurar as configurações de rede virtual para o IR do Azure-SSIS a unir
```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
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
        # Assign VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="configure-the-azure-ssis-ir"></a>Configure o IR do Azure-SSIS
Para configurar o tempo de execução de integração do Azure-SSIS para unir à rede virtual, execute o comando `Set-AzDataFactoryV2IntegrationRuntime`: 

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Type Managed `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>Inicie o IR do Azure-SSIS
Para iniciar o tempo de execução de integração do Azure-SSIS, execute o comando a seguir: 

```powershell
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```

Esse comando demora de 20 a 30 minutos para concluir.

## <a name="next-steps"></a>Próximas etapas
Para obter mais informações sobre o tempo de execução de integração do Azure-SSIS, consulte os seguintes tópicos: 
- [Tempo de execução de integração do Azure-SSIS](concepts-integration-runtime.md#azure-ssis-integration-runtime). Este artigo fornece informações conceituais sobre os tempos de execução de integração em geral, incluindo o IR do Azure-SSIS. 
- [Tutorial: implantar pacotes do SSIS para o Azure](tutorial-create-azure-ssis-runtime-portal.md). Este tutorial fornece instruções passo a passo para criar seu IR do Azure-SSIS. Ele usa o Banco de Dados SQL do Azure para hospedar o catálogo do SSIS. 
- [Criar um Integration Runtime do Azure-SSIS](create-azure-ssis-integration-runtime.md). Este artigo expande o tutorial e fornece instruções sobre como usar o banco de dados SQL do Azure com pontos de extremidade de serviço de rede virtual/Instância Gerenciada em uma rede virtual para hospedar o catálogo do SSIS e ingressar o IR do Azure-SSIS em uma rede virtual. 
- [Monitore um IR do Azure-SSIS](monitor-integration-runtime.md#azure-ssis-integration-runtime). Este artigo mostra como recuperar informações sobre o IR do Azure-SSIS e fornece descrições de status nas informações retornadas. 
- [Gerencie um IR do Azure-SSIS](manage-azure-ssis-integration-runtime.md). Este artigo mostra como parar, iniciar ou excluir seu IR do Azure-SSIS. Adicionalmente, mostra como escalar horizontalmente o IR do Azure-SSIS adicionando nós.
