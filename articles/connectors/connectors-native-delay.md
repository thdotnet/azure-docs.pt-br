---
title: Atrasar a próxima ação em fluxos de trabalho - aplicativos lógicos do Azure
description: Esperar para executar a próxima ação em fluxos de trabalho de aplicativo lógico usando os atraso ou atraso até que ações em aplicativos lógicos do Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
tags: connectors
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: 27475fb3f086dbc5166a473e9d657d2dab723938
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66297675"
---
# <a name="delay-running-the-next-action-in-azure-logic-apps"></a>Atraso em execução a próxima ação em aplicativos lógicos do Azure

Para que seu aplicativo lógico Aguarde um período de tempo antes de executar a próxima ação, você pode adicionar o interno **atrasar – agendar** ação antes de uma ação no fluxo de trabalho do aplicativo lógico. Ou, você pode adicionar o interno **atrasar até – agendar** ação para aguardar até que uma data e hora específicas antes de executar a próxima ação. Para obter mais informações sobre as ações internas de agenda e os gatilhos, consulte [agendamento e execução recorrente, automatizado, tarefas e fluxos de trabalho com aplicativos lógicos do Azure](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

* **atraso**: Aguarde até que o número especificado de unidades de tempo, segundos, minutos, horas, dias, semanas ou meses, antes da próxima execução da ação.

* **Atrasar até**: Aguarde até a data e hora especificadas antes da próxima execução da ação.

Aqui estão algumas maneiras de exemplo para usar essas ações:

* Aguardar até um dia útil para enviar uma atualização de status por email.

* Atrase o fluxo de trabalho até que uma chamada HTTP seja concluído antes da retomada e recuperação de dados.

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure. Se você não tiver uma assinatura, você poderá [inscrever-se para uma conta gratuita do Azure](https://azure.microsoft.com/free/).

* Conhecimento básico sobre [aplicativos lógicos](../logic-apps/logic-apps-overview.md). Antes de usar uma ação, seu aplicativo lógico pela primeira vez deve começar com um gatilho. Você pode usar qualquer gatilho desejado e adicionar outras ações antes de adicionar uma ação de atraso. Este tópico usa um gatilho do Outlook do Office 365. Se você estiver familiarizado com aplicativos lógicos, saiba [como criar seu primeiro aplicativo lógico](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="add-delay"></a>

## <a name="add-the-delay-action"></a>Adicione a ação de atraso

1. No Designer do aplicativo lógico, na etapa onde você deseja adicionar a ação de atraso, escolha **nova etapa**.

   Para adicionar a ação de atraso entre as etapas, mova o ponteiro sobre a seta que conecta as etapas. Escolha o sinal de adição (+) que aparece e selecione **adicionar uma ação**.

1. Na caixa de pesquisa, insira "atraso" como filtro. Na lista de ações, selecione esta ação: **Atraso**

   ![Adicionar ação de "Atraso"](./media/connectors-native-delay/add-delay-action.png)

1. Especifique a quantidade de tempo de espera antes da próxima execução da ação.

   ![Definir intervalo de tempo do atraso](./media/connectors-native-delay/delay-time-intervals.png)

   | Propriedade | Nome JSON | Obrigatório | Type | DESCRIÇÃO |
   |----------|-----------|----------|------|-------------|
   | Count | count | Sim | Número inteiro | O número de unidades de tempo a serem atrasadas |
   | Unidade | unit | Sim | Cadeia de caracteres | A unidade de tempo, por exemplo: `Second`, `Minute`, `Hour`, `Day`, `Week`, ou `Month` |
   ||||||

1. Adicione quaisquer outras ações que você deseja executar no fluxo de trabalho.

1. Quando terminar, salve o aplicativo lógico.

<a name="add-delay-until"></a>

## <a name="add-the-delay-until-action"></a>Adicionar o atraso-ação until

1. No Designer do aplicativo lógico, na etapa onde você deseja adicionar a ação de atraso, escolha **nova etapa**.

   Para adicionar a ação de atraso entre as etapas, mova o ponteiro sobre a seta que conecta as etapas. Escolha o sinal de adição (+) que aparece e selecione **adicionar uma ação**.

1. Na caixa de pesquisa, insira "atraso" como filtro. Na lista de ações, selecione esta ação: **Atrasar até**

   ![Adicionar ação "Atrasar até"](./media/connectors-native-delay/add-delay-until-action.png)

1. Forneça a data de término e a hora para quando você quiser retomar o fluxo de trabalho.

   ![Especificar o carimbo de hora de quando terminar o atraso](./media/connectors-native-delay/delay-until-timestamp.png)

   | Propriedade | Nome JSON | Obrigatório | Type | DESCRIÇÃO |
   |----------|-----------|----------|------|-------------|
   | Timestamp |  timestamp | Sim | Cadeia de caracteres | A data de término e a hora para retomar o fluxo de trabalho usando este formato: <p>YYYY-MM-DDThh:mm:ssZ <p>Por exemplo, se você quiser 18 de setembro de 2017 às 14H, especifique "2017-09-18T14:00:00Z". <p>**Observação:** Esse formato de hora deve seguir a [especificação de tempo de data ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) na [formato de hora UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), mas sem uma [deslocamento UTC](https://en.wikipedia.org/wiki/UTC_offset). Sem um fuso horário, você deve adicionar a letra "Z" no final sem espaços. Essa letra "Z" refere-se ao equivalente em [hora náutica](https://en.wikipedia.org/wiki/Nautical_time). |
   ||||||

1. Adicione quaisquer outras ações que você deseja executar no fluxo de trabalho.

1. Quando terminar, salve o aplicativo lógico.

## <a name="next-steps"></a>Próximas etapas

* [Criar, agendar e executar tarefas recorrentes e fluxos de trabalho com o gatilho de recorrência](../connectors/connectors-native-recurrence.md)
* [Conectores para aplicativos lógicos](../connectors/apis-list.md)