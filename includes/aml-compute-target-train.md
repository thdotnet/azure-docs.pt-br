---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 05/30/2019
ms.openlocfilehash: 50905eb987defac612f1055b450b682726f0a56f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66752971"
---
|Treinamento &nbsp;destinos| Suporte de GPU |[ML automatizado](../articles/machine-learning/service/concept-automated-ml.md) | [Pipelines do ML](../articles/machine-learning/service/concept-ml-pipelines.md) | [Interface Visual](../articles/machine-learning/service/ui-concept-visual-interface.md)
|----|:----:|:----:|:----:|:----:|
|[Computador local](../articles/machine-learning/service/how-to-set-up-training-targets.md#local)| Talvez | Sim | &nbsp; | &nbsp; |
|[Computação do Azure Machine Learning](../articles/machine-learning/service/how-to-set-up-training-targets.md#amlcompute)| Sim | Sim & <br/>hyperparameter&nbsp;tuning | Sim | Sim |
|[VM remota](../articles/machine-learning/service/how-to-set-up-training-targets.md#vm) |Sim | Sim & <br/>gráfico de ajuste de hiperparâmetro | Sim | &nbsp; |
|[Azure&nbsp;Databricks](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#databricks)| &nbsp; | Sim | Sim | &nbsp; |
|[Análise Azure Data Lake](../articles/machine-learning/service/how-to-create-your-first-pipeline.md#adla)| &nbsp; | &nbsp; | Sim | &nbsp; |
|[Azure HDInsight](../articles/machine-learning/service/how-to-set-up-training-targets.md#hdinsight)| &nbsp; | &nbsp; | Sim | &nbsp; |
|[Lote do Azure](../articles/machine-learning/service/how-to-set-up-training-targets.md#azbatch)| &nbsp; | &nbsp; | Sim | &nbsp; |

**Todos os destinos de computação podem ser reutilizados para vários trabalhos de treinamento**. Por exemplo, após anexar uma VM remota ao seu workspace, você pode reutilizá-la para vários trabalhos.