---
title: Suporte a contêiner
titleSuffix: Azure Cognitive Services
description: Saiba como criar um recurso de análise de texto dos serviços cognitivos.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 022ffcd806d4d4f89f8a8cf256a541518ea12602
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455137"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Criar um recurso de análise de texto de serviços Cognitivos

1. Entre no [Portal do Azure](https://portal.azure.com)
1. Selecione **+ criar um recurso**, navegue até **IA + Machine Learning > análise de texto**, ou clique em [criar **de análise de texto**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics)
1. Insira todas as configurações necessárias:

    |Configuração|Valor|
    |--|--|
    |NOME|Nome desejado (2 a 64 caracteres)|
    |Assinatura|Selecione a assinatura apropriada|
    |Location|Selecione um local próximo|
    |Camada de preços|`S` -tipo de preço standard|
    |Grupo de recursos|Selecione um grupo de recursos disponíveis|

1. Clique em **criar** e aguarde até que o recurso a ser criado. Depois que ele é criado, seu navegador redireciona automaticamente para a página de recursos criado recentemente
1. Coletar configurado `endpoint` e uma chave de API:

    |Guia de recursos no Portal|Configuração|Value|
    |--|--|--|
    |**Visão geral**|Ponto de extremidade|Copie o ponto de extremidade. Ele é semelhante a `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |**Chaves**|Chave de API|Copie 1 das duas chaves. É uma cadeia de caracteres alfanuméricos 32 caracteres sem espaços ou traços, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|