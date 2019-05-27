---
title: Proteger arquitetura de computação do Azure
description: Essa arquitetura de referência para uma arquitetura de rede de Perímetro de nível empresarial usa dispositivos de rede virtual e outras ferramentas. Essa arquitetura foi projetada para atender Secure Cloud computação arquitetura requisitos do departamento de defesa funcionais. Ele também pode ser usado para qualquer organização. Essa referência inclui duas opções automatizadas que usam dispositivos Citrix ou F5.
author: jahender
ms.author: jahender
ms.date: 4/9/2019
ms.topic: article
ms.service: security
ms.openlocfilehash: 017a26d5672f666d4d8eaf629a0f53fe0cfe517f
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65963237"
---
# <a name="secure-azure-computing-architecture"></a>Proteger arquitetura de computação do Azure

EUA Os clientes do departamento de defesa (DoD) que implantem cargas de trabalho para o Azure solicitaram orientação configurar redes virtuais seguras e configurar as ferramentas de segurança e serviços que são estipulados pelo prática e padrões do DoD. 

Defesa Information System Agency (DISA) publicado a [documentos de requisitos funcionais Secure Cloud Computing Architecture (SCCA) (FRD)](https://iasecontent.disa.mil/stigs/pdf/SCCA_FRD_v2-9.pdf) em 2017. SCCA descreve os objetivos funcionais para proteger a defesa contra informações de sistema da rede (DISN) e a nuvem comercial pontos de conexão do provedor. SCCA também descreve como os proprietários de missão seguros nuvem aplicativos no limite da conexão. Cada entidade do DoD que se conecta à nuvem comercial deve seguir as diretrizes estabelecidas no FRD SCCA.
 
O SCCA tem quatro componentes:
 
- Limite de ponto de acesso na nuvem (BCAP)
- Pilha de segurança do Datacenter virtual (VDSS)
- Datacenter virtual Managed Service (VDMS)
- Gerenciador de credenciais de nuvem confiável (TCCM) 

Microsoft desenvolveu uma solução que atende aos requisitos de SCCA IL4 e IL5 cargas de trabalho que são executados no Azure. Essa solução específica do Azure é chamada de arquitetura de computação de Azure o Secure (SACA). Os clientes que implantam SACA estão em conformidade com o FRD SCCA. Eles podem habilitar os clientes do DoD mover cargas de trabalho para o Azure depois que eles estão conectados.

Arquiteturas e as diretrizes de SCCA são específicas para os clientes do DoD, mas as revisões mais recentes para os clientes civis de ajuda SACA estão em conformidade com diretrizes de conexão (TIC) de internet confiável. As revisões mais recentes também ajudam os clientes comerciais que deseja implementar uma DMZ segura para proteger seus ambientes do Azure.


## <a name="secure-cloud-computing-architecture-components"></a>Componentes de arquitetura de computação em nuvem seguros

### <a name="bcap"></a>BCAP

A finalidade de BCAP é proteger o DISN contra ataques que se originam no ambiente de nuvem. BCAP executa prevenção e detecção de intrusão. Ele também filtra tráfego não autorizado. Esse componente pode estar localizado junto com outros componentes do SCCA. É recomendável que você implante esse componente usando o hardware físico. Requisitos de segurança BCAP estão listados na tabela a seguir.

#### <a name="bcap-security-requirements"></a>Requisitos de segurança BCAP

![Matriz de requisitos BCAP](media/bcapreqs.png)


### <a name="vdss"></a>VDSS

A finalidade de VDSS é proteger aplicativos de missão-proprietário do DoD que são hospedados no Azure. VDSS executa a maior parte das operações de segurança a SCCA. Ele realiza a inspeção do tráfego para proteger os aplicativos executados no Azure. Esse componente pode ser fornecido dentro de seu ambiente do Azure.

#### <a name="vdss-security-requirements"></a>Requisitos de segurança VDSS

![Matriz de requisitos VDSS](media/vdssreqs.png)

### <a name="vdms"></a>VDMS

A finalidade do VDMS é fornecer a segurança do host e serviços do Centro de dados compartilhados. As funções de VDMS podem executar no hub de seu SCCA ou o proprietário de missão pode implantar partes em sua própria assinatura do Azure específica. Esse componente pode ser fornecido dentro de seu ambiente do Azure.

#### <a name="vdms-security-requirements"></a>Requisitos de segurança VDMS

![Matriz de requisitos VDMS](media/vdmsreqs.png)


### <a name="tccm"></a>TCCM

TCCM é uma função de negócios. Essa pessoa é responsável por gerenciar a SCCA. Suas obrigações são: 

- Estabelece políticas de acesso da conta para o ambiente de nuvem e planos. 
- Certifique-se de que o gerenciamento de identidades e acesso está operando corretamente. 
- Manter o plano de gerenciamento de credencial de nuvem. 

Essa pessoa é indicada pela autorização oficial. O BCAP, VDSS e VDMS fornecem os recursos que o TCCM precisa para realizar seu trabalho.

#### <a name="tccm-security-requirements"></a>Requisitos de segurança TCCM

![Matriz de requisitos TCCM](media/tccmreqs.png) 

## <a name="saca-components-and-planning-considerations"></a>Considerações de planejamento e os componentes SACA 

A arquitetura de referência SACA foi projetada para implantar os componentes VDSS e VDMS no Azure e habilitar o TCCM. Essa arquitetura é modular. Todas as partes de VDSS e VDMS podem residir em um hub centralizado. Alguns controles podem ser atendidas no espaço de missão-owner ou até mesmo localmente. A Microsoft recomenda que você localize os componentes VDSS e VDMS junto em uma rede virtual central que todos os proprietários de missão podem se conectar por meio do. O diagrama a seguir mostra essa arquitetura: 


![Diagrama de arquitetura de referência de SACA](media/sacav2generic.png)

Ao planejar sua estratégia de conformidade SCCA e arquitetura técnica, considere os seguintes tópicos desde o início, porque elas afetam todos os clientes. Os seguintes problemas surgem com os clientes do DoD e tendem a reduzir a velocidade de planejamento e execução. 

#### <a name="which-bcap-will-your-organization-use"></a>Quais BCAP será usado por sua organização?
   - DISA BCAP:
        - DISA tem dois BCAPs operacionais no Pentágono e em Camp Roberts, autoridade de certificação. Um terceiro está planejado para ficar online em breve. 
        - BCAPs do DISA todos os tem circuitos de ExpressRoute do Azure para o Azure, que pode ser usado por clientes do DoD para conectividade. 
        - DISA tem uma sessão de emparelhamento de Microsoft de nível empresarial para os clientes do DoD que deseja inscrever-se aos softwares da Microsoft como ferramentas de serviço (SaaS), como o Office 365. Usando o BCAP DISA, você pode habilitar a conectividade e o emparelhamento à sua instância SACA. 
    - Crie seu próprio BCAP:
        - Essa opção exige espaço em um centro de dados colocalizadas da concessão e configurar um circuito do ExpressRoute para o Azure. 
        - Essa opção requer aprovação adicional. 
        - Devido a uma aprovação adicional e um limite de compilação físico, essa opção leva mais tempo. 
    - É recomendável que você use o BCAP DISA. Essa opção estiver disponível prontamente, tem redundância interna e tem clientes operam nele atualmente em produção.
- Espaço IP roteável DoD:
    - Você deve usar o espaço IP roteável DoD em sua borda. A opção de usar o NAT para se conectar a esses espaços ao espaço IP privado no Azure está disponível.
    - Entre em contato com o Centro de informações de rede (NIC) da DoD para obter o espaço de IP. Você precisará dela como parte do seu envio do processo de aprovação de rede do sistema (SNAP) com DISA. 
    - Se você planeja usar o NAT para conectar-se o espaço de endereço privado no Azure, é necessário um mínimo de um/24 a sub-rede do espaço de endereço atribuído da NIC para cada região em que você planeja implantar SACA.
- Redundância:
    - Implante uma instância SACA em pelo menos duas regiões. Na nuvem do DoD, implantá-lo em ambas as regiões do DoD disponíveis.
    - Conecte-se pelo menos dois BCAPs por meio de circuitos de ExpressRoute separados. Ambas as conexões de ExpressRoute, em seguida, podem ser vinculadas a instância SACA de cada região. 
- Requisitos do DoD específicos do componente:
    - Sua organização tem requisitos específicos fora os requisitos de SCCA? Algumas organizações têm requisitos de IPS específicos.
- SACA é uma arquitetura modular:
    - Use somente os componentes que necessários para o seu ambiente. 
        - Implante soluções de virtualização de rede em uma camada única ou várias camadas.
        - Use IPS integrado ou traga seu próprio IPS.
- Nível de impacto de DoD de seus aplicativos e dados:
    - Se houver a possibilidade de aplicativos em execução no Microsoft IL5 regiões, crie sua instância SACA no IL5. A instância pode ser usada na frente de aplicativos IL4 e IL5. Uma instância de SACA IL4 na frente de um aplicativo de IL5 provavelmente não receberão capacitação.

#### <a name="which-network-virtual-appliance-vendor-will-you-use-for-vdss"></a>Qual fornecedor de solução de virtualização de rede você usará para VDSS?
Como mencionado anteriormente, você pode criar essa referência SACA usando uma variedade de dispositivos e serviços do Azure. Microsoft tenha automatizado os modelos de solução para implantar a arquitetura SACA com F5 e Citrix. Essas soluções são abordadas na seção a seguir.

#### <a name="which-azure-services-will-you-use"></a>Quais serviços do Azure você usará?
- Há serviços do Azure que podem atender aos requisitos para log analytics, a proteção baseada em host e a funcionalidade de IDS. É possível que alguns serviços não estão geralmente disponíveis nas regiões do Microsoft IL5. Nesse caso, talvez você precise usar ferramentas de terceiros se esses serviços do Azure não podem atender às suas necessidades. Examinar as ferramentas que você se sentir confortável com e a possibilidade de usar a capacidade das ferramentas nativa do Azure.
- É recomendável que você use ferramentas nativas do Azure tantos quanto possível. Eles são criados com base na segurança de nuvem e integram perfeitamente com o restante da plataforma Azure. Use as ferramentas nativas do Azure na lista a seguir para atender a vários requisitos de SCCA:

    - [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview )
    - [Central de Segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro) 
    - [Observador de Rede do Azure](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) 
    - [Cofre da Chave do Azure](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) 
    - [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)
    - [Application Gateway do Azure](https://docs.microsoft.com/azure/application-gateway/overview)
    - [Firewall do Azure](https://docs.microsoft.com/azure/firewall/overview) 
    - [Porta da frente do Azure](https://docs.microsoft.com/azure/frontdoor/front-door-overview)
    - [Grupos de segurança do Azure](https://docs.microsoft.com/azure/virtual-network/security-overview)
    - [Proteção contra DDoS do Azure](https://docs.microsoft.com/azure/virtual-network/ddos-protection-overview)
    - [Azure Sentinel](https://docs.microsoft.com/azure/sentinel/overview)
- Dimensionamento
    - Um exercício de dimensionamento deve ser concluído. Examinar o número de conexões simultâneas, que talvez você tenha por meio da instância SACA e os requisitos de taxa de transferência de rede. 
    - Esta etapa é essencial. Ele ajuda a dimensionar as VMs e identificar as licenças que são necessárias de vários fornecedores de que usar em sua instância SACA. 
    - Uma análise de custo BOM não pode ser feita sem esse exercício de dimensionamento. Também permite o dimensionamento correto para um melhor desempenho. 


## <a name="most-common-deployment-scenario"></a>Cenário de implantação mais comuns

 Vários clientes da Microsoft que passaram por meio da implantação completa ou no mínimo o fases de planejamento de seus ambientes de SACA. Suas experiências revelaram informações sobre o cenário mais comum de implantação. O diagrama a seguir mostra a arquitetura mais comuns: 


![Diagrama de arquitetura de referência de SACA](media/sacav2commonscenario.png) 


Como você pode ver no diagrama, os clientes do DoD normalmente inscrever-se a dois do BCAPs DISA. Um deles reside na Costa Oeste e o outro reside na Costa Leste. Um par de privado do ExpressRoute está habilitado para o Azure em cada local DISA BCAP. Esses pares de ExpressRoute, em seguida, são vinculados ao gateway de rede virtuais nas regiões do DoD Leste e Azure Central do DoD. Uma instância SACA é implantada nas regiões do DoD Leste e Azure Central do DoD. Todo o tráfego de entrada e saída flui através dele e para a conexão de ExpressRoute para o BCAP DISA.

Aplicativos de missão-proprietário, em seguida, escolha regiões do Azure em que planeja implantar seus aplicativos. Eles usam a rede virtual emparelhamento para se conectar a rede virtual do seu aplicativo para a rede virtual de SACA. Então, eles todo o tráfego por meio de instância VDSS de túnel de força.

Recomendamos essa arquitetura porque ele atende aos requisitos de SCCA. É altamente disponível e escalonável com facilidade. Isso também simplifica a implantação e gerenciamento.

## <a name="automated-saca-deployment-options"></a>Opções de implantação automatizadas de SACA

 Como mencionado anteriormente, Microsoft iniciou uma parceria com fornecedores de dois para criar um modelo de infraestrutura SACA automatizado. Os dois modelos de implantam os seguintes componentes do Azure: 

- Rede virtual de SACA
    - Subrede de gerenciamento
        - Essa sub-rede é onde o gerenciamento de VMs e serviços são implantados, também conhecido como um salto de caixas.
        - Sub-rede VDMS
            - Essa sub-rede é onde as VMs e serviços usados para VDMS são implantados.
        - Subredes não confiáveis e confiáveis
            - Essas sub-redes são em que os dispositivos virtuais são implantados.
        - Sub-rede de gateway
            - Essa sub-rede é onde o Gateway de ExpressRoute é implantado.
- Máquinas de virtuais de caixa de salto de gerenciamento
    - Eles são usados para gerenciamento fora de banda do ambiente.
- Dispositivos de rede virtual
    - Você usa o Citrix ou F5 com base em modelo que você implanta.
- IPs Públicos
    - Eles são usados para front-end até que o ExpressRoute é colocado online. Esses IPs traduzir para o espaço de endereço privado do Azure de back-end.
- Tabelas de rota 
    - Aplicado durante a automação, esses roteiam tabelas túnel à força todo o tráfego por meio do dispositivo virtual.
- Balanceadores de carga do Azure - SKU Standard
    - Eles são usados para balancear carga de tráfego entre os dispositivos.
- Grupos de segurança de rede
    - Eles são usados para controlar quais tipos de tráfego podem atravessar para determinados pontos de extremidade.


### <a name="citrix-saca-deployment"></a>Implantação do Citrix SACA

Um modelo de implantação do Citrix implanta duas camadas de dispositivos do Citrix ADC altamente disponíveis. Essa arquitetura atende aos requisitos de VDSS. 

![Diagrama de SACA Citrix](media/citrixsaca.png)


Para a documentação do Citrix e o script de implantação, consulte [este link do GitHub](https://github.com/citrix/netscaler-azure-templates/tree/master/templates/saca).


 ### <a name="f5-saca-deployment"></a>Implantação de F5 SACA

Dois modelos de implantação de F5 separados abrangem duas arquiteturas diferentes. O primeiro modelo tem apenas uma camada de dispositivos de F5 em uma configuração de ativo-ativo altamente disponível. Essa arquitetura atende aos requisitos do VDSS. O segundo modelo adiciona uma segunda camada de F5s de ativo-ativo altamente disponíveis. Essa segunda camada permite que os clientes adicionem seus próprios IPS separada da F5 entre as camadas de F5. Nem todos os componentes do DoD tenham prescrito para uso IPS específicos. Se esse for o caso, a única camada de dispositivos de F5 funciona para a maioria porque essa arquitetura inclui IPS nos dispositivos F5.

![Diagrama de SACA F5](media/f5saca.png)

Para a documentação de F5 e o script de implantação, consulte [este link do GitHub](https://github.com/f5devcentral/f5-azure-saca).












