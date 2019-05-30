---
title: Perguntas Frequentes – solução CloudSimple do VMware
description: Perguntas frequentes para solução de VMware do Azure por CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 05/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e727398e1b7bfa406166574ab40320c68dac5709
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358522"
---
# <a name="frequently-asked-questions-about-vmware-solution-by-cloudsimple"></a>Perguntas frequentes sobre a solução do VMware CloudSimple

Perguntas frequentes e respostas sobre a solução do VMware CloudSimple que ajudam você a entendem o serviço e como usá-lo.  As perguntas e respostas são organizadas nas categorias a seguir.

* Serviço CloudSimple
* Conectividade
* Rede
* Segurança
* Computação
* Armazenamento
* VMware
* Integração do Azure
  
## <a name="cloudsimple-service"></a>Serviço CloudSimple

**Qual é a solução VMware por CloudSimple?**

**Solução do VMware CloudSimple** transforma e estende as cargas de trabalho do VMware para nuvens privadas e dedicadas no Azure em minutos. Nós cuidamos do provisionamento, gerenciamento da infraestrutura e orquestrar as cargas de trabalho entre local e o Azure. Como seus aplicativos são executados exatamente o mesmo local e no Azure, você aproveitar a elasticidade e serviços de nuvem sem a complexidade de rearquitetar seus aplicativos. CloudSimple reduz o custo total de propriedade com um modelo de consumo de nuvem que fornece sob demanda provisionamento pague à medida que você se expandir e a otimização da capacidade.  Ver [qual é a solução VMware no Azure por CloudSimple](cloudsimple-vmware-solutions-overview.md) para recursos, benefícios e cenários.

**O que é uma nuvem privada CloudSimple?**

Você provisionar uma nuvem privada e dedicada consistindo de computação de alto desempenho, armazenamento e ambiente implantado na infraestrutura do Microsoft Azure (espaço de hardware e datacenter) de rede em locais do Azure.  Nuvem privada fornece uma plataforma de VMware nativa 'como um serviço'. Em termos de VMware, cada nuvem privada contém exatamente uma instância do vCenter Server. O vCenter Server gerencia vários nós de ESXi contidos em um ou mais Clusters do vSphere, juntamente com o armazenamento SAN Virtual (vSAN) correspondente. Um CloudSimple Service pode conter várias nuvens privadas em sua assinatura do Azure.  Para obter mais informações sobre nuvens privadas, consulte [visão geral da nuvem privada](cloudsimple-private-cloud.md).

**Em que o serviço de CloudSimple está disponível?**

CloudSimple está disponível nas regiões Leste dos EUA e Oeste dos EUA.

**Como habilitar o CloudSimple minha assinatura?**

Você pode contatar seu representante de conta da Microsoft em [ azurevmwaresales@microsoft.com ](mailto:azurevmwaresales@microsoft.com) para ativar sua assinatura para o serviço CloudSimple. Forneça sua ID de assinatura no email para o qual você deseja que o serviço CloudSimple habilitado.  

**Como acessar o portal do CloudSimple?**

Acessar o portal de CloudSimple do portal do Azure.  Ver [acessando a solução VMware pelo portal CloudSimple do portal do Azure](https://docs.azure.cloudsimple.com/access-cloudsimple-portal) artigo para obter detalhes sobre como acessar o portal de CloudSimple. 

**Como aumentar a capacidade para uma nuvem privada?**

Você compra nós do portal do Azure e expanda sua nuvem privada a partir do portal CloudSimple.  Você pode expandir sua nuvem privada, adicionar nós extras ao cluster existente do vSphere ou criando novo cluster do vSphere.  Ver [expandir uma nuvem privada CloudSimple](https://docs.azure.cloudsimple.com/expand-private-cloud) artigo para o procedimento.

**O que acontece com Meus nuvem privada durante a manutenção?**

CloudSimple fornece notificações periódicas de dias antes da manutenção agendada.  Manutenção é feita de forma ininterrupta para garantir a disponibilidade da sua nuvem privada.  Manutenção pode ser dos seguintes tipos:

1. **Infraestrutura CloudSimple:**  Infraestrutura CloudSimple foi projetada para ser altamente disponível.  Durante a manutenção, conectividade e a disponibilidade da sua nuvem privada é assegurada Atualizando componentes redundantes, um por vez sem qualquer impacto.  Você terá acesso a seu vCenter de nuvem privada, todas as máquinas virtuais, conexão de internet da sua nuvem privada e conexões no local ou no Azure.
2. **CloudSimple Portal:** Durante a manutenção, alguns recursos no portal de CloudSimple podem não estar acessível ou está desabilitado.  Notificação de manutenção incluirá os detalhes do que pode ser feito no portal.

## <a name="connectivity"></a>Conectividade

**Quais são minhas opções de conectividade à rede da região de CloudSimple?**

CloudSimple fornece três opções diferentes de conectividade para conectar-se à sua rede de região CloudSimple.  Todos os três podem ser usados juntos.

1. Conexão de ExpressRoute do seu datacenter local à rede de região CloudSimple - conexão privada segura em alta velocidade baixa latência ponte seu circuito do ExpressRoute local com seu circuito do ExpressRoute de CloudSimple usando alcance Global. Ver [conectar-se do local a CloudSimple usando o ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection) artigo para configurar a conexão.
2. Conexão de ExpressRoute de sua rede virtual do Azure à sua rede de região CloudSimple - conexão privada segura em alta velocidade baixa latência a ponte de sua rede virtual no Azure com seu circuito do ExpressRoute de CloudSimple usando gateways de rede virtual.  Ver [conectar-se o seu ambiente de nuvem privada CloudSimple à rede virtual do Azure usando o ExpressRoute](https://docs.azure.cloudsimple.com/azure-expressroute-connection) artigo para configurar a conexão.
3. Conexão de VPN site a Site do seu datacenter local à sua rede de região CloudSimple - proteger a rede virtual privada do dispositivo VPN local para sua região de nuvem privada CloudSimple.  Consulte [configurar a conexão VPN entre sua rede local e a nuvem privada de CloudSimple] o artigo para configurar a conexão VPN.

**Como conectar a uma nuvem privada?**

Você pode exibir detalhes da sua nuvem privada no Portal do CloudSimple. Para se conectar ao vCenter correspondente à sua nuvem privada, verifique se a conexão de rede é estabelecida usando o Site a Site, ponto a Site ou ExpressRoute. Em seguida, inicie o portal de CloudSimple do portal do Azure e clique *inicie o cliente vSphere* na Home page ou na página de detalhes da nuvem privada.

**O que é a vantagem de circuito de ExpressRoute?**

Circuito de ExpressRoute do Azure fornece conexão segura de alta velocidade baixa latência.  CloudSimple fornece um circuito dedicado do ExpressRoute por região por cliente.  Usando este circuito, você pode estabelecer uma conexão segura de locais e/ou sua assinatura do Azure.

**Quais são os custos de rede para conectar-se de/para CloudSimple. Quaisquer encargos de saída de/para CloudSimple para o Azure? Entre regiões?**

Não há nenhum custo para qualquer saída de rede.  As taxas padrão do Azure se aplicam a qualquer tráfego de saída de sua rede virtual ou de circuito de ExpressRoute do local.

## <a name="networking"></a>Rede

**Quais recursos de rede estão disponíveis para a minha nuvem privada?**

Você pode provisionar VLANs (e suas sub-redes), tabelas, de firewall e atribuir endereços IP públicos e mapear para uma máquina virtual em execução em sua nuvem privada.  Para obter mais informações, consulte [visão geral de VLANs/sub-redes](cloudsimple-vlans-subnets.md), [visão geral de tabelas de Firewall](cloudsimple-firewall-tables.md), e [visão geral do endereço IP público](cloudsimple-public-ip-address.md) artigos.

**Como configurar sub-redes diferentes para meus aplicativos na minha nuvem privada?**

Você pode criar VLANs em sua nuvem privada do seu CloudSimple Portal.  Depois de criar a VLAN, você pode criar um grupo de portas distribuído em seu vCenter de nuvem privada usando a VLAN e criar máquinas virtuais conectadas ao grupo de portas distribuído.  Você pode habilitar a tabela de firewall para a sub-rede/VLAN e definir regras de firewall para proteger o tráfego de rede.

**Quais configurações de firewall estão disponíveis para meu nuvens privadas?**

Você pode configurar regras para tráfego Norte-Sul e Leste-Oeste.  As regras são definidas em uma tabela de firewall.  A tabela de firewall pode ser anexada a VLANs em sua nuvem privada.  Ver [configurar tabelas de firewall e regras para nuvens privadas](https://docs.azure.cloudsimple.com/firewall) artigo para configurar o procedimento.

**Posso atribuir endereços IP públicos para VMs em meu ambiente de nuvem privada?**

No portal do CloudSimple, você pode facilmente alocar um novo endereço IP público e associá-lo a um endereço IP privado da sua máquina virtual ou um dispositivo.  Você também pode criar novas regras de firewall ou aplique regras de firewall existente para permitir que o tráfego de portas específicas e/ou específica do conjunto de endereços IP no Portal do. Ver [alocar endereços IP públicos para o ambiente de nuvem privada](https://docs.azure.cloudsimple.com/public-ips) para configurar o procedimento.

## <a name="security"></a>Segurança

**Quais são minhas opções de segurança em CloudSimple?**

Nuvem privada CloudSimple fornece os seguintes recursos de segurança para proteger o ambiente de nuvem privada:

1. **Dados com criptografia em repouso**: Você pode criptografar dados em repouso que residem no armazenamento vSAN na sua nuvem privada. vSAN dá suporte a servidor de gerenciamento de chaves externas, que pode ser implantado em seu ambiente de rede virtual ou no local do Azure.  Ver [configurar a criptografia de vSAN para sua nuvem privada CloudSimple](https://docs.azure.cloudsimple.com/vsan-encryption) para obter mais detalhes.
2. **Segurança de rede**: Controle baixo tráfego de rede de/para sua nuvem privada da Internet, local e em sub-redes da sua nuvem privada usando regras de firewall.
3. **Proteger a conexão privada**: Conexão privada segura entre sua rede local e sua assinatura do Azure.

## <a name="compute"></a>Computação

**Que tipo de hosts estão disponíveis?**

CloudSimple oferece dois tipos de host:

* **Nó CS28:** CPU:2 2,2 GHz, 28 núcleos no Total, 48 HT.  RAM: 256 GB.  Armazenamento: Cache do NVMe 1600 GB, 5760 GB de dados (todos os Flash). Rede: 2x25Gbe NIC
* **Nó CS36:** CPU 2 x 2.3 GHz, 36 Total de núcleos, 72 HT.  RAM: 512 GB.  Armazenamento: GB 3200 NVMe Cache 11.520 GB de dados (todos os Flash).  Rede: 2x25Gbe NIC

**Como as falhas de hardware são tratadas?**

Toda a infraestrutura de CloudSimple é monitorado continuamente pela plataforma CloudSimple e nossas equipes de operações de serviço.  Se for detectada uma falha de hardware, um novo nó é adicionado à sua nuvem privada e o nó com falha é removido para garantir a alta disponibilidade da sua nuvem privada.

## <a name="storage"></a>Armazenamento

**O tipo de armazenamento tem suporte em uma nuvem privada?**

Oferece CloudSimple **VMware flash All vSAN armazenamento** com cada uma nuvem privada.  Cada vSphere é criado com seu próprio repositório de dados vSAN.  Ver [componentes de VMware de nuvem privada – armazenamento vSAN](https://docs.azure.cloudsimple.com/vmware-components/#vsan-storage) artigo para obter mais detalhes.

**É a criptografia de dados com suporte?**
Sim.  Você pode configurar o armazenamento vSAN em sua nuvem privada para usar um servidor de gerenciamento de chaves (KMS), que é implantado localmente ou no Azure para a criptografia de dados armazenados em vSAN

**Como os discos com falha são tratados?**

Monitoramento de CloudSimple monitora continuamente a todos os componentes de hardware da nuvem privada.  Se for detectada qualquer falha de disco ou qualquer disco é identificado como com falha (com base na heurística), um novo nó é adicionado automaticamente para a nuvem privada.  O nó com falhas de disco é removido da nuvem privada.

## <a name="vmware"></a>VMware

**Como executar em grande escala upload/migrar aplicativos e dados do local?**

CloudSimple fornece uma solução nativa de vSphere VMware.  Qualquer ferramenta usada para a migração de dados em massa pode ser usada com a nuvem privada CloudSimple.  Algumas das opções disponíveis são:

1. HCX VMware para migração em massa de dados.
2. Migração fria de dados usando o Storage vMotion do local para CloudSimple.

**Posso instalar as ferramentas do VMware?**

CloudSimple fornece uma solução nativa de vSphere VMware.  Qualquer ferramenta usada para gerenciar o vSphere ambiente local pode ser usada em CloudSimple.  CloudSimple dá suporte ao modelo de Bring-Your-própria licença (BYOL) para instalar as ferramentas do VMware.

**Como são gerenciadas de atualizações?**

CloudSimple gerencia e atualiza todos os componentes da infraestrutura da sua nuvem privada de uma maneira perfeita de sem interrupções.  Qualquer atualização ou patch de segurança lançado por fornecedores de VMware ou infraestrutura será agendado para atualização assim que ela será qualificada por CloudSimple.

CloudSimple não executa upgrades ou atualizações de aplicativos instalados sobre a nuvem privada.

## <a name="azure-integration"></a>Integração do Azure

**Quais serviços do Azure têm suporte?**

CloudSimple fornece conexão de ExpressRoute do Azure à sua assinatura no Azure.  Todos os serviços em execução em sua assinatura tem conectividade de rede à sua nuvem privada e pode se conectar à sua nuvem privada.  Exemplos:

1. **O Azure Active Directory** como uma fonte de identidade para seu vCenter CloudSimple
2. **O armazenamento do Azure** para armazenar backups, imagens e outros dados da sua nuvem privada
3. **Aplicativos híbridos** -você pode criar uma arquitetura de aplicativo que abrange a nuvens públicas e privadas.  Por exemplo, você pode criar servidores Web no Azure que acessam aplicativos e servidores de banco de dados na nuvem privada CloudSimple.
4. **O Azure monitor** e **Central de segurança do Azure** -carga de trabalho em execução no VMware pode usá-los para o registro em log, métricas de desempenho e gerenciamento de segurança.

**Como faço para mapear Meus locatários de VMware para o Azure?**

CloudSimple fornece a capacidade exclusiva de gerenciar suas VMs do VMware na nuvem privada do portal do Azure.  Um pool de recursos do vCenter (configurado com as restrições de recurso desejado) pode ser mapeado para sua assinatura pelo administrador global.  

**Quais benefícios de licenciamento obter com o Azure?**

Com CloudSimple, você pode aproveitar o benefício de uso híbrido do Azure e economizar até 90% em licenças preserva seu investimento no Microsoft Licenses, reduzindo o TCO em comparação comparado outras nuvens. Além disso, você obter outras atualizações de segurança para Windows Server 2008 e Microsoft SQL Server 2008.  Manter os custos baixos com BYOL Traga sua própria licenças () para a nuvem para aplicativos comuns, como Veeam, Zerto e outros.  