---
title: Noções básicas do suporte do Azure IoT Hub AMQP | Microsoft Docs
description: Guia do desenvolvedor – suporte para dispositivos que se conectam ao IoT Hub voltados para o dispositivo e serviço voltado para os pontos de extremidade usando o protocolo AMQP. Inclui informações sobre suporte interno a AMQP nos SDKs do dispositivo IoT do Azure.
author: rezasherafat
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: rezas
ms.openlocfilehash: d256faa42161e276e165f95c944b9f58ac4a8927
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66297411"
---
# <a name="communicate-with-your-iot-hub-using-the-amqp-protocol"></a>Se comunicar com o hub IoT usando o protocolo AMQP

Dá suporte ao IoT Hub [AMQP versão 1.0](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf) para fornecer uma variedade de funcionalidades por meio de pontos de extremidade voltados para o dispositivo e para o serviço. Este documento descreve o uso de clientes AMQP para se conectar ao IoT Hub para usar a funcionalidade do IoT Hub.

## <a name="service-client"></a>Cliente de serviço

### <a name="connection-and-authenticating-to-iot-hub-service-client"></a>Conexão e autenticação no IoT Hub (cliente de serviço)
Para se conectar ao IoT Hub usando o AMQP, um cliente pode usar o [segurança com base em declarações (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) ou [Simple Authentication and Security Layer SASL () de autenticação](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer).

As seguintes informações são necessárias para o cliente de serviço:

| Informações | Value | 
|-------------|--------------|
| Nome do host do Hub IoT | `<iot-hub-name>.azure-devices.net` |
| Nome da chave | `service` |
| Chave de acesso | Chave primária ou secundária associada ao serviço |
| Assinatura de acesso compartilhado | SAS de curta duração no seguinte formato: `SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}` (o código para gerar essa assinatura pode ser encontrado [aqui](./iot-hub-devguide-security.md#security-token-structure)).


O trecho de código abaixo usa [uAMQP biblioteca em Python](https://github.com/Azure/azure-uamqp-python) para se conectar ao hub IoT por meio de um link do remetente.

```python
import uamqp
import urllib
import time

# Use generate_sas_token implementation available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '<operation-link-name>' # e.g., '/messages/devicebound'

username = '{policy_name}@sas.root.{iot_hub_name}'.format(iot_hub_name=iot_hub_name, policy_name=policy_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

# Create a send or receive client
send_client = uamqp.SendClient(uri, debug=True)
receive_client = uamqp.ReceiveClient(uri, debug=True)
```

### <a name="invoking-cloud-to-device-messages-service-client"></a>Invocar mensagens de nuvem para o dispositivo (cliente de serviço)
A troca de mensagens de nuvem para dispositivo entre o serviço e o IoT Hub, bem como entre o dispositivo e IoT Hub é descrita [aqui](iot-hub-devguide-messages-c2d.md). O cliente do serviço usa dois links descritos abaixo para enviar mensagens e receba comentários para mensagens enviadas anteriormente de dispositivos.

| Criado por | Tipo de link | Caminho do link | DESCRIÇÃO |
|------------|-----------|-----------|-------------|
| Serviço | Link do remetente | `/messages/devicebound` | Mensagens C2D destinadas a dispositivos são enviadas para este link pelo serviço. As mensagens enviadas por esse link têm suas `To` propriedade definida como o caminho do link de destinatário do dispositivo de destino: ou seja, `/devices/<deviceID>/messages/devicebound`. |
| Serviço | Link receptor | `/messages/serviceBound/feedback` | Mensagens de comentários abandono de conclusão e rejeição provenientes de dispositivos recebidos neste link pelo serviço. Para obter mais informações sobre mensagens de comentários, consulte [aqui](./iot-hub-devguide-messages-c2d.md#message-feedback). |

O trecho de código a seguir demonstra como criar uma mensagem C2D e enviá-lo para um dispositivo usando [uAMQP biblioteca em Python](https://github.com/Azure/azure-uamqp-python).

```python
import uuid
# Create a message and set message property 'To' to the devicebound link on device
msg_id = str(uuid.uuid4())
msg_content = b"Message content goes here!"
device_id = '<device-id>'
to = '/devices/{device_id}/messages/devicebound'.format(device_id=device_id)
ack = 'full' # Alternative values are 'positive', 'negative', and 'none'
app_props = { 'iothub-ack': ack }
msg_props = uamqp.message.MessageProperties(message_id=msg_id, to=to)
msg = uamqp.Message(msg_content, properties=msg_props, application_properties=app_props)

# Send the message using the send client created and connected IoT Hub earlier
send_client.queue_message(msg)
results = send_client.send_all_messages()

# Close the client if not needed
send_client.close()
```

Para receber comentários, o cliente de serviço cria um link do receptor. O trecho de código a seguir demonstra como fazer isso usando [uAMQP biblioteca em Python](https://github.com/Azure/azure-uamqp-python).

```python
import json

operation = '/messages/serviceBound/feedback'

# ...
# Recreate the URI using the feedback path above and authenticate
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
batch = receive_client.receive_message_batch(max_batch_size=10)
for msg in batch:
  print('received a message')
  # Check content_type in message property to identify feedback messages coming from device
  if msg.properties.content_type == 'application/vnd.microsoft.iothub.feedback.json':
    msg_body_raw = msg.get_data()
    msg_body_str = ''.join(msg_body_raw)
    msg_body = json.loads(msg_body_str)
    print(json.dumps(msg_body, indent=2))
    print('******************')
    for feedback in msg_body:
      print('feedback received')
      print('\tstatusCode: ' + str(feedback['statusCode']))
      print('\toriginalMessageId: ' + str(feedback['originalMessageId']))
      print('\tdeviceId: ' + str(feedback['deviceId']))
      print
  else:
    print('unknown message:', msg.properties.content_type)
```

Como mostrado acima, uma mensagem de comentários C2D tem o tipo de conteúdo do `application/vnd.microsoft.iothub.feedback.json` e propriedades em seu corpo JSON podem ser usadas para inferir o status de entrega da mensagem original:
* Chave `statusCode` comentários corpo tem um destes valores: `['Success', 'Expired', 'DeliveryCountExceeded', 'Rejected', 'Purged']`.
* Chave `deviceId` comentários corpo tem a ID do dispositivo de destino.
* Chave `originalMessageId` comentários corpo tem a ID da mensagem C2D original enviada pelo serviço. Isso pode ser usado para correlacionar seus comentários para mensagens C2D.

### <a name="receive-telemetry-messages-service-client"></a>Receber mensagens de telemetria (cliente de serviço)
Por padrão, o IoT Hub armazena mensagens de telemetria do dispositivo ingeridos em um hub de eventos internos. Seu cliente de serviço pode usar o protocolo AMQP para receber os eventos armazenados.

Para essa finalidade, o cliente do serviço primeiro precisa se conectar ao ponto de extremidade do IoT Hub e receber um endereço de redirecionamento para os Hubs de eventos internos. Cliente de serviço, em seguida, usa o endereço fornecido para conectar-se ao hub de eventos internos.

Em cada etapa, o cliente precisa apresentar as seguintes partes de informações:
* Credenciais de serviço válido (token SAS de serviço).
* Um caminho bem formatado para a partição de grupo de consumidor pretende recuperar mensagens. Para uma ID de partição e de grupo de consumidor de determinado, o caminho tem o seguinte formato: `/messages/events/ConsumerGroups/<consumer_group>/Partitions/<partition_id>` (o grupo de consumidores padrão é `$Default`).
* Um predicado de filtragem opcional para designar um ponto de partida na partição (Isso pode ser na forma de um carimbo de hora de número, offset ou enfileiradas de sequência).

O trecho de código abaixo usa [uAMQP biblioteca em Python](https://github.com/Azure/azure-uamqp-python) para demonstrar as etapas acima.

```python
import json
import uamqp
import urllib
import time

# Use generate_sas_token implementation available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '/messages/events/ConsumerGroups/{consumer_group}/Partitions/{p_id}'.format(consumer_group='$Default', p_id=0)

username = '{policy_name}@sas.root.{iot_hub_name}'.format(policy_name=policy_name, iot_hub_name=iot_hub_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

# Optional filtering predicates can be specified using endpiont_filter
# Valid predicates include:
# - amqp.annotation.x-opt-sequence-number
# - amqp.annotation.x-opt-offset
# - amqp.annotation.x-opt-enqueued-time
# Set endpoint_filter variable to None if no filter is needed
endpoint_filter = b'amqp.annotation.x-opt-sequence-number > 2995'

# Helper function to set the filtering predicate on the source URI
def set_endpoint_filter(uri, endpoint_filter=''):
  source_uri = uamqp.address.Source(uri)
  source_uri.set_filter(endpoint_filter)
  return source_uri

receive_client = uamqp.ReceiveClient(set_endpoint_filter(uri, endpoint_filter), debug=True)
try:
  batch = receive_client.receive_message_batch(max_batch_size=5)
except uamqp.errors.LinkRedirect as redirect:
  # Once a redirect error is received, close the original client and recreate a new one to the re-directed address
  receive_client.close()

  sas_auth = uamqp.authentication.SASTokenAuth.from_shared_access_key(redirect.address, policy_name, access_key)
  receive_client = uamqp.ReceiveClient(set_endpoint_filter(redirect.address, endpoint_filter), auth=sas_auth, debug=True)

# Start receiving messages in batches
batch = receive_client.receive_message_batch(max_batch_size=5)
for msg in batch:
  print('*** received a message ***')
  print(''.join(msg.get_data()))
  print('\t: ' + str(msg.annotations['x-opt-sequence-number']))
  print('\t: ' + str(msg.annotations['x-opt-offset']))
  print('\t: ' + str(msg.annotations['x-opt-enqueued-time']))
```

Para uma ID de determinado dispositivo, o IoT Hub usa um hash da ID do dispositivo para determinar qual partição deve armazenar suas mensagens. O trecho de código acima demonstra o recebimento de eventos de uma única partição de tal. No entanto, observe que muitas vezes um aplicativo típico precisará recuperar eventos armazenados em todas as partições do hub de eventos.


## <a name="device-client"></a>Cliente de dispositivo

### <a name="connection-and-authenticating-to-iot-hub-device-client"></a>Conexão e autenticação no IoT Hub (cliente de dispositivo)
Para se conectar ao IoT Hub usando o AMQP, um dispositivo pode usar o [segurança com base em declarações (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) ou [Simple Authentication and Security Layer SASL () de autenticação](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer).

As seguintes informações são necessárias para o cliente de dispositivo:

| Informações | Value | 
|-------------|--------------|
| Nome do host do Hub IoT | `<iot-hub-name>.azure-devices.net` |
| Chave de acesso | Chave primária ou secundária associado ao dispositivo |
| Assinatura de acesso compartilhado | SAS de curta duração no seguinte formato: `SharedAccessSignature sig={signature-string}&se={expiry}&sr={URL-encoded-resourceURI}` (o código para gerar essa assinatura pode ser encontrado [aqui](./iot-hub-devguide-security.md#security-token-structure)).


O trecho de código abaixo usa [uAMQP biblioteca em Python](https://github.com/Azure/azure-uamqp-python) para se conectar ao hub IoT por meio de um link do remetente.

```python
import uamqp
import urllib
import uuid

# Use generate_sas_token implementation available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
device_id = '<device-id>'
access_key = '<primary-or-secondary-key>'
username = '{device_id}@sas.{iot_hub_name}'.format(device_id=device_id, iot_hub_name=iot_hub_name)
sas_token = generate_sas_token('{hostname}/devices/{device_id}'.format(hostname=hostname, device_id=device_id), access_key, None)

operation = '<operation-link-name>' # e.g., '/devices/{device_id}/messages/devicebound'
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
send_client = uamqp.SendClient(uri, debug=True)
```

Os caminhos de link a seguir têm suporte como operações de dispositivo:

| Criado por | Tipo de link | Caminho do link | DESCRIÇÃO |
|------------|-----------|-----------|-------------|
| Dispositivos | Link receptor | `/devices/<deviceID>/messages/devicebound` | Mensagens C2D destinadas a dispositivos são recebidas nesse link por cada dispositivo de destino. |
| Dispositivos | Link do remetente | `/devices/<deviceID>messages/events` | Mensagens D2C enviadas de um dispositivo são enviadas por este link. |
| Dispositivos | Link do remetente | `/messages/serviceBound/feedback` | Comentários da mensagem C2D enviados ao serviço por esse link pelos dispositivos. |


### <a name="receive-c2d-commands-device-client"></a>Receber comandos C2D (cliente de dispositivo)
C2D comandos enviados aos dispositivos chegarem `/devices/<deviceID>/messages/devicebound` link. Dispositivos podem receber essas mensagens em lotes e usar a carga de dados de mensagem, as propriedades da mensagem, anotações ou propriedades do aplicativo na mensagem, conforme necessário.

O trecho de código abaixo usa [uAMQP biblioteca em Python](https://github.com/Azure/azure-uamqp-python) para receber mensagens C2D por um dispositivo.

```python
# ... 
# Create a receive client for the C2D receive link on the device
operation = '/devices/{device_id}/messages/devicebound'.format(device_id=device_id)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
while True:
  batch = receive_client.receive_message_batch(max_batch_size=5)
  for msg in batch:
    print('*** received a message ***')
    print(''.join(msg.get_data()))

    # Property 'to' is set to: '/devices/device1/messages/devicebound',
    print('\tto:                     ' + str(msg.properties.to))

    # Property 'message_id' is set to value provided by the service
    print('\tmessage_id:             ' + str(msg.properties.message_id))

    # Other properties are present if they were provided by the service
    print('\tcreation_time:          ' + str(msg.properties.creation_time))
    print('\tcorrelation_id:         ' + str(msg.properties.correlation_id))
    print('\tcontent_type:           ' + str(msg.properties.content_type))
    print('\treply_to_group_id:      ' + str(msg.properties.reply_to_group_id))
    print('\tsubject:                ' + str(msg.properties.subject))
    print('\tuser_id:                ' + str(msg.properties.user_id))
    print('\tgroup_sequence:         ' + str(msg.properties.group_sequence))
    print('\tcontent_encoding:       ' + str(msg.properties.content_encoding))
    print('\treply_to:               ' + str(msg.properties.reply_to))
    print('\tabsolute_expiry_time:   ' + str(msg.properties.absolute_expiry_time))
    print('\tgroup_id:               ' + str(msg.properties.group_id))

    # Message sequence number in the built-in Event hub
    print('\tx-opt-sequence-number:  ' + str(msg.annotations['x-opt-sequence-number']))
```

### <a name="send-telemetry-messages-device-client"></a>Enviar mensagens de telemetria (cliente de dispositivo)
Mensagens de telemetria também ser enviados sobre AMQP dos dispositivos. O dispositivo pode, opcionalmente, fornecer um dicionário de propriedades do aplicativo, ou várias propriedades, como a ID da mensagem. de mensagem

O trecho de código abaixo usa [uAMQP biblioteca em Python](https://github.com/Azure/azure-uamqp-python) para enviar mensagens D2C de um dispositivo.


```python
# ... 
# Create a send client for the D2C send link on the device
operation = '/devices/{device_id}/messages/events'.format(device_id=device_id)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

send_client = uamqp.SendClient(uri, debug=True)

# Set any of the applicable message properties
msg_props = uamqp.message.MessageProperties()
msg_props.message_id = str(uuid.uuid4())
msg_props.creation_time = None
msg_props.correlation_id = None
msg_props.content_type = None
msg_props.reply_to_group_id = None
msg_props.subject = None
msg_props.user_id = None
msg_props.group_sequence = None
msg_props.to = None
msg_props.content_encoding = None
msg_props.reply_to = None
msg_props.absolute_expiry_time = None
msg_props.group_id = None

# Application properties in the message (if any)
application_properties = { "app_property_key": "app_property_value" }

# Create message
msg_data = b"Your message payload goes here"
message = uamqp.Message(msg_data, properties=msg_props, application_properties=application_properties)

send_client.queue_message(message)
results = send_client.send_all_messages()

for result in results:
    if result == uamqp.constants.MessageState.SendFailed:
        print result
```

## <a name="additional-notes"></a>Observações adicionais
* As conexões AMQP podem ser interrompidas devido à falha ou expiração do token de autenticação (gerado no código) de rede. O cliente do serviço deve lidar com essas situações e restabelecer a conexão e os links se necessário. Para o caso de expiração do token de autenticação, o cliente também de forma proativa pode renovar o token antes de sua expiração para evitar uma queda de conexão.
* Em alguns casos, o cliente deve ser capaz de manipular corretamente os redirecionamentos de link. Consulte a documentação do cliente AMQP sobre como lidar com essa operação.

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre o protocolo AMQP, consulte o [especificação do AMQP v1.0](http://www.amqp.org/sites/amqp.org/files/amqp.pdf).

Para saber mais sobre mensagens do IoT Hub, consulte:

* [Mensagens da nuvem para dispositivo](./iot-hub-devguide-messages-c2d.md)
* [Suporte a protocolos adicionais](iot-hub-protocol-gateway.md)
* [Suporte para o protocolo MQTT](./iot-hub-mqtt-support.md)
