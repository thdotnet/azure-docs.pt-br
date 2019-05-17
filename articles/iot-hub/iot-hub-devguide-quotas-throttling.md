---
title: Entender cotas e limitação do Hub IoT do Azure | Microsoft Docs
description: Guia do desenvolvedor ‑ Descrição das cotas que se aplicam ao Hub IoT e o comportamento de limitação esperado.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/11/2019
ms.openlocfilehash: ea9bea8b314d00db87ad7addacc49a976e0da08e
ms.sourcegitcommit: f013c433b18de2788bf09b98926c7136b15d36f1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/13/2019
ms.locfileid: "65550478"
---
# <a name="reference---iot-hub-quotas-and-throttling"></a>Referência - Cotas e limitação do Hub IoT

## <a name="quotas-and-throttling"></a>Cotas e limitação

Cada assinatura do Azure pode ter no máximo 50 hubs IoT e pelo menos um hub Gratuito.

Cada Hub IoT é provisionado com um determinado número de unidades em uma camada específica. A camada e o número de unidades determinam a cota diária máxima de mensagens que você pode enviar. O tamanho da mensagem usado para calcular a cota diária é de 0,5 KB para um hub de nível gratuito e de 4KB para todos os outros níveis. Para saber mais, confira [Preço do Hub IoT do Azure](https://azure.microsoft.com/pricing/details/iot-hub/).

A camada também determina os limites de limitação que o Hub IoT aplicam em todas as operações.

## <a name="operation-throttles"></a>Restrições de operação

Restrições de operação são limites de taxa aplicados em intervalos de minutos e têm como objetivo evitar o abuso. Eles também são sujeitos aos [a modelagem de tráfego](#traffic-shaping).

A tabela a seguir mostra as limitações impostas. Os valores referem-se a um hub individual.

| Restrição | Gratuito, B1 e S1 | B2 e S2 | B3 e S3 | 
| -------- | ------- | ------- | ------- |
| [Operações de registro de identidade](#identity-registry-operations-throttle) (criar, recuperar, listar, atualizar e excluir) | 1,67/s/unidade (100/min/unidade) | 1,67/s/unidade (100/min/unidade) | 83.33/SEC/Unit (5.000/min/unidade) |
| [Novas conexões de dispositivo](#device-connections-throttle) (esse limite se aplica à taxa de _novas conexões_, não o número total de conexões) | Máximo de 100/s ou 12/s/unidade <br/> Por exemplo, duas unidades de S1 são 2\*12 = 24 novas conexões/s, mas você tem pelo menos 100 novas conexões s em suas unidades. Com nove unidades S1 você tem 108/s novas conexões/s (9\*12) em suas uniddes. | 120 novas conexões/s/unidade | 6.000 novas conexões/s/unidade |
| Envios do dispositivo para a nuvem | Máximo de 100/s ou 12/s/unidade <br/> Por exemplo, duas unidades de S1 são 2\*12 = 24/s, mas você tem pelo menos 100/s em suas unidades. Com nove unidades S1 você tem 108/s (9\*12) em suas unidades. | 120/s/unidade | 6.000/s/unidade |
| Envios de nuvem para dispositivo<sup>1</sup> | 1,67/s/unidade (100/min/unidade) | 1,67/s/unidade (100/min/unidade) | 83.33/SEC/Unit (5.000/min/unidade) |
| Recebimentos de nuvem para dispositivo<sup>1</sup> <br/> (somente quando o dispositivo usar HTTPS)| 16.67/SEC/Unit (1.000/min/unidade) | 16.67/SEC/Unit (1.000/min/unidade) | 833.33/SEC/Unit (50.000/min/unidade) |
| Upload de arquivo | 1,67 notificações de carregamento de arquivo/s/unidade (100/min/unidade) | 1,67 notificações de carregamento de arquivo/s/unidade (100/min/unidade) | arquivo 83,33 notificações de upload/s/unidade (5.000/min/unidade) |
| Métodos diretos<sup>1</sup> | 160KB/seg/unidade<sup>2</sup> | 480KB/seg/unidade<sup>2</sup> | 24MB/seg/unidade<sup>2</sup> | 
| Consultas | 20/min/unidade | 20/min/unidade | 1.000/min/unidade |
| Leituras de (dispositivos e módulos) gêmeos <sup>1</sup> | 100/s | Máximo de 100/s ou 10/s/unidade | 500/s/unidade |
| Atualizações de (dispositivo e módulo) gêmeos <sup>1</sup> | 50/s | Máximo de 50/s ou 5/s/unidade | 250/s/unidade |
| Operações de trabalhos<sup>1</sup> <br/> (criar, atualizar, listar, excluir) | 1,67/s/unidade (100/min/unidade) | 1,67/s/unidade (100/min/unidade) | 83.33/SEC/Unit (5.000/min/unidade) |
| Operações de dispositivo de trabalhos<sup>1</sup> <br/> (atualizar gêmeos, invocar o método direto) | 10/s | Máximo de 10/s ou 1/s/unidade | 50/s/unidade |
| Configurações e implantações de borda <sup>1</sup> <br/> (criar, atualizar, listar, excluir) | 0,33/s/unidade (20/min/unidade) | 0,33/s/unidade (20/min/unidade) | 0,33/s/unidade (20/min/unidade) |
| Taxa de iniciação do fluxo de dispositivo<sup>1</sup> | 5 novos fluxos/s | 5 novos fluxos/s | 5 novos fluxos/s |
| Número máximo de dispositivos conectados simultaneamente fluxos<sup>1</sup> | 50 | 50 | 50 |
| Transferência de dados de fluxo máxima do dispositivo<sup>1</sup> (agregar volume por dia) | 300 MB | 300 MB | 300 MB |

<sup>1</sup>Esse recurso não está disponível na camada básica do Hub IoT. Para obter mais informações, consulte [Como escolher o Hub IoT correto](iot-hub-scaling.md). <br/><sup>2</sup>tamanho do medidor de limitação é de 4 KB.

### <a name="traffic-shaping"></a>Modelagem de tráfego

Para acomodar picos de tráfego, o IoT Hub aceita solicitações acima a limitação por um período limitado. As primeiro algumas dessas solicitações são processadas imediatamente. No entanto, se o número de solicitações continua viole a restrição, o IoT Hub começa colocando as solicitações em uma fila e processados com a taxa de limite. Esse efeito é chamado *a modelagem de tráfego*. Além disso, o tamanho dessa fila é limitado. Se continuar a violação de restrição, eventualmente, a fila está cheia e IoT Hub inicia rejeitando solicitações com `429 ThrottlingException`. 

Por exemplo, você deve usar um dispositivo simulado para enviar mensagens de dispositivo para nuvem 200 por segundo para o Hub do IoT S1 (que tem um limite de envios de D2C 100/s). Para o primeiro minuto ou dois, as mensagens são processadas imediatamente. No entanto, uma vez que o dispositivo continuará a enviar mais mensagens que o limite de aceleração, o IoT Hub começa a processar apenas 100 mensagens por segundo e coloca o resto em uma fila. Você começa a perceber um aumento da latência. Por fim, você começar a receber `429 ThrottlingException` como o fila é preenchido e o "número de erros de limitação" na [métricas do IoT Hub](iot-hub-metrics.md) começa a aumentar.

### <a name="identity-registry-operations-throttle"></a>Acelerar operações de registro de identidade

Operações de registro de identidade do dispositivo são destinadas a uso de tempo de execução em cenários de provisionamento e gerenciamento de dispositivo. A leitura ou atualização de grandes números de identidades de dispositivo é compatível por meio de [trabalhos de importação e exportação](iot-hub-devguide-identity-registry.md#import-and-export-device-identities).

### <a name="device-connections-throttle"></a>Limitar as conexões de dispositivo

As *conexões de dispositivo* controla a taxa em que novas conexões de dispositivo podem ser estabelecidas com um Hub IoT. A restrição de *conexões de dispositivo* não controla o número máximo de dispositivos conectados simultaneamente. A limitação da taxa de *conexões de dispositivo* depende do número de unidades provisionadas para o hub IoT.

Por exemplo, se você comprar uma única unidade S1, obterá uma restrição de 100 conexões por segundo. Portanto, 100, dispositivos 000, serão necessários para conectar pelo menos 1.000 segundos (aproximadamente 16 minutos). No entanto, você pode conectar ao mesmo tempo todos os seus dispositivos registrados no registro de identidade.


## <a name="other-limits"></a>Outros limites

IoT Hub impõe outros limites operacionais:

| Operação | Limite |
| --------- | ----- |
| Dispositivos | O número máximo de dispositivos que você pode se conectar a um único hub IoT é 1.000.000. A única maneira de aumentar esse limite é contatar [Microsoft Support](https://azure.microsoft.com/support/options/).| 
| URIs de upload de arquivos | URIs de SAS de 10.000 podem estar fora de uma conta de armazenamento ao mesmo tempo. <br/>  10 URIs de SAS/dispositivo podem estar fora ao mesmo tempo. |
| Trabalhos<sup>1</sup> | Máximo de trabalhos simultâneos é 1 (para gratuito e S1), 5 (para S2) e 10 (para S3). No entanto, a máximo simultâneas [trabalhos de importação/exportação do dispositivo](iot-hub-bulk-identity-mgmt.md) é 1 para todas as camadas. <br/>Histórico de trabalhos é retido por até 30 dias. |
| Pontos de extremidade adicionais | Hubs SKU pagos podem ter 10 pontos de extremidade adicionais. Hubs SKU gratuitos podem ter um ponto de extremidade adicional. |
| Regras de roteamento de mensagem | Hubs SKU pagos podem ter 100 regras de roteamento. Hubs SKU gratuitos podem ter cinco regras de roteamento. |
| Mensagens do dispositivo para a nuvem | Tamanho máximo da mensagem 256 KB |
| Mensagens de nuvem para dispositivo<sup>1</sup> | Tamanho máximo da mensagem 64 KB. O máximo de mensagens pendentes para entrega é 50. |
| Método direto<sup>1</sup> | O tamanho de payload do método direto máximo é 128 KB. |
| Configurações automáticas de dispositivo<sup>1</sup> | 100 configurações por hub SKU pago. 20 configurações por hub SKU gratuito. |
| Implantações automáticas do Edge<sup>1</sup> | 20 módulos por implantação. 100 implantações por hub SKU pago. 20 implantações por hub SKU gratuito. |
| Gêmeos<sup>1</sup> | O tamanho máximo por seções gêmeas (marcas, propriedades desejadas, propriedades relatadas) é de 8 KB |

<sup>1</sup>Esse recurso não está disponível na camada básica do Hub IoT. Para obter mais informações, consulte [Como escolher o Hub IoT correto](iot-hub-scaling.md).

## <a name="increasing-the-quota-or-throttle-limit"></a>Aumentar o limite de cota ou acelerador

A qualquer momento, você pode aumentar as cotas ou limitar os limites por [aumentar o número de unidades provisionadas em um hub IoT](iot-hub-upgrade.md).

## <a name="latency"></a>Latency

O Hub IoT busca oferecer baixa latência para todas as operações. No entanto, devido às condições de rede e outros fatores imprevisíveis ele não garante uma latência de determinados. Ao projetar sua solução, você deve:

* Evitar fazer suposições sobre a latência máxima de qualquer operação de IoT Hub.
* Provisionar o hub IoT na região do Azure mais próxima de seus dispositivos.
* Considere usar o Azure IoT Edge para executar operações sensíveis à latência no dispositivo ou no gateway perto do dispositivo.

Várias unidades de Hub IoT afetam limitação, conforme descrito anteriormente, mas não oferecem nenhum benefício de latência nem garantia adicional.

No caso de aumentos inesperados na latência da operação, entre em contato com o [Suporte da Microsoft](https://azure.microsoft.com/support/options/).

## <a name="next-steps"></a>Próximas etapas

Para uma discussão aprofundada do comportamento de limitação do Hub IoT, confira a postagem do blog [A limitação do Hub IoT e você](https://azure.microsoft.com/blog/iot-hub-throttling-and-you/).

Outros tópicos de referência neste Guia do desenvolvedor do Hub IoT incluem:

* [Pontos de extremidade do Hub IoT](iot-hub-devguide-endpoints.md)
