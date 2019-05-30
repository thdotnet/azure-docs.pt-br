---
title: Visão geral dos aprimoramentos de mensagem do IoT Hub do Azure
description: Visão geral dos aprimoramentos de mensagem para mensagens do IoT Hub do Azure
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/10/2019
ms.author: robinsh
ms.openlocfilehash: 2de945d2ef1d87b47ea81aac034916466d3c9bda
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66258843"
---
# <a name="message-enrichments-for-device-to-cloud-iot-hub-messages-preview"></a>Aprimoramentos de mensagem para mensagens do IoT Hub do dispositivo para nuvem (visualização)

*Aprimoramentos da mensagem* é a capacidade do IoT Hub *carimbo de data /* mensagens com informações adicionais antes que as mensagens são enviadas ao ponto de extremidade designado. Um motivo para usar os aprimoramentos de mensagem é para incluir dados que podem ser usados para simplificar o processamento de downstream. Por exemplo, enriquecer mensagens de telemetria do dispositivo com uma marca de gêmeo de dispositivo pode reduzir a carga sobre os clientes façam chamadas de API para obter essas informações do dispositivo gêmeo.

![Aprimoramentos de mensagem de fluxo](./media/iot-hub-message-enrichments-overview/message-enrichments-flow.png)

O enriquecimento da mensagem tem três elementos principais:

* Enriquecimento nome ou chave

* Um valor

* Um ou mais [pontos de extremidade](iot-hub-devguide-endpoints.md) para que o enriquecimento deve ser aplicado.

A chave pode ser qualquer cadeia de caracteres.

O valor pode ser qualquer um dos exemplos a seguir:

* Qualquer cadeia de caracteres estática. Os valores dinâmicos, como condições, lógica, operações e funções não são permitidos. Por exemplo, se você desenvolver um aplicativo SaaS que é usado por vários clientes, você pode atribuir um identificador para cada cliente e disponibilizar esse identificador no aplicativo. Quando o aplicativo é executado, o IoT Hub será carimbar o dispositivo de mensagens de telemetria com o identificador do cliente, tornando possível processar as mensagens de forma diferente para cada cliente.

* Informações do dispositivo gêmeo, como o seu caminho. Exemplos seriam *$twin.tags.field* e *$twin.tags.latitude*.

* O nome do hub IoT enviar a mensagem. Esse valor é *$iothubname*.

## <a name="applying-enrichments"></a>Aprimoramentos de aplicação

As mensagens podem vir de qualquer fonte de dados com suporte pelo [roteamento de mensagens do IoT Hub](iot-hub-devguide-messages-d2c.md), incluindo os exemplos a seguir:

* Telemetria do dispositivo, como temperatura ou pressão
* notificações de alteração de gêmeo do dispositivo – as alterações no dispositivo gêmeo
* eventos de ciclo de vida do dispositivo, como quando o dispositivo é criado ou excluído

Você pode adicionar aprimoramentos para mensagens que serão o ponto de extremidade interno de um IoT Hub, ou que estão sendo roteadas para os pontos de extremidade personalizados, como o armazenamento de BLOBs do Azure, uma fila do barramento de serviço ou um tópico do barramento de serviço.

Aprimoramentos são aplicados por ponto de extremidade. Se você especificar cinco aprimoramentos ser marcada para um ponto de extremidade específico, todas as mensagens enviadas para esse ponto de extremidade são marcadas com os mesmos aprimoramentos de cinco.

Para ver como experimentar os aprimoramentos de mensagem, consulte o [tutorial de aprimoramentos de mensagem](tutorial-message-enrichments.md)

## <a name="limitations"></a>Limitações

* Você pode adicionar até 10 aprimoramentos por IoT Hub para os hubs na camada standard ou básica. Para os Hubs IoT na camada gratuita, você pode adicionar até 2 aprimoramentos.

* Em alguns casos, se você estiver aplicando um aprimoramento com um valor definido como uma marca ou propriedade no dispositivo gêmeo, o valor será marcado como um valor de cadeia de caracteres. Por exemplo, se um valor de enriquecimento é definido como $twin.tags.field, as mensagens serão carimbadas com a cadeia de caracteres "$twin.tags.field" em vez do valor do campo do gêmeo. Isso acontece nos seguintes casos:

   * O IoT Hub está na camada básica. Hubs de IoT de camada básica não dão suporte a dispositivos gêmeos.

   * O IoT Hub está na camada standard, mas o dispositivo que envia a mensagem não tem nenhum dispositivo gêmeo.

   * O IoT Hub está na camada standard, mas o caminho do dispositivo gêmeo usado para o valor o enriquecimento de não existe. Por exemplo, se o valor de enriquecimento é definido como $twin.tags.location e o dispositivo gêmeo não tem uma propriedade de local nas marcas, a mensagem é marcada com a cadeia de caracteres "$twin.tags.location". 

* Atualizações para um dispositivo gêmeo podem levar até cinco minutos para ser refletida no valor enriquecimento correspondente.

* O tamanho total de mensagens, incluindo aprimoramentos, não pode exceder 256 KB. Se um tamanho de mensagem exceder 256 KB, o IoT Hub descartará a mensagem. Você pode usar [métricas do IoT Hub](iot-hub-metrics.md) para identificar e depurar erros quando as mensagens são descartadas. Por exemplo, você pode monitorar d2c.telemetry.egress.invalid.

## <a name="pricing"></a>Preços

Aprimoramentos de mensagem estão disponíveis sem nenhum custo adicional. No momento, você será cobrado quando você envia uma mensagem para um IoT Hub. Você é cobrado apenas uma vez para a mensagem, mesmo se a mensagem é enviada para vários pontos de extremidade.

## <a name="availability"></a>Disponibilidade

Esse recurso está disponível em visualização e está disponível em todas as regiões, exceto o Leste dos EUA, oeste dos EUA, Europa Ocidental, [do Azure governamental](/azure-government/documentation-government-welcome.md), [do Azure na China 21Vianet](/azure/china/china-welcome.md), e [Azure Alemanha](https://azure.microsoft.com/global-infrastructure/germany/).

## <a name="next-steps"></a>Próximas etapas

Confira estes artigos para obter mais informações sobre como rotear mensagens para um IoT Hub:

* [Usar o roteamento de mensagens do IoT Hub para enviar mensagens do dispositivo para nuvem para pontos de extremidade diferentes](iot-hub-devguide-messages-d2c.md)

* [Tutorial: Roteamento do IoT Hub](tutorial-routing.md)