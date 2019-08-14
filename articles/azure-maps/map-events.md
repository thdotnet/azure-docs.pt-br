---
title: Manipular eventos de mouse com o Azure Maps | Microsoft Docs
description: Como criar um mapa de SDK da Web interativo com eventos de mapa
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 37a3fc3178fe5caeacedfd355a6065ee189a5890
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976537"
---
# <a name="interact-with-the-map---mouse-events"></a>Interagir com o mapa - eventos de mouse

Este artigo mostra como usar a propriedade de [eventos da classe Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?#events) para realçar eventos no mapa e em camadas diferentes do mapa. Ele também mostra como usar a propriedade map class events para realçar eventos ao interagir com um marcador HTML.

## <a name="interact-with-the-map"></a>Interagir com o mapa

Jogue com o mapa abaixo e veja os eventos de mouse correspondentes realçados à direita. Você pode clicar na **guia JS** para visualizar e editar o código JavaScript. Você também pode clicar no botão **Edit on CodePen** e editar o código no CodePen.

<br/>

<iframe height='600' scrolling='no' title='Interagir com o mapa – eventos do mouse' src='//codepen.io/azuremaps/embed/bLZEWd/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Veja a Caneta <a href='https://codepen.io/azuremaps/pen/bLZEWd/'>Interagir com os eventos do mapa do mouse</a> pelos Mapas do Azure (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) em <a href='https://codepen.io'> CodePen</a>.
</iframe>

## <a name="interact-with-map-layers"></a>Interagir com as camadas do mapa

O código a seguir realça o nome dos eventos que são acionados enquanto você interage com a camada de símbolo. O símbolo, a bolha, a linha e a camada de polígono oferecem suporte ao mesmo conjunto de eventos. As camadas mapa de calor e bloco não oferecem suporte a nenhum desses eventos.

<br/>

<iframe height='600' scrolling='no' title='Interação com o mapa – eventos de camada' src='//codepen.io/azuremaps/embed/bQRRPE/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a Caneta <a href='https://codepen.io/azuremaps/pen/bQRRPE/'>Interação com o mapa – eventos de camada</a> pelo Azure Mapas (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) em <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="interact-with-html-marker"></a>Interagir com o marcador HTML

O código a seguir adiciona eventos de mapa JavaScript a um marcador HTML. Ele também destaca o nome dos eventos que são acionados conforme você interage com o marcador HTML.

<br/>

<iframe height='500' scrolling='no' title='Interação com o mapa – eventos de marcador HTML' src='//codepen.io/azuremaps/embed/VVzKJY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte a Caneta <a href='https://codepen.io/azuremaps/pen/VVzKJY/'>Interação com o mapa – eventos de marcador HTML</a> pelo Azure Mapas (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) em <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Próximas etapas

Consulte os seguintes artigos para obter exemplos de código completo:

> [!div class="nextstepaction"]
> [Usando o módulo de serviços do Azure Maps](./how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Exemplos de código](https://docs.microsoft.com/samples/browse/?products=azure-maps)
