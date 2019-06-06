---
title: Estruturas de dados de serviço de mobilidade em mapas do Azure | Microsoft Docs
description: Estruturas de dados no serviço de mobilidade do Azure mapas
author: walsehgal
ms.author: v-musehg
ms.date: 06/05/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 29e8a9d7555ca836b6266879f3b3c1e32ffd3980
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66735550"
---
# <a name="data-structures-in-azure-maps-mobility-service"></a>Estruturas de dados no serviço de mobilidade do Azure mapas

Este artigo apresenta o conceito de Metro área [serviço de mobilidade do Azure mapas](https://aka.ms/AzureMapsMobilityService) e alguns dos campos comuns retornados por meio de serviços, quando é consultado para públicos de trânsito para e linhas. É recomendável passar por este artigo antes de começar com as APIs de serviço de mobilidade. Discutimos esses campos comuns abaixo.

## <a name="metro-area"></a>Área metropolitana

Dados de serviço de mobilidade são divididos em áreas de metrô com suporte. Áreas de metrô não seguem os limites de cidade, uma área metropolitana pode conter várias cidades, por exemplo, cidade densamente populada e suas cidades ao redor; e um país/região pode ser uma área metropolitana. 

O `metroID` é ID de uma área metropolitana que pode ser usado para chamar o [obter API de informações da área do Metro](https://aka.ms/AzureMapsMobilityMetroAreaInfo) para tipos de solicitação de suporte de trânsito e detalhes adicionais para a área do metro como agências de trânsito e alertas ativos. Você pode usar a API do Azure mapas obter Metro para solicitar o metroIDs e áreas de metrô com suporte. IDs de área metropolitana estão sujeitos a alterações.

**metroID:** 522 **nome:** Seattle-Tacoma-Bellevue

![Seattle-metro-area](./media/mobility-service-data-structure/seattle-metro.png)

## <a name="stop-ids"></a>Parar IDs

Paradas de trânsito podem ser referenciadas por dois tipos de IDs, o [especificação de Feed de trânsito geral (GFTS)](https://gtfs.org/) ID (conhecido como stopKey) e o Azure mapas param ID (conhecido como stopId). Para se referir a paradas ao longo do tempo, é recomendável para usar a ID de parada de mapas do Azure, como essa ID é mais estável e não provavelmente será alterado desde que exista a parada de física. A ID de parada GTFS é atualizada com mais frequência, por exemplo, caso o provedor GTFS precisa alterá-la ou nova versão GTFS for lançada, embora a parada física não teve nenhuma alteração.

Para começar, você pode solicitar para de trânsito próximo usando [obter a API de trânsito próximos](https://aka.ms/AzureMapsMobilityNearbyTransit).

## <a name="line-groups-and-lines"></a>Grupos de linhas e linhas

O serviço de mobilidade usa um modelo de dados em paralelo para linhas e grupos de linhas para lidar melhor com as alterações herdadas do [GTFS](https://gtfs.org/) modelo de dados de corridas e rotas.


### <a name="line-groups"></a>Grupos de linhas

Um grupo de linha é uma entidade, que agrupa todas as linhas logicamente fazem parte do mesmo grupo. Normalmente, um grupo de linha conterá duas linhas, um vai do ponto A e B e a outros retornar de ponto B para um, ambos os pertencentes à mesma agência de transporte público e ter o mesmo número de linha. No entanto, pode haver casos em que uma linha do grupo tem mais de duas linhas ou apenas uma única linha dentro dele.


### <a name="lines"></a>Linhas

Como discutido acima, cada grupo de linhas é composto de um conjunto de linhas. Geralmente cada linha descreve uma direção e cada linha de grupo é composto de duas linhas. No entanto, há casos em que mais linhas compõem um grupo de linhas, por exemplo lá é uma linha que, às vezes, desvia por meio de um determinado ambiente e, às vezes, não faz isso e é operada em ambos os casos em que o mesmo número de linha e há outros casos em que uma linha g rupar é composto de uma única linha, por exemplo, uma linha circular com uma única direção.

Para começar, você pode solicitar os grupos de linhas usando o [API de linha de trânsito obter](https://aka.ms/AzureMapsMobilityTransitLine) e posterior drill down a linhas.


## <a name="next-steps"></a>Próximas etapas

Saiba como solicitar dados de trânsito usando o serviço de mobilidade:

> [!div class="nextstepaction"]
> [Como solicitar dados de trânsito](how-to-request-transit-data.md)

Saiba como solicitar dados em tempo real usando o serviço de mobilidade:

> [!div class="nextstepaction"]
> [Como solicitar dados em tempo real](how-to-request-real-time-data.md)

Explorar a documentação da API do serviço de mobilidade do Azure mapas

> [!div class="nextstepaction"]
> [Documentação da API do serviço de mobilidade](https://aka.ms/AzureMapsMobilityService)
