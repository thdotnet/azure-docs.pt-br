---
title: Agendar tarefas recorrentes com gatilho de recorrência - aplicativos lógicos do Azure
description: Agendar e executar tarefas automatizadas recorrentes e fluxos de trabalho com o gatilho de recorrência em aplicativos lógicos do Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: f5fc778ee4d8f91232bc732cc276f642f748b29d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66297517"
---
# <a name="create-schedule-and-run-recurring-tasks-and-workflows-with-the-recurrence-trigger-in-azure-logic-apps"></a>Criar, agendar e executar tarefas recorrentes e fluxos de trabalho com o gatilho de recorrência em aplicativos lógicos do Azure

Para executar regularmente os trabalhos, tarefas ou processos em agendamento específico, você pode iniciar o fluxo de trabalho do aplicativo lógico com internos **recorrência - agenda** gatilho. Você pode definir uma data e hora, bem como um fuso horário para iniciar o fluxo de trabalho e uma recorrência de repetição do fluxo de trabalho. Se as recorrências não estão presentes por algum motivo, esse gatilho continua recorrente no próximo intervalo agendado. Para obter mais informações sobre os gatilhos internos de agendamento e ações, consulte [agendamento e execução recorrente, automatizado, tarefas e fluxos de trabalho com aplicativos lógicos do Azure](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

Aqui estão alguns padrões que esse gatilho dá suporte a junto com as recorrências mais avançadas e agendamentos complexos:

* Executar imediatamente e repetir a cada *n* número de segundos, minutos, horas, dias, semanas ou meses.

* Iniciar em uma determinada data e hora, em seguida, executar e repetir a cada *n* número de segundos, minutos, horas, dias, semanas ou meses.

* Executar e repetir uma ou mais vezes por dia, por exemplo, às 8h e 17h.

* Executar e repetir a cada semana, mas apenas em dias específicos, como sábado e domingo.

* Executar e repetir a cada semana, mas somente em determinados dias e horas, como de segunda a sexta-feira, às 8h e às 17h.

As diferenças entre esse gatilho e o gatilho de janela deslizante, ou para obter mais informações sobre o agendamento de fluxos de trabalho recorrentes, consulte [automatizada de agendamento e execução recorrente tarefas, processos e fluxos de trabalho com aplicativos lógicos do Azure](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

> [!TIP]
> Se você deseja disparar seu aplicativo lógico e executar apenas uma vez no futuro, consulte [execução de trabalhos somente uma vez](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#run-once).

## <a name="prerequisites"></a>Pré-requisitos

* Uma assinatura do Azure. Se você não tiver uma assinatura, você poderá [inscrever-se para uma conta gratuita do Azure](https://azure.microsoft.com/free/).

* Conhecimento básico sobre [aplicativos lógicos](../logic-apps/logic-apps-overview.md). Se você estiver familiarizado com aplicativos lógicos, saiba [como criar seu primeiro aplicativo lógico](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-recurrence-trigger"></a>Adicionar gatilho de recorrência

1. Entre no [Portal do Azure](https://portal.azure.com). Criar um aplicativo lógico em branco.

1. Depois que o Designer de aplicativos lógicos é exibido na caixa de pesquisa, digite "recorrência" como filtro. Na lista de gatilhos, selecione esse gatilho como a primeira etapa no fluxo de trabalho de aplicativo lógico: **Recorrência**

   ![Selecione o gatilho "Recorrência"](./media/connectors-native-recurrence/add-recurrence-trigger.png)

1. Defina o intervalo e a frequência da recorrência. Neste exemplo, defina essas propriedades para que executem o fluxo de trabalho toda semana.

   ![Definir o intervalo e a frequência](./media/connectors-native-recurrence/recurrence-trigger-details.png)

   | Propriedade | Obrigatório | Nome JSON | Type | DESCRIÇÃO |
   |----------|----------|-----------|------|-------------|
   | **Intervalo** | Sim | intervalo | Número inteiro | Um inteiro positivo que descreve a frequência na qual o fluxo de trabalho é executado com base na frequência. Aqui estão os intervalos mínimos e máximos: <p>- Mês: 1 a 16 meses </br>- Dia: 1 a 500 dias </br>- Hora: 1 a 12.000 horas </br>- Minuto: 1 a 72.000 minutos </br>- Segundo: 1-9.999.999 segundos<p>Por exemplo, se o intervalo for 6 e a frequência for "Mês", a recorrência será a cada 6 meses. |
   | **Frequência** | Sim | frequência | Cadeia de caracteres | A unidade de tempo para a recorrência: **Segundo**, **Minuto**, **Hora**, **Dia**, **Semana** ou **Mês** |
   ||||||

   Para obter mais opções de agendamento, abra o **adicionar novo parâmetro** lista. 
   As opções que você selecione aparecem no gatilho após a seleção.

   ![Opções de agendamento avançadas](./media/connectors-native-recurrence/recurrence-trigger-more-options-details.png)

   | Propriedade | Obrigatório | Nome JSON | Type | DESCRIÇÃO |
   |----------|----------|-----------|------|-------------|
   | **Fuso horário** | Não | timeZone | Cadeia de caracteres | Aplica-se somente quando você especifica uma hora de início, porque o gatilho não aceita [diferença UTC](https://en.wikipedia.org/wiki/UTC_offset). Selecione o fuso horário que você deseja aplicar. |
   | **Hora de início** | Não | startTime | Cadeia de caracteres | Forneça uma data de início e a hora neste formato: <p>AAAA-MM-DDThh:mm:ss se você selecionar um fuso horário <p>-ou- <p>AAAA-MM-DDThh:mm:ssZ se você não selecionar um fuso horário <p>Por exemplo, se você quiser 18 de setembro de 2017 às 2:00 PM, em seguida, especifique "2017-09-18T14:00:00" e selecione um fuso horário, como hora oficial do Pacífico. Ou, especifique "2017-09-18T14:00:00Z" sem um fuso horário. <p>**Observação:** a hora de início deve seguir a [especificação de data e hora ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Combined_date_and_time_representations) no [formato de data e hora UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time), mas sem uma [diferença UTC](https://en.wikipedia.org/wiki/UTC_offset). Se você não selecionar um fuso horário, será necessário adicionar a letra "Z" no final sem espaços. Essa letra "Z" refere-se ao equivalente em [hora náutica](https://en.wikipedia.org/wiki/Nautical_time). <p>Para agendamentos simples, a hora de início é a primeira ocorrência, enquanto que, para agendamentos complexos, o gatilho não é disparado antes da hora de início. [*Quais são as maneiras que posso usar a data e hora de início?* ](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time) |
   | **Nestes dias** | Não | weekDays | Cadeia de caracteres ou matriz de cadeia de caracteres | Se você selecionar "Semana", poderá selecionar um ou mais dias em que você deseja executar o fluxo de trabalho: **Segunda-feira**, **Terça-feira**, **Quarta-feira**, **Quinta-feira**, **Sexta-feira**, **Sábado** e **Domingo** |
   | **A estas horas** | Não | hours | Inteiro ou matriz de inteiros | Se você selecionar "Dia" ou "Semana", você pode selecionar um ou mais números inteiros de 0 a 23 como as horas do dia para quando você deseja executar o fluxo de trabalho. <p><p>Por exemplo, se você especificar "10", "12" e "14", você obtém 10h, 12h e 14H para as horas do dia, mas os minutos do dia são calculados com base em quando começa a recorrência. Para definir os minutos do dia, especifique o valor para o **a estes minutos** propriedade. |
   | **A estes minutos** | Não | minutes | Inteiro ou matriz de inteiros | Se você selecionar "Dia" ou "Semana", poderá selecionar um ou mais números inteiros, de 0 a 59, como os minutos da hora em que você deseja executar o fluxo de trabalho. <p>Por exemplo, você pode especificar "30" como a marca de minutos e, usando o exemplo anterior como as horas do dia, você obtém 10h30, 12h30 e 14h30. |
   |||||

   Por exemplo, suponha que hoje é segunda-feira, 4 de setembro de 2017. O seguinte gatilho de recorrência não dispara *antes* à data de início e hora, que é segunda-feira, 18 de setembro de 2017 às 8:00 AM PST. No entanto, o agendamento da recorrência está definido para 10h30, 12h30 e 14h30, somente às segundas-feiras. Portanto, a primeira vez que o gatilho será acionado e criará uma instância de fluxo de trabalho do aplicativo lógico será às 10h30. Para saber mais sobre como as horas de início funcionam, consulte estes [exemplos de hora de início](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#start-time).

   As execuções futuras acontecerão às 12h30 e às 14h30 no mesmo dia. Cada recorrência cria sua própria instância de fluxo de trabalho. Depois disso, todo o agendamento se repete novamente na próxima segunda-feira. [*Quais são outras ocorrências de exemplo?* ](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md#example-recurrences)

   ![Exemplo de agentamento avançado](./media/connectors-native-recurrence/recurrence-trigger-more-options-advanced-schedule.png)

   > [!NOTE]
   > O gatilho mostra uma visualização da recorrência especificada somente quando você seleciona "Dia" ou "Semana" como a frequência.

1. Agora, compile seu fluxo de trabalho restante com outras ações. Para obter mais ações que você pode adicionar, consulte [conectores para aplicativos lógicos do Azure](../connectors/apis-list.md).

## <a name="workflow-definition---recurrence"></a>Definição de fluxo de trabalho – recorrência

Na definição fluxo de trabalho subjacente do seu aplicativo lógico que usa JSON, você pode exibir o [definição de gatilho de recorrência](../logic-apps/logic-apps-workflow-actions-triggers.md#recurrence-trigger) com as opções que você escolheu. Para exibir essa definição, na barra de ferramentas designer, escolha **exibição de código**. Para retornar para o designer, escolha na barra de ferramentas designer **Designer**.

Este exemplo mostra a aparência de uma definição de gatilho de recorrência em uma definição de fluxo de trabalho subjacente:

``` json
"triggers": {
   "Recurrence": {
      "type": "Recurrence",
      "recurrence": {
         "frequency": "Week",
         "interval": 1,
         "schedule": {
            "hours": [
               10,
               12,
               14
            ],
            "minutes": [
               30
            ],
            "weekDays": [
               "Monday"
            ]
         },
         "startTime": "2017-09-07T14:00:00Z",
         "timeZone": "Pacific Standard Time"
      }
   }
}
```

## <a name="next-steps"></a>Próximas etapas

* [Fluxos de trabalho de pausar com ações atrasar](../connectors/connectors-native-delay.md)
* [Conectores para aplicativos lógicos](../connectors/apis-list.md)
