---
title: Definir atividade variável no Azure Data Factory | Microsoft Docs
description: Saiba como usar a atividade Definir Variável para definir o valor de uma variável existente definida em um pipeline do Data Factory
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 10/10/2018
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.openlocfilehash: cfe6dd63234a7750fe01614d6f1b38bb7cce1adb
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70142436"
---
# <a name="set-variable-activity-in-azure-data-factory"></a>Definir Atividade Variável no Azure Data Factory

Use a atividade Definir Variável para definir o valor de uma variável existente do tipo Cadeia de caracteres, Booliano ou Matriz definido em um pipeline do Data Factory.

## <a name="type-properties"></a>Propriedades de tipo

Propriedade | Descrição | Obrigatório
-------- | ----------- | --------
name | Nome da atividade no pipeline | Sim
description | Texto descrevendo o que a atividade realiza | não
type | Tipo de atividade é SetVariable | sim
value | Literal de cadeia de caracteres ou valor do objeto de expressão usado para definir a variável especificada | sim
variableName | Nome da variável que será definida por essa atividade | sim


## <a name="next-steps"></a>Próximas etapas
Saiba mais sobre uma atividade de fluxo de controle relacionada com suporte pelo Data Factory: 

- [Acrescentar Atividade Variável](control-flow-append-variable-activity.md)
