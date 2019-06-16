---
title: Visão geral do Insights no Azure Monitor | Microsoft Docs
description: Insights fornecem uma experiência de monitoramento personalizada no Azure Monitor para serviços e aplicativos específicos. Este artigo fornece uma breve descrição de cada uma das informações que estão disponíveis no momento.
services: azure-monitor
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: bwren
ms.openlocfilehash: 8cb39a174c570b7019e872d731f49252a9505406
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66247223"
---
# <a name="overview-of-insights-in-azure-monitor"></a>Visão geral do Insights no Azure Monitor
Insights fornecem uma experiência de monitoramento personalizada para determinados aplicativos e serviços. Eles armazenam dados na [plataforma de dados do Azure Monitor](../platform/data-platform.md) e aproveitar outros recursos do Azure Monitor para análise e alertas, mas pode coletar dados adicionais e fornecer uma experiência de usuário exclusiva no portal do Azure. Acessar informações sobre o **Insights** seção do menu do Azure Monitor no portal do Azure.

As seções a seguir fornecem uma breve descrição das informações que estão atualmente disponíveis no Azure Monitor. Consulte a documentação detalhada para obter detalhes sobre cada um.

## <a name="application-insights"></a>Application Insights
O Application Insights é um serviço de gerenciamento de desempenho de aplicativo (APM) extensível para desenvolvedores da Web em várias plataformas. Use-o para monitorar seu aplicativo Web online. Ele funciona para aplicativos em uma ampla variedade de plataformas, incluindo .NET, Node. js e Java EE, hospedado no local, híbrido ou qualquer nuvem pública. Ele também se integra ao seu processo DevOps e tem pontos de conexão para uma variedade de ferramentas de desenvolvimento.

Ver [o que é o Application Insights?](../app/app-insights-overview.md).

![Application Insights](media/insights-overview/app-insights.png)

## <a name="azure-monitor-for-containers"></a>O Azure Monitor para contêineres
O Azure Monitor para contêineres monitora o desempenho das cargas de trabalho de contêineres implantadas nas Instâncias de Contêiner do Azure ou nos clusters Kubernetes gerenciados hospedados no AKS (Serviço de Kubernetes do Azure). Monitorar os contêineres é fundamental, principalmente ao executar um cluster de produção em grande escala e com vários aplicativos.

Ver [Azure Monitor para a visão geral dos contêineres](../insights/container-insights-overview.md).

![O Azure Monitor para contêineres](media/insights-overview/container-insights.png)

## <a name="azure-monitor-for-resource-groups-preview"></a>O Azure Monitor para grupos de recursos (visualização)
O Azure Monitor para grupos de recursos ajuda a triagem e diagnosticar quaisquer problemas que encontram os recursos individuais, além de oferecer o contexto sobre a integridade e o desempenho do grupo de recursos como um todo.

Ver [monitorar grupos de recursos com o Azure Monitor (versão prévia)](../insights/resource-group-insights.md).

![O Azure Monitor para grupos de recursos](media/insights-overview/resource-group-insights.png)

## <a name="azure-monitor-for-vms-preview"></a>O Azure Monitor para máquinas virtuais (versão prévia)
O Azure Monitor para VMs monitora as VMs (máquinas virtuais) e os conjuntos de dimensionamento de máquinas virtuais do Azure em escala. Ele analisa o desempenho e a integridade das VMs do Windows e do Linux e monitora os processos e as dependências de outros recursos e processos externos.

Consulte [o que é o Azure Monitor para VMs?](vminsights-overview.md)

![Azure Monitor para VMs](media/insights-overview/vm-insights.png)

## <a name="next-steps"></a>Próximas etapas
* Saiba mais sobre o [plataforma de dados do Azure Monitor](../platform/data-platform.md) aproveitado por insights.
* Saiba mais sobre os diferentes [fontes de dados usadas pelo Azure Monitor](../platform/data-sources.md) e os diferentes tipos de dados coletados por cada uma das informações.
