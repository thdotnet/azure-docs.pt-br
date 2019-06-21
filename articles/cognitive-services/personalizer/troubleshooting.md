---
title: solução de problemas
titleSuffix: Azure Cognitive Services
description: Perguntas sobre Personalizer de solução de problemas pode ser encontrada neste artigo.
author: edjez
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: article
ms.date: 06/15/2019
ms.author: edjez
ms.openlocfilehash: 5136bd295c12c4439a894b4dcf0b868d32ce43ca
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313150"
---
# <a name="personalizer-troubleshooting"></a>Solução de problemas de personalizer

Este artigo contém respostas para perguntas frequentes de solução de problemas sobre Personalizer.

## <a name="learning-loop"></a>Loop de aprendizado

### <a name="the-learning-loop-doesnt-seem-to-learn-how-do-i-fix-this"></a>O loop de aprendizado parece não saber. Como corrigir isso?

O loop de aprendizado precisa de algumas chamadas de recompensa milhares antes chamadas de classificação priorizar efetivamente. 

Se você não tiver certeza sobre como seu loop de aprendizado no momento, está se comportando, executar uma [avaliação offline](concepts-offline-evaluation.md)e aplicar a política de aprendizado corrigido. 

## <a name="next-steps"></a>Próximas etapas

[Configurar a frequência de atualização do modelo](how-to-settings.md#model-update-frequency)