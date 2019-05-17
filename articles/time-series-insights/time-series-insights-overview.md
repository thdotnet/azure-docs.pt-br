---
title: 'Visão geral: O que é o Azure Time Series Insights? | Microsoft Docs'
description: Introdução à Azure Time Series Insights, um novo serviço para soluções de IoT e de análises de dados de séries temporais.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: overview
ms.date: 04/26/2019
ms.custom: seodec18
ms.openlocfilehash: 8c7a24c4c4e071271d16de036ef6896e9d172913
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65406323"
---
# <a name="what-is-azure-time-series-insights"></a>O que é o Azure Time Series Insights?

O Azure Time Series Insights (TSI) foi criado para armazenar, visualizar e consultar grandes quantidades de dados de série temporal, como os gerados por dispositivos IoT.  Se você deseja armazenar, gerenciar, consultar ou visualizar os dados de série temporal na nuvem, Time Series Insights é provavelmente certa para você.  

![Fluxograma do Time Series Insights Explorer](media/overview/time-series-insights-flowchart.png)

O Azure Time Series Insights tem quatro tarefas principais:

1. Ele é totalmente integrado com gateways de nuvem, como o Hub IoT e Hubs de Eventos do Azure. Ele se conecta facilmente a essas fontes de eventos e analisa JSON de mensagens e estruturas que têm dados em colunas e linhas limpas. Ele une metadados à telemetria e indexa seus dados em um repositório de colunas.
1. O TSI gerencia o armazenamento de seus dados. Para garantir que os dados estejam sempre facilmente acessíveis, ele armazena seus dados na memória e SSD por até 400 dias. Interativamente, você pode consultar bilhões de eventos em segundos – sob demanda.
1. O TSI fornece visualização pronta para uso por meio do gerenciador do TSI.  
1. O TSI fornece um serviço de consulta no gerenciador do TSI e por meio de APIs que são fáceis de integrar para inserir dados de série temporal em aplicativos personalizados.  

Se você estiver compilando um aplicativo para consumo interno ou para a utilização de clientes externos, o TSI pode ser usado como um back-end para indexação, armazenamento e agregação de dados de série temporal. Você pode criar uma visualização personalizada e uma experiência do usuário no topo usando o [SDK Cliente](tutorial-explore-js-client-lib.md). O TSI também está equipado com várias [APIs de Consulta](how-to-shape-query-json.md) para ativar esses cenários personalizados.  

Dados de série temporal representam como um processo ou um ativo é alterado ao longo do tempo. Os dados de série temporal são, portanto, indexados por carimbos de data/hora e o tempo é o eixo mais significativo ao longo do qual esses dados são organizados. Normalmente, os dados de série temporal chegam em ordem sequencial e, em geral, são tratados como uma inserção em vez de uma atualização para seu banco de dados como resultado.

Em grandes volumes, armazenamento, indexação, consultas, análises e visualizações de dados de série temporal podem ser desafiadoras.
No entanto, o Azure Time Series Insights captura e armazena cada novo evento como uma linha e a alteração é medida com eficiência ao longo do tempo, permitindo que você pesquise para trás para distinguir insights do passado e prever mudanças futuras.

## <a name="video"></a>Vídeo

### <a name="learn-more-about-azure-time-series-insights-the-cloud-based-iot-analytics-platformbr"></a>Saiba mais sobre o Azure Time Series Insights, a plataforma de análise de IoT baseada em nuvem.</br>

[![VÍDEO](https://img.youtube.com/vi/GaARrFfjoss/0.jpg)](https://www.youtube.com/watch?v=GaARrFfjoss)

## <a name="primary-scenarios"></a>Principais cenários

- Armazenando dados de série temporal de maneira escalonável.  
  - Essencialmente, Time Series Insights tem um banco de dados criado com dados de série temporal em mente.  Como ele é totalmente gerenciado e escalonável, Time Series Insights manipula o trabalho de armazenamento e gerenciamento de eventos.

- Próximo a exploração de dados em tempo real.  
  - Time Series Insights fornece um explorador que visualiza todos os dados em um ambiente de dados de streaming.  Logo após se conectar a uma fonte de evento, dados de evento podem ser exibidos, explorado e consultados em Time Series Insights.  Os dados são úteis para validar se um dispositivo estiver emitindo dados conforme esperado e monitorar um ativo de IoT para integridade, a produtividade e a eficiência geral.  

- Acelerar a detecção de anomalias e análise de causas raiz.
  - Time Series Insights tem ferramentas como padrões e as exibições de perspectiva para conduzir e salvar a análise da causa raiz de várias etapas.  Além disso, Time Series Insights funciona em conjunto com alertas de serviços, como o Azure Stream Analytics, então anomalias detectadas e alertas podem ser exibidos quase em tempo real no Gerenciador de Time Series Insights.  

- Uma exibição global dos dados de série temporal de streaming de dados de locais diferentes para comparação de vários asset/site.
  - Você pode se conectar a várias fontes de evento em um ambiente de Time Series Insights.  Isso significa que dados de streaming em vários locais diferentes podem ser exibidos juntos quase em tempo real.  Os usuários podem aproveitar essa visibilidade para compartilhar dados com líderes de negócios e para habilitar a colaboração melhor com os especialistas de domínio que podem aplicar seu conhecimento para ajudar a resolver problemas, aplicar as práticas recomendadas e compartilhar conhecimentos.

- Compilando um aplicativo cliente sobre Time Series Insights. 
  - Time Series Insights expõe as APIs REST de consulta, permitindo que você compile aplicativos que usam dados de série temporal.

## <a name="capabilities"></a>Funcionalidades

- **Fácil de começar:** O Azure Time Series Insights não exige nenhuma preparação de dados inicial. Conecte-se a milhões de eventos em seu Hub IoT do Azure ou Hub de Eventos em minutos. Uma vez conectado, visualize e interaja com os dados do sensor para validar rapidamente suas soluções de IoT. Você pode interagir com os dados sem gravar código.
Não há nenhuma nova linguagem a aprender. O Time Series Insights fornece uma superfície de consulta de texto livre granular para usuários avançados, assim como a exploração do tipo apontar e clicar, para todos.

- **Insights quase em tempo real:** O Time Series Insights pode incluir milhões de eventos de sensor por dia, com latência de um minuto. O Time Series Insights ajuda você a realizar análises profundas em seus dados de sensor ajudando-o a identificar tendências e anomalias, realize facilmente análises de causas raiz e evitar um tempo de inatividade dispendioso. Ao habilitar a correlação cruzada entre dados de histórico e em tempo real, o Time Series Insights ajuda você a desbloquear tendências ocultas nos dados.

- **Crie soluções personalizadas:** Insira dados do Azure Time Series Insights nos seus aplicativos existentes ou crie novas soluções personalizadas com as APIs REST do Time Series Insights. Criar e compartilhar exibições personalizadas que você pode compartilhar para que outras pessoas explorem suas descobertas.

- **Escalabilidade:** O Time Series Insights foi projetado para dar suporte à IoT em escala. É possível inserir de 1 milhão a 100 milhões de eventos por dia, com um período de tempo de retenção padrão de 31 dias. Você pode visualizar e analisar os fluxos de dados ativos quase em tempo real, juntamente com dados de histórico. Mais adiante, taxas de entrada e retenção aumentarão para acomodar uma escala empresarial em constante evolução.

## <a name="getting-started"></a>Introdução

Guia de Introdução leva menos de 5 minutos.

1. Para começar, forneça um novo ambiente de Time Series Insights no portal do Azure.
1. Conecte-se a uma fonte de evento como um Hub IoT do Azure ou Hub de eventos.  
1. Carregar dados de referência (não é um serviço adicional).
1. Ver seus dados em minutos com o Gerenciador de Time Series Insights.

## <a name="time-series-insights-explorer"></a>Time Series Insights Explorer

Este diagrama mostra um exemplo de dados do Time Series Insights exibidos por meio do gerenciador: ![Time Series Insights Explorer](media/time-series-insights-explorer/explorer4.png)

## <a name="next-steps"></a>Próximas etapas

- Explore o [ambiente de demonstração gratuito](./time-series-quickstart.md) de disponibilidade geral do Azure Time Series Insights.

- Saiba mais sobre [como planejar seu ambiente do Time Series Insights](time-series-insights-environment-planning.md).
