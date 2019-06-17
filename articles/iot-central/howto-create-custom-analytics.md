---
title: Estender o Azure IoT Central com análises personalizadas | Microsoft Docs
description: Como desenvolvedor de soluções, configure um aplicativo de Central da IoT para fazer análises personalizadas e visualizações. Esta solução usa o Azure Databricks.
author: dominicbetts
ms.author: dobett
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
ms.custom: mvc
manager: philmea
ms.openlocfilehash: e039e2b8d9c183b5bfee1bee47e4addc4c873bf7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66743434"
---
# <a name="extend-azure-iot-central-with-custom-analytics"></a>Estender o Azure IoT Central com análises personalizadas

Este guia de instruções mostra a você como desenvolvedor de soluções, como estender o aplicativo de Central da IoT com análises personalizadas e visualizações. O exemplo usa uma [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/) espaço de trabalho para analisar o fluxo de telemetria de IoT Central e gerar visualizações, como [caixa plotagens](https://wikipedia.org/wiki/Box_plot).

Este guia de instruções mostra como estender o IoT Central além do que ele já pode fazer com o [ferramentas de análise interna](howto-create-analytics.md).

Este guia de instruções, você aprenderá como:

* Stream de telemetria de um aplicativo de IoT Central usando *exportação contínua de dados*.
* Crie um ambiente do Azure Databricks para analisar e criar gráficos de telemetria do dispositivo.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir as etapas neste guia de instruções, é necessário ter uma assinatura ativa do Azure.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

### <a name="iot-central-application"></a>Aplicativo de IoT Central

Criar um aplicativo de IoT Central dos [do Azure IoT Central - meus aplicativos](https://aka.ms/iotcentral) página com as seguintes configurações:

| Configuração | Value |
| ------- | ----- |
| Plano de Pagamento | Pré-paga |
| Modelo de aplicativo | Exemplo Contoso |
| Nome do aplicativo | Aceite o padrão ou escolha seu próprio nome |
| URL | Aceite o padrão ou escolha seu próprio prefixo de URL exclusivo |
| Diretório | Seu locatário do Azure Active Directory |
| Assinatura do Azure | Sua assinatura do Azure |
| Região | Leste dos EUA |

Os exemplos e capturas de tela neste artigo usam o **Leste dos EUA** região. Escolha um local perto de você e certifique-se de que criar todos os seus recursos na mesma região.

### <a name="resource-group"></a>Grupo de recursos

Use o [portal do Azure para criar um grupo de recursos](https://portal.azure.com/#create/Microsoft.ResourceGroup) chamado **IoTCentralAnalysis** para conter outros recursos que você criou. Crie os recursos do Azure no mesmo local do seu aplicativo IoT Central.

### <a name="event-hubs-namespace"></a>Namespace do Hubs de Eventos

Use o [portal do Azure para criar um namespace de Hubs de eventos](https://portal.azure.com/#create/Microsoft.EventHub) com as seguintes configurações:

| Configuração | Valor |
| ------- | ----- |
| NOME    | Escolha seu nome de namespace |
| Tipo de preço | Basic |
| Assinatura | Sua assinatura |
| Grupo de recursos | IoTCentralAnalysis |
| Local padrão | Leste dos EUA |
| Unidades de produtividade | 1 |

### <a name="azure-databricks-workspace"></a>Espaço de trabalho do Databricks do Azure

Use o [portal do Azure para criar um serviço do Azure Databricks](https://portal.azure.com/#create/Microsoft.Databricks) com as seguintes configurações:

| Configuração | Value |
| ------- | ----- |
| Nome do workspace    | Escolha seu nome de espaço de trabalho |
| Assinatura | Sua assinatura |
| Grupo de recursos | IoTCentralAnalysis |
| Local padrão | Leste dos EUA |
| Camada de preços | Standard |

Quando você tiver criado os recursos necessários, sua **IoTCentralAnalysis** grupo de recursos é semelhante a captura de tela a seguir:

![Grupo de recursos de análise de IoT Central](media/howto-create-custom-analytics/resource-group.png)

## <a name="create-an-event-hub"></a>Criar um Hub de Evento

Você pode configurar um aplicativo de IoT Central a exportação contínua de telemetria para um hub de eventos. Nesta seção, você deve criar um hub de eventos para receber telemetria de seu aplicativo IoT Central. O hub de eventos fornece a telemetria para seu trabalho de Stream Analytics para processamento.

1. No portal do Azure, navegue até seu namespace de Hubs de eventos e selecione **+ Hub de eventos**.
1. Nomeie seu hub de eventos **centralexport**e selecione **criar**.
1. Na lista de hubs de eventos em seu namespace, selecione **centralexport**. Em seguida, escolha **políticas de acesso compartilhado**.
1. Selecione **+ adicionar**. Criar uma diretiva denominada **escutar** com o **escutar** de declaração.
1. Quando a política estiver pronta, selecione-o na lista e, em seguida, copie o **chave primária da cadeia de Conexão** valor.
1. Anote essa cadeia de caracteres de conexão, usá-lo mais tarde quando você configura seu notebook do Databricks para ler do hub de eventos.

Seu namespace de Hubs de eventos é semelhante a captura de tela a seguir:

![Namespace do Hubs de Eventos](media/howto-create-custom-analytics/event-hubs-namespace.png)

## <a name="configure-export-in-iot-central"></a>Configurar a exportação no IoT Central

Navegue até a [aplicativo IoT Central](https://aka.ms/iotcentral) criados a partir do modelo de Contoso. Nesta seção, você deve configurar o aplicativo para transmitir a telemetria de seus dispositivos simulados para seu hub de eventos. Para configurar a exportação:

1. Navegue até a **exportação contínua de dados** página, selecione **+ novo**e então **Hubs de eventos**.
1. Use as configurações a seguir para configurar a exportação e, em seguida, selecione **salvar**:

    | Configuração | Value |
    | ------- | ----- |
    | Nome para exibição | Exportar para Hubs de eventos |
    | Enabled | Por |
    | Namespace do Hubs de Eventos | O nome do namespace de Hubs de eventos |
    | Hub de Eventos | centralexport |
    | Medidas | Por |
    | Dispositivos | Desativar |
    | Modelos de Dispositivo | Desativar |

![Configuração de exportação contínua de dados](media/howto-create-custom-analytics/cde-configuration.png)

Aguarde até que seja o status de exportação **executando** antes de continuar.

## <a name="configure-databricks-workspace"></a>Configurar o espaço de trabalho do Databricks

No portal do Azure, navegue até o serviço do Azure Databricks e selecione **inicializar espaço de trabalho**. Uma nova guia é aberta no seu navegador e entrará no seu espaço de trabalho.

### <a name="create-a-cluster"></a>Criar um cluster

Sobre o **Azure Databricks** página, na lista de tarefas comuns, selecionadas **novo Cluster**.

Use as informações na tabela a seguir para criar o cluster:

| Configuração | Valor |
| ------- | ----- |
| Nome do cluster | centralanalysis |
| Modo de cluster | Standard |
| Versão de tempo de execução do Databricks | 5.3 (Scala 2.11, Spark 2.4.0) |
| Versão do Python | 3 |
| Habilitar o dimensionamento automático | Não |
| Terminar após minutos de inatividade | 30 |
| Tipo de trabalho | Standard_DS3_v2 |
| Trabalhos | 1 |
| Tipo de driver | Mesmo que o trabalho |

Criar um cluster pode levar vários minutos, aguarde a criação do cluster concluir antes de continuar.

### <a name="install-libraries"></a>Instalar bibliotecas

Sobre o **Clusters** página, aguarde até que o estado do cluster está **executando**.

As etapas a seguir mostram como importar a biblioteca seu exemplo precisa para o cluster:

1. No **Clusters** página, aguarde até que o estado da **centralanalysis** cluster interativo é **executando**.

1. Selecione o cluster e, em seguida, escolha o **bibliotecas** guia.

1. Sobre o **bibliotecas** guia, escolha **instalar novos**.

1. Sobre o **instalar biblioteca** , escolha **Maven** como a fonte de biblioteca.

1. No **coordena** caixa de texto, digite o seguinte valor: `com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.10`

1. Escolher **instalar** para instalar a biblioteca no cluster.

1. O status de biblioteca agora é **instalado**:

    ![Biblioteca instalada](media/howto-create-custom-analytics/cluster-libraries.png)

### <a name="import-a-databricks-notebook"></a>Importar um notebook do Databricks

Use as etapas a seguir para importar um notebook do Databricks que contém o código do Python para analisar e visualizar sua telemetria de IoT Central:

1. Navegue até a **espaço de trabalho** página em seu ambiente do Databricks. Selecione o menu suspenso ao lado do nome da sua conta e, em seguida, escolha **importação**.

1. Escolha esta opção para importar de uma URL e digite o seguinte endereço: [https://github.com/Azure-Samples/iot-central-docs-samples/blob/master/databricks/IoT%20Central%20Analysis.dbc?raw=true](https://github.com/Azure-Samples/iot-central-docs-samples/blob/master/databricks/IoT%20Central%20Analysis.dbc?raw=true)

1. Para importar o bloco de anotações, escolha **importação**.

1. Selecione o **espaço de trabalho** para exibir o bloco de anotações importado:

    ![Importado do bloco de anotações](media/howto-create-custom-analytics/import-notebook.png)

1. Edite o código na primeira célula do Python para adicionar a cadeia de conexão de Hubs de eventos que você salvou anteriormente:

    ```python
    from pyspark.sql.functions import *
    from pyspark.sql.types import *

    ###### Event Hub Connection strings ######
    telementryEventHubConfig = {
      'eventhubs.connectionString' : '{your Event Hubs connection string}'
    }
    ```

## <a name="run-analysis"></a>Executar a análise

Para executar a análise, você deve anexar o bloco de anotações para o cluster:

1. Selecione **desanexado** e, em seguida, selecione o **centralanalysis** cluster.
1. Se o cluster não está em execução, inicie-o.
1. Para iniciar o bloco de notas, selecione o botão de execução.

Você verá um erro na última célula. Nesse caso, verifique as células anteriores estão em execução, aguarde um minuto para alguns dados a serem gravados no armazenamento e, em seguida, execute novamente a última célula.

### <a name="view-smoothed-data"></a>Exibir dados suaves

No bloco de anotações, role para baixo até a célula 14 para ver um gráfico da umidade média sem interrupção por tipo de dispositivo. Essa plotagem atualiza continuamente conforme chega de transmissão de telemetria:

![Suavizadas plotagem de telemetria](media/howto-create-custom-analytics/telemetry-plot.png)

Você pode redimensionar o gráfico no bloco de anotações.

### <a name="view-box-plots"></a>Gráficos de caixa de exibição

No bloco de anotações, role para baixo até a célula 20 para ver os [caixa plotagens](https://en.wikipedia.org/wiki/Box_plot). Os gráficos de caixa baseiam-se em dados estáticos para atualizá-los, você deve executar novamente a célula:

![Gráficos de caixa](media/howto-create-custom-analytics/box-plots.png)

Você pode redimensionar os gráficos no bloco de anotações.

## <a name="tidy-up"></a>Limpar

Para limpar após estas instruções e evitar custos desnecessários, exclua o **IoTCentralAnalysis** grupo de recursos no portal do Azure.

Você pode excluir o aplicativo IoT Central a **gerenciamento** página dentro do aplicativo.

## <a name="next-steps"></a>Próximas etapas

Neste guia de instruções, você aprendeu como:

* Stream de telemetria de um aplicativo de IoT Central usando *exportação contínua de dados*.
* Crie um ambiente do Azure Databricks para analisar e plotar os dados de telemetria.

Agora que você sabe como criar análises personalizadas, a próxima etapa sugerida é saber como [visualizar e analisar os dados do Azure IoT Central em um dashboard do Power BI](howto-connect-powerbi.md).
