---
title: Visualizar anomalias usando a detecção de lote e o Power BI
titlesuffix: Azure Cognitive Services
description: Use a API do Detector de anomalias e o Power BI para visualizar anomalias ao longo de seus dados de série temporal.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 04/30/2019
ms.author: aahi
ms.openlocfilehash: de353c946da46b2bbe4e755e21e355f6bd18725b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65827133"
---
# <a name="tutorial-visualize-anomalies-using-batch-detection-and-power-bi"></a>Tutorial: Visualizar anomalias usando a detecção de lote e o Power BI

Use este tutorial para encontrar anomalias dentro de um conjunto de dados de série temporal como um lote. Usando o Power BI desktop, você irá pegar um arquivo do Excel, preparar os dados para a API do Detector de anomalias e visualizar anomalias estatísticas ao longo dele.

Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Use o Power BI Desktop para importar e transformar um conjunto de dados de série temporal
> * Integrar o Power BI Desktop com a API do Detector de anomalias em lotes para detecção de anomalias
> * Visualize anomalias encontradas dentro de seus dados, incluindo os valores esperados e vistos e limites de detecção de anomalias.

## <a name="prerequisites"></a>Pré-requisitos

* [Microsoft Power BI Desktop](https://powerbi.microsoft.com/get-started/), disponível gratuitamente.
* Pontos de dados de série de tempo que contém um excel (. xlsx) do arquivo. Os dados de exemplo para este início rápido podem ser encontrados no [GitHub](https://go.microsoft.com/fwlink/?linkid=2090962)

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

## <a name="load-and-format-the-time-series-data"></a>Carregar e formatar os dados de série temporal

Para começar, abra o Power BI Desktop e carregar dados de série temporal que você baixou dos pré-requisitos. Este arquivo do excel contém uma série de pares de carimbo de data e valor de tempo Universal Coordenado (UTC).  

> [!NOTE]
> Power BI pode usar dados de uma ampla variedade de fontes, como arquivos. csv, bancos de dados SQL, armazenamento de BLOBs do Azure e muito mais.  

Na janela principal do Power BI Desktop, clique o **Home** faixa de opções. No **dados externos** grupo da faixa de opções, abra o **obter dados** menu suspenso e clique em **Excel**.

![Uma imagem do botão "Get Data" no Power BI](../media/tutorials/power-bi-get-data-button.png)

Depois que a caixa de diálogo for exibida, navegue até a pasta onde você baixou o arquivo. xlsx de exemplo e selecioná-lo. Após o **Navigator** caixa de diálogo for exibida, clique em **Sheet1**e então **editar**.

![Uma imagem da tela de "Navegador" do código-fonte de dados no Power BI](../media/tutorials/navigator-dialog-box.png)

Power BI converterá os carimbos de hora na primeira coluna para um `Date/Time` tipo de dados. Essas marcações de tempo devem ser convertidas em texto para que sejam enviados para a API do Detector de anomalias. Se o editor do Power Query não abrir automaticamente, clique em **editar consultas** na guia Início. 

Clique o **transformar** faixa de opções no Editor do Power Query. No **qualquer coluna** grupo, abra o **tipo de dados:** menu suspenso e selecione **texto**.

![Uma imagem da tela de "Navegador" do código-fonte de dados no Power BI](../media/tutorials/data-type-drop-down.png)

Quando você receber um aviso sobre como alterar o tipo de coluna, clique em **substituir atual**. Em seguida, clique em **fechar e aplicar** ou **Apply** no **Home** faixa de opções. 

## <a name="create-a-function-to-send-the-data-and-format-the-response"></a>Crie uma função para enviar os dados e formatar a resposta

Para formatar e enviar o arquivo de dados para a API do Detector de anomalias, você pode chamar uma consulta na tabela criada acima. No Editor do Power Query, do **página inicial** faixa de opções, abra o **nova fonte** menu suspenso e clique em **consulta em branco**.

Verifique se sua nova consulta está selecionada e clique em **Editor Avançado**. 

![Uma imagem do botão "Editor Avançado" no Power BI](../media/tutorials/advanced-editor-screen.png)

No Editor Avançado, use o seguinte trecho do Power Query M para extrair as colunas da tabela e enviá-lo para a API. Depois disso, a consulta criará uma tabela da resposta JSON e retorná-lo. Substitua os `apiKey` variável com sua chave de API do Detector de anomalias válido, e `endpoint` com seu ponto de extremidade. Depois de inserir a consulta no Editor Avançado, clique em **feito**.

```M
(table as table) => let

    apikey      = "[Placeholder: Your Anomaly Detector resource access key]",
    endpoint    = "[Placeholder: Your Anomaly Detector resource endpoint]/anomalydetector/v1.0/timeseries/entire/detect",
    inputTable = Table.TransformColumnTypes(table,{{"Timestamp", type text},{"Value", type number}}),
    jsontext    = Text.FromBinary(Json.FromValue(inputTable)),
    jsonbody    = "{ ""Granularity"": ""daily"", ""Sensitivity"": 95, ""Series"": "& jsontext &" }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Content-Type" = "application/json", #"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),

    respTable = Table.FromColumns({
                    
                     Table.Column(inputTable, "Timestamp")
                     ,Table.Column(inputTable, "Value")
                     , Record.Field(jsonresp, "IsAnomaly") as list
                     , Record.Field(jsonresp, "ExpectedValues") as list
                     , Record.Field(jsonresp, "UpperMargins")as list
                     , Record.Field(jsonresp, "LowerMargins") as list
                     , Record.Field(jsonresp, "IsPositiveAnomaly") as list
                     , Record.Field(jsonresp, "IsNegativeAnomaly") as list

                  }, {"Timestamp", "Value", "IsAnomaly", "ExpectedValues", "UpperMargin", "LowerMargin", "IsPositiveAnomaly", "IsNegativeAnomaly"}
               ),
    
    respTable1 = Table.AddColumn(respTable , "UpperMargins", (row) => row[ExpectedValues] + row[UpperMargin]),
    respTable2 = Table.AddColumn(respTable1 , "LowerMargins", (row) => row[ExpectedValues] -  row[LowerMargin]),
    respTable3 = Table.RemoveColumns(respTable2, "UpperMargin"),
    respTable4 = Table.RemoveColumns(respTable3, "LowerMargin"),

    results = Table.TransformColumnTypes(

                respTable4,
                {{"Timestamp", type datetime}, {"Value", type number}, {"IsAnomaly", type logical}, {"IsPositiveAnomaly", type logical}, {"IsNegativeAnomaly", type logical},
                 {"ExpectedValues", type number}, {"UpperMargins", type number}, {"LowerMargins", type number}}
              )

 in results
```

Invocar a consulta em sua folha de dados, selecionando `Sheet1` abaixo **Inserir parâmetro**e clique em **Invoke**. 

![Uma imagem do botão "Editor Avançado"](../media/tutorials/invoke-function-screenshot.png)

## <a name="data-source-privacy-and-authentication"></a>Autenticação e a privacidade de fonte de dados

> [!NOTE]
> Esteja ciente das políticas da sua organização para acesso e privacidade de dados. Ver [níveis de privacidade do Power BI Desktop](https://docs.microsoft.com/power-bi/desktop-privacy-levels) para obter mais informações.

Você poderá receber uma mensagem de aviso quando você tentar executar a consulta, pois ele utiliza uma fonte de dados externa. 

![Uma imagem que mostra um aviso criado pelo Power BI](../media/tutorials/blocked-function.png)

Para corrigir isso, clique em **arquivo**, e **opções e configurações**. Em seguida, clique em **opções**. Abaixo **arquivo atual**, selecione **privacidade**, e **Ignore os níveis de privacidade e potencialmente melhorar o desempenho**. 

Além disso, você pode obter uma mensagem solicitando que você especifique como você deseja se conectar à API.

![Uma imagem que mostra uma solicitação para especificar credenciais de acesso](../media/tutorials/edit-credentials-message.png)

Para corrigir isso, clique em **Editar credenciais** na mensagem. Depois que a caixa de diálogo for exibida, selecione **Anonymous** para conectar-se anonimamente para a API. E clique em **Conectar**. 

Em seguida, clique em **fechar e aplicar** na **Home** faixa de opções para aplicar as alterações.

## <a name="visualize-the-anomaly-detector-api-response"></a>Visualize a resposta da API do Detector de anomalias

Na tela principal do Power BI, comece a usar as consultas criadas acima ao visualizar os dados. Primeiro, selecione **gráfico de linhas** na **visualizações**. Em seguida, adicione o carimbo de hora da função de chamada para o gráfico de linhas **eixo**. Clique com botão direito nele e selecione **carimbo de hora**. 

![Clicando duas vezes o valor de carimbo de hora](../media/tutorials/timestamp-right-click.png)

Adicione os seguintes campos do **função invocada** para o gráfico **valores** campo. Use a captura de tela para ajudar a criar o gráfico abaixo.

    * Value
    * UpperMargins
    * LowerMargins
    * ExpectedValues

![Uma imagem da tela nova medida rápida](../media/tutorials/chart-settings.png)

Depois de adicionar os campos, clique no gráfico e redimensione-a para mostrar todos os pontos de dados. O gráfico parecerá com a captura de tela abaixo:

![Uma imagem da tela nova medida rápida](../media/tutorials/chart-visualization.png)

### <a name="display-anomaly-data-points"></a>Exibir pontos de dados de anomalias

No lado direito da janela do Power BI, abaixo a **CAMPOS** painel, clique com botão direito em **valor** sob o **consulta de função invocada**e clique em **novo rápido medida**.

![Uma imagem da tela nova medida rápida](../media/tutorials/new-quick-measure.png)

Na tela que aparece, selecione **filtrado valor** como o cálculo. Definir **valor de Base** para `Sum of Value`. Em seguida, arraste `IsAnomaly` do **função invocada** campos **filtro**. Selecione `True` do **filtro** menu suspenso.

![Uma imagem da tela nova medida rápida](../media/tutorials/new-quick-measure-2.png)

Depois de clicar em **Okey**, você terá um `Value for True` campo na parte inferior da lista de seus campos. Clique duas vezes e renomeie-o para **anomalias**. Adicioná-lo para o gráfico **valores**. Em seguida, selecione a **formato** ferramenta e defina o tipo de eixo x como **categórico**.

![Uma imagem da tela nova medida rápida](../media/tutorials/format-x-axis.png)

Aplicar cores ao gráfico clicando na **formato** ferramenta e **cores de dados**. O gráfico deve ser algo semelhante ao seguinte:

![Uma imagem da tela nova medida rápida](../media/tutorials/final-chart.png)

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
>[Detecção de anomalias de streaming com o Azure Databricks](anomaly-detection-streaming-databricks.md)
