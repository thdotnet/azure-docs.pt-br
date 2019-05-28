---
title: 'Tutorial: Configurar um ambiente da Versão Prévia do Azure Time Series Insights | Microsoft Docs'
description: Saiba como configurar seu ambiente na Versão Prévia do Azure Time Series Insights.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 04/25/2019
ms.custom: seodec18
ms.openlocfilehash: f83063a88207f51f9d481447923fd8a8498692a2
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64713902"
---
# <a name="tutorial-set-up-an-azure-time-series-insights-preview-environment"></a>Tutorial: Configurar um ambiente de Versão Prévia do Azure Time Series Insights

Este tutorial orienta você pelo processo de criação de um ambiente PAYG (Pago Conforme o Uso) da Versão Prévia do Azure Time Series Insights. Neste tutorial, você aprenderá como:

* Criar um ambiente de Versão Prévia do Azure Time Series Insights.
* Conectar o ambiente de Versão Prévia do Azure Time Series Insights a um hub de eventos nos Hubs de Eventos do Azure.
* Executar uma amostra do acelerador de solução para transmitir dados para o ambiente da Versão Prévia do Azure Time Series Insights.
* Executar uma análise básica nos dados.
* Definir uma hierarquia e um tipo de Modelo de Série Temporal e associá-los às suas instâncias.

# <a name="create-a-device-simulation"></a>Criar uma simulação de dispositivo

Nesta seção, você criará três dispositivos simulados que enviarão dados para um hub IoT.

1. Acesse a [página de aceleradores de solução do IoT do Azure](https://www.azureiotsolutions.com/Accelerators). A página exibe vários exemplos pré-criados. Entre usando sua conta do Azure. Em seguida, selecione **Simulação de Dispositivo**.

   [![Página de aceleradores de solução do IoT do Azure](media/v2-update-provision/device-one-accelerator.png)](media/v2-update-provision/device-one-accelerator.png#lightbox)

   Selecione **Experimentar Agora**.

1. Insira os parâmetros necessários na página **Criar solução de Simulação de dispositivo**:

   | Parâmetro | DESCRIÇÃO |
   | --- | --- |
   | **Nome da solução** |    Insira um valor exclusivo para a criação de um novo grupo de recursos. Os recursos do Azure listados são criados e atribuídos ao grupo de recursos. |
   | **Assinatura** | Especifique a mesma assinatura usada para a criação do ambiente do Time Series Insights. |
   | **Região** |   Especifique a mesma região usada para a criação do ambiente do Time Series Insights. |
   | **Implantar recursos opcionais do Azure**    | Deixe o Hub IoT selecionado, porque os dispositivos simulados o usarão para se conectarem e transmitir dados. |

   Em seguida, selecione **Criar solução**. Aguarde 10 a 15 minutos para que a solução seja implantada.

   [![Página da solução Criar Simulação de Dispositivo](media/v2-update-provision/device-two-create.png)](media/v2-update-provision/device-two-create.png#lightbox)

1. No painel do acelerador de solução, selecione o botão **Iniciar**:

   [![Inicializar a solução de simulação de dispositivo](media/v2-update-provision/device-three-launch.png)](media/v2-update-provision/device-three-launch.png#lightbox)

1. Você será redirecionado para a página **Simulação de Dispositivo IoT do Microsoft Azure**. Selecione **+ Nova simulação** no canto superior direito da página.

   [![Página de simulação de IoT do Azure](media/v2-update-provision/device-four-iot-sim-page.png)](media/v2-update-provision/device-four-iot-sim-page.png#lightbox)

1. Preencha os parâmetros obrigatórios da seguinte maneira:

    [![Parâmetros a preencher](media/v2-update-provision/device-five-params.png)](media/v2-update-provision/device-five-params.png#lightbox)

    |||
    | --- | --- |
    | **Nome** | Insira um nome exclusivo para um simulador. |
    | **Descrição** | Insira uma definição. |
    | **Duração da simulação** | Defina essa opção como **Executar indefinidamente**. |
    | **Modelo do dispositivo** | **Nome**: Digite `Chiller`. </br>**Valor**: Digite `3`. |
    | **Hub IoT de destino** | Defina essa opção como **Usar o Hub IoT pré-provisionado**. |

    Em seguida, selecione **Iniciar simulação**.

1. No painel de simulação do dispositivo, confira **Dispositivos ativos** e **Mensagens por segundo**.

    [![Painel de simulação de IoT do Azure](media/v2-update-provision/device-seven-dashboard.png)](media/v2-update-provision/device-seven-dashboard.png#lightbox)

## <a name="list-device-simulation-properties"></a>Listar propriedades de simulação de dispositivo

Antes de criar um ambiente do Azure Time Series Insights, você precisa dos nomes do hub IoT, da assinatura e do grupo de recursos.

1. Acesse o painel do acelerador de solução e entre usando a mesma conta de assinatura do Azure. Encontre a simulação de dispositivo que você criou nas etapas anteriores.

1. Selecione o simulador de dispositivo e selecione **Iniciar**. Selecione o link **Portal de Gerenciamento do Azure** no lado direito.

    [![Listagens de simulador](media/v2-update-provision/device-six-listings.png)](media/v2-update-provision/device-six-listings.png#lightbox)

1. Anote os nomes do hub IoT, da assinatura e do grupo de recursos.

    [![Portal do Azure](media/v2-update-provision/device-eight-portal.png)](media/v2-update-provision/device-eight-portal.png#lightbox)

## <a name="create-a-time-series-insights-preview-payg-environment"></a>Criar um ambiente PAYG de Versão Prévia do Time Series Insights

Esta seção descreve como criar um ambiente de Versão Prévia do Azure Time Series Insights usando o [portal do Azure](https://portal.azure.com/).

1. Entre no portal do Azure usando sua conta da assinatura.

1. Selecione **Criar um recurso**.

1. Selecione a categoria **Internet das Coisas** e depois selecione **Time Series Insights**.

   [![Selecione Internet das Coisas e, em seguida, Time Series Insights](media/v2-update-provision/payg-one-azure.png)](media/v2-update-provision/payg-one-azure.png#lightbox)

1. Preencha os campos na página da seguinte maneira:

   | | |
   | --- | ---|
   | **Nome do ambiente** | Escolha um nome exclusivo para o ambiente de Versão Prévia do Azure Time Series Insights. |
   | **Assinatura** | Insira sua assinatura em que você deseja criar o ambiente de Versão Prévia do Azure Time Series Insights. É uma melhor prática usar a mesma assinatura do restante dos recursos IoT criados pelo simulador de dispositivo. |
   | **Grupo de recursos** | Um grupo de recursos é um contêiner para os recursos do Azure. Escolha um grupo de recursos existente ou crie um novo para o recurso de ambiente de Versão Prévia do Azure Time Series Insights. É uma melhor prática usar o mesmo grupo de recursos do restante dos recursos IoT criados pelo simulador de dispositivo. |
   | **Localidade** | Escolha uma região do datacenter para o ambiente da Versão Prévia do Azure Time Series Insights. Para evitar custos de largura de banda adicional e latência, é melhor manter o ambiente de Versão Prévia do Azure Time Series Insights na mesma região que outros recursos de IoT. |
   | **Camada** |  Selecione **PAYG**, que significa Pago Conforme o Uso. Essa é a SKU para a Versão Prévia do Azure Time Series Insights. |
   | **ID da Propriedade** | Insira algo que identifique exclusivamente a série temporal. Observe que esse campo é imutável e não pode ser alterado posteriormente. Neste tutorial, utilize `iothub-connection-device-id`. Para saber mais sobre a ID de Série Temporal, leia [Como escolher uma ID de Série Temporal](./time-series-insights-update-how-to-id.md). |
   | **Nome da conta de armazenamento** | Insira um nome global exclusivo para uma nova conta de armazenamento a ser criada. |

   Em seguida, selecione **Avançar: Origem do Evento**.

   [![Página para criação de um ambiente do Time Series Insights](media/v2-update-provision/payg-two-create.png)](media/v2-update-provision/payg-two-create.png#lightbox)

1. Na página da origem do evento, preencha os campos da seguinte maneira:

   | | |
   | --- | --- |
   | **Criar uma origem do evento?** | Digite `Yes`.|
   | **Nome** | Insira um valor exclusivo que é usado para nomear a origem do evento.|
   | **Tipo de fonte** | Selecione **Hub IoT**. |
   | **Selecionar um hub?** | Escolha **Selecionar existente**. |
   | **Assinatura** | Selecione a assinatura usada para o simulador de dispositivo. |
   | **Nome do Hub IoT** | Selecione o nome do hub IoT criado para o simulador de dispositivo. |
   | **Política de acesso do Hub IoT** | Selecione **iothubowner**. |
   | **Grupo de consumidores do Hub IoT** | Você precisa de um grupo de consumidores exclusivo para a Versão Prévia do Azure Time Series Insights. Selecione **Novo**, insira um nome exclusivo e, em seguida, selecione **Adicionar**. |
   | **Propriedade de carimbo de data/hora** | Esse campo é usado para identificar a propriedade de carimbo de data/hora em seus dados de telemetria de entrada. Para este tutorial, não preencha o campo. Esse simulador usa o carimbo de data/hora de entrada do Hub IoT, que é usado por padrão pelo Time Series Insights.|

   Em seguida, selecione **Examinar + criar**.

   [![Configurar uma Fonte de Evento](media/v2-update-provision/payg-five-event-source.png)](media/v2-update-provision/payg-five-event-source.png#lightbox)

1. Examine todos os campos na página Examinar e selecione **Criar**.

   [![Página Revisar + Criar com o botão Criar](media/v2-update-provision/payg-six-review.png)](media/v2-update-provision/payg-six-review.png#lightbox)

1. Você pode ver o status da implantação.

   [![Notificação de que a implantação foi concluída](media/v2-update-provision/payg-seven-deploy.png)](media/v2-update-provision/payg-seven-deploy.png#lightbox)

1. Você deverá receber o acesso ao ambiente da Versão Prévia do Azure Time Series Insights se você for o proprietário do locatário. Para verificar se você tem acesso:

    a. Pesquise o grupo de recursos e selecione o ambiente da Versão Prévia do Azure Time Series Insights:

      [![Ambiente selecionado](media/v2-update-provision/payg-eight-environment.png)](media/v2-update-provision/payg-eight-environment.png#lightbox)

   b. Na página da Versão Prévia do Azure Time Series Insights, acesse **Políticas de Acesso a Dados**.

     [![Políticas de acesso de dados](media/v2-update-provision/payg-nine-data-access.png)](media/v2-update-provision/payg-nine-data-access.png#lightbox)

   c. Verifique se suas credenciais estão listadas.

     [![Credenciais listadas](media/v2-update-provision/payg-ten-verify.png)](media/v2-update-provision/payg-ten-verify.png#lightbox)

   Se as credenciais não estiverem listadas, você precisará conceder permissão a você mesmo para acessar o ambiente. Para saber mais sobre como definir permissões, leia [Conceder o acesso a dados](./time-series-insights-data-access.md).

## <a name="analyze-data-in-your-environment"></a>Analisar dados em seu ambiente

Nesta seção, você executa análise básica em seus dados de série temporal usando o [gerenciador de Versão Prévia do Azure Time Series Insights](./time-series-insights-update-explorer.md).

1. Acesse o gerenciador da Versão Prévia do Azure Time Series Insights selecionando a URL na página do recurso no [portal do Azure](https://portal.azure.com/).

   [![A URL do gerenciador da Versão Prévia do Time Series Insights](media/v2-update-provision/analyze-one-portal.png)](media/v2-update-provision/analyze-one-portal.png#lightbox)

1. No gerenciador, selecione o nó **Time Series Insights** para ver todas as instâncias da Versão Prévia do Azure Time Series Insights no ambiente.

   [![Lista de instâncias órfãs](media/v2-update-provision/analyze-two-unparented.png)](media/v2-update-provision/analyze-two-unparented.png#lightbox)

1. Na série temporal mostrada, selecione a primeira instância. Em seguida, selecione **Mostrar pressão média**.

   [![Instância selecionada com o comando de menu para mostrar a pressão média](media/v2-update-provision/analyze-three-show-pressure.png)](media/v2-update-provision/analyze-three-show-pressure.png#lightbox)

   Um gráfico de série temporal deve aparecer à direita. Ajuste o **Intervalo** para `15s`.

   [![Gráfico de série temporal](media/v2-update-provision/analyze-four-chart.png)](media/v2-update-provision/analyze-four-chart.png#lightbox)

1. Repita a **etapa 3** com as outras duas séries temporais. Em seguida, você poderá exibir todas as séries temporais, conforme mostrado neste gráfico:

   [![Gráfico de todas as séries temporais](media/v2-update-provision/analyze-five-chart.png)](media/v2-update-provision/analyze-five-chart.png#lightbox)

1. Modifique o intervalo de tempo para ver as tendências de séries temporais da última hora.

    a. Selecione a caixa da opção **Período**:

      [![Definir o intervalo de tempo como uma hora](media/v2-update-provision/analyze-six-time.png)](media/v2-update-provision/analyze-six-time.png#lightbox)

## <a name="define-and-apply-a-model"></a>Definir e aplicar um modelo

Nesta seção, você aplicará um modelo para estruturar seus dados. Para concluir o modelo, você definirá tipos, hierarquias e instâncias. Para saber mais sobre modelagem de dados, acesse [Modelo de Série Temporal](./time-series-insights-update-tsm.md).

1. No gerenciador, selecione a guia **Modelo**:

   [![Guia Modelo no gerenciador](media/v2-update-provision/define-one-model.png)](media/v2-update-provision/define-one-model.png#lightbox)

1. Selecione **+ Adicionar** para adicionar um tipo. No lado direito, um editor de tipos será aberto.

   [![O botão Adicionar para tipos](media/v2-update-provision/define-two-add.png)](media/v2-update-provision/define-two-add.png#lightbox)

1. Defina três variáveis para o tipo: pressão, temperatura e umidade. Insira as seguintes informações:

   | | |
   | --- | ---|
   | **Nome** | Digite `Chiller`. |
   | **Descrição** | Digite `This is a type definition of Chiller`. |

   * Defina a pressão com três variáveis:

      | | |
      | --- | ---|
      | **Nome** | Digite `Avg Pressure`. |
      | **Valor** | Selecione **pressão (Duplo)** . Observe que poderá levar alguns minutos para esse campo ser populado depois que a Versão Prévia do Azure Time Series Insights começar a receber eventos. |
      | **Operação de Agregação** | Selecione **AVG**. |

      [![Seleções para definição da pressão](media/v2-update-provision/define-three-variable.png)](media/v2-update-provision/define-three-variable.png#lightbox)

      Selecione **+ Adicionar Variável** para adicionar a próxima variável.

   * Defina a temperatura:

      | | |
      | --- | ---|
      | **Nome** | Digite `Avg Temperature`. |
      | **Valor** | Selecione **temperatura (Duplo)** . Observe que poderá levar alguns minutos para esse campo ser populado depois que a Versão Prévia do Azure Time Series Insights começar a receber eventos. |
      | **Operação de Agregação** | Selecione **AVG**.|

      [![Seleções para definição da temperatura](media/v2-update-provision/define-four-avg.png)](media/v2-update-provision/define-four-avg.png#lightbox)

   * Defina a umidade:

      | | |
      | --- | ---|
      | **Nome** | Inserir `Max Humidity` |
      | **Valor** | Selecione **umidade (Duplo)** . Observe que poderá levar alguns minutos para esse campo ser populado depois que a Versão Prévia do Azure Time Series Insights começar a receber eventos. |
      | **Operação de Agregação** | Selecione **MAX**.|

      [![Seleções para definição da temperatura](media/v2-update-provision/define-five-humidity.png)](media/v2-update-provision/define-five-humidity.png#lightbox)

   Em seguida, selecione **Criar**.

1. Você poderá ver o tipo adicionado:

   [![Informações sobre o tipo adicionado](media/v2-update-provision/define-six-type.png)](media/v2-update-provision/define-six-type.png#lightbox)

1. A próxima etapa é adicionar uma hierarquia. Na seção **Hierarquias**, selecione **+ Adicionar**:

   [![Guia Hierarquias com o botão Adicionar](media/v2-update-provision/define-seven-hierarchy.png)](media/v2-update-provision/define-seven-hierarchy.png#lightbox)

1. Defina a hierarquia. Preencha os campos da seguinte maneira:

   | | |
   | --- | ---|
   | **Nome** | Digite `Location Hierarchy`. |
   | **Nível 1** | Digite `Country`. |
   | **Nível 2** | Digite `City`. |
   | **Nível 3** | Digite `Building`. |

   Em seguida, selecione **Criar**.

   [![Campos de hierarquia com o botão Criar](media/v2-update-provision/define-eight-add-hierarchy.png)](media/v2-update-provision/define-eight-add-hierarchy.png#lightbox)

1. Você poderá ver a hierarquia criada:

   [![Informações sobre a hierarquia](media/v2-update-provision/define-nine-created.png)](media/v2-update-provision/define-nine-created.png#lightbox)

1. Selecione **Instâncias** à esquerda. Depois que as instâncias forem exibidas, selecione a primeira instância e, em seguida, selecione **Editar**:

   [![Selecionando o botão Editar de uma instância](media/v2-update-provision/define-ten-edit.png)](media/v2-update-provision/define-ten-edit.png#lightbox)

1. No lado direito, um editor de texto será exibido. Adicione as seguintes informações:

   | | |
   | --- | --- |
   | **Tipo** | Selecione **Resfriador**. |
   | **Descrição** | Digite `Instance for Chiller-01.1`. |
   | **Hierarquias** | Selecione **Hierarquia de Localizações**. |
   | **País** | Digite `USA`. |
   | **Cidade** | Digite `Seattle`. |
   | **Prédio** | Digite `Space Needle`. |

    Em seguida, selecione **Salvar**.

   [![Campos de instância com o botão Salvar](media/v2-update-provision/define-eleven-chiller.png)](media/v2-update-provision/define-eleven-chiller.png#lightbox)

1. Repita a etapa anterior para os outros sensores. Use os seguintes campos:

   * Para o Resfriador 01.2:

     | | |
     | --- | --- |
     | **Tipo** | Selecione **Resfriador**. |
     | **Descrição** | Digite `Instance for Chiller-01.2`. |
     | **Hierarquias** | Selecione **Hierarquia de Localizações**. |
     | **País** | Digite `USA`. |
     | **Cidade** | Digite `Seattle`. |
     | **Prédio** | Digite `Pacific Science Center`. |

   * Para o Resfriador 01.3:

     | | |
     | --- | --- |
     | **Tipo** | Selecione **Resfriador**. |
     | **Descrição** | Digite `Instance for Chiller-01.3`. |
     | **Hierarquias** | Selecione **Hierarquia de Localizações**. |
     | **País** | Digite `USA`. |
     | **Cidade** | Digite `New York`. |
     | **Prédio** | Digite `Empire State Building`. |

1. Acesse a guia **Analisar** e atualize a página. Expanda todos os níveis de hierarquia para encontrar a série temporal.

   [![A guia Analisar](media/v2-update-provision/define-twelve.png)](media/v2-update-provision/define-twelve.png#lightbox)

1. Para explorar a série temporal na última hora, altere **Tempos Rápidos** para **Última Hora**:

    [![A caixa Tempos Rápidos, com a Última Hora selecionada](media/v2-update-provision/define-thirteen-explore.png)](media/v2-update-provision/define-thirteen-explore.png#lightbox)

1. Selecione a série temporal em **Centro de Ciência do Pacífico** e selecione **Mostrar Umidade Máxima**.

    [![Série temporal selecionada com a seleção de menu Mostrar Umidade Máxima](media/v2-update-provision/define-fourteen-show-max.png)](media/v2-update-provision/define-fourteen-show-max.png#lightbox)

1. A série temporal de **Umidade Máxima** com um tamanho de intervalo igual a **1 minuto** será aberta. Selecione uma região para filtrar um intervalo. Em seguida, clique com o botão direito do mouse e selecione **Aplicar Zoom** para analisar os eventos do período:

   [![Intervalo selecionado com o comando Aplicar Zoom em um menu de atalho](media/v2-update-provision/define-fifteen-filter.png)](media/v2-update-provision/define-fifteen-filter.png#lightbox)

1. Selecione também uma região e, em seguida, clique com o botão direito do mouse para ver os detalhes do evento:

   [![Lista detalhada de eventos](media/v2-update-provision/define-eighteen.png)](media/v2-update-provision/define-eighteen.png#lightbox)

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu como:  

> [!div class="checklist"]
> * Criar e usar um acelerador de simulação de dispositivo.
> * Crie um ambiente PAYG de Versão Prévia do Azure Time Series Insights.
> * Conecte o ambiente de Versão Prévia do Azure Time Series Insights a um hub de eventos.
> * Executar uma amostra do acelerador de solução para transmitir dados para o ambiente da Versão Prévia do Azure Time Series Insights.
> * Execute uma análise básica dos dados.
> * Definir uma hierarquia e um tipo de Modelo de Série Temporal e associá-los às suas instâncias.

Agora que você sabe como criar seu próprio ambiente de Versão Prévia do Azure Time Series Insights, saiba mais sobre os principais conceitos no Azure Time Series Insights.

Leia mais sobre a configuração de armazenamento do Azure Time Series Insights:

> [!div class="nextstepaction"]
> [Armazenamento e entrada da Versão Prévia do Azure Time Series Insights](./time-series-insights-update-storage-ingress.md)

Saiba mais sobre Modelos do Time Series:

> [!div class="nextstepaction"]
> [Modelagem de dados da Versão Prévia do Azure Time Series Insights](./time-series-insights-update-tsm.md)