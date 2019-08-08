---
title: Adicionar uma origem de evento dos Hubs de Eventos ao Azure Time Series Insights | Microsoft Docs
description: Este artigo descreve como adicionar uma origem do evento que está conectada a Hubs de Eventos do Azure ao ambiente do Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: 8403c8fbc4faf35e0ccd3c87347e88a46f0769ff
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2019
ms.locfileid: "68854469"
---
# <a name="add-an-event-hub-event-source-to-your-time-series-insights-environment"></a>Adicionar uma origem de evento do hub de eventos ao ambiente do Time Series Insights

Este artigo descreve como usar o portal do Azure para adicionar uma origem do evento que leia de um Hubs de Eventos do Azure para o ambiente do Azure Time Series Insights.

> [!NOTE]
> As etapas descritas neste artigo se aplicam aos ambientes Time Series Insights GA e Time Series Insights Preview.

## <a name="prerequisites"></a>Pré-requisitos

- Crie um ambiente de Time Series Insights conforme descrito em [criar um ambiente de Azure Time Series insights](./time-series-insights-update-create-environment.md).
- Crie um hub de eventos. Consulte [criar um namespace de hubs de eventos e um hub de eventos usando o portal do Azure](../event-hubs/event-hubs-create.md).
- O hub de eventos deve ter eventos de mensagem ativos enviados a ele. Saiba como [enviar eventos para os hubs de eventos do Azure usando o .NET Framework](../event-hubs/event-hubs-dotnet-framework-getstarted-send.md).
- Crie um grupo de consumidores dedicado no hub de eventos do qual o ambiente do Time Series Insights possa consumir. Cada origem do evento do Time Series Insights deve ter seu próprio grupo de consumidores dedicado que não esteja compartilhado com nenhum outro consumidor. Se vários leitores consumirem eventos do mesmo grupo de consumidores, provavelmente, todos os leitores verão falhas. Há um limite de 20 grupos de consumidores por Hub de Eventos. Para obter detalhes, confira o [Guia de programação dos Hubs de Eventos](../event-hubs/event-hubs-programming-guide.md).

### <a name="add-a-consumer-group-to-your-event-hub"></a>Adicionar um grupo de consumidores ao hub de eventos

Aplicativos usam grupos de consumidores para efetuar pull dos dados de Hubs de Eventos do Azure. Para ler dados de forma confiável do seu hub de eventos, forneça um grupo de consumidores dedicado que é usado somente por esse ambiente de Time Series Insights.

Para adicionar um novo grupo de consumidores ao seu hub de eventos:

1. No portal do Azure, localize e abra o hub de eventos.

1. Em **Entidades**, selecione **Grupos de consumidores** e selecione **Grupo de Consumidores**.

   [![Hub de eventos-adicionar um grupo de consumidores](media/time-series-insights-how-to-add-an-event-source-eventhub/5-event-hub-consumer-group.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/5-event-hub-consumer-group.png#lightbox)

1. Na página **Grupos de consumidores**, insira um novo valor exclusivo para **Nome**.  Use esse mesmo nome ao criar uma nova fonte de evento no ambiente do Time Series Insights.

1. Selecione **Criar**.

## <a name="add-a-new-event-source"></a>Adicionar uma nova origem do evento

1. Entre no [Portal do Azure](https://portal.azure.com).

1. Localize seu ambiente Time Series Insights existente. No menu à esquerda, selecione **Todos os recursos** e, em seguida, selecione o seu ambiente do Time Series Insights.

1. Em **Topologia do Ambiente**, selecione **Origens dos Eventos** e, em seguida, selecione **Adicionar**.

   [![Em origens do evento, selecione o botão Adicionar](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-sources.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/1-event-sources.png#lightbox)

1. Insira um valor para **Nome da fonte do evento** que seja exclusivo para esse ambiente do Time Series Insights, como **fluxo-de-evento**.

1. Para **Origem**, selecione **Hub de Eventos**.

1. Selecione os valores apropriados para **Opção de importação**:
   - Se você tiver um hub de eventos existente em uma de suas assinaturas, selecione **Usar Hub de Eventos de assinaturas disponíveis**. Essa opção é a abordagem mais fácil.

       [![No painel nova origem do evento, insira valores para os três primeiros parâmetros](media/time-series-insights-how-to-add-an-event-source-eventhub/2-import-option.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/2-import-option.png#lightbox)


       [![Detalhes de assinatura e Hub de eventos](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/3-new-event-source.png#lightbox)

     A tabela a seguir descreve as propriedades necessárias para a opção **Usar Hub de Eventos de assinaturas disponíveis**:

     | Propriedade | Descrição |
     | --- | --- |
     | ID da assinatura | Selecione a assinatura na qual esse hub de eventos foi criado.
     | Namespace do Barramento de Serviço | Selecione o namespace do Barramento de Serviço do Azure que contém o hub de eventos.
     | Nome do Hub de Eventos | Selecione o nome do hub de eventos.
     | Nome da política do hub de eventos | Selecione a política de acesso compartilhado. Você pode criar a política de acesso compartilhado na guia **Configurar** do hub de eventos. Cada política de acesso compartilhado tem um nome, as permissões definidas por você e as chaves de acesso. A política de acesso compartilhado para a origem do evento *deve* ter permissões de **leitura**.
     | Chave de política do hub de eventos | O valor da chave pode ser preenchido previamente.
     | Grupo de consumidores do hub de eventos | O grupo de consumidor que lê os eventos do hub de eventos. Recomendamos que você use um grupo de consumidores dedicado para a origem do evento. |
     | Formato de serialização de evento | No momento, JSON é único o formato de serialização disponível. As mensagens de evento devem estar nesse formato ou os dados não podem ser lidos. |
     | Nome da propriedade timestamp | Para determinar esse valor, você precisa entender o formato da mensagem dos dados de mensagem enviados para o hub de eventos. Esse valor é o **nome** da propriedade de evento específica nos dados da mensagem que você deseja usar como o carimbo de data/hora do evento. O valor diferencia maiúsculas de minúsculas. Se deixado em branco, o **tempo de enfileiramento do evento** na origem do evento será usado como o carimbo de data/hora do evento. |

    - Se o hub de eventos for externo às suas assinaturas ou se você quiser selecionar opções avançadas, selecione **Fornecer configurações do Hub de Eventos manualmente**.

      A tabela a seguir descreve as propriedades necessárias para a opção **Fornecer configurações do Hub de Eventos manualmente**:
 
      | Propriedade | Descrição |
      | --- | --- |
      | ID da assinatura | A assinatura na qual esse hub de eventos foi criado.
      | Grupo de recursos | O grupo de recursos no qual este hub de eventos foi criado.
      | Namespace do Barramento de Serviço | Um namespace Barramento de Serviço é um contêiner para um conjunto de entidades de mensagens. Ao criar um novo hub de eventos, você também criou um namespace Barramento de Serviço.
      | Nome do Hub de Eventos | O nome do seu hub de eventos. Quando você criou o hub de eventos, também deu a ele um nome específico.
      | Nome da política do hub de eventos | A política de acesso compartilhado. Você pode criar uma política de acesso compartilhado na guia **Configurar** do hub de eventos. Cada política de acesso compartilhado tem um nome, as permissões definidas por você e as chaves de acesso. A política de acesso compartilhado para a origem do evento *deve* ter permissões de **leitura**.
      | Chave de política do hub de eventos | A chave de acesso compartilhado usada para autenticar o acesso ao namespace do Barramento de Serviço. Insira aqui a chave primária ou secundária.
      | Grupo de consumidores do hub de eventos | O grupo de consumidor que lê os eventos do hub de eventos. Recomendamos que você use um grupo de consumidores dedicado para a origem do evento.
      | Formato de serialização de evento | No momento, JSON é único o formato de serialização disponível. As mensagens de evento devem estar nesse formato ou os dados não podem ser lidos. |
      | Nome da propriedade timestamp | Para determinar esse valor, você precisa entender o formato da mensagem dos dados de mensagem enviados para o hub de eventos. Esse valor é o **nome** da propriedade de evento específica nos dados da mensagem que você deseja usar como o carimbo de data/hora do evento. O valor diferencia maiúsculas de minúsculas. Se deixado em branco, o **tempo de enfileiramento do evento** na origem do evento será usado como o carimbo de data/hora do evento. |

1. Adicione o nome de grupo de consumidores do Time Series Insights dedicado que você adicionou ao seu hub de eventos.

1. Selecione **Criar**.

   [![Selecione criar](media/time-series-insights-how-to-add-an-event-source-eventhub/4-create-button.png)](media/time-series-insights-how-to-add-an-event-source-eventhub/4-create-button.png#lightbox)

   Depois que a origem do evento é criada, o Time Series Insights inicia automaticamente os dados de streaming para o seu ambiente.

## <a name="next-steps"></a>Próximas etapas

* [Definir as políticas de acesso de dados](time-series-insights-data-access.md) para proteger os dados.

* [Enviar eventos](time-series-insights-send-events.md) para a origem do evento.

* Acesse seu ambiente no [explorer do Time Series Insights](https://insights.timeseries.azure.com).
