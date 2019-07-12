---
title: Escalar verticalmente um cluster do Data Explorer do Azure para acomodar a demanda de alteração
description: Este artigo descreve as etapas para escalar verticalmente e reduzir verticalmente um cluster do Gerenciador de dados do Azure com base na demanda de alteração.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/30/2019
ms.openlocfilehash: dc9ca8bb592e699d19835efeafb91e81408ae297
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571524"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Gerenciar o aumento do cluster para acomodar as mudanças de demanda

Há dois fluxos de trabalho para dimensionar um cluster do Gerenciador de dados do Azure:
1. [O dimensionamento horizontal](manage-cluster-horizontal-scaling.md), também chamado e redução de escala.
2. Dimensionamento vertical, também chamado de aumento e redução.

Este artigo mostra como gerenciar o dimensionamento vertical do cluster.

Dimensionar um cluster apropriadamente é essencial para o desempenho do Azure Data Explorer. Mas por demanda em um cluster não pode ser prevista com exatidão absoluta. Um tamanho de cluster estático pode levar a subutilização ou superutilização, nenhum deles é ideal. Uma abordagem melhor é *escala* um cluster, adicionando e removendo a capacidade e os recursos de CPU com a demanda de alteração. 

## <a name="steps-to-configure-vertical-scaling"></a>Etapas para configurar o dimensionamento vertical

1. Vá para seu cluster. Sob **as configurações**, selecione **escalar verticalmente**.

    Você vê uma lista de SKUs disponíveis. Por exemplo, na figura a seguir, apenas quatro SKUs estão disponíveis.

    ![Escalar verticalmente](media/manage-cluster-vertical-scaling/scale-up.png)

    SKUs estão desabilitados porque eles são o SKU atual, ou eles não estão disponíveis na região em que o cluster está localizado.

1. Para alterar a SKU, selecione a SKU desejado e escolha o **selecionar** botão.

> [!NOTE]
> O processo de dimensionamento vertical pode levar alguns minutos e, durante esse tempo o cluster será suspenso. Observe que reduzir verticalmente pode prejudicar o desempenho do cluster.

Agora, você fez uma operação de ampliação ou redução de escala para o cluster do Gerenciador de dados do Azure.

Se precisar de ajuda com problemas de dimensionamento de cluster [abrir uma solicitação de suporte](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) no portal do Azure.

## <a name="next-steps"></a>Próximas etapas

* [Gerenciar o dimensionamento horizontal do cluster](manage-cluster-horizontal-scaling.md) que dinamicamente escalar horizontalmente a contagem de instâncias com base nas métricas que você especificar.

* Monitore o uso de recursos, seguindo este artigo: [Monitorar o desempenho do Data Explorer do Azure, integridade e uso com métricas](using-metrics.md).

