---
title: Agendar tarefas recorrentes com gatilho de janela deslizante - aplicativos lógicos do Azure
description: Agendar e executar tarefas automatizadas recorrentes e fluxos de trabalho com o gatilho de janela deslizante em aplicativos lógicos do Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: 44944955019fcf81fb0d296592577e2b00a15928
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66299496"
---
# <a name="create-schedule-and-run-recurring-tasks-and-workflows-with-the-sliding-window-trigger-in-azure-logic-apps"></a>Criar, agendar e executar tarefas recorrentes e fluxos de trabalho com o gatilho de janela deslizante em aplicativos lógicos do Azure

Para executar regularmente os trabalhos que precisam lidar com dados em partes contínuas, processos ou tarefas, você pode iniciar o fluxo de trabalho de aplicativo lógico com o **janela deslizante - agenda** gatilho. Você pode definir uma data e hora, bem como um fuso horário para iniciar o fluxo de trabalho e uma recorrência de repetição do fluxo de trabalho. Se as recorrências não estão presentes por algum motivo, esse gatilho processa essas recorrências perdidas. Por exemplo, ao sincronizar dados entre seu banco de dados e armazenamento de backup, use o gatilho de janela deslizante para que os dados são sincronizados sem incorrer em lacunas. Para obter mais informações sobre os gatilhos internos de agendamento e ações, consulte [agendamento e execução recorrente, automatizado, tarefas e fluxos de trabalho com aplicativos lógicos do Azure](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

Aqui estão alguns padrões que dá suporte a esse gatilho:

* Executar imediatamente e repetir a cada *n* número de segundos, minutos ou horas.

* Iniciar em uma determinada data e hora, em seguida, executar e repetir a cada *n* número de segundos, minutos ou horas. Com esse gatilho, você pode especificar uma hora de início no passado, que executa todos os anteriores recorrências.

* Atrase cada recorrência por uma duração específica antes de executar.

As diferenças entre esse gatilho e o gatilho de recorrência ou para obter mais informações sobre o agendamento de fluxos de trabalho recorrentes, consulte [automatizada de agendamento e execução recorrente tarefas, processos e fluxos de trabalho com aplicativos lógicos do Azure](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

> [!TIP]
> Se você deseja disparar seu aplicativo lógico e executar apenas uma vez no futuro, consulte [execução de trabalhos somente uma vez](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#run-once).

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure. Se você não tiver uma assinatura, você poderá [inscrever-se para uma conta gratuita do Azure](https://azure.microsoft.com/free/).

* Conhecimento básico sobre [aplicativos lógicos](../logic-apps/logic-apps-overview.md). Se você estiver familiarizado com aplicativos lógicos, saiba [como criar seu primeiro aplicativo lógico](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-sliding-window-trigger"></a>Adicionar gatilho de janela deslizante

1. Entre no [Portal do Azure](https://portal.azure.com). Criar um aplicativo lógico em branco.

1. Depois que o Designer de aplicativos lógicos é exibido na caixa de pesquisa, digite "janela deslizante" como filtro. Na lista de gatilhos, selecione esse gatilho como a primeira etapa no fluxo de trabalho de aplicativo lógico: **Janela deslizante**

   ![Selecione o gatilho "Janela deslizante"](./media/connectors-native-sliding-window/add-sliding-window-trigger.png)

1. Defina o intervalo e a frequência da recorrência. Neste exemplo, defina essas propriedades para que executem o fluxo de trabalho toda semana.

   ![Definir o intervalo e a frequência](./media/connectors-native-sliding-window/sliding-window-trigger-details.png)

   | Propriedade | Obrigatório | Nome JSON | Type | DESCRIÇÃO |
   |----------|----------|-----------|------|-------------|
   | **Intervalo** | Sim | intervalo | Número inteiro | Um inteiro positivo que descreve a frequência na qual o fluxo de trabalho é executado com base na frequência. Aqui estão os intervalos mínimos e máximos: <p>- Hora: 1 a 12.000 horas </br>- Minuto: 1 a 72.000 minutos </br>- Segundo: 1 a 9.999.999 segundos<p>Por exemplo, se o intervalo for 6, e a frequência for "Hour", a recorrência será a cada 6 horas. |
   | **Frequência** | Sim | frequência | Cadeia de caracteres | A unidade de tempo para a recorrência: **Segunda**, **minuto**, ou **hora** |
   ||||||

   ![Opções de recorrência avançadas](./media/connectors-native-sliding-window/sliding-window-trigger-more-options-details.png)

   Para obter mais opções de recorrência, abra o **adicionar novo parâmetro** lista. 
   As opções que você selecione aparecem no gatilho após a seleção.

   | Propriedade | Obrigatório | Nome JSON | Type | DESCRIÇÃO |
   |----------|----------|-----------|------|-------------|
   | **Atraso** | Não | delay | Cadeia de caracteres | A duração do atraso cada recorrência usando o [especificação de tempo de data ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Durations) |
   | **Fuso horário** | Não | timeZone | Cadeia de caracteres | Aplica-se somente quando você especifica uma hora de início, porque o gatilho não aceita [diferença UTC](https://en.wikipedia.org/wiki/UTC_offset). Selecione o fuso horário que você deseja aplicar. |
   | **Hora de início** | Não | startTime | Cadeia de caracteres | Forneça uma data de início e a hora neste formato: <p>AAAA-MM-DDThh:mm:ss se você selecionar um fuso horário <p>-ou- <p>AAAA-MM-DDThh:mm:ssZ se você não selecionar um fuso horário <p>Por exemplo, se você quiser 18 de setembro de 2017 às 2:00 PM, em seguida, especifique "2017-09-18T14:00:00" e selecione um fuso horário, como hora oficial do Pacífico. Ou, especifique "2017-09-18T14:00:00Z" sem um fuso horário. <p>**Observação:** a hora de início deve seguir a [especificação de data e hora ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) no [formato de data e hora UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), mas sem uma [diferença UTC](https://en.wikipedia.org/wiki/UTC_offset). Se você não selecionar um fuso horário, será necessário adicionar a letra "Z" no final sem espaços. Essa letra "Z" refere-se ao equivalente em [hora náutica](https://en.wikipedia.org/wiki/Nautical_time). <p>Para agendamentos simples, a hora de início é a primeira ocorrência, enquanto para recorrências avançadas, o gatilho não é disparado antes da hora de início. [*Quais são as maneiras que posso usar a data e hora de início?* ](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time) |
   |||||

1. Agora, compile seu fluxo de trabalho restante com outras ações. Para obter mais ações que você pode adicionar, consulte [conectores para aplicativos lógicos do Azure](../connectors/apis-list.md).

## <a name="workflow-definition---sliding-window"></a>Definição de fluxo de trabalho - janela deslizante

Na definição fluxo de trabalho subjacente do seu aplicativo lógico que usa JSON, você pode exibir a definição do gatilho de janela deslizante com as opções que você escolheu. Para exibir essa definição, na barra de ferramentas designer, escolha **exibição de código**. Para retornar para o designer, escolha na barra de ferramentas designer **Designer**.

Este exemplo mostra como uma definição de gatilho de janela deslizante pode parecer em uma definição de fluxo de trabalho subjacente em que o atraso para cada recorrência é de cinco segundos para uma recorrência por hora:

``` json
"triggers": {
   "Recurrence": {
      "type": "SlidingWindow",
      "Sliding_Window": {
         "inputs": {
            "delay": "PT5S"
         },
         "recurrence": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2019-05-13T14:00:00Z",
            "timeZone": "Pacific Standard Time"
         }
      }
   }
}
```

## <a name="next-steps"></a>Próximas etapas

* [Atrasar a próxima ação em fluxos de trabalho](../connectors/connectors-native-delay.md)
* [Conectores para aplicativos lógicos](../connectors/apis-list.md)