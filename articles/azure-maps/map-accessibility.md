---
title: Criar um aplicativo acessível com o Azure Mapas | Microsoft Docs
description: Como criar um aplicativo acessível usando o Azure Mapas
services: azure-maps
author: chgennar
ms.author: chgennar
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
manager: timlt
ms.openlocfilehash: 8865027c895be09150797608e43184f1fdefb267
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638777"
---
# <a name="building-an-accessible-application"></a>Criando um aplicativo acessível

Este artigo mostra como criar um aplicativo de mapa que pode ser usado por leitores de tela.

## <a name="understand-the-code"></a>Compreender o código

<iframe height='500' scrolling='no' title='Criar um aplicativo acessível' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Veja a caneta <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>Criar um aplicativo acessível</a> pelo Azure Mapas (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) em <a href='https://codepen.io'>CodePen</a>.
</iframe>

O mapa é pré-compilado com alguns recursos de acessibilidade. Os usuários podem navegar o mapa usando o teclado. Se um leitor de tela estiver em execução, o mapa notificará o usuário de alterações em seu estado.
Por exemplo, os usuários serão notificados das alterações do mapa quando o mapa é deslocado ou ampliado. Qualquer informação adicional é colocada no mapa de base deve ter informações textuais correspondentes para os usuários de leitores de tela.

Usar o [pop-up](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) é uma maneira de fazer isso. No exemplo acima de pesquisa, um pop-up com informações textuais é adicionado ao mapa para cada marcador colocado no mapa. Usar o método anexar[do pop-up](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) [anexar](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#attach) permite que o popup seja visto por um leitor de tela sem exibir visualmente o pop-up no mapa.

## <a name="next-steps"></a>Próximas etapas

Saiba mais sobre as classes e métodos de pop-up usados neste artigo:

> [!div class="nextstepaction"]
> [Pop-up](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Confira mais exemplos de código:

> [!div class="nextstepaction"]
> [Página de exemplo de código](https://aka.ms/AzureMapsSamples)
