---
title: Conectar redes virtuais do Azure de Aplicativos Lógicos do Azure por meio de um ambiente do serviço de integração (ISE)
description: Criar um ISE (Ambiente de Serviço de Integração) para que os aplicativos lógicos e contas de integração possam acessar as VNets (redes virtuais) do Azure, enquanto permanecem privados e isolados do Azure público ou “global”
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: conceptual
ms.date: 05/20/2019
ms.openlocfilehash: bd1f06c93a75673f86f0c52f78cad8a60f7a1a1e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65961457"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>Conectar redes virtuais do Azure a partir dos Aplicativos Lógicos do Azure, usando um ISE (Ambiente de Serviço de Integração)

Para cenários em que seus aplicativos lógicos e contas de integração precisam de acesso a uma [Rede virtual do Azure](../virtual-network/virtual-networks-overview.md), crie um [*ISE* (Ambiente de Serviço de Integração)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md). Um ISE é um ambiente privado e isolado que usa armazenamento dedicado e outros recursos que são mantidos separados do serviço de aplicativos lógicos "global" ou público. Essa separação também reduz os impactos que outros locatários do Azure podem ter no desempenho de seus aplicativos. Seu ISE é *injetado* na rede virtual do Azure, que, em seguida, implanta o serviço de Aplicativos Lógicos em sua rede virtual. Quando cria aplicativos lógicos e contas de integração, você seleciona esse ISE como sua localização. Sua conta de integração ou de aplicativo lógico, em seguida, pode acessar diretamente os recursos, como máquinas virtuais (VMs), servidores, sistemas e serviços em sua rede virtual.

![Selecionar o ambiente de serviço de integração](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

Este artigo mostra como concluir essas tarefas:

* Verifique se que quaisquer portas necessárias em uma rede virtual estejam abertas para que o tráfego pode percorrer seu ambiente do serviço de integração (ISE) entre as sub-redes na rede virtual.

* Crie seu ISE (ambiente de serviço de integração).

* Crie um aplicativo lógico que pode ser executado no ISE.

* Crie uma conta de integração para seus aplicativos lógicos no ISE.

Para obter mais informações sobre os ambientes do serviço de integração, confira [Acesso aos recursos da Rede Virtual do Azure dos Aplicativos Lógicos do Azure](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md).

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure. Se você não tiver uma assinatura do Azure, <a href="https://azure.microsoft.com/free/" target="_blank">inscreva-se em uma conta gratuita do Azure</a>.

  > [!IMPORTANT]
  > Aplicativos lógicos, gatilhos internos, as ações internas e conectores que são executados durante o uso do ISE um plano de preços diferente do plano de preços baseado em consumo. Para obter mais informações, consulte [Preços de Aplicativos Lógicos](../logic-apps/logic-apps-pricing.md).

* Uma [Rede virtual do Azure](../virtual-network/virtual-networks-overview.md). Se ainda não tiver uma rede virtual, aprenda a [criar uma Rede virtual do Azure](../virtual-network/quick-create-portal.md). 

  * Sua rede virtual deve ter quatro *vazio* sub-redes para a implantação e criação de recursos em seu ISE. Você pode criar essas sub-redes com antecedência, ou você pode esperar até que você crie seu ISE onde você pode criar sub-redes ao mesmo tempo. Saiba mais sobre [requisitos de sub-rede](#create-subnet). 
  
    > [!NOTE]
    > Se você usar [ExpressRoute](../expressroute/expressroute-introduction.md), que fornece uma conexão privada para serviços de nuvem da Microsoft, você deve [criar uma tabela de rotas](../virtual-network/manage-route-table.md) que tem a seguinte rota e vincular essa tabela com cada sub-rede usada pelo seu ISE:
    > 
    > **Nome da**: <*nome da rota*><br>
    > **Prefixo de endereço**: 0.0.0.0/0<br>
    > **Próximo salto**: Internet

  * Certifique-se de que sua rede virtual [disponibiliza essas portas](#ports) para que seu ISE funciona corretamente e permanece acessível.

* Se você quiser usar servidores DNS personalizados para sua rede virtual do Azure [configurar esses servidores seguindo estas etapas](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) antes de implantar o ISE em sua rede virtual. Caso contrário, todas as vezes que você alterar o servidor DNS, será necessário também reiniciar o ISE, que é um recurso disponível na visualização pública do ISE.

* Conhecimento básico sobre [como criar aplicativos lógicos](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="ports"></a>

## <a name="check-network-ports"></a>Verifique as portas de rede

Quando você usa um ambiente de serviço de integração (ISE) com uma rede virtual, um problema de configuração comum é ter uma ou mais portas bloqueadas. Os conectores que você pode usar para criar conexões entre seu ISE e o sistema de destino também podem ter seus próprios requisitos de porta. Por exemplo, ao comunicar-se com um sistema FTP usando o conector FTP, certifique-se de que a porta usada nesse sistema FTP, como a porta 21 para enviar comandos, está disponível.

Para controlar o tráfego entre sub-redes da rede virtual em que você implantar o ISE, você pode configurar [grupos de segurança de rede](../virtual-network/security-overview.md) pela [filtragem de tráfego de rede em sub-redes](../virtual-network/tutorial-filter-network-traffic.md). No entanto, o ISE deve ter portas específicas aberto em uma rede virtual que usa grupos de segurança de rede. Dessa forma, seu ISE permanece acessível e possa funcionar corretamente para que você não perca o acesso ao seu ISE. Caso contrário, se quaisquer portas necessárias não estiverem disponíveis, o ISE deixará de funcionar.

Essas tabelas descrevem as portas na rede virtual que o ISE usa e onde essas portas são usadas. O [marcas de serviço do Gerenciador de recursos](../virtual-network/security-overview.md#service-tags) representa um grupo de prefixos de endereço IP que ajudam a minimizar a complexidade ao criar regras de segurança.

> [!IMPORTANT]
> Para a comunicação interna dentro de suas sub-redes, a ISE requer a abertura de todas as portas dentro dessas sub-redes.

| Finalidade | Direction | Portas | Marca de serviço de origem | Marca de serviço de destino | Observações |
|---------|-----------|-------|--------------------|-------------------------|-------|
| Comunicação a partir do Aplicativos Lógicos do Azure | Saída | 80 e 443 | VirtualNetwork | Internet | A porta depende do serviço externo com o qual o serviço de aplicativos lógicos se comunica |
| Azure Active Directory | Saída | 80 e 443 | VirtualNetwork | AzureActiveDirectory | |
| Dependência de Armazenamento do Azure | Saída | 80 e 443 | VirtualNetwork | Armazenamento | |
| Comunicação intersubnet | Entrada e Saída | 80 e 443 | VirtualNetwork | VirtualNetwork | Para a comunicação entre as sub-redes |
| Comunicação para Aplicativos Lógicos do Azure | Entrada | 443 | Internet  | VirtualNetwork | O endereço IP do computador ou serviço que chama qualquer gatilho de solicitação ou o webhook que existe em seu aplicativo lógico. Fechar ou bloquear essa porta impede chamadas HTTP para aplicativos lógicos com gatilhos de solicitação.  |
| Histórico de execução do aplicativo lógico | Entrada | 443 | Internet  | VirtualNetwork | O endereço IP para o computador do qual você pode exibir o aplicativo lógico do histórico de execução. Embora o fechamento ou bloquear essa porta não impede que você exibir o histórico de execução, não é possível exibir as entradas e saídas para cada etapa em que o histórico de execução. |
| Gerenciamento de Conexão | Saída | 443 | VirtualNetwork  | Internet | |
| Publicar métricas e logs de diagnóstico | Saída | 443 | VirtualNetwork  | AzureMonitor | |
| Comunicação do Gerenciador de tráfego do Azure | Entrada | 443 | AzureTrafficManager | VirtualNetwork | |
| Designer de Aplicativos Lógicos - propriedades dinâmicas | Entrada | 454 | Internet  | VirtualNetwork | As solicitações vêm de aplicativos lógicos [acessar o ponto de extremidade de entrada a endereços IP nessa região](../logic-apps/logic-apps-limits-and-config.md#inbound). |
| Dependência de Gerenciamento de Serviço de Aplicativo | Entrada | 454 e 455 | AppServiceManagement | VirtualNetwork | |
| Implantação do conector | Entrada | 454 & 3443 | Internet  | VirtualNetwork | Necessário para implantar e atualizar os conectores. Fechar ou bloquear essa porta faz com que as implantações de ISE falhe e impede que o conector atualizações ou correções. |
| Dependência SQL do Azure | Saída | 1433 | VirtualNetwork | SQL |
| Azure Resource Health | Saída | 1886 | VirtualNetwork | Internet | Para publicar o status de integridade no Resource Health |
| Gerenciamento de API - ponto de extremidade de gerenciamento | Entrada | 3443 | APIManagement  | VirtualNetwork | |
| Dependência do Log para agente de monitoramento e política do Hub de Eventos | Saída | 5672 | VirtualNetwork  | EventHub | |
| Acessar Instâncias do Cache do Azure para Redis entre Instâncias de Função | Entrada <br>Saída | 6379-6383 | VirtualNetwork  | VirtualNetwork | Além disso, para o ISE trabalhar com o Cache do Azure para Redis, você deve abri-los [portas de entrada e saídas descritas no Cache do Azure para perguntas frequentes sobre Redis](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements). |
| Azure Load Balancer | Entrada | * | AzureLoadBalancer | VirtualNetwork |  |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>Criar seu ISE

Para criar seu ISE (ambiente de serviço de integração), siga estas etapas:

1. No [portal do Azure](https://portal.azure.com), no menu principal do Azure, selecione **Criar um recurso**.
Na caixa de pesquisa, digite “ambiente de serviço de integração” como filtro.

   ![Criar novo recurso](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. No painel de criação do ambiente do serviço de integração, escolha **criar**.

   ![Escolha “Criar”](./media/connect-virtual-network-vnet-isolated-environment/create-integration-service-environment.png)

1. Forneça esses detalhes para seu ambiente e, em seguida, escolha **Revisar + criar**, por exemplo:

   ![Fornecer detalhes do ambiente](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | Propriedade | Necessário | Value | DESCRIÇÃO |
   |----------|----------|-------|-------------|
   | **Assinatura** | Sim | <*Azure-subscription-name*> | A assinatura do Azure a ser usada para o ambiente |
   | **Grupo de recursos** | Sim | <*Azure-resource-group-name*> | O grupo de recursos do Azure no qual deseja criar seu ambiente |
   | **Nome do Ambiente de Serviço de Integração** | Sim | <*environment-name*> | O nome que o ambiente terá |
   | **Localidade** | Sim | <*Azure-datacenter-region*> | A região do datacenter do Azure na qual o ambiente será implantado |
   | **Capacidade adicional** | Sim | 0 a 10 | O número de unidades de processamento adicional a ser usado para esse recurso ISE. Para adicionar capacidade após a criação, consulte [capacidade de adicionar ISE](#add-capacity). |
   | **Rede virtual** | Sim | <*Azure-virtual-network-name*> | A rede virtual do Azure na qual você deseja injetar seu ambiente para que os aplicativos lógicos no ambiente possam acessar sua rede virtual. Se você não tiver uma rede [criar uma rede virtual do Azure primeiro](../virtual-network/quick-create-portal.md). <p>**Importante**: Você pode executar essa injeção *apenas* quando cria seu ISE. |
   | **Sub-redes** | Sim | <*subnet-resource-list*> | Um ISE requer quatro sub-redes *vazias* para criar recursos em seu ambiente. Para criar cada sub-rede, [siga as etapas nesta tabela](#create-subnet).  |
   |||||

   <a name="create-subnet"></a>

   **Criar sub-rede**

   Para criar recursos em seu ambiente, o ISE precisa quatro *vazio* sub-redes que não são delegadas a qualquer serviço. 
   Você *não é possível* alterar esses endereços de sub-rede após criar seu ambiente. Cada sub-rede deve atender a estes critérios:

   * Tem um nome que começa com um caractere alfabético ou sublinhado e não tem estes caracteres: `<`, `>`, `%`, `&`, `\\`, `?`, `/`

   * Usa o [formato de Classless inter-Domain Routing (CIDR)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) e um espaço de endereço de classe B.

   * Usa pelo menos um `/27` o endereço de espaço porque cada sub-rede deve ter 32 endereços como o *mínimo*. Por exemplo:

     * `10.0.0.0/27` tem 32 endereços porque 2<sup>(32-27)</sup> é 2<sup>5</sup> ou 32.

     * `10.0.0.0/24` tem 256 endereços porque 2<sup>(32-24)</sup> é 2<sup>8</sup> ou 256.

     * `10.0.0.0/28` tem somente 16 endereços e é muito pequeno porque 2<sup>(32-28)</sup> é 2<sup>4</sup> ou 16.

     Para saber mais sobre como calcular os endereços, consulte [blocos CIDR IPv4](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks).

   * Se você usar [ExpressRoute](../expressroute/expressroute-introduction.md), lembre-se [criar uma tabela de rotas](../virtual-network/manage-route-table.md) que tem a seguinte rota e vincular essa tabela com cada sub-rede usada pelo seu ISE:

     **Nome da**: <*nome da rota*><br>
     **Prefixo de endereço**: 0.0.0.0/0<br>
     **Próximo salto**: Internet

   1. Na lista **Sub-redes**, escolha **Gerenciar configuração de sub-rede**.

      ![Gerenciar configuração de sub-rede](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet.png)

   1. No painel **Sub-redes**, escolha **Sub-rede**.

      ![Adicionar sub-rede](./media/connect-virtual-network-vnet-isolated-environment/add-subnet.png)

   1. No painel **Adicionar sub-rede**, forneça estas informações.

      * **Nome**: o nome da sub-rede
      * **Intervalo de endereços (bloco CIDR)** : o intervalo da sub-rede em sua rede virtual e no formato CIDR

      ![Adicionar detalhes da sub-rede](./media/connect-virtual-network-vnet-isolated-environment/subnet-details.png)

   1. Quando terminar, escolha **OK**.

   1. Repita essas etapas para mais três sub-redes.

      > [!NOTE]
      > Se tentar criar sub-redes não são válidas, o portal do Azure mostra uma mensagem, mas não bloqueia seu progresso.

1. Depois que o Azure validar com êxito suas informações do ISE, escolha **Criar**; por exemplo:

   ![Após a validação bem-sucedida, escolha "Criar"](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   O Azure começa a implantar seu ambiente, mas esse processo pode levar *até duas horas* para ser concluído. 
   Para verificar o status de implantação, na barra de ferramentas do Azure, escolha o ícone de notificações, o que abre o painel de notificações.

   ![Verifique o status da implantação](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   Se a implantação for concluída com êxito, o Azure mostrará esta notificação:

   ![Implantação bem-sucedida](./media/connect-virtual-network-vnet-isolated-environment/deployment-success.png)

   Caso contrário, siga as instruções do portal do Azure para implantação de solução de problemas.

   > [!NOTE]
   > Se a falha de implantação ou você exclua seu ISE, Azure pode levar até uma hora antes de liberar suas sub-redes. Esse atraso significa significa que você terá que esperar antes de reutilizar essas sub-redes no ISE do outro. 
   >
   > Se você excluir sua rede virtual, Azure geralmente leva até duas horas antes de liberar o backup de suas sub-redes, mas essa operação pode levar mais tempo. 
   > Ao excluir redes virtuais, certifique-se de que não há recursos ainda estão conectados. Ver [excluir rede virtual](../virtual-network/manage-virtual-network.md#delete-a-virtual-network).

1. Para exibir seu ambiente, escolha **Ir para o recurso** se o Azure não for automaticamente para seu ambiente após a conclusão da implantação.  

Para obter mais informações sobre como criar sub-redes, consulte [adicionar uma sub-rede de rede virtual](../virtual-network/virtual-network-manage-subnet.md).

<a name="create-logic-apps-environment"></a>

## <a name="create-logic-app---ise"></a>Criar aplicativo lógico – ISE

Para criar aplicativos lógicos que são executados no ambiente do serviço de integração (ISE) [criar aplicativos lógicos como de costume](../logic-apps/quickstart-create-first-logic-app-workflow.md) , exceto quando você define o **local** propriedade, selecione seu ISE do  **Ambientes do serviço de integração** seção, por exemplo:

  ![Selecionar o ambiente de serviço de integração](./media/connect-virtual-network-vnet-isolated-environment/create-logic-app-with-integration-service-environment.png)

As diferenças em como gatilhos e ações de trabalho e como eles estão rotulado como quando você usa um ISE em comparação com o serviço global de aplicativos lógicos, consulte [isolados global na visão geral do ISE](connect-virtual-network-vnet-isolated-environment-overview.md#difference).

<a name="create-integration-account-environment"></a>

## <a name="create-integration-account---ise"></a>Criar conta de integração – ISE

Se você quiser usar uma conta de integração com aplicativos lógicos em um ambiente de serviço de integração (ISE), essa conta de integração deve usar o *mesmo ambiente* como os aplicativos lógicos. Os aplicativos lógicos em um ISE podem fazer referência somente às contas de integração no mesmo ISE.

Para criar uma conta de integração que usa um ISE [criar sua conta de integração como de costume](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) , exceto quando você define o **local** propriedade, selecione seu ISE do **integração ambientes de serviço** seção, por exemplo:

![Selecionar o ambiente de serviço de integração](./media/connect-virtual-network-vnet-isolated-environment/create-integration-account-with-integration-service-environment.png)

<a name="add-capacity"></a>

## <a name="add-ise-capacity"></a>Adicionar capacidade ISE

A unidade base do ISE corrigiu capacidade, portanto, se você precisar de mais taxa de transferência, você pode adicionar mais unidades de escala. Você pode o dimensionamento automático com base em métricas de desempenho ou em um número de unidades de processamento adicional. Se você escolher o dimensionamento automático com base nas métricas, você pode escolher entre vários critérios e especifique as condições de limite para atender a esses critérios.

1. No portal do Azure, localize seu ISE.

1. Para revisar as métricas de uso e desempenho para o ISE, no menu principal do seu do ISE, selecione **visão geral**.

   ![Uso do modo de exibição para o ISE](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-usage.png)

1. Para configurar o dimensionamento automático, sob **as configurações**, selecione **expandir**. Sobre o **configurar** guia, escolha **Habilitar dimensionamento automático**.

   ![Ativar o dimensionamento automático](./media/connect-virtual-network-vnet-isolated-environment/scale-out.png)

1. Para **nome da configuração de dimensionamento automático**, forneça um nome para a sua configuração.

1. No **padrão** seção, escolha **dimensionar com base em uma métrica** ou **dimensionar para uma contagem de instância específica**.

   * Se você escolher com base em instância, insira o número de unidades de processamento entre 0 e 10, inclusive.

   * Se você escolher com base em métrica, siga estas etapas:

     1. No **regras** , escolha **adicionar uma regra**.

     1. Sobre o **regra de dimensionamento** painel, configurar seus critérios e a ação a ser tomada quando a regra dispara.

     1. Quando terminar, escolha **adicionar**.

1. Quando tiver terminado com as configurações de AutoEscala, salve suas alterações.

## <a name="next-steps"></a>Próximas etapas

* Saiba mais sobre a [Rede Virtual do Azure](../virtual-network/virtual-networks-overview.md)
* Saiba mais sobre a [integração de rede virtual para os serviços do Azure](../virtual-network/virtual-network-for-azure-services.md)
