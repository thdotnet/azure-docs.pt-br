---
title: "Usando o DNS do Azure com outros serviços do Azure | Microsoft Docs"
description: "Noções básicas sobre como usar o DNS do Azure a fim de resolver o nome para outros serviços do Azure"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure dns
ms.assetid: e9b5eb94-7984-4640-9930-564bb9e82b78
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 09/21/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 119275f335344858cd20b6a17ef87e3ef32b6e12
ms.openlocfilehash: 3a917b34c925965aacd161e8a01284d9a01bebae
ms.lasthandoff: 03/01/2017

---
# <a name="how-azure-dns-works-with-other-azure-services"></a>Como o Azure DNS funciona com outros serviços do Azure

O DNS do Azure é um serviço hospedado de gerenciamento de DNS e resolução de nomes. Isso permite que você crie nomes DNS públicos para outros aplicativos e serviços implantados no Azure. A criação de um nome para um serviço do Azure no seu domínio personalizado é tão simples quanto adicionar um registro do tipo correto ao seu serviço.

* Para endereços IP alocados dinamicamente, você deve criar um registro DNS CNAME que mapeia para o nome DNS que o Azure criou para seu serviço. Padrões DNS o impedem de usar um registro CNAME para o apex da zona.
* Para endereços IP alocados estaticamente, você pode criar um registro DNS A usando qualquer nome, incluindo um nome de *domínio raiz* no apex da zona.

A tabela a seguir descreve os tipos de registro com suporte que podem ser usados para vários serviços do Azure. Como você pode ver nessa tabela, o DNS do Azure dá suporte apenas a registros DNS para recursos de rede voltados para a Internet. O DNS do Azure não pode ser usado para a resolução de nome de endereços internos, privadas.

| Serviço do Azure | Interface de rede | Descrição |
| --- | --- | --- |
| Application Gateway |IP público de front-end |Você pode criar um registro CNAME ou DNS A. |
| Balanceador de carga |IP público de front-end |Você pode criar um registro CNAME ou DNS A. O Balanceador de Carga pode ter um endereço IP público do IPv6 que é atribuído dinamicamente. Portanto, você deve criar um registro CNAME para um endereço IPv6. |
| Gerenciador de Tráfego |Nome público |Você só pode criar um CNAME que mapeia para o nome trafficmanager.net atribuído ao seu perfil do Gerenciador de Tráfego. Para saber mais, confira [Como o Gerenciador de Tráfego funciona](../traffic-manager/traffic-manager-how-traffic-manager-works.md#traffic-manager-example). |
| Serviço de Nuvem |IP público |Para endereços IP alocados estaticamente, você pode criar um registro DNS A. Para endereços IP alocados dinamicamente, você deve criar um registro CNAME que mapeia para o nome *cloudapp.net* . Essa regra se aplica a máquinas virtuais criadas no portal clássico, já que elas são implantadas como um serviço de nuvem. Para saber mais, confira [Configurar um nome de domínio personalizado nos Serviços de Nuvem](../cloud-services/cloud-services-custom-domain-name-portal.md). |
| Serviço de Aplicativo |IP Externa |Para endereços IP externos, você pode criar um registro DNS A. Caso contrário, você deve criar um registro CNAME que mapeia para o nome azurewebsites.net. Para saber mais, confira [Mapear um nome de domínio personalizado para um aplicativo do Azure](../app-service-web/web-sites-custom-domain-name.md) |
| VMs do Resource Manager |IP público |As VMs do Gerenciador de Recursos pode ter endereços IP públicos. Uma VM com um endereço IP público também pode estar por trás de um balanceador de carga. Você pode criar um registro DNS A ou CNAME para o endereço público. Esse nome personalizado pode ser usado para ignorar o VIP no balanceador de carga. |
| VMs clássicas |IP público |VMs clássicas criadas usando o PowerShell ou a CLI podem ser configuradas com um endereço virtual (reservado) dinâmico ou estático. Você pode criar um registro DNS CNAME ou A, respectivamente. |


