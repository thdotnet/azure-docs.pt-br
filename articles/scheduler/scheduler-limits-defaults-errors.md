---
title: Limites e cotas e limitações no Agendador do Azure
description: Saiba mais sobre limites, cotas, valores padrão e limitações do Agendador do Azure
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: 293cd956f8270a4863fcc657f58c970096cec1e3
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71300914"
---
# <a name="limits-quotas-and-throttle-thresholds-in-azure-scheduler"></a>Limites e cotas e limitações no Agendador do Azure

> [!IMPORTANT]
> O [aplicativo lógico do Azure](../logic-apps/logic-apps-overview.md) está substituindo o Agendador do Azure, que está [sendo desativado](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date). Para continuar trabalhando com os trabalhos que você configurou no Agendador, [migre para o aplicativo lógico do Azure](../scheduler/migrate-from-scheduler-to-logic-apps.md) assim que possível.

## <a name="limits-quotas-and-thresholds"></a>Limites, cotas e limitações

[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="x-ms-request-id-header"></a>Cabeçalho x-ms-request-id

Cada solicitação feita no serviço do Agendador retorna um cabeçalho de resposta chamado **x-ms-request-id**. Esse cabeçalho contém um valor opaco que identifica exclusivamente a solicitação. Portanto, se você confirmar que uma solicitação está formatada corretamente e ela falhar de forma consistente, relate o erro à Microsoft fornecendo o valor do cabeçalho de resposta **x-ms-request-id** e incluindo estes detalhes: 

* O valor de **x-ms-request-id**
* O tempo aproximado quando a solicitação foi feita 
* Os identificadores da assinatura do Azure, da coleção de trabalhos e do trabalho 
* O tipo de operação que a solicitação tentou

## <a name="see-also"></a>Consulte também

* [O que é o Agendador do Azure?](scheduler-intro.md)
* [Conceitos, terminologia e hierarquia de entidades do Agendador do Azure](scheduler-concepts-terms.md)
