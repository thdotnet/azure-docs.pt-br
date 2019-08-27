---
title: Intenções e entidades predefinidas – LUIS
titleSuffix: Azure Cognitive Services
description: Neste tutorial, adicione intenções e entidades predefinidas a um aplicativo para obter rapidamente a previsão de intenção e a extração de dados. Não é necessário rotular nenhum enunciado com as entidades pré-criadas. A entidade é detectada automaticamente.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 08/20/2019
ms.author: diberry
ms.openlocfilehash: 4697bad15a374bed0de08b7cabc5aceaad7f1259
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876719"
---
# <a name="tutorial-identify-common-intents-and-entities"></a>Tutorial: Identificar intenções e entidades comuns

Neste tutorial, adicione intenções e entidades pré-criadas a um aplicativo do tutorial de recursos humanos para obter rapidamente a previsão de intenção e a extração de dados. Não é necessário marcar nenhum enunciado com as entidades predefinidas, porque a entidade é detectada automaticamente.

Os modelos predefinidos (domínios, intenções e entidades) ajudam a criar o modelo rapidamente.

**Neste tutorial, você aprenderá a:**

> [!div class="checklist"]
> * Criar novo aplicativo
> * Adicionar intenções predefinidas 
> * Adicionar entidades predefinidas 
> * Treinar 
> * Publicar 
> * Obter intenções e entidades do ponto de extremidade

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="create-a-new-app"></a>Criar um novo aplicativo

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]


## <a name="add-prebuilt-intents-to-help-with-common-user-intentions"></a>Adicionar intenções predefinidas para ajudar com as intenções de usuário comuns

O LUIS oferece várias intenções predefinidas para ajudar com intenções do comuns do usuário.  

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Selecione **Adicionar intenção de domínio predefinida**. 

1. Pesquise `Utilities`. 

1. Selecione todas as intenções e selecione **Concluído**. Essas intenções são úteis para determinar em que ponto da conversa o usuário está e o que ele está solicitando que seja feito. 

## <a name="add-prebuilt-entities-to-help-with-common-data-type-extraction"></a>Adicionar as entidades predefinidas para ajudar com a extração de tipo de dados comuns

O LUIS fornece várias entidades predefinidas para extração de dados comuns. 

1. Selecione **Entidades** no menu de navegação à esquerda.

1. Selecione o botão **Adicionar entidade predefinida**.

1. Selecione as entidades a seguir na lista de entidades predefinidas e selecione **Pronto**:

   * **[PersonName](luis-reference-prebuilt-person.md)** 
   * **[GeographyV2](luis-reference-prebuilt-geographyV2.md)**

     ![Captura de tela do número selecionado na caixa de diálogo de entidades predefinidas](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

     Essas entidades ajudarão você a adicionar o reconhecimento de nome e local ao aplicativo cliente.

## <a name="add-example-utterances-to-the-none-intent"></a>Adicionar enunciados de exemplo à intenção None 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Treinar o aplicativo para que as alterações à intenção possam ser testadas 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Publicar o aplicativo para que o modelo em que foi feito o treinamento possa ser consultado por meio do ponto de extremidade

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Obter a previsão de entidade e de intenção do ponto de extremidade

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

1. Acesse o fim da URL na barra de endereço do navegador e insira `I want to cancel my trip to Seattle to see Bob Smith`. O último parâmetro da cadeia de consulta é `q`, a declaração **query**. 

    ```json
    {
      "query": "I want to cancel my trip to Seattle to see Bob Smith.",
      "topScoringIntent": {
        "intent": "Utilities.ReadAloud",
        "score": 0.100361854
      },
      "intents": [
        {
          "intent": "Utilities.ReadAloud",
          "score": 0.100361854
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.08102781
        },
        {
          "intent": "Utilities.SelectNone",
          "score": 0.0398852825
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.0277276486
        },
        {
          "intent": "Utilities.SelectItem",
          "score": 0.0220712926
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.0145813478
        },
        {
          "intent": "None",
          "score": 0.012434179
        },
        {
          "intent": "Utilities.Escalate",
          "score": 0.0122632384
        },
        {
          "intent": "Utilities.ShowNext",
          "score": 0.008534077
        },
        {
          "intent": "Utilities.ShowPrevious",
          "score": 0.00547111453
        },
        {
          "intent": "Utilities.SelectAny",
          "score": 0.00152912608
        },
        {
          "intent": "Utilities.Repeat",
          "score": 0.0005556819
        },
        {
          "intent": "Utilities.FinishTask",
          "score": 0.000169488427
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.000149565312
        },
        {
          "intent": "Utilities.GoBack",
          "score": 0.000141017343
        },
        {
          "intent": "Utilities.Reject",
          "score": 6.27324E-06
        }
      ],
      "entities": [
        {
          "entity": "seattle",
          "type": "builtin.geographyV2.city",
          "startIndex": 28,
          "endIndex": 34
        },
        {
          "entity": "bob smith",
          "type": "builtin.personName",
          "startIndex": 43,
          "endIndex": 51
        }
      ]
    }
    ```

    O resultado previu a intenção de Utilities.Cancel com 80% de confiança e extraiu os dados de nome de cidade e de pessoa. 


## <a name="clean-up-resources"></a>Limpar recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Informações relacionadas

Saiba mais sobre modelos predefinidos:

* [Domínios predefinidos](luis-reference-prebuilt-domains.md): esses são domínios comuns que reduzem a criação de aplicativo LUIS geral
* Intenções predefinidas: essas são as intenções individuais dos domínios comuns. Você pode adicionar intenções individualmente, em vez de adicionar o domínio inteiro.
* [Entidades predefinidas](luis-prebuilt-entities.md): esses são tipos de dados comuns úteis para a maioria dos aplicativos do LUIS.

Saiba mais sobre como trabalhar com seu aplicativo do LUIS:

* [Como treinar](luis-how-to-train.md)
* [Como publicar](luis-how-to-publish-app.md)
* [Como testar no portal do LUIS](luis-interactive-test.md)

## <a name="next-steps"></a>Próximas etapas

Adicionando entidades e intenções pré-criadas, o aplicativo cliente pode determinar as intenções comuns do usuário e extrair os tipos de dados comuns.  

> [!div class="nextstepaction"]
> [Saiba como adicionar uma entidade de expressão regular](luis-quickstart-intents-regex-entity.md)

