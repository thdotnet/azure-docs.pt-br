---
title: 'Início Rápido: Azure Time Series Insights Explorer | Microsoft Docs'
description: Este guia de início rápido mostra como começar a usar o Azure Time Series Insights Explorer no navegador da Web para visualizar grandes volumes de dados de IoT. Conheça os principais recursos em um ambiente de demonstração.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc seodec18
ms.date: 04/22/2019
ms.openlocfilehash: be663520c9c1afd11ace57cd9dcb8ffb8219b86b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64696951"
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Início Rápido: Explorar o Azure Time Series Insights

Este gerenciador de início rápido é fornecido para mostrar como começar a usar o Azure Time Series Insights em um ambiente de demonstração gratuita. Com ele, você aprenderá a usar o navegador da Web para visualizar grandes volumes de dados de IoT e conhecerá os principais recursos atualmente em disponibilidade geral.

O Azure Time Series Insights é um serviço totalmente gerenciado de análise, armazenamento e visualização que facilita a exploração e análise de bilhões de eventos de IoT simultaneamente. Ele fornece uma exibição global dos dados, o que permite validar rapidamente sua solução de IoT e evitar um tempo de inatividade dispendioso de dispositivos críticos. Com o Azure Time Series Insights, descubra tendências ocultas, detecte anomalias e realize análises de causa raiz quase em tempo real.

Para obter flexibilidade adicional, o Azure Time Series Insights podem ser adicionado a um aplicativo já existente por meio de suas poderosas [APIs REST](./time-series-insights-update-tsq.md) e [SDK do cliente](./tutorial-create-tsi-sample-spa.md). As APIs permitem armazenar, consultar e consumir dados de série temporal em um aplicativo cliente de sua escolha. Você também pode optar por usar o SDK do cliente para adicionar componentes de interface do usuário ao seu aplicativo.

O gerenciador do Time Series Insights é um tour guiado que apresenta a disponibilidade geral atual.

## <a name="prepare-the-demo-environment"></a>Preparar o ambiente de demonstração

1. Crie uma [conta gratuita do Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio), caso uma ainda não tenha sido criada.

1. No seu navegador, vá até a [demonstração de disponibilidade geral](https://insights.timeseries.azure.com/demo).

1. Se solicitado, entre no gerenciador do Time Series Insights usando suas credenciais da conta do Azure.

1. A página de tour rápido do Time Series Insights será exibida. Clique em **Avançar** para iniciar o tour rápido.

   [![Clique em Avançar](media/quickstart/quickstart1.png)](media/quickstart/quickstart1.png#lightbox)

## <a name="explore-the-demo-environment"></a>Explorar o ambiente de demonstração

1. O **Painel de seleção de tempo** será exibido. Use esse painel para selecionar um período a ser visualizado.

   [![Painel de seleção de tempo](media/quickstart/quickstart2.png)](media/quickstart/quickstart2.png#lightbox)

1. Clique e arraste na região e, em seguida, clique no botão **Pesquisar**.

   [![Selecionar um período](media/quickstart/quickstart3.png)](media/quickstart/quickstart3.png#lightbox)

   O Time Series Insights exibe uma visualização de gráfico para o período especificado. Realize várias ações internas com o gráfico de linhas, como filtragem, anexação, classificação e empilhamento.

   Para retornar ao **Painel de seleção de tempo**, clique na seta para baixo, conforme mostrado:

   [![Gráfico](media/quickstart/quickstart4.png)](media/quickstart/quickstart4.png#lightbox)

1. Clique em **Adicionar** no **Painel de termos** para adicionar um novo termo de pesquisa.

   [![Adicionar item](media/quickstart/quickstart5.png)](media/quickstart/quickstart5.png#lightbox)

1. No gráfico, você pode selecionar uma região, clicar com o botão direito do mouse na região e selecionar **Explorar Eventos**.

   [![Explorar Eventos](media/quickstart/quickstart6.png)](media/quickstart/quickstart6.png#lightbox)

   Uma grade dos dados brutos é exibida na região que está sendo explorada:

   [![Exibição de grade](media/quickstart/quickstart7.png)](media/quickstart/quickstart7.png#lightbox)

## <a name="select-and-filter-data"></a>Seleciona e filtra conjuntos de dados

1. Edite os termos para alterar os valores no gráfico e adicione outro termo para fazer uma correlação cruzada entre diferentes tipos de valores:

   [![Adicionar um termo](media/quickstart/quickstart8.png)](media/quickstart/quickstart8.png#lightbox)

1. Insira um termo de filtro na caixa **Filtrar série...** para a filtragem improvisada da série. Para o guia de início rápido, insira **Station5** para fazer uma correlação cruzada entre a temperatura e a pressão dessa estação.

   [![Filtrar série](media/quickstart/quickstart9.png)](media/quickstart/quickstart9.png#lightbox)

Depois de concluir o guia de início rápido, faça experiências com o conjunto de dados de exemplo para criar visualizações diferentes.

## <a name="next-steps"></a>Próximas etapas

Você está pronto para criar seu próprio ambiente do Time Series Insights:
> [!div class="nextstepaction"]
> [Planejar o ambiente do Time Series Insights](time-series-insights-environment-planning.md)
