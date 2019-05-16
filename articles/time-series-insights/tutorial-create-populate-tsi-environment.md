---
title: 'Tutorial: Criar um ambiente de Análise de Séries Temporais do Azure | Microsoft Docs'
description: Saiba como criar um ambiente de Time Series Insights, preenchido com dados de dispositivos simulados.
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: tutorial
ms.date: 04/26/2019
ms.author: anshan
manager: cshankar
ms.custom: seodec18
ms.openlocfilehash: 42a7ba0c66bd603b19d60c7b3407ae5ca80db28e
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65210177"
---
# <a name="tutorial-create-an-azure-time-series-insights-environment"></a>Tutorial: Criar um ambiente do Azure Time Series Insights

Este tutorial guiará você pelo processo de criação de um ambiente Time Series Insight (TSI), preenchido com dados de dispositivos simulados. Neste tutorial, você aprenderá como:

> [!div class="checklist"]
> * Criar um ambiente TSI 
> * Criar uma solução de simulação de dispositivo que contém um Hub IoT
> * Conectar o ambiente TSI ao Hub IoT
> * Executar uma simulação de dispositivo para transmitir dados ao ambiente TSI
> * Verificar os dados telemétricos simulados

## <a name="video"></a>Vídeo

### <a name="learn-how-to-use-an-azure-iot-solution-accelerator-to-generate-data-and-get-started-with-time-series-insights-br"></a>Saiba como usar um acelerador de solução de IoT do Azure para gerar dados e começar a usar o Time Series Insights. </br>

> [!VIDEO https://www.youtube.com/embed/6ehNf6AJkFo]

## <a name="prerequisites"></a>Pré-requisitos

* Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/).

* Sua conta de entrada do Azure também precisa ser um membro da função **Proprietário** da assinatura. Confira [Gerenciar o acesso usando o RBAC e o portal do Azure](/azure/role-based-access-control/role-assignments-portal) para obter mais detalhes.

## <a name="overview"></a>Visão geral

O ambiente TSI é onde os dados do dispositivo são coletados e armazenados. Depois que os dados forem armazenados no ambiente do TSI, o [Gerenciador do TSI](time-series-quickstart.md) e a [API de Consulta do TSI](/rest/api/time-series-insights/ga-query-api) podem ser usados para consultar e analisar os dados. O Hub IoT é o ponto de conexão usado por todos os dispositivos (simulados ou físicos) para se conectar com segurança e transmitir dados à nuvem do Azure. A [Visão geral do TSI](time-series-insights-overview.md) indica que o Hub IoT do Azure também serve como uma origem do evento para transmitir dados para o ambiente do TSI. Este tutorial usa um [acelerador de solução de IoT](/azure/iot-accelerators/) para gerar e transmitir dados telemétricos de exemplo para o Hub IoT.

>[!TIP]
> Os aceleradores de solução de IoT fornecem soluções de nível empresarial pré-configuradas que permitem acelerar o desenvolvimento de soluções personalizadas de IoT.

## <a name="create-a-tsi-environment"></a>Criar um ambiente TSI

Primeiro, crie um ambiente TSI na sua assinatura do Azure:

1. Entre no [Portal do Azure](https://portal.azure.com) usando sua conta da assinatura do Azure.  
1. Selecione **+ Criar um recurso** no canto superior esquerdo.  
1. Selecione a categoria **Internet das Coisas** e depois **Time Series Insights**.  

   [![Selecione o recurso de ambiente Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi.png#lightbox)

1. Na página **Ambiente Time Series Insights**, preencha os parâmetros necessários:

   Parâmetro|DESCRIÇÃO
   ---|---
   **Nome do ambiente** | Escolha um nome exclusivo para ambiente TSI. O nome é usado pelo Gerenciador do TSI e pelas APIs de Consulta.
   **Assinatura** | As assinaturas são contêineres para recursos do Azure. Escolha uma assinatura na qual o ambiente do TSI será criado.
   **Grupo de recursos** | Um grupo de recursos é um contêiner para os recursos do Azure. Escolha um grupo de recursos existente ou crie um para o recurso do ambiente do TSI.
   **Localidade** | Escolha uma região do data center para seu ambiente TSI. Para evitar custos e latência adicionais de largura de banda, é melhor manter o ambiente TSI na mesma região que outros recursos da IoT.
   **SKU de preço** | Escolha a taxa de transferência necessária. Para obter o custo mais baixo e a capacidade inicial, selecione `S1`.
   **Capacidade** | A capacidade é o multiplicador aplicado à taxa de ingresso, à capacidade de armazenamento e ao custo associado com a SKU selecionada.  Você pode alterar a capacidade após a criação. Para obter o custo mais baixo, selecione uma capacidade igual a 1.

   Ao concluir, clique em **Criar** para iniciar o processo de provisionamento.

   [![Criar um recurso do ambiente do Time Series Insights](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-params.png#lightbox)

1. Verifique o painel **Notificações** para monitorar a conclusão da implantação.  

   [![Implantação do ambiente do Time Series Insights bem-sucedida](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png)](media/tutorial-create-populate-tsi-environment/ap-create-resource-tsi-deployment-succeeded.png#lightbox)

## <a name="create-a-device-simulation"></a>Criar uma simulação de dispositivo

Em seguida, crie a solução de simulação do dispositivo, que gerará dados de teste para preencher o seu ambiente TSI:

1. Em uma guia/janela separada, acesse [azureiotsolutions.com](https://www.azureiotsolutions.com). Entre usando a mesma conta de assinatura do Azure e selecione o acelerador de **Simulação de Dispositivo**.

   [![Executar o acelerador de Simulação de Dispositivo](media/tutorial-create-populate-tsi-environment/sa-main.png)](media/tutorial-create-populate-tsi-environment/sa-main.png#lightbox)

1. Insira os parâmetros necessários na página da **solução Criar Simulação de Dispositivo**.

   Parâmetro|DESCRIÇÃO
   ---|---
   **Nome da solução** | Um valor exclusivo usado para a criação de um novo grupo de recursos. Os recursos do Azure listados são criados e atribuídos ao grupo de recursos.
   **Assinatura** | Especifique a mesma assinatura usada para a criação de seu ambiente TSI, na seção anterior.
   **Região** | Especifique a mesma região usada para a criação de seu ambiente TSI, na seção anterior.
   **Implantar recursos opcionais do Azure** | Deixe a opção **Hub IoT** marcada, pois os dispositivos simulados usarão para se conectar/transmitir dados.

   Quando terminar, clique em **Criar solução** para provisionar os recursos do Azure da solução. Esse processo pode levar de 6 a 7 minutos para ser concluído.

   [![Provisionar a solução de simulação de dispositivo](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution.png#lightbox)

1. Após a conclusão do provisionamento, o texto acima da nova solução será alterado de **Provisionando…** para **Pronto**:

   >[!IMPORTANT]
   > Não clique no botão **Iniciar** ainda! Mas mantenha essa página da Web aberta, pois você retornará a ela mais tarde.

   [![Provisionamento da solução de simulação de dispositivo concluído](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard-ready.png#lightbox)

1. Agora volte ao portal do Azure e inspecione os recursos criados recentemente na sua assinatura. No página do portal do **Grupos de recursos**, você verá que um novo grupo de recursos foi criado usando o **Nome da solução** fornecido na última etapa. Observe também todos os recursos criados para oferecer suporte à solução de simulação do dispositivo:

   [![Recursos da solução de simulação de dispositivo](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png)](media/tutorial-create-populate-tsi-environment/ap-device-sim-solution-resources.png#lightbox)

## <a name="connect-the-tsi-environment-to-the-iot-hub"></a>Conectar o ambiente TSI ao Hub IoT

Neste ponto, você aprendeu como criar dois conjuntos de recursos, cada um em seu próprio grupo de recursos:

- Um ambiente do TSI vazio.
- Recursos da solução de simulação de dispositivo, incluindo um hub IoT, gerado por um acelerador de solução.

Lembre-se de que os dispositivos simulados precisam se conectar a um Hub IoT para transmitir dados de dispositivo. Para transmitir dados para o ambiente TSI, você precisa fazer alterações de configuração para o Hub IoT e o ambiente TSI.

### <a name="iot-hub-configuration-define-a-consumer-group"></a>Configuração do Hub IoT: definir um grupo de consumidores

O Hub IoT fornece vários pontos de extremidade para compartilhar a funcionalidade para outros atores. O ponto de extremidade de “Eventos” fornece uma maneira para outros aplicativos consumirem dados, uma vez que ele é transmitido para uma instância do Hub IoT. Especificamente, os “grupos de consumidores” fornecem um mecanismo para aplicativos ouvirem e receberem dados do Hub IoT.

Em seguida, você definirá uma nova propriedade de **grupo de consumidores** no **ponto de extremidade de Eventos** do hub IoT da solução de simulação de dispositivo.

1. No portal do Azure, vá até a página **Visão geral** do grupo de recursos que você criou para a solução de simulação do dispositivo e selecione o recurso de Hub IoT:

   [![Grupo de recursos da solução de simulação de dispositivo](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-view-rg.png#lightbox)

   Também anote o **Nome** do recurso Hub IoT gerado para a solução, pois você vai se referir a ele mais tarde.

1. Role para baixo e selecione a página **Pontos de extremidade**, depois selecione o ponto de extremidade de **Eventos**. Na página do ponto de extremidade **Propriedades**, insira um nome exclusivo para o ponto de extremidade sob o grupo de consumidores “$Default” e clique em **Salvar**:

   [![Pontos de extremidade do Hub IoT da solução de simulação de dispositivo](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png)](media/tutorial-create-populate-tsi-environment/ap-add-iot-hub-consumer-group-create.png#lightbox)

### <a name="tsi-configuration-define-an-event-source"></a>Configuração do TSI: definir uma origem do evento

Agora, conecte o novo ponto de extremidade de evento do **grupo de consumidores** do Hub IoT ao ambiente do TSI como uma **origem do evento**.

1. Vá até a página **Visão geral** do grupo de recursos que você criou para o ambiente TSI e selecione o ambiente TSI:

   [![Ambiente e grupo de recursos do ambiente TSI](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-view-rg.png#lightbox)

1. Na página do ambiente do TSI, selecione **Origens do Evento** e, em seguida, clique em **+ Adicionar**.

   [![Visão geral do ambiente do TSI](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add.png#lightbox)

1. Insira os parâmetros necessários na página **Nova origem do evento**.

   Parâmetro|DESCRIÇÃO
   ---|---
   **Nome da origem do evento** | Requer um valor exclusivo, que é usado para nomear a origem do evento.
   **Fonte** | Selecione **Hub IoT**.
   **Importar opção** | Selecione o `Use IoT hub from available subscriptions` padrão. Essa opção fará com que a próxima lista suspensa seja preenchida com as assinaturas disponíveis.
   **Assinatura** | Selecione a mesma assinatura em que você criou o ambiente TSI e os recursos de Simulação de dispositivo.
   **Nome do Hub IoT** | Deve ser usado como padrão para o nome do Hub IoT que você anotou anteriormente. Caso contrário, selecione o Hub IoT correto.
   **Nome da política do Hub IoT** | Selecione **iothubowner**.
   **Grupo de consumidores do Hub IoT** | Deve ser usado como padrão para o nome do grupo de consumidores do Hub IoT que você criou anteriormente. Caso contrário, selecione o nome do grupo de consumidores correto.
   **Formato de serialização do evento** | Mantenha-o como o valor padrão de `JSON`.
   **Nome da propriedade carimbo de data/hora** | Especifique-o como `timestamp`.

   Ao concluir, clique em **Criar** para adicionar a origem do evento. Ao retornar para a página **Visão geral** do grupo de recursos, junto com seu recurso de ambiente TSI você vê um novo recurso de “Origem do evento de Time Series Insights”.

   [![Nova origem do evento do ambiente do TSI](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png)](media/tutorial-create-populate-tsi-environment/ap-add-env-event-source-add-event-source.png#lightbox)

## <a name="run-device-simulation-to-stream-data-into-tsi"></a>Executar uma simulação de dispositivo para transmitir dados para o TSI

Agora que todo o trabalho de configuração está concluído, é hora de preencher o ambiente TSI com dados de exemplo dos dispositivos simulados.

Você pode se lembrar de [Criar uma seção de simulação de dispositivo](#create-a-device-simulation), vários recursos do Azure foram criados pelo acelerador para oferecer suporte à solução. Junto com o Hub IoT discutido anteriormente, um aplicativo Web do Serviço de Aplicativo do Azure foi gerado para criar e transmitir a telemetria do dispositivo simulado.

1. Volte para o [Painel de aceleradores da solução](https://www.azureiotsolutions.com/Accelerators#dashboard). Entre novamente se necessário, usando a mesma conta do Azure que você usou neste tutorial. Agora, você pode clicar no botão **Iniciar** na solução "Simulação de Dispositivo".

     [![Painel de aceleradores de solução](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png)](media/tutorial-create-populate-tsi-environment/sa-create-device-sim-solution-dashboard.png#lightbox)

1. O aplicativo Web do dispositivo de simulação será iniciado nesse ponto e pode levar alguns segundos após o carregamento inicial. Também será solicitado o consentimento para dar ao aplicativo Web a permissão para “Conectar você e ler seu perfil”. Com essa permissão, o aplicativo pode recuperar as informações de perfil do usuário necessárias para dar suporte ao funcionamento do aplicativo.

     [![Consentimento do aplicativo Web da simulação de dispositivo](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png)](media/tutorial-create-populate-tsi-environment/sawa-signin-consent.png#lightbox)

1. Assim que a página **Instalação da simulação** for carregada, insira os parâmetros necessários.

   Parâmetro|DESCRIÇÃO
   ---|---
   **Hub IoT de destino** | Selecione **Usar o Hub IoT pré-provisionado**.
   **Modelo do dispositivo** | Selecione **Resfriador**.
   **Número de dispositivos**  | Insira `1000` em **Quantidade**.
   **Frequência de telemetria** | Insira `10` segundos.
   **Duração da simulação** | Selecione **Terminar em:** e insira `5` minutos.

   Quando terminar, clique em **Começar a simulação**. A simulação será executada por um total de 5 minutos, gerando dados de 1000 dispositivos simulados a cada 10 segundos.  

   [![Instalação da simulação de dispositivo](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-setup.png#lightbox)

1. Enquanto a simulação é executada, você verá a atualização dos campos **Total de mensagens** e **Mensagens por segundo** aproximadamente a cada 10 segundos. A simulação terminará após cerca de 5 minutos, levando você de volta à **Instalação da simulação**.

   [![Simulação de dispositivo em execução](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png)](media/tutorial-create-populate-tsi-environment/sawa-simulation-running.png#lightbox)

## <a name="verify-the-telemetry-data"></a>Verificar os dados telemétricos

Nesta seção final, verifique se os dados telemétricos foram gerados e armazenados no ambiente TSI. Para verificar os dados, use o explorer do Time Series Insights, que é usado para consultar e analisar dados telemétricos.

1. Retorne à página **Visão Geral** do grupo de recursos do ambiente do TSI. Selecione o ambiente do TSI.

   [![Ambiente e grupo de recursos do ambiente TSI](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-rg.png#lightbox)

1. Na página **Visão Geral** do ambiente do TSI, clique em **URL do Gerenciador do Time Series Insights** para abrir o Gerenciador do TSI.

   [![Explorer do TSI](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png)](media/tutorial-create-populate-tsi-environment/ap-view-tsi-env-explorer-url.png#lightbox)

1. O explorer do TSI carregará e autenticará usando sua conta do portal do Azure. Após a exibição inicial, você pode ver na área do gráfico que o ambiente TSI realmente foi preenchido com dados telemétricos simulados. Para filtrar um intervalo mais estreito de tempo, selecione na lista suspensa no canto superior esquerdo. Depois insira um intervalo de tempo grande o suficiente para abranger a duração da simulação de dispositivo. Em seguida, clique na lupa de pesquisa.

   [![Filtro de intervalo de tempo do explorer do TSI ](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-filter-time-range.png#lightbox)

1. Restringir o intervalo de tempo permite que o gráfico amplie as intermitências distintas da transferência de dados para o Hub IoT e o ambiente TSI. Observe também o texto **Transmissão completa** no canto superior direito, que mostra o número total de eventos encontrados. Arraste também o controle deslizante **Tamanho do intervalo** para controlar a granularidade do gráfico.

   [![Exibição filtrada do intervalo de tempo do explorer do TSI](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range.png#lightbox)

1. Por fim, clique também em uma região para filtrar um intervalo e, em seguida, clique com o botão direito do mouse e use **Explorar eventos** para mostrar os detalhes do evento na exibição de tabela **Eventos**.

   [![Exibição filtrada e eventos do intervalo de tempo do explorer do TSI](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png)](media/tutorial-create-populate-tsi-environment/tsie-view-time-range-events.png#lightbox)

## <a name="clean-up-resources"></a>Limpar recursos

Este tutorial cria vários serviços que executam o Azure, para oferecer suporte à solução de ambiente TSI e simulação de dispositivo. Caso queira abandonar e/ou atrasar a conclusão desta série de tutoriais, é recomendável excluir todos os recursos para evitar incorrer em custos desnecessários.

No menu à esquerda no portal do Azure:

1. Clique no ícone de **Grupos de recursos**, depois selecione o grupo de recursos que você criou para o ambiente TSI. Na parte superior da página, clique em **Excluir grupo de recursos**, insira o nome do grupo de recursos e depois clique em **Excluir**.

1. Clique no ícone de **Grupos de recursos**, depois selecione o grupo de recursos que foi criado com o acelerador de solução de simulação do dispositivo. Na parte superior da página, clique em **Excluir grupo de recursos**, insira o nome do grupo de recursos e, em seguida, clique em **Excluir**

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:

> [!div class="checklist"]
> * Criar um ambiente TSI 
> * Criar uma solução de simulação de dispositivo que contém um Hub IoT
> * Conectar o ambiente TSI ao Hub IoT
> * Executar uma simulação de dispositivo para transmitir dados ao ambiente TSI
> * Verificar os dados telemétricos simulados

Agora que você sabe como criar seu próprio ambiente TSI, saiba como criar um aplicativo Web que recebe dados de um ambiente TSI:

> [!div class="nextstepaction"]
> [Criar um aplicativo Web de página única do Azure Time Series Insights](tutorial-create-tsi-sample-spa.md)