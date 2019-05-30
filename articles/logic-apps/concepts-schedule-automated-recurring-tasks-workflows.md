---
title: Agendar tarefas recorrentes e fluxos de trabalho em aplicativos lógicos do Azure
description: Uma visão geral sobre como agendar tarefas automatizadas recorrentes, processos e fluxos de trabalho com aplicativos lógicos do Azure
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: deli, klam, LADocs
ms.topic: conceptual
ms.date: 05/25/2019
ms.openlocfilehash: 7f15dc5b28a44dc8405e2f0524913e6012ebe380
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66356042"
---
# <a name="schedule-and-run-recurring-automated-tasks-processes-and-workflows-with-azure-logic-apps"></a>Agendar e executar tarefas automatizadas recorrentes, processos e fluxos de trabalho com aplicativos lógicos do Azure

Os aplicativos lógicos ajudam você a criar e executar tarefas recorrentes automatizadas e processos em um agendamento. Criando um fluxo de trabalho de aplicativo lógico que começa com um internos gatilho recorrência ou de janela deslizante, que são gatilhos do tipo de agenda, você pode executar as tarefas imediatamente, em um momento posterior, ou em um intervalo recorrente. Você pode chamar serviços dentro e fora do Azure, como pontos de extremidade HTTP ou HTTPS, postar mensagens em serviços do Azure como armazenamento do Azure e do barramento de serviço do Azure ou obter arquivos carregados para um compartilhamento de arquivos. Gatilho de recorrência, você também pode configurar agendamentos complexos e avançados de recorrências para execução de tarefas. Para obter mais informações sobre os gatilhos internos de agendamento e ações, consulte [agendamento e execução recorrente, automatizado, tarefas e fluxos de trabalho com aplicativos lógicos do Azure](../logic-apps/concepts-schedule-automated-recurring-tasks-workflows.md).

Aqui estão alguns exemplos que mostram os tipos de tarefas que você pode executar:

* Obter dados internos, como a execução de um procedimento armazenado SQL todos os dias.

* Obter dados externos, como relatórios pull meteorológicos do NOAA a cada 15 minutos.

* Envie dados de relatório, como email um resumo de todos os pedidos de mais de uma quantidade específica na última semana.

* Processar dados, como compactar hoje carregou imagens de cada dia da semana durante o horário de pico.

* Limpe dados, como excluir todos os tweets com mais de três meses.

* Arquivar dados, como faturas por push para um serviço de backup à 1h de todos os dias nos próximos nove meses.

Você também pode usar as ações internas de agendamento para pausar o fluxo de trabalho antes da próxima ação é executada, por exemplo:

* Aguardar até um dia útil para enviar uma atualização de status por email.

* Atrasar o fluxo de trabalho até que uma chamada HTTP tenha tempo para ser concluída antes da retomada e da recuperação do resultado.

Este artigo descreve os recursos para os gatilhos internos de agendamento e as ações.

## <a name="schedule-triggers"></a>Gatilhos de agendamento

Você pode iniciar seu fluxo de trabalho do aplicativo lógico usando o gatilho de recorrência ou um gatilho de janela deslizante, que não estão associados a qualquer sistema, por exemplo, o Outlook do Office 365 ou o SQL Server ou serviço específico. Esses disparadores iniciarem e executar seu fluxo de trabalho com base em sua recorrência especificados em que você selecione o intervalo e a frequência, como o número de segundos, minutos e horas para os gatilhos, ou o número de dias, semanas ou meses para o gatilho de recorrência. Você também pode definir a data de início e hora, bem como o fuso horário. Cada vez que um gatilho é acionado, os aplicativos lógicos cria e executa uma nova instância de fluxo de trabalho para seu aplicativo lógico.

Aqui estão as diferenças entre esses gatilhos:

* **Recorrência**: Executa o fluxo de trabalho em intervalos regulares com base em seu agendamento especificado. Se as recorrências estiverem faltando, o gatilho de recorrência não processa as recorrências perdidas, mas reinicia recorrências com o próximo intervalo agendado. Você pode especificar uma data de início e hora, bem como o fuso horário. Se você selecionar "Dia", você pode especificar as horas do dia e os minutos da hora, por exemplo, todos os dias às 2 minutos e 30. Se você selecionar "Semana", você também pode selecionar os dias da semana como quarta-feira e sábado. Para obter mais informações, consulte [Create, agendamento e execução de tarefas recorrentes e fluxos de trabalho com o gatilho de recorrência](../connectors/connectors-native-recurrence.md).

* **Janela deslizante**: Executa o fluxo de trabalho em intervalos regulares que lidam com dados em partes contínuas. Se as recorrências estiverem faltando, o gatilho de janela deslizante volta e processa as recorrências perdidas. Você pode especificar uma data de início e hora, fuso horário e uma duração para atrasar a cada recorrência no fluxo de trabalho. Esse gatilho não tem opções para especificar os dias, semanas e meses, horas do dia, os minutos da hora e dias da semana. Para obter mais informações, consulte [Create, agendamento e execução de tarefas recorrentes e fluxos de trabalho com o gatilho de janela deslizante](../connectors/connectors-native-sliding-window.md).

## <a name="schedule-actions"></a>Ações de agendamento

Após qualquer ação em seu fluxo de trabalho do aplicativo lógico, você pode usar as ações de atraso e o atraso até que façam com que seu fluxo de trabalho espera antes da próxima execução da ação.

* **atraso**: Espera para executar a próxima ação para o número especificado de unidades de tempo como segundos, minutos, horas, dias, semanas ou meses. Para obter mais informações, consulte [atrasar a próxima ação em fluxos de trabalho](../connectors/connectors-native-delay.md).

* **Atrasar até**: Espera para executar a próxima ação até que a data e hora especificadas. Para obter mais informações, consulte [atrasar a próxima ação em fluxos de trabalho](../connectors/connectors-native-delay.md).

## <a name="patterns-for-start-date-and-time"></a>Padrões para a data e hora de início

<a name="start-time"></a>

Aqui estão alguns padrões que mostram como você pode controlar a recorrência com a data de início e hora, e como os serviços de aplicativos lógicos executa essas recorrências:

| Hora de início | Recorrência sem agendamento | Recorrência com agendamento (modo de gatilho de recorrência) |
|------------|-----------------------------|----------------------------------------------------|
| {none} | Executa a primeira carga de trabalho imediatamente. <p>Executa futuras cargas de trabalho com base na última hora de execução. | Executa a primeira carga de trabalho imediatamente. <p>Executa futuras cargas de trabalho com base no agendamento especificado. |
| Hora de início no passado | **Recorrência** gatilho: Calcula os tempos de execução com base nos horários de início especificados e descarta as horas de execução anteriores. Executa a primeira carga de trabalho na próxima hora de execução no futuro. <p>Executa futuras cargas de trabalho com base em cálculos da última hora de execução. <p><p>**Janela deslizante** gatilho: Calcula os tempos de execução com base na hora de início especificada e executar segue últimos tempos. <p>Executa futuras cargas de trabalho com base em cálculos da hora de início especificada. <p><p>Para obter mais explicações, consulte o exemplo após essa tabela. | Executa a primeira carga de trabalho *não antes* da hora de início, com base no agendamento calculado com base na hora de início. <p>Executa futuras cargas de trabalho com base no agendamento especificado. <p>**Observação:** se você especificar uma recorrência com um agendamento, mas não especificar horas ou minutos para esse agendamento, as horas de execução futuras serão calculadas usando as horas ou os minutos, respectivamente, da primeira hora de execução. |
| Hora de início no presente ou no futuro | Executa a primeira carga de trabalho na hora de início especificada. <p>Executa futuras cargas de trabalho com base em cálculos da última hora de execução. | Executa a primeira carga de trabalho *não antes* da hora de início, com base no agendamento calculado com base na hora de início. <p>Executa futuras cargas de trabalho com base no agendamento especificado. <p>**Observação:** se você especificar uma recorrência com um agendamento, mas não especificar horas ou minutos para esse agendamento, as horas de execução futuras serão calculadas usando as horas ou os minutos, respectivamente, da primeira hora de execução. |
||||

*Exemplo de última hora de início e recorrência, mas sem agendamento*

Suponha que a data e hora atual seja 8 de setembro de 2017 às 1:00. Você especifica a data de início e a hora como 7 de setembro de 2017 às 2:00, que está no passado e uma recorrência que é executado a cada 2 dias.

| Hora de início | Hora atual | Recorrência | Agenda |
|------------|--------------|------------|----------|
| 2017-09-**07**T14:00:00Z <br>(2017-09 -**07** às 2:00) | 2017-09-**08**T13:00:00Z <br>(2017-09 -**08** às 1:00) | A cada 2 dias | {none} |
|||||

Para o gatilho de recorrência, os aplicativos lógicos calcula mecanismo executado vezes com base na hora de início, descarta após tempos de execução, usa o futuro da próxima hora de início para a primeira execução e calcula o futuro é executada com base na última hora de execução.

Veja a aparência dessa recorrência:

| Hora de início | Primeira hora de execução | Horas de execução futuras |
|------------|----------------|------------------|
| 2017-09-**07** às 14h | 2017-09-**09** às 14h | 2017-09-**11** às 14h </br>2017-09-**13** às 14h </br>2017-09-**15** às 14h </br>e assim por diante... |
||||

Assim, não importa quanto no passado você especifica a hora de início, por exemplo, 2017-09 -**05** às 2:00 ou 2017-09 -**01** às 2:00, sua primeira execução sempre usa a hora de início no futuro próximo.

Para o gatilho de janela deslizante, os aplicativos lógicos calcula mecanismo executado vezes com base na hora de início, segue últimos tempos de execução, usa a hora de início para a primeira execução e calcula as execuções futuras com base na hora de início.

Veja a aparência dessa recorrência:

| Hora de início | Primeira hora de execução | Horas de execução futuras |
|------------|----------------|------------------|
| 2017-09-**07** às 14h | 2017-09-**07** às 14h | 2017-09-**09** às 14h </br>2017-09-**11** às 14h </br>2017-09-**13** às 14h </br>2017-09-**15** às 14h </br>e assim por diante... |
||||

Assim, não importa quanto no passado você especifica a hora de início, por exemplo, 2017-09 -**05** às 2:00 ou 2017-09 -**01** às 2:00, sua primeira execução sempre usa especificado hora de início.

<a name="example-recurrences"></a>

## <a name="example-recurrences"></a>Recorrências de exemplo

Aqui estão vários recorrências de exemplo que podem ser definidas para os gatilhos que dão suporte as opções:

| Gatilho | Recorrência | Interval | Frequência | Hora de início | Nestes dias | A estas horas | A estes minutos | Observação |
|---------|------------|----------|-----------|------------|---------------|----------------|------------------|------|
| Recorrência, <br>Janela Deslizante | Executar a cada 15 minutos (sem data e hora de início) | 15 | Minuto | {none} | {não disponível} | {none} | {none} | Esse agendamento será iniciado imediatamente e, em seguida, calculará as recorrências futuras com base na última hora de execução. |
| Recorrência, <br>Janela Deslizante | Executar a cada 15 minutos (com data e hora de início) | 15 | Minuto | *startDate*T*startTime*Z | {não disponível} | {none} | {none} | Esse agendamento não se inicia *antes* da data e hora de início especificada e, em seguida, calcula as recorrências futuras com base na última hora de execução. |
| Recorrência, <br>Janela Deslizante | Executar a cada hora, na hora cheia (com data e hora de início) | 1 | Hora | *startDate*Thh:00:00Z | {não disponível} | {none} | {none} | Esse agendamento não se inicia *antes* da data e hora de início especificadas. As recorrências futuras executado a cada hora em que a marca de minuto "00", que é calculado a partir da hora de início. <p>Se a frequência for "Semana" ou "Mês", esse agendamento será executado, respectivamente, somente um dia por semana ou um dia por mês. |
| Recorrência, <br>Janela Deslizante | Executar a cada hora, todos os dias (sem data e hora de início) | 1 | Hora | {none} | {não disponível} | {none} | {none} | Esse agendamento será iniciado imediatamente e calculará as recorrências futuras com base na última hora de execução. <p>Se a frequência for "Semana" ou "Mês", esse agendamento será executado, respectivamente, somente um dia por semana ou um dia por mês. |
| Recorrência, <br>Janela Deslizante | Executar a cada hora, todos os dias (com data e hora de início) | 1 | Hora | *startDate*T*startTime*Z | {não disponível} | {none} | {none} | Esse agendamento não se inicia *antes* da data e hora de início especificada e, em seguida, calcula as recorrências futuras com base na última hora de execução. <p>Se a frequência for "Semana" ou "Mês", esse agendamento será executado, respectivamente, somente um dia por semana ou um dia por mês. |
| Recorrência, <br>Janela Deslizante | Executar a cada 15 minutos após a hora cheia, a cada hora (com data e hora de início) | 1 | Hora | *startDate*T00:15:00Z | {não disponível} | {none} | {none} | Esse agendamento não se inicia *antes* da data e hora de início especificadas. As recorrências futuras executadas na marca minuto "15", que é calculada desde o início de tempo, isso no 00H15, 1H15, 2H15 e assim por diante. |
| Recorrência | Executar a cada 15 minutos após a hora cheia, a cada hora (sem data e hora de início) | 1 | Dia | {none} | {não disponível} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 15 | Esse agendamento é executado às 00h15, 1h15, 2h15 e assim por diante. Além disso, esse agendamento é equivalente a uma frequência de "Hora" e uma hora de início com "15" minutos. |
| Recorrência | Execute a cada 15 minutos em que as marcas de minutos especificadas (sem data de início e hora). | 1 | Dia | {none} | {não disponível} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Esse agendamento não se inicia até a próxima marca de 15 minutos especificada. |
| Recorrência | Executar às 8h, todos os dias (sem data e hora de início) | 1 | Dia | {none} | {não disponível} | 8 | {none} | Esse agendamento é executado às 8h, todos os dias, com base no agendamento especificado. |
| Recorrência | Executar às 8h, todos os dias (com data e hora de início) | 1 | Dia | *startDate*T08:00:00Z | {não disponível} | {none} | {none} | Esse agendamento é executado às 8h, todos os dias, com base na hora de início especificada. | 
| Recorrência | Executar às 8h30, todos os dias (sem data e hora de início) | 1 | Dia | {none} | {não disponível} | 8 | 30 | Esse agendamento é executado às 8h30, todos os dias, com base no agendamento especificado. |
| Recorrência | Executar às 8h30, todos os dias (com data e hora de início) | 1 | Dia | *startDate*T08:30:00Z | {não disponível} | {none} | {none} | Esse agendamento se inicia na data de início especificada, às 8h30. |
| Recorrência | Executar às 8h30 e 16h30 todos os dias | 1 | Dia | {none} | {não disponível} | 8, 16 | 30 | |
| Recorrência | Executar às 8h30, 8h45, 16h30 e 16h45 todos os dias | 1 | Dia | {none} | {não disponível} | 8, 16 | 30, 45 | |
| Recorrência | Executar todos os sábados às 17h (sem data e hora de início) | 1 | Semana | {none} | "Sábado" | 17 | 00 | Esse agendamento é executado todos os sábados às 17h. |
| Recorrência | Executar todos os sábados às 17h (com data e hora de início) | 1 | Semana | *startDate*T17:00:00Z | "Sábado" | {none} | {none} | Esse agendamento não se inicia *antes* da data e hora de início especificadas, nesse caso, 9 de setembro de 2017 às 17h. As recorrências futuras serão executadas todos os sábados às 17h. |
| Recorrência | Executar todas as terças e quintas-feiras às 17h | 1 | Semana | {none} | "Terça-feira", "Quinta-feira" | 17 | {none} | Esse agendamento é executado todas as terças-feiras e quintas-feiras às 17h. |
| Recorrência | Executado a cada hora durante o horário comercial | 1 | Semana | {none} | Selecione todos os dias, exceto sábado e domingo. | Selecione as horas do dia que você deseja. | Selecione os minutos da hora que você deseja. | Por exemplo, se o horário de trabalho é de 8h às 17h, selecione "8, 9, 10, 11, 12, 13, 14, 15, 16, 17" como as horas do dia. <p>Se o horário de trabalho é das 8h30 às 17h30, selecione as horas anteriores do dia e selecione também "30" como os minutos da hora. |
| Recorrência | Executar uma vez por dia aos finais de semana | 1 | Semana | {none} | "Sábado", "Domingo" | Selecione as horas do dia que você deseja. | Selecione os minutos da hora conforme apropriado. | Esse agendamento é executado aos sábados e domingos no agendamento especificado. |
| Recorrência | Executar a cada 15 minutos, quinzenalmente, somente às segundas-feiras | 2 | Semana | {none} | "Segunda-feira" | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | 0, 15, 30, 45 | Esse agendamento é executado em todas as outras segundas-feiras em cada marca de 15 minutos. |
| Recorrência | Executar todos os meses | 1 | Mês | *startDate*T*startTime*Z | {não disponível} | {não disponível} | {não disponível} | Esse agendamento não se inicia *antes* de data e hora de início e calculará as recorrências futuras na data de início e hora especificado. Se você não especificar uma data e hora de início, esse agendamento usará a data e a hora de criação. |
| Recorrência | Executar a cada hora durante um dia por mês | 1 | Mês | {consulte a observação} | {não disponível} | 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23 | {consulte a observação} | Se você não especificar uma data e hora de início, esse agendamento usará a data e a hora de criação. Para controlar os minutos do agendamento da recorrência, especifique os minutos da hora, uma hora de início ou use o tempo de criação. Por exemplo, se a hora de início ou hora de criação for 8h25, esse agendamento será executado às 8h25, 9h25, 10h25 e assim por diante. |
|||||||||

<a name="run-once"></a>

## <a name="run-one-time-only"></a>Execute apenas uma vez

Se você quiser executar seu aplicativo lógico somente ao mesmo tempo no futuro, você pode usar o **Agendador: executar trabalhos uma vez**. Depois de criar um novo aplicativo lógico, mas antes de abrir o Designer de aplicativos lógicos, sob o **modelos** seção, da **categoria** lista, selecione **agenda**e, em seguida, selecione Este modelo:

![Selecione "Agendador: executar trabalhos uma vez"](./media/concepts-schedule-automated-recurring-tasks-workflows/choose-run-once-template.png)

Ou, se você puder iniciar o aplicativo lógico com o **quando uma solicitação HTTP é recebida - solicitação** disparar e passar a hora de início como um parâmetro para o gatilho. Para a primeira ação, use o **atrasar até – agendar** ação e fornecer a hora de quando a próxima ação começa a ser executado.

## <a name="next-steps"></a>Próximas etapas

* [Criar, agendar e executar tarefas recorrentes e fluxos de trabalho com o gatilho de recorrência](../connectors/connectors-native-recurrence.md)
* [Criar, agendar e executar tarefas recorrentes e fluxos de trabalho com o gatilho de janela deslizante](../connectors/connectors-native-sliding-window.md)
* [Fluxos de trabalho de pausar com ações atrasar](../connectors/connectors-native-delay.md)