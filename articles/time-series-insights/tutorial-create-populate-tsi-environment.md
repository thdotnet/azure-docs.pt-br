---
title: 'Tutorial: Criar um ambiente de Análise de Séries Temporais do Azure | Microsoft Docs'
description: Saiba como criar um ambiente do Time Series Insights populado com os dados de dispositivos simulados.
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 04/26/2019
ms.author: dpalled
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: b8b46db043113f29f559ad44855d19f0d6ca73c3
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244153"
---
# <a name="tutorial-create-an-azure-time-series-insights-environment"></a>Tutorial: Criar um ambiente do Azure Time Series Insights

Este tutorial orientará você pelo processo de criação de um ambiente do Azure Time Series Insights populado com os dados de dispositivos simulados. Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Crie um ambiente do Time Series Insights.
> * Criar uma solução de simulação de dispositivo que contém um hub IoT.
> * Conectar o ambiente do Time Series Insights ao hub IoT.
> * Executar uma simulação de dispositivo para transmitir dados para o ambiente do Time Series Insights.
> * Verificar os dados telemétricos simulados.

## <a name="video"></a>Vídeo

### <a name="learn-how-to-use-an-azure-iot-solution-accelerator-to-generate-data-and-get-started-with-time-series-insights-br"></a>Saiba como usar um acelerador de solução de IoT do Azure para gerar dados e começar a usar o Time Series Insights. </br>

> [!VIDEO https://www.youtube.com/embed/6ehNf6AJkFo]

## <a name="prerequisites"></a>Pré-requisitos

* Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/).
* Sua conta de entrada do Azure também precisa ser um membro da função **Proprietário** da assinatura. Para obter mais informações, confira [Gerenciar o acesso usando o controle de acesso baseado em função e o portal do Azure](/azure/role-based-access-control/role-assignments-portal).

## <a name="overview"></a>Visão geral

O ambiente do Time Series Insights é o local em que os dados do dispositivo são coletados e armazenados. Depois que os dados forem armazenados, o [Gerenciador do Azure Time Series Insights](time-series-quickstart.md) e a [API de Consulta do Time Series Insights](/rest/api/time-series-insights/ga-query-api) poderão ser usados para consultar e analisar os dados. O Hub IoT do Azure é o ponto de conexão usado por todos os dispositivos (simulados ou físicos) para se conectar com segurança e transmitir dados à nuvem do Azure. A [Visão geral do Time Series Insights](time-series-insights-overview.md) indica que o Hub IoT do Azure também serve como uma origem do evento para a transmissão de dados para um ambiente do Time Series Insights. Este tutorial usa um [acelerador de solução de IoT](/azure/iot-accelerators/) para gerar e transmitir dados telemétricos de exemplo para o Hub IoT.

>[!TIP]
> Os aceleradores de solução de IoT fornecem soluções de nível empresarial pré-configuradas que você pode usar para acelerar o desenvolvimento de soluções personalizadas de IoT.

## <a name="create-an-environment"></a>Criar um ambiente

Primeiro, crie um ambiente do Time Series Insights em sua assinatura do Azure.

1. Entre no [portal do Azure](https://portal.azure.com) usando sua conta da assinatura do Azure. 
1. Selecione **+ Criar um recurso** no canto superior esquerdo. 
1. Selecione a categoria **Internet das Coisas** e depois selecione **Time Series Insights**. 

   [![Selecione o recurso de ambiente Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png#lightbox)

1. Na página **Ambiente do Time Series Insights**, preencha os parâmetros necessários.

   Parâmetro|DESCRIÇÃO
   ---|---
   **Nome do ambiente** | Escolha um nome exclusivo para o ambiente do Time Series Insights. Os nomes são usados pelo Gerenciador do Time Series Insights e pelas APIs de Consulta.
   **Assinatura** | As assinaturas são contêineres para recursos do Azure. Escolha uma assinatura para criar o ambiente do Time Series Insights.
   **Grupo de recursos** | Um grupo de recursos é um contêiner para os recursos do Azure. Escolha um grupo de recursos existente ou crie um novo para o recurso de ambiente do Time Series Insights.
   **Localidade** | Escolha uma região do data center para o seu ambiente do Time Series Insights. Para evitar custos de largura de banda adicional e latência, mantenha o ambiente do Time Series Insights na mesma região de outros recursos de IoT.
   **SKU de preço** | Escolha a taxa de transferência necessária. Para obter o custo mais baixo e a capacidade inicial, selecione `S1`.
   **Capacidade** | A capacidade é o multiplicador aplicado à taxa de ingresso, à capacidade de armazenamento e ao custo associado com a SKU selecionada. Você pode alterar a capacidade após a criação. Para obter o custo mais baixo, selecione uma capacidade igual a 1.

   Quando terminar, selecione **Criar** para iniciar o processo de provisionamento.

   [![Criar um recurso do ambiente do Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png#lightbox)

1. Verifique o painel **Notificações** para monitorar a conclusão da implantação. 

   [![Implantação do ambiente do Time Series Insights bem-sucedida](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png#lightbox)

## <a name="create-a-device-simulation"></a>Criar uma simulação de dispositivo

Em seguida, crie a solução de simulação de dispositivo, que gera dados de teste para popular o ambiente do Time Series Insights.

1. Em uma janela ou uma guia separada, acesse [azureiotsolutions.com](https://www.azureiotsolutions.com). Entre usando a mesma conta de assinatura do Azure e selecione o acelerador de **Simulação de Dispositivo**.

   [![Executar o acelerador de Simulação de Dispositivo](media/tutorial-create-populate-tsi-environment/sa-main.png)](media/tutorial-create-populate-tsi-environment/sa-main.png#lightbox)

1. Insira os parâmetros necessários na página da **solução Criar Simulação de Dispositivo**.

   Parâmetro|DESCRIÇÃO
   ---|---
   **Nome da solução** | Esse valor exclusivo é usado para criar um grupo de recursos. Os recursos do Azure listados são criados e atribuídos ao grupo de recursos.
   **Assinatura** | Especifique a mesma assinatura usada para criar o ambiente do Time Series Insights na seção anterior.
   **Região** | Especifique a mesma região usada para criar o ambiente do Time Series Insights na seção anterior.
   **Implantar recursos opcionais do Azure** | Mantenha **Hub IoT** marcado. Os dispositivos simulados o usarão para se conectar ou transmitir dados.

   Quando terminar, selecione **Criar solução** para provisionar os recursos do Azure da solução. Esse processo pode levar de 6 a 7 minutos para ser concluído.

   [![Provisionar a solução de simulação de dispositivo](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png#lightbox)

1. Após a conclusão do provisionamento, o texto acima da nova solução será alterado de **Provisionando** para **Pronto**.

   >[!IMPORTANT]
   > Não selecione **Iniciar** ainda. Mantenha essa página da Web aberta porque você retornará a ela mais tarde.

   [![Provisionamento da solução de simulação de dispositivo concluído](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png#lightbox)

1. Agora volte ao portal do Azure e inspecione os recursos recém-criados em sua assinatura. Na página **Grupos de recursos** do portal, observe que um grupo de recursos foi criado usando o **Nome da solução** fornecido na última etapa. Observe também todos os recursos criados para dar suporte à solução de simulação de dispositivo.

   [![Recursos da solução de simulação de dispositivo](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png)](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png#lightbox)

## <a name="connect-the-environment-to-the-iot-hub"></a>Conectar o ambiente ao Hub IoT

Neste ponto, você aprendeu como criar dois conjuntos de recursos, cada um em seu próprio grupo de recursos:

- Um ambiente vazio do Time Series Insights.
- Recursos da solução de simulação de dispositivo, incluindo um hub IoT, gerados por um acelerador de solução.

Lembre-se de que os dispositivos simulados precisam se conectar a um Hub IoT para transmitir dados de dispositivo. Para criar um fluxo de dados para o ambiente do Time Series Insights, você precisará fazer alterações de configuração no hub IoT e no ambiente do Time Series Insights.

### <a name="iot-hub-configuration-define-a-consumer-group"></a>Configuração do hub IoT: Definir um grupo de consumidores

O Hub IoT fornece vários pontos de extremidade para compartilhar a funcionalidade com outros atores. O ponto de extremidade "Eventos" fornece uma maneira para outros aplicativos consumirem dados, conforme eles são transmitidos para uma instância do hub IoT. Especificamente, os "grupos de consumidores" fornecem um mecanismo para os aplicativos escutarem e efetuarem pull dados do hub IoT.

Em seguida, você definirá uma nova propriedade do **grupo de consumidores** no **ponto de extremidade Eventos** do hub IoT da solução de simulação de dispositivo.

1. No portal do Azure, acesse a página **Visão geral** do grupo de recursos criado para a solução de simulação de dispositivo. Selecione o recurso do hub IoT.

   [![Grupo de recursos da solução de simulação de dispositivo](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png#lightbox)

   Anote o **nome** do recurso do Hub IoT gerado para a solução. Você o consultará mais tarde.

1. Role a página para baixo e selecione a página **Pontos de extremidade** e, em seguida, o ponto de extremidade **Eventos**. Na página **Propriedades** do ponto de extremidade, insira um nome exclusivo para o ponto de extremidade no grupo de consumidores "$Default". Clique em **Salvar**.

   [![Pontos de extremidade do Hub IoT da solução de simulação de dispositivo](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png#lightbox)

### <a name="environment-configuration-define-an-event-source"></a>Configuração do ambiente: definir uma origem do evento

Agora, conecte o novo ponto de extremidade de evento do **grupo de consumidores** do hub IoT ao ambiente do Time Series Insights como uma **origem do evento**.

1. Acesse a página **Visão Geral** do grupo de recursos que você criou para o ambiente do Time Series Insights. Selecione o ambiente do Time Series Insights.

   [![Ambiente e grupo de recursos do ambiente do Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png#lightbox)

1. Na página do ambiente do Time Series Insights, selecione **Origens do Evento**. Em seguida, selecione **+ Adicionar**.

   [![Visão geral do ambiente do Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png#lightbox)

1. Insira os parâmetros necessários na página **Nova origem do evento**.

   Parâmetro|DESCRIÇÃO
   ---|---
   **Nome da origem do evento** | Requer um valor exclusivo, que é usado para nomear a origem do evento.
   **Fonte** | Selecione **Hub IoT**.
   **Importar opção** | Selecione o `Use IoT hub from available subscriptions` padrão. Essa opção faz com que a próxima lista suspensa seja populada com as assinaturas disponíveis.
   **Assinatura** | Selecione a mesma assinatura em que você criou o ambiente do Time Series Insights e os recursos de Simulação de dispositivo.
   **Nome do Hub IoT** | Deve ser usado como padrão para o nome do Hub IoT que você anotou anteriormente. Caso contrário, selecione o Hub IoT correto.
   **Nome da política do Hub IoT** | Selecione **iothubowner**.
   **Grupo de consumidores do Hub IoT** | Deve ser usado como padrão para o nome do grupo de consumidores do Hub IoT que você criou anteriormente. Caso contrário, selecione o nome do grupo de consumidores correto.
   **Formato de serialização do evento** | Mantenha-o como o valor padrão de `JSON`.
   **Nome da propriedade carimbo de data/hora** | Especifique-o como `timestamp`.

   Quando terminar, selecione **Criar** para adicionar a origem do evento. Ao retornar para a página **Visão geral** do grupo de recursos, junto com seu recurso de ambiente do Time Series Insights você vê um novo recurso de "Origem do evento de Time Series Insights".

   [![Tempo Origem de evento do novo ambiente do Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png#lightbox)

## <a name="run-device-simulation-to-stream-data"></a>Executar simulação de dispositivo para transmitir dados

Agora que todo o trabalho de configuração está concluído, é hora de popular o ambiente do Time Series Insights com os dados de exemplo dos dispositivos simulados.

Você pode se lembrar da [seção Criar uma simulação de dispositivo](#create-a-device-simulation), em que vários recursos do Azure foram criados pelo acelerador para dar suporte à solução. Junto com o Hub IoT discutido anteriormente, um aplicativo Web do Serviço de Aplicativo do Azure foi gerado para criar e transmitir a telemetria do dispositivo simulado.

1. Volte para o [Painel de aceleradores da solução](https://www.azureiotsolutions.com/Accelerators#dashboard). Entre novamente, se necessário, usando a mesma conta do Azure que você usou neste tutorial. Agora você pode selecionar **Iniciar** na solução de "Simulação de Dispositivo".

     [![Painel de aceleradores de solução](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png#lightbox)

1. O aplicativo Web de simulação de dispositivo será iniciado neste ponto e poderá levar alguns segundos após o carregamento inicial. Você também deverá fornecer consentimento para conceder ao aplicativo Web a permissão "Conectar você e ler seu perfil". Com essa permissão, o aplicativo pode recuperar as informações de perfil do usuário necessárias para dar suporte ao funcionamento do aplicativo.

     [![Consentimento do aplicativo Web da simulação de dispositivo](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png)](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png#lightbox)

1. Depois que a página **Instalação da simulação** for carregada, insira os parâmetros necessários.

   Parâmetro|DESCRIÇÃO
   ---|---
   **Hub IoT de destino** | Selecione **Usar o Hub IoT pré-provisionado**.
   **Modelo do dispositivo** | Selecione **Resfriador**.
   **Número de dispositivos**  | Insira `1000` em **Quantidade**.
   **Frequência de telemetria** | Insira `10` segundos.
   **Duração da simulação** | Selecione **Terminar em:** e insira `5` minutos.

   Quando terminar, selecione **Iniciar Simulação**. A simulação é executada por um total de 5 minutos. Ela gera dados de 1.000 dispositivos simulados a cada 10 segundos. 

   [![Instalação da simulação de dispositivo](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png#lightbox)

1. Enquanto a simulação é executada, observe que os campos **Total de mensagens** e **Mensagens por segundo** são atualizados, em intervalos de aproximadamente 10 segundos. A simulação é concluída após cerca de 5 minutos, levando você de volta à **Instalação da simulação**.

   [![Simulação de dispositivo em execução](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png#lightbox)

## <a name="verify-the-telemetry-data"></a>Verificar os dados telemétricos

Nesta seção final, verifique se os dados telemétricos foram gerados e armazenados no ambiente do Time Series Insights. Para verificar os dados, use o explorer do Time Series Insights, que é usado para consultar e analisar dados telemétricos.

1. Retorne à página **Visão geral** do grupo de recursos do ambiente do Time Series Insights. Selecione o ambiente do Time Series Insights.

   [![Ambiente e grupo de recursos do ambiente do Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png#lightbox)

1. Na página **Visão geral** do ambiente do Time Series Insights, selecione **URL do Gerenciador do Time Series Insights** para abrir o Gerenciador do Time Series Insights.

   [![Gerenciador do Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png#lightbox)

1. O Gerenciador do Time Series Insights é carregado e autenticado usando sua conta do portal do Azure. Após a exibição inicial, você poderá ver na área do gráfico que o ambiente do Time Series Insights foi populado com os dados telemétricos simulados. Para filtrar um intervalo de tempo mais estreito, selecione a lista suspensa no canto superior esquerdo. Insira um intervalo de tempo grande o suficiente para abranger a duração da simulação de dispositivo. Em seguida, selecione a lupa de pesquisa.

   [![Filtro de intervalo de tempo do Gerenciador do Time Series Insights](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png#lightbox)

1. A restrição do intervalo de tempo permite que o gráfico amplie as intermitências distintas da transferência de dados para o hub IoT e o ambiente do Time Series Insights. Observe também o texto **Streaming concluído** no canto superior direito, que mostra o número total de eventos encontrados. Arraste também o controle deslizante **Tamanho do intervalo** para controlar a granularidade do gráfico.

   [![Exibição filtrada do intervalo de tempo do Gerenciador do Time Series Insights](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png#lightbox)

1. Por fim, clique também em uma região para filtrar um intervalo. Em seguida, clique com o botão direito do mouse e use **Explorar eventos** para mostrar os detalhes do evento na exibição **Eventos** de tabela.

   [![Eventos e exibição filtrada do intervalo de tempo do Gerenciador do Time Series Insights](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png#lightbox)

## <a name="clean-up-resources"></a>Limpar recursos

Este tutorial cria vários serviços que executam o Azure para dar suporte ao ambiente do Time Series Insights e à solução de simulação de dispositivo. Caso deseje abandonar ou adiar seu trabalho nesta série de tutoriais, exclua todos os recursos para evitar a geração de custos desnecessários.

No menu à esquerda no portal do Azure:

1. Selecione o ícone **Grupos de recursos**. Em seguida, selecione o grupo de recursos criado para o ambiente do Time Series Insights. Na parte superior da página, selecione **Excluir grupo de recursos**, insira o nome do grupo de recursos e selecione **Excluir**.

1. Selecione o ícone **Grupos de recursos**. Em seguida, selecione o grupo de recursos criado pelo acelerador de solução de simulação de dispositivo. Na parte superior da página, selecione **Excluir grupo de recursos**, insira o nome do grupo de recursos e selecione **Excluir**.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

> [!div class="checklist"]
> * Crie um ambiente do Time Series Insights.
> * Criar uma solução de simulação de dispositivo que contém um hub IoT.
> * Conectar o ambiente do Time Series Insights ao hub IoT.
> * Executar uma simulação de dispositivo para transmitir dados para o ambiente do Time Series Insights.
> * Verificar os dados telemétricos simulados.

Agora que você sabe como criar seu próprio ambiente do Time Series Insights, saiba como criar um aplicativo Web que recebe dados de um ambiente do Time Series Insights:

> [!div class="nextstepaction"]
> [Criar um aplicativo Web de página única do Azure Time Series Insights](tutorial-create-tsi-sample-spa.md)
