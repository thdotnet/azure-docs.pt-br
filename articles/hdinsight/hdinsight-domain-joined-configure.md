---
title: "Configurar clusters HDInsight ingressados no domínio | Microsoft Docs"
description: "Saiba como instalar e configurar clusters HDInsight ingressados no domínio"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
translationtype: Human Translation
ms.sourcegitcommit: 86a0f6f2bc27f1411652b273325e73144582eee0
ms.openlocfilehash: b0122a87ec64d16d6e026f9b37a563125a5f1920


---
# <a name="configure-domain-joined-hdinsight-clusters-preview"></a>Configurar clusters HDInsight ingressados no domínio (Preview)
Saiba como configurar um cluster Azure HDInsight com o Azure Active Directory (Azure AD) e [Apache Ranger](http://hortonworks.com/apache/ranger/) para tirar proveito das políticas de autenticação forte e do RBAC (controle de acesso avançado baseado em função).  O HDInsight ingressado em domínio só pode ser configurado em clusters baseados em Linux. Para obter mais informações, consulte [Introduzir clusters HDInsight ingressados no domínio](hdinsight-domain-joined-introduction.md).

Este artigo é o primeiro tutorial de uma série:

* Crie um cluster HDInsight conectado ao Azure AD (por meio da funcionalidade Active Directory Domain Services) com o Apache Ranger habilitado.
* Crie e aplique políticas de Hive por meio do Apache Ranger e permita que os usuários (por exemplo, os cientistas de dados) se conectem ao Hive usando ferramentas baseadas em ODBC, por exemplo, Excel, Tableau, etc. A Microsoft está trabalhando para adicionar outras cargas de trabalho como Spark, HBase e Storm ao HDInsight ingressado no domínio, em breve.

Um exemplo de topologia final será semelhante ao seguinte:

![Topologia de HDInsight ingressado no domínio](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-topology.png)

Já que o Azure AD dá suporte atualmente apenas a VNets (redes virtuais) clássicas e os clusters HDInsight baseados em Linux dão suporte apenas a VNets com base no Azure Resource Manager, a integração do Azure AD do HDInsight requer duas VNets e um emparelhamento entre elas. Para as informações de comparação entre os dois modelos de implantação, veja [Implantação do Azure Resource Manager versus clássica: compreenda os modelos de implantação e o estado de seus recursos](../azure-resource-manager/resource-manager-deployment-model.md). As duas VNets devem estar na mesma região que o Azure AD DS.

Nomes de serviço do Azure devem ser globalmente exclusivos. Os nomes a seguir são usados neste tutorial. Contoso é um nome fictício. Você deve substituir *contoso* por um nome diferente ao passar pelo tutorial. 

**Nomes:**

| Propriedade | Valor |
| --- | --- |
| VNet do Azure AD |contosoaadvnet |
| Grupo de recursos de VNet do Azure AD |contosoaadrg |
| Máquina virtual do Azure AD (VM) |contosoaadadmin. Essa VM é usada para configurar a unidade de organização e a zona DNS inversa. |
| Diretório do AD do Azure |contosoaaddirectory |
| Nome de domínio do Azure AD |contoso (contoso.onmicrosoft.com) |
| VNet HDInsight |contosohdivnet |
| Grupo de recursos de VNet HDInsight |contosohdirg |
| Cluster HDInsight |contosohdicluster |

Este tutorial fornece as etapas para configurar um cluster HDInsight ingressado no domínio. Cada seção tem links para outros artigos com informações complementares.

## <a name="prerequisite"></a>Pré-requisito:
* Familiarize-se com o [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) e sua estrutura de [preços](https://azure.microsoft.com/pricing/details/active-directory-ds/).
* Certifique-se de que sua assinatura está na lista de permissões para essa visualização pública. Você pode fazer isso enviando um email para hdipreview@microsoft.com com sua ID da assinatura.
* Um certificado SSL assinado por uma autoridade de autenticação para seu domínio. O certificado é necessário para configurar o LDAP seguro. Os certificados autoassinados não podem ser usados.

## <a name="procedures"></a>Procedimentos
1. Crie uma VNet clássica do Azure para o Azure AD.  
2. Crie e configure o Azure AD e o Azure AD DS.
3. Adicione uma VM à VNet clássica para criar a unidade organizacional. 
4. Crie uma unidade organizacional para o Azure AD DS.
5. Crie uma VNet HDInsight no modo de gerenciamento de recursos do Azure.
6. Configure zonas DNS inversas para o Azure AD DS.
7. Emparelhe as duas VNets.
8. Crie um cluster HDInsight.

> [!NOTE]
> Este tutorial presume que você não tenha um Azure AD. Se você tiver um, poderá ignorar a parte contida na etapa 2.
> 
> 

Há um script do PowerShell que automatiza as etapas 3 a 7.  Para obter mais informações, consulte [Configurar clusters HDInsight ingressados em domínio usando o Azure PowerShell](hdinsight-domain-joined-configure-use-powershell.md).

## <a name="create-an-azure-classic-vnet"></a>Criar uma VNet clássica do Azure
Nesta seção, você deve criar uma VNet clássica usando o portal do Azure. Na próxima seção, você pode habilitar o Azure AD DS para o Azure AD na VNet clássica. Para obter mais informações sobre o procedimento a seguir e usar outros métodos de criação de VNet, consulte [Create a virtual network (classic) by using the Azure portal](../virtual-network/virtual-networks-create-vnet-classic-portal.md) (Criar uma rede virtual (clássica) usando o portal do Azure).

**Para criar uma VNet clássica**

1. Entre no [portal do Azure](https://portal.azure.com). 
2. Clique em **Nova** > **Rede** > **Rede Virtual**.
3. Em **Selecionar um modelo de implantação**, selecione **Clássico** e, em seguida, clique em **Criar**.
4. Digite ou selecione os valores a seguir:
   
   * **Nome**: contosoaadvnet
   * **Espaço de endereço**: 10.1.0.0/16
   * **Nome da sub-rede**: Subnet1
   * **Intervalo de endereços da sub-rede**: 10.1.0.0/24
   * **Assinatura**: (Selecione uma assinatura usada para criar esta VNet.)
   * **ResourceGroup**: contosoaadrg
   * **Localização**: (Selecione uma região para seu cluster HDInsight.)
     
     > [!IMPORTANT]
     > Você deve escolher uma localização que dê suporte ao Azure AD DS. Para obter mais informações, veja [Produtos disponíveis por região](https://azure.microsoft.com/en-us/regions/services/). 
     > 
     > Ambas a VNet clássica e a VNet do grupo de recursos devem estar na mesma região que o Azure AD DS.
     > 
     > 
5. Clique em **Criar** para criar a VNet.

## <a name="create-and-configure-azure-ad-ds-for-your-azure-ad"></a>Criar e configurar o Azure AD DS para o Azure AD
Nesta seção, você irá:

1. Criar um Azure AD.
2. Criar usuários do Azure AD. Esses usuários são usuários do domínio. Você pode usar o primeiro usuário para configurar o cluster HDInsight com o Azure AD.  Os dois usuários são opcionais para este tutorial. Eles serão usados em [Configurar políticas Hive para clusters HDInsight ingressados no domínio](hdinsight-domain-joined-run-hive.md), quando você configurar políticas do Apache Ranger.
3. Crie o grupo de administradores de AAD DC e adicione o usuário do Azure AD ao grupo. Você pode usar esse usuário para criar a unidade organizacional.
4. Habilite o Azure AD DS (Azure AD Domain Services) para o Azure AD.
5. Configure o LDAPS para o Azure AD. O protocolo LDAP é usado para ler e gravar no Azure AD.

Se preferir usar um Azure AD existente, você poderá ignorar as etapas 1 e 2.

**Para criar um Azure AD**

1. Do [Portal clássico do Azure](https://manage.windowsazure.com), clique em **Novo** > **Serviços de Aplicativos** > **Active Directory** > **Diretório** > **Criação Personalizada**. 
2. Digite ou selecione os valores a seguir:
   
   * **Nome**: contosoaaddirectory
   * **Nome de domínio**: contoso.  Esse nome deve ser globalmente exclusivo.
   * **País ou região**: selecione seu país ou região.
3. Clique em **Concluído**.

**Criar um usuário do Azure AD**

1. Do [Portal clássico do Azure](https://manage.windowsazure.com), clique em **Active Directory** -> **contosoaaddirectory**. 
2. No menu superior, clique em **Usuários**.
3. Clique em **Adicionar Usuário**.
4. Insira o **Nome de Usuário** e, em seguida, clique em **Próximo**. 
5. Configure o perfil do usuário; em **Função**, selecione **Administrador Global** e, em seguida, clique em **Próximo**.  A função de Administrador Global é necessária para criar unidades organizacionais.
6. Clique em **Criar** para obter uma senha temporária.
7. Faça uma cópia da senha e, em seguida, clique em **Concluir**. Mais tarde neste tutorial, você usará esse usuário de administrador global para fazer logon na VM de administrador para criar uma unidade organizacional e configurando o DNS reverso.

Siga o mesmo procedimento para criar mais dois usuários com a função **User**, hiveuser1 e hiveuser2. Os usuários a seguir serão usados em [Configurar políticas Hive para clusters HDInsight ingressados no domínio](hdinsight-domain-joined-run-hive.md).

**Para criar o grupo de administradores de AAD DC e adicionar um usuário do Azure AD**

1. Do [Portal clássico do Azure](https://manage.windowsazure.com), clique em **Active Directory** > **contosoaaddirectory**. 
2. No menu superior, clique em **Grupos**.
3. Clique em **Adicionar um Grupo** ou **Adicionar Grupo**.
4. Digite ou selecione os valores a seguir:
   
   * **Nome**: Administradores do AAD DC.  Não altere o nome do grupo.
   * **Tipo de Grupo**: segurança.
5. Clique em **Concluído**.
6. Clique em **Administradores do AAD DC** para abrir o grupo.
7. Clique em **Adicionar Membros**.
8. Selecione o primeiro usuário que você criou na etapa anterior e clique em **Concluir**.
9. Repita as mesmas etapas para criar outro grupo chamado **HiveUsers** e adicione os dois usuários do Hive ao grupo.

Para obter mais informações, veja [Azure AD Domain Services (Preview) – criar o grupo “Administradores do AAD DC”](../active-directory-domain-services/active-directory-ds-getting-started.md).

**Para habilitar o Azure AD DS para o Azure AD**

1. Do [Portal clássico do Azure](https://manage.windowsazure.com), clique em **Active Directory** > **contosoaaddirectory**. 
2. No menu superior, clique em **Configurar**.
3. Role para baixo até **Serviços de Domínio** e defina os seguintes valores:
   
   * **Habilitar os serviços de domínio para este diretório**: sim.
   * **Nome DNS dos serviços de domínio**: mostra o nome DNS padrão do diretório do Azure. Por exemplo, contoso.onmicrosoft.com.
   * **Conectar serviços de domínio a essa rede virtual**: selecione a rede virtual clássica que você criou anteriormente, ou seja, **contosoaadvnet**.
4. Na parte inferior da página, clique em **Salvar**. Você verá **Pendente...** ao lado de **Habilitar os serviços de domínio para este diretório**.  
5. Aguarde até que **Pendente...** desapareça e o campo **Endereço IP** seja populado. Dois endereços IP serão populados. Esses são os endereços IP dos controladores de domínio configurados pelo Domain Services. Cada endereço IP ficará visível depois que o controlador de domínio correspondente estiver provisionado e pronto. Anote os dois endereços IP. Você precisará delas mais tarde.

Para obter mais informações, veja [Azure AD Domain Services (Preview) – Habilitar o Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-enableaadds.md).

**Para sincronizar a senha**

Se você usar o seu próprio domínio, precisará sincronizar a senha. Veja [Enable password synchronization to Azure AD domain services for a cloud-only Azure AD directory](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) (Habilitar a sincronização de senha nos serviços de domínio do Azure AD para um diretório do Azure AD somente na nuvem).

**Para configurar o LDAPS para o Azure AD**

1. Obtenha um certificado SSL assinado por uma autoridade de autenticação para seu domínio. Os certificados autoassinados não podem ser usados. Se você não puder obter um certificado SSL, deverá alcançar hdipreview@microsoft.com para uma exceção.
2. Do [Portal clássico do Azure](https://manage.windowsazure.com), clique em **Active Directory** > **contosoaaddirectory**. 
3. No menu superior, clique em **Configurar**.
4. Role ate os **serviços de domínio**.
5. Clique em **Configurar certificado**.
6. Siga as instruções para especificar o arquivo de certificado e a senha. Você verá **Pendente...** ao lado de **Habilitar os serviços de domínio para este diretório**.  
7. Aguarde até que **Pendente...** desapareça e que **Certificado LDAP Seguro** seja populado.  Isso pode levar 10 minutos ou mais.

> [!NOTE]
> Se algumas tarefas em segundo plano estiverem sendo executadas no Azure AD DS, você verá um erro ao carregar o certificado – <i>Há uma operação sendo executada para este locatário. Tente novamente mais tarde</i>.  Caso esse erro ocorra, tente novamente após algum tempo. O IP do segundo controlador de domínio pode levar até 3 horas para ser provisionado.
> 
> 

Para obter mais informações, veja [Configurar o LDAPS (LDAP Seguro) para um domínio gerenciado do Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="configure-an-organizational-unit-and-reverse-dns"></a>Configurar uma unidade organizacional e DNS reverso
Nesta seção, você deve adicionar uma máquina virtual à VNet do Azure AD e instalar ferramentas administrativas nessa VM, de modo que você possa configurar uma unidade organizacional e DNS reverso. A pesquisa inversa de DNS é necessária para a autenticação Kerberos.

**Para criar uma máquina virtual na rede virtual**

1. Do [Portal clássico do Azure](https://manage.windowsazure.com), clique em **Novo** > **Computação** > **Máquina Virtual** > **Da Galeria**.
2. Selecione uma imagem e clique em **Próximo**.  Se você não souber qual delas usar, selecione a padrão, **Windows Server 2012 R2 Datacenter**.
3. Digite ou selecione os valores a seguir:
   
   * Nome da Máquina Virtual: **contosoaadadmin**
   * Camada: **básica**
   * Novo Nome do Usuário: (Insira um nome de usuário)
   * Senha: (Insira uma senha)
     
     Observe que o nome de usuário e a senha são de administrador local.
4. Clique em **Próximo**
5. Em **Região/Rede Virtual**, selecione a nova rede virtual que você criou na última etapa (contosoaadvnet) e, em seguida, clique em **Próximo**.
6. Clique em **Concluído**.

**Para acessar a VM via RDP**

1. No [Portal clássico do Azure](https://manage.windowsazure.com), clique em **Máquinas Virtuais** > **contosoaadadmin**.
2. Clique em **Painel** no menu superior.
3. Na parte inferior da página, clique em **Conectar**.
4. Siga as instruções e use o nome de usuário e a senha de administrador local para se conectar.

**Para adicionar a VM ao domínio do Azure AD**

1. Na sessão RDP, clique em **Iniciar** e, em seguida, clique em **Gerenciador do Servidor**.
2. No menu esquerdo, clique em **Servidor Local**.
3. Em Grupo de trabalho, clique em **Grupo de trabalho**.
4. Clique em **Alterar**.
5. Clique em **Domínio**, insira **contoso.onmicrosoft.com** e, em seguida, clique em **OK**.
6. Insira as credenciais do usuário do domínio e **OK**.
7. Clique em **OK**.
8. Clique em **OK** para aceitar reinicializar o computador.
9. Clique em **fechar**
10. Clique em **Reiniciar Agora**.

Para obter mais informações, veja [Ingressar uma máquina virtual do Windows Server em um domínio gerenciado](../active-directory-domain-services/active-directory-ds-admin-guide-join-windows-vm.md).

**Para Instalar as ferramentas de DNS e ferramentas de administração do Active Directory**

1. Acesse **contosoaadadmin** via RDP usando a conta de usuário do Azure AD.
2. Clique em **Iniciar** e, em seguida, clique em **Gerenciador do Servidor**.
3. Clique em **Painel** no menu esquerdo.
4. Clique em **Gerenciar** e, em seguida, clique em **Adicionar funções e recursos**.
5. Clique em **Próximo**.
6. Selecione **Instalação baseada em função ou recurso** e clique em **Próximo**.
7. Selecione a máquina virtual atual no pool de servidores e clique em **Próximo**.
8. Clique em **Próximo** para ignorar a funções.
9. Expanda **Ferramentas de Administração de Servidor Remoto**, expanda **Ferramentas de Administração de Função**, selecione **Ferramentas do AD DS e AD LDS** e **Ferramentas do Servidor DNS** e, em seguida, clique em **Próximo**. 
10. Clique em **Próximo**
11. Clique em **Instalar**.

Para obter mais informações, veja [Instalar ferramentas de administração do Active Directory na máquina virtual](../active-directory-domain-services/active-directory-ds-admin-guide-administer-domain.md#task-2---install-active-directory-administration-tools-on-the-virtual-machine).

**Para configurar o DNS reverso**

1. Acesse contosoaadadmin via RDP usando a conta de usuário do Azure AD.
2. Clique em **Iniciar**, em **Ferramentas Administrativas** e em **DNS**. 
3. Clique em **Não** para ignorar a adição de ContosoAADAdmin.
4. Selecione **O computador a seguir**, insira o endereço IP do primeiro servidor DNS configurado anteriormente e, em seguida, clique em **OK**.  Você deverá ver o DC/DNS ser adicionado ao painel esquerdo.
5. Expanda o servidor DNS/DC, clique com o botão direito do mouse em **Zonas de Pesquisa Inversa** e, em seguida, clique em **Nova Zona**. O Assistente de Nova Zona se abre.
6. Clique em **Próximo**.
7. Selecione a **Zona primária** e, em seguida, clique em **Próximo**.
8. Selecione **Para todos os servidores DNS executando em controladores de domínio nesse domínio** e, em seguida, clique em **Próximo**.
9. Selecione a **Zona de Pesquisa Inversa IPv4** e, em seguida, clique em **Próximo**.
10. Em **ID de rede**, digite o prefixo para o intervalo de rede da VNET HDInsight e, em seguida, clique em **Próximo**. Você criará a VNet HDInsight na seção a seguir.
11. Clique em **Próximo**.
12. Clique em **Próximo**.
13. Clique em **Concluir**.

A unidade organizacional que você criar em seguida será usada ao criar o cluster HDInsight. Os usuários e contas de computador do sistema Hadoop serão colocadas nessa UO.

**Criar uma UO (Unidade Organizacional) em um domínio gerenciado do Azure AD Domain Services**

1. Acesse **contosoaadadmin** via RDP usando a conta de domínio que está no grupo **AAD DC administradores**.
2. Clique em **Iniciar**, clique em **Ferramentas Administrativas** e, em seguida, clique em **Centro Administrativo do Active Directory**.
3. Clique no nome de domínio no painel esquerdo. Por exemplo, contoso.
4. Clique em **Novo** sob o nome do domínio no painel **Tarefa** e, em seguida, clique em **Unidade Organizacional**.
5. Digite um nome, por exemplo **HDInsightOU** e, em seguida, clique em **OK**. 

Para obter mais informações, veja [Criar uma UO (Unidade Organizacional) em um domínio gerenciado do Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md).

## <a name="create-a-resource-manager-vnet-for-hdinsight-cluster"></a>Criar uma VNet do Resource Manager para o cluster HDInsight
Nesta seção, você criará uma VNet do Azure Resource Manager que será usada para o cluster HDInsight. Para obter mais informações sobre como criar VNET do Azure usando outros métodos, veja [Criar uma rede virtual](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)

Depois de criar a VNet, você configurará a VNet do Resource Manager para usar os mesmos servidores DNS que os usados na VNet do Azure AD. Se você tiver seguido as etapas deste tutorial para criar a VNet clássica e o Azure AD, os servidores DNS serão 10.1.0.4 e 10.1.0.5.

**Para criar uma VNet do Resource Manager**

1. Entre no [portal do Azure](https://portal.azure.com).
2. Clique em **Nova**, **Rede**, **Rede virtual**. 
3. Em **Selecionar um modelo de implantação**, selecione **Resource Manager** e clique em **Criar**.
4. Digite ou selecione os valores a seguir:
   
   * **Nome**: contosohdivnet
   * **Espaço de endereço**: 10.2.0.0/16. Certifique-se de que o intervalo de endereços não possa se sobrepor ao intervalo de endereços IP da VNet clássica.
   * **Nome da sub-rede**: Subnet1
   * **Intervalo de endereços da sub-rede**: 10.2.0.0/24
   * **Assinatura**: (Selecione sua assinatura do Azure.)
   * **Grupo de recursos**: contosohdirg
   * **Localização**: (Selecione a mesma localização da VNet do Azure AD, ou seja, contosoaadvnet.)
5. Clique em **Criar**.

**Para configurar o DNS para a VNet do Resource Manager**

1. Do [Portal do Azure](https://portal.azure.com), clique em **Mais serviços** -> **Redes virtuais**. Certifique-se de não clicar em **Redes virtuais (clássicas)**.
2. Clique em **contosohdivnet**.
3. Clique em **Servidores DNS** do lado esquerdo da nova folha.
4. Clique em **Personalizado** e insira os seguintes valores:
   
   * 10.1.0.4
   * 10.1.0.5
     
     Esses endereços IP do servidor DNS devem corresponder aos servidores DNS na VNet do Azure AD (VNet clássica).
5. Clique em **Salvar**.

## <a name="peer-the-azure-ad-vnet-and-the-hdinsight-vnet"></a>Emparelhar a VNet do Azure AD e a VNet HDInsight
**Para emparelhar as duas VNets**

1. Entre no [portal do Azure](https://portal.azure.com).
2. No menu esquerdo, clique em **Mais serviços**.
3. Clique em **Redes Virtuais**. Não clique em **Redes virtuais (clássicas)**.
4. Clique em **contosohdivnet**.  Essa é a VNet HDInsight.
5. No menu esquerdo da folha, clique em **Emparelhamentos**.
6. No menu superior, clique em **Adicionar**. Ele abre a folha **Adicionar emparelhamento**.
7. Na folha **Adicionar emparelhamento**, defina ou selecione os valores a seguir:
   
   * **Nome**: ContosoAADHDIVNetPeering
   * **Modelo de implantação de rede virtual**: clássico
   * **Assinatura**: selecione o nome da assinatura usada para a VNET clássica (Azure AD).
   * **Rede virtual**: contosoaadvnet.
   * **Permitir acesso à rede virtual**: (Marcar)
   * **Permitir tráfego encaminhado**: (Marcar). Deixe as outras duas caixas de seleção desmarcadas.
8. Clique em **OK**.

## <a name="create-hdinsight-cluster"></a>Criar cluster HDInsight
Nesta seção, você criará um cluster Hadoop baseado em Linux no HDInsight usando o Portal do Azure ou então o [modelo do Azure Resource Manager](../azure-resource-manager/resource-group-template-deploy.md). Para outros métodos de criação de cluster e noções básicas sobre as configurações, confira [Criar clusters do HDInsight](hdinsight-hadoop-provision-linux-clusters.md). Para obter mais informações sobre como usar um modelo do Resource Manager para criar clusters Hadoop no HDInsight, confira [Criar clusters Hadoop no HDInsight usando modelos do Resource Manager](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

**Para criar um cluster HDInsight ingressado no Domínio usando o Portal do Azure**

1. Entre no [portal do Azure](https://portal.azure.com).
2. Clique em **Novo**, **Inteligência + análise** e então em **HDInsight**.
3. Da folha **Novo cluster HDInsight**, insira ou selecione os valores a seguir:
   
   * **Nome do cluster**: insira um novo nome do cluster para o cluster HDInsight ingressado no domínio.
   * **Assinatura**: selecione uma assinatura do Azure usada para criar esse cluster.
   * **Configuração do cluster**:
     
     * **Tipo de cluster**: Hadoop. Atualmente, apenas clusters Hadoop dão suporte ao HDInsight ingressado no domínio.
     * **Sistema Operacional**: Linux.  Apenas clusters HDInsight baseados em Linux dão suporte ao HDInsight ingressado no domínio.
     * **Versão**: Hadoop 2.7.3 (HDI 3.5). Somente a versão 3.5 de cluster HDInsight dá suporte ao HDInsight ingressado no domínio.
     * **Tipo de cluster**: PREMIUM
       
       Clique em **Selecionar** para salvar as alterações.
   * **Credenciais**: configure as credenciais para o usuário do cluster e o usuário SSH.
   * **Fonte de Dados**: crie uma nova conta de armazenamento ou use uma conta de armazenamento existente como a conta de armazenamento padrão para o cluster HDInsight. A localização deve ser a mesma que a das duas VNets.  A localização também é a localização do cluster HDInsight.
   * **Preço**: selecione o número de nós de trabalho do seu cluster.
   * **Configurações avançadas**: 
     
     * **Ingresso no domínio e Sub-rede/VNet**: 
       
       * **Configurações de domínio**: 
         
         * **Nome de domínio**: contoso.onmicrosoft.com
         * **Nome de usuário de domínio**: insira um nome de usuário de domínio. Esse domínio deve ter os seguintes privilégios: adicionar computadores ao domínio e colocá-los na unidade organizacional que você configurou anteriormente; criar entidades de serviço dentro da unidade organizacional que você configurou anteriormente; criar entradas de DNS reverso. Esse usuário de domínio se tornará o administrador deste cluster HDInsight ingressado no domínio.
         * **Senha de domínio**: insira a senha de usuário de domínio.
         * **Unidade Organizacional**: insira o nome diferenciado da UO que você configurou anteriormente. Por exemplo: OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com
         * **URL LDAPS**: ldaps://contoso.onmicrosoft.com:636
         * **Acessar grupo de usuários**: especifique o grupo de segurança cujos usuários você deseja sincronizar com o cluster. Por exemplo, HiveUsers.
           
           Clique em **Selecionar** para salvar as alterações.
           
           ![Configuração de domínio para configurar o portal do HDInsight ingressado no domínio](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-portal-domain-setting.png)
       * **Rede virtual**: contosohdivnet
       * **Subrede**: Subnet1
         
         Clique em **Selecionar** para salvar as alterações.        
         Clique em **Selecionar** para salvar as alterações.
   * **Grupo de Recursos**: selecione o grupo de recursos usado para a VNet HDInsight (contosohdirg).
4. Clique em **Criar**.  

Outra opção para criar o cluster HDInsight de domínio é usar um modelo do Azure Resource Manager. O procedimento a seguir mostra como fazer isso:

**Para criar um cluster HDInsight ingressado no domínio usando um modelo do Azure Resource Manager**

1. Clique na imagem a seguir para abrir um modelo do Resource Manager no Portal do Azure. O modelo do Resource Manager está localizado em um contêiner de blob público. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/hdinsight-domain-joined-configure/deploy-to-azure.png" alt="Deploy to Azure"></a>
2. Na folha **Parâmetros**, insira os seguintes valores:
   
   * **Assinatura**: (Selecione sua assinatura do Azure).
   * **Grupo de recursos**: clique em **Usar existente** e especifique o mesmo grupo de recursos que você vem usando.  Por exemplo, contosohdirg. 
   * **Localização**: especifique uma localização para o grupo de recursos.
   * **Nome do Cluster**: insira um nome para o cluster Hadoop que você criará. Por exemplo, contosohdicluster.
   * **Tipo de Cluster**: selecione um tipo de cluster.  O valor padrão é **hadoop**.
   * **Localizaçao**: selecione uma localização para o cluster.  A conta de armazenamento padrão usa a mesma localização.
   * **Contagem de Nós de Trabalho do Cluster**: selecione o número de nós de trabalho.
   * **Nome e senha de logon do cluster**: o nome de logon padrão é **admin**.
   * **Nome de usuário e senha SSH**: o nome de usuário padrão é **sshuser**.  Você pode renomeá-lo. 
   * **ID da Rede Virtual**: /subscriptions/&lt;SubscriptionID>/resourceGroups/&lt;ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/&lt;VNetName>
   * **Sub-rede da Rede Virtual**: /subscriptions/&lt;SubscriptionID>/resourceGroups/&lt;ResourceGroupName>/providers/Microsoft.Network/virtualNetworks/&lt;VNetName>/subnets/Subnet1
   * **Nome de Domínio**: contoso.onmicrosoft.com
   * **DN da Unidade Organizacional**: OU=HDInsightOU,DC=contoso,DC=onmicrosoft,DC=com
   * **DNs do Grupo de Usuários do Cluster**: "\"CN=HiveUsers,OU=AADDC Users,DC=<DomainName>,DC=onmicrosoft,DC=com\""
   * **LDAPUrls**: ["ldaps://contoso.onmicrosoft.com:636"]
   * **DomainAdminUserName**: (Insira o nome de usuário do administrador do domínio)
   * **DomainAdminPassword**: (Insira a senha de usuário do administrador do domínio)
   * **Concordo com os termos e condições declarados acima**: (Marcar)
   * **Fixar no painel**: (Marcar)
3. Clique em **Comprar**. Você verá um novo bloco intitulado **Implantando a implantação de modelo**. A criação de um cluster demora cerca de 20 minutos. Após a criação do cluster, você pode clicar na folha do cluster no portal para abri-la.

Depois de concluir o tutorial, talvez você queira excluir o cluster. Com o HDInsight, seus dados são armazenados no Armazenamento do Azure, assim você poderá excluir, com segurança, um cluster quando ele não estiver em uso. Você também é cobrado por um cluster HDInsight, mesmo quando ele não está em uso. Como os encargos para o cluster são muitas vezes maiores do que os encargos para armazenamento, faz sentido, do ponto de vista econômico, excluir os clusters quando não estiverem em uso. Para obter instruções sobre como excluir um cluster, confira [Gerenciar clusters Hadoop no HDInsight usando o Portal do Azure](hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Próximas etapas
* Para configurar um cluster do HDInsight ingressado no domínio usando o Azure PowerShell, confira [Configure Domain-joined HDInsight clusters use Azure PowerShell](hdinsight-domain-joined-configure-use-powershell.md) (Configurar clusters do HDInsight Ingressado no Domínio usando Azure PowerShell).
* Para configurar políticas do Hive e executar consultas do Hive, confira [Configure Hive policies for Domain-joined HDInsight clusters](hdinsight-domain-joined-run-hive.md) (Configurar políticas do Hive para clusters do HDInsight Ingressado no Domínio).
* Para usar SSH para conectar-se a clusters do HDInsight Ingressados no Domínio, confira [Usar SSH com Hadoop baseado em Linux no HDInsight do Linux, Unix ou OS X](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).




<!--HONumber=Nov16_HO5-->


