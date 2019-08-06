---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/06/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: b1bbc11d7772e4f56d7dc6ead580b0a0cbd3cd8d
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68669509"
---
As funções facilitam a adição da integração do Application Insights a um aplicativo de funções do [portal do Azure].

1. No [portal][Portal do Azure], selecione **Todos os serviços > Aplicativos de Funções**, selecione o aplicativo de funções e, em seguida, selecione a faixa do **Application Insights** na parte superior da janela

    ![Habilitar o Application Insights no portal](media/functions-connect-new-app-insights/enable-application-insights.png)

1. Crie um recurso do Application Insights usando as configurações especificadas na tabela abaixo da imagem.

   ![Criar um recurso do Application Insights](media/functions-connect-new-app-insights/ai-general.png)

    | Configuração      | Valor sugerido  | Descrição                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nome** | Nome de aplicativo exclusivo | É mais fácil usar o mesmo nome que seu aplicativo de funções, que deve ser exclusivo em sua assinatura. | 
    | **Localidade** | Europa Ocidental | Se possível, use a mesma [região](https://azure.microsoft.com/regions/) que seu aplicativo de funções ou uma região próxima dela. |

1. Selecione **OK**. O recurso do Application Insights é criado no mesmo grupo de recursos e assinatura que seu aplicativo de funções. Depois que o recurso for criado, feche a janela do Application Insights.

1. No aplicativo de funções, selecione **Configurações do aplicativo** e role para baixo até **Configurações do aplicativo**. Se você vir uma configuração chamada `APPINSIGHTS_INSTRUMENTATIONKEY`, isso significa que a integração do Application Insights está habilitada para seu aplicativo de funções em execução no Azure.

[Portal do Azure]: https://portal.azure.com
