---
title: Visualizar dados no gerenciador da versão prévia do Azure Time Series Insights | Microsoft Docs
description: Este artigo descreve os vários recursos e opções disponíveis no aplicativo Web do Time Series Insights Explorer.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 05/03/2019
ms.custom: seodec18
ms.openlocfilehash: c4f3053063ce33d2777387da2c53effd61b05f1a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66399869"
---
# <a name="visualize-data-in-the-explorer-preview"></a>Visualizar os dados na atualização do gerenciador

Este documento descreve a interface do usuário e recursos da experiência do usuário e interface de visualização de Insights do Azure tempo série [aplicativo web de demonstração](https://insights.timeseries.azure.com/preview/demo). Especificamente, ele aborda o layout do exemplo hospedado, opções de personalização da interface e navegação por meio de demonstração fornecida.

## <a name="prerequisites"></a>Pré-requisitos

Para começar com o Gerenciador de visualização de Insights de série de tempo do Azure, faça o seguinte:

* Criar um ambiente do Time Series Insights. Para saber mais sobre o provisionamento de uma instância, tente as [versão prévia do Azure tempo série Insights](./time-series-insights-update-create-environment.md) tutorial.
* [Fornecer acesso a dados](./time-series-insights-data-access.md) no ambiente do Time Series Insights que você criou para a conta. Você pode fornecer acesso a outras pessoas, bem como para si mesmo.
* Adicione uma origem do evento no ambiente do Time Series Insights para enviar dados por push para o ambiente:
  * Saiba mais [como se conectar a um hub de eventos](./time-series-insights-how-to-add-an-event-source-eventhub.md).
  * Saiba mais [como se conectar a um hub IoT](./time-series-insights-how-to-add-an-event-source-iothub.md).

## <a name="learn-about-the-preview-explorer"></a>Saiba mais sobre o Gerenciador de visualização

O Gerenciador da Versão prévia do Azure Time Series Insights consiste dos seguintes elementos:

[![O modo de exibição do Explorer](media/v2-update-explorer/explorer-one.png)](media/v2-update-explorer/explorer-one.png#lightbox)

- <a href="#environment-drop-down-list">Painel de ambiente</a>: Exibe seus ambientes do Azure Time Series Insights.
- <a href="#navigation-menu">Menu de navegação</a>: Use-o para alternar entre o **Analyze** e **modelo** páginas.
- <a href="#hierarchy-tree">Árvore hierárquica</a>: Use para selecionar os elementos de modelo e dados específicos devem ser plotados.
- <a href="#preview-well">Série temporal</a>: Exibe os elementos de dados selecionado no momento em formato de tabela com codificação de cores.
- <a href="#preview-chart">Painel gráfico</a>: Exibe o gráfico de trabalho atual.
- <a href="#time-editor-panel">Linha do tempo</a>: Usá-lo para modificar o período de tempo de trabalho.
- <a href="#navigation-panel">Barra de aplicativo</a>: Contém opções de gerenciamento de usuário, como o locatário atual. Você pode usá-lo para alterar as configurações de tema e idioma.

## <a name="environment-drop-down-list"></a>Lista suspensa de ambiente

A lista suspensa de ambiente exibe todos os ambientes de Time Series Insights, que você tem acesso. A lista inclui ambientes pago conforme o uso, como a visualização de Insights de série de tempo. A lista também inclui ambientes de S1/S2, que estão disponíveis.

1. Selecione a seta suspensa ao lado de seu ambiente exibido.

   [![O painel de controle](media/v2-update-explorer/explorer-two.png)](media/v2-update-explorer/explorer-two.png#lightbox)

1. Em seguida, selecione o ambiente desejado.

## <a name="navigation-menu"></a>Menu de navegação

  [![O menu de navegação](media/v2-update-explorer/explorer-three.png)](media/v2-update-explorer/explorer-three.png#lightbox)

Use o menu de navegação para selecionar entre duas exibições:

* **Analisar**: Usá-lo para executar análises avançadas em seus dados de série de tempo modeladas ou não modelados e gráfico.
* **Modelo**: Usá-lo para enviar por push novos tipos de visualização de Insights de série de tempo, hierarquias e instâncias ao seu modelo Time Series Insights.

## <a name="hierarchy-tree"></a>árvore de hierarquia

Árvore hierárquica exibe elementos de dados selecionada que incluem modelos de dispositivos específicos e sensores em seus dispositivos.

### <a name="model-search-panel"></a>Painel de pesquisa de modelo

Você pode usar o painel de pesquisa de modelo para facilmente pesquisar e navegar em sua hierarquia de modelo de série temporal para localizar as instâncias da série de tempo específico que você deseja exibir no gráfico. Depois de selecionar as instâncias, eles são adicionados ao gráfico atual e os dados também.

  [![O painel de pesquisa de modelo](media/v2-update-explorer/explorer-four.png)](media/v2-update-explorer/explorer-four.png#lightbox)

### <a name="model-authoring"></a>Criação de modelos

O dá suporte a visualização de Insights de série de tempo do Azure completo cria, ler, atualizar e excluir (CRUD) no seu modelo de série temporal.

* **Tipo de Modelo do Time Series**: Você pode usar tipos de análise de séries temporais para definir variáveis ou fórmulas para fazer cálculos. Elas são associadas uma determinada instância do Time Series Insights. Um tipo pode ter uma ou mais variáveis.
* **Hierarquias do modelo do Time Series**: Hierarquias são organizações sistemáticas de seus dados. Hierarquias descrevem as relações entre entidades diferentes em seus dados de Azure Time Series Insights.
* **Instâncias do modelo do Time Series**: As instâncias são a série temporal em si. Na maioria dos casos, eles são os **DeviceID** ou **AssetID**, que é o identificador exclusivo do ativo no ambiente.

Para saber mais sobre o Modelo do Time Series, consulte [Times Series Models](./time-series-insights-update-tsm.md).

## <a name="preview-well"></a>Visualizar bem

O também exibe campos de instância e outros metadados associados com instâncias selecionadas do Time Series Insights. Marcando as caixas de seleção no lado direito, você pode ocultar ou exibir instâncias específicas do gráfico atual. Você também pode remover elementos de dados específicos de seus dados atuais bem selecionando o vermelho **excluir** (Lixeira) controle no lado esquerdo do elemento.

  [![A visualização bem](media/v2-update-explorer/explorer-five.png)](media/v2-update-explorer/explorer-five.png#lightbox)

Para reconfigurar o layout de seu **analisar** página do gráfico, selecione o ícone de reticências no canto superior direito:

  [![Opções de layout de telemetria](media/v2-update-explorer/explorer-six.png)](media/v2-update-explorer/explorer-six.png#lightbox)

> [!NOTE]
> Se você vir a mensagem a seguir, a instância não tem todos os dados durante o período de tempo selecionado. Para resolver o problema, aumente o período de tempo ou confirmar que a instância está enviando dados.
>
> ![Nenhuma notificação de dados](media/v2-update-explorer/explorer-seven.png)

## <a name="preview-chart"></a>Visualização do gráfico

Com o gráfico, você pode exibir instâncias de séries temporais como linhas. Você pode recolher o painel de ambiente, o modelo de dados e o painel de controle de intervalo de tempo, selecionando os controles da web para tornar o gráfico maior.

  [![Visão geral do gráfico de visualização](media/v2-update-explorer/explorer-eight.png)](media/v2-update-explorer/explorer-eight.png#lightbox)

- **Selecionar intervalo de datas**: Controla quais elementos de dados estão disponíveis para visualização.

- **Ferramenta de controle deslizante de intervalo de data interna**: Use os dois controles de ponto de extremidade, arrastando-os no período de tempo desejado.

- **Controle de recolhimento do período de tempo**: Recolhe e expande o editor de alcance do painel de tempo.

- **Controle de formato do eixo y**: Alterna entre as opções de exibição do eixo y disponíveis:

    * `Default`: Cada linha tem um eixo de y individual.
    * `Stacked`: Usá-lo para empilhar várias linhas no mesmo eixo y, com a alteração de dados do eixo y com base na linha selecionada.
    * `Shared`: Todos os dados do eixo y exibidos juntos.

- **Elemento de dados atual**: O elemento de dados selecionado no momento e seus detalhes associados.

Para detalhar ainda mais em uma fatia de dados específico, clique em um ponto de dados no gráfico atual e, em seguida, arraste na área selecionada para o ponto de extremidade de sua escolha. Clique com botão direito na área selecionada cinza e, em seguida, selecione **Zoom**, conforme mostrado nesta imagem a seguir:

  [![Zoom de gráfico de visualização](media/v2-update-explorer/explorer-nine.png)](media/v2-update-explorer/explorer-nine.png#lightbox)

Depois de executar o **Zoom** ação, você ver os dados selecionados definido. Selecione o controle de formato do eixo y para alternar entre as representações de eixo y três dos seus dados de séries Temporais.

  [![Eixo y de gráfico de visualização](media/v2-update-explorer/explorer-ten.png)](media/v2-update-explorer/explorer-ten.png#lightbox)

Aqui, você pode ver um exemplo de eixo de Y compartilhado:

  [![Visualização compartilhado eixos Y](media/v2-update-explorer/explorer-eleven.png)](media/v2-update-explorer/explorer-eleven.png#lightbox)

## <a name="time-editor-panel"></a>Painel do editor de tempo

Quando você trabalha com a versão prévia do Time Series Insights, você primeiro selecione um período de tempo. O período de tempo selecionado controla o conjunto de dados que está disponível para manipulação com os widgets de visualização de Insights de série de tempo. Os controles da web a seguir estão disponíveis na visualização de Insights de série de tempo para selecionar o período de tempo de trabalho:

  [![Painel de seleção de tempo](media/v2-update-explorer/explorer-twelve.png)](media/v2-update-explorer/explorer-twelve.png#lightbox)

1. **Ferramenta de controle deslizante de intervalo de data interna**: Use os dois controles de ponto de extremidade, arrastando-os no período de tempo desejado. Esse intervalo de datas interna é restrito pelo controle de controle deslizante de intervalo de data externa.

1. **Aumentar e diminuir os botões de intervalo de data**: Aumente ou diminua o período de tempo, selecionando um dos botões para o intervalo desejado.

1. **Controle de recolhimento do período de tempo**: Esse controle da web permite ocultar todos os controles, exceto para a ferramenta de controle deslizante de intervalo de data interna.

1. **Controle de controle deslizante de intervalo de data externa**: Use os controles de ponto de extremidade para selecionar o intervalo de datas externa, que estará disponível para o seu controle de intervalo de data interna.

1. **Tempos rápidos de intervalo de datas na lista suspensa**: Use-o para alternar rapidamente entre seleções span horário predefinido, como a última **30 minutos**, o **últimas 12 horas**, ou uma **intervalo personalizado**. A alteração desse valor também muda os intervalos de intervalo disponíveis discutidos na ferramenta de controle deslizante de tamanho do intervalo.

1. **Ferramenta de controle deslizante de tamanho do intervalo**: Usá-lo para ampliar e reduzir intervalos no mesmo período de tempo. Essa ação fornece controle mais preciso dos movimentos entre fatias de tempo grandes. Ele exibe tendências suaves até fatias tão pequenas quanto um milissegundo. Você pode usá-lo para ver recortes granulares e de alta resolução dos seus dados. O ponto de partida padrão do controle deslizante é definido como a exibição mais ideal dos dados na seleção, que balanceia a resolução, velocidade de consulta e granularidade.

1. **Controle de web para e do intervalo de datas**: Com esse controle da web, você pode selecionar facilmente os intervalos de data e hora. Você também pode usar o controle para alternar entre fusos horários diferentes. Depois de fazer as alterações para aplicar ao seu espaço de trabalho atual, selecione **salvar**.

   [![Para e do painel de seleção](media/v2-update-explorer/explorer-thirteen.png)](media/v2-update-explorer/explorer-thirteen.png#lightbox)

## <a name="navigation-panel"></a>Painel de navegação

O painel de navegação de visualização de Insights de série de tempo é exibida na parte superior do seu aplicativo de análise de séries Temporais. Ele fornece as seguintes funcionalidades.

### <a name="current-session-share-link-control"></a>Controle de link de compartilhamento de sessão atual

  [![Ícone de compartilhamento](media/v2-update-explorer/explorer-fifteen.png)](media/v2-update-explorer/explorer-fifteen.png#lightbox)

Selecione a nova **compartilhar** ícone para compartilhar um link de URL com sua equipe.

  [![Compartilhe sua URL de instância](media/v2-update-explorer/url-share.png)](media/v2-update-explorer/url-share.png#lightbox)

### <a name="tenant-section"></a>Seção de Locatário

  [![Seleção de locatário](media/v2-update-explorer/explorer-sixteen.png)](media/v2-update-explorer/explorer-sixteen.png#lightbox)

* Exibe suas informações de conta de logon atuais do Time Series Insights.
* Usá-lo para alternar entre os temas disponíveis do Time Series Insights.
* Usá-lo para exibir a versão prévia [aplicativo web de demonstração](https://insights.timeseries.azure.com/preview/demo).

### <a name="theme-selection"></a>Seleção de tema

Para selecionar um novo tema, selecione o ícone do perfil localizado no canto superior direito. Em seguida, selecione **alterar o tema**.

  [![Seleção de tema](media/v2-update-explorer/theme-selection.png)](media/v2-update-explorer/theme-selection.png#lightbox)

> [!TIP]
> Seleção de idioma também está disponível, selecionando o ícone do perfil.

O Azure Time Series Insights Preview suporta dois temas:

* **Tema claro**: O tema padrão mostrado ao longo deste documento.
* **Tema escuro**: Renderiza o explorer conforme mostrado aqui:

  [![Tema escuro selecionado](media/v2-update-explorer/explorer-seventeen.png)](media/v2-update-explorer/explorer-seventeen.png#lightbox)

## <a name="s1s2-environment-controls"></a>Controles de ambiente S1/S2

### <a name="preview-terms-panel"></a>Painel de termos de visualização

Esta seção se aplica somente a ambientes existentes do S1/S2 que tentam usar o gerenciador na interface do usuário atualizada. Você talvez queira usar o produto em disponibilidade geral e a visualização em combinação. Adicionamos algumas funcionalidades de interface do usuário existente para ao gerenciador atualizado, mas você pode obter a experiência de interface do usuário completa para o ambiente S1/S2 no gerenciador do Time Series Insights existente. 

Em vez de hierarquia, você verá o painel de termos do Time Series Insights, onde você define consultas em seu ambiente. Usá-lo para seus dados com base em um predicado de filtro.

  [![Em que o painel de consulta](media/v2-update-explorer/explorer-eighteen.png)](media/v2-update-explorer/explorer-eighteen.png#lightbox)

O painel de editor de termos de visualização do Azure Time Series Insights leva em consideração os seguintes parâmetros:

**Onde**: Usar o onde cláusula para filtrar rapidamente os eventos usando o conjunto de operandos listados na tabela a seguir. Caso você realize uma pesquisa selecionando um operando, o predicado é atualizado automaticamente com base nessa pesquisa. Os tipos de operando com suporte incluem:

| Operação | Tipos com suporte   | Observações |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` | Double, DateTime, TimeSpan | |
| `=`, `!=`, `<>` | String, Bool, Double, DateTime, TimeSpan, NULL |
| `IN` | String, Bool, Double, DateTime, TimeSpan, NULL | Todos os operandos devem ser do mesmo tipo ou ser uma constante NULL. |
| `HAS` | Cadeia de caracteres | Literais de cadeia de caracteres constante só são permitidas no lado direito. Cadeia de caracteres vazia e NULL não são permitidas. |

Para saber mais sobre as operações de consulta com suporte e tipos de dados, consulte [série de tempo de expressão (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx).

### <a name="examples-of-where-clauses"></a>Exemplos de onde as cláusulas

  [![Onde obter exemplos da cláusula](media/v2-update-explorer/explorer-nineteen.png)](media/v2-update-explorer/explorer-nineteen.png#lightbox)

**Medida**: Uma lista suspensa que exibe todas as colunas numéricas (**duplos**) você pode usar como elementos do gráfico atual.

**Dividir por**: Essa lista suspensa exibe todas as disponíveis categóricas colunas (cadeias de caracteres) em seu modelo que você pode agrupar os dados. Você pode adicionar até cinco termos a serem exibidos no mesmo eixo x. Insira os parâmetros que você deseja e, em seguida, selecione **adicionar** para adicionar um novo termo.

  [![Exibição filtrada e consultada um](media/v2-update-explorer/explorer-twenty.png)](media/v2-update-explorer/explorer-twenty.png#lightbox)

Você pode mostrar e ocultar elementos no painel de gráfico, selecionando o ícone visível, conforme mostrado na imagem a seguir. Para remover completamente a consultas, selecione o vermelho **X**.

  [![Exibição filtrada e consultada dois](media/v2-update-explorer/explorer-twenty-one.png)](media/v2-update-explorer/explorer-twenty-one.png#lightbox)

## <a name="next-steps"></a>Próximas etapas

- Saiba mais sobre [armazenamento e a entrada](./time-series-insights-update-storage-ingress.md) na visualização de Insights de série de tempo do Azure.
- Leia o documento de visualização de Insights de série de tempo sobre [modelagem de dados](./time-series-insights-update-tsm.md).
- Saiba mais [como diagnosticar e solucionar problemas de](./time-series-insights-update-how-to-troubleshoot.md) sua instância do Time Series Insights.
