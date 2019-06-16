---
title: Recomendações de segurança na Central de segurança do Azure | Microsoft Docs
description: Este documento mostra como as recomendações na Central de Segurança do Azure ajudam a proteger os recursos do Azure e a cumprir as políticas de segurança.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 86c50c9f-eb6b-4d97-acb3-6d599c06133e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/13/2019
ms.author: v-mohabe
ms.openlocfilehash: fe1d4bf27f3c4bb1f70c1c1fa9767c27f8767998
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064214"
---
# <a name="security-recommendations-in-azure-security-center"></a>Recomendações de segurança na Central de Segurança do Azure 
Este tópico explica como exibir e entender as recomendações na Central de segurança do Azure para ajudar a proteger seus recursos do Azure.

> [!NOTE]
> Este documento apresenta o serviço usando uma implantação de exemplo.  Este documento não é um guia passo a passo.
>

## <a name="what-are-security-recommendations"></a>O que são recomendações de segurança?
A Central de Segurança analisa periodicamente o estado de segurança de seus recursos do Azure. Quando a Central de Segurança identifica possíveis vulnerabilidades de segurança, cria recomendações. As recomendações o orientam ao longo do processo de configuração dos controles necessários.

## <a name="implementing-security-recommendations"></a>Implementando recomendações de segurança
### <a name="set-recommendations"></a>Definir recomendações
Em [Configurando políticas de segurança na Central de Segurança do Azure](tutorial-security-policy.md), você aprenderá a:

* Configurar políticas de segurança.
* Habilitar a coleta de dados.
* Escolher quais recomendações ver como parte da política de segurança.

As recomendações de política atuais giram em torno de atualizações do sistema, regras de linha de base, programas antimalware, [grupos de segurança de rede](../virtual-network/security-overview.md) em sub-redes e em interfaces de rede, auditoria do banco de dados SQL, Transparent Data Encryption do banco de dados SQL e firewalls do aplicativo Web.  [Configurando políticas de segurança](tutorial-security-policy.md) fornece uma descrição de cada opção de recomendação.

### <a name="monitor-recommendations"></a>Monitorar as recomendações
Depois de definir uma política de segurança, a Central de Segurança analisa o estado de segurança de seus recursos para identificar possíveis vulnerabilidades. O **recomendações** lado a lado sob **visão geral** mostra o número total de recomendações identificadas pela Central de segurança.

![Visão geral da Central de segurança](./media/security-center-recommendations/asc-overview.png)

1. Selecione o **bloco recomendações** sob **visão geral**. O **recomendações** lista é aberta.
    
      ![Exibir recomendações](./media/security-center-recommendations/view-recommendations.png)

    Você pode filtrar as recomendações. Para filtrar as recomendações, selecione **Filtro** na folha **Recomendações**. A folha **Filtro** é aberta e você seleciona os valores de gravidade e de estado que deseja ver.

   * **RECOMENDAÇÕES**: A recomendação.
   * **PROTEGER O IMPACTO DE PONTUAÇÃO**: Uma pontuação gerada pela Central de segurança usando suas recomendações de segurança e aplicando algoritmos avançados para determinar como crucial cada recomendação é. Para obter mais informações, consulte [proteger o cálculo de pontuação](security-center-secure-score.md#secure-score-calculation).
   * **RECURSO:** lista os recursos aos quais essa recomendação se aplica.
   * **BARRAS DE STATUS**:  Descreve a gravidade dessa recomendação específica:
       * **Alta (vermelho)** : existe uma vulnerabilidade em um recurso significativo (como um aplicativo, uma VM ou um grupo de segurança de rede) e ela requer atenção.
       * **Médio (laranja)** : existe uma vulnerabilidade e etapas não críticas ou adicionais são necessárias para eliminá-la ou para concluir um processo.
       * **Baixa (azul)** : existe uma vulnerabilidade que deve ser resolvida, mas não exige atenção imediata. (Por padrão, não são apresentadas recomendações baixas, mas você pode filtrar as recomendações baixas caso deseje vê-las.) 
       * **Íntegro (verde)** :
       * **Não disponível (cinza)** :

1. Para exibir os detalhes de cada recomendação, clique na recomendação.

    ![Detalhes da recomendação](./media/security-center-recommendations/recommendation-details.png)

>[!NOTE] 
> Ver [clássico e modelos de implantação do Resource Manager](../azure-classic-rm.md) para recursos do Azure.
  
 ### <a name="apply-recommendations"></a>Aplicar recomendações
> Depois de examinar todas as recomendações, decida qual delas aplicar primeiro. É recomendável que você use o seguro pontuar impacto para avaliar quais recomendações devem ser aplicadas primeiro.

1. Na lista, clique na recomendação.
1. Siga as instruções de *etapas de correção* seção.

## <a name="next-steps"></a>Próximas etapas
Neste documento, você foi apresentado às recomendações de segurança da Central de Segurança. Para saber mais sobre a Central de segurança, consulte os tópicos a seguir:

* [Configurando políticas de segurança na Central de Segurança do Azure](tutorial-security-policy.md) : saiba como configurar políticas de segurança para suas assinaturas e grupos de recursos do Azure.
* [Monitoramento da integridade de segurança na Central de Segurança do Azure](security-center-monitoring.md) : saiba como monitorar a integridade dos recursos do Azure.
* [Gerenciando e respondendo aos alertas de segurança na Central de Segurança do Azure](security-center-managing-and-responding-alerts.md) : aprenda a gerenciar e responder aos alertas de segurança.
* [Monitorando as soluções de parceiros com a Central de Segurança do Azure](security-center-partner-solutions.md) : saiba como monitorar o status de integridade de suas soluções de parceiros.
* [Perguntas frequentes da Central de Segurança do Azure](security-center-faq.md) : encontre as perguntas frequentes sobre como usar o serviço.
* [Blog de Segurança do Azure](https://blogs.msdn.com/b/azuresecurity/) : encontre postagens no blog sobre conformidade e segurança do Azure.
