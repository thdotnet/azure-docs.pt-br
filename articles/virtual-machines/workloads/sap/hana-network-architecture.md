---
title: Arquitetura de rede do SAP HANA do Azure (Instâncias Grandes) | Microsoft Docs
description: arquitetura de rede para implantar o SAP HANA no Azure (Instâncias Grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: gwallace
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/25/2019
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2359f1ea1d55b8ce153295cc71fdf78040e8e316
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707408"
---
# <a name="sap-hana-large-instances-network-architecture"></a>Arquitetura de rede do SAP HANA (Instâncias Grandes)

A arquitetura dos serviços de rede do Azure é um componente essencial da implantação com êxito dos aplicativos do SAP HANA em Instâncias Grandes. Normalmente, as implantações do SAP HANA no Azure (Instâncias Grandes) têm uma estrutura SAP maior com diversas soluções SAP com vários tamanhos de bancos de dados, consumo de recursos de CPU e a utilização de memória. É provável que nem todos os sistemas de TI estão localizados no Azure já. Paisagem do SAP geralmente é híbrida também de um ponto DBMS e SAP ponto de vista do aplicativo usando uma combinação de NetWeaver e 4hana/s e SAP HANA e outros DBMS. O Azure oferece serviços diferentes que permitem executar diferentes sistemas DBMS, NetWeaver e 4hana/s no Azure. Azure também oferece a que você para tornar a aparência do Azure a tecnologia de rede, como um data center virtual às suas implantações de software local

A menos que seus sistemas de TI completos estão hospedados no Azure. O recurso de rede do Azure é usado para conectar o mundo de locais com seus ativos do Azure para tornar a aparência de um datacenter virtual seu o Azure. A funcionalidade de rede do Azure usada é: 

- Redes virtuais do Azure são conectadas para o [ExpressRoute](https://azure.microsoft.com/services/expressroute/) circuito que se conecta aos seus ativos de rede local.
- Um circuito do ExpressRoute que conecta o local ao Azure deve ter uma largura de banda mínima de [1 Gbps ou superior](https://azure.microsoft.com/pricing/details/expressroute/). Essa largura de banda mínima permite largura de banda adequada para a transferência de dados entre sistemas locais e sistemas em execução nas VMs. Também permite largura de banda adequada para conexão a sistemas do Azure a partir de usuários locais.
- Todos os sistemas SAP no Azure são configurados em redes virtuais se comuniquem entre si.
- O Active Directory e DNS hospedados localmente são estendidos para o Azure por meio do ExpressRoute do local ou em execução completa no Azure.

Para o caso específico da integração do HANA em instâncias grandes a malha de rede do Centro de dados do Azure, a tecnologia Azure ExpressRoute é usada também


> [!NOTE] 
> Apenas uma assinatura do Azure pode ser vinculada a apenas um locatário em um carimbo de instância grande do HANA em uma região específica do Azure. Por outro lado, um único locatário de carimbo de instância grande do HANA pode ser vinculado a apenas uma assinatura do Azure. Esse requisito é consistente com outros objetos cobráveis no Azure.

Se o SAP HANA no Azure (instâncias grandes) é implantado em várias regiões do Azure diferentes, um locatário separado será implantado no carimbo da instância grande do HANA. É possível executar ambos sob a mesma assinatura do Azure, desde que essas instâncias façam parte da mesma paisagem do SAP. 

> [!IMPORTANT] 
> O método de implantação do Azure Resource Manager é compatível com o SAP HANA no Azure (instâncias grandes).

 

## <a name="additional-virtual-network-information"></a>Informações adicionais sobre rede virtual

Para se conectar a uma rede virtual para ExpressRoute, um gateway de ExpressRoute do Azure deve ser criado. Para obter mais informações, consulte [gateways sobre o Expressroute para o ExpressRoute](../../../expressroute/expressroute-about-virtual-network-gateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

Um gateway de ExpressRoute do Azure é usado com o ExpressRoute para uma infraestrutura fora do Azure ou para um carimbo de instância grande do Azure. Você pode conectar o gateway de ExpressRoute do Azure a um máximo de quatro diferentes circuitos do ExpressRoute, desde que essas conexões sejam originadas de diferentes roteadores de borda do Microsoft enterprise. Para obter mais informações, consulte [Infraestrutura e conectividade do SAP HANA (Instâncias Grandes) do Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

> [!NOTE] 
> A taxa de transferência máxima que você pode obter com um gateway de ExpressRoute é de 10 Gbps usando uma conexão de ExpressRoute. A cópia de arquivos entre uma VM que reside em uma rede virtual e um sistema local (como um único fluxo de cópia) não alcança a taxa de transferência total as diferentes SKUs do gateway. Para aproveitar a largura de banda completa do gateway de ExpressRoute, use vários fluxos. Ou, será necessário copiar arquivos diferentes em fluxos paralelos de um único arquivo.


## <a name="networking-architecture-for-hana-large-instance"></a>Arquitetura de rede para SAP HANA em Instâncias Grandes
A arquitetura de rede para o SAP HANA em Instâncias Grandes pode ser separada em quatro partes diferentes:

- Rede local e conexão do ExpressRoute para o Azure. Essa parte é o domínio do cliente e é conectada ao Azure por meio do ExpressRoute. Este circuito do Expressroute é totalmente pagos por você como um cliente. A largura de banda deve ser grande o suficiente para lidar com o tráfego de rede entre seus ativos locais e a região do Azure que você está se conectando contra. Observe o canto inferior direito na figura a seguir.
- Serviços de rede do Azure, conforme discutidos anteriormente, com as redes virtuais, gateways de ExpressRoute adicionados novamente, precisa. Essa parte é uma área na qual você precisa localizar os projetos apropriados para os requisitos do aplicativo, segurança e requisitos de conformidade. Se você usa o SAP HANA em Instâncias Grandes, é outro ponto a ser considerado em termos do número de redes virtuais e SKUs do gateway do Azure a escolher. Observe a parte superior direita na figura.
- Conectividade do SAP HANA em Instâncias Grandes através da tecnologia do ExpressRoute no Azure. Essa parte é implantada e feita pela Microsoft. Tudo o que você precisa fazer é fornecer alguns intervalos de endereço IP após a implantação dos ativos no SAP HANA em Instâncias Grandes conectar o circuito do ExpressRoute às redes virtuais. Para obter mais informações, consulte [Infraestrutura e conectividade do SAP HANA (Instâncias Grandes) do Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Não há nenhuma taxa adicional para você como um cliente para a conectividade entre a malha de rede do Centro de dados do Azure e as unidades de instância grande do HANA.
- Rede dentro do carimbo de instância grande do HANA, que geralmente é transparente para você.

![Rede virtual conectada ao SAP HANA do Azure (Instâncias Grandes) e no local](./media/hana-overview-architecture/image1-architecture.png)

O requisito de que os ativos locais devem conectar-se por meio do ExpressRoute para o Azure não altera porque você utiliza o SAP HANA em Instâncias Grandes. O requisito de ter uma ou várias redes virtuais executando as VMs, que hospedam a camada de aplicativo que conecta-se às instâncias do HANA hospedadas nas unidades do SAP HANA em Instâncias Grandes, também não é alterado. 

As diferenças para implantações do SAP no Azure são:

- As unidades do SAP HANA em Instâncias Grandes do locatário do cliente são conectadas por meio de outro circuito do ExpressRoute às redes virtuais. Para separar as condições de carga, o local para circuitos de ExpressRoute de rede virtual do Azure e os circuitos entre redes virtuais do Azure e instâncias grandes do HANA não compartilham os mesmos roteadores.
- O perfil de carga de trabalho entre a camada de aplicativo do SAP e o SAP HANA em Instâncias Grandes é de natureza diferente, com muitas solicitações e intermitências pequenas, como transferências de dados (conjuntos de resultados) do SAP HANA para a camada de aplicativo.
- A arquitetura de aplicativos do SAP é mais sensível à latência de rede que os cenários típicos em que os dados são trocados entre o local e o Azure.
- O gateway de ExpressRoute do Azure tem pelo menos duas conexões do ExpressRoute. Um circuito que estiver conectado a local e outro que estiver conectado a instâncias grandes do HANA. Isso deixa apenas o espaço para outra dois circuitos adicionais dos MSEEs diferentes para conectar-se ao Gateway de ExpressRoute. Essa restrição é independente do uso de caminho rápido de ExpressRoute. Todos os circuitos conectados compartilham a largura de banda máxima para dados de entrada do gateway de ExpressRoute.

A latência de rede entre as VMs e as unidades do SAP HANA em Instâncias Grandes pode ser maior que uma latência de viagem de ida e volta típica de rede de VM para VM. Dependendo da região do Azure, os valores medidos podem exceder a latência de ida e volta de 0,7 ms classificada como abaixo da média em [nota SAP 1100926 – perguntas Frequentes: Desempenho de rede](https://launchpad.support.sap.com/#/notes/1100926/E). Dependendo da ferramenta e da região do Azure para medir a latência da viagem de ida e volta da rede entre uma VM do Azure e a unidade de Instância Grande do HANA, a latência medida pode ser até cerca de 2 milissegundos. No entanto, os clientes implantam aplicativos do SAP de produção baseados no SAP HANA com êxito no SAP HANA em Instâncias Grandes. Certifique-se de testar os processos empresariais completamente no SAP HANA em Instâncias Grandes do Azure. Uma nova funcionalidade, chamada ExpressRoute Fast caminho, é capaz de reduzir a latência de rede entre VMs no Azure de camada de aplicativo e de instâncias grandes do HANA substancialmente (veja abaixo). 

Para fornecer latência de rede determinística entre VMs e instâncias grandes HANA, a escolha do gateway de ExpressRoute SKU é essencial. Diferentemente dos padrões de tráfego entre VMs locais e VMs, o padrão de tráfego entre as VMs e o SAP HANA em Instâncias Grandes pode desenvolver pequenas, mas elevadas intermitências de solicitações e volumes de dados a serem transmitidos. Para lidar bem com essas intermitências, é recomendável usar a SKU do gateway UltraPerformance. Para a classe de tipo II de SKUs de instância grande do HANA, o uso do SKU do gateway UltraPerformance como um gateway de expressroute das é obrigatório.

> [!IMPORTANT] 
> Dado o tráfego geral entre o aplicativo do SAP e as camadas do banco de dados, somente as SKUs do gateway HighPerformance ou UltraPerformance para redes virtuais têm suporte para conexão com o SAP HANA do Azure (Instâncias Grandes). Para HANA grande instância SKUs do tipo II, apenas o SKU do gateway UltraPerformance tem suporte como um gateway de ExpressRoute. Exceções se aplicam ao usar o ExpressRoute Fast caminho (veja abaixo)

### <a name="expressroute-fast-path"></a>Caminho rápido de ExpressRoute
Para reduzir a latência, o caminho rápido de ExpressRoute foi introduzido e lançado em maio de 2019 para conectividade específica do HANA em instâncias grandes redes virtuais do Azure que hospedam as VMs do aplicativo SAP. A principal diferença para a solução distribuiu até agora, é, o que os fluxos de dados entre VMs e instâncias grandes do HANA não sejam roteados por meio do gateway de ExpressRoute mais. Em vez disso, as VMs atribuídas nas sub-redes da rede virtual do Azure estão se comunicando diretamente com o roteador de borda corporativa dedicados. 

> [!IMPORTANT] 
> A funcionalidade de caminho rápido de ExpressRoute requer que as sub-redes de execução das VMs de aplicativo SAP estejam na mesmo rede virtual do Azure que foi conectado a instâncias grandes do HANA. As VMs localizadas em redes virtuais do Azure que são emparelhadas com a rede virtual do Azure conectados diretamente para as unidades de instância grande do HANA não estão se beneficiando de caminho rápido de ExpressRoute. Como resultado típico hub e spoke virtual designs de rede, onde os circuitos do ExpressRoute estão se conectando em relação a uma rede virtual de hub e redes virtuais, que contém a camada de aplicativo do SAP (raios) estão obtendo emparelhadas, a otimização pelo ExpressRoute Fast Caminho não funcionará. Além disso, caminho rápido de ExpressRoute não suporta regras de roteamento definido pelo usuário (UDR) hoje mesmo. Para obter mais informações, consulte [ExpressRoute virtual de rede gateway e FastPath](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways). 


Para obter mais detalhes sobre como configurar o ExpressRoute Fast caminho, leia o documento [conectar uma rede virtual para instâncias grandes do HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-connect-vnet-express-route).    

> [!NOTE]
> Um gateway UltraPerformance ExpressRoute é necessário ter o trabalho de caminho rápido de ExpressRoute


## <a name="single-sap-system"></a>Único sistema SAP

A infraestrutura local mostrada anteriormente é conectada por meio do ExpressRoute no Azure. O circuito do ExpressRoute conecta-se em um roteador de borda do Microsoft enterprise (MSEE). Para obter mais informações, consulte [Visão geral técnica do ExpressRoute](../../../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Após o estabelecimento da rota, ele se conecta ao backbone do Azure.

> [!NOTE] 
> Para executar paisagens do SAP no Azure, conecte o roteador de borda corporativa mais próximo da região do Azure na paisagem do SAP. Carimbos de instância grande do HANA são conectados por meio de dispositivos de roteador de borda corporativa dedicados para minimizar a latência de rede entre VMs no IaaS do Azure e os carimbos de instância grande do HANA.

O gateway de ExpressRoute para as VMs que hospedam as instâncias do aplicativo SAP está conectado a um circuito de ExpressRoute que conecta a locais. A mesma rede virtual é conectada a um roteador de borda corporativa separado dedicado para conectar os carimbos da Instância Grande. Usando o ExpressRoute Fast caminho, o fluxo de dados do HANA em instâncias grandes para a camada de aplicativo SAP VMs não são roteadas por meio do gateway de ExpressRoute mais e com isso reduzem a rede de latência de viagem de ida e volta.

Esse sistema é um exemplo direto de um único sistema SAP. A camada de aplicativo do SAP é hospedada no Azure. O banco de dados do SAP HANA é executado no SAP HANA do Azure (Instâncias Grandes). A suposição é que a largura de banda de gateway de ExpressRoute de taxa de transferência de 2 Gbps ou 10 Gbps não representa um gargalo.

## <a name="multiple-sap-systems-or-large-sap-systems"></a>Vários sistemas SAP ou grandes sistemas SAP

Se vários sistemas SAP ou grandes sistemas SAP forem implantados para se conectar ao SAP HANA no Azure (instâncias grandes), a taxa de transferência do gateway de ExpressRoute pode se tornar um gargalo. Ou você deseja isolar os sistemas de produção e não seja de produção em diferentes redes virtuais do Azure. Nesse caso, divida as camadas do aplicativo em várias redes virtuais. Também é possível criar uma rede virtual especial que conecta-se ao SAP HANA em Instâncias Grandes para casos como:

- Execução de backups diretamente das instâncias do HANA na Instância Grande do HANA para uma VM no Azure que hospeda compartilhamentos NFS.
- Cópia de backups grandes ou de outros arquivos de unidades de Instância Grande do HANA com o espaço em disco gerenciado no Azure.

Use uma rede virtual separada para hospedar as VMs que gerenciam o armazenamento para transferência em massa de dados entre instâncias grandes do HANA e o Azure. Esse acordo evita os efeitos de grandes arquivos ou transferência de dados de instância grande do HANA no Azure no gateway do ExpressRoute que atende às VMs que executem a camada de aplicativo SAP. 

Uma arquitetura de rede mais escalonável:

- Aproveite várias redes virtuais para uma única camada de aplicativo do SAP maior.
- Implante uma rede virtual separada para cada sistema SAP implantado, em comparação à combinação desses sistemas SAP em sub-redes separadas sob a mesma rede virtual.

  Uma arquitetura de rede mais escalonável para o SAP HANA no Azure (Instâncias Grandes):

![Implantar camada de aplicativo do SAP em várias redes virtuais](./media/hana-overview-architecture/image4-networking-architecture.png)

Depende de regras e restrições, que você deseja aplicar entre as redes virtuais diferentes hospedando máquinas virtuais de diferentes sistemas SAP, você deve emparelhar essas redes virtuais. Para obter mais informações sobre o emparelhamento de rede virtual, consulte [Emparelhamento de rede virtual](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview).


## <a name="routing-in-azure"></a>Roteamento no Azure

Implantação padrão, três considerações de roteamento de rede são importantes para SAP HANA no Azure (instâncias grandes):

* SAP HANA no Azure (instâncias grandes) pode ser acessado somente por meio de VMs do Azure e a conexão do ExpressRoute dedicada, não diretamente do local. O acesso direto do local para as unidades do SAP HANA em Instâncias Grandes, conforme fornecido pela Microsoft a você, não é possível imediatamente. As restrições de roteamento transitivas devem-se à arquitetura de rede atual do Azure usada para o SAP HANA em Instâncias Grandes. Alguns clientes de administração e quaisquer aplicativos que precisam de acesso direto, como o SAP Solution Manager em execução no local, não podem conectar ao banco de dados do SAP HANA. Para exceções, verifique a seção 'Roteamento direto a instâncias grandes HANA'.

* Se você tiver unidades de instância grande do HANA implantadas em duas regiões diferentes do Azure para recuperação de desastres, as mesmas restrições de roteamentos transitórias aplicadas no passado. Em outras palavras, os endereços IP de uma unidade de instância grande do HANA em uma região (por exemplo, oeste dos EUA) não foram roteados para uma unidade de instância grande do HANA implantada em outra região (por exemplo, Leste dos EUA). Essa restrição era independente do uso do emparelhamento entre regiões ou da conexão cruzada os circuitos do ExpressRoute que conectam as unidades de instância grande do HANA para redes virtuais de rede do Azure. Para uma representação gráfica, consulte a figura na seção "Usar unidades do SAP HANA em Instâncias Grandes em várias regiões". Essa restrição, que acompanha a arquitetura implantada, proibido o uso imediato da replicação do sistema HANA como funcionalidade de recuperação de desastres. Para alterações recentes, consulte a seção 'Unidades de instância grande do HANA de uso em várias regiões'. 

* SAP HANA em unidades do Azure (instâncias grandes) têm um endereço IP atribuído do que o intervalo de endereços de pool IP do servidor que você enviou ao solicitar a implantação de instância grande do HANA. Para obter mais informações, consulte [Infraestrutura e conectividade do SAP HANA (Instâncias Grandes) do Azure](hana-overview-infrastructure-connectivity.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Esse endereço IP é acessível por meio de assinaturas do Azure e o circuito que se conecta a redes virtuais do Azure para instâncias grandes do HANA. O endereço IP atribuído fora desse intervalo de endereços do pool de IP do servidor é atribuído diretamente à unidade de hardware. Ele*não* é atribuído pelo NAT, como foi o caso nas primeiras implantações dessa solução. 

### <a name="direct-routing-to-hana-large-instances"></a>Direcionar o roteamento para instâncias grandes do HANA
Por padrão o roteamento transitivo entre unidades de instância grande do HANA e no local ou entre a instância grande do HANA roteamento que são implantadas em duas regiões diferentes não funciona. Existem várias possibilidades para habilitar a tal um roteamento transitivo.

- Um proxy reverso para rotear dados, de e para. Por exemplo, F5 BIG-IP, NGINX com o Gerenciador de tráfego implantado na rede virtual do Azure que se conecta às instâncias grandes do HANA e no local como uma solução de roteamento de tráfego/firewall virtual.
- Usar [regras IPTables](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_%3a_Ch14_%3a_Linux_Firewalls_Using_iptables#.Wkv6tI3rtaQ) em uma VM Linux para habilitar o roteamento entre localidades locais e as unidades do SAP HANA em Instâncias Grandes ou entre unidades do SAP HANA em Instâncias Grandes em regiões diferentes. A VM em execução IPTables precisa para ser implantado na rede virtual do Azure que se conecta ao HANA grandes instâncias e locais. A VM precisa ser dimensionados adequadamente, isso, o que a taxa de transferência de rede da VM é suficiente para o tráfego de rede esperado. Para obter detalhes sobre a VM da largura de banda de rede, verifique o artigo [máquinas virtuais de tamanhos do Linux no Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-network%2ftoc.json).
- [Firewall do Azure](https://azure.microsoft.com/services/azure-firewall/) seria outra solução para permitir o tráfego direto entre locais e unidades de instância grande do HANA. 

Todo o tráfego dessas soluções será encaminhado por meio de uma rede virtual do Azure e como tal, o tráfego pode ser adicionalmente restrito pelos dispositivos do soft usados ou pelo Azure grupos de segurança, portanto, que varia de determinados endereços IP ou o endereço IP de no local pode ser bloqueado ou permitido explicitamente acessando instâncias grandes HANA. 

> [!NOTE]  
> Esteja ciente de que a implementação e o suporte para soluções personalizadas envolvendo IPTables ou dispositivos de rede de terceiros não são fornecidos pela Microsoft. O suporte deve ser fornecido pelo fornecedor do componente usado ou pelo integrador. 

#### <a name="express-route-global-reach"></a>Express alcance Global de rota
A Microsoft introduziu uma nova funcionalidade chamada [alcance Global de ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-global-reach). Alcance global pode ser usado para instâncias grandes do HANA em dois cenários:

- Habilitar o acesso direto local às suas unidades de instância grande do HANA implantadas em diferentes regiões
- Habilitar a comunicação direta entre suas unidades de instância grande do HANA implantadas em diferentes regiões


##### <a name="direct-access-from-on-premise"></a>Acesso direto do local
As regiões do Azure onde o alcance Global é oferecido, você pode solicitar ativando a funcionalidade de alcance Global para seu circuito de ExpressRoute que conecta sua rede local para a rede virtual do Azure que se conecta a suas unidades de instância grande do HANA também. Há algumas implicações de custo para o lado local de seu circuito do ExpressRoute. Para obter os preços, verifique os preços para [complemento de alcance Global](https://azure.microsoft.com/pricing/details/expressroute/). Não há nenhum custo adicional para você relacionados para o circuito que conecta as unidades de instância grande do HANA no Azure. 

> [!IMPORTANT]  
> No caso de usar o alcance Global para habilitar o acesso direto entre seus ativos de locais e unidades de instância grande do HANA, é o fluxo de dados e controle de rede **não é roteado por meio de redes virtuais do Azure**, mas diretamente entre a Microsoft roteadores de exchange Enterprise. Como resultado quaisquer regras NSG ou ASG ou qualquer tipo de NVA, firewall ou proxy implantado em uma rede virtual do Azure, não são obtendo tocadas. **Se você usar o alcance Global de ExpressRoute para habilitar o acesso direto local às restrições de unidades de instância grande do HANA e permissões para acessar as unidades de instância grande do HANA precisam ser definidos em firewalls no lado do local** 

##### <a name="connecting-hana-large-instances-in-different-azure-regions"></a>Conectar-se a instâncias grandes do HANA em diferentes regiões do Azure
Da mesma forma, conforme o alcance Global do ExpressRoute pode ser usado para conectar-se no local para unidades de instância grande do HANA, ele pode usado para conectar duas que locatários de instância grande do HANA implantadas em duas regiões diferentes para você. O isolamento é os circuitos do ExpressRoute que seus locatários de instância grande do HANA estão usando para se conectar ao Azure em ambas as regiões. Não há encargos adicionais para se conectar a dois locatários de instância grande do HANA implantadas em duas regiões diferentes. 

> [!IMPORTANT]  
> O fluxo de dados e fluxo de controle do tráfego de rede entre os diferentes grandes do HANA locatários de instância não serão roteados por meio de redes do azure. Como resultado, é possível usar a funcionalidade do Azure ou NVAs para impor restrições de comunicação entre os dois locatários de instâncias grandes do HANA. 

Para obter mais detalhes sobre como obter a ExpressRoute alcance Global habilitado, leia o documento [conectar uma rede virtual para instâncias grandes do HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-connect-vnet-express-route).


## <a name="internet-connectivity-of-hana-large-instance"></a>Conectividade com a Internet do SAP HANA em Instâncias Grandes
O SAP HANA em Instâncias Grandes *não* tem conectividade direta com a Internet. Por exemplo, essa limitação pode restringir a capacidade de registrar a imagem do SO diretamente com o fornecedor do SO. Talvez seja necessário trabalhar com o servidor da Ferramenta de Gerenciamento de Assinaturas do SUSE Linux Enterprise Server ou com o Gerenciador de Assinaturas do Red Hat Enterprise Linux.

## <a name="data-encryption-between-vms-and-hana-large-instance"></a>Criptografia de dados entre VMs e o SAP HANA em Instâncias Grandes
Os dados transferidos entre o SAP HANA em Instâncias Grandes e as VMs não são criptografados. No entanto, apenas para a troca entre o lado do DBMS do HANA e aplicativos baseados em JDBC/ODBC, é possível habilitar criptografia do tráfego. Para obter mais informações, consulta [esta documentação pelo SAP](http://help-legacy.sap.com/saphelp_hanaplatform/helpdata/en/db/d3d887bb571014bf05ca887f897b99/content.htm?frameset=/en/dd/a2ae94bb571014a48fc3b22f8e919e/frameset.htm&current_toc=/en/de/ec02ebbb57101483bdf3194c301d2e/plain.htm&node_id=20&show_children=false).

## <a name="use-hana-large-instance-units-in-multiple-regions"></a>Usar unidades do SAP HANA em Instâncias Grandes em várias regiões

Para perceber no-break conjunto de recuperação de desastre, você precisa ter unidades de instância grande do SHANA em várias regiões do Azure. Mesmo com o uso do Azure [emparelhamento de rede virtual Global], o roteamento transitivas por padrão não está funcionando entre locatários de instância grande do HANA em duas regiões diferentes. No entanto, alcance Global abre o caminho de comunicação entre as unidades de instância grande do HANA que você provisionou em duas regiões diferentes. Esse cenário de uso do ExpressRoute o alcance Global permite:

 - Replicação de sistema do HANA sem outros proxies ou firewalls
 - Cópia de backups entre unidades de instância grande do HANA em duas regiões diferentes para executar cópias do sistema ou atualizações do sistema


![Rede virtual conectada a carimbos da Instância Grande do Azure em diferentes regiões do Azure](./media/hana-overview-architecture/image8-multiple-regions.png)

A figura mostra como as redes virtuais diferentes em ambas as regiões estão conectadas a dois circuitos do ExpressRoute diferentes que são usados para se conectar ao SAP HANA no Azure (instâncias grandes) em ambas as regiões do Azure (linhas cinza). Motivo para essa duas conexões cruzadas é proteger contra uma interrupção dos MSEEs nos dois lados. O fluxo de comunicação entre as duas redes virtuais nas duas regiões do Azure deve ser tratado pela [emparelhamento global](https://blogs.msdn.microsoft.com/azureedu/2018/04/24/how-to-setup-global-vnet-peering-in-azure/) das duas redes virtuais em duas regiões diferentes (linha pontilhada azul). A linha vermelha espessa descreve a conexão ExpressRoute o alcance Global, que permite que as unidades de instância grande do HANA dos seus locatários em duas regiões diferentes para se comunicar entre si. 

> [!IMPORTANT] 
> Se foram utilizados vários circuitos do ExpressRoute, as configurações de prefixação do AS Path e de BGP de Preferência Local deverão ser usadas para garantir o roteamento apropriado do tráfego.

**Próximas etapas**
- Confira [Arquitetura de armazenamento do SAP HANA (Instâncias Grandes)](hana-storage-architecture.md)