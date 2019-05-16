---
title: 'Tutorial: Explorar a biblioteca de clientes JavaScript do Azure Time Series Insights | Microsoft Docs'
description: Saiba mais sobre a biblioteca de cliente JavaScript do Azure Time Series Insights e seu respectivo modelo de programação.
author: ashannon7
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 05/06/2019
ms.author: anshan
ms.custom: seodec18
ms.openlocfilehash: 8cb1d06872f7eae04bac934220da9d58982d0f4b
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233742"
---
# <a name="tutorial-explore-the-azure-time-series-insights-javascript-client-library"></a>Tutorial: Explorar a biblioteca de cliente JavaScript do Azure Time Series Insights

Para ajudar a consulta de desenvolvedores Web e visualizar dados armazenados no Time Series Insights (TSI), foi desenvolvida a biblioteca de cliente do TSI baseada em JavaScript D3. Este tutorial orientará você pela biblioteca do Cliente do TSI e o modelo de programação usando um aplicativo de exemplo hospedado.

O tutorial fornece detalhes sobre como trabalhar com a biblioteca, acessar dados do TSI e usar controles de gráfico para renderizar e visualizar dados. Você também aprenderá a fazer experiências com diferentes tipos de grafos para visualizar dados. Após a conclusão do tutorial, você estará apto a usar a biblioteca de clientes para incorporar recursos do TSI em seu próprio aplicativo Web.

Especificamente, você aprenderá sobre:

> [!div class="checklist"]
> * O aplicativo de exemplo do TSI.
> * A biblioteca de cliente JavaScript do TSI.
> * Como o aplicativo de exemplo usa a biblioteca para visualizar dados do TSI.

> [!NOTE]
> * O tutorial usa uma [demonstração da Web hospedada e gratuita do Time Series Insights](https://insights.timeseries.azure.com/clientsample).
> * Os arquivos de origem do aplicativo de exemplo do Time Series Insights são fornecidos no [repositório de exemplo do GitHub](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).
> * Leia a [documentação de referência do cliente do TSI](https://github.com/microsoft/tsiclient/blob/master/docs/API.md).

## <a name="video"></a>Vídeo

### <a name="in-this-video-we-introduce-the-open-source-time-series-insights-javascript-sdkbr"></a>Neste vídeo, apresentamos o SDK do JavaScript do Time Series Insights de software livre.</br>

> [!VIDEO https://www.youtube.com/embed/X8sSm7Pl9aA]

## <a name="prerequisites"></a>Pré-requisitos

Este tutorial usa o recurso **Ferramentas para Desenvolvedores** do navegador. Os navegadores da Web modernos ([Microsoft Edge](/microsoft-edge/devtools-guide), [Chrome](https://developers.google.com/web/tools/chrome-devtools/), [Firefox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), [Safari](https://developer.apple.com/safari/tools/) e outros) normalmente fornecem acesso à **Exibição do Inspetor da Web** por meio da tecla de atalho `F12`. Caso contrário, ela pode ser acessada clicando com o botão direito do mouse em uma página da Web e selecionando **Inspecionar Elemento**.

## <a name="time-series-insights-sample-application"></a>Aplicativo de exemplo do Time Series Insights

Em todo este tutorial, um aplicativo de exemplo hospedado e gratuito do Time Series Insights é usado para explorar o código-fonte por trás do aplicativo e da biblioteca de clientes JavaScript do TSI. Por meio dele, você aprenderá a interagir com o TSI em JavaScript e visualizar dados por meio de gráficos e grafos.

1. Navegue até o [Aplicativo de exemplo Time Series Insights](https://insights.timeseries.azure.com/clientsample). O seguinte prompt de entrada será exibido:

   [![Prompt de entrada de exemplo de Cliente do TSI](media/tutorial-explore-js-client-lib/tcs-sign-in.png)](media/tutorial-explore-js-client-lib/tcs-sign-in.png#lightbox)

1. Selecione **Fazer logon** e insira ou selecione suas credenciais. Use uma conta de organização empresarial (Azure Active Directory) ou uma conta pessoal (conta Microsoft ou MSA).

   [![Solicitação de credenciais de exemplo de Cliente do TSI](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png#lightbox)

1. Depois de entrar, você verá uma página com vários tipos de gráficos populados com os dados do TSI. Sua conta de usuário e a opção **Logoff** são visíveis no canto superior direito:

   [![Página principal de exemplo de Cliente do TSI após entrada](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png#lightbox)

### <a name="page-source-and-structure"></a>Estrutura e fonte da página

Primeiro, vamos exibir o [código-fonte HTML e JavaScript](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html) da página da Web renderizada:

1. Abra **Ferramentas para Desenvolvedores** em seu navegador. Inspecione os elementos HTML que compõem a página atual (também conhecidos como árvore DOM ou de HTML).

1. Expanda os elementos `<head>` e `<body>` e observe as seguintes seções:

   * No elemento `<head>`, você encontrará metadados e dependências da página que permitem a execução do aplicativo:
     * Um elemento `<script>` que é usado para referenciar a biblioteca de autenticação do Azure Active Directory **adal.min.js** (também conhecido como ADAL). ADAL é uma biblioteca do JavaScript que fornece autenticação OAuth 2.0 (entrada) e aquisição de token para acessar APIs.
     * Vários `<link>` elementos para folhas de estilos (também conhecidos como CSS) como **sampleStyles.css** e **tsiclient.css**. As folhas de estilo são usadas para controlar os detalhes de estilo visual da página, como cores, fontes, espaçamento e assim por diante.
     * Um `<script>` elemento que é usado para fazer referência à biblioteca JavaScript do Cliente TSI **tsiclient.js**. A biblioteca é usada pela página para chamar as APIs do serviço TSI e renderizar controles de gráfico na página.

     >[!NOTE]
     > * O código-fonte para a biblioteca JavaScript ADAL está disponível no [repositório azure-activedirectory-library-for-js repository](https://github.com/AzureAD/azure-activedirectory-library-for-js).
     > * O código-fonte para a biblioteca TSI cliente JavaScript está disponível no repositório [tsiclient](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).

   * No elemento `<body>`, você encontrará elementos `<div>`, que ajudam a definir o layout de itens na página, e outro elemento `<script>`:
     * O primeiro elemento `<div>` especifica a caixa de diálogo **Entrar** (`id="loginModal"`).
     * O segundo elemento `<div>` atua como um pai para:
       * Um cabeçalho do elemento `<div>`, usado para mensagens de status e informações de entrada na parte superior da página (`class="header"`).
       * Um elemento `<div>` para o restante dos elementos do corpo da página, incluindo todos os gráficos (`class="chartsWrapper"`).
       * Uma seção `<script>` que contém todos os elementos do JavaScript usados para controlar a página.

   [![Exemplo de Cliente TSI com as Ferramentas para Desenvolvedores](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

1. Expanda o elemento `<div class="chartsWrapper">` e você descobrirá mais elementos `<div>` filho. Esses elementos são usados para posicionar cada exemplo de controle de gráfico. Observe que há vários pares de elementos `<div>`, um para cada exemplo de gráfico:

   * O primeiro elemento (`class="rowOfCardsTitle"`) contém um título descritivo para resumir o que ilustra os gráficos. Por exemplo: `Static Line Charts With Full-Size Legends.`
   * O segundo elemento (`class="rowOfCards"`) é um pai, que contém os elementos filho `<div>` que posicionam o controle de gráfico real em uma linha.

   [![Elementos div do corpo](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png#lightbox)

1. Agora, expanda o elemento `<script type="text/javascript">` logo abaixo do elemento `<div class="chartsWrapper">`. Observe o início da seção do JavaScript no nível de página usado para manipular toda a lógica da página (autenticação, chamada de APIs de serviço do TSI, renderização de controles de gráfico, entre outros):

   [![Script do corpo](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png#lightbox)

## <a name="tsi-javascript-client-library-concepts"></a>Conceitos de biblioteca de cliente JavaScript do TSI

A biblioteca do Cliente do TSI (**tsclient.js**) fornece abstrações para duas funcionalidades importantes do JavaScript:

* **Métodos de wrapper para chamar as APIs de consulta TSI**: APIs REST que permitem consultar dados TSI usando expressões de agregação. Os métodos estão organizados no namespace `TsiClient.Server` da biblioteca.

* **Métodos para criar e popular os vários tipos de controles de criação de gráficos**: métodos que são usados para renderizar o TSI agregam dados em uma página da Web. Os métodos estão organizados no namespace `TsiClient.UX` da biblioteca.

Por meio dessas simplificações, os desenvolvedores podem criar componentes de gráfico e grafo de interface do usuário ativados com os dados do TSI com mais facilidade.

### <a name="authentication"></a>Authentication

O [aplicativo de exemplo do Time Series Insights](https://insights.timeseries.azure.com/clientsample) é um aplicativo de página única com suporte à autenticação de usuário OAuth 2.0 da ADAL:

1. Ao usar a ADAL para autenticação, o aplicativo cliente precisa ser registrado no Azure Active Directory. Na verdade, o aplicativo de página única está registrado para usar o [fluxo de concessão implícita do OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-implicit-grant-flow).
1. Dessa forma, o aplicativo precisa especificar algumas das propriedades de registro em tempo de execução. Elas incluem o GUID do cliente (`clientId`) e o URI de redirecionamento (`postLogoutRedirectUri`).
1. Posteriormente, o aplicativo solicitará um **token de acesso** do Azure Active Directory. O token de acesso é emitido para um conjunto finito de permissões para um identificador específico do serviço/API (`https://api.timeseries.azure.com`). As permissões de token são emitidas em nome do usuário conectado. O identificador do serviço/API é outra propriedade contida no registro do Azure Active Directory do aplicativo.
1. Depois que a ADAL retorna o token de acesso para o aplicativo, ele é passado como um **token de portador** ao acessar as APIs de serviço do TSI.

   [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-204&highlight=3-7,34-37)]

> [!TIP]
> Para saber mais sobre as bibliotecas ADAL com suporte da Microsoft, confira a [documentação de referência da ADAL](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries#microsoft-supported-client-libraries).

### <a name="control-identification"></a>Identificação de controle

No exemplo fornecido, os elementos `<div>` são organizados no elemento `<body>` pai para fornecer um layout adequado para todos os controles de gráfico renderizados na página.

Cada elemento `<div>` especifica as propriedades para o posicionamento e os atributos visuais dos controles de gráfico. As propriedades `id` do elemento HTML servem como identificadores exclusivos a serem associados a controles específicos para renderização e atualização dos dados visualizados.

### <a name="aggregate-expressions"></a>Expressões de agregação

As APIs da biblioteca do Cliente do TSI usam expressões de agregação:

* Uma expressão de agregação oferece a capacidade de construir um ou mais **termos de pesquisa**.

* As APIs do Cliente foram projetadas para fornecer uma funcionalidade semelhante a outro aplicativo de demonstração fornecido (o [Gerenciador do Time Series Insights](https://insights.timeseries.azure.com/demo)), que usa o intervalo de pesquisa, o predicado WHERE, medidas e o valor dividir por.

* A maioria das APIs da biblioteca de clientes usa uma matriz de expressões de agregação que são usadas pelo serviço para criar uma consulta de dados do TSI.

### <a name="call-pattern"></a>Padrão de chamada

O preenchimento e a renderização de controles de gráfico seguem um padrão geral. Esse padrão geral pode ser observado em todo o aplicativo de exemplo e o ajudará no uso da biblioteca do Cliente:

1. Declare uma `array` para manter uma ou mais expressões de agregação TSI:

   ```javascript
   var aes =  [];
   ```

1. Crie *1* a *n* objetos de expressão de agregação. Em seguida, adicione-os à matriz de expressão de agregação:

   ```javascript
   var ae = new tsiClient.ux.aggregateExpression(predicateObject, measureObject, measureTypes, searchSpan, splitByObject, color, alias, contextMenuActions);
   aes.push(ae);
   ```

   **parâmetros de aggregateExpression**

   | Parâmetro | DESCRIÇÃO | Exemplo |
   | --------- | ----------- | ------- |
   | `predicateObject` | A expressão de filtragem de dados. |`{predicateString: "Factory = 'Factory3'"}` |
   | `measureObject`   | O nome da propriedade da medida que está sendo usada. | `{property: 'Temperature', type: "Double"}` |
   | `measureTypes`    | As agregações desejadas da propriedade de medida. | `['avg', 'min']` |
   | `searchSpan`      | O tamanho de intervalo e a duração da expressão de agregação. | `{from: startDate, to: endDate, bucketSize: '2m'}` |
   | `splitByObject`   | A propriedade de cadeia de caracteres que você deseja dividir (opcional – pode ser nulo). | `{property: 'Station', type: 'String'}` |
   | `color`         | A cor dos objetos que você deseja renderizar. | `'pink'` |
   | `alias`           | Um nome fácil para a expressão de agregação. | `'Factory3Temperature'` |
   | `contextMenuActions` | Uma matriz de ações a ser associada aos objetos de série de tempo em uma visualização (opcional). | Para obter mais informações, confira a seção [Menus de contexto pop-up](#pop-up-context-menus) |

1. Chame uma consulta TSI usando as APIs `TsiClient.Server` para solicitar os dados de agregação:

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```

   **parâmetros getAggregates**

   | Parâmetro | DESCRIÇÃO | Exemplo |
   | --------- | ----------- | ------- |
   | `token`     | O token de acesso para a API TSI. |  `authContext.getTsiToken()` Para mais informações, consulte a [seção de autenticação.](#authentication) |
   | `envFQDN`   | O nome de domínio totalmente qualificado (FQDN) para o ambiente TSI. | No portal do Azure, por exemplo: `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com`. |
   | `aeTsxArray` | Uma matriz de expressões de consulta TSI. | Use a variável `aes`, conforme descrito anteriormente: `aes.map(function(ae){return ae.toTsx()}`. |

1. Transforme o resultado compactado que é retornado da consulta TSI em JSON para visualização:

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

1. Crie um controle de gráfico usando as APIs `TsiClient.UX` e associe-o a um dos elementos `<div>` na página:

   ```javascript
   var barChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

1. Preencha o controle de gráfico com os objetos de dados JSON renderize o controle na página:

   ```javascript
   barChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="rendering-controls"></a>Controles de renderização

A biblioteca do Cliente do TSI fornece oito controles de análise exclusivos prontos para uso:

* **gráfico de linhas**
* **gráfico de pizza**
* **gráfico de barras**
* **heatmap**
* **controles de hierarquia**
* **grade acessível**
* **linhas do tempo de evento discreto**
* **linhas do tempo de transição de estado**

### <a name="line-bar-pie-chart-examples"></a>Exemplos de gráfico de linha, barras e de pizza

Examine o código de demonstração usado para renderizar alguns dos controles de gráfico padrão. Observe o modelo de programação e os padrões para criação desses controles. Especificamente, examine a seção de HTML no comentário `// Example 3/4/5`, que renderiza os controles com os valores de `id` HTML `chart3`, `chart4` e `chart5`.

Lembre-se da **etapa 3** da [seção Origem e estrutura da página](#page-source-and-structure), na qual os controles de gráfico são organizados em linhas na página, cada qual com uma linha de título descritivo. Neste exemplo, os três gráficos são populados no elemento `<div>` sob o título `"Multiple Chart Types From the Same Data"` e são vinculados aos três elementos `<div>` abaixo do título:

[!code-html[code-sample1-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=59-73&highlight=1,5,9,13)]

A seção de código JavaScript a seguir usa o padrões descritos anteriormente: crie expressões de agregação TSI, use-os para consultar dados TSI e renderize os três gráficos. Observe os três tipos usados no namespace `tsiClient.ux`: `LineChart`, `BarChart` e `PieChart`, para criar e renderizar os respectivos gráficos. Observe também que todos os três gráficos podem usar os mesmos dados de expressão de agregação `transformedResult`:

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=241-262&highlight=13-14,16-17,19-20)]

Os três gráficos da seguinte maneira aparecem quando renderizados:

[![Vários tipos de gráfico dos mesmos dados](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox)

## <a name="advanced-features"></a>Recursos avançados

A biblioteca do Cliente do TSI tem vários recursos adicionais, que você pode usar para implementar visualizações de dados de forma criativa.

### <a name="states-and-events"></a>Estados e eventos

Uma funcionalidade avançada é a capacidade de adicionar transições de estado e eventos discretos aos gráficos. Esse recurso é útil para realce de incidentes, alertas e criação de opções de estado (opções ativado/desativado, por exemplo).

Examine o código em torno do comentário `// Example 10`. O código renderiza um controle de linha sob o título `"Line Charts with Multiple Series Types"` e o associa ao elemento `<div>` com o valor de `id` HTML igual a `chart10`.

1. Primeiro, uma estrutura chamada `events4` é definida para conter os elementos de alteração de estado a serem acompanhados. A estrutura contém:

   * Uma chave de cadeia de caracteres chamada `Component States`.
   * Uma matriz de objetos de valor que representam os estados. Cada objeto inclui:
     * Uma chave de cadeia de caracteres que contém um carimbo de data/hora ISO JavaScript.
     * Uma matriz que contém as características do estado: uma cor e uma descrição.

2. Em seguida, a estrutura `events5` é definida para `Incidents`, que contém uma matriz dos elementos de evento a serem acompanhados. A estrutura de matriz tem a mesma forma que a descrita por `events4`.

3. Por fim, o gráfico de linha é renderizado, passando as duas estruturas com os parâmetros de opções de gráfico: `events:` e `states:`. Observe os outros parâmetros de opção para especificar um `tooltip:`, `theme:`, ou `grid:`.

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/tutorial/index.html?range=337-389&highlight=5,26,51)]

Visualmente, os marcadores de losango/janelas pop-up, que são usados para indicar incidentes e as barras coloridas/janelas pop-up ao longo da escala de tempo, indicam as mudanças de estado:

[![Gráficos de linha com vários tipos de série](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox)

### <a name="pop-up-context-menus"></a>Menus de contexto pop-up

Outra funcionalidade avançada é a capacidade de criar menus de contexto personalizados (menus pop-up de atalho). Menus de contexto personalizados são úteis para habilitar ações e próximas etapas lógicas dentro do escopo de seu aplicativo.

Examine o código em torno do comentário `// Example 13/14/15`. Esse código inicialmente renderiza um gráfico de linhas sob o título `"Line Chart with Context Menu to Create Pie/Bar Chart"` e o gráfico é vinculado ao elemento `<div>` com o valor de `id` HTML igual a `chart13`.

Usando os menus de contexto, o gráfico de linhas fornece a capacidade de criar dinamicamente um gráfico de piza e barra associado aos elementos `<div>` com IDs `chart14` e `chart15`. Além disso, gráficos de barras e pizza também usam menus de contexto para habilitar seus próprios recursos: a capacidade de copiar dados de gráfico de pizza para barra e imprimir os dados de gráfico de barras para a janela de console do navegador, respectivamente.

1. Primeiro, uma série de ações personalizadas são definidas. Cada ação contém uma matriz com um ou mais elementos. Cada elemento define um item de menu de contexto único:

   * `barChartActions`: esta ação define o menu de contexto para o gráfico de pizza, que contém um elemento para definir um único item:
     * `name`: O texto que é usado para o item de menu: “Parâmetros de impressão para o console”.
     * `action`: a ação associada ao item de menu. A ação é sempre uma função anônima que usa três argumentos com base na expressão de agregação usada para criar o gráfico. Nesse caso, os argumentos são gravados na janela de console do navegador:
       * `ae`: a matriz de expressão de agregação.
       * `splitBy`: O valor `splitBy`.
       * `timestamp`: o carimbo de data/hora.

   * `pieChartActions`: esta ação define o menu de contexto do gráfico de barras, que contém um elemento para definir um único item. A forma e o esquema são os mesmos que os anteriores `barChartActions`, mas observe que a função `action` é drasticamente diferente, pois cria e processa o gráfico de barras. Observe também que o argumento `ae` é usado para especificar a matriz de expressão de agregação, que é passada no tempo de execução quando o item de menu se abre. A função também define a `ae.contextMenu` propriedade com o `barChartActions` menu de contexto.
   * `contextMenuActions`: esta ação define o menu de contexto do gráfico de linhas, que contém três elementos para definir três itens de menu. A forma e o esquema para cada elemento é o mesmo dos elementos anteriormente descritos. Assim como o elemento `barChartActions`, o primeiro item grava os argumentos das três funções na janela do navegador do console. Semelhante ao elemento `pieChartActions`, os dois itens instanciam e renderizam os gráficos de pizza e de barras, respectivamente. Os dois itens também definem suas `ae.contextMenu` propriedades com o `pieChartActions` e `barChartActions` menus de contexto, respectivamente.

2. Em seguida, duas expressões de agregação são enviadas por push para a matriz de expressão de agregação `aes`, especificando a matriz `contextMenuActions` para cada item. As expressões são usadas com o controle de gráfico de linha.

3. Por fim, o gráfico de linhas é inicialmente renderizado, do qual a pizza e um gráfico de barras podem ser renderizados em tempo de execução.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/tutorial/index.html?range=461-540&highlight=7,16,29,61-64,78)]

A captura de tela mostra os gráficos, com seus respectivos menus de contexto pop-up. Os gráficos de pizza e de barras foram criados dinamicamente usando as opções de menu de contexto de gráfico de linha.

[![Gráfico de linha com o Menu de contexto para criar o gráfico de pizza/barra](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>Pincéis

Os pincéis são usados para examinar um intervalo de tempo para definir ações, como ampliar e explorar.

O código usado para ilustrar pincéis é mostrado no exemplo anterior "Gráfico de linhas com o menu de contexto para criar gráfico de pizza/barras", que descreve os Menus de contexto pop-up.

1. As ações de pincel são semelhantes a um menu de contexto, que definem uma série de ações personalizadas para o pincel. Cada ação contém uma matriz com um ou mais elementos. Cada elemento define um item de menu de contexto único:
   * `name`: O texto que é usado para o item de menu: “Parâmetros de impressão para o console”.
   * `action`: a ação associada ao item de menu, que é sempre uma função anônima que usa dois argumentos. Nesse caso, os argumentos são gravados na janela de console do navegador:
      * `fromTime`: O carimbo de data/hora `from` da seleção do pincel.
      * `toTime`: O carimbo de data/hora `to` da seleção do pincel.

2. Ações de pincel são adicionadas como outra propriedade de opção de gráfico. Observe a propriedade `brushContextMenuActions: brushActions`, que é transferida para a chamada `linechart.Render`.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/tutorial/index.html?range=526-540&highlight=1,13)]

[![Gráfico de linhas com um menu de contexto para criar um gráfico de pizza/barras usando pincéis](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png#lightbox)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

> [!div class="checklist"]
> * Entre e explore o aplicativo de exemplo TSI e sua origem.
> * Use APIs na biblioteca de clientes JavaScript do TSI.
> * Use o JavaScript para criar e preencher os controles de gráfico com dados TSI.

Como visto, o aplicativo de exemplo TSI usa um conjunto de dados de demonstração. Para saber como você pode criar seu próprio ambiente TSI e o conjunto de dados, vá para o seguinte artigo:

> [!div class="nextstepaction"]
> [Tutorial: criar um ambiente do Azure Time Series Insights](tutorial-create-populate-tsi-environment.md)

Ou exiba os arquivos de origem do aplicativo de exemplo TSI:

> [!div class="nextstepaction"]
> [Repositório do aplicativo de exemplo TSI](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial)

Leia a documentação de referência da API do cliente do TSI:

> [!div class="nextstepaction"]
> [Documentação de referência da API do TSI](https://github.com/microsoft/tsiclient/blob/master/docs/API.md)
