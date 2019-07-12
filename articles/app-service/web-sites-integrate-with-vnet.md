---
title: Integrar o aplicativo com a rede Virtual do Azure - Serviço de Aplicativo do Azure
description: Mostra como conectar um aplicativo do Azure no Serviço de Aplicativo do Azure a uma rede virtual do Azure nova ou existente
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
ms.assetid: 90bc6ec6-133d-4d87-a867-fcf77da75f5a
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 940163d01e562d5a7d9107e8d893ba981fa0f84a
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795936"
---
# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Integrar seu aplicativo Web a uma Rede Virtual do Azure
Este documento descreve o recurso de integração de rede virtual do serviço de aplicativo do Azure e como configurá-lo com os aplicativos na [serviço de aplicativo do Azure](https://go.microsoft.com/fwlink/?LinkId=529714). [Redes virtuais do Azure][VNETOverview] (VNets) permitem que você colocar muitos dos recursos do Azure em uma rede roteável de fora da internet.  

O serviço de aplicativo do Azure tem duas variações. 

1. Os sistemas multilocatário que dão suporte a toda a gama de planos de preço, exceto Isolado
2. O aplicativo de serviço ASE (ambiente), que implanta em sua rede virtual e oferece suporte a aplicativos de plano de preços isolado

Este documento percorre os dois recursos de integração de rede virtual, que é para uso no serviço de aplicativo multilocatário. Se seu aplicativo está no [ambiente de serviço de aplicativo][ASEintro], em seguida, ele já está em uma rede virtual e não requer o uso do recurso Integração VNet para acessar recursos na mesma rede virtual. Para obter detalhes sobre todos os recursos de rede do serviço de aplicativo, leia [recursos de rede do serviço de aplicativo](networking-features.md)

Há duas formas para o recurso de integração de rede virtual

1. Uma versão permite a integração com redes virtuais na mesma região. Essa forma do recurso requer uma sub-rede em uma rede virtual na mesma região. Este recurso ainda está em visualização, mas há suporte para cargas de trabalho de produção de aplicativos Windows com algumas restrições indicadas abaixo.
2. A outra versão permite a integração com redes virtuais em outras regiões ou com redes virtuais clássicas. Esta versão do recurso requer a implantação de um Gateway de rede Virtual em sua rede virtual. Isso é o recurso com base em VPN de ponto a site e só é compatível com aplicativos do Windows.

Um aplicativo só pode usar uma forma do recurso de integração de rede virtual por vez. Em seguida, a pergunta é quais recursos você deve usar. Você pode usar qualquer uma para muitas coisas. Embora sejam os diferenciais claros:

| Problema  | Solução | 
|----------|----------|
| Deseja acessar um endereço de RFC 1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) na mesma região | Integração de VNet regional |
| Deseja alcançar recursos em uma rede virtual clássica ou uma rede virtual em outra região | Gateway necessário integração VNet |
| Deseja alcançar os pontos de extremidade de RFC 1918 em ExpressRoute | Integração de VNet regional |
| Deseja alcançar recursos nos pontos de extremidade de serviço | Integração de VNet regional |

Nenhum desses recursos permitem acessar endereços de RFC 1918 pelo ExpressRoute. Para fazer isso, você precisa usar um ASE por enquanto.

Usando a integração de rede virtual regional não conectar sua rede virtual no local ou configurar pontos de extremidade de serviço. Que é separado de configuração de rede. A integração de VNet regional simplesmente permite que seu aplicativo fazer chamadas entre esses tipos de conexão.

Independentemente da versão usada, integração VNet concede ao seu aplicativo web acesso a recursos em sua rede virtual, mas não concede acesso privado de entrada para seu aplicativo web da rede virtual. Acesso ao site privado significa tornar seu aplicativo acessível somente de uma rede privada, como de dentro de uma rede virtual do Azure. Integração VNet é apenas para fazer chamadas de saída do seu aplicativo em sua rede virtual. 

O recurso de integração VNet:

* requer um plano de preços Standard, Premium ou PremiumV2 
* dá suporte a TCP e UDP
* funciona com aplicativos de serviço de aplicativo e aplicativos de funções

Há algumas coisas a que a Integração VNet não dá suporte, incluindo:

* montagem de unidade
* integração ao AD 
* NetBios

## <a name="regional-vnet-integration"></a>Integração de VNet regional 

Quando a integração VNet é usada com redes virtuais na mesma região que seu aplicativo, ele requer o uso de uma sub-rede delegada com pelo menos 32 endereços nele. A sub-rede não pode ser usada para qualquer outra coisa. Chamadas de saída feitas a partir de seu aplicativo serão feitas de endereços na sub-rede do delegado. Quando você usa essa versão da integração VNet, as chamadas são feitas de endereços em sua rede virtual. Usando endereços na sua rede virtual permite que seu aplicativo:

* fazer chamadas para serviços protegidos do ponto de extremidade do serviço
* acessar recursos em conexões de ExpressRoute
* acessar recursos na VNet que você está conectado a
* acessar recursos em conexões emparelhadas, incluindo conexões do ExpressRoute

Esse recurso está em visualização, mas há suporte para cargas de produção de aplicativo do Windows com as seguintes limitações:

* Você só pode acessar os endereços que estão no intervalo RFC 1918. Esses são os endereços 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16 blocos de endereço.
* Você não pode acessar recursos em conexões de emparelhamento globais
* não é possível definir as rotas no tráfego proveniente de seu aplicativo em sua rede virtual
* o recurso só está disponível de unidades de escala de serviço de aplicativo mais recentes que dão suporte a planos de serviço de aplicativo PremiumV2.
* A sub-rede de integração só pode ser usada por apenas um plano de serviço de aplicativo
* o recurso não pode ser usado por aplicativos de plano isolado que estão em um ambiente de serviço de aplicativo
* O recurso requer uma sub-rede não utilizada que seja um/27 com 32 endereços ou mais em sua VNet do Resource Manager
* O aplicativo e a VNet devem estar na mesma região
* Você não pode excluir uma VNet com um aplicativo integrado. Você deve primeiro remover a integração 
* Você pode ter apenas uma integração de VNet regional por plano do serviço de aplicativo. Vários aplicativos no mesmo plano do serviço de aplicativo podem usar a mesma rede virtual. 

Um endereço é usado para cada instância de plano do serviço de aplicativo. Se você tiver dimensionado seu aplicativo para 5 instâncias, que é 5 endereços usados. Uma vez que o tamanho da sub-rede não pode ser alterado após a atribuição, você deve usar uma sub-rede que seja grande o suficiente para acomodar qualquer escala que seu aplicativo pode atingir. Um/27 com 32 endereços é o tamanho recomendado, pois que acomodará um plano de serviço de aplicativo Premium é dimensionado para 20 instâncias.

O recurso está em versão prévia também para Linux. Para usar o recurso Integração VNet com uma VNet do Resource Manager na mesma região:

1. Vá para a interface do usuário de rede no portal. Se seu aplicativo for capaz de usar o novo recurso, você verá uma opção para adicionar rede virtual (versão prévia).  

   ![Selecione a integração de rede virtual][6]

1. Selecione **Adicionar uma VNet (versão prévia)** .  

1. Selecione a VNet do Resource Manager que você deseja integrar e, em seguida, crie uma nova sub-rede ou escolha uma sub-rede vazia já existente. A integração leva menos de um minuto para ser concluída. Durante a integração, o aplicativo é reiniciado.  Quando a integração for concluída, você verá detalhes sobre a VNet que integrou, com uma faixa na parte superior que informa que o recurso está em versão prévia.

   ![Selecionar a VNet e a sub-rede][7]

Depois que seu aplicativo é integrado com sua rede virtual, ele usará o mesmo servidor DNS configurado com sua rede virtual. 

Para desconectar o aplicativo da VNet, selecione **Desconectar**. Isso reiniciará o aplicativo Web. 


#### <a name="web-app-for-containers"></a>Aplicativo Web para Contêineres

Se você usar o serviço de aplicativo no Linux com imagens internas, o recurso de integração de rede virtual regional funciona sem alterações adicionais. Se você usar o aplicativo Web para contêineres, você precisa modificar sua imagem do docker para usar a integração de rede virtual. Em sua imagem do docker, use a variável de ambiente de porta como porta de escuta do servidor web principal, em vez de usar um número de porta embutidos em código. A variável de ambiente PORT é definido automaticamente pela plataforma de serviço de aplicativo no tempo de inicialização do contêiner.

### <a name="service-endpoints"></a>Pontos de extremidade de serviço

O novo recurso de integração de VNet permite que você use pontos de extremidade de serviço.  Para usar pontos de extremidade de serviço com seu aplicativo, use a nova Integração VNet para se conectar a uma VNet selecionada e, em seguida, configure pontos de extremidade de serviço na sub-rede que você usou para a integração. 


### <a name="how-vnet-integration-works"></a>Como a Integração VNet funciona

Aplicativos no serviço de aplicativo são hospedados em funções de trabalho. O básico e posteriores de planos de preços são dedicados a planos de hospedagem onde houver outras clientes cargas de trabalho em execução no mesmos trabalhadores. Integração de rede virtual funciona montando interfaces virtuais com endereços na sub-rede do delegado. Porque a partir de endereço está em sua rede virtual, ele tem acesso para a maioria das coisas no ou por meio de sua rede virtual exatamente como faria uma VM em sua rede virtual. A implementação de rede é diferente de uma VM em execução na sua rede virtual e é por que alguns recursos de rede ainda não estão disponíveis ao usar esse recurso.

![Integração VNet](media/web-sites-integrate-with-vnet/vnet-integration.png)

Ao habilitar a integração VNet, seu aplicativo ainda fará as chamadas de saída à internet por meio dos canais mesmos como de costume. Os endereços de saída são listados no portal de propriedades do aplicativo ainda são os endereços usados pelo seu aplicativo. Quais são as alterações para seu aplicativo, chamadas para serviços ou endereços RFC 1918 protegidos do ponto de extremidade do serviço entra em sua rede virtual. 

O recurso dá suporte apenas a uma interface virtual por trabalho.  Uma interface virtual por trabalho significa uma integração de VNet regional por plano do serviço de aplicativo. Todos os aplicativos no mesmo plano do serviço de aplicativo podem usar a integração VNet com o mesmo, mas se você precisar de um aplicativo para se conectar a uma rede de virtual adicional, você precisará criar outro plano de serviço de aplicativo. A interface virtual usada não é um recurso que os clientes têm acesso direto ao.

Devido à natureza de como funciona essa tecnologia, o tráfego que é usado com a integração VNet não aparecem nos logs de fluxo do observador de rede ou NSG.  

## <a name="gateway-required-vnet-integration"></a>Gateway necessário integração VNet 

O Gateway necessário recurso Integração VNet:

* pode ser usado para se conectar a redes virtuais em qualquer região sejam eles Resource Manager ou clássicas
* permite que um aplicativo para se conectar à rede virtual apenas 1 por vez
* permite que até cinco VNets sejam integrados em um plano de serviço de aplicativo 
* permite que a mesma rede virtual a ser usado por vários aplicativos em um plano do serviço de aplicativo sem afetar o número total que pode ser usado por um plano de serviço de aplicativo.  Se você tiver 6 aplicativos que usam a mesma rede virtual no mesmo plano do serviço de aplicativo, que conta como 1 rede virtual que está sendo usado. 
* requer um Gateway de rede Virtual que está configurado com a VPN ponto a Site
* Não há suporte para uso com aplicativos do Linux
* Dá suporte a um SLA de 99,9% devido ao SLA no gateway

Esse recurso não oferece suporte:

* Acessando recursos no ExpressRoute 
* Acessando recursos entre Pontos de Extremidade de Serviço 

### <a name="getting-started"></a>Introdução

Veja aqui algumas coisas para se ter em mente antes de conectar seu aplicativo Web a uma rede virtual:

* Uma rede virtual de destino precisa ter a VPN ponto a site habilitada com um gateway baseado em rota antes que possa ser conectada a um aplicativo. 
* A VNet deve estar na mesma assinatura que o ASP (Plano do Serviço de Aplicativo).
* Os aplicativos que se integram a uma VNet usam o DNS especificado daquela VNet.

### <a name="set-up-a-gateway-in-your-vnet"></a>Configurar um gateway em sua VNet ###

Se você já tiver um gateway configurado com endereços de ponto a site, você poderá pular para a configuração da Integração VNet com seu aplicativo.  
Para criar um gateway:

1. [Criar uma sub-rede de gateway][creategatewaysubnet] em sua rede virtual.  

1. [Criar o gateway de VPN][creategateway]. Selecione um tipo VPN baseado em rota.

1. [Definir o ponto como endereços de sites][setp2saddresses]. Se o gateway não estiver no SKU básico, então o IKEV2 precisará ser desabilitado na configuração ponto a site e o SSTP deverá ser selecionado. O espaço de endereço deve ser nos blocos de endereços RFC 1918, 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16

Se você estiver apenas criando o gateway para usar com a integração de VNet do serviço de aplicativo, em seguida, você não precisará carregar um certificado. A criação do gateway pode levar 30 minutos. Você não poderá integrar seu aplicativo com sua VNet até que o gateway seja provisionado. 

### <a name="configure-vnet-integration-with-your-app"></a>Configurar a Integração VNet com o aplicativo 

Para habilitar a Integração VNet com o aplicativo: 

1. Vá para seu aplicativo no portal do Azure e abra as configurações do aplicativo e selecione Rede > Integração VNet. O ASP deve estar em uma SKU padrão ou melhor usar um desses recursos de integração de rede virtual. 
 ![Interface do usuário de Integração VNet][1]

1. Selecione **Adicionar VNet**. 
 ![Adicionar Integração VNet][2]

1. Selecione sua VNet. 
  ![Selecione sua VNet][8]
  
Seu aplicativo será reiniciado após essa última etapa.  

### <a name="how-the-gateway-required-vnet-integration-feature-works"></a>Funciona como o gateway deve recurso Integração VNet

O gateway necessário recurso Integração VNet baseia-se na tecnologia VPN ponto a site. A tecnologia ponto a site limita o acesso à rede apenas à máquina virtual que está hospedando o aplicativo. Os aplicativos são restritos apenas ao envio de tráfego para a Internet, por meio de Conexões Híbridas ou por meio da Integração VNet. 

![Como a Integração VNet funciona][3]

## <a name="managing-vnet-integration"></a>Gerenciar a integração de rede virtual
A capacidade de se conectar e desconectar de uma VNet está no nível dos aplicativos. As operações que podem afetar a Integração VNet entre vários aplicativos estão no nível do Plano do Serviço de Aplicativo. Do aplicativo > rede > portal de integração VNet, você pode obter detalhes sobre sua VNet. Você pode ver informações semelhantes no nível do ASP em ASP > rede > portal de integração de rede virtual, incluindo quais aplicativos nesse plano do serviço de aplicativo estiver usando uma determinada integração.

 ![Detalhes da VNet][4]

As informações disponíveis para você na interface do usuário de integração VNet é o mesmo entre os portais ASP e o aplicativo.

* Nome da VNet – links para a interface do usuário da rede virtual
* Localização – reflete a localização de sua VNet. A integração com uma rede virtual em outra localização pode causar problemas de latência para seu aplicativo. 
* Status do certificado – reflete se os certificados estão sincronizados entre o Plano do Serviço de Aplicativo e a VNet.
* Status do gateway – se você estiver usando o gateway necessário integração VNet, você poderá ver o status do gateway.
* Espaço de endereço da VNet – mostra o espaço de endereços IP para sua VNet. 
* Espaço de endereço ponto a site – mostra o espaço de endereços IP ponto a site para sua VNet. Ao fazer chamadas em sua rede virtual ao usar o recurso de gateway necessária, o endereço de seu aplicativo será um desses endereços. 
* Espaço de endereço site a site – você pode usar as VPNs site a site para conectar sua VNet a seus recursos locais ou a outras VNet. Os intervalos de IP definidos com essa conexão VPN são mostrados aqui.
* Servidores DNS – mostra os servidores DNS configurados com sua VNet.
* Endereços IP roteados para a rede virtual – mostra os blocos de endereços roteados usado para direcionar o tráfego em sua VNet 

A única operação que você pode executar no modo de exibição de aplicativo da Integração VNet é desconectar-se da VNet à qual seu aplicativo está atualmente conectado. Para desconectar o aplicativo de uma VNet, selecione **Desconectar**. Seu aplicativo será reiniciado quando você se desconectar de uma VNet. Desconectar-se não altera a VNet. A sub-rede ou gateway não é removido. Se você quiser excluir sua rede virtual, você precisa primeiro desconectar o aplicativo de rede virtual e excluir os recursos nela, como gateways. 

Para acessar a interface do usuário da Integração VNet do ASP, abra a interface do usuário do ASP e selecione **Rede**.  Na Integração VNet, selecione **Clique aqui para configurar** para abrir a interface do usuário do Status de Recurso de Rede.

![Informações da Integração VNet do ASP][5]

A interface do usuário da Integração VNet do ASP mostrará todas as redes virtuais que são usadas pelos aplicativos no seu ASP. Para ver detalhes em cada VNet, clique na VNet que lhe interessa. Existem duas ações que você pode executar aqui.

* **Sincronizar rede**. A operação de rede de sincronização é apenas para o recurso de integração de rede virtual dependentes do gateway. Executar uma operação de rede de sincronização garante que seus certificados e informações de rede estão em sincronia. Se você adicionar ou alterar o DNS da VNet, será necessário executar uma operação **Sincronizar rede**. Esta operação reiniciará todos os aplicativos usando essa VNet.
* **Adicionar rotas** A adição de rotas orientará o tráfego de saída em sua VNet.

**Roteamento** As rotas que são definidas em sua VNet são usadas para direcionar o tráfego para sua VNet vindo de seu aplicativo. Se você precisar enviar o tráfego de saída adicional para a VNet, você poderá adicionar esses blocos de endereço aqui. Este recurso só funciona com o gateway necessária integração VNet.

**Certificados** quando o gateway necessária a integração VNet foi habilitada, há uma troca de certificados para garantir a segurança da conexão necessária. Juntamente com os certificados, há a configuração do DNS, as rotas e outros itens semelhantes que descrevem a rede.
Se os certificados ou as informações de rede forem alterados, você precisará clicar em "Sincronizar Rede". Quando clica em "Sincronizar Rede", você causa uma breve interrupção na conectividade entre o aplicativo e a VNet. Embora seu aplicativo não seja reiniciado, a perda de conectividade pode fazer com que seu site não funcione corretamente. 

## <a name="accessing-on-premises-resources"></a>Como acessar recursos locais
Aplicativos podem acessar recursos locais ao integrarem-se com VNets que têm conexões site a site. Se você estiver usando o gateway necessário integração VNet, você precisará atualizar as rotas de gateway VPN local com seus blocos de endereço ponto a site. Quando o VPN site a site é configurado pela primeira vez, os scripts usados para configurá-lo devem configurar as rotas adequadamente. Se adicionar os endereços ponto a site depois de criar uma VPN site a site, você precisará atualizar as rotas manualmente. Os detalhes sobre como fazer isso variam de acordo com o gateway, e não são descritos aqui. Você não pode ter o BGP configurado com uma conexão de VPN site a site.

Não há nenhuma configuração adicional necessária para o recurso de integração de rede virtual regional acessar por meio de sua rede virtual e no local. Você só precisa se conectar a sua rede virtual no local usando uma VPN site a site ou ExpressRoute. 

> [!NOTE]
> O gateway necessário recurso Integração VNet não integra um aplicativo com uma rede virtual que tem um Gateway de ExpressRoute. Mesmo que o ExpressRoute Gateway esteja configurado no [modo de coexistência][VPNERCoex] the VNet Integration doesn't work. If you need to access resources through an ExpressRoute connection, then you can use the regional VNet Integration feature or an [App Service Environment][ASE], que é executado em sua rede virtual. 
> 
> 

## <a name="peering"></a>Emparelhamento
Se você estiver usando o emparelhamento com a integração de rede virtual regional, você não precisará fazer nenhuma configuração adicional. 

Se você estiver usando o gateway necessário integração VNet com o emparelhamento, você precisará configurar alguns itens adicionais. Para configurar o emparelhamento para funcionar com o aplicativo:

1. Adicione que uma conexão de emparelhamento na rede virtual à qual o aplicativo se conecta. Ao adicionar a conexão de emparelhamento, habilite **Permitir acesso à rede virtual** e marque **Permitir tráfego encaminhado** e **Permitir trânsito de gateway**.
1. Adicione uma conexão de emparelhamento na rede virtual que está sendo emparelhada à rede virtual à qual você está conectado. Ao adicionar a conexão de emparelhamento à VNet de destino, habilite **Permitir acesso à rede virtual** e marque **Permitir tráfego encaminhado** e **Permitir gateways remotos**.
1. Vá para o Plano do Serviço de Aplicativo > Rede > Interface do usuário da Integração VNet no portal.  Selecione a VNet à qual o aplicativo se conecta. Na seção de roteamento, adicione o intervalo de endereços da VNet que está emparelhada com a VNet à qual o aplicativo está conectado.  


## <a name="pricing-details"></a>Detalhes de preço
O recurso de integração de rede virtual regional não tem nenhum encargo adicional pelo uso além do ASP encargos de camada de preços.

Há três cobranças relacionadas ao uso do recurso de integração de rede virtual de gateway necessário:

* Encargos de camada preços do ASP - seus aplicativos precisam estar em uma Standard, Premium ou plano do serviço de aplicativo PremiumV2. Você pode ver mais detalhes sobre esses custos aqui: [Preços do serviço de aplicativo][ASPricing]. 
* Os custos de transferência de dados - lá é um encargo para a saída de dados, mesmo se a rede virtual estiver no mesmo data center. Esses encargos são descritos em [detalhes de preços de transferência de dados][DataPricing]. 
* Custos de Gateway de VPN - lá é um custo para o gateway de rede virtual é necessário para a VPN ponto a site. Os detalhes são sobre o [preços do Gateway de VPN][VNETPricing] página.


## <a name="troubleshooting"></a>Solução de problemas
Embora o recurso seja fácil de configurar, isso não significa que sua experiência estará livre de problemas. Se você encontrar problemas ao acessar o ponto de extremidade desejado, há utilitários que você pode usar para testar a conectividade do console do aplicativo. Há dois consoles que você pode usar. Uma é o console do Kudu e a outra é o console no portal do Azure. Para acessar o console do Kudu do aplicativo, vá para Ferramentas -> Kudu. Isso é o mesmo que ir para [nomedosite].scm.azurewebsites.net. Depois de aberto, vá para a guia do console Depuração. Para obter a console hospedado no portal do Azure, em seu aplicativo, vá para Ferramentas -> Console. 

#### <a name="tools"></a>Ferramentas
As ferramentas **ping**, **nslookup** e **tracert** não funcionarão no console devido a restrições de segurança. Para compensar essa ausência, duas ferramentas separadas foram adicionadas. Para testar a funcionalidade do DNS, adicionamos uma ferramenta chamada nameresolver.exe. A sintaxe é:

    nameresolver.exe hostname [optional: DNS Server]

Você pode usar **nameresolver** para verificar os nomes de host de que seu aplicativo depende. Dessa forma, você pode testar se não há nada mal configurado no DNS ou se não tem acesso ao seu servidor DNS. Você pode ver o servidor DNS que seu aplicativo usará no console do, observando as variáveis ambientais WEBSITE_DNS_SERVER e WEBSITE_DNS_ALT_SERVER.

A próxima ferramenta permite testar a conectividade do TCP de uma combinação de host e porta. Essa ferramenta é chamada **tcpping** e a sintaxe é:

    tcpping.exe hostname [optional: port]

O utilitário **tcpping** informa se você pode acessar um host específico e uma porta. Ele só pode mostrar êxito se: houver um aplicativo escutando na combinação de porta e host, e houver acesso de rede de seu aplicativo para o host e porta especificados.

#### <a name="debugging-access-to-vnet-hosted-resources"></a>Depurar o acesso a recursos hospedados por VNet
Há várias coisas que podem impedir que seu aplicativo alcance um host e uma porta específicos. Na maioria das vezes, é uma dentre três coisas:

* **Há um firewall no caminho.** Se houver um firewall no caminho, você atingirá o tempo limite de TCP. O tempo limite de TCP é 21 segundos neste caso. Use a ferramenta **tcpping** para testar a conectividade. Tempos limite de TCP podem ocorrer por muitos motivos, além de firewalls, mas comece com isso. 
* **O DNS não está acessível.** O tempo limite do DNS é de três segundos por servidor DNS. Se você tiver dois servidores DNS, o tempo limite será de seis segundos. Use nameresolver para ver se o DNS está funcionando. Lembre-se de que você não pode usar nslookup, pois ele não usa o DNS com o qual sua VNet está configurada. Se estiver inacessível, você poderia ter um firewall ou bloqueando o acesso NSG para DNS, ou ele pode estar inativo.

Se esses itens não resolverem seus problemas, procure primeiro para coisas como: 

**Integração de VNet regional**
* é o destino de um endereço de RFC 1918
* Há uma saída de bloqueio NSG da sub-rede de sua integração
* Se for em uma VPN ou ExpressRoute, é o seu gateway local configurado para rotear o tráfego de fazer backup no Azure? Se você pode acessar os pontos de extremidade em sua rede virtual, mas não em locais, isso é bom verificar.

**Gateway necessário integração VNet**
* é o intervalo de endereços de ponto a site em intervalos RFC 1918 (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255)?
* O Gateway é mostrado como estando em execução no Portal? Se o gateway estiver inativo, ative-o.
* Os certificados são mostrados como sincronizados ou se você suspeitar que a configuração de rede foi alterada?  Se os certificados estiverem fora de sincronização ou se você suspeitar de que houve uma alteração feita à sua configuração de rede virtual não foi sincronizada com seus ASPs, em seguida, pressione "Sincronizar rede".
* Se for em uma VPN ou ExpressRoute, é o seu gateway local configurado para rotear o tráfego de fazer backup no Azure? Se você pode acessar os pontos de extremidade em sua rede virtual, mas não em locais, isso é bom verificar.

Depuração de problemas de rede é um desafio, pois ali, você não pode ver o que está bloqueando o acesso a uma combinação de host: porta específica. Algumas das causas incluem:

* você tem um firewall no seu host que impede o acesso à porta do aplicativo usando o intervalo de IP ponto a site. O cruzamento de sub-redes geralmente exige acesso Público.
* o host de destino está inoperante
* seu aplicativo está inoperante
* você tinha o IP ou nome de host incorreto
* seu aplicativo está escutando em uma porta diferente da que você esperava. Você pode corresponder a sua ID de processo com a porta de escuta usando "netstat -aon" no host do ponto de extremidade. 
* os grupos de segurança de rede estão configurados de modo a impedir o acesso ao host do aplicativo e à porta do intervalo de IP ponto a site.

Lembre-se de que você não sabe qual endereço de seu aplicativo irá usar. Pode ser qualquer endereço no integração ponto a site ou sub-rede intervalo de endereços, portanto, você precisa permitir o acesso do intervalo de endereço inteiro. 

As etapas de depuração adicionais incluem:

* conecte-se a uma VM na sua VNet e tente acessar o host:porta do recurso de lá. Para testar o acesso TCP, use o comando do PowerShell **test-netconnection**. A sintaxe é:

      test-netconnection hostname [optional: -Port]

* colocar um aplicativo em uma VM e testar o acesso a esse host e porta do console do seu aplicativo usando **tcpping**

#### <a name="on-premises-resources"></a>Recursos locais ####

Se o aplicativo não puder acessar um recurso local, verifique se é possível acessar o recurso da sua VNet. Use o comando do PowerShell **test-netconnection** para verificar se há acesso TCP. Se sua VM não conseguir acessar seu recurso local, sua conexão VPN ou ExpressRoute pode não estar configurado corretamente.

Se a VM hospedada na VNet puder acessar seu sistema local, mas seu aplicativo não, isso provavelmente ocorrerá devido a um dos seguintes motivos:

* suas rotas não estão configuradas com sua sub-rede ou apontam para os intervalos de endereços de site em seu gateway local
* seus grupos de segurança de rede estão bloqueando o acesso para o intervalo de IP Ponto a Site
* os firewalls locais estão bloqueando o tráfego do intervalo de IP Ponto a Site
* Você está tentando acessar um endereço não RFC 1918 usando o recurso de integração de rede virtual regional


## <a name="powershell-automation"></a>Automação do PowerShell

Você pode integrar o Serviço de Aplicativo com uma Rede Virtual do Azure usando o PowerShell. Para um script pronto para uso, consulte [Conectar um aplicativo do Azure no Serviço de Aplicativo do Azure a uma Rede Virtual do Azure](https://gallery.technet.microsoft.com/scriptcenter/Connect-an-app-in-Azure-ab7527e3).


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-app.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-addvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-details.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-aspdetails.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-newvnet.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-newvnetdetails.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-selectvnet.png

<!--Links-->
[VNETOverview]: https://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: https://portal.azure.com/
[ASPricing]: https://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: https://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[ASEintro]: environment/intro.md
[ILBASE]: environment/create-ilb-ase.md
[V2VNETPortal]: ../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md
[VPNERCoex]: ../expressroute/expressroute-howto-coexist-resource-manager.md
[ASE]: environment/intro.md
[creategatewaysubnet]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#gatewaysubnet
[creategateway]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#creategw
[setp2saddresses]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal#addresspool
