---
title: Azure Blockchain Service suporte para versões do razão, aplicação de patch e atualização
description: Visão geral das versões razões com suporte no serviço de Blockchain do Azure, incluindo as políticas em relação a atualizações gerenciadas pelo usuário e de sistemas para aplicação de patches e gerenciado pelo sistema.
services: azure-blockchain
keywords: blockchain
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 53f65ec91a1e0f1e5a6322f0125bf83cd3e400b2
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399100"
---
# <a name="supported-azure-blockchain-service-ledger-versions"></a>Versões com suporte de razão do serviço do Azure Blockchain

Azure Blockchain Service usa o Ethereum baseado [Quorum](https://www.goquorum.com/developers) razão projetado para o processamento de transações privadas dentro de um grupo de participantes conhecidos, identificado como um consórcio no serviço de Blockchain do Azure.

Atualmente, o serviço de Blockchain do Azure suporta [Quorum versão 2.2.1](https://github.com/jpmorganchase/quorum/releases/tag/v2.2.1) e [Gerenciador de transações Tessera](https://github.com/jpmorganchase/tessera).

## <a name="managing-updates-and-upgrades"></a>Gerenciar atualizações e upgrades

Controle de versão do Quorum é feito por meio de um principal, secundário e versões de patch. Por exemplo, se a versão de Quorum é 2.0.1, tipo de versão seria categorizado da seguinte maneira:

|Principal | Secundária  | Patch  |
| :--- | :----- | :----- |
| 2 | 0 | 1 | 

Azure Blockchain Service atualiza automaticamente as versões de patch do Quorum para os membros existentes em execução dentro de 30 dias do que está sendo disponibilizado de Quorum.

## <a name="availability-of-new-ledger-versions"></a>Disponibilidade de novas versões do razão

Azure Blockchain Service fornece as mais recentes versões principais e secundárias do razão Quorum dentro de 60 dias de estarem disponíveis do fabricante do Quorum. Um máximo de quatro versões secundárias são fornecidos para consórcios de escolha ao provisionamento de um novo membro e consortium. Atualizando a partir de um principal ou secundária de versão não é suportada atualmente.

## <a name="next-steps"></a>Próximas etapas

[Limites de serviço de Blockchain do Azure](limits.md)
