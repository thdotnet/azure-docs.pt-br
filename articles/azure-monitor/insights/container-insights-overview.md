---
title: Visão geral do Azure Monitor para contêineres | Microsoft Docs
description: Este artigo descreve o Azure Monitor para contêineres que monitora as soluções de Insights do Contêiner AKS e o valor que ele oferece monitorando a integridade dos clusters AKS e de Instâncias de Contêiner no Azure.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/25/2019
ms.author: magoedte
ms.openlocfilehash: d6321564672097fbf901d1d33afac9f606fcb63a
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65521829"
---
# <a name="azure-monitor-for-containers-overview"></a>Visão geral do Azure Monitor para contêineres

O Azure Monitor para contêineres é um recurso projetado para monitorar o desempenho de cargas de trabalho de contêiner implantadas em Instâncias de Contêiner do Azure ou em clusters do Kubernetes gerenciado hospedados no AKS (Serviço de Kubernetes do Azure). Monitorar os contêineres é fundamental, principalmente ao executar um cluster de produção em grande escala e com vários aplicativos.

O Azure Monitor para contêineres oferece visibilidade de desempenho coletando métricas de processador e memória de controladores, nós e contêineres disponíveis no Kubernetes por meio da API de Métricas. Os logs do contêiner também são coletados.  Depois de habilitar o monitoramento de clusters de Kubernetes, métricas e logs são coletados automaticamente para você por meio de uma versão em contêineres do agente do Log Analytics para Linux. As métricas são gravadas no armazenamento de métricas e dados de log são gravados para o armazenamento de logs associado com sua [do Log Analytics](../log-query/log-query-overview.md) espaço de trabalho. 

![O Azure Monitor para a arquitetura de contêineres](./media/container-insights-overview/azmon-containers-architecture-01.png)
 
## <a name="what-does-azure-monitor-for-containers-provide"></a>O que o Azure Monitor para contêineres fornece?

O Azure Monitor para contêineres oferece uma experiência de monitoramento abrangente usando diferentes recursos do Azure Monitor permitindo que você entender o desempenho e a integridade do seu cluster Kubernetes e as cargas de trabalho do contêiner. Com o Azure Monitor para contêineres, você pode:

* Identificar contêineres AKS que estão em execução no nó e sua utilização média de processador e memória. Esse conhecimento pode ajudá-lo a identificar gargalos de recursos.
* Identifique a utilização de processador e de memória de grupos de contêineres e seus contêineres hospedados em Instâncias de Contêiner do Azure.  
* Identificar em que local o contêiner reside em um controlador ou em um pod. Esse conhecimento pode ajudá-lo a exibir o desempenho geral do controlador ou do pod.
* Examine a utilização de recursos de cargas de trabalho em execução no host não relacionadas aos processos padrão que dão suporte ao pod.
* Compreender o comportamento do cluster sob cargas mais pesadas e médias. Esse conhecimento pode ajudá-lo a identificar as necessidades de capacidade e determinar a carga máxima que o cluster pode sustentar. 
* Configure alertas para notificar você proativamente ou registre-o quando a utilização de CPU e memória em nós ou contêineres exceder seus limites.  

## <a name="how-do-i-access-this-feature"></a>Como acessar esse recurso?
Acesse o Azure Monitor para contêineres de duas maneiras, pelo Azure Monitor ou diretamente no cluster AKS selecionado. A partir do Azure Monitor, você tem uma perspectiva global de todos os contêineres implantados, que são monitorados e que não são, permitindo que você pesquisar e filtrar entre suas assinaturas e grupos de recursos e depois faça drill para o Azure Monitor para contêineres das contêiner selecionado.  Caso contrário, você pode acessar o recurso diretamente de um contêiner AKS selecionado da página do AKS.  

![Visão geral dos métodos para acessar o Azure Monitor para contêineres](./media/container-insights-overview/azmon-containers-experience.png)

Se você estiver interessado em monitorar e gerenciar hosts de contêiner Docker e Windows para exibir configuração, auditoria e utilização de recursos, consulte a [Solução de Monitoramento de Contêiner](../../azure-monitor/insights/containers.md).

## <a name="next-steps"></a>Próximas etapas
Para começar a monitorar seu cluster do AKS, examine [como habilitar o Azure Monitor para contêineres](container-insights-onboard.md) para entender os requisitos e os métodos disponíveis para habilitar o monitoramento.  
