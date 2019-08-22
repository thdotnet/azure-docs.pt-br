---
title: Plano de continuidade dos negócios – QnA Maker
titleSuffix: Azure Cognitive Services
description: O principal objetivo do plano de continuidade de negócios é criar um ponto de extremidade resiliente da base de conhecimento que garantiria a total ausência de tempo de inatividade para o bot ou aplicativo que a estivesse consumindo.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/20/2019
ms.author: diberry
ms.openlocfilehash: 84f13f7e1d83f1ead00303b694b617d3ba1c8931
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876639"
---
# <a name="create-a-business-continuity-plan-for-your-qna-maker-service"></a>Criar um plano de continuidade de negócios para o seu serviço QnA Maker

O principal objetivo do plano de continuidade de negócios é criar um ponto de extremidade resiliente da base de conhecimento que garantiria a total ausência de tempo de inatividade para o bot ou aplicativo que a estivesse consumindo.

![Plano de backup do QnA Maker](../media/qnamaker-how-to-bcp-plan/qnamaker-bcp-plan.png)

A ideia de alto nível como representada acima é a seguinte:

1. Configurar dois [serviços QnA Maker](../How-To/set-up-qnamaker-service-azure.md) em paralelo nas [regiões emparelhadas do Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

2. Manter os índices de pesquisa primários e secundários do Azure em sincronia. Use o exemplo do GitHub [aqui](https://github.com/pchoudhari/QnAMakerBackupRestore) para ver como fazer o backup e a restauração dos índices do Azure.

3. Fazer backup do Application Insights usando [exportação contínua](https://docs.microsoft.com/azure/application-insights/app-insights-export-telemetry).

4. Depois que as pilhas primárias e secundárias forem configuradas, use o [gerenciador de tráfego](https://docs.microsoft.com/azure/traffic-manager/) para configurar os dois pontos de extremidade e configurar um método de roteamento.

5. Você precisará criar um certificado SSL para o ponto de extremidade do gerenciador de tráfego. [Associar o certificado SSL](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-ssl) nos serviços de Aplicativo.

6. Por fim, use o ponto de extremidade do gerenciador de tráfego em seu bot ou aplicativo.

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Escolher a capacidade para sua implantação do QnA Maker](../Tutorials/choosing-capacity-qnamaker-deployment.md)
