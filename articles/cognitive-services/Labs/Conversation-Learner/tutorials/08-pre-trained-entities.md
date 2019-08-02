---
title: Como adicionar entidades previamente treinadas a um modelo de Conversation Learner – Serviços Cognitivos da Microsoft | Microsoft Docs
titleSuffix: Azure
description: Saiba como adicionar entidades previamente treinadas a um modelo de Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: fceca459b80725e1a9c8b7dbc6a4387ea98dbb7b
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704130"
---
# <a name="how-to-add-pre-trained-entities"></a>Como adicionar entidades previamente treinadas
Este tutorial mostra como adicionar entidades previamente treinadas ao modelo de Conversation Learner.

## <a name="video"></a>Vídeo

[![Visualização de Tutorial entidades previamente treinadas](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities_Preview)](https://aka.ms/cl_Tutorial_v3_PreTrainedEntities)

## <a name="requirements"></a>Requisitos
Este tutorial exige que Bot de tutorial esteja em execução

    npm run tutorial-general

## <a name="details"></a>Detalhes

Entidades previamente treinadas reconhecem tipos comuns de entidades, como números, datas, valores monetários e outros.  Elas funcionam "out-of-the-box," não exigem qualquer treinamento e seu comportamento não pode ser alterado ao contrário de entidades personalizadas.  Por padrão, entidades previamente treinadas têm vários valores, acumulando todas as instâncias da entidade identificadas.

## <a name="steps"></a>Etapas

Inicie na home page na interface de usuário da Web.

### <a name="create-the-model"></a>Criar o modelo

1. Selecione **novo modelo**.
2. Digite **PretrainedEntities** para o **nome**.
3. Selecione **Criar**.

### <a name="entity-creation"></a>Criação da entidade

1. Selecione **entidades** no painel esquerdo e **nova entidade**.
2. Selecione **pré-treinado/datetimeV2** para o **tipo de entidade**.
3. Verifique o **valor múltiplo** para habilitar a entidade para acumular um ou mais valores. Observe que as entidades pré-treinadas não podem ser negadas.
4. Selecione **Criar**.

![](../media/T08_entity_create.png)

1. Selecione **ações** no painel esquerdo e **nova ação**.
2. Insira **a data $Builtin-datetimev2** para a **resposta do bot...** .
3. Selecione **Criar**.

![](../media/T08_action_create_1.png)

### <a name="create-the-second-action"></a>Criar a segunda ação

1. Selecione **ações** no painel esquerdo e **nova ação**.
2. Insira **o que é a data? para a** **resposta do bot.** .. Entidades pré-treinadas não podem ser **entidades necessárias** , pois são reconhecidas por padrão para todos os declarações.
3. Insira **BuiltIn-datetimev2** para **desqualificar entidades**.
4. Selecione **Criar**.

![](../media/T08_action_create_2.png)

### <a name="train-the-model"></a>Treinar o modelo

1. Selecione **treinar caixas de diálogo** no painel esquerdo e, em seguida, **nova caixa de diálogo treinar**.
2. Insira **Olá** para o expressão do usuário no painel de bate-papo à esquerda.
3. Selecione **ações de Pontuação**.
4. Selecione **qual é a data?** na lista de ações
5. Insira **hoje** para o expressão do usuário no painel de bate-papo à esquerda.
    - O expressão **atual** é reconhecido automaticamente por modelos pré-qualificados em Luis.
    - Passar o mouse sobre os valores de entidades previamente treinadas mostra dados adicionais fornecidos pelo LUIS.

![](../media/T08_training.png)

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Resolvedores de entidade](./09-entity-resolvers.md)
