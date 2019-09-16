---
title: Usar o gateway de dados local para fontes de dados da Rede Virtual do Azure | Microsoft Docs
description: Saiba como configurar um servidor para usar um gateway para fontes de dados no VNet.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 675d8ecd3d6a3310a9b102df37df18bed02df3de
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70958815"
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>Usar gateway para fontes de dados em uma VNet (Rede Virtual) do Azure

Este artigo descreve a propriedade do servidor **AlwaysUseGateway** para uso quando as fontes de dados estiverem em uma [VNet (Rede Virtual) do Azure](../virtual-network/virtual-networks-overview.md).

## <a name="server-access-to-vnet-data-sources"></a>Acesso ao servidor para fontes de dados VNet

Se as fontes de dados forem acessadas por meio de uma VNet, o servidor do Azure Analysis Services deverá conectar essas fontes de dados como se estivessem no local, no próprio ambiente. Você pode configurar a propriedade de servidor **AlwaysUseGateway** para especificar o servidor para acessar todas as fontes de dados por meio de um [Gateway local](analysis-services-gateway.md). 

Instância Gerenciada do Banco de Dados SQL do Azure fontes de dados são executadas na VNet do Azure com um endereço IP privado. Se o ponto de extremidade público estiver habilitado na instância, um gateway não será necessário. Se o ponto de extremidade público não estiver habilitado, um gateway de dados local será necessário e a propriedade AlwaysUseGateway deverá ser definida como true.

> [!NOTE]
> Essa propriedade só é eficaz quando um [Gateway de dados local](analysis-services-gateway.md) é instalado e configurado. O gateway pode estar na VNet.

## <a name="configure-alwaysusegateway-property"></a>Configurar a propriedade AlwaysUseGateway

1. Em SSMS > servidor > **Propriedades** > **Geral**, selecione **Mostrar Propriedades Avançadas (Todas)** .
2. Em **ASPaaS\AlwaysUseGateway**, selecione **true**.

    ![Sempre use propriedade de gateway](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)


## <a name="see-also"></a>Consulte também
[Conectando fontes de dados locais](analysis-services-gateway.md)   
[Instalar e configurar um gateway de dados local](analysis-services-gateway-install.md)   
[VNET (Rede Virtual) do Azure](../virtual-network/virtual-networks-overview.md)   

