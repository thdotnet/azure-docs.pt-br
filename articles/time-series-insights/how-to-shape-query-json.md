---
title: Melhores práticas para formatar JSON nas consultas do Azure Time Series Insights | Microsoft Docs
description: Saiba como melhorar a eficiência de consulta do Azure Time Series Insights.
services: time-series-insights
author: ashannon7
manager: cshankar
ms.service: time-series-insights
ms.topic: article
ms.date: 05/09/2019
ms.author: dpalled
ms.custom: seodec18
ms.openlocfilehash: 089285637bb740fea47f1fd07de0906dfe46662b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244469"
---
# <a name="shape-json-to-maximize-query-performance"></a>Forma JSON para maximizar o desempenho de consulta 

Este artigo fornece diretrizes sobre como a forma JSON para maximizar a eficiência de suas consultas de análise de séries temporais do Azure.

## <a name="video"></a>Vídeo

### <a name="learn-best-practices-for-shaping-json-to-meet-your-storage-needsbr"></a>Aprenda as práticas recomendadas para a formatação JSON para atender às suas necessidades de armazenamento.</br>

> [!VIDEO https://www.youtube.com/embed/b2BD5hwbg5I]

## <a name="best-practices"></a>Práticas recomendadas
Pense em como você envia eventos para análise de séries Temporais. Ou seja, você sempre:

1. Envie dados pela rede de maneira mais eficiente possível.
1. Verifique se que seus dados são armazenados em uma maneira para que você pode executar agregações adequadas para seu cenário.
1. Certifique-se de que você não alcançar os limites de propriedade máximo de séries temporais do:
   - 600 propriedades (colunas) para ambientes S1.
   - 800 propriedades (colunas) para ambientes S2.

As diretrizes a seguir ajudam a garantir o melhor desempenho de consulta possíveis:

1. Não use propriedades dinâmicas, como uma ID de marca, como um nome de propriedade. Esse uso contribui para atingir o limite máximo de propriedades.
1. Não envie propriedades desnecessárias. Se uma propriedade de consulta não é necessária, é melhor não enviá-lo. Dessa forma, que você evita as limitações de armazenamento.
1. Use [dados de referência](time-series-insights-add-reference-data-set.md) para evitar o envio de dados estáticos na rede.
1. Compartilhar propriedades de dimensão entre vários eventos para enviar dados pela rede com mais eficiência.
1. Não use o aninhamento profundo de matriz. Time Series Insights dá suporte a até dois níveis de matrizes aninhadas que contêm objetos. Time Series Insights nivela matrizes nas mensagens em vários eventos com pares de valor de propriedade.
1. Se apenas algumas medidas existirem para a maioria ou todos os eventos, será melhor enviar essas medidas como propriedades separadas dentro do mesmo objeto. Enviá-los separadamente reduz o número de eventos e pode melhorar o desempenho da consulta, porque menos eventos precisam ser processados. Quando há várias medidas, enviá-los como valores em uma única propriedade minimiza a possibilidade de se atingir o limite máximo de propriedade.

## <a name="example-overview"></a>Visão geral de exemplo

Os exemplos a seguir demonstram como enviar eventos para realçar as recomendações anteriores. Após cada exemplo, você pode ver como as recomendações foram aplicadas.

Os exemplos são baseados em um cenário onde vários dispositivos enviam sinais ou medidas. Sinais ou medidas podem ser a taxa de fluxo, pressão de óleo do mecanismo, temperatura e umidade. No primeiro exemplo, há algumas medidas em todos os dispositivos. O segundo exemplo tem muitos dispositivos, e cada dispositivo envia muitas medidas exclusivas.

## <a name="scenario-one-only-a-few-measurements-exist"></a>Cenário um: Existem apenas algumas medidas

> [!TIP]
> É recomendável que você envie cada medida ou um sinal como uma propriedade separada ou coluna.

No exemplo a seguir, há uma única mensagem do IoT Hub do Azure em que a matriz externa contém uma seção compartilhada comum de valores de dimensão. A matriz externa usa dados de referência para aumentar a eficiência da mensagem. Dados de referência contém os metadados de dispositivo que não são alterados com todos os eventos, mas ela fornece propriedades úteis para análise de dados. Os valores de dimensão comum de envio em lote e empregando dados salva em bytes enviados durante a transmissão de referência, o que torna a mensagem mais eficiente.

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

* Tabela de dados de referência que tem a propriedade de chave **deviceId**:

   | deviceId | messageId | deviceLocation |
   | --- | --- | --- |
   | FXXX | DADOS\_LINHA | UE |
   | FAAA | DADOS\_LINHA | EUA |

* Análise de séries evento tabela tempo, após nivelar:

   | deviceId | messageId | deviceLocation | timestamp | series.Flow Rate ft3/s | series.Engine Oil Pressure psi |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | DADOS\_LINHA | UE | 2018-01-17T01:17:00Z | 1,0172575712203979 | 34,7 |
   | FXXX | DADOS\_LINHA | UE | 2018-01-17T01:17:00Z | 2,445906400680542 | 49,2 |
   | FAAA | DADOS\_LINHA | EUA | 2018-01-17T01:18:00Z | 0,58015072345733643 | 22,2 |

Observações sobre essas duas tabelas:

- A coluna **deviceId** serve como o cabeçalho de coluna para vários dispositivos em uma frota. Tornando o valor deviceId seu próprio nome de propriedade limita o total de dispositivos para 595 (para ambientes de S1) ou 795 (para ambientes de S2) com as outras cinco colunas.
- Propriedades desnecessárias são evitadas, para obter informações de exemplo, a marca e modelo. Porque as propriedades não ser consultadas no futuro, eliminá-las permite melhor eficiência da rede e armazenamento.
- Os dados de referência são usados para reduzir o número de bytes transferidos pela rede. Os dois atributos **messageId** e **deviceLocation** são unidas por meio da propriedade chave **deviceId**. Esses dados são Unidos com os dados de telemetria em tempo de entrada e, em seguida, são armazenados no Time Series Insights para a consulta.
- Duas camadas de aninhamento são usadas, que é a quantidade máxima de aninhamento com suporte pelo Time Series Insights. É essencial para evitar matrizes profundamente aninhadas.
- Medidas são enviadas como propriedades separadas dentro do mesmo objeto, porque há algumas medidas. Aqui, **series.Flow Rate psi** e **series.Engine Oil Pressure ft3/s** são colunas exclusivas.

## <a name="scenario-two-several-measures-exist"></a>Cenário dois: Existem várias medidas

> [!TIP]
> É recomendável que você envia as medidas como "tipo", "unidade" e "valor" de tuplas.

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

* Tabela de dados de referência que tem as propriedades de chave **deviceId** e **series.tagId**:

   | deviceId | series.tagId | messageId | deviceLocation | type | unit |
   | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | DADOS\_LINHA | UE | Taxa de Fluxo | ft3/s |
   | FXXX | oilPressure | DADOS\_LINHA | UE | Pressão de óleo do motor | psi |
   | FAAA | pumpRate | DADOS\_LINHA | EUA | Taxa de Fluxo | ft3/s |
   | FAAA | oilPressure | DADOS\_LINHA | EUA | Pressão de óleo do motor | psi |

* Análise de séries evento tabela tempo, após nivelar:

   | deviceId | series.tagId | messageId | deviceLocation | type | unit | timestamp | series.value |
   | --- | --- | --- | --- | --- | --- | --- | --- |
   | FXXX | pumpRate | DADOS\_LINHA | UE | Taxa de Fluxo | ft3/s | 2018-01-17T01:17:00Z | 1,0172575712203979 | 
   | FXXX | oilPressure | DADOS\_LINHA | UE | Pressão de óleo do motor | psi | 2018-01-17T01:17:00Z | 34,7 |
   | FXXX | pumpRate | DADOS\_LINHA | UE | Taxa de Fluxo | ft3/s | 2018-01-17T01:17:00Z | 2,445906400680542 | 
   | FXXX | oilPressure | DADOS\_LINHA | UE | Pressão de óleo do motor | psi | 2018-01-17T01:17:00Z | 49,2 |
   | FAAA | pumpRate | DADOS\_LINHA | EUA | Taxa de Fluxo | ft3/s | 2018-01-17T01:18:00Z | 0,58015072345733643 |
   | FAAA | oilPressure | DADOS\_LINHA | EUA | Pressão de óleo do motor | psi | 2018-01-17T01:18:00Z | 22,2 |

Observações sobre essas duas tabelas:

- As colunas **deviceId** e **series.tagId** servem como os cabeçalhos de coluna para os vários dispositivos e marcas em uma frota. Uso de cada como seu próprio atributo limita a consulta 594 (para ambientes de S1) ou 794 (para ambientes de S2) total de dispositivos com seis colunas.
- propriedades desnecessárias foram evitadas pelo motivo citado no primeiro exemplo.
- Dados de referência são usados para reduzir o número de bytes transferidos pela rede, introduzindo **deviceId**, que é usado para o par exclusivo de **messageId** e **deviceLocation**. A chave composta **series.tagId** é usado para o par exclusivo de **tipo** e **unidade**. A chave composta permite que o **deviceId** e **series.tagId** par a ser usado para se referir a quatro valores: **messageId, deviceLocation, digite,** e **daunidade**. Esses dados são associados com os dados de telemetria em tempo de entrada. Ele é armazenado no Time Series Insights para a consulta.
- duas camadas de aninhamento são usadas pelo motivo citado no primeiro exemplo.

### <a name="for-both-scenarios"></a>Para ambos os cenários

Para uma propriedade com um grande número de valores possíveis, é melhor enviar como valores distintos em uma única coluna em vez de criar uma nova coluna para cada valor. Dos dois exemplos anteriores:

  - No primeiro exemplo, algumas propriedades têm vários valores, portanto, é apropriado para que cada uma propriedade separada.
  - No segundo exemplo, as medidas não são especificadas como propriedades individuais. Em vez disso, eles são uma matriz de valores ou medidas em uma propriedade comum da série. A nova chave **tagId** é enviado, que cria a nova coluna **series.tagId** na tabela plana. As novas propriedades **tipo** e **unidade** são criados usando os dados de referência para que o limite de propriedade não é atingido.

## <a name="next-steps"></a>Próximas etapas

- Leia [sintaxe de consulta do Azure Time Series Insights](/rest/api/time-series-insights/ga-query-syntax) para saber mais sobre a sintaxe de consulta para a API REST de acesso os dados de séries Temporais.
- Saiba mais [como eventos de forma](./time-series-insights-send-events.md).
