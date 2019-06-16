---
title: Criar e configurar clusters do pacote de segurança empresarial no Azure HDInsight
description: Saiba como criar e configurar clusters do pacote de segurança empresarial no Azure HDInsight
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: howto
ms.date: 05/09/2019
ms.openlocfilehash: 5b5b83fe0028e43ca35bf883b29cb71bad6ca2c8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66253121"
---
# <a name="create-and-configure-enterprise-security-package-clusters-in-azure-hdinsight"></a>Criar e configurar clusters do pacote de segurança empresarial no Azure HDInsight

O pacote de segurança de Enterprise para HDInsight do Azure fornece acesso a autenticação baseada no Active Directory, suporte a vários usuários e controle de acesso baseado em função para os clusters do Apache Hadoop no Azure. Clusters de HDInsight ESP permitem que as organizações, que aderem às políticas de segurança corporativa estrita, para processar dados confidenciais com segurança.

O objetivo deste guia está configurado corretamente os recursos necessários para que local os usuários podem entrar um ESP habilitado o cluster HDInsight. Este artigo explica as etapas necessárias para criar um Cluster HDInsight do Azure do pacote de segurança empresarial habilitado. As etapas abordará a criação de uma VM IaaS do Windows com o Active Directory e serviços DNS (Domain Name) habilitado. Este servidor atuará como uma substituição para o seu **real** do ambiente local e permitem que você continue com as etapas de instalação e configuração para que você possa repeti-los posteriormente em seu próprio ambiente. Este guia irá ajudá-lo a criar um ambiente de identidade híbrida usando a sincronização de hash de senha com o Azure Active Directory.

Este guia destina-se para complementar [usar pacote de segurança Enterprise no HDInsight](apache-domain-joined-architecture.md)

Antes de usar esse processo em seu próprio ambiente, configure o Active Directory e serviços de nome de domínio (DNS). Além disso, habilite o Azure Active Directory e a sincronização de contas de usuário local ao Azure Active Directory.

![diagrama da arquitetura](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image002.png)

## <a name="create-on-premises-environment"></a>Criar um ambiente local

Visão geral: Nesta seção, você usará um modelo de implantação rápida do Azure para criar novas VMs, configurar serviços de nome de domínio (DNS) e uma nova floresta do AD.

1. Vá para [criar uma VM do Azure com uma nova floresta do AD](https://azure.microsoft.com/resources/templates/active-directory-new-domain/), para exibir o modelo de implantação rápida.

1. Clique em **implantar no Azure**.
1. Entre em sua assinatura do Azure.
1. Sobre o **criar uma VM do Azure com uma nova floresta do AD** tela, conclua as seguintes etapas:
    1. Selecione a assinatura onde você deseja que os recursos implantados do **assinatura** lista suspensa.
    1. Selecione **criar novo** lado **grupo de recursos** e insira o nome **OnPremADVRG**
    1. Insira os seguintes detalhes para o restante dos campos do modelo:

        * **Localização**: Centro dos EUA
        * **Nome de Usuário do Administrador**: HDIFabrikamAdmin
        * **Senha de administrador**: < YOUR_PASSWORD >
        * **Domínio**: HDIFabrikam.com
        * **Prefixo DNS**: hdifabrikam

        ![Modelo criar VM do Azure e a floresta do AD](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-azure-vm-ad-forest.png)

    1. Clique em **compra**
    1. Monitorar a implantação e aguarde até que ela seja concluída.
    1. Confirme se os recursos são criados sob o grupo de recursos correto `OnPremADVRG`.

## <a name="configure-users-and-groups-for-cluster-access"></a>Configurar usuários e grupos para acesso ao cluster

Visão geral: Nesta seção, você criará os usuários que terão acesso ao cluster HDInsight no final deste guia.

1. Conecte-se ao controlador de domínio usando a área de trabalho remota.
    1. Se você usou o modelo mencionado no início, o controlador de domínio é uma VM chamada **adVM** no `OnPremADVRG` grupo de recursos.
    1. Acesse o portal do Azure > **grupos de recursos** > **OnPremADVRG** > **adVM** > **Connect**.
    1. Clique o **RDP** guia e, em seguida, clique em **baixar arquivo RDP**.
    1. Salve o arquivo em seu computador e abri-lo.
    1. Quando solicitado a fornecer credenciais, use `HDIFabrikam\HDIFabrikamAdmin` como o nome de usuário e, em seguida, digite a senha que você escolheu para a conta de administrador.

1. Depois que sua sessão de área de trabalho remota é aberta no VM do controlador de domínio, inicie **Active Directory Users and Computers** da **Gerenciador do servidor** painel. Clique em **ferramentas** no canto superior direito e, em seguida **Active Directory Users and Computers** na lista suspensa.

    ![Gerenciamento de diretório do Active Directory do Gerenciador do servidor abrir](./media/apache-domain-joined-create-configure-enterprise-security-cluster/server-manager-active-directory-screen.png)

1. Crie dois novos usuários **HDIAdmin**, **HDIUser**. Esses dois usuários serão usados para entrar em clusters de HDInsight.

    1. No **Active Directory Users and Computers** tela, clique em **ação** > **novo** > **usuário**.

        ![Criar novo usuário do Active Directory](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-user.png)

    1. No **novo objeto - usuário** tela, insira `HDIUser` como o **nome de logon do usuário** e clique em **próximo**.

        ![Criar o primeiro usuário administrador](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image020.png)

    1. No popup que aparece, digite a senha desejada para a nova conta. Marque a caixa que diz **a senha nunca expira**. HDIClick **Okey**.
    1. Clique em **concluir** para criar a nova conta.
    1. Crie outro usuário `HDIAdmin`.

        ![Criar o segundo usuário administrador](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image024.png)

1. No **Active Directory Users and Computers** tela, clique em **ação** > **novo** > **grupo**. Criar `HDIUserGroup` como um novo grupo.

    ![Criar novo grupo do Active Directory](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-group.png)

    ![Criar novo grupo de 2](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image028.png)

1. Adicione a **HDIUser** criado na etapa anterior para o **HDIUserGroup** como um membro.

    1. Clique com botão direito do **HDIUserGroup** e clique em **propriedades**.
    1. Vá para **membros** guia e clique em **Add**.
    1. ENTER `HDIUser` na caixa rotulada **inserir os nomes de objeto a serem selecionados** e clique em **Okey**.
    1. Repita as etapas anteriores para a outra conta `HDIAdmin`

        ![Adicionar membros ao grupo](./media/apache-domain-joined-create-configure-enterprise-security-cluster/active-directory-add-users-to-group.png)

Agora você criou seu ambiente do Active Directory, juntamente com dois usuários e um grupo de usuários para acessar o cluster HDInsight.

Esses usuários serão sincronizados com o Azure AD.

### <a name="create-a-new-azure-active-directory"></a>Criar um novo Azure Active Directory

1. Entre no Portal do Azure.
1. Clique em **criar um recurso** e digite **diretório**. Selecione **do Azure Active Directory** > **criar**.
1. Insira **HDIFabrikam** sob **nome da organização**.
1. Insira **HDIFabrikamoutlook** sob **nome de domínio inicial**.
1. Clique em **Criar**.
1. À esquerda no portal do Azure, clique em **Azure Active Directory**.
1. Se necessário, clique em **mudar diretório** para alterar para o novo diretório em que você criou **HDIFabrikamoutlook**.
1. Sob **Manage** clique em **nomes de domínio personalizados** > **Adicionar domínio personalizado**.
1. Insira **HDIFabrikam.com** sob **nome de domínio personalizado** e clique em **Adicionar domínio**.

![Crie um novo diretório do Active Directory do azure](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-directory.png)

![criar um novo domínio personalizado](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-custom-domain.png)

## <a name="configure-your-azure-ad-tenant"></a>Configurar seu locatário do AD do Azure

Visão geral: Agora você irá configurar seu locatário do AD do Azure para que você possa sincronizar usuários e grupos locais AD para a nuvem.

1. Crie um administrador de locatário do AD.
    1. Entre no portal do Azure e selecione o locatário do Azure AD **HDIFabrikam**
    1. Selecione **os usuários** sob **gerenciar** e, em seguida, **novo usuário**.
    1. Insira os seguintes detalhes para o novo usuário:

        * Name: fabrikamazureadmin
        * Nome de usuário: fabrikamazureadmin@hdifabrikam.com
        * Senha: uma senha de segurança de sua escolha

    1. Clique no **grupos** seção, pesquise por **AAD DC Administrators**e clique em **selecione**.

        ![Grupos](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image038.png)

    1. Clique no **função de diretório** seção e selecione **Administrador Global** no lado direito. Clique em **OK**.

        ![Função do diretório](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image040.png)

    1. Insira uma senha para o usuário. Clique em **Criar**.

1. Se você deseja alterar a senha para o usuário recém-criado <fabrikamazureadmin@hdifabrikam.com>. Entrar no portal do Azure usando que a identidade e, em seguida, você será solicitado a alterar a senha.

## <a name="sync-on-premises-users-to-azure-ad"></a>Sincronizar usuários locais para o Azure AD

### <a name="download-and-install-microsoft-azure-active-directory-connect"></a>Baixe e instale o Microsoft Azure Active Directory connect

1. [Baixar o Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594).

1. Instalação do Microsoft Azure Active Directory connect no controlador de domínio.
    1. Abra o arquivo executável que você baixou na etapa anterior e concorde com os termos de licença. Clique em **Continuar**.

        ![Azure AD Connect](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image052.png)

    1. Clique em **usar configurações expressas** e concluir a instalação.

        ![Usar configurações expressas](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image054.png)

### <a name="configure-sync-with-on-premises-domain-controller"></a>Configurar a sincronização com o controlador de domínio local

1. Sobre o **conectar ao Azure AD** tela, digite o nome de usuário e a senha do administrador global do Azure AD. Clique em **próxima**. Isso é o nome de usuário `fabrikamazureadmin@hdifabrikam.com` que você criou ao configurar seu locatário do AD.
    ![Conecte-se ao Azure AD](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image058.png)
1. Sobre o **conectar-se ao Active Directory Domain Services** tela, insira o nome de usuário e a senha para uma conta de administrador corporativo. Clique em **próxima**. Isso é o nome de usuário `HDIFabrikam\HDIFabrikamAdmin` e sua senha correspondente que você criou anteriormente.

   ![Conectar-se aos serviços de domínio do Active Directory](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image060.png)
1. Sobre o **configuração de entrada do Azure AD** , clique em **próxima**.
    ![Configuração de entrada do AD do Azure](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image062.png)
1. Em pronto para configurar a tela, clique em **instalar**.
    ![install](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image064.png)
1. Quando o **concluir a configuração** tela é exibida, clique em **sair**.
    ![Configuração concluída](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image078.png)

1. Depois que a sincronização seja concluída, confirme se os usuários que você criou no Active Directory IAAS são sincronizados ao Azure Active Directory.
    1. Entre no Portal do Azure.
    1. Selecione **do Azure Active Directory** > **HDIFabrikam** > **usuários**.

### <a name="create-an-user-assigned-managed-identity"></a>Criar uma identidade atribuída pelo usuário gerenciada

Crie uma identidade gerenciada atribuída pelo usuário que será usada para configurar o Azure Active Directory Domain Services (AD do Azure-DS). Para obter mais informações sobre como criar uma identidade atribuída pelo usuário gerenciada, consulte [Create, lista, delete ou atribuir uma função para uma identidade atribuída pelo usuário gerenciada usando o portal do Azure](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md).

1. Entre no Portal do Azure.
1. Clique em **criar um recurso** e digite **identidade gerenciada**. Selecione **usuário atribuído a identidade gerenciada** > **criar**.
1. Insira **HDIFabrikamManagedIdentity** como o **nome do recurso**.
1. Selecione sua assinatura.
1. Sob **grupo de recursos** clique em **criar novo** e insira **HDIFabrikam CentralUS**.
1. Selecione **EUA Central** sob **local**.
1. Clique em **Criar**.

![criar uma nova identidade atribuída pelo usuário gerenciada](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image082.png)

### <a name="enable-azure-active-directory-domain-services"></a>Habilitar o Azure Active Directory Domain Services

Para obter mais informações, consulte [Enable Azure Active Directory Domain Services using the Azure portal](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started) (Habilitar o Azure Active Directory Domain Services usando o Portal do Azure).

1. Crie a rede Virtual ao host do Azure AD DS. Execute o seguinte código do powershell.

    ```powershell
    Connect-AzAccount
    Get-AzSubscription
    Set-AzContext -Subscription 'SUBSCRIPTION_ID'
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS' -Location 'Central US' -Name 'HDIFabrikam-AADDSVNET' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'AADDS-subnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. Entre no Portal do Azure.
1. Clique em **criar recurso**, insira **serviços de domínio** e selecione **Azure AD Domain Services**.
1. Sobre o **Noções básicas de** tela conclua as seguintes etapas:
    1. Sob **nome do diretório** selecione Azure Active Directory criado para este tutorial **HDIFabrikam**.
    1. Insira um **nome de domínio DNS** dos **HDIFabrikam.com**.
    1. Selecione sua assinatura.
    1. Especifique o grupo de recursos **HDIFabrikam CentralUS** e o **local** dos **centro dos EUA**.

        ![detalhes básicos do Azure ad ds](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image084.png)

1. No **rede** completa de tela, selecione a rede (**VNET HDIFabrikam**) e a sub-rede (**AADDS sub-rede**) que você criou com o script do powershell anterior. Ou você pode usar o **criar novo** opção para criar uma rede virtual agora.

    ![Selecionar rede](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image086.png)

1. No **grupo de administradores** tela, você verá uma notificação de que um grupo denominado **AAD DC Administrators** já foi criado para administrar esse grupo. Opcionalmente, você pode modificar a associação desse grupo, mas ele não é necessário para as etapas deste tutorial. Clique em **OK**.

    ![grupo de administrador do modo de exibição](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image088.png)

1. Sobre o **sincronização** tela, habilitar a sincronização completa selecionando **todos os** e, em seguida, clique em **Okey**.

    ![Habilitar a sincronização](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image090.png)

1. Sobre o **resumo** tela, verifique os detalhes para o Azure AD-DS e clique em **Okey**.

    ![Verifique os detalhes de](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image092.png)

1. Depois de habilitar o Azure AD-DS, um servidor DNS (Serviço de Nomes de Domínio) local é executado nas VMs (máquinas virtuais) do AD.

### <a name="configure-your-azure-ad-ds-virtual-network"></a>Configurar sua rede virtual do Azure AD DS

As etapas nesta seção o ajudarão a configurar sua rede virtual do Azure AD DS (**HDIFabrikam AADDSVNET**) para usar os servidores DNS personalizados.

1. Localize os endereços IP dos servidores DNS personalizados. Clique no **HDIFabrikam.com** recursos do AD DS, clique em **propriedades** sob **gerenciar**   e observe os endereços IP listados em **IP Endereço na rede Virtual**.

    ![Localizar endereços de IP de DNS personalizados para o Azure AD DS](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image096.png)

1. Configure **HDIFabrikam AADDSVNET** para o IPs personalizado `10.0.0.4` e `10.0.0.5`.

    1. Selecione **servidores DNS** sob o **configurações** categoria. em seguida, clique no botão de opção ao lado **personalizado**, insira o primeiro endereço IP (10.0.0.4) na caixa de texto abaixo e clique em **salvar**.
    1. Adicione mais endereços IP (10.0.0.5) usando as mesmas etapas.

1. Em nosso cenário do Azure AD DS foi configurado para usar o endereço IP de endereços IP 10.0.0.4 e 10.0.0.5, definindo a mesma vnet AADDS conforme mostrado na imagem abaixo.

    ![Exibir servidores dns personalizados](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image098.png)

## <a name="securing-ldap-traffic"></a>Protegendo o tráfego LDAP

Protocolo de acesso de diretório leve (LDAP) é usado para ler e gravar no Active Directory. Você pode tornar LDAP de tráfego confidencial e seguro usando SSL Secure Sockets Layer () / tecnologia de segurança de camada de transporte (TLS). Você pode habilitar o LDAP sobre SSL (LDAPS) instalando um certificado formatado corretamente.

Para obter mais informações sobre LDAP seguro, consulte [configurar LDAPS (LDAP seguro) para um Azure AD Domain Services o domínio gerenciado](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap).

Nesta seção, você criar um certificado autoassinado, baixe o certificado e configurar o LDAP seguro (LDAPS) para o **hdifabrikam** domínio gerenciado do Azure AD DS.

O script a seguir cria um certificado para hdifabrikam. O certificado é salvo no caminho "LocalMachine".

> [!Note] 
> Qualquer utilitário ou um aplicativo que cria um PKCS válido \#10 solicitação podem ser usados para formar a solicitação de certificado SSL.

```powershell
$lifetime = Get-Date
New-SelfSignedCertificate -Subject hdifabrikam.com `
-NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
-Type SSLServerAuthentication -DnsName *.hdifabrikam.com, hdifabrikam.com
```

Verificar se o certificado está instalado no computador\'repositório pessoal de s. Conclua as seguintes etapas:

1. Inicie o Console de gerenciamento Microsoft (MMC).
1. Adicione o snap-in de certificados que gerencia certificados no computador local.
1. Expandir **certificados (computador Local)** , expanda **pessoais**e, em seguida, expanda **certificados**. Um novo certificado deve existir no repositório pessoal. Esse certificado é emitido para o nome de host totalmente qualificado.

    ![Verifique se a criação de certificado](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image102.png)

1. No painel direito, clique com botão direito no certificado que você criou na etapa anterior, aponte para **todas as tarefas**e, em seguida, clique em **exportar**.

1. Sobre o **Exportar chave privada** , clique em **Sim, exportar a chave privada,** . A chave privada é necessária para a mensagem criptografada a ser lido do computador em que a chave será importada.

    ![exportar chave privada](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image103.png)

1. Sobre o **Exportar formato de arquivo** página, deixe as configurações padrão e, em seguida, clique em **próximo**. 
1. Sobre o **senha** página, digite uma senha para a chave privada, selecione **TripleDES SHA1** para **criptografia** e clique em **Avançar**.
1. Sobre o **arquivo a ser exportado** página, digite o caminho e o nome para o arquivo de certificado exportado e, em seguida, clique em **próximo**.
1. O nome do arquivo tem de ser a extensão. pfx, esse arquivo está configurado no portal do Azure para estabelecer a conexão segura.
1. Habilite o LDAP seguro (LDAPS) para um domínio gerenciado do Azure AD Domain Services.
    1. Selecione o domínio **HDIFabrikam.com** do portal do Azure.
    1. Clique em **LDAP seguro** sob **gerenciar**.
    1. Sobre o **LDAP seguro** tela, clique em **habilitar** sob **LDAP seguro**.
    1. Procure o arquivo de certificado. pfx que você exportou no seu computador.
    1. Insira a senha do certificado.

    ![Habilitar o ldap seguro](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image113.png)

1. Agora que você habilitou o LDAP seguro, verifique se que ele está acessível, permitindo que a porta 636.
    1. Clique no grupo de segurança de rede **AADDS-HDIFabrikam.com-NSG** na **HDIFabrikam CentralUS** grupo de recursos.
    1. Sob **as configurações** clique em **regras de segurança de entrada** > **Add**.
    1. Sobre o **Adicionar regra de segurança de entrada** tela, insira as propriedades a seguir e clique em **Add**:

        | Propriedade | Value |
        |---|---|
        | source | Qualquer |
        | Intervalos de portas de origem | * |
        | Destino | Qualquer |
        | Intervalo de porta de destino | 636 |
        | Protocol | Qualquer |
        | Ação | PERMITIR |
        | Prioridade | <Desired Number> |
        | NOME | Port_LDAP_636 |

    ![regra de segurança de entrada](./media/apache-domain-joined-create-configure-enterprise-security-cluster/add-inbound-security-rule.png)

1. `HDIFabrikamManagedIdentity` é o usuário atribuído a identidade gerenciada, a função de Colaborador de serviços de domínio do HDInsight está habilitada para a identidade gerenciada que permitirá que essa identidade ler, criar, modificar e excluir operações de serviços de domínio.

    ![Criar identidade gerenciada atribuída pelo usuário](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image117.png)

## <a name="creating-enterprise-security-package-enabled-hdinsight-cluster"></a>Criando pacote de segurança empresarial habilitado para cluster do HDInsight

Esta etapa requer os seguintes pré-requisitos:

1. Criar um novo grupo de recursos `HDIFabrikam-WestUS` no local `West US`.
1. Criar uma máquina virtual habilitada de rede que hospedará o ESP cluster HDInsight.

    ```powershell
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS' -Location 'West US' -Name 'HDIFabrikam-HDIVNet' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'SparkSubnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. Criar uma relação de ponto a ponto entre a rede Virtual que está hospedando o AADDS (`HDIFabrikam-AADDSVNET`) e a rede Virtual que irá hospedar o ESP habilitado o cluster HDInsight (`HDIFabrikam-HDIVNet `). Use o seguinte código do powershell para emparelhar essas duas redes virtuais.

    ```powershell
    Add-AzVirtualNetworkPeering -Name 'HDIVNet-AADDSVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS')

    Add-AzVirtualNetworkPeering -Name 'AADDSVNet-HDIVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS')
    ```

1. Criar uma nova conta de armazenamento do Azure Data Lake Gen2 **Hdigen2store**, que é configurado com a identidade do usuário gerenciado **HDIFabrikamManagedIdentity**. Para obter mais informações sobre como criar contas do Data Lake armazenamento Gen2 habilitadas com usuário identidades gerenciadas, consulte [uso do Azure Data Lake armazenamento Gen2 com o Azure HDInsight clusters](../hdinsight-hadoop-use-data-lake-storage-gen2.md).

1. Configurar DNS personalizado na **HDIFabrikam AADDSVNET** rede virtual.
    1. Acesse o portal do Azure > **grupos de recursos** > **OnPremADVRG** > **HDIFabrikam AADDSVNET**  >   **Servidores DNS**.
    1. Selecione **personalizado** e insira `10.0.0.4` e `10.0.0.5`.
    1. Clique em **Salvar**.

        ![Salvar configurações de dns personalizado](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image123.png)

1. Crie um novo cluster Spark do HDInsight ESP habilitado.
    1. Clique em **personalizado (tamanho, configurações, aplicativos)** .
    2. Insira os detalhes desejados para a seção 1 **Noções básicas de**. Certifique-se de que o **tipo de Cluster** é **Spark 2.3 (HDI 3.6)** e o **grupo de recursos** é **HDIFabrikam CentralUS**

    1. Na seção 2 **segurança + rede**, conclua as seguintes etapas:
        1. Clique em **habilitados** sob **pacote de segurança empresarial**.
        1. Clique em **usuário administrador do Cluster** e selecione o **HDIAdmin** conta que você criou anteriormente como o usuário de administrador local. Clique em **Selecionar**.

        1. Clique em **grupo de acesso do Cluster** e, em seguida, selecione **HDIUserGroup**. Qualquer usuário que você adicionar a esse grupo no futuro será capaz de acessar os clusters do HDInsight.

            ![Selecione o grupo de acesso do cluster](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image129.jpg)

    1. Conclua as outras etapas da configuração do cluster e verifique se os detalhes sobre o **resumo do Cluster**. Clique em **Criar**.

1. Entrar para a UI Ambari para o cluster recém-criado durante `https://CLUSTERNAME.azurehdinsight.net` usando seu nome de usuário administrador `hdiadmin@hdifabrikam.com` e a senha.

    ![entre no Ambari](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image135.jpg)

1. Clique em **funções** do painel do cluster.
1. No **funções** , insira o grupo **hdiusergroup** atribuí-lo para o **administrador de Cluster** função sob **atribuir funções a eles**.

    ![Atribuir função de administrador de cluster para hdiusergroup](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image137.jpg)

1. Abra seu cliente SSH e o logon para o cluster usando o **hdiuser** que você criou anteriormente no Active Directory local.

    ![Faça logon no cluster com SSH](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image139.jpg)

Se você for capaz de fazer logon com essa conta, em seguida, você configurou seu cluster ESP corretamente para sincronizar com seu active directory no local.

## <a name="next-steps"></a>Próximas etapas

* [Uma introdução à segurança do Apache Hadoop com Enterprise Security Package](apache-domain-joined-introduction.md)
