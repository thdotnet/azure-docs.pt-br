---
title: Melhores práticas para formatar JSON nas consultas do Azure Time Series Insights | Microsoft Docs
description: Saiba como melhorar a eficiência de consulta do Azure Time Series Insights.
services: time-series-insights
author: ashannon7
manager: cshankar
ms.service: time-series-insights
ms.topic: article
ms.date: 05/09/2019
ms.author: anshan
ms.custom: seodec18
ms.openlocfilehash: 535fe9a7920db89e434b4cc1b66aa38bf72a7829
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65790075"
---
# <a name="how-to-shape-json-to-maximize-query-performance"></a>Como formatar JSON para maximizar o desempenho da consulta 

Este artigo fornece orientação para a formatação JSON, a fim de maximizar a eficiência de suas consultas de análise de séries temporais do Azure.

## <a name="video"></a>Vídeo

### <a name="learn-best-practices-for-shaping-json-to-meet-your-storage-needsbr"></a>Aprenda as práticas recomendadas para a formatação JSON para atender às suas necessidades de armazenamento.</br>

> [!VIDEO https://www.youtube.com/embed/b2BD5hwbg5I]

## <a name="best-practices"></a>Práticas recomendadas

É importante considerar como enviar eventos para o Time Series Insights. Ou seja, você sempre deve:

1. Envie dados pela rede de maneira mais eficiente possível.
1. Certifique-se de que seus dados são armazenados de forma que lhe permite executar agregações adequadas para seu cenário.
1. Verifique se você não atinge os limites de propriedade máximo do séries temporais de:
   - 600 propriedades (colunas) para ambientes S1.
   - 800 propriedades (colunas) para ambientes S2.

As diretrizes a seguir ajudam a garantir o melhor desempenho de consulta possível:

1. Não use as propriedades dinâmicas, como uma ID de marca, como nome de propriedade, pois ela contribui para atingir o limite máximo de propriedades.
1. Não envie propriedades desnecessárias. Se uma propriedade de consulta não for necessária, será melhor não enviá-lo e evitar limitações de armazenamento.
1. Use [dados de referência](time-series-insights-add-reference-data-set.md) para evitar o envio de dados estáticos na rede.
1. Compartilhe propriedades de dimensão entre vários eventos para enviar dados pela rede com mais eficiência.
1. Não use o aninhamento profundo de matriz. Time Series Insights dá suporte a até dois níveis de matrizes aninhadas que contêm objetos. Time Series Insights nivela matrizes nas mensagens, em vários eventos com pares de valor de propriedade.
1. Se apenas algumas medidas existirem para a maioria ou todos os eventos, será melhor enviar essas medidas como propriedades separadas dentro do mesmo objeto. Enviá-los separadamente reduz o número de eventos e pode tornar consultas mais funcionais, pois menos eventos precisam ser processados. Quando houver várias medidas, enviá-las como valores em uma única propriedade minimiza a possibilidade de atingir o limite máximo de propriedade.

## <a name="example-overview"></a>Visão geral de exemplo

Os exemplos a seguir demonstram como enviar eventos para destacar as recomendações anteriores. Após cada exemplo, você pode ver como as recomendações foram aplicadas.

Os exemplos são baseados em um cenário onde vários dispositivos enviam sinais ou medidas. As medidas ou sinais podem ser a Taxa de Fluxo, Pressão de Óleo do Motor, Temperatura e Umidade. No primeiro exemplo, há algumas medidas em todos os dispositivos. No segundo exemplo, há vários dispositivos, e cada um envia muitas medidas exclusivas.

## <a name="scenario-one-only-a-few-measurements-exist"></a>Cenário um: existem apenas algumas medidas

> [!TIP]
> **Recomendação**: enviar cada medida/sinal como uma propriedade/coluna separada.

No exemplo a seguir, há uma única mensagem do Hub IoT, onde a matriz externa contém uma seção compartilhada comuns dos valores de dimensão. A matriz externa usa dados de referência para aumentar a eficiência da mensagem. Os dados de referência contêm metadados do dispositivo, que não são alterados com cada evento, mas fornecem propriedades úteis para análise de dados. Os valores comuns de dimensão de envio em lote e a utilização de dados de referência, salva em bytes enviados pela rede, tornando a mensagem mais eficiente.

Carga JSON de exemplo:

```json
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "Flow Rate ft3/s": 1.0172575712203979,
                "Engine Oil Pressure psi ": 34.7
            },
            {
                "Flow Rate ft3/s": 2.445906400680542,
                "Engine Oil Pressure psi ": 49.2
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "Flow Rate ft3/s": 0.58015072345733643,
                "Engine Oil Pressure psi ": 22.2
            }
        ]
    }
]
```

* Tabela de dados de referência (propriedade de chave é **deviceId**):

   | deviceId | messageId | deviceLocation |
   | --- | --- | --- |
   | FXXX | DADOS\_LINHA | UE |
   | FAAA | DADOS\_LINHA | EUA |

* Tabela de eventos do Time Series Insights (após a mesclagem):

   | deviceId | messageId | deviceLocation |  timestamp | series.Flow Rate ft3/s | series.Engine Oil Pressure psi |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | DADOS\_LINHA | UE | 2018-01-17T01:17:00Z | 1,0172575712203979 | 34,7 |
   | FXXX | DADOS\_LINHA | UE | 2018-01-17T01:17:00Z | 2,445906400680542 | 49,2 |
   | FAAA | DADOS\_LINHA | EUA | 2018-01-17T01:18:00Z | 0,58015072345733643 | 22,2 |

Acima:

- A coluna **deviceId** serve como o cabeçalho de coluna para vários dispositivos em uma frota. Tentar tornar o valor de deviceId seu próprio nome de propriedade limitaria o total de dispositivos a 595 (ambientes S1) ou 795 (ambientes S2), com as outras cinco colunas.

- As propriedades desnecessárias são anuladas, por exemplo, informações de marca e modelo etc. Uma vez que eles não serão consultados no futuro, eliminá-los permite mais eficiência de rede e de armazenamento.

- Os dados de referência são usados para reduzir o número de bytes transferidos pela rede. Dois atributos, **messageId** e **deviceLocation**, são unidas usando a propriedade de chave **deviceId**. Esses dados são Unidos com os dados de telemetria em tempo de ingresso e subsequentemente armazenados no Time Series Insights para a consulta.

- Duas camadas de aninhamento são usadas, que é a quantidade máxima de aninhamento com suporte pelo Time Series Insights. É essencial para evitar matrizes profundamente aninhadas.

- As medidas são enviadas como propriedades separadas no mesmo objeto, já que há algumas medidas. Aqui, **series.Flow Rate psi** e **series.Engine Oil Pressure ft3/s** são colunas exclusivas.

## <a name="scenario-two-several-measures-exist"></a>Cenário dois: existem várias medidas

> [!TIP]
> **Recomendação:** envie medidas como as tuplas "tipo", "unidade", "valor".

Carga JSON de exemplo:

```json
[
    {
        "deviceId": "FXXX",
        "timestamp": "2018-01-17T01:17:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 1.0172575712203979
            },
            {
                "tagId": "oilPressure",
                "value": 49.2
            },
            {
                "tagId": "pumpRate",
                "value": 2.445906400680542
            },
            {
                "tagId": "oilPressure",
                "value": 34.7
            }
        ]
    },
    {
        "deviceId": "FYYY",
        "timestamp": "2018-01-17T01:18:00Z",
        "series": [
            {
                "tagId": "pumpRate",
                "value": 0.58015072345733643
            },
            {
                "tagId": "oilPressure",
                "value": 22.2
            }
        ]
    }
]
```

* Dados de referência (Propriedades de chave são **deviceId** e **series.tagId**):

   | deviceId | series.tagId | messageId | deviceLocation | tipo | unidade |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | DADOS\_LINHA | UE | Taxa de Fluxo | ft3/s |
   | FXXX | oilPressure | DADOS\_LINHA | UE | Pressão de óleo do motor | psi |
   | FAAA | pumpRate | DADOS\_LINHA | EUA | Taxa de Fluxo | ft3/s |
   | FAAA | oilPressure | DADOS\_LINHA | EUA | Pressão de óleo do motor | psi |

* Tabela de eventos do Time Series Insights (após a mesclagem):

   | deviceId | series.tagId | messageId | deviceLocation | tipo | unidade |  timestamp | series.value |
   | --- | --- | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | DADOS\_LINHA | UE | Taxa de Fluxo | ft3/s | 2018-01-17T01:17:00Z | 1,0172575712203979 | 
   | FXXX | oilPressure | DADOS\_LINHA | UE | Pressão de óleo do motor | psi | 2018-01-17T01:17:00Z | 34,7 |
   | FXXX | pumpRate | DADOS\_LINHA | UE | Taxa de Fluxo | ft3/s | 2018-01-17T01:17:00Z | 2,445906400680542 | 
   | FXXX | oilPressure | DADOS\_LINHA | UE | Pressão de óleo do motor | Psi | 2018-01-17T01:17:00Z | 49,2 |
   | FAAA | pumpRate | DADOS\_LINHA | EUA | Taxa de Fluxo | ft3/s | 2018-01-17T01:18:00Z | 0,58015072345733643 |
   | FAAA | oilPressure | DADOS\_LINHA | EUA | Pressão de óleo do motor | psi | 2018-01-17T01:18:00Z | 22,2 |

Acima:

- As colunas **deviceId** e **series.tagId** servem como os cabeçalhos de coluna para os vários dispositivos e marcas em uma frota. Usar cada um deles como seu próprio atributo limitaria a consultas um total de 594 (ambientes S1) ou 794 (ambientes S2) dispositivos com as outras seis colunas.

- propriedades desnecessárias foram evitadas pelo motivo citado no primeiro exemplo.

- dados de referência são usados para reduzir o número de bytes transferidos pela rede, introduzindo **deviceId**, para um par exclusivo de **messageId** e **deviceLocation**. Uma chave composta é usada, **series.tagId**, para o par exclusivo de **tipo** e **unidade**. A chave composta permite que o par **deviceId** e **series.tagId** seja usado para fazer referência a quatro valores: **messageId, deviceLocation, type** e **unit**. Esses dados são Unidos com os dados de telemetria em tempo de ingresso e subsequentemente armazenados no Time Series Insights para a consulta.

- duas camadas de aninhamento são usadas pelo motivo citado no primeiro exemplo.

### <a name="for-both-scenarios"></a>Para ambos os cenários

Se você tiver uma propriedade com um grande número de valores possíveis, é melhor enviar como valores distintos em uma única coluna em vez de criar uma nova coluna para cada valor. Dos dois exemplos anteriores:

  - No primeiro exemplo, há algumas propriedades que têm vários valores, portanto é melhor criar cada propriedade em separado.
  - No entanto, no segundo exemplo, você pode ver que as medidas não são especificadas como propriedades individuais, mas em vez disso, uma matriz de valores/medidas em uma propriedade comum da série. Uma nova chave é enviada, **tagId** , que cria uma nova coluna, **series.tagId**, na tabela plana. Novas propriedades são criadas, **type** e **unit**, usando dados de referência, evitando que o limite de propriedade seja atingido.

## <a name="next-steps"></a>Próximas etapas

- Leia [sintaxe de consulta do Azure Time Series Insights](/rest/api/time-series-insights/ga-query-syntax) para saber mais sobre a sintaxe de consulta para a API REST de acesso os dados de séries Temporais.

- Saiba mais [como eventos de forma](./time-series-insights-send-events.md).
