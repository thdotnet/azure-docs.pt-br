---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 08/02/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8b585a47d3950d232eb3e8047c12ee8949030c95
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780185"
---
Para criar uma Rede Virtual no modelo de implantação do Gerenciador de Recursos usando o portal do Azure, siga as etapas abaixo. Use os **valores de exemplo** se você estiver usando estas etapas como um tutorial. Se você não estiver executando estas etapas como um tutorial, substitua os valores pelos seus próprios. Para saber mais sobre como trabalhar com as redes virtuais, consulte a [Visão Geral da Rede Virtual](../articles/virtual-network/virtual-networks-overview.md)

>[!NOTE]
>Para que essa rede virtual se conecte a um local, é necessário coordenar com o administrador de rede local para que ele consiga um intervalo de endereços IP que possa ser usado especificamente para essa rede virtual. Se um intervalo de endereços duplicado existir em ambos os lados da conexão de VPN, o tráfego não será roteado da maneira esperada. Além disso, se você deseja se conectar esta rede virtual a outra rede virtual, o espaço de endereços não pode sobrepor outra rede virtual. Tome cuidado e planeje sua configuração de rede de forma adequada.
>

1. Em um navegador, acesse o [Portal do Azure](https://portal.azure.com) e entre com sua conta do Azure.
2. Clique em **Criar um recurso**. No campo **Pesquisar no marketplace**, digite ‘rede virtual’. Localize a **Rede virtual** na lista retornada e clique para abrir a página **Rede Virtual**.
3. Clique em **Criar**. Isso abre a página **Criar rede virtual**.
4. Na página **Criar rede virtual**, defina as configurações da VNet. Durante o preenchimento dos campos, o ponto de exclamação vermelho se tornará um ponto de seleção verde quando os caracteres digitados no campo forem válidos. Use os seguintes valores:

   - **Nome**: VNet1
   - **Espaço de endereço**: 10.1.0.0/16
   - **Assinatura**: Verifique se a assinatura listada é aquela que você deseja usar. Você pode alterar as assinaturas usando o menu suspenso.
   - **Grupo de recursos**: TestRG1 (clique em **criar novo** para criar um novo grupo)
   - **Localização**: East US
   - **Sub-rede**: Front-end
   - **Intervalo de endereços**: 10.1.0.0/24

   ![Criar página da rede virtual](./media/vpn-gateway-create-virtual-network-portal-include/create-virtual-network1.png)
5. Deixe DDoS como básico, pontos de extremidade de serviço como desabilitado e firewall como desabilitado.
6. Clique em **Criar** para criar a VNet.