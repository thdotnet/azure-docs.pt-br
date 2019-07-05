---
title: Acessar redes virtuais do Azure a partir dos Aplicativos Lógicos do Azure com ISEs (Ambientes de Serviço de Integração)
description: Esta visão geral descreve como ISEs (ambientes de serviço de integração) ajudam os aplicativos lógicos a acessarem as VNETs (redes virtuais) do Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 05/06/2019
ms.openlocfilehash: f981452b06ec06f2be1b8fe0319cd49a678ccfe0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67441575"
---
# <a name="access-to-azure-virtual-network-resources-from-azure-logic-apps-by-using-integration-service-environments-ises"></a>Acessar recursos de rede virtual do Azure a partir dos Aplicativos Lógicos do Azure usando ISEs (Ambientes de Serviço de Integração)

Às vezes, suas contas de integração e de aplicativos lógicos precisam de acesso a recursos protegidos, como máquinas virtuais (VMs) e outros sistemas ou serviços que estejam dentro de um [rede virtual do Azure](../virtual-network/virtual-networks-overview.md). Para configurar esse acesso, você pode [criar um *ambiente do serviço de integração* (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) onde você pode executar seus aplicativos lógicos e criar a integração de contas.

Quando você cria um ISE, o Azure implanta uma instância privada e isolada do serviço de aplicativos lógicos em sua rede virtual do Azure. Essa instância privada usa recursos dedicados, como o armazenamento, sendo executada separadamente do serviço público "global" de Aplicativos Lógicos. A separação sua instância privada isolada e a instância global pública também ajuda a reduzir o impacto de outros locatários do Azure podem ter no desempenho de seus aplicativos, que também é conhecido como o [efeito de "vizinhos ruidosos"](https://en.wikipedia.org/wiki/Cloud_computing_issues#Performance_interference_and_noisy_neighbors).

Depois de criar seu ISE, quando você vai criar sua conta de integração ou de aplicativo lógico, você pode selecionar seu ISE como local de sua lógica aplicativo ou a integração da conta:

![Selecionar o ambiente de serviço de integração](./media/connect-virtual-network-vnet-isolated-environment-overview/select-logic-app-integration-service-environment.png)

Seu aplicativo lógico agora pode acessar diretamente os sistemas que estão dentro ou conectado à sua rede virtual usando qualquer um desses itens:

* Uma **ISE**-rotulados de conector para o sistema, como o SQL Server
* Um **Core**-rotuladas internos gatilho ou ação, como o gatilho HTTP ou a ação
* Um conector personalizado

Esta visão geral descreve mais detalhes sobre como um ISE oferece a seus aplicativos lógicos e integração de contas de acesso direto à rede virtual do Azure e compara as diferenças entre um ISE e o serviço global de aplicativos lógicos.

> [!NOTE]
> Aplicativos lógicos, gatilhos internos, as ações internas e conectores que são executados durante o uso do ISE um plano de preços diferente do plano de preços baseado em consumo. Para obter mais informações, consulte [Preços de Aplicativos Lógicos](../logic-apps/logic-apps-pricing.md). O ISE também aumentou limites na duração da execução, retenção de armazenamento, taxa de transferência, solicitação HTTP e tempos limite de resposta, tamanhos de mensagens e solicitações do conector personalizado. Para obter mais informações, consulte [limites e configuração para aplicativos lógicos do Azure](logic-apps-limits-and-config.md).

<a name="difference"></a>

## <a name="isolated-versus-global"></a>Isolado versus global

Quando você cria um ambiente de serviço integrado (ISE) no Azure, você pode selecionar a rede virtual do Azure onde você deseja *injetar* seu ISE. Azure injeta ou implanta uma instância privada do serviço aplicativos lógicos em sua rede virtual. Essa ação cria um ambiente isolado em que é possível criar e executar os aplicativos lógicos em recursos dedicados. Quando você cria seu aplicativo lógico, selecione seu ISE como o local do seu aplicativo, que oferece seu aplicativo lógico acesso direto à sua rede virtual e os recursos da rede.

Os aplicativos lógicos em um ISE fornecem as mesmas experiências de usuário e funcionalidades semelhantes às do serviço global dos Aplicativos Lógicos. Não só você pode usar o mesmos gatilhos internos, as ações internas e conectores do serviço de aplicativos lógicos global, mas você também pode usar conectores específicos do ISE. Por exemplo, eis alguns conectores padrão que oferecem versões que são executados em um ISE:

* Armazenamento de Blobs, Armazenamento de Arquivos e Armazenamento de Tabelas do Azure
* Filas do Azure, Barramento de Serviço do Azure, Hubs de Eventos do Azure e IBM MQ
* FTP e SFTP-SSH
* SQL Server, SQL Data Warehouse, Azure Cosmos DB
* AS2, X12 e EDIFACT

A diferença entre os conectores ISE e não ISE está nos locais em que os gatilhos e as ações são executados:

* Em seu ISE, interna de gatilhos e ações, como HTTP, sempre executam no ISE do mesmo que o seu aplicativo lógico e exibir o **Core** rótulo.

  ![Selecione as ações e gatilhos internos de "Core"](./media/connect-virtual-network-vnet-isolated-environment-overview/select-core-built-in-actions-triggers.png)

* Conectores que são executados em um ISE publicamente tenham hospedado versões disponíveis no serviço global de aplicativos lógicos. Para os conectores que oferecem duas versões, conectores com o **ISE** sempre rotular execução no ISE do mesmo que seu aplicativo lógico. Os conectores sem o rótulo do **ISE** são executados no serviço global dos Aplicativos Lógicos.

  ![Selecionar conectores ISE](./media/connect-virtual-network-vnet-isolated-environment-overview/select-ise-connectors.png)

Um ISE também fornece o aumento dos limites para a duração da execução, retenção de armazenamento, taxa de transferência, solicitação HTTP e tempos limite de resposta, tamanhos de mensagens e solicitações do conector personalizado. Para obter mais informações, consulte [limites e configuração para aplicativos lógicos do Azure](logic-apps-limits-and-config.md).

### <a name="access-to-on-premises-data-sources"></a>Acesso a fontes de dados local

Para sistemas locais que estão conectados a uma rede virtual do Azure, injete um ISE em rede para que seus aplicativos lógicos possam acessar diretamente esses sistemas usando qualquer um desses itens:

* Conector de versão do ISE para o sistema, por exemplo, o SQL Server
* Ação HTTP
* Conector personalizado

  * Se você tiver conectores personalizados que exigem o gateway de dados local, e você criou esses conectores fora um ISE, aplicativos lógicos em um ISE também podem usar esses conectores.
  
  * Conectores personalizados criados em um ISE não funcionam com o gateway de dados local. No entanto, esses conectores podem acessar diretamente fontes de dados locais que estão conectadas à rede virtual que hospeda o ISE. Portanto, aplicativos lógicos em um ISE provavelmente não é necessário o gateway de dados ao se comunicar com esses recursos.

Para sistemas locais que não estão conectados a uma rede virtual ou não tem conectores de versão do ISE, você deve primeiro [configurar o gateway de dados local](../logic-apps/logic-apps-gateway-install.md) antes de sua lógica de aplicativos podem se conectar a esses sistemas.

<a name="create-integration-account-environment"></a>

## <a name="integration-accounts-with-ise"></a>Contas de integração com ISE

É possível usar contas de integração com aplicativos lógicos dentro de um ISE (ambiente de serviço de integração). No entanto, essas contas de integração devem usar o *mesmo ISE* que os aplicativos lógicos vinculados. Os aplicativos lógicos em um ISE podem fazer referência somente às contas de integração que estão no mesmo ISE. Ao criar uma conta de integração, é possível selecionar o ISE como local para a conta de integração.

## <a name="next-steps"></a>Próximas etapas

* Saiba como [conectar-se a redes virtuais do Azure de aplicativos lógicos isolados](../logic-apps/connect-virtual-network-vnet-isolated-environment.md)
* Saiba mais sobre a [Rede Virtual do Azure](../virtual-network/virtual-networks-overview.md)
* Saiba mais sobre a [integração de rede virtual para os serviços do Azure](../virtual-network/virtual-network-for-azure-services.md)
