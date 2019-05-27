---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: batch
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 04/06/2018
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: 7ba4c90811bd8051ed9c307d9f9fa33e08e69dc7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66127534"
---
Uma tarefa em execução em lote do Azure pode produzir dados de saída quando ele é executado. Com frequência, os dados de saída precisam ser armazenados para a recuperação por outras tarefas no trabalho, o aplicativo cliente que executou o trabalho, ou ambos. As tarefas gravam dados de saída no sistema de arquivos de um nó de computação do Lote, mas todos os dados no nó serão perdidos quando ele for reimaginado ou quando o nó deixar o pool. As tarefas também podem ter um período de retenção de arquivo, após o qual os arquivos criados pela tarefa são excluídos. Por esses motivos, é importante manter a saída da tarefa que você vai precisar posteriormente para um repositório de dados, como o [Armazenamento do Azure](https://docs.microsoft.com/azure/storage/).

Para opções de conta de armazenamento em Lote, confira a [Visão geral do recurso de Lote](../articles/batch/batch-api-basics.md#azure-storage-account).
