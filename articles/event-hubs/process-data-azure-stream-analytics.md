---
title: Processar dados de eventos do Azure usando o Stream Analytics | Microsoft Docs
description: Este artigo mostra como processar dados de seu hub de eventos do Azure usando um trabalho do Azure Stream Analytics.
services: event-hubs
author: spelluru
manager: ''
ms.author: spelluru
ms.date: 07/09/2019
ms.topic: article
ms.service: event-hubs
ms.openlocfilehash: f179687b0983e145244e228a3d3b06b4eabead48
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723409"
---
# <a name="process-data-from-your-event-hub-using-azure-stream-analytics"></a>Processar dados de hub de eventos usando o Azure Stream Analytics
O serviço Azure Stream Analytics facilita ingerir, processar e analisar dados de streaming dos Hubs de eventos do Azure, permitindo insights poderosos para direcionar ações em tempo real. Essa integração permite que você crie rapidamente um pipeline de análise de caminhos recorrentes. Você pode usar o portal do Azure para visualizar os dados de entrada e escrever uma consulta do Stream Analytics. Quando sua consulta estiver pronta, você pode movê-lo em produção em apenas alguns cliques. 

## <a name="key-benefits"></a>Principais benefícios
Aqui estão os principais benefícios da integração dos Hubs de eventos do Azure e o Azure Stream Analytics: 
- **Visualizar dados** – você pode visualizar os dados de entrada de um hub de eventos no portal do Azure.
- **Testar sua consulta** – preparar uma consulta de transformação e testá-lo diretamente no portal do Azure. Para obter a sintaxe de linguagem de consulta, consulte [linguagem de consulta do Stream Analytics](/stream-analytics-query/built-in-functions-azure-stream-analytics) documentação.
- **Implantar a sua consulta para a produção** – você pode implantar a consulta para a produção, criar e iniciar um trabalho do Azure Stream Analytics.

## <a name="end-to-end-flow"></a>Fluxo de ponta a ponta

1. Entre no [Portal do Azure](https://portal.azure.com). 
1. Navegue até sua **namespace de Hubs de eventos** e, em seguida, navegue até a **hub de eventos**, que tem os dados de entrada. 
1. Selecione **processar dados** na página de hub de eventos.  

    ![Bloco de dados de processo](./media/process-data-azure-stream-analytics/process-data-tile.png)
1. Selecione **explorar** sobre o **habilitar insights em tempo real de eventos** lado a lado. 

    ![Selecionar Stream Analytics](./media/process-data-azure-stream-analytics/process-data-page-explore-stream-analytics.png)
1. Você verá uma página de consulta com valores já definidos para os seguintes campos:
    1. Sua **hub de eventos** como uma entrada para a consulta.
    1. Amostra **consulta SQL** com a instrução SELECT. 
    1. Uma **saída** alias para se referir aos resultados do teste. 

        ![Editor de consultas](./media/process-data-azure-stream-analytics/query-editor.png)
        
        > [!NOTE]
        >  Quando você usa esse recurso pela primeira vez, essa página solicita sua permissão para criar um grupo de consumidores e uma política para o hub de eventos para visualizar os dados de entrada.
1. Selecione **Create** na **visualização de entrada** painel conforme mostrado na imagem anterior. 
1. Você verá imediatamente um instantâneo dos dados de entrada mais recente nesta guia.
    - O tipo de serialização em seus dados é automaticamente detectado (CSV/JSON). Você pode alterar manualmente-lo também para JSON/CSV/AVRO.
    - Você pode visualizar os dados de entrada no formato bruto ou formato de tabela. 
    - Se seus dados não for atuais, selecione **Refresh** para ver os eventos mais recentes. 

        Aqui está um exemplo de dados na **formato de tabela**:   ![Resultados no formato de tabela](./media/process-data-azure-stream-analytics/snapshot-results.png)

        Aqui está um exemplo de dados na **formato bruto**: 

        ![Resultados no formato bruto](./media/process-data-azure-stream-analytics/snapshot-results-raw-format.png)
1. Selecione **consulta de teste** para ver o instantâneo de resultados de teste de sua consulta na **resultados de teste** guia. Você também pode baixar os resultados.

    ![Testar os resultados da consulta](./media/process-data-azure-stream-analytics/test-results.png)
1. Gravar sua própria consulta para transformar os dados. Ver [referência de linguagem de consulta do Stream Analytics](/stream-analytics-query/stream-analytics-query-language-reference).
1. Depois que você testou a consulta e você deseja movê-lo na produção, selecione **Deploy consulta**. Para implantar a consulta, crie um trabalho do Azure Stream Analytics, onde você pode definir uma saída para o seu trabalho e iniciar o trabalho. Para criar um trabalho do Stream Analytics, especifique um nome para o trabalho e selecione **criar**.

      ![Criar um trabalho do Azure Stream Analytics](./media/process-data-azure-stream-analytics/create-stream-analytics-job.png)

      > [!NOTE] 
      >  É recomendável que você crie um grupo de consumidores e uma política para cada novo trabalho do Azure Stream Analytics que você cria da página de Hubs de eventos. Grupos de consumidores permitem apenas cinco leitores simultâneos, portanto, fornecer um grupo de consumidores dedicado para cada trabalho irá evitar os erros que possam surgir exceder esse limite. Uma política dedicada permite girar sua chave ou revogar permissões sem afetar outros recursos. 
1. O trabalho do Stream Analytics agora é criado em que sua consulta é o mesmo que você testou e entrada é o hub de eventos. 

9.  Para concluir o pipeline, defina as **saída** da consulta e selecione **iniciar** para iniciar o trabalho.

    > [!NOTE]
    > Antes de iniciar o trabalho, não se esqueça de substituir o outputalias pelo nome da saída que você criou no Azure Stream Analytics.

      ![Definir a saída e iniciar o trabalho](./media/process-data-azure-stream-analytics/set-output-start-job.png)


## <a name="known-limitations"></a>Limitações conhecidas
Ao testar sua consulta, os resultados do teste levar aproximadamente 6 segundos para carregar. Estamos trabalhando para melhorar o desempenho de teste. No entanto, quando implantado em produção, o Azure Stream Analytics terão latência de subsegundos.

## <a name="streaming-units"></a>Unidades de transmissão
O Azure Stream Analytics de trabalho padrão é três unidades de streaming (SUs). Para ajustar essa configuração, selecione **dimensionamento** no menu à esquerda na **trabalho do Stream Analytics** página no portal do Azure. Para saber mais sobre unidades de streaming, consulte [compreender e ajustar as unidades de Streaming](../stream-analytics/stream-analytics-streaming-unit-consumption.md).

![Dimensionar unidades de streaming](./media/process-data-azure-stream-analytics/scale.png)

## <a name="next-steps"></a>Próximas etapas
Para saber mais sobre as consultas do Stream Analytics, consulte [linguagem de consulta do Stream Analytics](/stream-analytics-query/built-in-functions-azure-stream-analytics)