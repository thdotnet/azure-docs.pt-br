---
title: Entender mensagens de nuvem para o dispositivo Hub IoT do Azure| Microsoft Docs
description: Este guia de desenvolvedor discute como usar mensagens de nuvem para dispositivo com hub IoT. Ele inclui informações sobre as opções de configuração e o ciclo de vida da mensagem.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 03/15/2018
ms.openlocfilehash: 0fc1b65a4ba1c8a3d76b48206d6a4703035e05bc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055339"
---
# <a name="send-cloud-to-device-messages-from-an-iot-hub"></a>Enviar mensagens da nuvem para dispositivo de um hub IoT

Para enviar notificações unidirecionais para um aplicativo de dispositivo da sua solução back-end, envie mensagens da nuvem para dispositivo do hub IoT para seu dispositivo. Para uma discussão sobre outras opções de nuvem para dispositivo com suporte pelo IoT Hub do Azure, consulte [diretrizes de comunicações da nuvem para dispositivo](iot-hub-devguide-c2d-guidance.md).

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Você envia mensagens de nuvem para dispositivo por meio de um ponto de extremidade do serviço voltado *mensagens/devicebound*. Um dispositivo recebe as mensagens por meio de um ponto de extremidade específico do dispositivo */Devices/{iddodispositivo}/Files mensagens/devicebound*.

Para cada mensagem de nuvem para o dispositivo em um único dispositivo de destino, o hub IoT define o **à** propriedade */Devices/{iddodispositivo}/Files mensagens/devicebound*.

Cada fila de dispositivo contém, no máximo, 50 mensagens da nuvem para o dispositivo. Para tentar enviar mais mensagens para o mesmo dispositivo resultará em erro.

## <a name="the-cloud-to-device-message-life-cycle"></a>O ciclo de vida da mensagem da nuvem para dispositivo

Para garantir a entrega de mensagens at-least-once, hub IoT mantém as mensagens da nuvem para o dispositivo em filas por dispositivo. Para o hub IoT remover mensagens da fila, os dispositivos devem reconhecer explicitamente *conclusão*. Essa abordagem garante a resiliência contra conectividade e falhas de dispositivo.

O grafo de estado do ciclo de vida é exibido no diagrama a seguir:

![Ciclo de vida da mensagem da nuvem para dispositivo](./media/iot-hub-devguide-messages-c2d/lifecycle.png)

Quando o serviço hub IoT envia uma mensagem para um dispositivo, o serviço define o estado da mensagem para *enfileiradas*. Quando um dispositivo deseja *receber* uma mensagem, o hub IoT *bloqueios* a mensagem, definindo o estado como *invisível*. Esse estado permite que outros threads no dispositivo para começar a receber outras mensagens. Quando um thread de dispositivo conclui o processamento de uma mensagem, ele notifica o hub IoT *Concluindo* a mensagem. O hub IoT, em seguida, define o estado como *concluído*.

Um dispositivo também pode:

* *Rejeitar* a mensagem, que faz com que o hub IoT para defini-lo o *mensagens mortas* estado. Dispositivos que se conectam através do protocolo de transporte de telemetria de enfileiramento de mensagens (MQTT) mensagem não é possível rejeitar mensagens da nuvem para dispositivo.

* *Abandonar* a mensagem, que faz com que o hub IoT para colocar a mensagem de volta na fila, com o estado definido como *enfileiradas*. Dispositivos que se conectam através do protocolo MQTT não podem abandonar mensagens da nuvem para dispositivo.

Um thread pode falhar ao processar uma mensagem sem notificar o hub IoT. Nesse caso, as mensagens passam automaticamente do *invisível* estado de volta para o *enfileiradas* estado após um *visibilidade* tempo limite (ou *bloqueio* tempo limite). O valor padrão desse tempo limite é um minuto.

O **máximo de contagem de entrega** propriedade no hub IoT determina o número máximo de vezes que uma mensagem pode transitar entre os *enfileiradas* e *invisível* estados. Após esse número de transições, o hub IoT define o estado da mensagem a ser *mensagens mortas*. Da mesma forma, o hub IoT define o estado de uma mensagem para *mensagens mortas* após o tempo de expiração. Para obter mais informações, consulte [tempo de vida](#message-expiration-time-to-live).

O [como enviar mensagens da nuvem para dispositivo com o IoT Hub](iot-hub-csharp-csharp-c2d.md) artigo mostra como enviar mensagens da nuvem para dispositivo de nuvem e recebê-las em um dispositivo.

Um dispositivo normalmente conclui uma mensagem de nuvem para o dispositivo quando a perda da mensagem não afeta a lógica do aplicativo. Um exemplo disso seria quando o dispositivo persistiu o conteúdo da mensagem localmente ou executou com êxito uma operação. A mensagem também poderia portar informações transitórias cuja perda não afeta a funcionalidade do aplicativo. Às vezes, para tarefas de execução longa, você pode:

* Conclua a mensagem da nuvem para dispositivo após o dispositivo persistiu a descrição da tarefa no armazenamento local.

* Notificar o back-end da solução com uma ou mais mensagens de dispositivo para a nuvem em vários estágios de progresso da tarefa.

## <a name="message-expiration-time-to-live"></a>Expiração da mensagem (vida útil)

Todas as mensagens da nuvem para o dispositivo têm um tempo de expiração. Esse tempo é definido por um destes procedimentos:

* O **ExpiryTimeUtc** propriedade no serviço
* O hub IoT, usando o padrão *tempo de vida* que é especificado como uma propriedade do hub IoT

Consulte [Opções de configuração da nuvem para o dispositivo](#cloud-to-device-configuration-options).

Uma maneira comum para tirar proveito da expiração de uma mensagem e evitar o envio de mensagens para dispositivos desconectados é definir curto *tempo de vida* valores. Essa abordagem proporciona o mesmo resultado que manter o estado de conexão do dispositivo, mas é mais eficiente. Quando você solicita confirmações de mensagem, o hub IoT notifica quais dispositivos:

* São capazes de receber mensagens.
* Não estão online ou falharam.

## <a name="message-feedback"></a>Comentários da mensagem

Quando você envia uma mensagem da nuvem para dispositivo, o serviço pode solicitar a entrega de comentário por mensagem sobre o estado final dessa mensagem. Você pode fazer isso definindo a **iothub-ack** propriedade do aplicativo na mensagem de nuvem para o dispositivo que está sendo enviada para um dos quatro valores a seguir:

| Valor da propriedade ACK | Comportamento |
| ------------ | -------- |
| Nenhum     | O hub IoT não gera uma mensagem de comentários (comportamento padrão). |
| positivo | Se a mensagem da nuvem para dispositivo alcançar o *concluído* de estado, o hub IoT gera uma mensagem de comentários. |
| Negativo | Se a mensagem da nuvem para dispositivo alcançar o *mensagens mortas* de estado, o hub IoT gera uma mensagem de comentários. |
| completo     | O hub IoT gera uma mensagem de comentários em ambos os casos. |

Se o **Ack** valor estiver *completo*e você não receber uma mensagem de comentários, isso significa que a mensagem de comentários expirou. O serviço não pode saber o que aconteceu com a mensagem original. Na prática, um serviço deve garantir que possa processar os comentários antes que eles expirem. O tempo de expiração máximo é dois dias, tempo suficiente para obter o serviço em execução novamente caso ocorra uma falha.

Conforme explicado em [pontos de extremidade](iot-hub-devguide-endpoints.md), o hub IoT fornece comentários por meio de um ponto de extremidade voltados para serviço */messages/servicebound/feedback*, como mensagens. A semântica de recebimento dos comentários é a mesma das mensagens da nuvem para o dispositivo. Sempre que possível, os comentários de mensagem são feitos em lotes em uma única mensagem, com o seguinte formato:

| Propriedade     | DESCRIÇÃO |
| ------------ | ----------- |
| EnqueuedTime | Um carimbo de hora que indica quando a mensagem de comentários foi recebida pelo hub |
| UserId       | `{iot hub name}` |
| ContentType  | `application/vnd.microsoft.iothub.feedback.json` |

O corpo é uma matriz de registros serializada em JSON, cada um com as seguintes propriedades:

| Propriedade           | DESCRIÇÃO |
| ------------------ | ----------- |
| EnqueuedTimeUtc    | Um carimbo de hora que indica quando ocorreu o resultado da mensagem (por exemplo, o hub recebeu a mensagem de comentários ou a mensagem original expirou) |
| OriginalMessageId  | O *MessageId* da mensagem de nuvem para dispositivo ao qual está associado a essa informação de comentários |
| StatusCode         | Uma cadeia de caracteres necessária, usada em mensagens de comentários que são geradas pelo hub IoT: <br/> *Êxito* <br/> *expirado* <br/> *DeliveryCountExceeded* <br/> *Rejected* <br/> *Limpo* |
| DESCRIÇÃO        | Valores de cadeia de caracteres *StatusCode* |
| deviceId           | O *DeviceId* do dispositivo de destino da mensagem de nuvem para dispositivo ao qual esse comentário se relaciona |
| DeviceGenerationId | O *DeviceGenerationId* do dispositivo de destino da mensagem de nuvem para dispositivo ao qual esse comentário se relaciona |

Para a mensagem da nuvem para dispositivo correlacionar seus comentários com a mensagem original, o serviço deve especificar uma *MessageId*.

O corpo de uma mensagem de comentários é mostrado no código a seguir:

```json
[
  {
    "OriginalMessageId": "0987654321",
    "EnqueuedTimeUtc": "2015-07-28T16:24:48.789Z",
    "StatusCode": 0,
    "Description": "Success",
    "DeviceId": "123",
    "DeviceGenerationId": "abcdefghijklmnopqrstuvwxyz"
  },
  {
    ...
  },
  ...
]
```

## <a name="cloud-to-device-configuration-options"></a>Opções de configuração da nuvem para o dispositivo

Cada Hub IoT expõe as seguintes opções de configuração para mensagens de nuvem para o dispositivo:

| Propriedade                  | DESCRIÇÃO | Intervalo e padrão |
| ------------------------- | ----------- | ----------------- |
| defaultTtlAsIso8601       | TTL padrão para mensagens da nuvem para dispositivo | Intervalo ISO_8601 de até 2 dias (mínimo 1 minuto); padrão: 1 hora |
| maxDeliveryCount          | Contagem máxima de entrega para filas de nuvem para o dispositivo por dispositivo | 1 a 100; padrão: 10 |
| feedback.ttlAsIso8601     | Retenção de mensagens de comentários de associação de serviço | Intervalo ISO_8601 de até 2 dias (mínimo 1 minuto); padrão: 1 hora |
| feedback.maxDeliveryCount | Contagem máxima de entrega para a fila de comentários | 1 a 100; padrão: 100 |

Para obter mais informações sobre como configurar essas opções de configuração, consulte [Criar Hubs IoT](iot-hub-create-through-portal.md).

## <a name="next-steps"></a>Próximas etapas

Para obter informações sobre os SDKs que você pode usar para receber mensagens da nuvem para dispositivo, consulte [SDKs do IoT do Azure](iot-hub-devguide-sdks.md).

Para experimentar a recepção de mensagens de nuvem para dispositivo, consulte o tutorial [Enviar de nuvem para dispositivo ](iot-hub-csharp-csharp-c2d.md).
