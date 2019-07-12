---
title: Alertas de segurança na Central de segurança do Azure | Microsoft Docs
description: Este tópico explica o que são alertas de segurança e os diferentes tipos disponíveis na Central de segurança do Azure.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: 1b71e8ad-3bd8-4475-b735-79ca9963b823
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 06f41bbfd97d5deb59e7bfd08615b2f28e256070
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571667"
---
# <a name="security-alerts-in-azure-security-center"></a>Alertas de segurança na Central de segurança do Azure

Este artigo apresenta os diferentes tipos de alertas de segurança disponíveis na Central de segurança do Azure (ASC). Há uma variedade de alertas para os vários tipos de recursos diferente. ASC gera alertas para ambos os recursos implantados no Azure e recursos implantados no local ou híbrido, ambientes de nuvem. 

## <a name="what-are-security-alerts"></a>O que são alertas de segurança?

Os alertas são notificações que a Central de segurança gera quando ele detecta ameaças em seus recursos. Ele prioriza e lista os alertas, juntamente com as informações necessárias para que você possa investigar rapidamente o problema. A Central de segurança também fornece recomendações para como você pode corrigir um ataque.

## <a name="how-does-security-center-detect-threats"></a>Como a Central de segurança detectar ameaças?

Para detectar ameaças reais e reduzir os falsos positivos, a Central de segurança coleta, analisa e integra-se os dados de log de seus recursos do Azure, a rede e soluções de parceiros conectadas, como firewall e soluções de proteção de ponto de extremidade. Central de segurança analisa essas informações geralmente correlacionando informações de várias fontes para identificar ameaças.

ASC monitora os recursos se implantado no Azure ou implantado em outros locais e híbridos ambientes de nuvem. Para saber mais sobre como detectar e responder a ameaças, consulte [como a Central de segurança detecta e responde às ameaças](security-center-detection-capabilities.md#asc-detects).

## <a name="security-alert-types"></a>Tipos de alertas de segurança

Os tópicos a seguir orientam você através de alertas do ASC diferentes acordo com os tipos de recursos:

* [Alertas de servidores e VMs de IaaS](security-center-alerts-iaas.md)
* [Computação nativa de alertas](security-center-alerts-compute.md)
* [Alertas de serviços de dados](security-center-alerts-data-services.md)

Os tópicos a seguir explicam como a Central de segurança aproveita a telemetria de diferente que coleta ao integrar com a infraestrutura do Azure para aplicar as camadas de proteção adicional para os recursos implantados no Azure:

* [Alertas de camada de serviço](security-center-alerts-service-layer.md)
* [Integração com produtos de segurança do Azure](security-center-alerts-integration.md)

## <a name="what-are-alert-incidents"></a>Quais são os incidentes de alertas?

Um incidente de segurança é uma coleção de alertas relacionados, em vez de listar cada alerta individualmente. A Central de segurança usa fusion para correlacionar os alertas diferentes e sinais de baixa fidelidade em incidentes de segurança.

Usando a incidentes, a Central de segurança fornece uma exibição única de uma campanha de ataque e todos os alertas relacionados. Esta exibição permite que você a entender rapidamente quais ações o invasor executou e quais recursos foram afetados. Para obter mais informações, consulte [correlação de alertas inteligentes de nuvem](security-center-alerts-cloud-smart.md).

## <a name="get-started-with-alerts"></a>Comece com alertas

Consulte os tópicos a seguir para entender mais sobre os recursos monitorados pelo ASC e para obter diretrizes sobre como responder aos alertas apresentados pelo ASC.

* Para ver quais plataformas e recursos estão protegidos pelo ASC, consulte [recursos com suporte pela Central de segurança do Azure e plataformas](security-center-os-coverage.md).  
* Para entender quais são os incidentes de segurança e como ASC responde a eles, consulte [como lidar com incidentes de segurança na Central de segurança do Azure](security-center-incident.md). 
* Para saber como gerenciar os alertas que você recebe, consulte [Gerenciando e respondendo a alertas de segurança na Central de segurança do Azure](security-center-managing-and-responding-alerts.md).
* Para obter informações sobre como validar que a Central de segurança está configurada corretamente e para estimular um alerta de teste, consulte [validação de alertas na Central de segurança do Azure](security-center-alert-validation.md).  


## <a name="upgrade-to-standard-for-advanced-detections"></a>Atualizar para o padrão para as detecções avançadas

Para configurar as detecções avançadas, atualize para a Central de Segurança do Azure Standard. 

1. No menu à Central de segurança, selecione **política de segurança**.
2. Para as assinaturas que você deseja mover para a camada Standard, clique em **editar as configurações**. 
3. Na página de configurações, selecione **tipo de preço**. 
   Uma avaliação gratuita está disponível por um mês. Para saber mais, consulte a [página de preços](https://azure.microsoft.com/pricing/details/security-center/). 

## <a name="next-steps"></a>Próximas etapas

Neste artigo, você aprendeu quais são os diferentes tipos de alertas disponíveis na Central de segurança e alertas de segurança. Para mais informações, consulte os seguintes tópicos:

* [Guia de planejamento e operações da Central de Segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)
* [Perguntas frequentes sobre a Central de Segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-faq): Encontre as perguntas frequentes sobre como usar o serviço.

