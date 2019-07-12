---
title: Nuvem inteligente correlação de alertas na Central de segurança do Azure (incidentes) | Microsoft Docs
description: Este tópico explica como fusion usa nuvem inteligente correlação de alertas para gerar incidentes de segurança na Central de segurança do Azure.
services: security-center
documentationcenter: na
author: monhaber
manager: rkarlin
editor: ''
ms.assetid: e9d5a771-bfbe-458c-9a9b-a10ece895ec1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/02/2019
ms.author: monhaber
ms.openlocfilehash: 2ab4dab8cb7729b0c2ca023f22066f7b5d166a02
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571771"
---
# <a name="cloud-smart-alert-correlation-in-azure-security-center-incidents"></a>Correlação de alertas inteligente de nuvem na Central de segurança do Azure (incidentes)

A Central de segurança analisa continuamente as cargas de trabalho de nuvem híbrida usando análise avançada e inteligência contra ameaças para alertar você sobre atividades mal-intencionadas.

Conforme cresce a abrangência de cobertura de ameaça e a necessidade de detectar até mesmo a indicação mesmo de comprometida aumenta, ele pode ser um desafio para os analistas de segurança para os alertas diferentes de triagem e identificar um ataque real. A Central de segurança ajuda os analistas de lidar com alerta fadiga e diagnosticando ataques conforme elas ocorrem, correlacionando os alertas diferentes e sinais de baixa fidelidade em incidentes de segurança.

Fusion é a tecnologia e analítica back-end, que é usada para incidentes de Central de segurança, habilitá-la para correlacionar os alertas diferentes e contextuais sinais juntos. Fusão funciona, observando os sinais diferentes são relatados em uma assinatura entre os recursos e encontrar padrões predominantes que mostra a progressão de ataque ou sinaliza o com informações contextuais compartilhadas que indica um procedimento de resposta unificada deve ser feito para eles.

Análise de fusão combina dados de Conhecimento de domínio de segurança com inteligência Artificial para analisar os alertas, descobrindo novos padrões de ataque, conforme elas ocorrem. 

A Central de segurança aproveita MITRE matriz de ataque para associar alertas a respectiva intenção percebida, ajudando a formalizar o conhecimento do domínio de segurança. Além disso, usando as informações coletadas para cada etapa de um ataque, a Central de segurança pode eliminar atividade que parece ser etapas de um ataque, mas não é.  

Como ataques geralmente ocorrem em locatários diferentes, a Central de segurança pode combinar os algoritmos de inteligência Artificial para analisar sequências de ataque que são relatadas em cada assinatura para identificá-los como padrões de alerta predominantes em vez de apenas sendo por acaso associadas a cada outros.

Durante uma investigação de um incidente, os analistas muitas vezes precisam contexto extra para atingirem um veredito sobre a natureza da ameaça e como resolvê-la. Por exemplo, mesmo quando for detectada uma anomalia de rede, sem compreender o que mais está acontecendo na rede ou em relação ao recurso de destino é difícil de entender as ações a serem tomadas. Para ajudar, um incidente de segurança pode incluir artefatos, eventos relacionados e informações. As informações adicionais disponíveis para incidentes de segurança variam dependendo do tipo de ameaça detectada e a configuração do seu ambiente. 

![Detalhes do incidente de segurança](./media/security-center-alerts-cloud-smart/security-incident.png)

Para entender melhor os incidentes de segurança, consulte [como lidar com incidentes de segurança na Central de segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-incident).

