---
title: Introdução aos ambientes de equipe com base em Máquina Virtual de Ciência de Dados – Azure | Microsoft Docs
description: Padrões para implantar a VM de Ciência de Dados em um ambiente de equipe corporativo.
keywords: aprendizado profundo, IA, ferramentas de ciência de dados, máquina virtual de ciência de dados, análise geoespacial, processo de ciência de dados da equipe
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: vijetaj
ms.openlocfilehash: 5f34498fbdacf7fc6e62788913c795ab70ceef23
ms.sourcegitcommit: 007ee4ac1c64810632754d9db2277663a138f9c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69991623"
---
# <a name="data-science-virtual-machine-based-team-analytics-and-ai-environment"></a>Ambiente de IA e análise de equipe com base em Máquina Virtual de Ciência de Dados 
O [máquina virtual de ciência de dados](overview.md) (DSVM) fornece um ambiente avançado na plataforma do Azure, com software predefinido para ia (inteligência artificial) e análise de dados.

Tradicionalmente, a DSVM era usada como uma área de trabalho de análise individual. Cientistas de dados individuais têm produtividade com esse ambiente de análise compartilhado e predefinido. Como equipes de análise de grande porte planejam ambientes para seus cientistas de dados e desenvolvedores de ia, um dos temas recorrentes é uma infraestrutura de análise compartilhada para desenvolvimento e experimentação. Essa infraestrutura é gerenciada em linha com políticas de ti corporativas que também facilitam a colaboração e a consistência entre as equipes de ciência de dados e de análise.

Uma infraestrutura compartilhada permite uma melhor utilização de ti do ambiente de análise. Algumas organizações chamam a infraestrutura de ciência/análise de dados baseada em equipe em uma *área restrita de análise*. Ele permite que os cientistas de dados acessem vários ativos de dados para entender rapidamente os dados. Esse ambiente de área restrita também ajuda os cientistas de dados a executar experimentos, validar as mesmas e criar modelos de previsão sem afetar o ambiente de produção.

Como a DSVM opera no nível de infraestrutura do Azure, os administradores de TI podem configurar prontamente a DSVM para operar em conformidade com as políticas de TI da empresa. O DSVM oferece flexibilidade total na implementação de várias arquiteturas de compartilhamento, além de oferecer acesso a ativos de dados corporativos de forma controlada.

Esta seção discute alguns padrões e diretrizes que você pode usar para implantar a DSVM como uma infraestrutura de ciência de dados baseada em equipe. Como os blocos de construção desses padrões são provenientes da IaaS (infraestrutura como serviço) do Azure, eles se aplicam a qualquer VM do Azure. Esta série de artigos se concentra na aplicação desses recursos de infraestrutura padrão do Azure ao DSVM.

Os principais blocos de construção de um ambiente do Enterprise Team Analytics incluem:

* [Um pool de DSVMs em escala](dsvm-pools.md)
* [Identidade e acesso comum ao workspace de qualquer uma das DSVMs no pool](dsvm-common-identity.md)
* [Acesso seguro a fontes de dados](dsvm-secure-access-keys.md)


Esta série fornece orientações e ponteiros para cada um dos tópicos anteriores. Ele não cobre todas as considerações e requisitos para a implantação de DSVMs em grandes configurações corporativas. Aqui estão alguns outros recursos do Azure que você pode usar ao implementar instâncias de DSVM em sua empresa:

* [Segurança de rede](https://docs.microsoft.com/azure/security/fundamentals/network-security)
* [Monitoramento](https://docs.microsoft.com/azure/virtual-machines/windows/monitor) e [gerenciamento](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates)
* [Log e auditoria](https://docs.microsoft.com/azure/security/fundamentals/log-audit)
* [Controle de acesso baseado em função](https://docs.microsoft.com/azure/role-based-access-control/overview)
* [Configuração da política e execução](../../governance/policy/overview.md)
* [Antimalware](https://docs.microsoft.com/azure/security/fundamentals/antimalware)
* [Criptografia](https://docs.microsoft.com/azure/virtual-machines/windows/encrypt-disks)
* [Descoberta de dados e governança](https://docs.microsoft.com/azure/data-catalog/)

Por fim, a [centro de arquitetura do Azure](https://docs.microsoft.com/azure/architecture/) fornece uma arquitetura e modelos detalhados de ponta a ponta para criar e gerenciar sua infraestrutura de análise baseada em nuvem.
