---
title: Rede global Microsoft - Azure
description: Descreve como a Microsoft cria sua rede global rápida e confiável
services: networking
documentationcenter: ''
author: KumudD
manager: ''
ms.service: networking
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2019
ms.author: ypitsch,kumud
ms.openlocfilehash: 99650577b17874e61d1cede979d7ef2f4bfd491b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66247283"
---
# <a name="microsoft-global-network"></a>Rede global da Microsoft

A Microsoft possui e opera uma das maiores redes de backbone do mundo. Essa arquitetura global e sofisticada, abrangendo mais de 100.000 milhas, conecta-se nossos data centers e clientes. 
 
Todos os dias, os clientes em todo o mundo se conectar e passam trilhões de solicitações para o Microsoft Azure, Bing, do Dynamics 365, Office 365, XBox e muitos outros. Independentemente do tipo, os clientes esperam instantânea confiabilidade e capacidade de resposta de nossos serviços. 
 
O [a rede global Microsoft](https://azure.microsoft.com/global-infrastructure/global-network/) (longa distância WAN) é uma parte central do fornecimento de uma experiência excelente de nuvem. Conexão do nosso Microsoft [datacenters](https://azure.microsoft.com/global-infrastructure/) em 54 malha grande de nós de borda estrategicamente posicionados em todo o mundo e regiões do Azure, a nossa rede global oferece tanto a disponibilidade, capacidade e a flexibilidade para atender a qualquer demanda.

![Rede global da Microsoft](./media/microsoft-global-network/microsoft-global-wan.png)
 
## <a name="get-the-premium-cloud-network"></a>Obter a rede de nuvem premium
 
Aceitar para o [melhor experiência possível](https://www.sdxcentral.com/articles/news/azure-tops-aws-gcp-in-cloud-performance-says-thousandeyes/2018/11/) é fácil quando você usa a nuvem da Microsoft. A partir do momento quando o tráfego de cliente entra em nossa rede global por meio de nosso estrategicamente posicionados nós de borda, seus dados são transferidos através de rotas otimizadas no próximo a velocidade da luz. Isso garante a latência ideal para melhor desempenho. Esses nós do edge, todos interconectados para mais de 3500 exclusivo Internet parceiros (pares) por meio de milhares de conexões em mais de 145 locais, fornecem a base de nossa estratégia de interconexão. 
 
Se conectando de Londres para Tóquio ou de Washington, D.C. a Los Angeles, desempenho de rede é quantificado e afetado por fatores como latência, tremulação, perda de pacotes e taxa de transferência.  Na Microsoft, Preferimos e uso direto interconexões em vez de links de trânsito, isso mantém o tráfego de resposta simétrica e ajuda a manter os saltos, emparelhamento de duas partes e caminhos mais curto e simples possível. Essa abordagem premium, também conhecido como [frio Batata roteamento](https://en.wikipedia.org/wiki/Hot-potato_and_cold-potato_routing), garante que os clientes de rede permanece tráfego na rede da Microsoft quanto a possíveis antes, podemos transferi-lo.  
 
Portanto, isso significa que toda e qualquer tráfego ao usar os serviços da Microsoft? Sim, qualquer tráfego entre data centers, dentro do Microsoft Azure ou entre serviços da Microsoft, como máquinas virtuais, Office 365, XBox, bancos de dados SQL, armazenamento, e redes virtuais são roteadas dentro de nossa rede global e nunca na Internet pública, para garantir que o ideal desempenho e integridade.  
 
Grandes investimentos em capacidade de fibra e diversidade em caminhos de terrestre e submarino metro, são essenciais para que possamos manter consistente e de alto nível de serviço ao mesmo tempo que impulsiona o crescimento extremo de nossa nuvem e serviços online. Adições recentes para nossa rede global são nossos [MAREA](https://www.submarinecablemap.com/#/submarine-cable/marea) submarino cabo, primeiro abrir linha sistema (ferramentas do setor) sobre subsea entre Bilbao, Espanha e Virginia Beach, Virgínia, EUA, bem como o [AEC](https://www.submarinecablemap.com/#/submarine-cable/aeconnect-1)entre Nova York, EUA e Dublin, Irlanda e [cruzada novo do Pacífico (NCP)](https://www.submarinecablemap.com/#/submarine-cable/new-cross-pacific-ncp-cable-system) entre Tóquio, Japão e Portland, Oregon, EUA. 
 

## <a name="our-network-is-your-network"></a>Nossa rede é a sua rede

Colocamos duas décadas de experiência, além de grandes investimentos na rede garantir um desempenho ideal em todos os tempos. As empresas podem aproveitar ao máximo nosso ativos da rede e criem arquiteturas de sobreposição avançadas na parte superior. 
 
O Microsoft Azure oferece o portfólio mais avançado de serviços e recursos, permitindo que os clientes a criar com rapidez e facilidade, expandam e atender aos requisitos de rede em qualquer lugar. Nossa família de serviços de conectividade abranger o emparelhamento de rede virtual entre regiões, híbridos e na nuvem point-to-site e cenários de tráfego de arquiteturas de site a site, bem como IP global.  Para as empresas que buscam conectar-se o seu próprio datacenter ou rede no Azure ou os clientes com necessidades de trânsito, ou ingestão de dados em massa [ExpressRoute](../expressroute/expressroute-introduction.md), e [diretos do ExpressRoute](../expressroute/expressroute-erdirect-about.md) fornecem opções até 100 Gbps de largura de banda, diretamente na rede global da Microsoft em locais de emparelhamento em todo o mundo.  
 
[O alcance Global do ExpressRoute](../expressroute/expressroute-global-reach.md) foi projetado para complementar a implementação de WAN de seu provedor de serviços e conectar seus sites locais em todo o mundo. Se você executar uma operação global, você pode usar o alcance Global do ExpressRoute em conjunto com seus provedores de serviço preferenciais e local para se conectar a todos os seus sites globais usando a rede global da Microsoft. Expandir sua nova rede na nuvem de longa distância (WAN) para englobar um grande número de sites de filial pode ser feito por meio do Azure WAN Virtual, que oferece a capacidade de conectar diretamente o suas ramificações à rede global da Microsoft com dispositivos SDWAN & VPN (ou seja, Equipamentos de local do cliente ou CPE) com facilidade internos de uso e gerenciamento automatizado de conectividade e configuração. 
 
[O emparelhamento VNet global](../virtual-network/virtual-network-peering-overview.md) permite que os clientes conectar-se duas ou mais redes virtuais do Azure entre regiões perfeitamente. Uma vez emparelhadas, as redes virtuais aparecerão como um. O tráfego entre máquinas virtuais em redes virtuais emparelhadas é roteado por meio da infraestrutura de backbone da Microsoft, assim como o tráfego é roteado entre máquinas virtuais na mesma rede virtual - por meio de apenas endereços IP privados. 
 

## <a name="well-managed-using-software-defined-innovation"></a>Gerenciar usando inovação definida pelo software

Executando uma das nuvens do líderes do mundo, a Microsoft ganhou muita insight e experiência em [criando e gerenciando](https://myignite.techcommunity.microsoft.com/sessions/66668) infra-estrutura global de alto desempenho.  
 
Seguimos um conjunto robusto de princípios operacionais: 
 
- Use o hardware de comutação de ponta entre as várias camadas da rede.  
- Implante novos recursos com impacto zero para os usuários finais.  
- Implantar atualizações de forma segura e confiável na frota, o mais rápido possível. Horas em vez de semanas.  
- Utilizar detalhado de telemetria de escala de nuvem e totalmente automatizada de mitigação de falhas.  
- Use tecnologia de rede unificada e definida pelo software para controlar todos os elementos de hardware na rede.  Eliminação de duplicação e diminuir as falhas. 
 
Esses princípios se aplicam a todas as camadas da rede: funções no data center como balanceadores de carga, todo o caminho até a WAN com nossa plataforma de engenharia de tráfego e nossa redes ópticas de rede do host do adaptador de rede, plataforma de comutação.  
 
O crescimento exponencial do Azure e sua rede tiver atingido um ponto em que, eventualmente, percebemos que intuição humana não pode ser usada para gerenciar as operações de rede global. Para atender a necessidade de validar long, médio e alterações a curto prazo na rede, desenvolvemos uma plataforma para espelhar e emular nossa rede de produção synthetically. A capacidade de criar ambientes espelhados e executar milhões de simulações, nos permite testar software e alterações de hardware e seu impacto antes de confirmá-las para nossa plataforma de produção e a rede. 

## <a name="next-steps"></a>Próximas etapas
- [Saiba mais sobre os serviços de rede fornecidos no Azure](https://azure.microsoft.com/product-categories/networking/)
