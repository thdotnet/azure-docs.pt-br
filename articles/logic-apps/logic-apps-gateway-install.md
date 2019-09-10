---
title: Instalar o gateway de dados local-aplicativos lógicos do Azure
description: Antes de poder acessar dados nas instalações dos Aplicativos Lógicos do Azure, baixe e instale o gateway de dados local
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: arthii, LADocs
ms.topic: article
ms.date: 07/01/2019
ms.openlocfilehash: 657bc704e33e89b1646dffa6123a27169e6c317a
ms.sourcegitcommit: 65131f6188a02efe1704d92f0fd473b21c760d08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70860672"
---
# <a name="install-on-premises-data-gateway-for-azure-logic-apps"></a>Instalar o gateway de dados no local para os Aplicativos Lógicos do Azure

Antes de poder se conectar a fontes de dados locais dos Aplicativos Lógicos do Azure, baixe e instale o gateway de dados local em um computador local. O gateway funciona como uma ponte que fornece transferência de dados rápida e criptografia entre as fontes de dados locais (fora da nuvem) e os aplicativos lógicos. Você pode usar a mesma instalação de gateway com outros serviços de nuvem, como Power BI, Microsoft Flow, PowerApps e Azure Analysis Services. Para obter informações sobre como usar o gateway com esses serviços, consulte estes artigos:

* [Gateway de dados local do Microsoft Power BI](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
* [Gateway de dados local do Microsoft PowerApps](https://powerapps.microsoft.com/tutorials/gateway-management/)
* [Gateway de dados local do Microsoft Flow](https://flow.microsoft.com/documentation/gateway-manage/)
* [Gateway de dados local do Azure Analysis Services](../analysis-services/analysis-services-gateway.md)

Este artigo mostra como baixar, instalar e configurar seu gateway de dados local para que você possa acessar fontes de dados locais de aplicativos lógicos do Azure. Você também pode saber mais sobre [como o gateway de dados funciona](#gateway-cloud-service) mais adiante neste tópico.

<a name="supported-connections"></a>

O gateway dá suporte a [conectores locais](../connectors/apis-list.md#on-premises-connectors) nos Aplicativos Lógicos do Azure para essas fontes de dados:

* BizTalk Server 2016
* Sistema de arquivos
* IBM DB2  
* IBM Informix
* IBM MQ
* MySQL
* Oracle Database
* PostgreSQL
* SAP
* SharePoint Server
* SQL Server
* Teradata

Embora o gateway sozinho não incorra em custos adicionais, o [modelo de preços dos aplicativos lógicos](../logic-apps/logic-apps-pricing.md) se aplica a esses conectores e a outras operações nos aplicativos lógicos do Azure.

<a name="requirements"></a>

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure. Se você não tiver uma assinatura do Azure, [inscreva-se em uma conta gratuita do Azure](https://azure.microsoft.com/free/).

  * Você deve usar a mesma conta do Azure para instalar e administrar o gateway. Durante a instalação, você usa essa conta do Azure para associar o gateway em seu computador a uma assinatura do Azure. Posteriormente, você usará a mesma conta do Azure ao criar um recurso do Azure no portal do Azure para a instalação do gateway. 

  * Você deve entrar com uma conta corporativa ou de estudante, também conhecida como uma conta de *organização* , que se parece `username@contoso.com`com. Você não pode usar contas do Azure B2B (convidado) ou contas pessoais da @hotmail.com Microsoft, como ou. @outlook.com

    > [!TIP]
    > Se você se inscreveu para uma oferta do Office 365 e não forneceu seu endereço de email de trabalho, `username@domain.onmicrosoft.com`seu endereço pode ser semelhante a. Sua conta é armazenada em um locatário em um Azure Active Directory (Azure AD). Na maioria dos casos, o nome principal do usuário (UPN) para sua conta do Azure AD é o mesmo que seu endereço de email.
    >
    > Para usar uma [assinatura padrão do Visual Studio](https://visualstudio.microsoft.com/vs/pricing/) associada a um conta Microsoft, primeiro [crie um locatário no Azure ad](../active-directory/develop/quickstart-create-new-tenant.md)ou use o diretório padrão. Adicione um usuário com uma senha ao diretório, em seguida, forneça o acesso a esse usuário à sua assinatura. 
    > Em seguida, você pode entrar durante a instalação do gateway com esse nome de usuário e senha.

* Aqui estão os requisitos para o computador local:

  **Requisitos mínimos**

  * .NET Framework 4.6
  * Versão de 64 bits do Windows 7 ou Windows Server 2008 R2 (ou posterior)

  **Requisitos recomendados**

  * CPU com oito núcleos
  * Memória de 8 GB
  * versão de 64 bits do Windows Server 2012 R2 ou posterior
  * Armazenamento SSD (unidade de estado sólido) para o spooling

  > [!NOTE]
  > O gateway não dá suporte ao Windows Server 2016 Core.

* **Considerações relacionadas**

  * É possível instalar o gateway de dados local apenas em um computador local, não em um controlador de domínio. No entanto, não é necessário instalar o gateway no mesmo computador que a fonte de dados. Você precisa de apenas um gateway para todas as suas fontes de dados, portanto, não é necessário instalar o gateway para cada fonte de dados.

    > [!TIP]
    > Para minimizar a latência, você pode instalar o gateway o mais próximo possível da sua fonte de dados ou no mesmo computador, presumindo que você tenha permissões.

  * Instale o gateway em um computador que esteja em uma rede com fio, conectado à Internet, sempre ligado e não vá para o estado de suspensão. Caso contrário, o gateway não pode ser executado e o desempenho pode ser afetado em uma rede sem fio.

  * Se você planeja usar a autenticação do Windows, certifique-se de instalar o gateway em um computador que seja membro do mesmo ambiente de Active Directory que suas fontes de dados.

  * A região que você selecionar para a instalação do gateway é o mesmo local que você deve selecionar quando você cria posteriormente o recurso de gateway do Azure para seu aplicativo lógico. Por padrão, essa região é o mesmo local que o seu locatário do Azure AD que gerencia sua conta do Azure. No entanto, você pode alterar o local durante a instalação do gateway.

  * O gateway tem dois modos: modo padrão e modo pessoal, que se aplica somente a Power BI. Você não pode ter mais de um gateway em execução no mesmo modo no mesmo computador.

<a name="install-gateway"></a>

## <a name="install-data-gateway"></a>Instalar gateway de dados

1. [Baixe e execute o instalador do gateway em um computador local](https://aka.ms/on-premises-data-gateway-installer).

1. Depois que o instalador for aberto, selecione **Avançar**.

   ![Introdução ao instalador](./media/logic-apps-gateway-install/gateway-intro-screen.png)

1. Selecione **Gateway de dados local (recomendado)** , que é o modo padrão e, em seguida, selecione **Avançar**.

   ![Selecionar modo de gateway](./media/logic-apps-gateway-install/select-gateway-mode.png)

1. Examine os requisitos mínimos, mantenha o caminho de instalação padrão, aceite os termos de uso e, em seguida, selecione **instalar**.

   ![Revise os requisitos e aceite os termos de uso](./media/logic-apps-gateway-install/accept-terms.png)

1. Depois que o gateway for instalado com êxito, forneça o endereço de email para sua conta da organização e, em seguida, selecione **entrar**, por exemplo:

   ![Entrar com uma conta corporativa ou de estudante](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

   Agora você está conectado à sua conta.

1. Selecione **registrar um novo gateway neste computador** > em**seguida**. Essa etapa registra a instalação do gateway com o [serviço de nuvem do gateway](#gateway-cloud-service).

   ![Registrar gateway](./media/logic-apps-gateway-install/register-gateway.png)

1. Forneça essas informações para a instalação do gateway:

   * Um nome de gateway que é exclusivo em seu locatário do Azure AD
   * A chave de recuperação, que deve ter pelo menos oito caracteres, que você deseja usar
   * Confirmação para a chave de recuperação

   ![Configurar o gateway](./media/logic-apps-gateway-install/set-up-gateway.png)

   > [!IMPORTANT]
   > Salve e mantenha a chave de recuperação em um local seguro. Você precisará dessa chave se quiser alterar o local, mover, recuperar ou assumir uma instalação de gateway.

   Observe a opção para **Adicionar a um cluster de gateway existente**, que você seleciona ao instalar gateways adicionais para [cenários de alta disponibilidade](#high-availability).

1. Verifique a região para o serviço de nuvem do gateway e o [barramento de serviço do Azure](https://azure.microsoft.com/services/service-bus/) que é usado pela instalação do seu gateway. Por padrão, essa região é o mesmo local que o locatário do Azure AD para sua conta do Azure.

   ![Verificar a região](./media/logic-apps-gateway-install/check-region.png)

1. Para aceitar a região padrão, selecione **Configurar**. No entanto, se a região padrão não for aquela mais próxima de você, você poderá alterar a região.

   *Por que alterar a região de instalação do gateway?*

   Por exemplo, para reduzir a latência, é possível alterar a região do gateway para a mesma região que o aplicativo lógico. Ou então, é possível selecionar a região mais próxima à fonte de dados local. O *recurso de gateway no Azure* e o aplicativo lógico podem ter locais diferentes.

   1. Ao lado de região atual, selecione **Alterar Região**.

      ![Alterar região](./media/logic-apps-gateway-install/change-region.png)

   1. Na página seguinte, abra a lista **selecionar região** , selecione a região desejada e selecione **concluído**.

      ![Selecionar outra região](./media/logic-apps-gateway-install/select-region-gateway-install.png)

1. Examine as informações na janela de confirmação final. Este exemplo usa a mesma conta para aplicativos lógicos, Power BI, PowerApps e Microsoft Flow, portanto, o gateway está disponível para todos esses serviços. Quando estiver pronto, selecione **fechar**.

   ![Gateway concluído](./media/logic-apps-gateway-install/finished-gateway-default-location.png)

1. Agora, [crie o recurso do Azure para a instalação do gateway](../logic-apps/logic-apps-gateway-connection.md).

<a name="high-availability"></a>

## <a name="high-availability-support"></a>Suporte de alta disponibilidade

Para evitar pontos únicos de falha para acesso a dados local, você pode ter várias instalações de gateway (somente no modo padrão) com cada um em um computador diferente e configurá-las como um cluster ou grupo. Dessa forma, se o gateway primário não estiver disponível, as solicitações de dados serão roteadas para o segundo gateway e assim por diante. Como você pode instalar apenas um gateway padrão em um computador, você deve instalar cada gateway adicional que está no cluster em um computador diferente. Todos os conectores que trabalham com o gateway de dados local dão suporte à alta disponibilidade. 

* Você já deve ter a instalação de pelo menos um gateway na mesma assinatura do Azure como o gateway primário e a chave de recuperação para essa instalação.

* O gateway primário deve estar executando a atualização do gateway de novembro de 2017 ou posterior.

Depois de configurar o gateway primário, quando você for instalar outro gateway, selecione **Adicionar a um cluster de gateway existente**, selecione o gateway primário, que é o primeiro gateway que você instalou e forneça a chave de recuperação para esse gateway. Para obter mais informações, consulte [Clusters de alta disponibilidade para gateway de dados locais](https://docs.microsoft.com/data-integration/gateway/service-gateway-install#add-another-gateway-to-create-a-cluster).

<a name="update-gateway-installation"></a>

## <a name="change-location-migrate-restore-or-take-over-existing-gateway"></a>Alterar o local, migrar, restaurar ou assumir um gateway existente

Se for necessário alterar o local do gateway, mover a instalação do gateway para um novo computador, recuperar um gateway danificado ou assumir a propriedade de um gateway existente, você precisará da chave de recuperação que foi fornecida durante a instalação do gateway.

1. Execute o instalador do gateway no computador que tem o gateway existente. Se você não tiver o instalador do gateway mais recente, [Baixe a versão mais recente do gateway](https://aka.ms/on-premises-data-gateway-installer).

   > [!NOTE]
   > Antes de restaurar o gateway no computador que tem a instalação do gateway original, você deve primeiro desinstalar o gateway nesse computador. Essa ação desconecta o gateway original.
   > Se você remover ou excluir um cluster de gateway para qualquer serviço de nuvem, não poderá restaurar esse cluster.

1. Depois que o instalador for aberto, entre com a mesma conta do Azure que foi usada para instalar o gateway.

1. Selecione **migrar, restaurar ou tomada um gateway** > existente**em seguida**, por exemplo:

   ![Selecionar "Migrar, restaurar ou assumir um gateway existente"](./media/logic-apps-gateway-install/migrate-recover-take-over-gateway.png)

1. Selecione entre os clusters e gateways disponíveis e insira a chave de recuperação para o gateway selecionado, por exemplo:

   ![Selecionar gateway](./media/logic-apps-gateway-install/select-existing-gateway.png)

1. Para alterar a região, selecione **alterar região**e selecione a nova região.

1. Quando estiver pronto, selecione **Configurar** para que você possa concluir a tarefa.

## <a name="configure-proxy-or-firewall"></a>Configurar proxy ou firewall

Se o seu ambiente de trabalho exigir que o tráfego passe por um proxy para acessar a Internet, essa restrição poderá impedir que o gateway de dados local se conecte ao serviço de nuvem do gateway e ao [barramento de serviço do Azure](../service-bus-messaging/service-bus-messaging-overview.md). Para obter mais informações, consulte [definir configurações de proxy para o gateway de dados local](https://docs.microsoft.com/power-bi/service-gateway-proxy).

Para verificar se o proxy ou o firewall pode bloquear conexões, confirme se o computador pode realmente se conectar à Internet e ao barramento de serviço do Azure. Em um prompt do PowerShell, execute este comando:

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

> [!NOTE]
> Esse comando apenas testa a conectividade de rede e a conectividade com o Barramento de Serviço do Azure. O comando não tem qualquer relação com o gateway nem com o serviço de nuvem do gateway que criptografa e armazena as credenciais e detalhes do gateway. 
>
> Além disso, esse comando só está disponível no Windows Server 2012 R2 ou posterior e no Windows 8.1 ou posterior. Em versões anteriores do SO, você pode usar o Telnet para testar a conectividade. Saiba mais sobre [Soluções híbridas e de Barramento de Serviço do Azure](../service-bus-messaging/service-bus-messaging-overview.md).

Os resultados devem ser semelhantes a este exemplo com **TcpTestSucceeded** definido como **True**:

```text
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Se **TcpTestSucceeded** não for definido como **True**, o gateway pode ser bloqueado por um firewall. Se você quiser ser abrangente, substitua os valores de **ComputerName** e **Port** pelos listados em [Configurar portas](#configure-ports) neste artigo.

O firewall também pode bloquear conexões que o Barramento de Serviço do Azure faz com os datacenters do Azure. Se esse cenário ocorrer, aprove (desbloqueie) todos os endereços IP para os datacenters em sua região. Para esses endereços IP, [obtenha a lista de endereços IP do Azure aqui](https://www.microsoft.com/download/details.aspx?id=41653).

<a name="configure-ports"></a>

## <a name="configure-ports"></a>Configure portas

O gateway cria uma conexão de saída com o barramento de serviço do Azure e se comunica nas portas de saída: TCP 443 (padrão), 5671, 5672, 9350 a 9354. O gateway não exige portas de entrada. Saiba mais sobre [Soluções híbridas e de Barramento de Serviço do Azure](../service-bus-messaging/service-bus-messaging-overview.md).

O gateway usa estes nomes de domínio totalmente qualificados:

| Nomes de domínio | Portas de saída | Descrição |
| ------------ | -------------- | ----------- |
| *.analysis.windows.net | 443 | HTTPS |
| *.core.windows.net | 443 | HTTPS |
| *.frontend.clouddatahub.net | 443 | HTTPS |
| *.login.windows.net | 443 | HTTPS |
| *.microsoftonline p.com | 443 | Usado para autenticação, dependendo da configuração. |
| *.msftncsi.com | 443 | Usado para testar a conectividade com a Internet quando o gateway não puder ser acessado pelo serviço do Power BI. |
| *.servicebus.windows.net | 443, 9350-9354 | Ouvintes de Retransmissão do Barramento de Serviço por meio de TCP (requer 443 para aquisição de token de Controle de Acesso) |
| *.servicebus.windows.net | 5671-5672 | Advanced Message Queuing Protocol (AMQP) |
| login.microsoftonline.com | 443 | HTTPS |
||||

Em alguns casos, as conexões do Barramento de Serviço do Azure são feitas com o Endereço IP, em vez de nomes de domínio totalmente qualificados. Portanto, talvez você queira desbloquear os endereços IP para sua região de dados em seu firewall. Para permitir o acesso a endereços IP em vez de domínios, você pode baixar e usar a [lista de intervalos de IP Microsoft Azure datacenter](https://www.microsoft.com/download/details.aspx?id=41653). Os endereços IP nessa lista estão na notação [CIDR (Roteamento entre Domínios sem Classificação)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).

### <a name="force-https-communication-with-azure-service-bus"></a>Forçar comunicação HTTPS com o Barramento de Serviço do Azure

Alguns proxies permitem o tráfego somente para as portas 80 e 443. Por padrão, a comunicação com o Barramento de Serviço do Azure ocorre em portas diferentes da 443. Você pode forçar o gateway para comunicar-se com o Barramento de Serviço do Azure usando HTTPS em vez de TCP direto, mas fazer isso pode reduzir consideravelmente o desempenho. Para obter mais informações, consulte [forçar a comunicação HTTPS com o barramento de serviço do Azure](https://docs.microsoft.com/data-integration/gateway/service-gateway-communication#force-https-communication-with-azure-service-bus).

<a name="windows-service-account"></a>

## <a name="windows-service-account"></a>Conta de serviço do Windows

Por padrão, a instalação do gateway no computador local é executada como uma conta de serviço do Windows denominada "serviço de gateway de dados local". No entanto, a instalação do `NT SERVICE\PBIEgwService` gateway usa o nome para suas credenciais de conta "fazer logon como" e tem permissões para "fazer logon como um serviço".

> [!NOTE]
> Sua conta de serviço do Windows difere da conta usada para conectar-se a fontes de dados locais e da conta do Azure que você usa ao entrar nos serviços de nuvem.

<a name="restart-gateway"></a>

## <a name="restart-gateway"></a>Reiniciar o gateway

O gateway de dados é executado como um serviço Windows, então, da mesma forma como qualquer outro serviço Windows, é possível iniciar e parar o gateway de várias maneiras. Para obter mais informações, consulte [reiniciar um gateway de dados local](https://docs.microsoft.com/data-integration/gateway/service-gateway-restart).

## <a name="tenant-level-administration"></a>Administração no nível do locatário

Para obter visibilidade de todos os gateways de dados locais em um locatário do Azure AD, os administradores globais nesse locatário podem entrar no centro de [Administração do Power Platform](https://powerplatform.microsoft.com) como administrador de locatários e selecionar a opção de **gateways de dados** . Para obter mais informações, consulte [administração em nível de locatário para o gateway de dados local](https://docs.microsoft.com/data-integration/gateway/service-gateway-tenant-level-admin).

<a name="gateway-cloud-service"></a>

## <a name="how-the-gateway-works"></a>Como funciona o gateway

O gateway de dados facilita a comunicação rápida e segura entre seu aplicativo lógico, o serviço de nuvem do gateway e sua fonte de dados local. O serviço de nuvem do gateway criptografa e armazena suas credenciais da fonte de dados e os detalhes do gateway. O serviço também encaminha consultas e seus resultados entre seu aplicativo lógico, o gateway de dados local e sua fonte de dados local.

O gateway funciona com firewalls e usa apenas conexões de saída. Todo o tráfego é originado como tráfego de saída seguro do agente de gateway. O gateway retransmite dados de fontes locais em canais criptografados por meio do barramento de serviço do Azure. O barramento de serviço cria um canal entre o gateway e o serviço de chamada, mas não armazena nenhum dado. Todos os dados que trafegam através do gateway são criptografados.

![Arquitetura do gateway de dados local](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

Estas etapas descrevem o que acontece quando um usuário na nuvem interage com um elemento que está conectado à fonte de dados local:

1. O serviço de nuvem do gateway cria uma consulta, juntamente com as credenciais criptografadas para a fonte de dados, e envia a consulta à fila para processamento pelo gateway.

1. O serviço de nuvem do gateway analisa a consulta e envia a solicitação por push ao Barramento de Serviço do Azure.

1. O gateway de dados local sonda o Barramento de Serviço do Azure para verificar solicitações pendentes.

1. O gateway obtém a consulta, descriptografa as credenciais e conecta-se à fonte de dados com essas credenciais.

1. O gateway envia a consulta à fonte de dados para execução.

1. Os resultados são enviados da fonte de dados de volta ao gateway e, em seguida, para o serviço de nuvem do gateway. O serviço de nuvem do gateway então usa os resultados.

<a name="faq"></a>

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="general"></a>Geral

**P**: Preciso de um gateway para fontes de dados na nuvem, como o Banco de Dados SQL do Azure? <br/>
**R**: Não, o gateway conecta-se apenas a fontes de dados locais.

**P**: O gateway precisa ser instalado no mesmo computador que a fonte de dados? <br/>
**R**: Não, o gateway se conecta à fonte de dados usando as informações de conexão fornecidas. Considere o gateway como um aplicativo de cliente nesse sentido. O gateway precisa apenas da capacidade de se conectar ao nome do servidor que foi fornecido.

<a name="why-azure-work-school-account"></a>

**P**: Por que eu devo usar uma conta corporativa ou de estudante para entrar? <br/>
**R**: Você somente pode usar um conta corporativa ou de estudante quando instalar o gateway de dados local. Sua conta de entrada é armazenada em um locatário gerenciado pelo Azure AD (Azure Active Directory). Geralmente, o nome UPN da conta do Azure AD corresponde ao endereço de email.

**P**: Em que local minhas credenciais são armazenadas? <br/>
**R**: As credenciais inseridas para uma fonte de dados são criptografadas e armazenadas no serviço de nuvem do gateway. As credenciais são descriptografadas no gateway de dados local.

**P**: Há algum requisito de largura de banda de rede? <br/>
**R**: Verifique se a conexão de rede tem boa taxa de transferência. Cada ambiente é diferente e a quantidade de dados enviados pode afetar os resultados. Para garantir um nível de taxa de transferência entre a fonte de dados local e os datacenters do Azure, tente usar o [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/). Para ajudar a medir a taxa de transferência, tente usar uma ferramenta externa como o Azure Speed Test.

**P**: Qual é a latência para execução de consultas para uma fonte de dados do gateway? Qual é a melhor arquitetura? <br/>
**R**: Para reduzir a latência de rede, instale o gateway o mais próximo possível da fonte de dados. Se você puder instalar o gateway na fonte de dados real, essa proximidade minimizará a latência introduzida. Além disso, considere a proximidade aos datacenters do Azure. Por exemplo, se o serviço usar o datacenter Oeste dos EUA e o SQL Server estiver hospedado em uma VM do Azure, a VM do Azure também deverá estar na região Oeste dos EUA. Essa proximidade minimiza a latência e evita encargos de saída na VM do Azure.

**P**: Como os resultados são enviados para a nuvem? <br/>
**R**: Os resultados são enviados por meio do Barramento de Serviço do Azure.

**P**: Existem conexões de entrada para o gateway da nuvem? <br/>
**R**: Não, o gateway usa conexões de saída para o Barramento de Serviço do Azure.

**P**: O que acontecerá se eu bloquear conexões de saída? O que preciso abrir? <br/>
**R**: Verifique as portas e os hosts que o gateway usa.

**P**: Qual é o serviço Windows que é de fato chamado? <br/>
**R**: Na guia Serviços no Gerenciador de Tarefas, o nome do serviço é "PBIEgwService" ou Power BI Enterprise Gateway Service. No console Serviços, o nome do serviço é "Serviço de gateway de dados local". O serviço Windows usa "NT SERVICE\PBIEgwService" como o SSID (SID de Serviço).

**P**: O serviço Windows do gateway pode ser executado com uma conta do Azure Active Directory? <br/>
**R**: Não, o serviço Windows deve ter uma conta do Windows válida.

### <a name="disaster-recovery"></a>Recuperação de desastres

**P**: Quais opções estão disponíveis para recuperação de desastre? <br/>
**R**: Você pode usar a chave de recuperação para restaurar ou mover um gateway. Ao instalar o gateway, especifique a chave de recuperação.

**P**: Qual é o benefício da chave de recuperação? <br/>
**R**: A chave de recuperação oferece uma maneira de migrar ou recuperar as configurações do gateway após um desastre.

## <a name="troubleshooting"></a>Solução de problemas

Esta seção aborda alguns problemas comuns que você pode ter ao configurar e usar o gateway de dados local.

**P**: Por que minha instalação do gateway falhou? <br/>
**R**: Esse problema poderá ocorrer se o software antivírus no computador de destino estiver desatualizado. Você pode atualizar ou desabilitar o software antivírus, mas somente durante a instalação do gateway e, em seguida, habilitar o software novamente.

**P**: Por que não vejo minha instalação do gateway ao criar o recurso de gateway no Azure? <br/>
**R**: Esse problema pode ocorrer pelos seguintes motivos:

* Sua instalação do gateway já foi registrada e declarada por outro recurso de gateway no Azure. Instalações do gateway não aparecem na lista de instâncias depois que os recursos de gateway são criados para elas. Para verificar os registros de gateway no portal do Azure, examine todos os seus recursos do Azure com o tipo dos **Gateways de dados locais** para *toda as* assinaturas do Azure.

* A identidade do Azure AD da pessoa que instalou o gateway é diferente da pessoa que entrou no portal do Azure. Verifique se você está conectado com a mesma identidade que instalou o gateway.

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

**P**: Em que local estão os logs do gateway? <br/>
**R**: Consulte a seção [**Logs** mais](#logs) adiante neste artigo.

**P**: Como posso ver quais consultas estão sendo enviadas à fonte de dados local? <br/>
**R**: Você pode habilitar o rastreamento de consulta, que inclui as consultas que são enviadas. Lembre-se de alterar o rastreamento de consulta de volta para o valor original quando concluir a solução de problemas. Deixar o acompanhamento de consulta ativado cria logs maiores.

Você também pode examinar ferramentas de rastreamento de consultas disponibilizadas por sua fonte de dados. Por exemplo, você pode usar o Extended Events ou o SQL Profiler para SQL Server e Analysis Services.

### <a name="outdated-gateway-version"></a>Versão do gateway desatualizada

Muitos problemas podem surgir quando a versão do gateway fica desatualizada. Como prática geral recomendada, certifique-se de ter a última versão. Se você não atualiza o gateway há um mês ou mais, convém instalar a versão mais recente do gateway e verificar se o problema pode ser reproduzido.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>Erro: Falha ao adicionar usuário ao grupo. (-2147463168 PBIEgwService Performance Log Users)

Você poderá obter esse erro se tentar instalar o gateway em um controlador de domínio, o que não tem suporte. Certifique-se de implantar o gateway em um computador que não seja um controlador de domínio.

<a name="logs"></a>

### <a name="logs"></a>Logs

Para ajudá-lo a solucionar problemas, sempre comece coletando e examinando os logs do gateway. Há várias maneiras de coletar os logs, mas a opção mais simples, após instalar o gateway, é por meio da interface do usuário do instalador do gateway.

1. Em seu computador, abra o instalador do gateway de dados local.

1. No menu esquerdo, selecione **Diagnósticos**.

1. Em **logs de Gateway**, selecione **Exportar logs**.

   ![Exportar logs do instalador do gateway](./media/logic-apps-gateway-install/export-logs.png)

Aqui estão outros locais onde é possível encontrar vários logs:

| Tipo de log | Location |
|----------|----------|
| **Logs do instalador** | %localappdata%\Temp\On-premises_data_gateway_<*yyyymmdd*>.<*number*>.log |
| **Logs de configuração** | C:\Users\<*username*>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator<*yyyymmdd*>.<*number*>.log |
| **Logs de serviço do gateway corporativo** | C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\Gateway<*yyyymmdd*>.<*number*>.log |
|||

**Logs de eventos**

Para localizar os logs de eventos do gateway, siga estas etapas:

1. No computador com a instalação do gateway, abra o **Visualizador de Eventos**.

1. Expanda o **Visualizador de Eventos (Local)**  > **Logs de Aplicativos e Serviços**.

1. Selecione **Serviço de gateway de dados local**.

   ![Exibir logs de eventos do gateway](./media/logic-apps-gateway-install/event-viewer.png)

### <a name="review-slow-query-performance"></a>Examinar desempenho de consulta lenta

Se você achar que as consultas são executadas lentamente por meio do gateway, é possível ativar o registro em log adicional que emite consultas e suas durações. Esses logs podem ajudá-lo a descobrir quais consultas são lentas ou de longa execução. Para ajustar o desempenho de consultas, talvez seja necessário modificar a fonte de dados, por exemplo, ajustar índices para consultas do SQL Server.

Para determinar a duração de uma consulta, siga estas etapas:

1. Navegue até o mesmo local que o cliente do gateway, que geralmente é possível encontrar aqui: ```C:\Program Files\On-premises data gateway```

   Caso contrário, para encontrar o local do cliente, abra o console de Serviços no mesmo computador, localize **Serviço de gateway de dados local** e exiba a propriedade **Caminho do executável**.

1. Abra e edite esses arquivos de configuração, conforme descrito:

   * **Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config**

     Nesse arquivo, altere o valor de **EmitQueryTraces** de **false** para **true** para que o gateway possa registrar consultas enviadas do gateway para uma fonte de dados:

     ```html
     <setting name="EmitQueryTraces" serializeAs="String">
        <value>true</value>
     </setting>
     ```

     > [!IMPORTANT]
     > Ativar a configuração EmitQueryTraces pode aumentar significativamente o tamanho do log com base no uso do gateway. Ao terminar a revisão dos logs, certifique-se de redefinir EmitQueryTraces para **false** novamente, em vez de deixar essa configuração em longo prazo.

   * **Microsoft.PowerBI.DataMovement.Pipeline.Diagnostics.dll.config**

     Para que o gateway registre entradas detalhadas, incluindo as entradas que mostram a duração, altere o valor **TracingVerbosity** de **4** para **5** executando qualquer etapa:

     * Nesse arquivo de configuração, altere o valor **TracingVerbosity** de **4** para **5**

       ```html
       <setting name="TracingVerbosity" serializeAs="String">
          <value>5</value>
       </setting>
       ```

     * Abra o instalador do gateway, selecione **Diagnósticos**, ative **Log adicional** e, em seguida, escolha **Aplicar**:

       ![Ativar registro em log adicional](./media/logic-apps-gateway-install/turn-on-additional-logging.png)

     > [!IMPORTANT]
     > Ativar a configuração TracingVerbosity pode aumentar significativamente o tamanho do log com base no uso do gateway. Ao terminar a revisão dos logs, certifique-se de desativar **Registro em log adicional** no instalador do gateway ou redefinir o TracingVerbosity para **4** novamente no arquivo de configuração, em vez de deixar essa configuração em longo prazo.

1. Para descobrir a duração de uma consulta, siga estas etapas:

   1. [Exporte](#logs) e abra o log de gateway.

   1. Para localizar uma consulta, pesquise um tipo de atividade, por exemplo:

      | Tipo de atividade | Descrição |
      |---------------|-------------|
      | MGEQ | Consultas que são executadas em ADO.NET |
      | MGEO | Consultas que são executadas sobre OLEDB |
      | MGEM | Consultas executadas a partir do mecanismo de mashup |
      |||

   1. Observe o segundo GUID, que é a ID da Solicitação.

   1. Continue pesquisando o tipo de atividade até encontrar uma entrada nomeada "FireActivityCompletedSuccessfullyEvent" que tem uma duração em milissegundos. Confirme se a entrada tem a mesma ID da Solicitação, por exemplo:

      ```text
      DM.EnterpriseGateway Verbose: 0 : 2016-09-26T23:08:56.7940067Z DM.EnterpriseGateway    baf40f21-2eb4-4af1-9c59-0950ef11ec4a    5f99f566-106d-c8ac-c864-c0808c41a606    MGEQ    21f96cc4-7496-bfdd-748c-b4915cb4b70c    B8DFCF12 [DM.Pipeline.Common.TracingTelemetryService] Event: FireActivityCompletedSuccessfullyEvent (duration=5004)
      ```

      > [!NOTE]
      > A entrada "FireActivityCompletedSuccessfullyEvent" é uma entrada detalhada e não é registrada, a menos que a configuração "TracingVerbosity" esteja no nível 5.

### <a name="trace-traffic-with-fiddler"></a>Rastrear tráfego com o Fiddler

[Fiddler](https://www.telerik.com/fiddler) é uma ferramenta gratuita da Telerik que monitora o tráfego HTTP. Você pode examinar esse tráfego com o serviço do Power BI da máquina cliente. Esse serviço pode mostrar erros e outras informações relacionadas.

## <a name="next-steps"></a>Próximas etapas

* [Conectar-se a dados locais de aplicativos lógicos](../logic-apps/logic-apps-gateway-connection.md)
* [Recursos de integração corporativa](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Conectores de Aplicativos Lógicos do Azure](../connectors/apis-list.md)
