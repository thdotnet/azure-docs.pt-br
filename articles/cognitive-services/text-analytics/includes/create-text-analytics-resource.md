---
title: Criar um recurso de análise de texto de serviços Cognitivos
titleSuffix: Azure Cognitive Services
description: Saiba como criar um recurso de análise de texto de serviços Cognitivos.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 7e8b4480911f00afa8524ef4b81d697bb8ee5bcc
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/15/2019
ms.locfileid: "67877471"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Criar um recurso de análise de texto de serviços Cognitivos

1. Entre no [Portal do Azure](https://portal.azure.com).
1. Selecione **criar um recurso**e, em seguida, vá para **IA + Machine Learning** > **análise de texto**.
   Ou, acesse [análise de texto criar](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics).
1. Insira todas as configurações necessárias:

    |Configuração|Valor|
    |--|--|
    |Nome|Insira um nome (2 a 64 caracteres)|
    |Assinatura|Selecione a assinatura apropriada|
    |Location|Selecione um local próximo|
    |Tipo de preço| Insira **S**, tipo de preço standard|
    |Grupo de recursos|Selecione um grupo de recursos disponíveis|

1. Selecione **criar** e aguarde até que o recurso a ser criado. Seu navegador redireciona automaticamente para a página de recursos criado recentemente.
1. Coletar configurado `endpoint` e uma chave de API:

    |Guia de recursos no portal|Configuração|Valor|
    |--|--|--|
    |**Visão geral**|Ponto de extremidade|Copie o ponto de extremidade. Ele se pareça com `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |**Chaves**|Chave de API|Copie uma das duas chaves. É uma cadeia de caracteres alfanumérica de 32 caracteres sem espaços ou traços: <`xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`>.|
