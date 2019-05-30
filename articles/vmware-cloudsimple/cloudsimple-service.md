---
title: Soluções do VMware por CloudSimple - serviço
description: Descreve a visão geral do serviço CloudSimple e conceitos.
author: sharaths-cs
ms.author: b-shsury
ms.date: 4/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f7b4be0ff3997e27dd5b5321dd44b5006ae52102
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358102"
---
# <a name="cloudsimple-service-overview"></a>Visão geral do CloudSimple Service

O serviço CloudSimple permite consumir solução CloudSimple do VMware do Azure.  Criando o serviço permite a compra de nós, reserva de nós e criar nuvens privadas.  Você pode criar o serviço CloudSimple em cada região do Azure onde o serviço CloudSimple está disponível.  O serviço define a rede de borda da solução CloudSimple do VMware do Azure.  A rede de borda dá suporte a serviços que incluem o VPN, ExpressRoute e conectividade à internet em suas nuvens privadas.

## <a name="gateway-subnet"></a>Gateway de sub-rede

Uma sub-rede de gateway é necessária por serviço CloudSimple e é exclusiva para a região na qual ele é criado. A sub-rede de gateway é usada ao criar a rede de borda e exige uma de/28 bloco CIDR.  O espaço de endereço de sub-rede de gateway deve ser exclusivo. Ele não deve sobrepor qualquer rede que se comunica com o ambiente CloudSimple. As redes que se comunicam com CloudSimple incluem redes locais e rede virtual do Azure.  Uma sub-rede de gateway não pode ser excluída depois que ele é criado.  A sub-rede de gateway é removida quando o serviço é excluído.

## <a name="next-steps"></a>Próximas etapas

* Saiba como [criar um serviço CloudSimple no Azure](quickstart-create-cloudsimple-service.md)